<script>
    import axios from "axios";
    import whiteboards from "../stores/allWhiteboards";
    export let id;
    export let setShow;

    async function createNew(e){
        e.preventDefault();
        const title = document.forms["whiteboardform"]["title"].value;
        try {
            const resp = await axios.post(`whiteboard/new?user=${id}`, {title})
            whiteboards.set([...$whiteboards, resp.data])
            setShow();
        } catch (error) {
            alert(error.message)
        }
    }

</script>
<style>
div {
    padding: 20px;
    margin: 20px;
    position: absolute;
    top: 50px;
    left: calc(50% - 300px);
    height: 300px;
    width: 600px;
    z-index: 5;
    background-color: grey;
}

.all {
    background-color: transparent;
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
}
</style>
<div class="all" on:click={setShow}>
    <div on:click|stopPropagation>
        <h2>Please give your new whiteboard a title</h2>
        <p>This title will also be the category for all your future postit notes on this whiteboard</p>
        <form name="whiteboardform">
            <labal for="title">Title</labal>
            <input type="text" placeholder="Title" name="title" required/>
            <button on:click={createNew}>Submit</button>
        </form>
    </div>
</div>