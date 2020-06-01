<template>
<div>
    <Loader v-if='!covidCases' />
    <table v-else class='table table-bordered'>
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
	</table>
</div>
</template>

<script>
import Loader from '@/components/Loader.vue'

export default {
	data: () => ({
		covidCases: null
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
	}
}
</script>

<style lang="css" scoped>
</style>
