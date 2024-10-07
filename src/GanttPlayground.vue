<script lang="ts" setup>
import { computed, ref } from "vue"
import dayjs from "dayjs"
import customParseFormat from "dayjs/plugin/customParseFormat"
import type { GanttBarObject } from "./types"
import weekday from "dayjs/plugin/weekday"
import GGanttChart from "./components/GGanttChart.vue"

dayjs.extend(weekday)
dayjs.extend(customParseFormat)

const chartStart = "2024-10-01 00:00"
const chartEnd = "2027-07-01 00:00"
const grid = ref(true)
const hideTimeAxis = ref(false)
const currentTime = ref(true)
const dark = ref(true)

const generateRows = (rowCount: number) => {
  const rows: { title: string; bars: GanttBarObject[] }[] = []
  const maxDate = dayjs("2027-06-14T00:00:00")

  for (let i = 0; i < rowCount; i++) {
    const bars: GanttBarObject[] = []
    let previousEndDate = dayjs()

    while (previousEndDate < maxDate) {
      const beginDateTime = previousEndDate.add(Math.floor(Math.random() * 3), "day").startOf("day")
      const endDateTime = beginDateTime.add(Math.floor(Math.random() * 9) + 2, "day").startOf("day")
      if (endDateTime > maxDate) break

      bars.push({
        beginDate: beginDateTime.format("YYYY-MM-DD HH:mm"),
        endDate: endDateTime.format("YYYY-MM-DD HH:mm"),
        ganttBarConfig: {
          id: `${i}-${bars.length}-${Date.now()}`,
          hasHandles: true,
          label: "Maximilian Fixel",
          style: {
            background: "#" + Math.floor(Math.random() * 16777215).toString(16),
            borderRadius: "20px",
            color: "black"
          }
        }
      })

      previousEndDate = endDateTime
    }

    rows.push({
      title: `App. ${i + 1}`,
      bars
    })
  }

  return rows
}

const ganttRows = computed(() => generateRows(10))

</script>

<template>
  <div class="wrapper">
    <g-gantt-chart
      :chart-start="chartStart"
      :chart-end="chartEnd"
      precision="day"
      :row-height="40"
      grid
      height="280px"
      bar-start="beginDate"
      bar-end="endDate"
      label-column-sticky
      timeaxis-sticky
      :color-scheme="dark ? 'dark' : 'default'"
      :current-time="currentTime"
    >
      <template #timeunit="p">
        <div style="padding: 15px">
          {{ new Date(p.value).toLocaleDateString("en-US", { day: "2-digit" }) }}
        </div>
      </template>
      <g-gantt-row
        v-for="(row, rowIndex) in ganttRows"
        :key="`row-${rowIndex}`"
        :bars="row.bars"
        :label="row.title"
        highlight-on-hover
      />
    </g-gantt-chart>
  </div>
</template>

<style>
.wrapper {
  height: 100%;
  width: 100%;
  overflow-x: auto;
}

.g-gantt-chart {
  height: fit-content;
  width: max-content !important;
  min-width: 100%;
}
</style>
