---
layout: post
name: Geolocation API
---
<body onload="getLocation()">  
<p id="demo"></p>

<script>
const x = document.getElementById("demo");

function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(showPosition);
  } else { 
    x.innerHTML = "Geolocation is not supported by this browser.";
  }
}

function showPosition(position) {
  const lat = position.coords.latitude;
  const log = position.coords.longitude;
  x.innerHTML = `<iframe width="500" height="500" src="https://www.openstreetmap.org/export/embed.html?bbox=${log}%2C${lat}%2C${log}&amp;layer=mapnik" style="border: 1px solid black"></iframe><br/>`
}
</script>
