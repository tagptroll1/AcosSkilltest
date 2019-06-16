<script>
    import axios from "axios";
    export let id;
    export let setShow;
    export let triggerReload;

    async function createNew(e){
        e.preventDefault();
        const title = document.forms["whiteboardform"]["title"].value;
        try {
            const resp = await axios.post(`whiteboard/new?user=${id}`, {title})
            setShow();
            triggerReload();
        } catch (error) {
            alert(error.message)
        }
    }

</script>
<style>
div {
    position: absolute;
    top: 50px;
    left: calc(50% - 300px);
    height: 300px;
    width: 600px;

    background-color: grey;
}
</style>

<div>
    <h2>Please give your new whiteboard a title</h2>
    <p>This title will also be the category for all your future postit notes on this whiteboard</p>
    <form name="whiteboardform">
        <labal for="title">Title</labal>
        <input type="text" placeholder="Title" name="title" required/>
        <button on:click={createNew}>Submit</button>
    </form>
</div>