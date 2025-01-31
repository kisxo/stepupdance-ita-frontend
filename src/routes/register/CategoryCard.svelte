<div class="category-container">
    <span class="category-title">
        Choose a Category
    </span>
    <div class="category-card">
        <div class="card-image-container">
            <img src="/images/hiphop.png" alt="">
        </div>
        <div class="card-detail">
            <span class="card-title"> Dance </span>
        </div>
    </div>

    <div class="category-card">
        <div class="card-detail">
            <span class="card-title"> Fashion </span>
        </div>
        <div class="card-image-container">
            <img src="/images/fashion-miss.png" alt="">
        </div>
    </div>

    <div class="category-card">
        <div class="card-image-container">
            <img src="/images/art.png" alt="">
        </div>
        <div class="card-detail">
            <span class="card-title"> Art </span>
        </div>
    </div>

    <div class="category-card">
        <div class="card-detail">
            <span class="card-title"> Cooking </span>
        </div>
        <div class="card-image-container">
            <img src="/images/cooking.png" alt="">
        </div>
    </div>

    <div class="category-card">
        <div class="card-image-container">
            <img src="/images/yoga.png" alt="">
        </div>
        <div class="card-detail">
            <span class="card-title"> Yoga </span>
        </div>
    </div>
</div>

<style>
    .category-container {
        display: flex;
        flex-direction: column;
        padding: 1rem;
        gap: 1.5rem;
    }

    .category-title{
        color: var(--primary-500);
        text-align: center;
        font-weight: bold;
        font-size: 2.3rem;
    }

    .category-card {
        border-radius: 0.5rem;
        padding: 1rem;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        background-color: var(--primary-100);
        color: var(--primary-400);
        border-top: 1px solid var(--accent-500);
        border-right: 1px solid var(--accent-500);
        border-left: 5px solid var(--primary-500);
        border-bottom: 5px solid var(--primary-500);
    }

    .card-image-container{
        width: 40%;
        border-radius: .3rem;
        height: 9rem;
        display: flex;
        justify-content: center;
        align-items: center;
        & > img{
            max-height: 100%;
            filter: drop-shadow(-2px 2px 8px #666666);
        }

    }

    .card-detail{
        /* background-color: var(--white); */
        width: 50%;
        display: block;
        place-content: center;
        text-align: center;
    }

    .card-title{
        font-weight: bold;
        font-size: 2.1rem;
    }

</style>
