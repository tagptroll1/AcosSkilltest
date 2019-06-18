<script>
    import selected from "../stores/selectedWhiteboard";
    import Postit from "./Postit.svelte";
    import selectedPostits from "../stores/selectedPostits";

    export let id;
    export let axios;


    async function newPostit(){
        console.log(new Date(Date.now()).toISOString())
        const newNote = {
            todo:{
                title: "New postit",
                created: new Date(Date.now()).toISOString(),
                content: " ",
                finished: false
            },
            whiteboard: {
                id: $selected.id
            },
            color: "#ffffe0",
            x: 100,
            y: 100
        }

        selectedPostits.set([...$selectedPostits, newNote])
        try {
            await axios.post("http://localhost:5000/postit/new", {...newNote});
        } catch (err) {
            console.error(err)
        }
    }

</script>
<button on:click={newPostit}>Create new Postit</button>
{#each $selectedPostits as postit}
    <Postit {postit} {axios}/>
{/each}
