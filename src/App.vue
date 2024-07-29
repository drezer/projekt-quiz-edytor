<template>
    <div id="app" class="pt-5 pb-5" style="height: 100%;">
        <b-container>
            <b-row v-if="questionNumber === 0">
                <b-col cols="12">
                    <b-card>
                        <b-card-body>
                            <b-card-text>
                                <b-button variant="success" class="d-block w-100 fw-bold fs-5" @click="start">Rozpocznij quiz</b-button>
                            </b-card-text>
                            <b-card-text v-if="questionsAnswered.length > 0">
                                <b-row>
                                    <b-col cols="12" v-for="(answered, key) in questionsAnswered" :key="key">
                                        <b-row v-if="answered !== undefined" class="mt-5">
                                            <b-col cols="12">
                                                Pytanie: <b>{{answered.question}}</b>
                                            </b-col>
                                            <b-col cols="4">Odpowiedzi: </b-col>
                                            <b-col cols="2" v-for="(char, key) in ['A', 'B', 'C', 'D']" :key="key">
                                                <div v-if="answered.selected === char" class="d-inline-block" :style="answered.answer === char ? 'border: 2px solid rgb(22, 119, 74);' : 'border: 2px solid rgb(200, 48, 63);'" style=" border-radius: 100%; padding: 0 7px;">{{char}}</div>
                                                <div v-if="answered.selected !== char" class="d-inline-block">{{char}}</div>
                                                : <span :class="answered.answer === char ? 'text-success fw-bold' : ''">{{answered[char]}}</span>
                                            </b-col>
                                        </b-row>
                                    </b-col>
                                </b-row>
                            </b-card-text>
                        </b-card-body>
                    </b-card>
                </b-col>
            </b-row>
            <b-row v-if="questionNumber > 0">
                <b-col cols="12" class="points-box mt-5">
                    <b-card class="p-4">
                        <b-card-body>
                            <b-card-text class="fw-bold fs-3 text-end">
                                <b-progress :value="progress" variant="success" striped></b-progress>
                            </b-card-text>
                            <b-card-text class="fw-bold fs-3 text-end">Pytanie: {{questionNumber}}/{{questionsAll}}</b-card-text>
                        </b-card-body>
                    </b-card>
                </b-col>
                <b-col cols="12" class="points-box mt-5">
                    <b-card class="p-4">
                        <b-card-title>{{questionsAnswered[questionNumber].question}}</b-card-title>
                        <b-card-body>
                            <b-card-text>
                                <b-form-group>
                                    <b-row>
                                        <b-col cols="6" v-for="(char, key) in ['A', 'B', 'C', 'D']" :key="key" class="mt-3">
                                            <b-form-radio v-model="questionsAnswered[questionNumber].selected" name="answers" :value="char" class="p-3 fs-4"><span class="ms-3">{{questionsAnswered[questionNumber][char]}}</span></b-form-radio>
                                        </b-col>
                                    </b-row>
                                </b-form-group>
                            </b-card-text>
                            <b-card-text>
                                <b-button variant="success" class="d-block w-25 fw-bold fs-5 float-end" @click="next" :disabled="!questionsAnswered[questionNumber].selected">
                                    <span v-if="questionNumber < questionsAll">Następne pytanie</span>
                                    <span v-if="questionNumber === questionsAll">Zakoncz quiz</span>
                                </b-button>
                            </b-card-text>
                        </b-card-body>
                    </b-card>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
import questionsData from '@/assets/data';

export default {
    name: 'App',
    data () {
        return {
            questionsAll: 3,
            questionNumber: 0,
            questionsAnswered: [],
            questionsTmp: questionsData,
            questions: [],
            progress: 0,
            previousNumbers: []
        }
    },
    methods: {
        start: function () {
            this.previousNumbers = []
            this.questionsAnswered = []
            this.random()
        },
        next: function () {
            if (this.questionNumber === this.questionsAll) {
                this.questionNumber = 0

                return true
            }

            this.random()
        },
        random: function () {
            if (this.questions.length === 0) {
                this.prepare()
            }

            let number = Math.round(Math.random() * (this.questions.length - 1) + 1)

            if (this.previousNumbers.includes(number)) {
                this.random()
            }

            this.questionNumber++
            this.previousNumbers.push(number)
            this.questionsAnswered[this.questionNumber] = this.questions[number]
            this.progress = (this.questionNumber / this.questionsAll) * 100
        },
        prepare: function () {
            let questions = this.questions

            this.questionsTmp.forEach(function(item) {
                questions.push(item)
            })

            this.questions = questions
        }
    }
}
</script>

<style>
#app {

}
</style>
