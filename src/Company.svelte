<script>

    let companyNumber = [];
    let bedrift;
    async function searchCompany() {
        const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter?navn=${bedrift}&konkurs=false`);
		const data = await res.json();
        companyNumber = data._embedded.enheter;
        console.log(companyNumber);
    }
</script>

<main>
    <div>
        <h3>Søk etter en bedrift</h3>
        <p>Finn org nummer</p>
        <input type="search" name="name" bind:value={bedrift} placeholder="Search for a company">
        <button id="search" on:click={searchCompany}>Search</button>
        {#each companyNumber as match}
            <p>{match.navn} med org nummer: {match.organisasjonsnummer}</p>    
        {/each}
        </div>
        <hr style="height:6px;border-width:0;color:black;background-color:black">
</main>