<script>
    import EventCards from "./EventCards.svelte";
</script>

<div class="hero">
    <span class="hero-title">Step Up<br> And <br>Let The <br> Talents Shine!</span>
    <span class="hero-text">Register now for Incredible Talent of Assam <br> Showcase your incredible talents at this stage <br> Win an opportunity to work with Step Up Production!</span>
    <div class="hero-button-container">
        <a class="hero-register" href="">Register</a>
        <a class="hero-login" href="/dashboard">Profile</a>
    </div>
</div>

<style>
    .hero{
        padding-block: 2rem;
        display: flex;
        flex-direction: column;
        gap: 1rem;
        text-align: center;
        color: rgb(0, 0, 0);
    }

    .hero-title{
        font-size: 1.8rem;
        font-weight: bold;
    }

    .hero-text{
        font-size: .8rem;
        padding-block: .5rem;
    }

    .hero-button-container{
        display: flex;
        flex-direction: row;
        justify-content: center;
        gap: 15px;

        & > a {
            width: 30%;
            padding-block: .5rem;
            border-radius: .2rem;
        }

        .hero-register{
            background-color: black;
            color: white;
        }

        .hero-login{
            outline: 1px solid #666666;
        }
    }
</style>