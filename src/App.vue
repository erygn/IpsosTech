<template>
  <div class="bg-gray-100 min-h-screen w-full text-center flex justify-center">
    <div class="px-4 pt-8 max-w-7xl">
      <h1 class="text-center text-6xl font-bold mb-4">IpsosTech</h1>
      <p class="mb-8">Pour suivre vos listes favorites</p>

      <h2 class="font-bold text-xl">Votez pour votre liste</h2>
      <div class="grid grid-cols-3 gap-4">
        <div class="p-4" v-for="(item, index) in list" :key="index">
          <div class="aspect-square rounded-full mb-2" :style="'background-image: url(\'' + item.img + '\'); background-position: center; background-size: contain'"></div>
          <h2 class="font-bold h-12 mb-2">{{ item.title }}</h2>

          <button @click="up(index)" class="w-full rounded text-white p-1" :style="'background-color:' + item.color">+</button>
          <div class="font-bold p-1">{{ item.value }}</div>
          <button @click="down(index)" class="w-full rounded text-white p-1" :style="'background-color:' + item.color">-</button>
        </div>
      </div>

      <div class="mt-4">
        <p>Site mis en place avec l'aide de <a class="underline text-blue-600 font-bold" href="https://rakura.fr" target="_blank">Rakura</a>, site d'escape game en ligne</p>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from "firebase/app";
import 'firebase/database'
export default {
  data() {
    return {
      list: null,
    }
  },
  methods: {
    up(li) {
      this.list[li].value++
      firebase.database().ref('List/' + li).update({
        value: this.list[li].value
      })
    },
    down(li) {
      this.list[li].value--
      firebase.database().ref('List/' + li).update({
        value: this.list[li].value
      })
    }
  },
  created() {
    firebase.database().ref('List').on('value', snapshot => {
      this.list = snapshot.val()
    })
  }
}
</script>