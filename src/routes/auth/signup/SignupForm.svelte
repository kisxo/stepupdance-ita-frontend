<script>
    import Cookies from "js-cookie";
    import { page } from "$app/stores";
    import { goto } from "$app/navigation";
    import { api_url } from "$lib/config";

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

    function check_phone() {
        let options = {
            method: "GET",
            headers: {
                "Content-Type": "application/json",
            },
        };

        fetch(api_url + `/users/check?phone=` + phone, options)
            .then((response) => {
                if (!response.ok) {
                    // user_exists = false;
                    throw new Error("Network response was not ok");
                }
                return response.json();
            })
            .then((data) => {
                if (data) {
                    goto("/auth/login?redirect=" + redirect_url + "&phone=" + phone);
                } else {
                    new_user = true;
                }
            })
            .catch((error) => {
                console.error("Fetch error:", error);
            });
    }

    function signup(){
        if(password != confirm_password)
        {
            alert("Password must be same!")
            return
        }

        let data = {
            "phone": phone,
            "password": password
        }
        let options = {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(data) 
        };

        fetch(api_url + `/users/`, options)
            .then((response) => {
                if (!response.ok) {
                    // user_exists = false;
                    throw new Error("Network response was not ok");
                }
                return response.json();
            })
            .then((data) => {
                if (data) {
                    goto("/auth/login?redirect=" + redirect_url + "&phone=" + phone);
                } else {
                    alert("Try Again")
                }
            })
            .catch((error) => {
                console.error("Fetch error:", error);
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
        {#if new_user}
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
            <button onclick={signup} class="button" type="button"> SignUp </button>
        {:else}
            <button onclick={check_phone} class="button" type="button"> Verify </button>
        {/if}
        
    </form>
</div>

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
