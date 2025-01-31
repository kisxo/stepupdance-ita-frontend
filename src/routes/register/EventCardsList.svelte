<script>
    import EventCard from "./EventCard.svelte";

    let n = [1,2,3,4]
</script>

{#each n as m}
    <EventCard/>
{/each}

