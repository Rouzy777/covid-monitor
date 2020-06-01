<template>
<div>
	<Loader v-if='!covidCases' />
	<div v-else class="col-12 row mx-auto text-left">
		<div class="bg-shadow mr-lg-2 rounded col-lg col-12 p-4">
			<h4 class='text-muted'>Most infected:</h4>
			<h1 class='font-weight-bold'>{{most.country}}</h1>
			<h1 class='responsive-text text-danger font-weight-bold'>{{most.cases.total}}</h1>
			<h4 class='opacity font-weight-bold'><span class='text-danger'>{{most.cases.new}}</span> TODAY</h4>
		</div>
		<div class="bg-shadow ml-lg-2 mt-lg-0 mt-3 rounded col-lg col-12 p-4">
			<h4 class='text-muted'>Least infected:</h4>
			<h1 class='font-weight-bold'>{{least.country}}</h1>
			<h1 class='responsive-text text-danger font-weight-bold'>{{least.cases.total}}</h1>
			<h4 class='opacity font-weight-bold'><span class='text-danger'>{{least.cases.new ? least.cases.new : 0}}</span> TODAY</h4>
		</div>
	</div>
	<div class="col-12 row mx-auto text-left mt-3">
		<div class="bg-shadow mr-lg-2 rounded col-lg col-12 p-4 min-h-special">
			<input v-model='country' placeholder="Type your country... E.g. USA" class='form-control text-lg mb-3'>
			<div v-if='countryCases'>
				<h4 class='text-muted'>Selected:</h4>
				<h1 class='font-weight-bold'>{{countryCases[0].country}}</h1>
				<h1 class='responsive-text text-danger font-weight-bold'>{{countryCases[0].cases.total}}</h1>
				<h4 class='opacity font-weight-bold'><span class='text-danger'>{{countryCases[0].cases.new ? countryCases[0].cases.new : 0}}</span> TODAY</h4>
			</div>
		</div>
		<div class="bg-shadow ml-lg-2 mt-lg-0 mt-3 mb-lg-0 mb-3 rounded col-lg col-12 p-4 min-h-special">
			<router-link to='/all'>
				<h3 class='text-light bg-primary rounded p-2'>See all countries</h3>
			</router-link>
			<div class="text-center mt-5">
				<h1 style='font-size: 100px'>🧼👏</h1>
				<h2 class='mt-4'>Don't forget to wash your hands!</h2>
			</div>
		</div>
	</div>
</div>
</template>

<script>
import Loader from '@/components/Loader.vue'

export default {
	name: 'Monitor',
	data: () => ({
		covidCases: null,
		most: null,
		least: null,
		country: '',
		countryCases: null
	}),
    components: {
        Loader
    },
	async created() {
		let result = await fetch("https://covid-193.p.rapidapi.com/statistics", {
			"method": "GET",
			"headers": {
				"x-rapidapi-host": "covid-193.p.rapidapi.com",
				"x-rapidapi-key": process.env.VUE_APP_API_KEY
			}
		});

		this.covidCases = (await result.json()).response;
		let countries = this.covidCases.filter(i => i.country !== "All");
		this.max(countries);
		this.min(countries);
	},
    watch: {
        country(typed) {
            this.covidCases.map(async i => {
                if(typed.toLowerCase() == i.country.toLowerCase()) {
                    let result = await fetch(`https://covid-193.p.rapidapi.com/statistics?country=${this.country}`, {
                        "method": "GET",
                        "headers": {
                            "x-rapidapi-host": "covid-193.p.rapidapi.com",
                            "x-rapidapi-key": process.env.VUE_APP_API_KEY
                        }
                    });
                    this.countryCases = (await result.json()).response;
                }
            })
        }
    },
	methods: {
		max(countries) {
			this.most = countries.sort((a, b) => b.cases.total - a.cases.total)[0];
		},
		min(countries) {
			this.least = countries.sort((a, b) => a.cases.total - b.cases.total)[0];
		}
	}
}
</script>

<style scoped media="screen">
.bg-shadow {
	background-color: rgba(0, 0, 0, 0.06)
}

.opacity {
	opacity: 0.75
}

.text-lg {
	font-size: 25px
}

a:hover {
	text-decoration: none
}

.min-h-special {
	min-height: 379px
}

.responsive-text {
    font-size: 7.4vmax
}
</style>
