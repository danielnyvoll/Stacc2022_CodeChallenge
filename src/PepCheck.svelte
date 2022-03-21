<script>
    import { navn } from './store.js';
    let name = "";
    let hits;
    let personHits;

    async function getName() {
        navn.update(navn => name);
        const res = await fetch(`https://code-challenge.stacc.dev/api/pep?name=${name}`);
        const data = await res.json()
        hits = data.numberOfHits
        console.log(data.hits)
        personHits = data.hits[0].name
    }
</script>

<main>
    <div>
        <h3>Pep-Check</h3>
        <p>Search for a person. Example: Knut Arild Hareide</p>
    <input type="search" name="name" bind:value={name} placeholder="Søk etter person..">
    <button id="search" on:click={getName}>Søk</button>
    {#if hits > 0}
        <p>{personHits} er flagget</p>
    {:else}
        <p>Personen er ikke flagget</p>
    {/if}
    </div>
    <hr style="height:6px;border-width:0;color:black;background-color:black">
</main>