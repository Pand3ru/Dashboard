<!-- Weather.svelte -->
<script lang="ts">
  const regexp = new RegExp('^([+-]?[0-9]+)°C:([+-]?[0-9]+)°C:([A-Za-z]+)');

  let temperature: number | null = null;
  let feel: number | null = null;
  let cond: string | null = null;
  let data: string | null = null;
  let src: string | null = null;

  function setIcon(cond) {
    switch (cond) {
    case "Clear":
      src = '../icons/sun.png';
      break;
    case "Partly Cloudy":
    case "Partly cloudy":
      src = '../icons/cloud1.png';
      break;
    case "Cloudy":
      src = '../icons/cloud2.png';
      break;
    case "Rain":
    case "Showers":
      src = '../icons/rain.png';
      break;
    case "Snow":
      src = '../icons/snow.png';
      break;
    case "Storm":
    case "Hail":
    case "Thunderstorm":
      src = '../icons/storm.png';
      break;
    case "Fog":
    case "Mist":
      src = '../icons/fog.png';
      break;
    case "Drizzle":
      src = '../icons/drizzle.png';
      break;
    default:
      src = '../icons/default.png'; // Default icon for unexpected conditions
      break;
  }}
  async function fetchWeatherData() {
    const url = "https://wttr.in/Nuremberg?format=%t:%f:%C";
    try {
      const response = await fetch(url);
      if(!response.ok) {
        data = null;
        return;
      }
      const text = await response.text();
      data = text;
      const match = regexp.exec(text);

      if (match) {
        temperature = parseInt(match[1], 10);
        feel = parseInt(match[2], 10);
        cond = match[3];
        setIcon(cond);
      }

    } catch(error) {
      console.error(error.message);
      data = null;
    }
  }

  import { onMount } from 'svelte';
  onMount(() => {
    fetchWeatherData();
  });

  $: weather = {
    deg: temperature ?? 0,
    condition: cond ?? "unkown",
    feels: feel ?? 0
  };

</script>

<style>
  p {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    font-weight: bold;
    font-size: 1rem;
  }
  #block {
    color: black;
    background-color: white;
    margin: 10px;
    padding: 10px;
    height: fit-content;
    width: fit-content;
    border-radius: 20px;
    border-color: black;
    display: flex;
    flex-direction: column;
    align-items: center; /* Center items horizontally */
    justify-content: center; /* Center items vertically */
    text-align: center; /* Center text inside the div */
    box-align: center;
  }
</style>
<div id="block">
  <img src={src}/>
  <p>Temperature {weather.deg}°C</p>
  <p> Feels like {weather.feels}°C </p>
</div>

