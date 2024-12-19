<template>
  <div class="container">
    <h1 class="title">Avocado Price Prediction</h1>
    <form @submit.prevent="submitForm" class="form">
      <div class="form-row">
        <div class="form-group">
          <label for="Quality1">Quality 1</label>
          <input v-model="formData.Quality1" type="number" required class="input" />
        </div>

        <div class="form-group">
          <label for="Quality2">Quality 2</label>
          <input v-model="formData.Quality2" type="number" required class="input" />
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="Quality3">Quality 3</label>
          <input v-model="formData.Quality3" type="number" required class="input" />
        </div>

        <div class="form-group">
          <label for="SmallBags">Small Bags</label>
          <input v-model="formData['Small Bags']" type="number" required class="input" />
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="LargeBags">Large Bags</label>
          <input v-model="formData['Large Bags']" type="number" required class="input" />
        </div>

        <div class="form-group">
          <label for="XLargeBags">XLarge Bags</label>
          <input v-model="formData['XLarge Bags']" type="number" required class="input" />
        </div>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="year">Year</label>
          <input v-model="formData.year" type="number" required class="input" />
        </div>

        <div class="form-group">
          <label for="type">Type</label>
          <select v-model="formData.type" required class="select">
            <option value="" disabled selected>Select type</option>
            <option value="conventional">Conventional</option>
            <option value="organic">Organic</option>
          </select>
        </div>
      </div>

      <div class="form-group">
        <label for="region">Region</label>
        <input v-model="formData.region" type="text" required class="input" />
      </div>

      <button type="submit" class="btn">Predict Price</button>
    </form>

    <div v-if="predictedPrice !== null" class="result">
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
        'Small Bags': null,
        'Large Bags': null,
        'XLarge Bags': null,
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
          // Formater le prix à 2 décimales
          this.predictedPrice = data.predicted_price.toFixed(2);
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
/* Global Container */
.container {
  max-width: 800px;
  margin: 40px auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.title {
  text-align: center;
  font-size: 2rem;
  color: #333;
  margin-bottom: 20px;
}

/* Form Styling */
.form {
  display: flex;
  flex-direction: column;
}

.form-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
}

.form-group {
  flex-basis: 48%; /* Prend 48% de la largeur pour chaque champ */
}

label {
  font-weight: bold;
  font-size: 1rem;
  margin-bottom: 5px;
  color: #555;
}

.input,
.select {
  width: 100%;
  padding: 10px;
  margin: 5px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.input:focus,
.select:focus {
  border-color: #4CAF50;
  outline: none;
}

select {
  height: 40px;
}

/* Button Styling */
.btn {
  padding: 12px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1.1rem;
  margin-top: 20px;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #45a049;
}

/* Result Styling */
.result {
  text-align: center;
  margin-top: 30px;
}

.result h2 {
  color: #333;
  font-size: 1.5rem;
}

/* Error Styling */
.error {
  text-align: center;
  margin-top: 20px;
  color: red;
}

/* Mobile Responsiveness */
@media (max-width: 600px) {
  .container {
    padding: 10px;
  }

  .title {
    font-size: 1.5rem;
  }

  .form {
    margin-top: 20px;
  }

  .btn {
    font-size: 1rem;
    padding: 10px 15px;
  }

  .form-row {
    flex-direction: column;
  }

  .form-group {
    flex-basis: 100%;
    margin-bottom: 10px;
  }
}
</style>