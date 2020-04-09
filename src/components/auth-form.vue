<template>
  <div class="form">
    <label class="form__row">
      <div class="form__label">
        email
      </div>
      <input
        type="text"
        class="form__input"
        :class="{ 'form__input--invalid': !emailValid }"
        v-model="credintails.login"
      />
      <div v-show="!emailValid" class="form__invalid">
        invalid email
      </div>
    </label>

    <label class="form__row">
      <div class="form__label">
        Password
      </div>
      <input
        :type="passwordInputType"
        class="form__input"
        v-model="credintails.password"
      />
      <a class="form__password-show" href="#" @click="togglePasswordHiden">{{
        showPasswordText
      }}</a>
    </label>

    <div v-show="errorText" class="form__error">
      {{ errorText }}
    </div>

    <button class="form__btn" @click="login">
      log in
    </button>
  </div>
</template>

<script>
import Cookies from 'js-cookie'

export default {
  name: "auth-form",

  data() {
    return {
      passwordInputType: "password",
      errorText: "",
      credintails: {
        login: "",
        password: ""
      }
    };
  },

  computed: {
    emailValid() {
      const re = /\S+@\S+\.\S+/;
      return (
        re.test(this.credintails.login) || this.credintails.login.length === 0
      );
    },
    showPasswordText() {
      return this.passwordInputType === "password"
        ? "show password"
        : "hide password";
    }
  },

  methods: {
    togglePasswordHiden() {
      this.passwordInputType === "password"
        ? (this.passwordInputType = "text")
        : (this.passwordInputType = "password");
    },

    login() {
      switch (true) {
        case !this.credintails.login:
          this.errorText = "enter an email";
          break;
        case !this.credintails.password:
          this.errorText = "enter password";
          break;
        case !this.emailValid:
          this.errorText = "enter a valid email";
          break;
        case this.auth():

          break;
        default:
      }
    },

    auth() {
      this.errorText = ""

      const database = require('../passwords.json')

      const user = database.find(pair => pair.login === this.credintails.login) ?? false

      if (!user) {
        this.errorText = "user not found"
        return false;
      }

      if (user.password !== this.credintails.password) {
        this.errorText = "password incorrect"
        return false;
      }

      Cookies.set('user', this.credintails.login)

      this.$emit('form:success', this.credintails.login)
    }
  }
};
</script>

<style scoped>
.form {
  padding: 2rem;
  border: 1px solid rgba(0, 0, 0, 0.05);
}
.form__row {
  margin-bottom: 1rem;
  display: block;
}

.form__label {
  text-align: left;
  margin-bottom: 0.5rem;
  font-weight: bold;
  text-transform: lowercase;
  letter-spacing: 1px;
}
.form__input {
  width: 100%;
  font: inherit;
  padding: 0.2rem 0.5rem;
  border: 1px solid rgba(0, 0, 0, 0.1);
  margin-bottom: 0.5rem;
  letter-spacing: 1px;
  outline: none;
}
.form__input--invalid {
  border-color: red;
}
.form__password-show {
  font-size: 0.8rem;
  color: #692aff;
  text-decoration: none;
  border-bottom: 1px dashed;
}
.form__invalid {
  font-size: 0.8rem;
  font-weight: bold;
  letter-spacing: 1px;
  color: red;
}
.form__btn {
  font: inherit;
  background: linear-gradient(180deg, #e00c81, #8f00bd);
  border: 2px solid;
  color: #fff;
  font-weight: bold;
  text-transform: uppercase;
  padding: 0.5rem 1.5rem;
  box-shadow: 0px 5px 10px 0px #bbbbbb;
  font-size: 0.8rem;
  outline: none;
  cursor: pointer;
  border-radius: 5px;
  transition: transform 0.2s ease;
}
.form__btn:active {
  transform: scale(0.95);
}
.form__error {
  font-weight: 200;
  color: red;
  font-size: .8rem;
  margin-bottom: 1rem;
  letter-spacing: 1px;
}
</style>
