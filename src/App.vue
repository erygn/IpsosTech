<template>
  <div class="min-h-screen w-full text-center" style="background-image: url('https://controller.genesis-mc.fr/uploads/5172658.jpg'); background-position: center; background-size: cover">
    <nav class="bg-white z-50 shadow-2xl flex px-4 md:px-32 py-2 fixed top-0 items-center w-full justify-between">
      <span class="font-bold">IpsosTech</span>
      <button class="px-4 py-2 text-sm text-white bg-blue-900 rounded-full" @click="scrollTo('voteSection')">Let's vote!</button>
    </nav>

    <div class="px-4 pt-48">
      <h1 class="text-center text-white text-6xl font-bold mb-4">IpsosTech</h1>
      <p class="mb-24 text-white">Pour suivre vos listes favorites</p>

      <div id="voteSection" class="w-full flex justify-center">
        <div class="max-w-6xl w-full md:mx-32 md:grid md:grid-cols-3 md:gap-4">
          <div class="py-2 md:px-2 skew-y-3" v-for="(item, index) in list" :key="index">
            <div class="flex items-center md:block p-4 md:p-8 rounded-xl drop-shadow-2xl" :style="'color: '+ item.colorText +'; background-color: ' + item.color">
              <div class="aspect-square cursor-pointer w-32 md:w-full rounded-full mb-2" :style="'background-image: url(\'' + item.img + '\'); background-position: center; background-size: contain'"></div>
              <!-- <h2 class="font-bold h-12 mb-2 md:text-4xl">{{ item.title }}</h2> -->

              <div class="flex-1 px-4 md:px-0">
                <button @click="up(index)" class="w-full font-bold text-xl rounded bg-white p-1 pb-2" :style="'color: ' + item.color">+</button>
                <div class="font-bold p-1 text-xl py-2">{{ item.value }}</div>
                <button @click="down(index)" class="w-full font-bold text-xl rounded bg-white p-1 pb-2" :style="'color: ' + item.color">-</button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="w-full mt-16 flex justify-center">
        {{ barHeightList }}
        <div class="max-w-6xl w-full md:mx-32 grid grid-cols-3 md:gap-4">
          <div class="py-2 md:px-2" v-for="(item, index) in list" :key="index">
            <div class="p-4 md:p-8 rounded-xl drop-shadow-2xl" :style="'color: '+ item.colorText">
              <div style="height: 300px" class="flex">toto</div>
              <h4>{{ item.title }}</h4>
            </div>
          </div>
        </div>
      </div>
      
      <div class="mt-4 text-white mt-16 pb-16">
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
      barHeightList: [],
    }
  },
  methods: {
    barHeight() {
      let res = {}
      Object.keys(this.list || {}).forEach(item => {
        res[item].value = this.list[item].value
        this.barHeightList.push({value: this.list[item].value, id: item})
      })
      if (this.barHeightList.length > 0) {
        this.barHeightList = this.barHeightList.sort((a,b) => {
          return b.value - a.value
        })
      }
      return sortList
    },
    scrollTo: function(n) {
      var doc = document.querySelector('#' + n);
      if (doc != null) {
        doc.scrollIntoView({behavior: 'smooth'});
        // location.hash = '#' + n;
      }
    },
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
      this.barHeight()
    })
  }
}
</script>