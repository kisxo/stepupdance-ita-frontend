<script>
    import { page } from "$app/stores";
    import Cookies from "js-cookie"
    import { api_url } from "$lib/config";

    import ParticipantCard from "./ParticipantCard.svelte";

    const page_url = $page.url;
    let participants

    let options = {
        method: 'GET',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': "Bearer " + Cookies.get("jwt")
        }
    };

    fetch(api_url+'/participants/', options)
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
    })
    .catch(error => {
        console.log(Response.code)
    });
</script>

{#if participants == ""}
    <h4 class="m-[1rem] font-bold">You have not registered for any events !</h4>
{/if}
<div class="participant-list">
    <a href="/register" class="mx-auto bg-pink-500 px-[2rem] py-[.5rem] rounded-md font-bold text-white">
        Register Now
    </a>
    {#each participants as participant}
        <ParticipantCard {...participant}/>
	{/each}
</div>

<style>
    .participant-list{
        display: flex;
        flex-direction: column;
        padding: 2rem;
        gap: 2rem;
    }
</style>
