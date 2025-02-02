<script >
    import { page } from "$app/stores";
    import { api_url } from "$lib/config";
    import Loader from "$lib/components/Loader.svelte";
    let { category } = $props();
    console.log(category);

    const genders = [
        { value: "male", label: "Male" },
        { value: "female", label: "Female" },
    ];

    let fname = $state();
    let age = $state();
    let gender = $state();
    let eventId = $state();

    let events = $state();
    const page_url = $page.url;

    let options = {
        method: "GET",
        headers: {
            "Content-Type": "application/json",
        },
    };

    fetch(api_url + "/events/" + category, options)
        .then((response) => {
            if (response.status === 401) {
                console.error("Auth Failed");
            }
            return response.json();
        })
        .then((data) => {
            events = data.events;
        })
        .catch((error) => {});
</script>

{#if events}
    <div class="font-sans bg-white rounded-md p-[2rem] flex flex-col gap-[.4rem] min-h-screen">
        <div class="flex flex-col gap-[.5rem] pb-[1.5rem] border-b border-gray-200">
            <span class="text-xl font-bold capitalize">
                Registration for {events[0].category}
            </span>
            <span class="text-base text-gray-600 font-semibold">
                Date: {events[0].date}
                <br />
                Location: Satupathar
            </span>
        </div>

        <span class="field-title text-[.9rem] font-medium pt-[1rem]">Category</span>
        <div class="group-select">
            <select bind:value={eventId} class="drop-shadow-sm bg-white text-gray-600 items-center rounded-md border border-border-input px-[11px] py-[10px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background">
                <option selected disabled hidden></option>
                {#each events as event}
                    <option value={event._id}>{event.title}</option>
                {/each}
            </select>
            <svg xmlns="http://www.w3.org/2000/svg" class="ml-auto size-6 text-muted-foreground text-gray-200" viewBox="0 0 256 256">
                <rect width="256" height="256" fill="none"/>
                <polyline points="80 176 128 224 176 176" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="16"/>
                <polyline points="80 80 128 32 176 80" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="16"/>
            </svg>
        </div>

        <span class="field-title text-[.9rem] font-medium">Full Name</span>
        <input bind:value={fname} type="text" class="inline-flex mb-[1rem] drop-shadow-sm items-center text-gray-600 py-[.5rem] rounded-md border border-border-input bg-background px-[11px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background"/>

        <div class="participant-detail">
            <div class="left">
                <span class="field-title">Age</span>
                <input bind:value={age} inputmode="numeric" type="text" class="w-2/3 drop-shadow-sm text-gray-600 py-[.5rem] rounded-md border border-border-input bg-background px-[11px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background"/>
            </div>
            <div class="right">
                <span class="field-title text-md">Gender</span>
                <div class="group-select">
                    <select bind:value={gender} class="drop-shadow-sm bg-white text-gray-600 items-center rounded-md border border-border-input px-[11px] py-[10px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background">
                        <option selected disabled hidden></option>
                        <option value="male">Male</option>
                        <option value="Female">Female</option>
                    </select>
                    <svg xmlns="http://www.w3.org/2000/svg" class="ml-auto size-6 text-muted-foreground text-gray-200" viewBox="0 0 256 256">
                        <rect width="256" height="256" fill="none"/>
                        <polyline points="80 176 128 224 176 176" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="16"/>
                        <polyline points="80 80 128 32 176 80" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="16"/>
                    </svg>
                </div>
        
            </div>
        </div>

        <button class=" h-10 my-[1.5rem] text-white bg-blue-500 rounded-input bg-dark px-[21px] text-[15px] font-semibold text-background shadow-mini hover:bg-dark/95 active:scale-98 active:transition-all">
            Register
        </button>

        {fname}
        <br>
        {age}
        <br>
        {gender}
        <br>
        {eventId}
    </div>
{:else}
    <Loader />
{/if}

<style>
    .group-select {
        display: flex;
        background-color: white;
        position: relative;
        select {
            width: 100%;
            background-color: white;
            appearance: none;
        }
        svg{
            position: absolute;
            top: 20%;
            right: 2%;
        }
    }

    .participant-detail {
        display: flex;
        flex-direction: row;
    }

    .left {
        width: 50%;
        display: flex;
        flex-direction: column;
    }
    .right {
        display: flex;
        flex-direction: column;
        width: 50%;
    }
</style>
