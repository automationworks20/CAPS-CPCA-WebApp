<script lang='ts'>
    import { data } from '$lib/translations';
</script>
<div class="container">
    {#each $data.cards as card}
        <div class="card">
            <h1>{card.title}</h1>
            <p>{card.desc}</p>
            <a class="primary" href={card.link[0]}>{card.link[1]}</a>
        </div>
    {/each}
</div>

<style>
    .card {
        display: flex;
        flex-direction: column;
        padding: 4rem 7rem;
        gap: 3rem;
        transition: all 0.3s ease-in-out;
    }
    .card:hover {
        background-color: #2094f32a;
    }
    .card:first-of-type:hover h1 {
        color: var(--Prescribing);
    }
    .card:nth-of-type(2):hover h1 {
        color: var(--Dispensing);
    }
    .card:last-of-type:hover h1 {
        color: var(--Supporting);
    }
    h1, p {
        color: #EAEAEA;
    }
</style>