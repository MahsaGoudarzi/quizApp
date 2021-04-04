<template>
    <div>
        <b-jumbotron class="d-flex justify-content-center align-items-center text-center" v-if = "numTotal+1 === all" bg-variant="secondary" text-variant="light" border-variant="dark">
            <template class="text-center" #header>
                <h3>Result :</h3>
                <h5>Your correct answer is <b>{{ numCorrect }}</b> of <b>{{ all }}</b></h5>
                <b-button @click="reloadPage" variant="primary" class="mt-3"> Try Another Chanse</b-button>
            </template>
        </b-jumbotron>
        
        <b-jumbotron :class="numTotal+1 === all ? 'opacity-25' : '' " class="text-center">

            <template #lead>
            {{ currentQuestion.question }}
            </template>

            <hr class="my-4">
            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in shuffledAnswers" 
                    :key="index" 
                    @click="selectAnswer(index)"
                    :class = "answerClass(index)"

                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <div class="mt-3">
                <b-button 
                    class="mr-1" 
                    variant="primary" 
                    @click="submitAnswers()"
                    :disabled="selectedIndex === null || answered || numTotal+1 === all"
                >
                    Submit
                </b-button>
                <b-button @click="next" variant="success" :disabled="numTotal+1 === all || !answered"> Next</b-button>
            </div>
            <div>
            </div>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash';
export default {
    props: {
        numCorrect: Number,
        all : Number,
        numTotal : Number,
        currentQuestion: Object,
        next: Function,
        increment : Function,
    },
    data () {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            correctIndex: null,
            answered : false,
        }
    },
    computed: {
        answers() {
            let answers = [this.currentQuestion.incorrect_answers]
            answers[0].push(this.currentQuestion.correct_answer)
            console.log(answers[0]);
            return answers
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null,
                this.answered = false 
                this.shuffleAnswers()
            }
        }
    },
    methods :{
        selectAnswer(index){
            this.selectedIndex = index
        },
        shuffleAnswers() {
            let allAnswers = this.currentQuestion.incorrect_answers;
            allAnswers.push(this.currentQuestion.correct_answer)
            this.shuffledAnswers = _.shuffle(allAnswers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswers () {
            let isCorrect = false

            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
                console.log("doroste")
            }
            this.answered = true
            this.increment(isCorrect)
        },
        answerClass(index) {
            let answerClass = ''
            if(!this.answered && this.selectedIndex === index){
                answerClass = 'selected'
            }
            else if(this.answered && this.correctIndex === index) {
                answerClass = 'correct'
            }
            else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
                answerClass = 'incorrect'
            }
            return answerClass
        },
        reloadPage() {
            window.location.reload();
        }
    },
}
</script>

<style scoped>
    .list-group-item:hover{
        background-color: #eee;
        cursor: pointer;
    }
    .selected {
        background-color: lightblue;
    }
    .correct {
        background-color: lightgreen;
    }
    .incorrect {
        background-color: red;
    }
    .opacity-25{
        opacity: .25;
    }
</style>
