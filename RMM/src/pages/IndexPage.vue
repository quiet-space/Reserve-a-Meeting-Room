<template>
  <q-page class="flex flex-center">
    <q-card>
      <q-card-section>
        <q-date v-model="current"
                :events="existDate"
                :locale="locale"

                @update:model-value="getSelectedDate(current)"
                mask="YYYY-MM-DD"
                style="width: 50vw; height: 60vh;"
                event-color="green-4"
                minimal landscape
        >
          <template #default>
            <div class="rounded-borders bg-grey-2 q-pa-md">
              <div>
                <div class="row items-center text-weight-bold text-green-7 text-subtitle1 q-mb-md">
                  <q-icon name="event_available" size="sm" class="q-mr-sm"/>
                  <q-input v-model="current" />
                </div>

                <q-range v-model="myMeet"
                         :min="8" :max="19"

                         @update:model-value="updateEvents"

                         snap marker-labels
                         color="green-3"
                         class="q-mt-md q-px-sm"
                >
                  <template v-slot:marker-label-group="scope">
                    <div
                      v-for="marker in scope.markerList"
                      :key="marker.index"
                      :class="[ existMarkerClass(marker), marker.classes ]"
                      :style="marker.style"
                    >
                      {{ marker.value }}
                    </div>
                  </template>
                </q-range>

              </div>
            </div>
          </template>
        </q-date>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
import {ref} from 'vue'
import {date} from 'quasar'


const current = new Date()
const locale = ref({months: ['1월', '2월', '3월', '4월', '5월', '6월', '7월', '8월', '9월', '10월', '11월', '12월']})

const eventClass = (props) => {
  return {
    date: props?.date,
    range: props?.range,
    title: props?.title,
  }
}

const events = ref([
  eventClass({
    date: '2022/10/14',
    range: {
      min: 9, max: 11
    },
    title: '면접'
  }),
  eventClass({
    date: '2022/10/20',
    range: {
      min: 12, max: 14
    },
    title: '면접'
  }),
  eventClass({
    date: '2022/10/24',
    range: {
      min: 17, max: 19
    },
    title: '면접'
  })])


const myMeet = ref({min: null, max: null})

const existDate = events.value.map(i => i.date)
const existMinRanges = events.value.map(i => i.range.min).sort((a, b) => a - b)
const existMaxRanges = events.value.map(i => i.range.max).sort((a, b) => a - b)

const getSelectedDate = (selected) => {
  current.value = selected
  console.log(current.value)
}

const updateEvents = () => {
  const {min, max} = myMeet.value

  const existMinNumber = existMinRanges[0]
  const existMaxNumber = existMaxRanges[existMaxRanges.length - 1]

  const existRange = (existMin, existMax) => {
    let range



    return range
  }


  for (let i = 0; i < events.value.length; i++) {
    if (min || max > existMinRanges[i] && min || max < existMaxRanges[i]) {
      // console.log('1')
    }
  }

}

const existMarkerClass = ({value}) => {

  if (existMinRanges.includes(value) || existMaxRanges.includes(value)) {
    return 'text-red'
  }

  for (let i = 0; i < events.value.length; i++) {
    if (value > existMinRanges[i] && value < existMaxRanges[i]) {
      return 'text-red'
    }
  }

}


</script>

<style>
* {
  font-family: 'Pretendard-Regular';
}


@font-face {
  font-family: 'Pretendard-Regular';
  src: url('https://cdn.jsdelivr.net/gh/Project-Noonnu/noonfonts_2107@1.1/Pretendard-Regular.woff') format('woff');
  font-weight: 400;
  font-style: normal;
}
</style>
