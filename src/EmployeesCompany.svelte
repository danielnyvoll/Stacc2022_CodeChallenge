<script>
    import { navn } from './store.js';
    let person;
    let companyNumber = [];
    let bedrift;
    const map = new Map();
    let test;
    navn.subscribe(navn => {
		test = navn;
	});

    async function searchCompany() {
        const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter`);//Finner alle bedrifter
		const data = await res.json();
        console.log(data);
        for(let i =0; i < data._embedded.enheter.length; i++) {
            companyNumber.push(data._embedded.enheter[i].organisasjonsnummer);//Legger til alle org nummer. MAX 20
        }
        console.log(companyNumber);
        console.log(companyNumber.length);
        for(let j=0; j<3; j++) {
            let nummer = companyNumber[j];
            console.log(nummer);
            const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter/${nummer}/roller?fraAntallAnsatte=2`);//Finner roller i bedriftene
		    const data = await res.json();
            console.log(data.rollegrupper);

//Går igjennom alle rollene og finner navn og etternavn som legges til i map med: key = org.nummer og value= fornavn + etternavn
            for(let k=0; k<data.rollegrupper.length; k++) {
                console.log(data);
                map.set(nummer, data.rollegrupper[k].roller[k].person.navn.fornavn + " " + data.rollegrupper[k].roller[k].person.navn.etternavn);
            }
        }
        console.log(map);
            map.forEach(function(value, key) {
                if(test == value){
                    bedrift = key;
                    person = value;
                }
	            console.log(key + " = " + value);
        })
    }
</script>

<main>
    <h3>Finn jobb og om ansatte er flagget</h3>
    <p>{person} jobber i {bedrift}</p>
    <div>
        <button id="search" on:click={searchCompany}>Se hvor personen jobber</button>
        {#each companyNumber as match}
            <p>{match.navn} med org nummer: {match.organisasjonsnummer}</p>    
        {/each}
        </div>
        <hr style="height:6px;border-width:0;color:black;background-color:black">
</main>