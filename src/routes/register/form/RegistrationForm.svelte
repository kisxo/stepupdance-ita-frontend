<script>
    import Cookies from "js-cookie";
    import { page } from "$app/stores";
    import { api_url } from "$lib/config";
    import Loader from "$lib/components/Loader.svelte";
    import { AlertDialog } from "bits-ui";
    import { fade } from "svelte/transition";
    import { flyAndScale } from "$lib/bits-ui/utils/transitions";
    import { derived } from "svelte/store";
    import { parse } from "svelte/compiler";
    import DangerModal from "$lib/components/DangerModal.svelte";
    import SuccessModal from "$lib/components/SuccessModal.svelte";
    import axios from "axios";
    import { goto } from "$app/navigation";
    let { category } = $props();

    let fname = $state();
    let age = $state();
    let gender = $state();
    let duo_name1 = $state();
    let duo_name2 = $state();
    let group_name = $state();
    let selected_event = $state("");
    let dialogOpen = $state(false);
    let alertOpen = $state(false);

    function get_input_data(){
        if(validate_input() == false){ return false }

        if(selected_event.title === "Dance Duo"){
            return {
                "eventId": selected_event._id,
                "duo_name1": duo_name1,
                "duo_name2": duo_name2
            }
        }
        else if(selected_event.title === "Dance Group"){
            return {
                "eventId": selected_event._id,
                "group_name": group_name
            }
        }
        else{
            return {
                "eventId": selected_event._id,
                "fname": fname,
                "age": parseInt(age),
                "gender": gender,
            }
        }
    }

    function validate_input(){
        if(selected_event.title === "Dance Duo"){
            if(!duo_name1 | !duo_name2)
            {
                showDanger("Please enter the names of both contestants!", "Okay")
                return false
            }
        }
        else if(selected_event.title === "Dance Group"){
            if(!group_name)
            {
                showDanger("Please enter a group name !", "Okay")
                return false
            }
        }
        else{
            if(!fname | !age | !gender | age < 4 | age > 60 | !parseInt(age))
            {
                showDanger("Please enter correct details !", "Okay")
                return false
            }
        }
        return true
    }

    let alert_data = $state();
    function showModal(title) {
        if (get_input_data() == false){ return false }

        alert_data = {
            "title": title,
            "list_data": get_input_data(),
        }
        dialogOpen = true;
    }

    let dangerModal_data = $state();
    function showDanger(body, cancel){
        dangerModal_data = {
            "toggle": true,
            "body": body,
            "cancel": cancel
        }
    }

    let successModal_data = $state();
    function showSuccess(body, cancel, redirect){
        successModal_data = {
            "toggle": true,
            "body": body,
            "cancel": cancel,
            "redirect": redirect,
        }
    }

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

    function register() {
        if (get_input_data() == false){ return false }

        let data = get_input_data();
        let options = {
            headers: {
                "Content-Type": "application/json",
                'Authorization': "Bearer " + Cookies.get("jwt")
            }
        };
        axios.post(api_url + '/participants/', data, options)
        .then((response) => {
            if(response.status === 201)
            {
                showSuccess("Create successful", "Continue", "/dashboard")
            }
        })
        .catch(function (error) {
            if(error.status === 422)
            {
                showDanger("Enter a Password !", "Ok")
            }
            if(error.status === 400)
            {
                showDanger(error.response.data.detail, "Ok")
            }
        });
    }
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
                Location: Sarupathar
            </span>
        </div>

        <span class="field-title text-[.9rem] font-medium pt-[1rem]">Category</span>
        <div class="group-select">
            <select bind:value={selected_event} class="drop-shadow-sm bg-white text-gray-600 items-center rounded-md border border-border-input px-[11px] py-[10px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background">
                <option selected disabled hidden></option>
                {#each events as event}
                    <option value={event}>{event.title}</option>
                {/each}
            </select>
            <svg xmlns="http://www.w3.org/2000/svg" class="ml-auto size-6 text-muted-foreground text-gray-200" viewBox="0 0 256 256">
                <rect width="256" height="256" fill="none"/>
                <polyline points="80 176 128 224 176 176" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="16"/>
                <polyline points="80 80 128 32 176 80" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="16"/>
            </svg>
        </div>

        {#if selected_event.title === "Dance Duo"}
            <span class="field-title text-[.9rem] font-medium">1st dancer Name</span>
            <input bind:value={duo_name1} type="text" class="inline-flex mb-[1rem] drop-shadow-sm items-center text-gray-600 py-[.5rem] rounded-md border border-border-input bg-background px-[11px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background"/>
            <span class="field-title text-[.9rem] font-medium">2nd dancer Name</span>
            <input bind:value={duo_name2} type="text" class="inline-flex mb-[1rem] drop-shadow-sm items-center text-gray-600 py-[.5rem] rounded-md border border-border-input bg-background px-[11px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background"/>
            <button onclick={() => {showModal("Confirm Registration")}} class=" h-10 my-[1.5rem] text-white bg-blue-500 rounded-input bg-dark px-[21px] text-[15px] font-semibold text-background shadow-mini hover:bg-dark/95 active:scale-98 active:transition-all">
                Register
            </button>

        {:else if selected_event.title === "Dance Group"}
            <span class="field-title text-[.9rem] font-medium">Group Name</span>
            <input bind:value={group_name} type="text" class="inline-flex mb-[1rem] drop-shadow-sm items-center text-gray-600 py-[.5rem] rounded-md border border-border-input bg-background px-[11px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background"/>
            <button onclick={() => {showModal("Confirm Registration")}} class=" h-10 my-[1.5rem] text-white bg-blue-500 rounded-input bg-dark px-[21px] text-[15px] font-semibold text-background shadow-mini hover:bg-dark/95 active:scale-98 active:transition-all">
                Register
            </button>
        {:else}

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
                            <option value="female">Female</option>
                        </select>
                        <svg xmlns="http://www.w3.org/2000/svg" class="ml-auto size-6 text-muted-foreground text-gray-200" viewBox="0 0 256 256">
                            <rect width="256" height="256" fill="none"/>
                            <polyline points="80 176 128 224 176 176" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="16"/>
                            <polyline points="80 80 128 32 176 80" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="16"/>
                        </svg>
                    </div>
                </div>
            </div>
            <button onclick={() => {showModal("Confirm Registration")}} class=" h-10 my-[1.5rem] text-white bg-blue-500 rounded-input bg-dark px-[21px] text-[15px] font-semibold text-background shadow-mini hover:bg-dark/95 active:scale-98 active:transition-all">
                Register
            </button>
        {/if}
    </div>

    <AlertDialog.Root bind:open={dialogOpen}>
        <AlertDialog.Portal>
          <AlertDialog.Overlay transition={fade} transitionConfig={{ duration: 150 }} class="fixed inset-0 z-50 bg-black/80"/>
          <AlertDialog.Content transition={flyAndScale} class="fixed bg-white left-[50%] top-[50%] z-50 grid w-full max-w-[94%] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-md border bg-background p-7 shadow-popover outline-none sm:max-w-lg md:w-full">
            <div class="flex flex-col gap-4 pb-6">
                <AlertDialog.Title class="text-lg font-semibold tracking-tight">
                    {alert_data.title}
                </AlertDialog.Title>
                <AlertDialog.Description class="text-sm text-foreground-alt">
                    {#if selected_event.title === "Dance Solo"}
                        Name: {alert_data.list_data.fname}
                        <br>
                        Age: {alert_data.list_data.age}
                        <br>
                        Gender: {alert_data.list_data.gender}
                        <br>
                        Event: {selected_event.title}
                        <br>
                        Amount: {selected_event.amount}
                    {/if}
                </AlertDialog.Description>
            </div>
            <div class="flex w-full items-center justify-center gap-2">
                <AlertDialog.Cancel class="inline-flex bg-gray-200 py-[.5rem] mx-auto h-input w-full items-center justify-center rounded-input bg-muted text-[15px] font-medium shadow-mini transition-all hover:bg-dark-10 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-foreground focus-visible:ring-offset-2 focus-visible:ring-offset-background active:scale-98">
                    Cancel
                </AlertDialog.Cancel>
                <AlertDialog.Action onclick={register} class="inline-flex bg-black py-[.5rem] text-white h-input w-full items-center justify-center rounded-input bg-dark text-[15px] font-semibold text-background shadow-mini transition-all hover:bg-dark/95 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-dark focus-visible:ring-offset-2 focus-visible:ring-offset-background active:scale-98">
                    Continue
                </AlertDialog.Action>
            </div>
          </AlertDialog.Content>
        </AlertDialog.Portal>
    </AlertDialog.Root>

    <DangerModal {...dangerModal_data}/>
    <SuccessModal {...successModal_data}/>
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
