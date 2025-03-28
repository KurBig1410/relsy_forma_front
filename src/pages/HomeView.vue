<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import { Logo } from '@/shared/logo'

const form = ref({
  email: '',
  sity: '',
  prise: '',
  table_link: '',
  master_info: '',
  long_offers: '',
  description: '',
})
const message = ref('')

// Список динамических полей
const dynamicFields = ref<string[]>([])

// Добавить новое поле
const addInputField = () => {
  dynamicFields.value.push('')
}

const submitForm = async () => {
  try {
    const response = await axios.post('http://127.0.0.1:8000/submit', form.value)
    message.value = response.data.message
    form.value = {
      email: '',
      sity: '',
      prise: '',
      table_link: '',
      master_info: '',
      long_offers: '',
      description: '',
    }
  } catch (error) {
    console.error(error)
    message.value = 'Failed to submit the application.'
  }
}
</script>

<template>
  <div class="main">
    <Logo />
    <form class="form-container" @submit.prevent="submitForm">
      <h1 class="title_form">Внесение правок на федеральный сайт</h1>
      <div id="dynamic-form">
        <div class="form-group">
          <label for="email">Электронная почта *</label>
          <textarea
            name="email"
            id="email"
            rows="2"
            placeholder="Введите ваш email"
            v-model="form.email"
            required
          ></textarea>
        </div>

        <!-- Динамические поля -->
        <div v-for="(field, index) in dynamicFields" :key="index" class="form-group">
          <label>Внесение правок на федеральный сайт {{ index + 1 }}</label>
          <textarea
            rows="2"
            placeholder="Введите значение"
            v-model="dynamicFields[index]"
          ></textarea>
        </div>

        <button type="button" class="add-button" @click="addInputField">+ Ещё</button>

        <div class="form-group">
          <label for="address">Город и адрес(а) вашей студии *</label>
          <textarea
            v-model="form.sity"
            rows="2"
            placeholder="Введите адрес студии"
            required
          ></textarea>
        </div>

        <div class="form-group">
          <label>Акционная цена на первое посещение студии</label>
          <li class="description">
            если нет акционной стоимости, то укажем стоимость за часовой сеанс
          </li>
          <textarea
            name="promo-price"
            rows="2"
            placeholder="Введите стоимость"
            required
            v-model="form.prise"
          ></textarea>
        </div>

        <div class="form-group">
          <label>Прикрепите ссылку на таблицу с прайсом *</label>
          <p class="description">
            * перед заполнением посмотрите на странице своего города описание программ и внесите
            новую актуальную информацию в соответствующую строку таблицы прайса
          </p>
          <textarea
            name="promo-price"
            rows="2"
            placeholder="Введите стоимость"
            required
            v-model="form.table_link"
          ></textarea>
        </div>

        <div class="form-group">
          <label>Информация о мастерах</label>
          <li class="description">
            здесь вы можете прикрепить ссылку на папку с фотографиями мастеров. Важно переименовать
            фотографии в соответсвии с именами мастером. Также прикрепите ссылку на описание в
            текстовом формате.
          </li>
          <textarea
            name="promo-price"
            rows="2"
            placeholder="Введите стоимость"
            required
            v-model="form.master_info"
          ></textarea>
        </div>

        <div class="form-group">
          <label>Долгосрочные офферы</label>
          <li class="description">
            скидка на первое посещение, скидка в честь дня рождения клиента и.т.д.
          </li>
          <textarea
            name="promo-price"
            rows="2"
            placeholder="Введите стоимость"
            required
            v-model="form.long_offers"
          ></textarea>
        </div>

        <div class="form-group">
          <label>Дополнительные изменения для сайта</label>
          <textarea
            name="promo-price"
            rows="2"
            placeholder="Введите стоимость"
            required
            v-model="form.description"
          ></textarea>
        </div>
      </div>

      <button type="submit" class="submit-button">Отправить</button>
      <p v-if="message">{{ message }}</p>
    </form>
  </div>
</template>

<style scoped>
body {
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.form-container {
  background-color: #fff;
  padding: 64px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 600px;
}

.form-container h1 {
  font-size: 20px;
  margin-bottom: 50px;
  text-align: center;
}

.form-group {
  width: 100%;
  margin-bottom: 5px;
}

.form-group label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

.form-group textarea {
  width: 96%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 14px;
  resize: vertical;
}

.add-button {
  display: inline-block;
  margin: 5px 0;
  padding: 10px 15px;
  font-size: 14px;
  color: #fff;
  background-color: #007bff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.add-button:hover {
  background-color: #0056b3;
}

.form-group .description {
  font-size: 12px;
  color: #666;
  margin-top: 5px;
}

.submit-button {
  display: block;
  width: 100%;
  padding: 10px 0;
  font-size: 16px;
  color: #fff;
  background-color: #28a745;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.submit-button:hover {
  background-color: #218838;
}

.main {
  display: flex;
  align-items: center;
  flex-direction: column;
}
</style>
