<script>
    import { onMount } from "svelte";
    import WhiteboardPopup from "./WhiteboardPopup.svelte";
    import currentUser from "../stores/currentUser";
    import selected from "../stores/selectedWhiteboard";
    import whiteboards from "../stores/allWhiteboards";
    import selectedPostits from "../stores/selectedPostits";

    let showCreateNew = false;


    if ($whiteboards.length > 0) {
        $selected = $whiteboards[0];
        $selectedPostits = $selected.postits;
    }

    function handleChange(e) {
        if (e.target.value !== " + New whiteboard")
            $selectedPostits = $selected.postits;
    }

    function createWhiteboard(e) {
        showCreateNew = true;
    }

</script>

<style>
select{
    padding-left: 10px;
    padding-top: 5px;
    width: 100%;
    height: 50px;
    background-color: lightskyblue;
}
</style>

{#if showCreateNew}
    <WhiteboardPopup 
        id={$currentUser.id}
        setShow="{() => showCreateNew=false}"
    />
{/if}
<select bind:value={$selected} on:change={handleChange}>
    {#each $whiteboards as board}
        <option value={board}>{board.title}</option>
    {/each}
        <option on:click={createWhiteboard}> + New whiteboard</option>
</select>