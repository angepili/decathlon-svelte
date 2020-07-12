<script>
    import AutoComplete from './AutoComplete.svelte'
    import axios from "axios";

    let city = null;
    let cities = [];
    let value = '';

    const decaUrl = 'https://api-eu.preprod.decathlon.net/dktrent/api/v1/sites/full-text-search.json';
    const decaKey = '771e8ae7-04ae-498a-8141-5568016852c5';

    const gooUrl = 'https://maps.googleapis.com/maps/api/place/autocomplete/json';
    const gooKey = 'AIzaSyAbDfHyJhPOkKrObUhecRn2b07CJHLu7fk';
    
    const getCity = e => {
        city = e.target.value;
        axios.get(
            `${decaUrl}?fullTextToSearch=${city}`,
            {
				method: 'GET',
				headers: {
					'x-api-key': decaKey,
					'Content-Type': 'application/json'
				}
			}
        )
        .then(resp => {
            const { data } = resp;
            if(data.length){    
                cities = [];
                data.map(item => {
                    cities.push({
                        id: item.slug,
                        name : item.name
                    })
                })
            } else {
                getCityByGoogleMaps();
            }
        })
    }

    const getCityByGoogleMaps = ()=> {
        const PROXY_URL = 'https://cors-anywhere.herokuapp.com/';
        axios.get(
            `${PROXY_URL}${gooUrl}?input=${city}&key=${gooKey}`,
            {
				method: 'GET',
				headers: {
					'Content-Type': 'application/json'
				}
			}
        )
        .then(resp => {
            const { data } = resp;
            const { predictions } = data;
            if(predictions.length){    
                cities = [];
                predictions.map(item => {
                    cities.push({
                        id: item.place_id,
                        name : item.description
                    })
                })
            }
        })
    }


</script>


<AutoComplete data="{cities}" bind:value="{value}" handleInput={getCity} placeholder={"Seleziona un luogo"} />