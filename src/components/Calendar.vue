<template>
  <VDatePicker
    expanded
    v-model.range.number="range"
    :color="selectedColor"
    :is-dark="true"
    :attributes="attrs"
    :min-date="today"
  >
    <template #footer>
      <div class="footer">
        <button type="button" class="btn" @click="handleClick">
          How Long 🤔
        </button>
      </div>
    </template>
  </VDatePicker>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["dateSubmitted"]);

const today = new Date();

const range = ref({ start: today.getTime(), end: today.getTime() });
const selectedColor = ref("purple");
const attrs = ref([
  {
    key: "test",
    highlight: true,
    // dates: { start: today, end: today },
  },
]);

const handleClick = () => {
  const rangeData = range.value;
  emit('dateSubmitted', rangeData)
};
</script>

<style>
.footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding-bottom: 2.5rem;
}
.vc-container button {
  background-color: transparent !important;
  font-family: inherit;
}
.vc-nav-container button {
  font-family: inherit;
}

.vc-nav-container button:not(.is-active) {
  background-color: transparent !important;
}
</style>
