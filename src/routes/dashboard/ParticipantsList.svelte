<script>
    import { page } from "$app/stores";
    import Cookies from "js-cookie"

    import ParticipantCard from "./ParticipantCard.svelte";

    const url = "http://localhost:8000";

    const page_url = $page.url;
    let participants

    let options = {
        method: 'GET',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': "Bearer " + Cookies.get("jwt")
        }
    };

    fetch(url+'/participants', options)
    .then(response => {
        if(response.status === 401)
        {
            console.error("Auth Failed")
            goto('/auth/login?redirect=' + page_url.pathname);
        }
        return response.json();
    })
    .then(data => {
        participants = data.participants
        console.log(participants)
    })
    .catch(error => {
        console.log(Response.code)
    });
</script>

<div class="participant-list">
    {#each participants as participant}
        <ParticipantCard {...participant}/>
	{/each}
</div>

<style>
    .participant-list{
        outline: 1px solid black;
        display: flex;
        flex-direction: column;
        padding: 2rem;
        gap: 2rem;
    }
</style>
