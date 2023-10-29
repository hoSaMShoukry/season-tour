<template>
    <div class="car-reservation">
        <div class="content rounded-1 bg-white p-4">
            <span
                class="p-2 d-block bg-secondary mb-4 bg-opacity-25 rounded-circle d-flex align-items-center justify-content-center"
                style="width: 40px; height: 40px; cursor: pointer;" @click="$emit('back-ward')" title="Back">
                <i :class="`fa-solid fa-chevron-${$i18n.locale === 'en' ? 'left' : 'right'}`"></i>
            </span>
            <h3 class="position-relative mb-5">{{ $t('carBook.heading') }}</h3>
            <form class="d-flex flex-column gap-3">
                <div class="top d-flex gap-3 flex-md-row flex-column">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.type') }}:</span>
                        <span class="text-capitalize p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                            {{ $route.params.lang === 'en' ?
                                carInfo.car_name_en || 'Unknown' :
                                carInfo.car_name || 'غير معروف' }}
                        </span>
                    </label>
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.driver.head') }}:</span>
                        <select class="p-2 px-3 rounded-1 w-100" name="driver" v-model="bookInfo.width_driver">
                            <option value="0" v-if="carInfo.price_per_day > 0">{{ $t('carBook.details.driver.without') }}</option>
                            <option value="1" v-if="carInfo.price_with_driver > 0" selected>{{ $t('carBook.details.driver.with') }}</option>
                        </select>
                    </label>
                </div>
                <label class="w-100">
                    <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.price') }}:</span>
                    <span class="d-block p-2 px-3 rounded-1 bg-secondary bg-opacity-25">
                        {{
                            bookInfo.width_driver === "1"
                            ? USDollar.format(carInfo.price_with_driver || 0)
                            : USDollar.format(carInfo.price_per_day || 0)
                        }}
                    </span>
                </label>
                <div class="first d-flex flex-column flex-md-row gap-3 mt-3">
                    <div class="w-100 position-relative">
                        <span class="d-block mb-2 fw-semibold">{{ $t('carBook.details.fName') }}: </span>
                        <input :class="`p-2 px-3 w-100 rounded-1 ${validation.first_name.$error ? 'error' : ''}`"
                            type="text" v-model="bookInfo.first_name" placeholder="First Name" />
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
                        <input :class="`p-2 px-3 w-100 rounded-1 ${validation.last_name.$error ? 'error' : ''}`" type="text"
                            v-model="bookInfo.last_name" placeholder="Last Name" />
                        <span v-if="validation.last_name.$error" class="error d-block text-danger fst-italic mx-2">
                            {{
                                validation.last_name.$errors[0].$validator === "required"
                                ? "Field Reuqired"
                                : ""
                            }}
                        </span>
                    </div>
                </div>
                <div class="bottom-middle d-flex gap-3 flex-md-row flex-column">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.phone') }}:</span>
                        <input :class="`rounded-1 w-100 p-2 px-3 ${validation.phone_number.$error ? 'error' : ''
                            }`" type="Phone" placeholder="Phone Number" v-model="bookInfo.phone_number" />
                        <span v-if="validation.phone_number.$error" class="fst-italic text-danger d-block mt-2">
                            {{
                                validation.phone_number.$errors[0].$validator === "required"
                                ? "Field Required"
                                : "Invalid Phone Number"
                            }}
                        </span>
                    </label>
                </div>
                <div class="bottom d-flex gap-3 flex-md-row flex-column">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.fDate') }}:</span>
                        <span class="p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                            {{ bookInfo.start_date }}
                        </span>
                    </label>
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.tDate') }}:</span>
                        <span class="p-2 fw-semibold d-block px-3 rounded-1 bg-secondary bg-opacity-25">
                            {{ bookInfo.end_date }}
                        </span>
                    </label>
                </div>
                <div class="group-1 d-flex gap-3 flex-md-row flex-column">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.days') }}:</span>
                        <span class="d-block p-2 px-3 rounded-1 bg-secondary bg-opacity-25">
                            {{ bookInfo.total_days }}
                        </span>
                    </label>
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.total') }}:</span>
                        <span class="d-block p-2 px-3 rounded-1 bg-secondary bg-opacity-25">
                            {{ USDollar.format(bookInfo.total_amount || 0) }}
                        </span>
                    </label>
                </div>
                <div class="group-2 d-flex gap-3 flex-md-row flex-column">
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.tax') }}:</span>
                        <span class="d-block p-2 px-3 rounded-1 bg-secondary bg-opacity-25">
                            {{ (bookInfo.tax || 0) }}%
                        </span>
                    </label>
                    <label class="w-100">
                        <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.agentDiscount') }}:</span>
                        <span class="d-block p-2 px-3 rounded-1 bg-secondary bg-opacity-25">
                            {{ USDollar.format(bookInfo.agent_discount || 0) }}
                        </span>
                    </label>
                </div>
                <label class="w-100">
                    <span class="d-block mb-1 fw-semibold">{{ $t('carBook.details.net') }}:</span>
                    <span class="d-block p-2 px-3 rounded-1 bg-secondary bg-opacity-25">
                        <!-- {{ (bookInfo.net_amount || 0).toFixed(2) }}$ -->
                        {{ USDollar.format(bookInfo.net_amount || 0) }}
                    </span>
                </label>
                <label class="w-100">
                    <span class="d-block mb-1">{{ $t('carBook.details.notes') }}:</span>
                    <textarea :class="`w-100 rounded-1 p-2 px-3 ${validation.notes.$error ? 'error' : ''}`" name="notes"
                        :placeholder="$i18n.locale === 'en' ?
                            'Please write down the place the car will wait for you' :
                            'برجاء تحديد المكان الذي يتم استلام السياره منه'" v-model="bookInfo.notes"></textarea>
                    <span v-if="validation.notes.$error" class="text-danger mt-1 d-block fst-italic">
                        {{ validation.notes.required.$message }}
                    </span>
                </label>
                <button class="butn rounded-1 p-2 px-3 text-uppercase fw-semibold" @click.prevent="submission">
                    {{ $t('buttons.confirm') }}
                </button>
            </form>
        </div>
        <div :class="`alert alert-success alert-dismissible text-${$i18n.locale === 'en' ? 'start' : 'end'} position-fixed`"
            role="alert">
            <div class="d-flex align-items-center gap-2">
                <i class="fa-solid fa-circle-check fs-5"></i>
                {{ $t('carBook.bookMessage') }}
            </div>
            <span class="d-block mt-1">{{ `${$t('carBook.bookingCode')}: ${randomCode}` }}</span>
            <span class="d-block mt-1">{{ `${$t('carBook.yourBalance')}: ${USDollar.format(updatedBalance)}` }}</span>
            <button @click="removeAlert('success')" type="button" class="btn-close"></button>
        </div>
        <div class="alert alert-danger alert-dismissible text-center position-fixed" role="alert">
            <div class="d-flex align-items-center gap-2">
                <i class="fa-solid fa-circle-xmark fs-5"></i>
                {{ $t('carBook.noBalance') }}
            </div>
            <button @click="removeAlert('danger')" type="button" class="btn-close"></button>
        </div>
        <Loader v-if="loading"></Loader>
    </div>
</template>
<script setup>
import { ref, onMounted, computed, watch } from "vue";
import axios from "axios";
import { useRoute } from "vue-router";
import { useVuelidate } from "@vuelidate/core";
import { required, email, numeric, helpers } from "@vuelidate/validators";
import Loader from '../../components/Loader.vue';
import router from "../../router";
import i18n from "../../i18n";

const social = ref({})
const loading = ref(false)
const route = useRoute();
const userInfo = ref({});
const carInfo = ref({});
const randomCode = ref("");
const updatedBalance = ref('')
const props = defineProps(['searchInfo', 'carId'])



// Formating Balanc 
const USDollar = Intl.NumberFormat("en-US", {
    currency: "USD",
    style: "currency"
})

const bookInfo = ref({
    type_id: props.carId,
    width_driver: computed(() => carInfo.value.price_with_driver > 0 ? '1' : '0'),
    first_name: '',
    last_name: '',
    email: props.searchInfo.country,
    phone_number: "",
    start_date: props.searchInfo.fromDate.toLocaleDateString('en-CA'),
    end_date: props.searchInfo.toDate.toLocaleDateString('en-CA'),
    total_days: computed(() => {
        if (bookInfo.value.end_date === bookInfo.value.start_date) {
            return 1;
        } else return (new Date(bookInfo.value.end_date).getTime() - new Date(bookInfo.value.start_date).getTime()) / (1000 * 60 * 60 * 24).toString()
    }),
    total_amount: computed(() => {
        if (bookInfo.value.width_driver === "1") {
            return (
                parseFloat(bookInfo.value.total_days * carInfo.value.price_with_driver)
            ) || 0;
        } else
            return (
                parseFloat(bookInfo.value.total_days * carInfo.value.price_per_day)
            ) || 0;
    }),
    tax: computed(() => parseFloat(carInfo.value.tax) || 0),
    agent_discount: computed(() => {
        return ((bookInfo.value.total_amount + ((bookInfo.value.tax / 100) * bookInfo.value.total_amount)) * userInfo.value.discount) / 100;
    }),
    net_amount: computed(() => (bookInfo.value.total_amount + ((bookInfo.value.tax / 100) * bookInfo.value.total_amount)) - bookInfo.value.agent_discount) || 0,
    notes: "",
    account_owner: computed(() => userInfo.value.id),
});

const rules = {
    // type_id: { required },
    first_name: { required },
    last_name: { required },
    // email: { required, email },
    phone_number: { required, numeric },
    notes: { required: helpers.withMessage('Field Required', required) },
};
const validation = useVuelidate(rules, bookInfo);


const submission = async () => {
    validation.value.$validate();
    if (!validation.value.$error) {
        loading.value = true
        if (userInfo.value.balance > bookInfo.value.net_amount) {
            await axios.post("https://api.seasonsge.com/car-order", bookInfo.value)
                .then((response) => {
                    if (response.data.status) {
                        updatedBalance.value = parseFloat(userInfo.value.balance) - parseFloat(bookInfo.value.net_amount)
                        const balance = new FormData()

                        balance.append("name", userInfo.value.name)
                        balance.append("email", userInfo.value.email)
                        balance.append("password", userInfo.value.password)
                        balance.append("type", userInfo.value.type)
                        balance.append("id", userInfo.value.id)
                        balance.append("discount", userInfo.value.discount)
                        balance.append("balance", updatedBalance.value)

                        axios.post("https://api.seasonsge.com/user-edit", balance)
                            .then(userResponse => {
                                if (userResponse.data.success) {
                                    randomCode.value = response.data.random_code
                                    document.querySelector(".alert-success").classList.add("active")
                                    loading.value = false

                                    axios.get(`https://api.seasonsge.com/car-rr?id=${userInfo.value.id}`)
                                        .then(data => {
                                            const bookId = data.data.pop()
                                            setTimeout(() => {
                                                router.push({
                                                    name: "Agents Cars Checkout",
                                                    params: { lang: i18n.global.locale.value, id: bookId.id, with: 3 }
                                                })
                                            }, 1500)
                                        })
                                }
                            })
                    }
                });
        } else {
            document.querySelector(".alert-danger").classList.add("active")
            setTimeout(() => {
                document.querySelector(".alert-danger").classList.remove("active")
            }, 3000)
            loading.value = false
        }
    }
};

const removeAlert = (type) => {
    document.querySelector(`.alert-${type}`).classList.remove("active")
}


onMounted(async () => {
    loading.value = true
    await axios.get("https://api.seasonsge.com/cars-view").then((data) => {
        data.data.filter(el => {
            if (+el.type_id === props.carId) {
                carInfo.value = el
                loading.value = false
            }
        })
    });
    await axios.get("https://api.seasonsge.com/cars-type-view").then((data) => {
        data.data.filter(el => {
            if (el.id == props.carId) {
                carInfo.value['car_name_en'] = el.name_en
                carInfo.value['car_name'] = el.name
                carInfo.value['car_image'] = el.img
            }
        })
    });
    await axios.get("https://api.seasonsge.com/usersview").then((data) => {
        userInfo.value = data.data.filter((el) => el.id == route.params.userId)[0];
        loading.value = false
    });
    await axios.get("https://api.seasonsge.com/info")
        .then(data => {
            social.value = data.data[0]
        })
});
</script>
<style lang="scss" scoped>
.car-reservation {

    input,
    select,
    textarea {
        &.error {
            border: 1px solid #dc3545;
        }
    }

    input,
    select,
    textarea {
        outline: none;
        border: 1px solid darkgray;
    }

    textarea {
        height: 150px;
        resize: none;
    }

    .content {
        box-shadow: 0 10px 15px rgba(0, 0, 0, 0.09);

        h3 {
            color: var(--blue-color);

            &::before {
                content: "";
                position: absolute;
                width: 10%;
                height: 3px;
                border-radius: 50px;
                background-color: var(--blue-color);
                bottom: -10px;
            }
        }
    }

    .alert {
        top: -25%;
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
        .alert {
            width: 90%;
            text-align: start !important;
            font-size: 12px;

            i {
                font-size: 16px !important;
            }
        }

        padding: 10px !important;

        .content {
            padding: 20px 15px !important;
        }
    }
}
</style>
