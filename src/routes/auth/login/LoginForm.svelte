<script>
    import Cookies from "js-cookie";
    import { page } from "$app/stores";
    import { goto } from "$app/navigation";
    import { api_url } from "$lib/config";
    import SuccessModal from "$lib/components/SuccessModal.svelte";
    import DangerModal from "$lib/components/DangerModal.svelte";
    import axios from "axios";

    const page_url = $page.url;
    let redirect_url;
    let phone = $state();
    let password = $state();
    if (page_url.searchParams.get("redirect") == null) {
        redirect_url = "/";
    } else {
        phone = page_url.searchParams.get("phone");
        redirect_url = page_url.searchParams.get("redirect");
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

    function login() {
        let data = {
            "phone": phone,
            "password": password,
        };
        let options = {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            }
        };

        axios.post(api_url + '/token/', data, options)
        .then((response) => {
            if (response.status ===200)
            {
                Cookies.set("jwt", response.data.access_token);
                goto(redirect_url);
            }
        })
        .catch(function (error) {
            if(error.status === 422)
            {
                showDanger("Enter both Phone and Password !", "Ok")
            }
            if(error.status === 401)
            {
                showDanger(error.response.data.detail, "Ok")
            }
        });
    }
</script>

<div class="auth-card">
    <form class="form">
        <label for="phone">Phone number:</label>
        <input
            bind:value={phone}
            type="tel"
            id="phone"
            name="phone"
            pattern="[0-9]{3}-[0-9]{2}-[0-9]{3}"
        />
        <label for="password"> Password </label>
        <input
            bind:value={password}
            id="password"
            type="password"
            placeholder="******************"
        />
        <button onclick={login} class="button" type="button"> Login </button>
    </form>
</div>

<DangerModal {...dangerModal_data}/>
<SuccessModal {...successModal_data}/>

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
