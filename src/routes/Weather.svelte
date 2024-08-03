<!-- Weather.svelte -->
<script lang="ts">
  const regexp = new RegExp("^([+-]?[0-9]+)°C:([+-]?[0-9]+)°C:([A-Za-z]+)");

  let temperature: number | null = null;
  let feel: number | null = null;
  let cond: string | null = null;
  let data: string | null = null;
  let src: string | null = null;

  function setIcon(condition: string) {
    switch (true) {
      case condition.toLowerCase().includes("clear") ||
        condition.toLowerCase().includes("sunny"):
        src = "/icons/sun.png";
        break;
      case condition.toLowerCase().includes("rain"):
        src = "/icons/rain.png";
        break;
      case condition.toLowerCase().includes("snow"):
        src = "/icons/snow.png";
        break;
      case condition.toLowerCase().includes("storm") ||
        condition.toLowerCase().includes("hail") ||
        condition.toLowerCase().includes("thunderstorm"):
        src = "/icons/storm.png";
        break;
      case condition.toLowerCase().includes("fog") ||
        condition.includes("mist"):
        src = "/icons/fog.png";
        break;
      case condition.toLowerCase().includes("cloud"):
        src = condition.toLowerCase().includes("partly")
          ? "/icons/cloud1.png"
          : "/icons/cloud2.png";
        break;
      case condition.toLowerCase().includes("drizzle"):
        src = "/icons/drizzle.png";
        break;
      case condition.toLowerCase().includes("patchy") &&
        condition.includes("rain"):
        src = "/icons/patchy_rain.png";
        break;
      default:
        src = "/icons/default.jpg";
        break;
    }
  }
  async function fetchWeatherData() {
    const url = "https://wttr.in/Nuremberg?format=%t:%f:%C";
    try {
      const response = await fetch(url);
      if (!response.ok) {
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
    } catch (error: any) {
      console.error(error.message);
      data = null;
    }
  }

  import { onMount } from "svelte";
  onMount(() => {
    fetchWeatherData();
  });

  $: weather = {
    deg: temperature ?? 0,
    condition: cond ?? "unknown",
    feels: feel ?? 0,
  };
</script>

<div id="block">
  <img {src} alt="Weather icon" />
  <p>Temperature {weather.deg}°C<br />Feels like {weather.feels}°C</p>
</div>

<style>
  p {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
      "Helvetica Neue", Arial, sans-serif;
    font-weight: bold;
    font-size: 1.1rem;
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
  img {
    padding-top: 20px;
  }
</style>
