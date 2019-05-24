<template>
    <div id="question-box-container">
        <b-jumbotron>
            <template slot="lead">
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item 
                    v-for="(answer, index) in shuffledAnswers" :key="index"
                    @click="selectAnswer(index)"
                    :class="answerClass(index)"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <hr class="my-4">

            <b-button 
                variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
                submit
            </b-button>
            <b-button variant="success" href="#" @click="next">next</b-button> 
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data(){
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            answered: false,
            correctAnswer:null
        }
    },
    watch:{
        currentQuestion:{
            immediate: true, //the watcher will run from the first time currentQuestion gets passed to the props
            handler() {
                this.selectedIndex = null
                this.shuffleAnswers()
                this.answered = false
            }
        }
    },
    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers) 
            this.correctAnswer = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            let isCorrect = false 
            if(this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.increment(isCorrect)
            this.answered = true
        },
        answerClass(index){
            let answerClass = ''
            if(!this.answered && this.selectedIndex === index) {
                answerClass = 'selected'
            } else if (this.answered && this.correctAnswer === index) {
                answerClass = 'correct'
            } else if (this.answered && this.selectedIndex === index && this.correctAnswer !== index){
                answerClass = 'incorrect'
            }
            return answerClass
        }
    }
}
</script>

<style scoped>

#question-box-container {
    margin: 10px 0px;
}

.list-group-item{
    cursor: pointer
}

.list-group-item:hover {
    background: #eee
}

.btn {
    margin: 0 5px;
}

.selected {
    background: rgb(138, 139, 221)
}

.correct {
    background: rgb(86, 231, 122)
}

.incorrect {
    background: rgb(236, 60, 74)
}
</style>
