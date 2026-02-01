<template>
    <router-link
        :to="{name: 'Home'}"
    >На главную</router-link>
    <h1>Emodji</h1>
    <input
        type="text"
        v-model="film"
        placeholder="Введите название фильма">
    <button
        @click="sendFilm"
        :disabled="isLoading"
    >
        Отправить
    </button>
    <h3>{{ result }}</h3>
</template>

<script>
    import axios from 'axios'

    export default {
        data() {
            return {
                film: '',
                result: '',
                isLoading: false
            }
        },
        computed: {
            prompt() {
                return `Напиши 3 emodji, которые ассоциируются с фильмом "${this.film}". Напиши только emodji, без объяснений и всего лишнего.`
            }
        },
        methods: {
            async sendFilm() {
                this.isLoading = true;
                try {
                    const response = await axios.post(
                        'https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=AIzaSyDKisokgDj4HheDtGjAdoEoZIucnaqJUkE',
                        {
                            "contents": [
                                {
                                    "parts": [
                                        {
                                            "text": this.prompt
                                        }
                                    ]
                                }
                            ]
                        }
                    )
                    this.result = response.data.candidates[0].content.parts[0].text
                } catch(error) {
                    alert(error)
                }
                this.isLoading = false;
            }
        }
    }
</script>