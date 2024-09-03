<!--TODO
created: 06/27/2024 author of todo: brandon
updated: 09/03/2024
1. create a sign-in form for members to digitally sign in
-->

<template>
  <div class="flex justify-center items-center min-h-fit">
    <div class="bg-calendar p-6 rounded-2xl w-full max-w-screen-lg">
      <div class="flex justify-between items-left mb-4">
        <div>
          <span class="text-4xl font-arimo font-bold text-black">{{
            months[currentMonth]
          }}</span>
          <span class="ml-2 text-4xl font-thin text-black">{{
            currentYear
          }}</span>
        </div>
        <div>
          <button @click="prevMonth">
            <svg
              class="h-6 w-6 text-white"
              fill="white"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <polygon
                points="8,0 24,12 8,24"
                transform="scale(-1, 1) translate(-24, 0)"
              />
            </svg>
          </button>
          <button @click="nextMonth">
            <svg
              class="h-6 w-6 text-white"
              fill="white"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <polygon points="8,0 24,12 8,24" />
            </svg>
          </button>
        </div>
      </div>
      <div class="grid grid-cols-7 gap-0.5 rounded-xs text-overflow">
        <!-- day headers -->
        <div
          v-for="dayName in days"
          :key="dayName"
          class="text-left text-[#111111] font-arimo px-1"
        >
          {{ dayName }}
        </div>

        <!-- previous month's days -->
        <div
          v-for="(pmDay, index) in previousMonthDays"
          :key="'prev-' + index"
          class="relative text-center rounded-lg py-2 focus:outline-none border-r border-b border-gray-300 h-24 bg-prev-next-month text-[#b3b2b2]"
        >
        <div class="justify-between">
          <div class="absolute top-0 left-0 ml-1 mt-1 mb-max text-sm rounded-lg p-1">
            {{ pmDay.date() }}
          </div>

          <!-- display events for previous month's days -->
          <div
            v-if="getEventsForDate(pmDay).length"
            class="absolute inset-x-0 bottom-0 ml-7 mb-1 mr-3"
          >
            <a
              v-for="event in getEventsForDate(pmDay)"
              :key="event.title"
              :href="'#event-' + event.title.replace(/\s+/g, '-').toLowerCase()"
              :class="{ 'line-through': isPastEvent(event.datetime) }"
              class="text-xs ellipsis-multiline"
            >
              {{ event.title }}
              <br >
              {{ event.datetime.format('h:mm A') }}
              <br >
            </a>
          </div>
        </div>
        </div>

        <!-- current month's days -->
        <!-- curreently !isCurrentMonth(date) is redundent since isToday(date) compares all daymonthyear-->
        <div
          v-for="date in daysInMonth"
          :key="date.format('YYYY-MM-DD')"
          class="relative bg-current-month rounded-lg text-[#333333] text-center py-2 focus:outline-none border-r border-b h-24 overflow-hidden"
        >
          <div
            :class="{
              'bg-red-500 bg-opacity-100 font-arimo rounded-full text-white':
                isToday(date),
            }"
            class="absolute top-0 left-0 ml-1 mt-1 text-sm p-1"
          >
            {{ date.date() }}
          </div>
          <div
            v-if="getEventsForDate(date).length"
            class="absolute inset-x-0 bottom-0 ml-7 mb-1 mr-3"
          >
            <!-- could be anchor tag link to scroll down page for event description -->

            <!-- might need to add event id for primary key to match schemaa -->
            <a
              v-for="event in getEventsForDate(date)"
              :key="event.title"
              :href="'#event-' + event.title.replace(/\s+/g, '-').toLowerCase()"
              :class="{ 'line-through': isPastEvent(event.datetime) }"
              class="font-arimo text-xs text-[#333333] ellipsis-multiline"
            >
              {{ event.title }}
              <br >
              {{ event.datetime.format('h:mm A') }}
              <br >
            </a>
          </div>
        </div>

        <!-- next month's days -->
        <div
          v-for="(nmDay, index) in nextMonthDays"
          :key="'next-' + index"
          class="relative text-center rounded-lg py-2 focus:outline-none border-r border-b border-gray-300 h-24 bg-prev-next-month"
        >
          <div class="absolute top-0 left-0 ml-1 mt-1 text-sm text-[#b3b2b2]">
            {{ nmDay.date() }}
          </div>

          <!-- display events for next month's days -->
          <div
            v-if="getEventsForDate(nmDay).length"
            class="absolute inset-x-0 bottom-0 ml-7 mb-1 mr-3"
          >
            <a
              v-for="event in getEventsForDate(nmDay)"
              :key="event.title"
              :href="'#event-' + event.title.replace(/\s+/g, '-').toLowerCase()"
              :class="{ 'line-through': isPastEvent(event.datetime) }"
              class="text-xs flexbox text-[#b3b2b2] ellipsis-multiline"
            >
              {{ event.title }}
              <br >
              {{ event.datetime.format('h:mm A') }}
              <br >
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- Event Detail Sections -->
  <br >
  <br >
  <!-- display future events above -->
  <div v-for="event in events" :key="event.title">
    <div
      v-if="!isPastEvent(event.datetime)"
      class="p-4 bg-base text-white border border-gray-300 rounded-md mb-2"
    >
      <h2 class="text-lg font-arimo font-bold">{{ event.title }}</h2>
      <p>Date: {{ event.datetime.format('dddd, MMMM D, YYYY') }}</p>
      <p>Time: {{ event.datetime.format('h:mm A') }}</p>
      <p>{{ event.description }}</p>
    </div>
  </div>

  <!-- toggle button for past events -->
  <button
    class="w-full text-left p-4 bg-lighter-base text-white border border-gray-300 rounded-md mb-2"
    @click="togglePastEvents"
  >
    <h2 class="text-lg font-arimo font-bold">Past Events</h2>
    <p v-if="!showPastEvents">Show Events</p>
    <p v-else>Hide Events</p>
  </button>

  <!-- display past events -->
  <div v-if="showPastEvents">
    <div v-for="event in events" :key="event.title">
      <div
        v-if="isPastEvent(event.datetime)"
        class="p-4 bg-lighter-b text-white line-through border border-gray-300 rounded-md mb-2"
      >
        <h2 class="text-lg font-arimo font-bold">{{ event.title }}</h2>
        <p>Date: {{ event.datetime.format('dddd, MMMM D, YYYY') }}</p>
        <p>Time: {{ event.time }}</p>
        <p>{{ event.description }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import dayjs from 'dayjs';

// reactive state for current date
const today = ref(dayjs().startOf('day'));
const currentMonth = ref(dayjs().month());
const currentYear = ref(dayjs().year());

// static data so no ref() here
const months = [
  'January',
  'February',
  'March',
  'April',
  'May',
  'June',
  'July',
  'August',
  'September',
  'October',
  'November',
  'December',
];
const days = ['Sun', 'Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat'];

const events = ref([
  {
    title: '1st General Meeting',
    datetime: dayjs('2024-08-22T20:00:00'),
    description: 'PLACEHOLDER',
  },
  {
    title: 'a duuuu',
    datetime: dayjs('2024-08-13T20:00:00'),
    description: 'PLACEHOLDER',
  },
  {
    title: 'Custom ARAM with Officers AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFGRHUIEJAHTGRUIESHGUIRHESIL;GREUIGSHGRLUIEGUIEHTG UIERH',
    datetime: dayjs('2024-07-30T21:00:00'),
    description: 'PLACEHOLDER',
  },

  {
    title: 'black myth wukong no hit run',
    datetime: dayjs('2024-08-25T19:00:00'),
    description: 'PLACEHOLDER',
  },
  {
    title: 'obamna',
    datetime: dayjs('2024-06-30T21:00:00'),
    description: 'PLACEHOLDER',
  },
  {
    title: 'demure',
    datetime: dayjs('2024-08-01T21:00:00'),
    description: 'PLACEHOLDER',
  },
]);

// method to check if an event is past today
const isPastEvent = (eventDate) => {
  return dayjs(eventDate).isBefore(today.value, 'day');
};

// reactive state to toggle past events section
const showPastEvents = ref(false);
const togglePastEvents = () => {
  showPastEvents.value = !showPastEvents.value;
};

// logic to help determine what days from prev or next months need to be shown on current month calendar
const daysInMonth = computed(() => { // gets current month days and populates array 
  const startOfMonth = dayjs()
    .year(currentYear.value)
    .month(currentMonth.value)
    .startOf('month');
  const endOfMonth = dayjs()
    .year(currentYear.value)
    .month(currentMonth.value)
    .endOf('month');

  const daysArray = [];

  for (let i = startOfMonth.date(); i <= endOfMonth.date(); i++) {
    daysArray.push(startOfMonth.date(i));
  }

  return daysArray;
});

// gets previous and next month from displayed. so that prev/next buttons works for any month
const prevMonth = () => {
  if (currentMonth.value === 0) {
    currentMonth.value = 11;
    currentYear.value--;
  } else {
    currentMonth.value--;
  }
};

const nextMonth = () => {
  if (currentMonth.value === 11) {
    currentMonth.value = 0;
    currentYear.value++;
  } else {
    currentMonth.value++;
  }
};

// based on current month, get any days that should be displayed. e.g. july 30th 2024 and sept 1st 2024 should both be displayed on august 2024
const previousMonthDays = computed(() => {
  const startOfMonth = dayjs()
    .year(currentYear.value)
    .month(currentMonth.value)
    .startOf('month');
  const previousMonth = startOfMonth.subtract(1, 'month');
  const daysInPreviousMonth = previousMonth.daysInMonth();

  const leadingDays = startOfMonth.day(); // days in prev month on current month calendar
  const previousMonthDaysArray = [];

  for (
    let i = daysInPreviousMonth - leadingDays + 1;
    i <= daysInPreviousMonth;
    i++
  ) {
    previousMonthDaysArray.push(previousMonth.date(i));
  }

  return previousMonthDaysArray;
});

const nextMonthDays = computed(() => {
  const endOfMonth = dayjs()
    .year(currentYear.value)
    .month(currentMonth.value)
    .endOf('month');
  const nextMonth = endOfMonth.add(1, 'month');
  const trailingDays = 6 - endOfMonth.day(); // days in next month on current month calendar
  const nextMonthDaysArray = [];

  for (let i = 1; i <= trailingDays; i++) {
    nextMonthDaysArray.push(nextMonth.date(i));
  }

  return nextMonthDaysArray;
});

// function for red circle highlight on current day
const isToday = (date) => date.isSame(dayjs(), 'day');

// get events for days displayed on calendar
const getEventsForDate = (date) => {
  return events.value.filter((event) =>
    dayjs(event.datetime).isSame(date, 'day')
  );
};
</script>


// unfortunate custom css tailwind doesn't work?
/* Custom CSS for multiline text truncation with ellipsis */
<style scoped>
.ellipsis-multiline {
  display: -webkit-box;
  text-align: start;
  line-clamp: 5; /* Adjust this number based on how many lines you want to show */
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
