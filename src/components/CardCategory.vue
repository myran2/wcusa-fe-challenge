<template>
  <div class="card-category" v-if="biometrics.length > 0">
    <div class="category-header">
        <h2 class="category-name">{{categoryName}}</h2>
        <span class="circle bad">{{badBioCount}}</span>
        <span class="circle meh">{{mehBioCount}}</span>
        <span class="circle good">{{goodBioCount}}</span>
    </div>
    <BiometricCard v-for="card in biometrics" :key=card.id :bioJson="card"></BiometricCard>
    <div class="category-footer">
        <a class="btn load-more" @click="loadMoreClickHandler">Load More</a>
        <div class="seperator"></div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import BiometricCard from '@/components/BiometricCard.vue'
import biometricsJson from '@/assets/biometrics.json'

export default defineComponent({
  name: 'CardCategory',
  props: {
    categoryId: Number,
    categoryName: String
  },
  components: {
    BiometricCard
  },
  setup (props) {
    let badBioCount = 0
    let mehBioCount = 0
    let goodBioCount = 0

    // only include Biometric cards that match this Category's id.
    const biometrics = biometricsJson.filter(function (biometric) {
      return biometric.categoryId === props.categoryId
    })

    // Count up all the included Bio card types for the dot display in the Category Header.
    biometrics.forEach(bio => {
      if (bio.intensity === 'Good') {
        goodBioCount += 1
      } else if (bio.intensity === 'Stuff To Watch') {
        mehBioCount += 1
      } else {
        badBioCount += 1
      }
    })

    // If this page was reactive, this button would show more Biometric Cards from this category.
    function loadMoreClickHandler (event: Event) {
      event.preventDefault()
      console.log(props.categoryName + ' "Load More" button clicked!')
    }

    return {
      biometrics,
      badBioCount,
      mehBioCount,
      goodBioCount,
      loadMoreClickHandler
    }
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/scss/cardCategory"
</style>
