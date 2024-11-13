<template>
  <div class="container mt-5">
    <h2 class="mb-4">Máy đổi tiền tệ</h2>
    <form @submit.prevent="convertCurrency">
      <div class="row mb-3">
        <div class="col">
          <label for="amount" class="form-label">Khoản tiền</label>
          <input
            type="number"
            v-model="amount"
            id="amount"
            class="form-control form-control-lg"
            placeholder="Điền vào khoản tiền"
            min="1"
            required
          />
        </div>
      </div>
      <div class="row mb-3">
        <div class="col">
          <label for="fromCurrency" class="form-label">Từ</label>
          <select
            v-model="fromCurrency"
            id="fromCurrency"
            class="form-select form-select-lg"
          >
            <option
              v-for="currency in currencies"
              :key="currency"
              :value="currency"
            >
              {{ currency }}
            </option>
          </select>
        </div>
        <div class="col">
          <label for="toCurrency" class="form-label">Sang</label>
          <select
            v-model="toCurrency"
            id="toCurrency"
            class="form-select form-select-lg"
          >
            <option
              v-for="currency in currencies"
              :key="currency"
              :value="currency"
            >
              {{ currency }}
            </option>
          </select>
        </div>
      </div>
      <button type="submit" class="btn btn-primary btn-lg">Đổi tiền</button>
    </form>
    <div v-if="convertedAmount !== null" class="mt-4">
      <h4>
        Số tiền đã đổi: {{ formatNumber(convertedAmount) }} {{ toCurrency }}
      </h4>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      amount: 1,
      fromCurrency: "USD",
      toCurrency: "EUR",
      convertedAmount: null,
      currencies: [
        "USD",
        "VND",
        "EUR",
        "GBP",
        "JPY",
        "AUD",
        "CAD",
        "CHF",
        "CNY",
        "SEK",
        "NZD",
      ],
      apiKey: "8a093256217bd7e0c12987a8",
    };
  },
  methods: {
    async convertCurrency() {
      try {
        const response = await fetch(
          `https://v6.exchangerate-api.com/v6/${this.apiKey}/pair/${this.fromCurrency}/${this.toCurrency}/${this.amount}`
        );
        const data = await response.json();
        if (data.result === "success") {
          this.convertedAmount = data.conversion_result;
        } else {
          alert("Error fetching conversion rate");
        }
      } catch (error) {
        console.error("Error:", error);
      }
    },
    formatNumber(number) {
      return new Intl.NumberFormat().format(number);
    },
  },
};
</script>
