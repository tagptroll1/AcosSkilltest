<script>
    import whiteboards from "../stores/allWhiteboards";
    import WhiteboardSelector from "./WhiteboardSelector.svelte";
    import WhiteboardPostits from "./WhiteboardPostits.svelte";
    import selected from "../stores/selectedWhiteboard";
    import WhiteboardPopup from "./WhiteboardPopup.svelte";

    export let token;
    export let id;
    export let username;
    export let axios;

    const getBoardsURL = `http://localhost:5000/whiteboard/user/${id}`;

    axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
    axios.defaults.headers.post['Content-Type'] = 'application/json';
 
    let boards = getWhiteBoards();

    async function getWhiteBoards(){
        try {
            const res = await axios.get(getBoardsURL);
            $whiteboards = res.data;
            return res
            
        } catch(error) {
            throw new Error(error.message);
        }
    }

</script>
<style>

section{
    position: relative;
    background-color: lightblue;
    width: 100%;
    height: 500px;
}


</style>

<section>
    {#await boards}
        Loading whiteboards...
    {:then result}
        <WhiteboardSelector />
        <WhiteboardPostits {id} {axios}/>
    {:catch err}
        {#if err.message.includes("404")}
                <p>No whiteboards found.</p>
                <WhiteboardPopup {id} setShow={getWhiteBoards}></WhiteboardPopup>
            {:else}
                Something went wrong: <br/>{err.message}
            {/if}
    {/await}
</section>
