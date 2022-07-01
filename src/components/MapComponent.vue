<template>
  <v-container>
    <v-card>
      <div id="map"></div>
    </v-card>
  </v-container>
</template>

<script>
import "leaflet/dist/leaflet.css";
import "leaflet";
import "@geoman-io/leaflet-geoman-free";
import "@geoman-io/leaflet-geoman-free/dist/leaflet-geoman.css";

const L = window["L"];

delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});

export default {
  name: "MapComponent",

  data() {
    return {
      map: null,
    };
  },

  mounted() {
    this.initMap();
  },

  methods: {
    initMap() {
      this.map = L.map("map").setView([40.0269319, 32.83604819], 13);
      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        maxZoom: 19,
        attribution:
          '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      }).addTo(this.map);
      this.map.pm.addControls({
        position: "topleft",
        drawControls: false,
        editControls: true,
        optionsControls: true,
        customControls: true,
        oneBlock: false,
      });
      this.map.pm.Toolbar.createCustomControl({
        name: "alertBox",
        block: "custom",
        className: "leaflet-pm-icon-marker xyz-class",
        title: "Count layers",
        onClick: () => {
          alert(
            "There are " + L.PM.Utils.findLayers(this.map).length + " layers on the map"
          );
        },
        toggle: false,
      });
      const _actions = [
        {
          text: "Custom message, with click event",
          onClick(e) {
            alert("click", e);
          },
          name: "actionName",
        },
      ];
      this.map.pm.Toolbar.copyDrawControl("Rectangle", {
        name: "RectangleCopy",
        block: "custom",
        title: "Display text on hover button",
        actions: _actions,
      });
      this.map.pm.Draw.RectangleCopy.setPathOptions({ color: "green" });

      this.map.pm.Toolbar.changeControlOrder(["RectangleCopy"]);

      this.map.on("pm:actionclick", function (e) {
        console.log(e);
      });
      this.map.on("pm:buttonclick", function (e) {
        console.log(e);
      });
    },
  },
};
</script>

<style scoped>
#map {
  height: 600px;
}
</style>
