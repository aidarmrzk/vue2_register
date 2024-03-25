<template>
    <form class="form" @submit.prevent="onSubmit">
        <div class="form__title">
            <h2 class="form__title-text">Регистрация</h2>
        </div>
        <div class="form__data">
            <h3 class="form__data-title">Заполните Ваши данные</h3>
            <div class="form__data-grid">
                <div v-for="item in items" :key="item.name" class="form__data-item">
                    <form-input
                        v-if="item.type === 'text' || item.type === 'email'"
                        :type="item.type"
                        :placeholder="item.placeholder"
                        v-model="formData[item.name]"
                        @input="() => item.error.status = false"
                    />

                    <form-password
                        v-if="item.type === 'password'"
                        :show="item.show"
                        :placeholder="item.placeholder"
                        :name="item.name"
                        v-model="formData[item.name]"
                        @updatePassword="updatePassword"
                        @toggleShow="toggleShow(item.name)"
                    />

                    <form-select
                        v-if="item.type === 'select'"
                        :options="item.options"
                        :placeholder="item.placeholder"
                        v-model="formData[item.name]"
                        @input="() => item.error.status = false"
                    />
                    <span
                        v-if="item.error && item.error.status"
                        class="error"
                    >
                        {{ item.error.text }}
                    </span>
                </div>
            </div>
        </div>

        <div class="form__visibility">
            <form-toggle v-model="formData['public']"/>
            <div class="form__visibility-info">
                <h3 class="form__visibility-title">Хотите чтобы Ваш профиль видели другие участники платформы? </h3>
                <p class="form__visibility-description">Включает профиль для просмотра другими пользователями по ссылке</p>
            </div>
        </div>

        <div class="form__agreement">
            <div class="form__agreement-confirm">
                <form-checkbox v-model="confirm"/>
                <p class="form__agreement-text">Регистрируясь, Вы соглашаетесь  <a href="#" class="form__agreement-link">с политикой конфиденциальности</a> и обработкой <a href="#" class="form__agreement-link">персональных данных</a></p>
            </div>
            <button class="form__agreement-button" type="submit" :disabled="!confirm" :class="{disabled: !confirm}">Зарегистрироваться</button>
        </div>
    </form>
</template>

<script>
import axios from 'axios'
import MockAdapter from 'axios-mock-adapter';

const mock = new MockAdapter(axios);
mock.onPost('https://example.com/api/form').reply(() => {
    return [200, { message: 'Регистрация прошла успешна' }];
});

import FormInput from "@/components/FormInput.vue";
import FormPassword from "@/components/FormPassword.vue";
import FormSelect from "@/components/FormSelect.vue";
import FormToggle from "@/components/FormToggle.vue";
import FormCheckbox from "@/components/FormCheckbox.vue";

export default {
    name: 'FormComponent',
    components: {
        FormInput,
        FormPassword,
        FormSelect,
        FormToggle,
        FormCheckbox
    },
    data() {
        return {
            items: [
                {
                    name: "name",
                    type: "text",
                    placeholder: "Имя",
                    error: {
                        text:'Введите имя',
                        status: false,
                    }
                },
                {
                    name: "email",
                    type: "email",
                    placeholder: "Email",
                    error: {
                        text:'Введите почту',
                        status: false,
                    }
                },
                {},
                {
                    name: "position",
                    type: "select",
                    placeholder: "Должность",
                    options: [
                        {
                        value: 1,
                        text: "HR"
                        },
                        {
                        value: 2,
                        text: "Разработчик",
                        },
                        {
                        value: 3,
                        text: "Project Manager",
                        }
                    ],
                    error: {
                        text:'Выберите должность',
                        status: false,
                    }
                },
                {
                    name: "pass",
                    type: "password",
                    placeholder: "Пароль",
                    show: false,
                    error: {
                        text:'Введите пароль',
                        status: false,
                    }
                },
                {
                    name: "repeat-pass",
                    type: "password",
                    placeholder: "Повторите пароль",
                    show:  false,
                    error: {
                        text:'Пароли не совпадают',
                        status: false,
                    }
                }
            ],
            formData: {
                name: '',
                email: '',
                position: '',
                pass: '',
                'repeat-pass': '',
                public: true,
            },
            confirm: true,
            validationItems: {

            }
        };
    },
    mounted() {
        console.log(this.formData)
    },
    methods: {
        updatePassword({name, value}) {
            this.formData[name] = value;

            this.items.forEach(item => {
                if (item.name === name) {
                    item.error.status = false;
                }
            });
        },
        toggleShow(name) {
            const foundItem = this.items.find(item => item.name === name);
            if (foundItem) {
                foundItem.show = !foundItem.show;
            }
        },
        validation() {
            console.log(this.formData)
            let isValid = true;

            const changeErrorStatus = (fieldName) => {
                this.items.forEach(item => {
                    if (item.name === fieldName) {
                        item.error.status = true;
                    }
                });
                isValid = false;
            }

            // email validation
            const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

            if (!emailPattern.test(this.formData['email'])) {
                changeErrorStatus('email');
            }

            // name, position, pass validation
            if (!this.formData['name'].trim()) {
                changeErrorStatus('name');
            }
            // position validation
            if (this.formData['position'] === '') {
                changeErrorStatus('position');
            }
            // pass validation
            if (!this.formData['pass'].trim()) {
                changeErrorStatus('pass');
            }

            // repeat-pass validation
            if (this.formData['pass'].trim() !== this.formData['repeat-pass'].trim()) {
                changeErrorStatus('repeat-pass');
            }

            return isValid;
        },
        onSubmit() {   
            if (this.validation()) {
                if (this.formData['pass'] !== this.formData['repeat-pass']) {
                    alert("Пароли не совпадают");
                    return;
                }
    
                const data = {...this.formData};
                console.log(data)
    
                axios.post('https://example.com/api/form', data)
                    .then(response => {
                    console.log('Успешно отправлено:', response.data);
                    })
                    .catch(error => {
                    console.error('Ошибка при отправке:', error.response.data);
                    });
            }
             
        }
    },   
}
</script>

<style scoped lang="scss">
.form {
    margin: 50px auto;
    padding: 20px 0 65px;
    max-width: 960px;
    border-radius: 15px;
    box-shadow: 0 0 8px 0 rgba(0,0,0,.3);
    .form__title {
        border-bottom: 1px solid #D9D9D9;
        padding: 15px 0 15px 30px;
        .form__title-text {
            margin: 0;
            font-size: 19px;
            font-weight: 700;
            line-height: 27px;
        }
    }
    .form__data {
        padding: 15px 15px 30px 30px;
        border-bottom: 1px solid #D9D9D9;
        .form__data-title {
            margin: 0 0 35px;
            font-size: 16px;
            font-weight: 500;
            line-height: 19px;
        }
        .form__data-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            grid-column-gap: 14px;
            grid-row-gap: 30px;
            .form__data-item {
                position: relative;
                .form__input, .form__select {
                    padding: 10px;
                    width: 100%;
                    border-radius: 10px;
                    border: 1px solid #E6E6EB;
                    font-size: 14px;
                    line-height: 19px;
                    &::placeholder {
                        color: #9292A0;
                    }
                }
                .error {
                    position: absolute;
                    left: 10px;
                    bottom: calc(100% + 2px);
                    font-size: 12px;
                    color: red;
                    white-space: nowrap;
                }
            }
        }
    }
    .form__visibility {
        margin: 20px 15px 30px 30px;
        max-width: 630px;
        display: flex;
        align-items: flex-start;
        gap: 5px;
        .form__visibility-title {
            margin: 0 0 10px;
            font-size: 16px;
            font-weight: 500;
            line-height: 19px;
        }
        .form__visibility-description {
            margin: 0;
            font-size: 14px;
            line-height: 19px;
            color: #696977;
        }
    }
    .form__agreement {
        display: flex;
        justify-content: space-between;
        padding: 0 15px 0 30px;
        .form__agreement-confirm {
            display: flex;
            align-items: flex-start;
            gap: 15px;
            .form__agreement-checkbox {
                width: 19px;
                height: 17px;
                border-radius: 5px;
            }
            .form__agreement-text {
                margin: 0;
                max-width: 500px;
                font-size: 14px;
                line-height: 19px;
                .form__agreement-link {
                    color: #3586FF;
                    text-decoration: none;
                }
            }
        }
        .form__agreement-button {
            display: inline-block;
            padding: 0;
            width: 100%;
            max-width: 300px;
            height: 40px;
            border-radius: 8px;
            background-color: #497ADA33;
            font-size: 12px;
            line-height: 100%;
            color: #497ADA;
            letter-spacing: -0.0015em;
            border: none;
            cursor: pointer;
            &.disabled {
                opacity: 0.7;
            }
        }
    }
}



</style>
