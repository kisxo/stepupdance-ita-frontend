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
            <a href="/register/form?category=dance">Next
                <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-arrow-up-right-circle-fill" viewBox="0 0 16 16">
                    <path d="M0 8a8 8 0 1 0 16 0A8 8 0 0 0 0 8m5.904 2.803a.5.5 0 1 1-.707-.707L9.293 6H6.525a.5.5 0 1 1 0-1H10.5a.5.5 0 0 1 .5.5v3.975a.5.5 0 0 1-1 0V6.707z"/>
                  </svg>
            </a>
        </div>
    </div>

    <div class="category-card">
        <div class="card-detail">
            <span class="card-title"> Fashion </span>
            <a href="/register/fashion">Next
                <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-arrow-up-right-circle-fill" viewBox="0 0 16 16">
                    <path d="M0 8a8 8 0 1 0 16 0A8 8 0 0 0 0 8m5.904 2.803a.5.5 0 1 1-.707-.707L9.293 6H6.525a.5.5 0 1 1 0-1H10.5a.5.5 0 0 1 .5.5v3.975a.5.5 0 0 1-1 0V6.707z"/>
                  </svg>
            </a>
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
            <a href="/register/art">Next
                <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-arrow-up-right-circle-fill" viewBox="0 0 16 16">
                    <path d="M0 8a8 8 0 1 0 16 0A8 8 0 0 0 0 8m5.904 2.803a.5.5 0 1 1-.707-.707L9.293 6H6.525a.5.5 0 1 1 0-1H10.5a.5.5 0 0 1 .5.5v3.975a.5.5 0 0 1-1 0V6.707z"/>
                  </svg>
            </a>
        </div>
    </div>

    <div class="category-card">
        <div class="card-detail">
            <span class="card-title"> Cooking </span>
            <a href="/register/cooking">Next
                <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-arrow-up-right-circle-fill" viewBox="0 0 16 16">
                    <path d="M0 8a8 8 0 1 0 16 0A8 8 0 0 0 0 8m5.904 2.803a.5.5 0 1 1-.707-.707L9.293 6H6.525a.5.5 0 1 1 0-1H10.5a.5.5 0 0 1 .5.5v3.975a.5.5 0 0 1-1 0V6.707z"/>
                  </svg>
            </a>
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
            <a href="/register/yoga">Next
                <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-arrow-up-right-circle-fill" viewBox="0 0 16 16">
                    <path d="M0 8a8 8 0 1 0 16 0A8 8 0 0 0 0 8m5.904 2.803a.5.5 0 1 1-.707-.707L9.293 6H6.525a.5.5 0 1 1 0-1H10.5a.5.5 0 0 1 .5.5v3.975a.5.5 0 0 1-1 0V6.707z"/>
                  </svg>
            </a>
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
        border-top: 1px solid var(--primary-500);
        border-right: 1px solid var(--primary-500);
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
        text-align: center;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;

        & > a {
            background-color: var(--primary-400);
            color: var(--white);
            border-radius: 1.5rem;
            font-size: .7rem;
            gap: 1rem;
            padding: .4rem .5rem .4rem 1rem;
            display: flex;
            flex-direction: row;
            justify-content: center;
            align-items: center;

            & > svg{
                height:20px;
                width: 20px;
            }
        }
    }

    .card-title{
        font-weight: bold;
        font-size: 1.5rem;
    }

</style>
