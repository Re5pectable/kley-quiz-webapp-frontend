<template>
    <WrapperComponent v-if="quiz">
        <HeaderComponent :nonSolid="true" :total="totalQuestions"/>
        <img :src="quiz?.logo_url" alt="Quiz Logo" />
        <div class="pad">
            <TagList :quiz="quiz"/>
            <h1>{{ quiz?.header.toUpperCase() }}</h1>
            <p v-html="enterToBR(quiz?.text)"></p>
            <button class="g" @click="startGame()">Начать</button>
        </div>
    </WrapperComponent>

</template>
<script>
import HeaderComponent from './_Header.vue';
import WrapperComponent from './_Wrapper.vue';
import { apiGetQuiz } from '@/api/quiz';
import { apiStartGame } from '@/api/game';
import TagList from './_TagList.vue';
import { enterToBR } from '@/utils';

export default {
    props: {
        quizId: {
            type: String,
            required: true,
        }
    },
    components: { HeaderComponent, TagList, WrapperComponent },
    data() {
        return {
            enterToBR,

            quiz: null,
            totalQuestions: null,
        }
    },
    methods: {
        async startGame() {
            const { game_id } = await apiStartGame(this.quizId)
            this.$router.push(`/game/${game_id}`)
        }
    },
    async mounted() {
        const data = await apiGetQuiz(this.quizId);
        this.quiz = data.quiz;
        this.totalQuestions = data.questions_amount
    },
}
</script>
<style scoped>
img {
    width: 100%;
    height: 500px;
    display: block;
    object-fit: cover;
    margin-bottom: 24px;
}

h1 {
    color: white;
    text-align: left;
    margin: 0px;
    margin-top: 12px;
    margin-bottom: 24px;
}

p {
    text-align: left;
    color: white;
    margin: 0;
    margin-bottom: 48px;
}
</style>