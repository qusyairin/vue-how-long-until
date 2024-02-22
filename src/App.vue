<template>
  <Header />
  <div class="container">
    <Calendar @dateSubmitted="handleSubmit" />
  </div>

  <Teleport to="#modal">
    <div class="modal-bg" v-if="isModalOpen">
      <div class="modal">
        <button class="close-btn" @click="isModalOpen = false">x</button>
        <h1>{{ result }}</h1>
        <h2>{{ description }}</h2>
        <add-to-calendar-button 
          name="Test-Event" 
          :startDate="formattedDate"
          :options="['Google']"
        ></add-to-calendar-button>
      </div>
    </div>
  </Teleport>
</template>

<script setup>
import Header from "./components/Header.vue";
import Calendar from "./components/Calendar.vue";
import { ref } from "vue";
import AddToCalendarButton from 'add-to-calendar-button-vue';

const isModalOpen = ref(false);
const emoji = ref("🤓");
const result = ref("You have selected none!");
const formattedDate = ref();
const description = ref();

const handleSubmit = (rangeData) => {
  const startDate = new Date(rangeData.start);
  const endDate = new Date(rangeData.end);

  const difference = endDate.getTime() - startDate.getTime();

  const daysDifference = difference / (1000 * 3600 * 24);
  
  if (daysDifference == 0) {
    result.value = "You have selected today!";
    isModalOpen.value = true;
  } else {
    if (daysDifference < 5) {
      emoji.value = "🤩";
    } else if (daysDifference < 10 && daysDifference >= 5) {
      emoji.value = "🫡";
    } else if (daysDifference < 30 && daysDifference >= 10) {
      emoji.value = "🙂";
    } else {
      emoji.value = "😴";
    }
    formattedDate.value = formatDate(endDate, 'display')
    result.value = `${daysDifference} Days in between! ${emoji.value}`;
    description.value = `You selected ${formatDate(startDate)} to ${formatDate(endDate)}`
    isModalOpen.value = true;
  }
};

function formatDate(date, display) {
  if (!display){
    const yyyy = date.getFullYear();
    let mm = date.getMonth() + 1;
    let dd = date.getDate();

    if (dd < 10) dd = '0' + dd;
    if (mm < 10) mm = '0' + mm; 

    return dd + '/' + mm + '/' + yyyy;
  }else{
    const yyyy = date.getFullYear();
    let mm = date.getMonth() + 1; // Months start at 0!
    let dd = date.getDate();

    if (dd < 10) dd = '0' + dd;
    if (mm < 10) mm = '0' + mm; 

    return yyyy + '-' + mm + '-' + dd;
  }
}
</script>

<style>
.button {
  width: 90%;
  color: white;
  background-color: rgb(218, 80, 218);
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  border: 1px solid;
  cursor: pointer;
  font-size: 1rem;
}

.button:hover {
  background-color: rgb(153, 55, 153);
}

.modal-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: black;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 50;
}

.modal {
  position: relative;
  background: white;
  text-align: center;
  padding: 50px 100px;
  border-radius: 5px;
  box-shadow: 0px 10px 5px 2px rgba(0, 0, 0, 0.1);
  width: 80%;
}

.modal .close-btn {
  position: absolute;
  top: 10px;
  right: 10px;

  background: none;
  border: none;
  cursor: pointer;
}
</style>
