<script>
    export let token;
    export let id;
    export let username;

    import {onMount} from "svelte";
    import axios from "axios";
    axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
    axios.defaults.headers.post['Content-Type'] = 'application/json';

    import CreateWhiteboardPopup from "./WhiteboardPopup.svelte";
    import Postit from "./Postit.svelte";
   

    const allBoardsEndpoint = "whiteboard/user/"+id;
    let showCreation = false;
    let showNewPostit = false;
    let allBoards = getAllBoards();;
    let selected = allBoards? allBoards[0]: []


    async function getAllBoards() {
        const resp = await axios.get(allBoardsEndpoint);
        const json = resp.data;

        if (resp.status == 200) {
            return json;
        } else {
            console.error(resp);
            
            throw new Error(resp);
        }
    }


    function createWhiteboard(e){
        showCreation = true;
    }

    function handleChange() {

    }

    async function newPostit(){
        console.log("new!")
        const newNote = {
            id: null,
            todo:{
                title:"New postit",
                created: Date.now(),
                content: "",
                finished: false
            },
            whiteboard: {
                id:selected.id
            },
            color: "#ffffe0",
            x: 100,
            y: 100
        }
        selected.postits.push(newNote)
        allBoards = new Promise((resolve, reject) => resolve(allBoards))
    }
</script>
<style>

section{
    position: relative;
    background-color: lightblue;
    width: 100%;
    height: 500px;
}

select{
    padding-left: 10px;
    padding-top: 5px;
    width: 100%;
    height: 50px;
    background-color: lightskyblue;
}
</style>

<section>
    {#if showCreation}
        <CreateWhiteboardPopup 
            {id}
            setShow="{() => showCreation=false}" 
            triggerReload="{() => allBoards=getAllBoards()}"
        />
    {/if}
    {#if allBoards !== undefined}
        {#await allBoards}
            <p>Loading boards...</p>
        {:then boards}
            <select bind:value={selected} on:change={handleChange}>
                {#each boards as board}
                    <option value={board}>{board.title}</option>
                {/each}
                    <option on:click={createWhiteboard}> + New whiteboard</option>
            </select>

            {#if selected !== undefined}
                {#await selected then selec}
                {#each selec.postits as postit}
                    <Postit {...postit} setXY="{(x,y) => {postit.x=x;postit.y=y}}"/>
                {/each}
                {/await}
            {/if}
            <button on:click={newPostit}>New Postit</button>
        {:catch err}
            {#if err.message.includes("404")}
                <p>No whiteboards found.</p>
                <button on:click={createWhiteboard}>Create one!</button>
            {:else}
                Something went wrong: <br/>{err.message}
            {/if}
        {/await}
    {/if}
</section>
