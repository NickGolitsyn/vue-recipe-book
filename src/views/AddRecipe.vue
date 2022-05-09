<template>
  <div>
    <h1>Добавить Рецепт</h1>
    <form @submit="handleSubmit">
      <label>
        <p>Название:</p>
        <input v-model="title" required type="text">
      </label>
      <label>
        <p>Как готовить:</p>
        <textarea rows="5" v-model="method" required type="text" />
      </label>
      <label>
        <p>Время (мин.):</p>
        <input v-model="time" required type="number" min="0">
      </label>
      <label>
        <p>Ингредиенты:</p>
        <div class="ing-box">
          <input v-model="newIngredient" v-on:keydown.enter.prevent='handleAdd' ref="ingInp" type="text">
          <button class="btn" type="button" @click="handleAdd">Добавить</button>
        </div>
        <div>
          <span v-for="i in ingredients" v-bind:key="i">{{i}}, </span>
        </div>
      </label>
      <button class="btn red" @click="handleReset" type="reset">Отменить</button>
      <button class="btn" type="submit">Добавить</button>
    </form>
  </div>
</template>

<script>
import { db, timestamp, auth } from '../firebase/index'
import { collection, addDoc } from "firebase/firestore"; 

  export default {
    data() {
      return {
        title: '',
        method: '',
        time: '',
        newIngredient: '',
        ingredients: [],
      }
    },
    methods: {
      async handleSubmit(e) {
        e.preventDefault()
        const docRef = await addDoc(collection(db, "recipes"), {
          title: this.title,
          method: this.method,
          time: this.time,
          ingredients: this.ingredients,
          timestamp: timestamp,
          uid: auth.currentUser.uid
        });
        console.log("Document written with ID: ", docRef.id);
        this.title = ''
        this.method = ''
        this.time = ''
        this.newIngredient = ''
        this.ingredients = []
      },
      handleReset(e) {
        e.preventDefault()
        this.title = ''
        this.method = ''
        this.time = ''
        this.newIngredient = ''
        this.ingredients = []
      },
      handleAdd(e) {
        e.preventDefault();
        const ing = this.newIngredient.trim()

        if (ing && !this.ingredients.includes(ing)) {
          // this.ingredients = (prevIngredients => [...prevIngredients, ing])
          this.ingredients.push(ing)
        }
        console.log(this.newIngredient);
        console.log(this.ingredients);
        this.newIngredient = '' 
        this.$refs.ingInp.focus()
      }
    }
    
  }
</script>

<style scoped>
  form {
    padding-top: 3rem !important;
  }
  input, textarea {
    margin: 1rem auto;
    border: 1px solid rgba(0, 0, 0, 0.5) !important;
    border-radius: 5px;
    padding: 0.5rem !important;
  }
  .ing-box {
    display: flex;
    margin: auto;
    max-width: 400px;
    align-items: center;
  }
  .ing-box input, .ing-box textarea {
    margin-left: 0;
    margin-right: 0;
    font-weight: 400 !important;
  }
  .ing-box .btn {
    margin: 0 0 0 0.5rem;
    height: fit-content;
  }
  textarea {
    resize: none;
    width: 100%;
    max-width: 400px;
    font-family: inherit;
    font-size: 1.5rem;
  }
  input:focus, textarea:focus {
    border: 2px solid black !important;
    transition: none;
  }
</style>