<template>
	<b-nav tabs>
		<b-nav-item disabled>Quiz App</b-nav-item>
		<b-nav-item disabled>You are :{{ numTotal+1 }} / {{ all }}</b-nav-item>
	</b-nav>
</template>

<script>
export default {
	props : [
		'numCorrect',
		'numTotal',
		'all'
	],
	mounted() {
		console.log(this.all)
	}
}
</script>