<script>
    import ReportWhiteboardStatistics from "./ReportWhiteboardStatistics.svelte";
    export let result;
</script>

<style>
    article {
        position: relative;
        margin: 20px;
    }

    h1, p {
        margin-left: 20px;
    }

    .user {
        background-color: lightgreen;
        margin: 5px;
        margin-left: 0;
        padding: 5px 0;
        border: 1px solid rgb(100, 172, 100);
    }
</style>

<article>
    {#each result.data as user}
        <div>
            <div class="user">
                <h1>User: {user.username}</h1>
                <p>Amount of whiteboards {user.whiteboards.length}</p>
            </div>
            {#each user.whiteboards as board}
                <ReportWhiteboardStatistics {board} />
            {/each}
        </div>
        <hr>
    {/each}
</article>