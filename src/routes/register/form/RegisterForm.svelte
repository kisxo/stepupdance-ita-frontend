<script lang="ts">
    import { Select } from "bits-ui";
    // import { CaretUpDown, Check, Palette } from "$icons/index.js";
    import { flyAndScale } from "$lib/bits-ui/utils/transitions";
    import { page } from "$app/stores";
    import { api_url } from "$lib/config";
    import Loader from "$lib/components/Loader.svelte";
    import { Button } from "bits-ui";
    let { category } = $props();
    console.log(category);

    const genders = [
        { value: "male", label: "Male" },
        { value: "female", label: "Female" },
    ];

    let fname = $state();
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
    <div
        class="font-sans bg-white rounded-md p-[2rem] flex flex-col gap-[.4rem] min-h-screen"
    >
        <div
            class="flex flex-col gap-[.5rem] pb-[1.5rem] border-b border-gray-200"
        >
            <span class="text-xl font-bold capitalize">
                Registration for {events[0].category}
            </span>
            <span class="text-base text-gray-600 font-semibold">
                Date: {events[0].date}
                <br />
                Location: Satupathar
            </span>
        </div>

        <span class="field-title text-[.9rem] font-medium pt-[1rem]"
            >Category</span
        >
        <div class="group-select">
            <select
                class="drop-shadow-sm bg-white text-gray-600 items-center rounded-md border border-border-input px-[11px] py-[10px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background"
            >
                <option selected disabled hidden></option>
                {#each events as event}
                    <option value={event._id}>{event.title}</option>
                {/each}
            </select>
        </div>

        <span class="field-title text-[.9rem] font-medium">Full Name</span>
        <input
            bind:value={fname}
            type="text"
            id="fname"
            name="fname"
            class="inline-flex mb-[1rem] drop-shadow-sm items-center text-gray-600 py-[.5rem] rounded-md border border-border-input bg-background px-[11px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background"
        />

        <div class="participant-detail">
            <div class="left">
                <span class="field-title">Age</span>
                <input
                    inputmode="numeric"
                    type="text"
                    id="age"
                    name="age"
                    class="w-2/3 drop-shadow-sm text-gray-600 py-[.5rem] rounded-md border border-border-input bg-background px-[11px] text-sm transition-colors placeholder:text-foreground-alt/50 focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background"
                />
            </div>
            <div class="right">
                <span class="field-title text-md">Gender</span>
                <Select.Root items={genders}>
                    <Select.Trigger
                        class="inline-flex drop-shadow-sm bg-white text-gray-600 items-center py-[.5rem] rounded-md border border-border-input bg-background px-[11px] py-[6px] text-sm transition-colors placeholder:text-foreground-alt/50  focus:outline-none focus:ring-2 focus:ring-foreground focus:ring-offset-2 focus:ring-offset-background"
                        aria-label="Select a theme"
                    >
                        <Select.Value
                            class="text-sm text-gray-600"
                            placeholder=""
                        />
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            class="ml-auto size-6 text-muted-foreground text-gray-200"
                            viewBox="0 0 256 256"
                            ><rect
                                width="256"
                                height="256"
                                fill="none"
                            /><polyline
                                points="80 176 128 224 176 176"
                                fill="none"
                                stroke="currentColor"
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="16"
                            /><polyline
                                points="80 80 128 32 176 80"
                                fill="none"
                                stroke="currentColor"
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="16"
                            /></svg
                        >
                    </Select.Trigger>
                    <Select.Content
                        class="w-full drop-shadow-sm rounded-xl text-gray-600 border bg-white border-muted bg-background px-1 py-3 shadow-popover outline-none"
                        transition={flyAndScale}
                        sideOffset={8}
                    >
                        {#each genders as gender}
                            <Select.Item
                                class="flex h-10 w-full  select-none items-center rounded-button py-3 pl-5 pr-1.5 text-sm outline-none transition-all duration-75 data-[highlighted]:bg-muted"
                                value={gender.value}
                                label={gender.label}
                            >
                                {gender.label}
                                <Select.ItemIndicator
                                    class="ml-auto"
                                    asChild={false}
                                >
                                    <!-- <Check /> -->
                                </Select.ItemIndicator>
                            </Select.Item>
                        {/each}
                    </Select.Content>
                    <Select.Input name="favoriteFruit" />
                </Select.Root>
            </div>
        </div>

        <Button.Root
            class=" h-10 my-[1.5rem] text-white bg-blue-500 rounded-input bg-dark
	                px-[21px] text-[15px] font-semibold text-background shadow-mini
                    hover:bg-dark/95 active:scale-98 active:transition-all"
        >
            Register
        </Button.Root>
    </div>
{:else}
    <Loader />
{/if}

<style>
    .group-select {
        display: flex;
        background-color: white;

        select{
            width: 100%;
            background-color: white;
            option{
                padding: .5rem;
                background-color: inherit;
            }
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
