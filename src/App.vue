<template>
    <div id="app" class="pt-5 pb-5" style="height: 100%;">
        <b-container>
            <b-row>
                <b-col cols="12">
                    <b-table-simple hover caption-top responsive>
                        <caption>
                            Lista pytań:
                            <a @click="addQuestion" class="float-end text-success" title="Dodaj nowe pytanie"><b-icon icon="plus-square"></b-icon></a>
                        </caption>
                        <b-thead head-variant="dark">
                            <b-tr>
                                <b-th>Lp.</b-th>
                                <b-th>Pytanie</b-th>
                                <b-th>Odpowiedzi <small class="text-success fw-bold">(+ zaznaczona prawidłowa)</small></b-th>
                                <b-th>Edycja</b-th>
                            </b-tr>
                        </b-thead>
                        <b-tbody>
                            <b-tr v-for="(question, key) in questions" :key="key">
                                <b-td>{{key + 1}}</b-td>
                                <b-td>{{question.question}}</b-td>
                                <b-td class="text-right">
                                    <b-row>
                                        <b-col cols="6" :class="question.answer === 'A' ? 'text-success fw-bold' : ''">A: {{question.A}}</b-col>
                                        <b-col cols="6" :class="question.answer === 'B' ? 'text-success fw-bold' : ''">B: {{question.B}}</b-col>
                                        <b-col cols="6" :class="question.answer === 'C' ? 'text-success fw-bold' : ''">C: {{question.C}}</b-col>
                                        <b-col cols="6" :class="question.answer === 'D' ? 'text-success fw-bold' : ''">D: {{question.D}}</b-col>
                                    </b-row>
                                </b-td>
                                <b-td>
                                    <a @click="editQuestion(key)" ref="btnShow" class="text-success" title="Edytuj pytanie"><b-icon icon="pencil-square"></b-icon></a>
                                </b-td>
                            </b-tr>
                        </b-tbody>
                        <b-tfoot>
                            <b-tr>
                                <b-td colspan="4" variant="secondary" class="text-right">
                                    Ilość pytań: <b>{{questions.length}}</b>
                                </b-td>
                            </b-tr>
                        </b-tfoot>
                    </b-table-simple>
                </b-col>
            </b-row>

            <b-modal id="modal-1" title="Dodaj / edytuj pytanie" size="lg" @ok="saveQuestion">
                <b-row>
                    <b-col cols="12">
                        <label for="field-question">Small:</label>
                        <b-form-textarea class="mb-3" id="field-question" placeholder="Small textarea" v-model="newEditQuestion.question"></b-form-textarea>
                    </b-col>
                    <b-col cols="12">
                        <label for="field-A">Odpowiedź A:</label>
                        <b-form-input id="field-A" v-model="newEditQuestion.A" type="text" class="mb-3" required></b-form-input>
                        <label for="field-B">Odpowiedź B:</label>
                        <b-form-input id="field-B" v-model="newEditQuestion.B" type="text" class="mb-3" required></b-form-input>
                        <label for="field-C">Odpowiedź C:</label>
                        <b-form-input id="field-C" v-model="newEditQuestion.C" type="text" class="mb-3" required></b-form-input>
                        <label for="field-D">Odpowiedź D:</label>
                        <b-form-input id="field-D" v-model="newEditQuestion.D" type="text" class="mb-3" required></b-form-input>
                    </b-col>
                    <b-col cols="12">
                        <label for="field-answer">Prawidłowa odpowiedź:</label>
                        <b-form-select v-model="newEditQuestion.answer" :options="[{value: 'A', text: 'A'}, {value: 'B', text: 'B'}, {value: 'C', text: 'C'}, {value: 'D', text: 'D'}]" class="ms-3 mb-3 w-25" id="field-answer"></b-form-select>
                    </b-col>
                </b-row>
            </b-modal>
        </b-container>
    </div>
</template>

<script>
import questionsData from '@/assets/data';

export default {
    name: 'App',
    data () {
        return {
            questions: questionsData,
            newEditQuestion: {},
            newEditQuestionId: 0
        }
    },
    methods: {
        addQuestion: function () {
            this.newEditQuestionId = 0
            this.newEditQuestion = {}
            this.$root.$emit('bv::show::modal', 'modal-1', '#btnShow')
        },
        editQuestion: function (id) {
            this.newEditQuestionId = id
            this.newEditQuestion = this.questions[this.newEditQuestionId]
            this.$root.$emit('bv::show::modal', 'modal-1', '#btnShow')
        },
        saveQuestion: function () {
            if (this.newEditQuestionId > 0) {
                this.questions[this.newEditQuestionId] = this.newEditQuestion
            } else {
                this.questions.push(this.newEditQuestion)
            }

            localStorage.setItem('questions', JSON.stringify(Object.assign({}, this.questions)))
        }
    },
    mounted: function () {
        let questions = localStorage.getItem('questions')

        if (questions === null) {
            localStorage.setItem('questions', JSON.stringify(Object.assign({}, questionsData)))
        } else {
            // this.questions = questions
        }
    }
}
</script>

<style>
#app {

}
</style>
