<template>
  <div class="biometric-card" :class="{critical: bio.addlWarning === 'Critical'}">
    <div class="card-header">
        <span :class="bio.getRecommendedValueClassName()">{{bio.addlWarning}}</span>
    </div>
    <div class="card-body">
        <div v-if="bio.value" class="metric-values">
            <span class="value">{{bio?.value}}</span>
            <span class="unit">{{bio?.unit}}</span>
        </div>
        <div class="metric-analysis">
            <span class="metric-name">{{bio.name}}</span>
            <p class="metric-description">
                <span v-if="bio.recommendedLevelDiff" :class="bio.getRecommendedValueClassName()">{{bio.recommendedLevelDiff}}</span>
                {{bio.description}}
            </p>
        </div>
    </div>
    <div class="card-actions">
        <span class="time-display">{{bio.timeDisplay}}</span>
        <a class="btn action-button" @click=actionButtonClickHandler>{{bio.actionLabel}}</a>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'

interface BiometricJson {
    id: number;
    name: string;
    description: string;
    value?: number;
    unit?: string;
    timeDisplay: string;
    actionLabel: string;
    categoryId: number;
    addlWarning: string;
}

class Biometric {
    // these fields come straight from the original JSON.
    id: number;
    name: string;
    description: string;
    value?: number;
    unit?: string;
    timeDisplay: string;
    actionLabel: string;
    categoryId: number;

    // I added this field to implement the top-right warning label
    addlWarning: string;

    // this fields is inferred from other JSON values.
    recommendedLevelDiff?: number;

    constructor (json: BiometricJson) {
      this.id = json.id
      this.name = json.name
      this.description = json.description
      this.value = json.value
      this.unit = json.unit
      this.timeDisplay = json.timeDisplay
      this.actionLabel = json.actionLabel
      this.categoryId = json.categoryId

      /* Have to do some wacky stuff here because the JSON format in the example doesn't have everything we need.
         In this case, we need to determine if we should change the color of the number in the description.
         Realistically, this would just be an additional JSON field.
      */
      this.addlWarning = json.addlWarning
      // find the first number (float or otherwise) in the description.
      const numberMatch:RegExpMatchArray|null = this.description.match('[0-9.]+ ')
      if (numberMatch) {
        this.recommendedLevelDiff = parseFloat(numberMatch[0])
        this.description = this.description.replace(numberMatch[0], '')
      }
    }

    public toString = () : string => {
      return `"${this.name}" (ID: ${this.id})`
    }

    public getRecommendedValueClassName = () : string => {
      return this.addlWarning.toLocaleLowerCase().replaceAll(' ', '-')
    }
}

export default defineComponent({
  name: 'BiometricCard',
  props: {
    bioJson: Object as PropType<BiometricJson>
  },
  setup (props) {
    // Given the scope of this project, we can be *really* sure that bioJson will never be null.
    // eslint-disable-next-line  @typescript-eslint/no-non-null-assertion
    const bio:Biometric = new Biometric(props.bioJson!)
    function actionButtonClickHandler (event: Event) {
      event.preventDefault()
      console.log(bio + ' ' + bio.actionLabel + ' button clicked!')
    }
    return {
      bio,
      actionButtonClickHandler
    }
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "@/scss/biometricCard"
</style>
