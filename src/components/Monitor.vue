<template>
<div>
	<div v-if='loading'>
		<h4>Loading</h4>
	</div>
	<div v-else class="col-12 row mx-auto text-left">
		<div class="bg-shadow mr-lg-2 rounded col-lg col-12 p-4">
			<h4 class='text-muted'>Most infected:</h4>
			<h1 class='font-weight-bold'>{{most.country}}</h1>
			<h1 class='display-1 text-danger font-weight-bold'>{{most.cases.total}}</h1>
			<h4 class='opacity font-weight-bold'><span class='text-danger'>{{most.cases.new}}</span> TODAY</h4>
		</div>
		<div class="bg-shadow ml-lg-2 mt-lg-0 mt-3 rounded col-lg col-12 p-4">
			<h4 class='text-muted'>Least infected:</h4>
			<h1 class='font-weight-bold'>{{least.country}}</h1>
			<h1 class='display-1 text-danger font-weight-bold'>{{least.cases.total}}</h1>
			<h4 class='opacity font-weight-bold'><span class='text-danger'>{{least.cases.new ? least.cases.new : 0}}</span> TODAY</h4>
		</div>
		<input v-model='country' v-on:keyup.enter="getCasesInCountry" placeholder="Type your country! E.g. USA" class='form-control text-lg col-12 my-3'>
        <div v-if='countryCases' class="bg-shadow col p-4 mb-4 rounded">
            <h4 class='text-muted'>Selected country:</h4>
            <h1 class='font-weight-bold'>{{countryCases[0].country}}</h1>
            <h1 class='display-1 text-danger font-weight-bold'>{{countryCases[0].cases.total}}</h1>
            <h4 class='opacity font-weight-bold'><span class='text-danger'>{{countryCases[0].cases.new ? countryCases[0].cases.new : 0}}</span> TODAY</h4>
        </div>
		<!--table class='table table-bordered'>
			<thead>
				<tr class='bg-shadow'>
					<th scope="col">#</th>
					<th scope="col">Country</th>
					<th scope="col">Cases</th>
					<th scope="col">Deaths</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for='(item, i) in covidCases' :key='item.country'>
					<th>{{i+1}}</th>
					<td>{{item.country}}</td>
					<td>{{item.cases.total}}</td>
					<td>{{item.deaths.total}}</td>
				</tr>
			</tbody>
		</table-->
	</div>
</div>
</template>

<script>
export default {
	name: 'Monitor',
	data: () => ({
		loading: true,
		covidCases: null,
		most: null,
		least: null,
		country: '',
		countryCases: null
	}),
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
		this.loading = false;
	},
	methods: {
		max(countries) {
			this.most = countries.sort((a, b) => b.cases.total - a.cases.total)[0];
		},
		min(countries) {
			this.least = countries.sort((a, b) => a.cases.total - b.cases.total)[0];
		},
		async getCasesInCountry() {
			let result = await fetch(`https://covid-193.p.rapidapi.com/statistics?country=${this.country}`, {
				"method": "GET",
				"headers": {
					"x-rapidapi-host": "covid-193.p.rapidapi.com",
					"x-rapidapi-key": process.env.VUE_APP_API_KEY
				}
			});
			this.countryCases = (await result.json()).response;

            if(this.countryCases.length === 0) {
                this.countryCases = null
            }
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
</style>
