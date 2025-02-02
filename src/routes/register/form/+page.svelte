<script>
    import Cookies from "js-cookie";
    import { page } from "$app/stores";
    import { goto } from "$app/navigation";
    import { api_url } from "$lib/config";

    import Loader from "$lib/components/Loader.svelte";
    import RegisterForm from "./RegisterForm.svelte";

    let auth_state = $state(false);

    const page_url = $page.url;

    let category = $state();

    if (page_url.searchParams.get("category") == null) {
        goto("/register");
    } else {
        category = page_url.searchParams.get("category");
    }

    let options = {
        method: "GET",
        headers: {
            "Content-Type": "application/json",
            Authorization: "Bearer " + Cookies.get("jwt"),
        },
    };

    fetch(api_url + "/users/", options)
        .then((response) => {
            if (response.status === 401) {
                console.error("Auth Failed");
                goto(
                    "/auth/signup?redirect=" +
                        page_url.pathname +
                        "?category=" +
                        category
                );
            }
            return response.json();
        })
        .then((data) => {
            auth_state = true;
            pass;
        })
        .catch((error) => {});
</script>

{#if auth_state}
    <RegisterForm {category} />
{:else}
    <Loader />
{/if}
