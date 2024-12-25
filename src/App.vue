<script>
import axios from "axios";
import User from "./components/User.vue";

export default {
  components: { User },
  data() {
    return {
      users: [],
      error: "",
      userName: "",
      userPass: "",
      userEmail: "",
      price: 0,
      city: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "<<" + this.city + ">>"
    },
    showTemp() {
      return "Температура: " + this.info.main.temp
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
    sendData() {
      if (this.userName == "") {
        this.error = "Имя не введено";
        return;
      } else if (this.userPass == "") {
        this.error = "Пароль не введен";
        return;
      } else if (this.userEmail == "") {
        this.error = "Почта не введена";
        return;
      }

      this.error = "";

      this.users.push({
        name: this.userName,
        pass: this.userPass,
        email: this.userEmail
      })
    },
    deleteUser(index) {
      this.users.splice(index, 1);
    },
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа"
        return false;
      }

      this.error = "";

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=9c10dd5bd1a5404e7c1d9181abf61545`)
        .then(res => (this.info = res.data))
    }
  }
}

</script>

<template>
  <div className="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе " :  cityName}}</p>
    <input type="text" v-model="city" placeholder="Введите город">
    <button v-if="city != ''" @click="getWeather()">Получить городу</button>
    <button disabled v-else>Введите название города</button>
    <p className="error">{{ error }}</p>

    <div v-if="info != null">
      <p >{{ showTemp }}</p>
      <p >{{ showFeelsLike }}</p>
      <p >{{ showMinTemp }}</p>
      <p >{{ showMaxTemp }}</p>
    </div>
  </div>
  <!-- <input type="text" v-model="userName" placeholder="Название товара">
  <input type="number" v-model="userPass" placeholder="Стоимость">
  <input type="number" v-model="userEmail" placeholder="Количество">
  <button type="button" @click="sendData()">Отправить</button>
  <p className="error">{{ error }}</p>
  <div v-if="users.length == 0">
    У нас нет пользователей
  </div>
  <div v-else-if="users.length == 1">
    Users has 1 element
  </div>

  <User v-for="(el, index) in users" :key="index" :user="el" :index="index" :deleteUser="deleteUser" />
  <p v-if="users.length != 0" v-for="(el) in users">Стоимость {{el.pass }}</p> -->
</template>

<style scoped>

.error {
  color: #d03939;
}
.wrapper {
  width: 900px;
  max-width: 100%;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  text-align: center;
  padding: 20px;
  color: #fff;
}

.wrapper h1 {
margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  font-family: "Montserrat", serif;
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button:disabled {
  background: #746027;
  cursor: not-allowed;
}

.wrapper button {
  font-family: "Montserrat", serif;
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
