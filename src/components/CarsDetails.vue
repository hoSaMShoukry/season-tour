<template>
    <div class="cars-details position-relative">
        <span
            class="p-2 d-block bg-secondary mb-4 bg-opacity-25 rounded-circle d-flex align-items-center justify-content-center"
            style="width: 40px; height: 40px; cursor: pointer;" @click="$emit('back-ward')" title="Back">
            <i :class="`fa-solid fa-chevron-${$i18n.locale === 'en' ? 'left' : 'right'}`"></i>
        </span>
        <div class="container py-5">
            <h1 class="heading mb-5 position-relative">
                {{ $t('carBook.heading') }}
            </h1>
            <div class="inner d-flex py-5 gap-4">
                <div class="img-cont w-50">
                    <img class="img-fluid" :src="`https://api.seasonsge.com/images/Agents/${carInfo.car_image}`" alt="" />
                </div>
                <div class="car-info flex-fill">
                    <div class="d-flex gap-3">
                        <div class="w-100">
                            <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.type') }}: </span>
                            <span class="text-capitalize p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                                {{ $route.params.lang === 'en' ? carInfo.car_name_en || 'Unknown' : carInfo.car_name
                                    || 'غير معروف' }}
                            </span>
                        </div>
                        <div class="w-100">
                            <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.price') }}: </span>
                            <span class="p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                                {{
                                    userInfo.with_driver == "1"
                                    ? USDollar.format(carInfo.price_with_driver)
                                    : USDollar.format(carInfo.price_per_day)
                                }}
                            </span>
                        </div>
                    </div>
                    <div class="w-100 my-3">
                        <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.driver.head') }}: </span>
                        <select class="p-2 px-3 w-100 rounded-1" name="driver" v-model="userInfo.with_driver">
                            <option value="1" v-if="carInfo.price_with_driver = 1">{{ $t('carBook.details.driver.with') }}
                            </option>
                            <option value="0" v-if="carInfo.price_per_day = 0">{{ $t('carBook.details.driver.without') }}
                            </option>
                        </select>
                    </div>
                    <form>
                        <div class="first d-flex flex-column flex-md-row gap-3 mt-3">
                            <div class="w-100 position-relative">
                                <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.fName') }}: </span>
                                <input :class="`p-2 px-3 w-100 rounded-1 ${validation.first_name.$error ? 'error' : ''}`"
                                    type="text" v-model="userInfo.first_name" placeholder="First Name" />
                                <span v-if="validation.first_name.$error" class="error d-block text-danger fst-italic mx-2">
                                    {{
                                        validation.first_name.$errors[0].$validator === "required"
                                        ? "Field Reuqired"
                                        : ""
                                    }}
                                </span>
                            </div>
                            <div class="w-100 position-relative">
                                <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.lName') }}: </span>
                                <input :class="`p-2 px-3 w-100 rounded-1 ${validation.last_name.$error ? 'error' : ''}`"
                                    type="text" v-model="userInfo.last_name" placeholder="Last Name" />
                                <span v-if="validation.last_name.$error" class="error d-block text-danger fst-italic mx-2">
                                    {{
                                        validation.last_name.$errors[0].$validator === "required"
                                        ? "Field Reuqired"
                                        : ""
                                    }}
                                </span>
                            </div>
                        </div>
                        <div class="second d-flex flex-column gap-3 mt-3">
                            <div class="w-100 position-relative">
                                <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.phone') }}: </span>
                                <input :class="`p-2 px-3 w-100 rounded-1 ${validation.phone_number.$error ? 'error' : ''
                                    }`" type="phone" v-model="userInfo.phone_number" placeholder="Phone Number" />
                                <span v-if="validation.phone_number.$error"
                                    class="error d-block text-danger fst-italic mx-2">
                                    {{
                                        validation.phone_number.$errors[0].$validator === "required"
                                        ? "Field Reuqired"
                                        : "Invalid Phone Number"
                                    }}
                                </span>
                            </div>
                        </div>
                        <div class="third mt-3 d-flex gap-3">
                            <div class="w-100">
                                <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.fDate') }}: </span>
                                <span class="p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                                    {{ userInfo.start_date }}
                                </span>
                            </div>
                            <div class="w-100">
                                <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.tDate') }}: </span>
                                <span class="p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                                    {{ userInfo.end_date }}
                                </span>
                            </div>
                        </div>
                    </form>
                    <div class="fourth mt-3">
                        <div class="w-100 mt-2">
                            <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.days') }}: </span>
                            <span class="p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                                {{ userInfo.total_days }}
                            </span>
                        </div>
                        <div class="w-100 mt-2">
                            <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.total') }}: </span>
                            <span class="p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                                {{ USDollar.format(userInfo.total_amount || 0) }}
                            </span>
                        </div>
                        <div class="w-100 mt-2">
                            <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.tax') }}: </span>
                            <span class="p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                                {{ +(userInfo.tax || 0) }}%
                            </span>
                        </div>
                        <div class="w-100 mt-2">
                            <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.net') }}: </span>
                            <span class="p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                                {{ USDollar.format(userInfo.net_amount || 0) }}
                            </span>
                        </div>
                        <div class="w-100 mt-2 position-relative">
                            <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.notes') }}: </span>
                            <textarea name="notes"
                                :class="`w-100 rounded-1 p-2 px-3 ${validation.notes.$error ? 'error' : ''}`" :placeholder="$i18n.locale === 'en' ?
                                    'Please write down the place the car will wait for you' :
                                    'برجاء تحديد المكان الذي يتم استلام السياره منه'"
                                v-model="userInfo.notes"></textarea>
                            <span v-if="validation.notes.$error" class="text-danger mt-1 d-block fst-italic">
                                {{ validation.notes.required.$message }}
                            </span>
                        </div>
                        <button class="butn rounded-1 p-2 px-5 w-100 mt-3 text-uppercase" @click.prevent="submission">
                            {{ $t('buttons.confirm') }}
                        </button>
                    </div>
                </div>
                <div class="alert alert-success alert-dismissible text-center position-fixed" role="alert">
                    <div class="d-flex align-items-center gap-2">
                        <i class="fa-solid fa-circle-check fs-5"></i>
                        {{ $t('carBook.bookMessage') }}
                    </div>
                    <span class="d-block mt-1">{{ `${$t('carBook.bookingCode')}: ${randomCode}` }}</span>
                    <button @click="removeAlert" type="button" class="btn-close"></button>
                </div>
            </div>
        </div>
        <Loader v-if="loading"></Loader>
    </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted, computed, watch } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { required, email, numeric, helpers } from "@vuelidate/validators";
import Loader from "./Loader.vue";
import router from "../router";
import i18n from "../i18n";

const loading = ref(false)
const carInfo = ref([]);
const randomCode = ref('')
const social = ref({})
const props = defineProps(['searchInfo', 'carId'])


// Formating Balanc 
const USDollar = Intl.NumberFormat("en-US", {
    currency: "USD",
    style: "currency"
})


const userInfo = ref({
    type_id: props.carId,
    // with_driver: computed(() => parseInt(carInfo.value.price_with_driver) > 0 ? carInfo.value.price_with_driver : 0),

    // with_driver: computed(() => {
    //     console.log('carInfo.value.price_with_driver:', carInfo.value.price_with_driver)
    //     if (carInfo.value.price_with_driver !== undefined) {
    //         console.log('carInfo.value.price_with_driver:', carInfo.value.price_with_driver);
    //         return parseInt(carInfo.value.price_with_driver) > 0 ? carInfo.value.price_with_driver : 0;
    //     } else {
    //         return 0;
    //     }
    // }),

    // with_driver: computed(() => {
    //     console.log('carInfo.value.price_with_driver:', carInfo.value.price_with_driver);
    //     return parseInt(carInfo.value.price_with_driver) > 0 ? carInfo.value.price_per_day : 0;
    // }),

    // with_driver: computed(() => carInfo.value.price_with_driver > 0 ? carInfo.value.price_with_driver : 0),
    with_driver: '',
    first_name: '',
    last_name: '',
    email: props.searchInfo.country,
    phone_number: "",
    start_date: props.searchInfo.fromDate.toLocaleDateString('en-CA'),
    end_date: props.searchInfo.toDate.toLocaleDateString('en-CA'),
    total_days: computed(() => {
        if (userInfo.value.start_date === userInfo.value.end_date) {
            return "1"
        } else return ((new Date(userInfo.value.end_date).getTime() - new Date(userInfo.value.start_date).getTime()) / (1000 * 60 * 60 * 24)).toString()
    }),
    total_amount: computed(() => {
        if (userInfo.value.with_driver === '0') {
            return parseFloat(userInfo.value.total_days * carInfo.value.price_per_day).toFixed(2)
        } else {
            return parseFloat(userInfo.value.total_days * carInfo.value.price_with_driver).toFixed(2)
        }
    }),
    tax: computed(() => parseFloat(carInfo.value.tax)),
    net_amount: computed(() => (parseFloat(userInfo.value.total_amount) + ((parseFloat(userInfo.value.tax)).toFixed(2) / 100) * parseFloat(userInfo.value.total_amount))),
    notes: "",
    account_owner: ""
});

watch(userInfo.value, (newVal) => {
    if (newVal.start_date < new Date().toLocaleDateString("en-CA")) {
        userInfo.value.start_date = new Date().toLocaleDateString("en-CA")
    }
    if (newVal.end_date < new Date().toLocaleDateString("en-CA")) {
        userInfo.value.end_date = new Date().toLocaleDateString("en-CA")
    }
})


const rules = {
    first_name: { required },
    last_name: { required },
    // email: { required, email },
    phone_number: { required, numeric },
    notes: { required: helpers.withMessage('Field Required', required) },
};

const validation = useVuelidate(rules, userInfo);

const submission = async () => {
    validation.value.$validate();
    if (!validation.value.$error) {
        if (localStorage.getItem("clientLogin")) {
            const userId = JSON.parse(localStorage.getItem("clientLogin"))
            userInfo.value.account_owner = userId.id
            loading.value = true
            await axios.post('https://api.seasonsge.com/car-order', userInfo.value)
                .then(response => {
                    if (response.data.status === 'success') {
                        document.querySelector('.alert').classList.add("active")
                        randomCode.value = response.data.random_code
                        loading.value = false
                        axios.get(`https://api.seasonsge.com/car-rr?id=${userId.id}`)
                            .then(data => {
                                const bookId = data.data.pop()
                                setTimeout(() => {
                                    router.push({
                                        name: "Cars Checkout",
                                        params: { lang: i18n.global.locale.value, id: bookId.id }
                                    })
                                }, 1500)
                            })
                    }
                }).catch(error => {
                    console.log(error);
                })
        } else {
            router.push({
                name: "Clients Login",
                params: { lang: i18n.global.locale.value }
            })
        }

    }
};


const removeAlert = () => {
    document.querySelector(".alert").classList.remove("active")
}


onMounted(async () => {
    loading.value = true
    await axios.get("https://api.seasonsge.com/cars-view").then((data) => {
        // debugger
        // carInfo.value =data.data.find((el)=>+el.type_id === props.carId)
        data.data.filter(el => {
            if (+el.type_id === props.carId) {
                carInfo.value = el
                userInfo.value.with_driver = el.price_with_driver;
            }
            console.log(carInfo.value, 'carInfo')
            loading.value = false
        })
        // console.log(data.data)
    });
    await axios.get("https://api.seasonsge.com/cars-type-view").then((data) => {
        data.data.filter(el => {
            if (el.id == props.carId) {
                carInfo.value['car_name_en'] = el.name_en
                carInfo.value['car_name'] = el.name
                carInfo.value['car_image'] = el.img
            }
            // console.log(carInfo.value)
        })
    });
    await axios.get("https://api.seasonsge.com/info")
        .then(data => {
            social.value = data.data[0]
            // console.log(data)
        })
});
</script>

<style lang="scss" scoped>
.cars-details {
    .heading {
        color: var(--blue-color);

        &::before {
            content: "";
            position: absolute;
            height: 3px;
            width: 10%;
            background-color: var(--blue-color);
            bottom: -10px;
            border-radius: 50px;
        }
    }

    .car-info {
        input {
            border: 1px solid darkgray;
            outline: none;

            &.error {
                border: 1px solid #dc3545;
            }
        }

        select {
            border: 1px solid darkgray;
            outline: none;
        }

        textarea {
            outline: none;
            border: 1px solid darkgray;
            resize: none;
            height: 125px;

            &.error {
                border: 1px solid #dc3545;
            }
        }
    }

    .alert {
        top: -15%;
        left: 50%;
        transform: translateX(-50%);
        transition: 0.3s;
        width: fit-content;
        z-index: 555555555;

        &.active {
            top: 5%;
        }

        button {
            box-shadow: none;
            outline: none;
        }
    }

    @media (max-width: 767px) {
        .car-info {
            width: 100%;
        }

        .inner {
            flex-direction: column;
            align-items: center;

            .img-cont {
                width: 100% !important;
            }

            .third {
                flex-direction: column;
            }
        }

        .alert {
            width: 90%;
            font-size: 12px;
        }
    }
}
</style>
