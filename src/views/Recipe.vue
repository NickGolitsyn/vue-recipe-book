<template>
  <div>
    <h1>{{recipe.title}}</h1>
    <p>Время готовки: {{recipe.time}} {{minutes(recipe.time)}}.</p>
    <p>{{recipe.method}}</p>
    <ol>
      <li v-for="i in recipe.ingredients" v-bind:key="i">{{i}}</li>
    </ol>
  </div>
</template>

<script>
  import { db } from '../firebase/index'
  import { doc, getDoc } from "firebase/firestore";

  export default {
    data() {
      return {
        recipe: {ingredients: [], metod: "", time: null, title: ""},
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
      }
    },
    async created() {
      const docRef = doc(db, "recipes", this.$route.params.id);
      const docSnap = await getDoc(docRef);

      if (docSnap.exists()) {
        console.log("Document data:", docSnap.data());
        this.recipe = docSnap.data()
        console.log(this.recipe);
      } else {
        console.log("No such document!");
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>