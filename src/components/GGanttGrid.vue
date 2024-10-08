<template>
  <div class="g-grid-container">
    <div
      v-for="{ label, value, width } in filteredLowerUnits"
      :key="label"
      class="g-grid-line"
      :style="{
        width,
        background: highlightedUnits?.includes(Number(value)) ? colors.hoverHighlight : undefined
      }"
    ></div>
  </div>
</template>

<script setup lang="ts">
import provideConfig from "../provider/provideConfig.js"
import useTimeaxisUnits from "../composables/useTimeaxisUnits.js"
import { computed } from "vue"

defineProps<{
  highlightedUnits?: number[]
}>()

const { colors } = provideConfig()
const { timeaxisUnits } = useTimeaxisUnits()

const filteredLowerUnits = computed(() =>
  timeaxisUnits.value.lowerUnits.filter(({ width }) => width !== "0%")
)
</script>

<style>
.g-grid-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: space-between;
}

.g-grid-line {
  width: 1px;
  height: 100%;
  border-left: 1px solid #eaeaea;
}
</style>
