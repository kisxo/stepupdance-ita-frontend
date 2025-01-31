<div class="nav-container box-shadow">
    <a class="nav-title" href="/">Incredible Talent of Assam</a>

    <a class="profile-link" href="/dashboard" aria-labelledby="Profile">
        <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-person-circle" viewBox="0 0 16 16">
            <path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0"/>
            <path fill-rule="evenodd" d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8m8-7a7 7 0 0 0-5.468 11.37C3.242 11.226 4.805 10 8 10s4.757 1.225 5.468 2.37A7 7 0 0 0 8 1"/>
          </svg>
    </a>
</div>

<style>
    .nav-container{
        padding: 1rem;
        display: flex;
        color: var(--white-100);
        background-color: var(--primary-500);
    }

    .nav-title{
        font-weight: bold;
        margin-right: auto;
    }

    .box-shadow {
        box-shadow: 0 2px 8px rgb(0 0 0 / 0.1);
    }

    .profile-link{
        display: flex;
        justify-content: center;
        place-items: center;
        
        & > svg{
            height: 1.5rem;
            width: 1.5rem;
        }
    }

</style>