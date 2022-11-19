<template>
  <div class="home">
    <h1>Your most important Risks</h1>
    <CategoryList :selectedIndex=1 :categoriesJson="CardCategoriesJson"/>
    <CardCategory v-for="category in CardCategoriesJson" :key="category.id" :categoryId=category.id :categoryName=category.name />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import CategoryList from '@/components/CategoryList.vue'
import CardCategory from '@/components/CardCategory.vue'
import CardCategoriesJson from '@/assets/categories.json'

export default defineComponent({
  name: 'HomeView',
  components: {
    CategoryList,
    CardCategory
  },
  setup () {
    return {
      CardCategoriesJson
    }
  }
})
</script>
