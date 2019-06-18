<script>
	import { spring } from 'svelte/motion';
    import {pannable} from "../utils/pannable";
    import selectedPostits from "../stores/selectedPostits";


    export let axios;
    export let postit;
    let putRequest;
    let deleted = false;
    let allowToSend = false;

    function updateCoords(event){
        postit.x = $coords.x + event.detail.dx;
        postit.y = $coords.y + event.detail.dy;
    }

	const coords = spring({ x: postit.x, y:postit.y }, {
		stiffness: 0.7,
		damping: 0.7
	});

	function handlePanMove(event) {
		coords.update($coords => ({
			x: $coords.x + event.detail.dx,
			y: $coords.y + event.detail.dy
        }));
        updateCoords(event);
    }
    
    async function setAllowSend(inp){
        allowToSend = inp;
    }

    async function putPostit() {
        if (putRequest === undefined || allowToSend) {
            await setAllowSend(false);
            try {
                putRequest = await axios.put("http://localhost:5000/postit", {
                    ...postit,
                    todo: {...postit.todo},
                    x: Number.parseInt(postit.x),
                    y: Number.parseInt(postit.y)
                })
            } catch (err) {
                console.error(err)
            }
            
            if (putRequest.status == 200)
                await setAllowSend(true);
        }
    }

    async function deleteNote(){
        console.log("boop");
        try {
            await axios.delete(`http://localhost:5000/postit/${postit.id}`)
            deleted = true;
            console.log($selectedPostits.filter(p => p.id !== postit.id));
            
            selectedPostits.set($selectedPostits.filter(p => p.id !== postit.id))
        } catch (err) {
            console.error(err)
        }
    }

</script>

<style>
    article{
        position: absolute;
        width: 200px;
        min-height: 170px;
        z-index: 2;

        top: 100px;
        left: 100px;
        box-shadow: 1px 1px 3px rgb(51, 44, 1);
        resize: both;

    }


    input[type="text"] {
        width: 100%;
        border: none;
        background-color: #f5f5b0;
        margin: 0;
    }

    button{
        border: unset;
        position: absolute;
        top: 5px;
        right: 5px;
        background-color: transparent;
    }

    input:focus {
        background-color: white;
    }

    #title {
        height: 15%;
    }

    #content{
        min-height: 110px;
        height: 75%;
        width: 100%;
        border: none;
        resize: none;
        overflow: hidden;
        padding: 10px 10px;
    }

    input[type="checkbox"] {
        display: inline-block;
        margin-left: 20px;
    }
    label {
        display: inline;
        max-width: 80%;
    }

</style>    

<article
    use:pannable
	on:panmove={handlePanMove}
	on:panend={deleted? ()=>{}: putPostit}
    	style="transform:translate({$coords.x}px,{$coords.y}px); background-color: {postit.color};"
>
    <h3>
        <input 
            id="title" 
            bind:value={postit.todo.title} 
            on:submit={deleted? ()=>{}: putPostit}
            type="text"/> 
    </h3>
    <button on:click={deleteNote}>❌</button>
    <textarea 
        id="content" 
        bind:value={postit.todo.content}
        on:submit={deleted? ()=>{}: putPostit}
        style="background-color: {postit.color};"
        type="text" 
    />
    <input 
        id="checkbox" 
        name="done" 
        type="checkbox" 
        bind:checked={postit.todo.finished}
        on:change={deleted? ()=>{}: putPostit}
    />
    <label for="done">Finished?</label>

</article>