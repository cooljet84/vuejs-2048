<template>
	<div>
		<h4>Affichage des scores de l'API</h4>
		<router-link to="/" tag="button" class="button back-button">Retour</router-link>
		<table class="score-table">
			<thead>
				<tr>
					<th>Pseudo</th>
					<th>Score</th>
					<th>Temps</th>
				</tr>
			</thead>
			<tbody>
				<tr class="line-score" v-for="(score, index) in orderedScores" :key="index" :index="index">
					<td class="col-score">{{ score.nickname }}</td>
					<td class="col-score">{{ score.score }}</td>
					<td class="col-score">{{ score.time }}</td>
				</tr>
			</tbody>
		</table>
	</div>	
</template>

<script>
import http from '@/utils/http'
import _ from 'lodash'

export default {
	name: 'Scores',
	data() {
		return {
			scores: []
		}
	},
	methods: {
		loadScores() {
			http
				.get('json')
				.then(resp => {
					this.scores = resp.data
				})
				.catch(err => console.log(err))
		}
	},
	created() {
		this.loadScores()
	},
	computed: {
		// need to fix the api, for prevent data with a wrong format
		orderedScores: function() {
			return _.orderBy(this.scores, 'score', 'desc')
		}
	}
}

</script>

<style>
.back-button{
	position: absolute;
	top: 10px;
}

.score-table{
	display: -webkit-inline-box;
	border: 1px solid black;
	overflow: auto;
	max-width: 1250px;
}

.col-score{
	max-width: 250px;
	overflow: hidden;
	padding: 10px;
}
</style>