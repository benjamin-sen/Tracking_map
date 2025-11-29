// =======================
//      INITIALISATION
// =======================
const map = L.map("map").setView([20, -30], 3);

const gebcoGray = L.tileLayer(
  "https://tiles.arcgis.com/tiles/C8EMgrsFcRFL6LrL/arcgis/rest/services/GEBCO_grayscale_basemap_NCEI/MapServer/tile/{z}/{y}/{x}",
  {
    maxZoom: 12,
    attribution: "GEBCO & NOAA NCEI"
  }
).addTo(map);

const cartoLight = L.tileLayer(
  "https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png",
  {
    maxZoom: 19,
    attribution: "OSM & Carto"
  }
);

const tracesGroup = L.layerGroup().addTo(map);

const baseLayers = {
  "GEBCO gris (NOAA)": gebcoGray,
  "Fond clair (Carto)": cartoLight
};
L.control.layers(baseLayers, { Traces: tracesGroup }).addTo(map);


// =======================
//   FONCTION GPX UTILITAIRE
// =======================
function addGpx(path, color) {
  const gpx = new L.GPX(path, {
    async: true,
    marker_options: {
      startIconUrl: null,
      endIconUrl: null,
      shadowUrl: null
    },
    polyline_options: {
      color,
      weight: 3,
      opacity: 0.9
    }
  })
    .on("loaded", e => {
      tracesGroup.addLayer(e.target);

      if (tracesGroup.getLayers().length === 1) {
        map.fitBounds(e.target.getBounds());
      }
    })
    .addTo(map);
}

fetch("data/files.json")
  .then(r => r.json())
  .then(files => {
    files.forEach((file, i) => {
      addGpx("data/" + file, randomColor(i));
    });
  });


// Couleur automatique
function randomColor(i) {
  const colors = ["#FF6600", "#00BCD4", "#FF0044", "#44FF66", "#4466FF"];
  return colors[i % colors.length];
}
