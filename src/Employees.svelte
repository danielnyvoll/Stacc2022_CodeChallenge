<script>
    let name;
    let nummer;
    let companyNumber = [];

    async function searchEmployees() {
        const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter/${nummer}/roller?fraAntallAnsatte=1`);
		const data = await res.json();
        companyNumber = data.rollegrupper;
        console.log(companyNumber);
    }

</script>

<main>
    <h3>Ansatte i bedrift med org nummer</h3>
    <p>Finn ansatte i en bedrift</p>
    <div>
        <input type="search" name="name" bind:value={nummer} placeholder="org:nummer">
        <button id="search" on:click={searchEmployees}>Search</button>
        {#each companyNumber as roles}
            {console.log(roles)}
            {#each roles.roller as ansatt}
                <p>{ansatt.person.navn.fornavn} {ansatt.person.navn.etternavn} </p>
        {/each}
    {/each}
    </div>
    <hr style="height:6px;border-width:0;color:black;background-color:black">
</main>