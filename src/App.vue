<template>
    <div id="app" class="app">
        <span>
            {{ text }}
        </span>
    </div>
</template>

<script>

export default {
    name: 'App',
    async mounted() {
        if (await this.getTime() >= Number(process.env.VUE_APP_TEXT_2_TIMESTAMP)) {
            this.text = process.env.VUE_APP_TEXT_2
        } else {
            this.text = process.env.VUE_APP_TEXT_1
            const interval = setInterval(async () => {
                if (await this.getTime() >= Number(process.env.VUE_APP_TEXT_2_TIMESTAMP)) {
                    this.text = process.env.VUE_APP_TEXT_2
                    clearInterval(interval)
                } else {
                    this.text = process.env.VUE_APP_TEXT_1
                }
            }, 1000)
        }
    },
    methods: {
        async getTime() {
            const response = await fetch('https://worldtimeapi.org/api/timezone/Etc/UTC')
            const result = await response.json()
            return result.unixtime
        }
    },
    data() {
        return {
            text: ''
        }
    }
}
</script>

<style>
.app {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
}

span {
    font-family: Arial;
    font-size: 20px;
}

body {
    padding: 0;
    margin: 0;
}

</style>
