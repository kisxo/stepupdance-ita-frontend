<div class="category-container">
    <div class="category-card">
        <div class="card-image-container">
            <img src="/images/hiphop.png" alt="">
        </div>
        <div class="card-detail">
            <span class="card-title"> Dance </span>
        </div>
    </div>
    <div class="category-card">
        <span class="card-title"> Fashion </span>
    </div>
    <div class="category-card">
        <span class="card-title"> Art </span>
    </div>
    <div class="category-card">
        <span class="card-title"> Yoga </span>
    </div>
    <div class="category-card">
        <span class="card-title"> Cooking </span>
    </div>
</div>

<style>
    .category-container {
        outline: 1px solid grey;
        display: flex;
        flex-direction: column;
        padding: 1rem;
        gap: 1.5rem;
    }

    .category-card {
        /* background-color: #E43D12; */
        box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 10px;
        border-radius: 0.2rem;
        padding: 1rem;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        /* color: white; */
    }

    .card-image-container{
        width: 40%;
        border-radius: .3rem;
        border-left: 4px solid rgb(228, 87, 87);
        border-bottom: 4px solid rgb(231, 74, 74);

        & > img{
            filter: drop-shadow(-7px 7px 4px #333333);
        }

    }

    .card-detail{
        width: 50%;
        display: block;
        place-content: center;
    }

    .card-title{
        font-weight: bold;
        font-size: 3rem;
    }

</style>
