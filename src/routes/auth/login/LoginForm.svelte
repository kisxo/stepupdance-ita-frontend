<script>
    import Cookies from "js-cookie";
    import { page } from "$app/stores";
    import { goto } from "$app/navigation";
    import { api_url } from "$lib/config";

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

    function login() {
        let data = {
            phone: phone,
            password: password,
        };
        let options = {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(data),
        };

        fetch(api_url + `/token/`, options)
            .then((response) => {
                if (!response.ok) {
                    // user_exists = false;
                    throw new Error("Network response was not ok");
                }
                return response.json();
            })
            .then((data) => {
                Cookies.set("jwt", data.access_token);
                goto(redirect_url);
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
