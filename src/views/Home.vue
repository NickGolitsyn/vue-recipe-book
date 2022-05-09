<template>
  <main class="home">
    <router-link to="/recipe">Добавить Рецепт</router-link>
    <h1>Рецепты</h1>
    <ul>
      <li v-for="recipe in recipes" v-bind:key="recipe.id">
        <div>
          <h3>{{recipe.title}}</h3>
          <p>{{recipe.time}} {{minutes(recipe.time)}}</p>
          <a :href="'/recipe/' + recipe.id">Открыть</a>
        </div>
        <button class="btn red" @click="handleDelete(recipe.id)">Delete</button>
      </li>
    </ul>
  </main>
</template>

<script>
import { auth, db } from '../firebase/index'
import { query, getDocs, doc, deleteDoc, collection, orderBy, where } from "firebase/firestore"; 

export default {
  data() {
    return {
      recipes: [],
    };
  },
  methods: {
    minutes(t) {
      let n = Math.abs(t) % 100; 
      let n1 = n % 10;
      if (n > 10 && n < 20) { return "минут" }
      if (n1 > 1 && n1 < 5) { return "минуты" }
      if (n1 == 1) { return "минута" }
      return "минут";
    },
    async handleDelete(id) {
      await deleteDoc(doc(db, "recipes", id));
      this.recipes = []
      const q = query(collection(db, "recipes"), where("uid", "==", auth.currentUser.uid), orderBy("timestamp", "desc"));
      const querySnapshot = await getDocs(q);
      querySnapshot.forEach((doc) => {
        this.recipes.push({ id: doc.id, ...doc.data() })
      });
    }
  },
  async created() {
    const q = query(collection(db, "recipes"), where("uid", "==", auth.currentUser.uid), orderBy("timestamp", "desc"));
    const querySnapshot = await getDocs(q);
    // const querySnapshot = await getDocs(collection(db, "recipes"));
    querySnapshot.forEach((doc) => {
      this.recipes.push({ id: doc.id, ...doc.data() })
    });
  },
}
</script>
<style scoped>
ul {
  list-style-type: none;
}
ul > li {
  padding: 1rem 1rem;
  margin: 1rem auto;
  border: 1px solid rgba(0, 0, 0, 0.5);
  max-width: fit-content;
  border-radius: 5px;
  min-width: 150px;
}
li > .red {
  margin-top: 1rem;
}
</style>
