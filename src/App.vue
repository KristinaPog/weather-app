<script>
import axios from "axios";
export default {
  data() {
    return {
      city: " ",
      error: " ",
      info: null,
    };
  },
  computed: {
    cityName() {
      return "«" + this.city + "»";
    },
    showTemp() {
      return "Температурa: " + this.info.main.temp
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min
    },
    showMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название длиннее одного символа: ";
        return false;
      }

      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ba7f1407603002e9806f1e729dd82928`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city === "" ? "вашем городе" : city }}</p>
    <input
      type="text"
      placeholder="Введите город"
      v-on:input="this.city = $event.target.value"
    />
    <button v-show="city != ''" @click="getWeather()">Получить погоду</button>
    <p class="error">{{ error }}</p>
    <div v-show="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
    
  </div>
</template>

<style scoped>
.error {
  color: #d03939;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #2e0061;
  /* display: flex;
    flex-direction: column; */
  padding: 20;
  text-align: center;
}

.wrapper h1 {
  padding: 50px;
  color: #fcfcfc;
}

.wrapper p {
  margin-top: 20px;
  color: #fcfcfc;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #995fdb;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #8c47d9;
}

.wrapper button {
  background: #8c47d9;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #995fdb;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
