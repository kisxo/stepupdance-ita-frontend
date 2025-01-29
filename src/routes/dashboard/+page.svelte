<script>
    import Cookies from "js-cookie"
    import { page } from "$app/stores";
    import { goto } from '$app/navigation';
    const url = "http://localhost:8000";

    const page_url = $page.url;

    let options = {
        method: 'GET',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': "Bearer " + Cookies.get("jwt")
        }
    };

    fetch(url+'/users', options)
    .then(response => {
        if(response.status === 401)
        {
            console.error("Auth Failed")
            goto('/auth/login?redirect=' + page_url.pathname);
        }
        return response.json();
    })
    .then(data => {
        console.log(data)
    })
    .catch(error => {
        console.log(Response.code)
    });

</script>