<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-toolbar-title>Pupe BNK48 Quiz</v-toolbar-title>
    </v-app-bar>
    <v-content>
      <v-container fill-height fluid>
        <v-row align="center" justify="center">
          <v-col md="8">
            <template v-if="isQuizStarted">
              <v-card>
                <v-card height="500px">
                  <v-progress-linear
                    :value="correctQuestionsPercentage * 100"
                    :bufferValue="answeredQuestionsPercentage * 100"
                  />

                  <v-container
                    fill-height
                    loading
                    class="d-flex align-content-center"
                  >
                    <v-card-title
                      >{{ currentQuestion.no }}.
                      {{ currentQuestion.question }}</v-card-title
                    >
                    <v-card-text>
                      <v-radio-group v-model="selectedChoice">
                        <v-radio
                          v-for="(choice, index) in currentQuestion.choices"
                          :value="choice"
                          :label="choice"
                          :key="index"
                          color="primary"
                        />
                      </v-radio-group>
                    </v-card-text>
                    <v-card-actions>
                      <v-btn class="primary" @click="checkAnswer"
                        >ยืนยันคำตอบ
                      </v-btn>
                    </v-card-actions>
                  </v-container>
                  <v-snackbar v-model="correctAnswerSnackbar" color="success"
                    >ถูกต้อง!
                  </v-snackbar>
                  <v-snackbar v-model="wrongAnswerSnackbar" color="error"
                    >ผิด!
                  </v-snackbar>
                </v-card>
              </v-card>
            </template>
            <template v-else
              ><QuizIntroductionCard v-on:startQuiz="startQuiz"
            /></template>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import QuizIntroductionCard from "./components/QuizIntroductionCard";
import quiz from "./quiz";

export default {
  name: "App",

  components: { QuizIntroductionCard },

  data: () => ({
    isQuizStarted: false,
    questionNumber: 0,
    currentQuestion: {},
    selectedChoice: "",
    correctAnswerSnackbar: false,
    wrongAnswerSnackbar: false,
    correctQuestionsCount: 0
  }),

  computed: {
    questions() {
      return quiz;
    },
    answeredQuestionsPercentage() {
      return this.questionNumber / this.questions.length;
    },
    correctQuestionsPercentage() {
      return this.correctQuestionsCount / this.questions.length;
    }
  },

  methods: {
    startQuiz() {
      this.isQuizStarted = true;
      this.goToNextQuestion();
    },

    checkAnswer() {
      if (this.selectedChoice !== "") {
        if (
          this.selectedChoice === this.questions[this.questionNumber - 1].answer
        ) {
          this.correctQuestionsCount += 1;
          this.correctAnswerSnackbar = true;
        } else {
          this.wrongAnswerSnackbar = true;
        }
        this.goToNextQuestion();
      }
    },
    goToNextQuestion() {
      this.selectedChoice = "";
      this.questionNumber += 1;
      this.setQuestion();
    },
    setQuestion() {
      this.currentQuestion = this.questions[this.questionNumber - 1];
    }
  }
};
</script>
