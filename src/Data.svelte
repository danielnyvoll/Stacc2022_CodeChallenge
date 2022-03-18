<script>
import { onMount } from 'svelte';
let companyData = [];
let personInCompanyData = [];
let personHits;
let job;
let bedrift;
let companyNumber= [];


async function searchCompany() {
        const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter?navn=${bedrift}&konkurs=false`);
		const data = await res.json();
        companyNumber = data._embedded.enheter;
        console.log(companyNumber);
}

async function searchEmployees() {
        const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter/${nummer}/roller`);
		const data = await res.json();
        companyNumber = data.rollegrupper;
        console.log(companyNumber);
}


async function getName() {
        const res = await fetch(`https://code-challenge.stacc.dev/api/pep?name=${name}`);
        const data = await res.json()
        console.log(data)
        hits = data.numberOfHits
        personHits = data.hits[0].aliases
        job = data.hits[0].dataset
        console.log(job)
        console.log(personHits)
        console.log(hits)
    }

let name = "";
let hits="";
let nummer;
let sjef;
</script>

<main>
    <p>Finn org nummer</p>
    <input type="search" name="name" bind:value={bedrift} placeholder="Search for a company">
    <button id="search" on:click={searchCompany}>Search</button>
    {#each companyNumber as match}
        <p>{match.navn} med org nummer: {match.organisasjonsnummer}</p>    
    {/each}

<div>
    <input type="search" name="name" bind:value={nummer} placeholder="org:nummer">
    <button id="search" on:click={searchEmployees}>Search</button>
    {#each companyNumber as roles}
    {console.log(roles)}
        {#each roles.roller as ansatt}
            {name = ansatt.person.navn.etternavn}
            {getName}
            <p>{name}</p>
        {/each}
    {/each}
</div>


    <div>
        <p>Search for a person. Example: Knut Arild Hareide</p>
    <input type="search" name="name" bind:value={name} placeholder="Search for a name..">
    <button id="search" on:click={getName}>Search</button>
    
    {#if hits > 0}
        <p>{personHits} er flagget</p>
    {:else}
        <p>Personen du søkte etter er ikke flagget</p>
    {/if}
    </div>
</main>