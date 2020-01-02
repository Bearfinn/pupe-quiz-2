<template>
  <v-card height="500px">
    <v-container fill-height class="d-flex align-content-center">
      <v-card-title>ยินดีต้อนรับสู่เกมตอบคำถาม Pupe BNK48</v-card-title>
      <v-card-text>
        มีคำถามทั้งหมด 10 คำถาม ตอนถูกเท่าไหร่ โดเนทเพิ่มให้เท่านั้น!
      </v-card-text>
      <v-card-actions>
        <v-btn class="primary" @click="$emit('startQuiz')">เริ่มเลย</v-btn>
      </v-card-actions>
    </v-container>
  </v-card>
</template>
<script>
export default {
  name: "QuizIntroductionCard"
};
</script>
