<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-sm-6 mx-auto">
        <form @submit.prevent="registerUser" novalidate>

          <!-- Первый шаг -->
          <div v-show="step === 1" class="step">

            <div class="form-group">
              <label for="name">Ваше имя</label>
              <input 
              @blur="$v.formReg.name.$touch()" 
              v-model="formReg.name" type="text" 
              :class="{'is-invalid': $v.formReg.name.$error}" 
              class="form-control" id="name">

              <div v-if="!$v.formReg.name.required" class="invalid-feedback">
                {{requiredField}}
              </div>
              <div v-if="!$v.formReg.name.alpha" class="invalid-feedback">
                {{requiredText}}
              </div>
            </div>
    
            <div class="form-group">
              <label for="surname">Ваша фамилия</label>
              <input 
              @blur="$v.formReg.surname.$touch()"  
              v-model="formReg.surname" type="text" 
              :class="{'is-invalid': $v.formReg.surname.$error}" 
              class="form-control" id="surname">

              <div v-if="!$v.formReg.surname.required" class="invalid-feedback">
                {{requiredField}}
              </div>
              <div v-if="!$v.formReg.surname.alpha" class="invalid-feedback">
                {{requiredText}}
              </div>
            </div>
  
            <div class="form-group">
              <label for="email">Ваш почтовый ящик</label>
              <input 
              @blur="$v.formReg.email.$touch()"  
              v-model="formReg.email" type="text" 
              :class="{'is-invalid': $v.formReg.email.$error}" 
              class="form-control" id="email">

              <div v-if="!$v.formReg.email.required" class="invalid-feedback">
                {{requiredField}}
              </div>
              <div v-if="!$v.formReg.email.email" class="invalid-feedback">
                {{requiredEmail}}
              </div>
            </div>
      
            <button @click="nextStep" :disabled="disabledBtn1" type="button" class="btn btn-primary">Следующий шаг</button>

          </div>

           <!-- Второй шаг -->
          <transition name="slide-fade"> 
            <div v-show="step === 2" class="step">
  
              <div class="form-group">
                <label for="password">Ваш пароль</label>
                <input 
                @blur="$v.formReg.password.$touch()" 
                v-model="formReg.password" type="password" 
                :class="{'is-invalid': $v.formReg.password.$error}" 
                class="form-control" id="password">

                <div v-if="!$v.formReg.password.required" class="invalid-feedback">
                  {{requiredField}}
                </div>
                <div v-if="!$v.formReg.password.minLength" class="invalid-feedback">
                  {{lengthMin}}
                </div>
              </div>
      
              <div class="form-group">
                <label for="passwordConfirm">Подтвердите пароль</label>
                <input 
                @blur="$v.formReg.passwordConfirm.$touch()" 
                v-model="formReg.passwordConfirm" type="password" 
                :class="{'is-invalid': $v.formReg.passwordConfirm.$error}" 
                class="form-control" id="passwordConfirm">

                <div v-if="!$v.formReg.passwordConfirm.sameAs" class="invalid-feedback">
                  {{passwordSame}}
                </div>
              </div>
  
              <button @click="backStep" type="button" class="btn btn-light mr-4">Назад</button>
              <button @click="nextStep" :disabled="disabledBtn2" type="button" class="btn btn-primary">Следующий шаг</button>
  
            </div>
           </transition>

          <!-- Третий шаг -->
          <transition name="slide-fade"> 
            <div v-show="step === 3" class="step">
  
              <div class="form-group">
                <label for="country">Страна</label>
                <input 
                @blur="$v.formReg.country.$touch()" 
                v-model="formReg.country" type="text" 
                :class="{'is-invalid': $v.formReg.country.$error}"  
                class="form-control" id="country">

                <div v-if="!$v.formReg.country.alpha" class="invalid-feedback">
                  {{requiredText}}
                </div>
              </div>
      
              <div class="form-group">
                <label for="city">Город</label>
                <input 
                @blur="$v.formReg.city.$touch()" 
                v-model="formReg.city" type="text" 
                :class="{'is-invalid': $v.formReg.city.$error}" 
                class="form-control" id="city">

                <div v-if="!$v.formReg.city.alpha" class="invalid-feedback">
                  {{requiredText}}
                </div>
              </div>
  
              <button @click="backStep" type="button" class="btn btn-light mr-4">Назад</button>
              <button @click="nextStep" type="type" class="btn btn-primary">Зарегистрироваться</button>
  
            </div>
          </transition>

        </form>
      </div>
    </div>
  </div>
</template> 

<script>
import { required, email, helpers, minLength, sameAs } from 'vuelidate/lib/validators'
const alpha = helpers.regex('alpha', /^[a-zA-Zа-яёА-ЯЁ]*$/)

export default {
  data() {
    return {
      step: 1,
      requiredField: 'Обязательное поле',
      requiredEmail: 'Введите правильный email адрес',
      requiredText: 'Нельзя писать числа',
      lengthMin: 'Не меньше 6 символов',
      passwordSame: 'Пароли не совпадают',
      formReg: {
        name: '',
        surname: '',
        email: '',
        password: '',
        passwordConfirm: '',
        country: '',
        city: ''
      }
    }
  },
  computed:{
    disabledBtn1() {

      return this.$v.formReg.name.$invalid ||
      this.$v.formReg.surname.$invalid ||
      this.$v.formReg.email.$invalid
    },
    disabledBtn2() {

      return this.$v.formReg.password.$invalid ||
      this.$v.formReg.passwordConfirm.$invalid 
    }
  },
  methods: {
    nextStep() {
      if (this.step < 3) {
        this.step++
      }
    },
    backStep() {
      if (this.step > 1) {
        this.step--
      }
    },
    registerUser() {
      console.log('Yes');
      console.log(this.formReg.name);

      this.step = 1;

      for (let input in this.formReg) {
        this.formReg[input] = ''
      }

      this.$v.$reset()
    }
  },
  validations: {
    formReg: {
      name: {
        required,
        alpha
      },
      surname: {
        required,
        alpha
      },
      email: {
        email,
        required
      },
      password: {
        required,
        minLength: minLength(6)
      },
      passwordConfirm: {
        sameAs: sameAs('password')
      },
      country: {
        alpha
      },
      city: {
        alpha
      }
    }
  }
}
</script>

<style>
.slide-fade-enter-active {
  transition: all 0.3s ease;
}

.slide-fade-enter {
  transform: translateX(10px);
  opacity: 0;
}
</style>
