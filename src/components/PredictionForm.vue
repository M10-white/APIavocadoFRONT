<template>
  <div>
    <h1>Avocado Price Prediction</h1>
    <form @submit.prevent="submitForm">
      <label for="Quality1">Quality1:</label>
      <input v-model="formData.Quality1" type="number" required /><br>

      <label for="Quality2">Quality2:</label>
      <input v-model="formData.Quality2" type="number" required /><br>

      <label for="Quality3">Quality3:</label>
      <input v-model="formData.Quality3" type="number" required /><br>

      <label for="SmallBags">Small Bags:</label>
      <input v-model="formData.SmallBags" type="number" required /><br>

      <label for="LargeBags">Large Bags:</label>
      <input v-model="formData.LargeBags" type="number" required /><br>

      <label for="XLargeBags">XLarge Bags:</label>
      <input v-model="formData.XLargeBags" type="number" required /><br>

      <label for="year">Year:</label>
      <input v-model="formData.year" type="number" required /><br>

      <label for="type">Type:</label>
      <input v-model="formData.type" type="text" required /><br>

      <label for="region">Region:</label>
      <input v-model="formData.region" type="text" required /><br><br>

      <button type="submit">Predict Price</button>
    </form>

    <div v-if="predictedPrice !== null">
      <h2>Predicted Price: {{ predictedPrice }}</h2>
    </div>

    <div v-if="errorMessage" class="error">
      <h2>{{ errorMessage }}</h2>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        Quality1: null,
        Quality2: null,
        Quality3: null,
        SmallBags: null,
        LargeBags: null,
        XLargeBags: null,
        year: null,
        type: '',
        region: ''
      },
      predictedPrice: null,
      errorMessage: null
    };
  },
  methods: {
    async submitForm() {
      try {
        const response = await fetch('http://127.0.0.1:5000/predict', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(this.formData)
        });

        const data = await response.json();
        if (data.predicted_price) {
          this.predictedPrice = data.predicted_price;
          this.errorMessage = null;
        } else {
          this.errorMessage = data.error || 'An error occurred';
          this.predictedPrice = null;
        }
      } catch (error) {
        this.errorMessage = 'Failed to connect to the server';
        this.predictedPrice = null;
      }
    }
  }
};
</script>

<style scoped>
h1 {
  text-align: center;
}

form {
  margin: 20px;
  display: flex;
  flex-direction: column;
  max-width: 300px;
  margin: 0 auto;
}

label,
input {
  margin-bottom: 10px;
}

button {
  background-color: #4CAF50;
  color: white;
  padding: 10px;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

.error {
  color: red;
  text-align: center;
  margin-top: 20px;
}
</style>
