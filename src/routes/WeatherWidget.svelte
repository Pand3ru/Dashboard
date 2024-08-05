<script lang="ts">
  import Weather from "./Weather.svelte";

  let container_height: Number;
  let container_width: Number;

  import { onMount } from "svelte";

  onMount(() => {
    requestAnimationFrame(() => {
      let container = document.getElementById("container");
      if (container) {
        container_height = container?.getBoundingClientRect().height;
        container_width = container?.getBoundingClientRect().width;
      }
    });
  });

  $: console.log(container_height + " <- height | width -> " + container_width);
</script>

<div id="container">
  <div><Weather /></div>
  <div class="flex-item"><Weather /></div>
  <div><Weather /></div>
</div>

<style>
  #container {
    display: flex;
    flex-direction: row;
    width: fit-content;
    height: fit-content;
  }
  #container:hover {
    animation-name: shrink;
    animation-duration: 1s;
    animation-fill-mode: forwards;
  }

  @keyframes shrink {
    from {
      transform-origin: right;
      transform: scaleX(100%);
    }
    to {
      transform-origin: right;
      transform: scaleX(0);
    }
  }
</style>
