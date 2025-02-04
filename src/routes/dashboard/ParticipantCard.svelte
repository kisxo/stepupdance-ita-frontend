<script>
    let participant = $props();
</script>

<div class="card-container box-shadow bg-white">
    <div class="heading">
        <span class="title"> {participant.event_detail.title} </span>
        <span class="event-detail">
            {participant.event_detail.date}
            <br />
            &#8377; {participant.event_detail.amount}
        </span>
    </div>

    <div class="participant-detail">
        {#if participant.event_detail.title == "Dance Duo"}
            <span> Fist: {participant.duo_name1} </span>
            <span> Second: {participant.duo_name2} </span>
        {:else if participant.event_detail.title == "Dance Group"}
            <span> Group Name: {participant.group_name} </span>
        {:else}
            <span> Name: {participant.fname} </span>
            <span>Age: {participant.age}</span>
            <span>Gender: {participant.gender}</span>
        {/if}
        <span> Payment Method: {participant.payment_method}</span>
        <span class="payment-status">
            Payment Status:

            {#if participant.payment_status}
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor"
                    class="bi bi-check-circle complete"
                    viewBox="0 0 16 16"
                >
                    <path
                        d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14m0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16"
                    />
                    <path
                        d="m10.97 4.97-.02.022-3.473 4.425-2.093-2.094a.75.75 0 0 0-1.06 1.06L6.97 11.03a.75.75 0 0 0 1.079-.02l3.992-4.99a.75.75 0 0 0-1.071-1.05"
                    />
                </svg>
            {:else}
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor"
                    class="bi bi-hourglass-split pending"
                    viewBox="0 0 16 16"
                >
                    <path
                        d="M2.5 15a.5.5 0 1 1 0-1h1v-1a4.5 4.5 0 0 1 2.557-4.06c.29-.139.443-.377.443-.59v-.7c0-.213-.154-.451-.443-.59A4.5 4.5 0 0 1 3.5 3V2h-1a.5.5 0 0 1 0-1h11a.5.5 0 0 1 0 1h-1v1a4.5 4.5 0 0 1-2.557 4.06c-.29.139-.443.377-.443.59v.7c0 .213.154.451.443.59A4.5 4.5 0 0 1 12.5 13v1h1a.5.5 0 0 1 0 1zm2-13v1c0 .537.12 1.045.337 1.5h6.326c.216-.455.337-.963.337-1.5V2zm3 6.35c0 .701-.478 1.236-1.011 1.492A3.5 3.5 0 0 0 4.5 13s.866-1.299 3-1.48zm1 0v3.17c2.134.181 3 1.48 3 1.48a3.5 3.5 0 0 0-1.989-3.158C8.978 9.586 8.5 9.052 8.5 8.351z"
                    />
                </svg>
            {/if}
        </span>
    </div>
</div>

<style>
    .box-shadow {
        /* box-shadow: 0 4px 10px rgb(0 0 0 / 0.15); */
        box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 10px;
    }

    .card-container {
        /* outline: 1px solid grey; */
        display: flex;
        flex-direction: column;
        padding: 1rem;
        gap: 0.5rem;
        border-radius: 0.4rem;
    }
    .heading {
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-weight: bold;
        margin-block: 0.5rem;
    }

    .title {
        font-size: 0.9rem;
        background-color: #16c47f;
        border-radius: 0 2rem 2rem 0;
        padding: 0.3rem 1.5rem;
        color: white;
    }

    .event-detail {
        text-align: right;
        font-size: 0.7rem;
    }

    .participant-detail {
        display: flex;
        flex-direction: column;
        gap: 0.2rem;
        font-size: 0.7rem;
    }

    .payment-status {
        display: flex;
        flex-direction: row;
        align-items: center;
        gap: 1rem;

        & > svg {
            height: 12px;
            width: 12px;
        }

        .pending {
            color: red;
        }
        .complete {
            color: green;
        }
    }
</style>
