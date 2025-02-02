<script>
    import Cookies from "js-cookie"
    import { page } from "$app/stores";
    import { goto } from "$app/navigation";
    import { api_url } from "$lib/config";

    const page_url = $page.url;
    let redirect_url
    let phone = $state();
    let password = $state();
    if (page_url.searchParams.get("redirect") == null)
    {
        redirect_url  = "/"
    }
    else
    {   
        phone = page_url.searchParams.get("phone")
        redirect_url = page_url.searchParams.get("redirect")
    }
    
    

    function login() {
        let data = {
            "phone": phone,
            "password": password
        }
        let options = {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
            body: JSON.stringify(data) 
        };

        fetch(api_url+`/token/`, options)
        .then(response => {
            if (!response.ok) {
                // user_exists = false;
                throw new Error('Network response was not ok');
            }
            return response.json();
        })
        .then(data => {
            Cookies.set('jwt', data.access_token)
            goto(redirect_url)
        })
        .catch(error => {
            console.error('Fetch error:', error);
        });

    }

</script>

<div class="login-container">
    <div class="login-card">
        <form class="login-form">
            <label for="phone">Phone number:</label>
            <input bind:value={phone} type="tel" id="phone" name="phone" pattern="[0-9]{3}-[0-9]{2}-[0-9]{3}">

            <label for="password"> Password </label>
            <input
            bind:value={password}
                id="password"
                type="password"
                placeholder="******************"
            />

            <button onclick={login} class="login-button" type="button"> Sign In </button>
        </form>
    </div>
</div>

<style>
    input{
        outline: 1px solid #dddddd;
        padding: .4rem;
        border-radius: .1rem;
        margin-top: .5rem;
        margin-bottom: 1rem;
    }

    label{
        font-weight: bold;
        color: #555555;
    }

    .login-container {
        margin-top: 5rem;
        display: flex;
        justify-content: center;
    }

    .login-card {
        width: 60%;
        display: flex;
        justify-content: center;
        outline: 1px solid #cccccc;
        border-radius: .25rem;
        padding-block: 2rem;
    }

    .login-form {
        width: 80%;
        display: flex;
        flex-direction: column;
        gap: .5rem; 
    }

    .login-button{
        padding: .25rem;
        background-color: cornflowerblue;
        transition: all 0.5s ease;
        width: 50%;
        margin: auto;
    }

    .login-button:active{
        transform: scale(0.95);
    }
</style>
