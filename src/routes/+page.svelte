<script>
    import Nested from "./Weather.svelte";
    import { onMount } from "svelte";
    let width_elem;
    onMount(() => {
        width_elem = document
            .getElementById("nested-container")
            ?.getBoundingClientRect().width;
    });
</script>

<div id="nested-container">
    <Nested />
    <Nested />
    <Nested />
</div>
<p>{width_elem}</p>

<style>
    #nested-container {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 10px;
        grid-auto-rows: minmax(100px, auto);

        display: flex;
        justify-content: center;
        align-items: center;
        margin: 15%;
        transform: scale(0.8);
    }
</style>
