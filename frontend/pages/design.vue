<template>
  <NuxtLayout name="default">
    <q-page>
      <div class="padding-16">
        <h2>My Surveys</h2>
        <div class="custom-sub-container">
          <q-list bordered class="rounded-borders custom-width-60-pc" style="max-width: 800px">
            <!--          <q-item-label header>Google Inbox style</q-item-label>-->
            <list-item-survey-design v-for="survey in surveys" :survey_object="survey"> </list-item-survey-design>
            <!--          <q-separator/>-->
          </q-list>
          <q-input v-model="textName" label="Name" />
          <q-input v-model="textDescription" label="Description" />
          <q-btn color="white" text-color="black" label="Add survey" @click="addNewSurvey" />
        </div>
      </div>
    </q-page>
  </NuxtLayout>
</template>

<script setup>
import { ref } from 'vue'
import BaseButton from "../components/BaseButton";
import ListItemSurveyDesign from "../components/ListItemSurveyDesign";
import { formatDate } from "~/utils/formatData"
import { useSurveyStore } from "~/stores/survey"

// Make sure the user is authenticated or trigger the reroute to login
definePageMeta({ middleware: 'authorization' })

/**
 * All `/api/**` are proxies pointing to the local or production server of the backend.
 */

const url = "/api/surveys/"
const surveyStore = useSurveyStore()
const { data: surveys } = await useAsyncData(() => $cmsApi(url));
var expire_date = new Date();
var current_date = new Date();
const textName = ref(null)
const textDescription = ref(null)

// set default expire date 100 days after current day
expire_date.setDate(expire_date.getDate() + 100);
current_date.setDate(current_date.getDate());

// Add a new survey using the surveyStore, based on what is entered in the field.
const addNewSurvey = async () => {
  await surveyStore.createSurvey(textName.value, textDescription.value, current_date, expire_date)
}
</script>

<style lang="scss" scoped>
.custom-bg-white {
  background-color: #fff;
}

.custom-list-item {
  //min-height: 50vh;
  height: calc(100vh/3);
  background: #fff;
  overflow: hidden;
  overflow-y: scroll;
}

::-webkit-scrollbar {
  width: 0;
  background: transparent;
  /* make scrollbar transparent */
}

.custom-scroll {
  position: relative;
  max-height: 100%;
  overflow-y: scroll;
}

::-webkit-scrollbar {
  width: 0;
  background: transparent;
  /* make scrollbar transparent */
}

.custom-bg-red {
  min-width: 100%;
  min-height: 100%;
  background: #b02a37;
}

.custom-sub-container {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: flex-start;
}

.custom-width-60-pc {
  width: 60%;
}

.padding-16 {
  padding: 16px;
}
</style>
