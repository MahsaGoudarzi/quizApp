<template>
	<div>
		<Header 
			:numCorrect = "numCorrect"
			:numTotal = "numTotal"
			:all = 'questions.length'
		/>
		<b-container class="bv-example-row">
			<b-row>
				<b-col sm='6' offset = '3'>
					<QuestionBox 
						:numCorrect = "numCorrect"
						v-if = "questions.length"
						:currentQuestion="questions[index]"
						:next="next"
						:increment="increment"
						:all = 'questions.length'
						:numTotal = 'numTotal'
					/>
				</b-col>
			</b-row>
		</b-container>
	</div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
export default {
	name: "App",
	components: {
		Header,
		QuestionBox
	},
	data(){
		return {
			questions:[],
			index :0,
			numCorrect : 0,
			numTotal: 0,
		}
	},
	methods:{
		next() {
			this.index++,
			this.numTotal++
		},
		increment(isCorrect) {
			if (isCorrect) {
				this.numCorrect++
			}
		}
	},
	mounted : function(){
		fetch('https://opentdb.com/api.php?amount=11&type=multiple',{
			method: 'get'
		})
		.then((response)=> {
			return response.json()
		})
		.then((jsonData) =>{
			this.questions = jsonData.results
		})
	}
};
</script>
