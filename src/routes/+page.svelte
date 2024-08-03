<script lang="ts">
    import Nested from "./Weather.svelte";
    import { onMount } from "svelte";
    let width_elem;
    let isShrinking = false;
    onMount(() => {
        width_elem = document
            .getElementById("nested-container")
            ?.getBoundingClientRect().width;
    });

    function onMouseEnter() {
        isShrinking = true;
    }
    function onMouseLeave() {
        isShrinking = false;
    }
</script>

<div
    id="nested-container"
    class={isShrinking ? "shrink" : ""}
    on:mouseenter={onMouseEnter}
    on:mouseleave={onMouseLeave}
>
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
    .shrink {
        animation: shrinkToRight 0.5s forwards;
    }
    @keyframes shrinkToRight {
        from {
        }
        to {
            transform: scaleX(0);
            opacity: 0;
        }
    }
</style>
