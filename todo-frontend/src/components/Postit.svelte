<script>
	import { spring } from 'svelte/motion';
    import {pannable} from "../utils/pannable";
    import axios from "axios";
    export let id;
    export let whiteboard;
    export let todo;
    export let color;
    export let x;
    export let y;
    export let setXY;

    async function updateCoords(){
        try {
            await axios.put("http://localhost:5000/postit", {
                X:$coords.x, Y:$coords.y, Color:color, Whiteboard:whiteboard, Id:id, Todo:todo
            })
            console.log("note update ok");
            
        } catch (error) {
            console.error("note uhoh", error);
        }
    }

	const coords = spring({ x, y }, {
		stiffness: 0.7,
		damping: 0.7
	});

	function handlePanMove(event) {
		coords.update($coords => ({
			x: $coords.x + event.detail.dx,
			y: $coords.y + event.detail.dy
        }));
        setXY($coords.x,$coords.y);
	}

	function handlePanEnd(event) {
        updateCoords()
	}
</script>

<style>
    article{
        position: absolute;
        background-color: #ffffe0;
        width: 200px;
        min-height: 200px;
        z-index: 2;

        top: 100px;
        left: 100px;
        box-shadow: 1px 1px 3px rgb(51, 44, 1);
        resize: both;

    }

    input {
        width: 100%;
        border: none;
        background-color: #f5f5b0;
    }

    input:focus {
        background-color: white;
    }

    #title {
        height: 15%;
    }

    #content{
        min-height: 130px;
        height: 75%;
        width: 100%;
        background-color: inherit;
        border: none;
        resize: none;
        overflow: hidden;
        padding: 0 10px;
    }
</style>    

<article
    use:pannable
	on:panmove={handlePanMove}
	on:panend={handlePanEnd}
    	style="transform:translate({$coords.x}px,{$coords.y}px)"
>
    <h3>
        <input id="title" bind:value={todo.title} type="text"/> 
    </h3>
    <textarea 
        id="content" 
        bind:value={todo.content} 
        type="text" 
    />
    <input name="done" type="checkbox" bind:checked={todo.finished}/>
    <label for="done">Finished?</label>

</article>