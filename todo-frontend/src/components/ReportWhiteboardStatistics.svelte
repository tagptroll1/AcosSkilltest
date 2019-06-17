<script>
    export let board;
</script>

<style>
    h2, .h2p, h3, .h3p{
        margin-left: 20px;
    }

    .postit {
        background-color: #ffffe0;
        margin: 5px;
        margin-left: 100px;
        border: 1px solid  #f5f5b0;
        padding: 5px 0;
    }

    .board {
        margin: 5px;
        background-color: lightblue;
        margin-left: 50px;
        padding: 5px 0;
        border: 1px solid  rgb(141, 181, 194);
    }
</style>
<div class="board">
    <h2>Whiteboard title: {board.title}</h2>
    <p class="h2p">Amout of postits: {board.postits.length}</p>
</div>
{#each board.postits as note}
    <div class="postit">
        <h3>{note.todo.finished ? "✅" : "❌"} {note.todo.title}</h3>
        <p class="h3p"> {note.todo.content}</p>
    </div>
{/each}