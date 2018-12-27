<template>
    <div class="wrapper">
        <div class="sample">
            <form @submit.prevent="formSubmited = true"
                  v-if="!formSubmited">
                <div class="progress">
                    <div class="progress-bar" :style="progressWidth"></div>
                </div>
                <div>
                    <app-input v-for="(item, index) in info"
                               :name="item.name"
                               :value="item.value"
                               :pattern="item.pattern"
                               :key="index"
                               @changedata="onChangeData(index, $event)"
                    >

                    </app-input>
                </div>
                <button class="btn btn-primary" :disabled="done < info.length">
                    Send Data
                </button>
            </form>
            <div v-else="">
                <table class="table table-bordered">
                    <tr v-for="(item, index) in info">
                        <td>{{ item.name }}</td>
                        <td>{{ item.value }}</td>
                    </tr>

                </table>

            </div>
        </div>
        <hr>
        <app-test @update="onStep1Update"
                  :quaere="questions[page_now].quaere"
                  :versions="questions[page_now].versions"
                  :type_test="questions[page_now].type_test"
                  :endtest="test_done"
        >

        </app-test>
        <hr>
    </div>

</template>

<script>
    import AppInput from './components/Input';
    import AppTest from './components/TestHome';

    export default {
        data() {
            return {

                info: [
                    {
                        name: 'Name',
                        value: '',
                        pattern: /^[a-zA-Z]{2,30}$/,
                    },
                    {
                        name: 'Phone',
                        value: '',
                        pattern: /^[0-9]{7,14}$/,
                    },
                    {
                        name: 'Email',
                        value: '',
                        pattern: /.+/,
                    },
                    {
                        name: 'Some Field 1',
                        value: '',
                        pattern: /.+/,
                    },
                    {
                        name: 'Some Field 2',
                        value: '',
                        pattern: /.+/,
                    },
                ],
                controls: [],
                done: 0,
                formSubmited: false,

                questions: [
                    {
                        quaere: 'Странный вопрос №1',
                        versions: ['№ 1', '№ 2', '№ 3', '№ 4'],
                        type_test: 'radio',
                        page: 0
                    },
                    {
                        quaere: 'Странный вопрос №2',
                        versions: ['№ 1', '№ 2', '№ 3', '№ 4', '№ 5', '№ 6'],
                        type_test: 'checkbox',
                        page: 1
                    },
                    {
                        quaere: 'Странный вопрос №3',
                        versions: ['№ 1', '№ 2', '№ 3', '№ 4', '№ 5', '№ 6'],
                        type_test: 'radio',
                        page: 2
                    },
                    {
                        quaere: 'Странный вопрос №4',
                        versions: ['№ 1', '№ 2'],
                        type_test: 'checkbox',
                        page: 3
                    }
                ],
                page_now: 0,
                test_done: false

            }
        },
        created() {
            for (let i = 0; i < this.info.length; i++) {
                this.controls.push(false);
            }
        },
        methods: {
            onChangeData(index, data) {
                this.info[index].value = data.value;
                this.controls[index] = data.valid;
                let done = 0;
                for (let i = 0; i < this.controls.length; i++) {
                    if (this.controls[i]) {
                        done++;
                    }
                }
                this.done = done;
            },

            onStep1Update(item) {
                if (this.page_now < this.questions.length-1) {
                    this.page_now += item;
                } else {
                    this.test_done = true;
                }
            },

        },
        computed: {
            progressWidth() {
                return {
                    width: `${(this.done / this.info.length * 100)}%`
                }
            }
        },
        watch: {
            page_now: function () {
                console.log('this.test_done => ' + this.test_done);

            }
        },
        components: {
            AppInput,
            AppTest,
        }
    }

</script>

<style scoped>
    .wrapper {
        max-width: 600px;
        margin: 0 auto;
    }
</style>