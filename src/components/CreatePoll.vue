<template>
  <div class="flex flex-col">
    <!-- Input for the question -->
    <input
      v-model.trim="question"
      placeholder="Type the question here"
      :maxlength="INPUT_LIMIT"
      class="border border-black p-2 mb-4"
    />
    <div class="flex flex-col gap-2 mb-3">
      <!-- Previous answers -->
      <div v-for="(answer, index) in answers" :key="index" class="flex gap-2 items-center">
        <input
          v-model.trim="answer.text"
          class="border border-black p-2 flex-1"
          :maxlength="INPUT_LIMIT"
          data-testid="answer-input"
        />
        <button class="bg-red-500 text-white py-2 px-4" @click="removeAnswer(answer.text)">
          Remove
        </button>
      </div>
      <!-- Input for new answer -->
      <div class="flex gap-2" v-if="answers.length < 10">
        <input
          v-model.trim="answer"
          placeholder="Type the answer here"
          class="border border-black p-2"
          :maxlength="INPUT_LIMIT"
        />
        <button class="bg-gray-400 text-white py-2 px-4" @click="handleAddAnswer">Add</button>
      </div>
    </div>
    <!-- Button to reset question and answers -->
    <div class="flex justify-between items-center mt-auto">
      <span>{{ answers.length }}/10 possible answers</span>
      <button class="bg-blue-500 text-white p-2" @click="handleResetBtn">Reset</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import { useStore } from 'vuex'
import {INPUT_LIMIT} from "@/constants";

const store = useStore()

// Computed properties to interact with the store
const question = computed({
  get: () => store.state.question,
  set: (value: string) => store.commit('setQuestion', value)
})

const answers = computed(() => store.state.answers)
const answer = ref('') // Local ref to store the answer input

/**
 * Add the answer to the store
 */
const handleAddAnswer = () => {
  if (!answer.value) return // Do not add empty answers

  store.commit('addAnswer', answer.value)
  answer.value = '' // Clear input after adding
}

/**
 * Reset the question and answers in the store
 */
const handleResetBtn = () => {
  store.commit('reset')
  answer.value = '' // Clear input after reset
}

/**
 * Remove an answer from the store
 * @param index - Index of the answer to remove
 */
const removeAnswer = (index: number) => {
  store.commit('removeAnswer', index)
}
</script>