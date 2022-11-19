<template>
  <div class="category-list">
    <ul>
      <li v-for="category in categoriesJson" :key="category.id" :class="{ active: category.id === selectedIndex }" @click="clickHandler">
        {{ category.name }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType, toRefs } from 'vue'

interface CategoryJson {
  id: number,
  name: string
}

export default defineComponent({
  name: 'CategoryList',
  props: {
    selectedIndex: Number,
    categoriesJson: Object as PropType<CategoryJson[]>
  },
  setup () {
    function clickHandler (event: Event) {
      const categoryName: string = (event.target as HTMLUListElement).textContent ?? 'Unknown Button'
      console.log('"' + categoryName + '" category button clicked!')
    }

    return {
      clickHandler
    }
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/scss/categoryList"
</style>
