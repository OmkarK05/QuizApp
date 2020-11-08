<template>
<vs-row class="container" w="12" justify="center">
    <vs-col lg="6" xs="12" class="question-container">
        <vs-row v-if="currentQuestion" class="question" justify="flex-start">{{currentQuestion.question}}</vs-row>
        <vs-row v-if="answers" class="answer-container" justify="flex-start">
            <vs-col type="flex" justify="flex-start" v-for="(answer, i) of rotateAnswers" :key="i">
                <vs-radio v-model="selectedAnswer" :val="answer">{{answer}}</vs-radio>
            </vs-col>
        </vs-row>
    </vs-col>
    <vs-col>
        <vs-row justify="center">
            <vs-col lg="2" sm="3">
                <vs-button  :disabled="questionNumber > 0 ? false : true" @click="handlePrevious">Prev</vs-button>
            </vs-col>
            <vs-col lg="2" sm="3">
                <vs-button :disabled="questionNumber === 9" @click="handleNext">Next</vs-button>
            </vs-col>
            <vs-col lg="2" sm="3">
                <vs-button :disabled="questionNumber < 9" @click="handleSubmit" success>Submit</vs-button>
            </vs-col>
        </vs-row>
    </vs-col>
</vs-row>
</template>

<script>
import Container from "./Container";
export default {
    name: "QuestionBox",
    props: {
        data: Object,
    },
    data() {
        return {
            questionsData: null,
            currentQuestion: null,
            questionNumber: 0,
            selectedAnswer: '',
            correctAnswer: '',
            answers: null,

        }
    },
    components: [Container],
    computed: {
        rotateAnswers: function () {
            let newAnswers = [...this.answers];
            console.log(newAnswers);

            for (let i = 0; i < Math.floor(Math.random() * 5); i++) {
                let temp = newAnswers.pop();
                newAnswers.unshift(temp);
            }

            return newAnswers;
        }
    },
    methods: {
        getQuestionData: function () {
            return JSON.stringify(localStorage.getItem('question_data'))
        },
        saveAnswer: function (id) {
            console.log('Save Answer');
            this.questionsData.forEach((el) => {
                if (el.id === id) {
                    el.selected_answer = this.selectedAnswer;
                }
            })

            console.log(this.questionsData);
            localStorage.setItem('question_data', JSON.stringify(this.questionsData));

        },
        handlePrevious: function () {
            if (this.questionNumber === 0) {
                return
            } else {
                this.saveAnswer(this.currentQuestion.id);
                this.questionNumber--;
            }
        },
        handleNext: function () {
            console.log();
            if (this.questionNumber === this.questionsData.length - 1) {
                return
            } else {
                console.log(this.currentQuestion.id)
                this.saveAnswer(this.currentQuestion.id);
                this.questionNumber++;
            }
        },
        handleSubmit: function () {
            //   if(!localStorage.getItem("answered_questions")){

            //   }
        }
    },
    watch: {
        data: function (val) {
            this.questionsData = [...val];
            this.answers = [...val[this.questionNumber].incorrect_answers, val[this.questionNumber].correct_answer];
            this.currentQuestion = val[this.questionNumber];
            this.correctAnswer = val[this.questionNumber].correct_answer;
        },
        questionNumber: function (val) {
            this.$emit('handleQuestionChange', this.questionNumber);
            console.log(typeof val, val)
            let newQuestionData = JSON.parse(localStorage.getItem('question_data'));
            console.log(newQuestionData);
            this.questionsData = [...newQuestionData];
            this.answers = [...newQuestionData[val].incorrect_answers, newQuestionData[val].correct_answer];
            this.currentQuestion = newQuestionData[val];
            this.correctAnswer = newQuestionData[val].correct_answer;
            this.selectedAnswer = newQuestionData[val].selected_answer || '';
        },

    },

    beforeUpdate: function () {

    }

}
</script>

<style lang="scss" scoped>
.question-container {
    margin: 1rem 0.5rem;
    height: 370px;
}

.question {
    padding: 15px;
}

.answer-container div {
    padding: 8px;
}
</style>
