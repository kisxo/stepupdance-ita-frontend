<script>
    import Cookies from "js-cookie";
    import { page } from "$app/stores";
    import { goto } from "$app/navigation";
    import { api_url } from "$lib/config";
    import { readonly } from "svelte/store";
    import DangerModal from "$lib/components/DangerModal.svelte";
    import axios from "axios";

    const page_url = $page.url;
    let redirect_url;
    if (page_url.searchParams.get("redirect") == null) {
        redirect_url = "/";
    } else {
        redirect_url = page_url.searchParams.get("redirect");
    }

    let new_user = $state(false);
    let phone = $state();
    let password = $state();
    let confirm_password = $state();
    let page_status = $state("CheckPhone");

    let dangerModal_data = $state();
    function showDanger(body, cancel){
        dangerModal_data = {
            "toggle": true,
            "body": body,
            "cancel": cancel
        }
    }

    function check_phone() {
        let options = {
            method: "GET",
            headers: {
                "Content-Type": "application/json",
            },
        };
        axios.get(api_url + '/users/check', {
            params: {
            phone: phone
            }
        })
        .then(function (response) {
            if(response.data.userExists == true)
            {
                goto("/auth/login?redirect=" + redirect_url + "&phone=" +phone );
            }
            else if (response.data.userExists == false)
            {
                page_status = "Signup"
            }
            
        })
        .catch(function (error) {
            if(error.status === 422)
            {
                showDanger("Enter a number !", "Ok")
            }
            if(error.status === 400)
            {
                showDanger(error.response.data.detail, "Ok")
            }
        })
        .finally(function () {
            // always executed
        });  

    }

    async function signup() {
        if (password != confirm_password) {
            showDanger("Password must does not match !", "Okay")
            return;
        }

        let data = {
            phone: phone,
            password: password,
        };
        let options = {
            headers: {
                "Content-Type": "application/json",
            }
        };
        axios.post(api_url + '/users/', data, options)
        .then((response) => {
            goto("/auth/login?redirect=" +redirect_url +"&phone=" +phone);
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

        // fetch(api_url + `/users/`, options)
        //     .then((response) => {
        //         if (!response.ok) {
        //             // user_exists = false;
        //             throw new Error("Network response was not ok");
        //         }
        //         return response.json();
        //     })
        //     .then((data) => {
        //         if (data) {
        //             goto(
        //                 "/auth/login?redirect=" +
        //                     redirect_url +
        //                     "&phone=" +
        //                     phone
        //             );
        //         } else {
        //             alert("Try Again");
        //         }
        //     })
        //     .catch((error) => {
        //         console.error("Fetch error:", error);
        //     });
    }
</script>

<div class="auth-card">
    <form class="form">
        <label for="phone">Phone number:</label>

        {#if page_status == "CheckPhone"}
            <input
                bind:value={phone}
                type="tel"
                id="phone"
                name="phone"
                pattern="[0-9]{3}-[0-9]{2}-[0-9]{3}"
            />
            <button onclick={check_phone} class="button" type="button">
                Verify
            </button>
        {:else if page_status == "Signup"}
            <input
                bind:value={phone}
                type="tel"
                id="phone"
                name="phone"
                pattern="[0-9]{3}-[0-9]{2}-[0-9]{3}"
                readonly
            />
            <label for="password"> Password </label>
            <input
                bind:value={password}
                id="password"
                type="password"
                placeholder="******************"
            />
            <label for="confirm-password">Confirm Password </label>
            <input
                bind:value={confirm_password}
                id="password"
                type="password"
                placeholder="******************"
            />
            <button onclick={signup} class="button" type="button">
                SignUp
            </button>   
        {/if}
    </form>
</div>

<DangerModal {...dangerModal_data}/>

<style>
    .auth-card {
        margin-block: 5rem;
        margin-inline: 1rem;
        padding: 2rem;
        outline: 1px solid var(--primary-200);
        /* background-color: var(--primary-100); */
        background-color: #ffffff;
        border-radius: 0.5rem;
    }

    input {
        outline: 1px solid #dddddd;
        background-color: #f5f5f5;
        padding: 0.4rem;
        border-radius: 0.1rem;
        margin-top: 0.5rem;
        margin-bottom: 1rem;
    }

    label {
        font-weight: bold;
        color: var(--primary-400);
    }

    .form {
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }

    .button {
        font-weight: bold;
        font-size: 0.7rem;
        margin: auto;
        padding-inline: 1rem;
        padding-block: 0.5rem;
        border-radius: 0.4rem;
        background-color: var(--primary-500);
        color: var(--primary-100);
        transition: all 0.5s ease;
    }

    .button:active {
        transform: scale(0.9);
    }
</style>
