<script>
    import { navn } from './store.js';
    let person = " ";
    let companyNumber = [];
    let bedrift = " ";
    const map = new Map();
    let test;
    navn.subscribe(navn => {
		test = navn;
	});

    async function searchCompany() {
        const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter?size=10000`);//Finner 10 000 bedrifter
		const data = await res.json();
        for(let i =0; i < data._embedded.enheter.length; i++) {
            companyNumber.push(data._embedded.enheter[i].organisasjonsnummer);//Legger til alle org nummer
        }

        for(let j=0; j<companyNumber.length; j++) {
            let nummer = companyNumber[j];
           
            try {

                const res = await fetch(`https://data.brreg.no/enhetsregisteret/api/enheter/${nummer}/roller`);

//Går igjennom alle rollene og finner navn og etternavn som legges til i map med: key = org.nummer og value = fornavn + etternavn
                if (res.ok) {
                    const data = await res.json();
                    for(let k=0; k<data.rollegrupper.length; k++) {
                        try {
                         map.set(nummer, data.rollegrupper[k].roller[k].person.navn.fornavn + " " + data.rollegrupper[k].roller[k].person.navn.etternavn);
                        }
                catch (e) {
                    break;
                    console.log(e instanceof TypeError) //Hvis det ikke finnes personer registrert
                }
                map.forEach(function(value, key) {
                if(test == value){
                    bedrift = key;
                    person = value;
                }
                })
             }
            }
            
            } catch (error) {
            console.log(Nummer + " failed" + error);
    };

    }    
    }
</script>

<main>
    <h3>Finn ut hvor personen du sjekket jobber</h3>
    <p>{person} jobber i {bedrift}</p>
    <div>
        <button id="search" on:click={searchCompany}>Se hvor personen jobber</button>
        {#each companyNumber as match}
            <p>{match.navn} med org nummer: {match.organisasjonsnummer}</p>    
        {/each}
        </div>
        <hr style="height:6px;border-width:0;color:black;background-color:black">
</main>