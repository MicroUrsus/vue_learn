<template>
    <div class="test">
        <form
                @submit.prevent="onNextQuestion($event)"
                v-if="!endtest"
        >
            <label class="quaere">{{ quaere }}</label>
            <div class="versions-wr">
                <p v-for="(version, index) in versions">

                    <input
                            :versions="version"
                            name="versions"
                            :value="version"
                            class="versions"
                            :key="index"
                            :type="type_test"
                            @input="testInput"
                    >
                    <span></span>
                    {{ version }}
                </p>
            </div>
            <button class="btn btn-primary"
                    :disabled="!button_ready"
            >
                Send Data Test
            </button>
        </form>
        <div v-else="">
            <table class="table table-bordered">
                <tr v-for="(item, index) in result">
                    <td>{{ item }}</td>
                </tr>

            </table>
        </div>

    </div>
</template>

<script>
    export default {
        props: {
            quaere: String,
            versions: Array,
            type_test: String,
            endtest: Boolean,
            page: Number
        },

        data() {
            return {
                button_ready: false,
                page_step: 1,
                result: []
            }
        },
        methods: {
            testInput() {
                let all_input = document.querySelectorAll(".versions");
                let flag_checked = false;
                all_input.forEach((elem)=>{
                    if (elem.checked) {
                        flag_checked = true;
                    }
                });
                if (flag_checked) {
                    this.button_ready = true;
                } else {
                    this.button_ready = false;
                }
            },
            onNextQuestion(event) {
                this.result.push({
                    'Вопрос': this.quaere,
                    'Ответ/ы': this.versions,
                });
                console.log(`this.quaere => ` + this.quaere);
                this.button_ready = false;
                this.$emit('update', this.page_step);
                console.log(this.result);
            }
        },
        computed: {

        }

    }
</script>

<style scoped>
    .versions-wr {
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-content: start;
        align-items: flex-start;
    }

    .versions {
        margin: 5px 0;

    }

    span {
        margin: 0 10px;
    }


</style>