<template>
  <div class="container">
    <h1>Рассчитайте стоимость автомобиля в лизинг</h1>
    <form class="form" @submit.prevent="handleSubmit">
      <div class="inputs__wrapper">
        <input-app v-model="price" label="Стоимость автомобиля" :min="minPrice" :max="maxPrice" text="&#8381;" />
        <input-app v-model="initial" label="Первоначальный взнос" :min="initialMin" :max="initialMax" :total="price" :isReadonly="true" />
        <input-app v-model="months" label="Срок лизинга" :min="monthsMin" :max="monthsMax" text="мес." />
      </div>
      <div class="nums__wrapper">
        <div class="num">
          <div class="num__label">Сумма договора лизинга</div>
          <div class="num__text">{{ totalSum }} <span class="rub"> &#8381;</span></div>
        </div>
        <div class="num">
          <div class="num__label">Ежемесячный платеж от</div>
          <div class="num__text">{{ monthPay }} ₽</div>
        </div>
        <div class="num">
          <button-app :isLoading="isLoading"></button-app>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import InputApp from './InputApp.vue'
import ButtonApp from './ButtonApp.vue'
import axios from 'axios'
export default {
  components: { InputApp, ButtonApp },
  data() {
    return {
      price: 3300000,
      minPrice: 1000000,
      maxPrice: 6000000,
      initial: 13,
      initialMin: 10,
      initialMax: 60,
      months: 60,
      monthsMin: 1,
      monthsMax: 60,
      rate: 0.035,
      isLoading: false,
    }
  },
  methods: {
    handleSubmit() {
      this.isLoading = true
      const data = {
        price: this.price,
        initial: this.initial,
        months: this.months,
      }
      axios
        .post('https://eoj3r7f3r4ef6v4.m.pipedream.net', data)
        .then(function (response) {
          console.log(response)
        })
        .catch(function (error) {
          console.log(error.response)
        })
        .finally(() => (this.isLoading = false))
    },
  },
  computed: {
    initialPay() {
      return (this.price * this.initial) / 100
    },
    monthPay() {
      const initial = this.initialPay
      const rate = this.rate
      const res = ((this.price - initial) * (rate * Math.pow(1 + rate, this.months))) / (Math.pow(1 + rate, this.months) - 1)
      return res.toFixed(0)
    },
    totalSum() {
      return (this.initialPay + this.monthPay * this.months).toFixed(0)
    },
  },
}
</script>
