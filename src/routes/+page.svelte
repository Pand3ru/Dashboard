<script>
  import Nested from "./Weather.svelte";
</script>

<div id="nested-container">
  <Nested />
</div>

<style>
  #nested-container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 15%;
    transform: scale(0.8);
  }
</style>
