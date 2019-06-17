<script>
    import ReportStatistics from "./ReportStatistics.svelte"
    import { onMount } from "svelte";
    export let axios;
    export let token;

    async function getReport() {
        try {
            return axios.get("http://localhost:5000/report", {
                headers: {
                    Authorization: `Bearer ${token}`
                }
            });
        } catch (error) {
            throw new Error(error);
        }
    }
    let response = getReport();


</script>

<style>
    
</style>


{#await response}
    <p>Loading report...</p>
{:then result}
    <ReportStatistics {result} />
{:catch err}
    <p>Ooops. {err}</p>
{/await}
