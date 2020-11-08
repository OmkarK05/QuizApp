<template>
<div>
    <Header :queNumber="questionNumber"></Header>
    <QuestionBox v-on:handleQuestionChange="questionChange" :data="data"></QuestionBox>
</div>
</template>

<script>
import QuestionBox from './components/QuestionBox.vue';
import Header from './components/Header';
import questions from "./Questions.json"

export default {
    name: 'App',
    data() {
        return {
            data: null,
            questionNumber : 1,
        };
    },
    components: {
        Header,
        QuestionBox
    },
    methods: {
        questionChange : function(num){
           this.questionNumber = parseInt(num + 1);
        }
    },
    mounted: function () {
        localStorage.clear();
        localStorage.setItem('questions_data', JSON.stringify(questions) )
        this.data = JSON.parse(localStorage.getItem('questions_data'));
    },

};
</script>

<style>
#app {
    width: 100%;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
