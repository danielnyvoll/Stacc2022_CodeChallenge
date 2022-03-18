<script>
let listen = [];
let personData = [];
let companyData = [];
let personInCompanyData = [];


async function info() {
        const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter`);
        const data = await res.json()
        console.log(data)
        for(var i = 0; i < data.length; i++) {
            var obj = json[i];
            console.log(obj.id);
        }
        personInCompanyData = data
    }


async function getInfoAboutRoles() {
        const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter/${nummer}/roller`);
        const data = await res.json()
        console.log(data)
        personInCompanyData = data
    }

async function getInfoAboutCompany() {
        const res = await fetch(`https://code-challenge.stacc.dev/api/roller?orgNr=${nummer}`);
        const data = await res.json()
        console.log(data)
        return data
    }

async function getInfoAboutPeopleInCompany() {
        const res = await fetch(`https://code-challenge.stacc.dev/api/pep?name=${nummer}`);
        const data = await res.json()
        console.log(data)
        return data
    }

async function getName() {
        const res = await fetch(`https://code-challenge.stacc.dev/api/pep?name=${name}`);
        const data = await res.json()
        console.log(data)
        hits = data.numberOfHits
        console.log(hits)
        return data
    }

let name = "";
let nummer = "";
let hits = 0;
</script>

<main>
    <button id="search" on:click={info}>Search</button>
    <p>988971375</p>
    <input type="search" name="company" bind:value={nummer} placeholder="">
    <button id="search" on:click={getInfoAboutRoles}>Search</button>
    {#each companyData as roller}
        <p>{roller}</p>
    {/each}
    <div>
        <p>Search for a person. Example: Knut Arild Hareide</p>
    <input type="search" name="name" bind:value={name} placeholder="Search for a name..">
    <button id="search" on:click={getName}>Search</button>
    
    {#if hits > 0}
        <p>This person is flagged</p>
    {:else}
        <p>{hits}</p>
    {/if}
    </div>

    <div>
        <p>Search for a Company by number. Example: 981078365</p>
        <input type="search" name="company" bind:value={nummer} placeholder="Search for a company by number..">
        <button on:click={() => companyData = getInfoAboutCompany()}>Search</button>
    </div>
</main>