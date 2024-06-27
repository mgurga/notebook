<script lang="ts">
    import { cubicInOut } from "svelte/easing";

    const cover = "./cover.png";
    const coverback = "./coverback.png";
    const page = "./page.png";

    let notebookleft = "",
        notebookleftbg = "",
        notebookright = cover,
        notebookrightbg = page;

    let flipright = false;
    let flipleft = false;
    let curpage = 0;

    function fliprightpage() {
        console.log("flipping right");

        curpage++;
        flipright = true;
        setTimeout(function () {
            notebookleft = curpage > 1 ? page : coverback;
            setTimeout(function () {
                flipleft = true;
                flipright = false;
                notebookright = page;
                setTimeout(function () {
                    notebookleftbg = coverback;
                    flipleft = false;
                }, 1000);
            }, 1);
        }, 1000);
    }

    function fliprightanim(node, { duration = 1000 }) {
        return {
            duration,
            css: (t) => {
                const eased = 1 - cubicInOut(t);
                return `transform: rotateY(${eased * 90}deg);`;
            },
        };
    }

    function flipleftanim(node, { duration = 1000 }) {
        return {
            duration,
            css: (t) => {
                const eased = cubicInOut(t);
                return `transform: rotateY(${eased * 90}deg);`;
            },
        };
    }

    function flipleftpage() {
        console.log("flipping left");
    }
</script>

<div id="notebookholder">
    <div id="notebookholderleft" style="transform: scaleX(-1);">
        <div
            id="notebookleftbg"
            style="background-image: url({notebookleftbg});"
        ></div>
        {#if !flipleft}
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <!-- svelte-ignore a11y-no-static-element-interactions -->
            <div
                id="notebookleft"
                style="background-image: url({notebookleft}); transform-origin: left;"
                out:flipleftanim
                on:click={flipleftpage}
            ></div>
        {/if}
    </div>

    <div id="notebookholderright">
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div
            id="notebookrightbg"
            style="background-image: url({notebookrightbg});"
        ></div>

        {#if !flipright}
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <!-- svelte-ignore a11y-no-static-element-interactions -->
            <div
                id="notebookright"
                style="background-image: url({notebookright}); transform-origin: left;"
                out:fliprightanim
                on:click={fliprightpage}
            ></div>
        {/if}
    </div>
</div>

<style>
    #notebookholder {
        aspect-ratio: 14 / 10;
        height: 90%;
        display: inline-flex;
    }

    #notebookholderleft,
    #notebookholderright {
        aspect-ratio: 7 / 10;
        width: 50%;
        cursor: pointer;
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 1fr;
    }

    #notebookleft,
    #notebookright {
        height: 100%;
        background-size: contain;
        background-repeat: no-repeat;
        background-position: center;
        z-index: 2;

        grid-column: 1;
        grid-row: 1;
    }

    #notebookleftbg,
    #notebookrightbg {
        aspect-ratio: 7 / 10;
        z-index: -2;
        height: 100%;
        background-size: contain;
        background-repeat: no-repeat;
        background-position: center;

        grid-column: 1;
        grid-row: 1;
    }
</style>
