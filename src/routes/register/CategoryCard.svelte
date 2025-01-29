<div class="category-container">
    <div class="category-card">
        <div class="card-image">
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
        padding: 2rem;
        gap: 1.5rem;
    }

    .category-card {
        box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 10px;
        border-radius: 0.2rem;
        padding: 1rem;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }

    .card-image{
        width: 40%;
        background-color: rgb(0, 106, 235);
        border-radius: .3rem;
        border-left: 4px solid red;
        border-bottom: 4px solid red;
    }

    .card-detail{
        width: 50%;

    }

    .card-title{
        font-weight: bold;
        font-size: .8rem;
    }

</style>
