<template>
  <div class="p-4 flex flex-col">
    <input
        v-model="question"
        placeholder="Type the question here"
        class="border border-black p-2 mb-4"
    />
    <div class="flex flex-col gap-2 mb-3">
      <div v-for="(answer, index) in answers" :key="index" class="flex gap-2 items-center">
        <input
            v-model="answer.text"
            @input="updateAnswer(index, $event.target.value)"
            class="border border-black p-2 flex-1"
        />
        <button class="bg-red-500 text-white py-2 px-4" @click="removeAnswer(answer.text)">Remove</button>
      </div>
      <!-- Input for new answer -->
      <div class="flex gap-2">
        <input
            v-model="answer"
            placeholder="Type the answer here"
            class="border border-black p-2"
        />
        <button class="bg-gray-400 text-white py-2 px-4" @click="handleAddAnswer">Add</button>
      </div>
    </div>
    <div class="flex justify-between items-center mt-auto">
      <span>{{ answers.length }}/10 possible answers</span>
      <button class="bg-blue-500 text-white p-2" @click="handleResetBtn">Reset</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import { useStore } from 'vuex'

const store = useStore();

// Computed properties to interact with the store
const question = computed({
  get: () => store.state.question,
  set: (value: string) => store.commit('setQuestion', value),
});

const answers = computed(() => store.state.answers);
const answer = ref(''); // Local ref to store the answer input

// Function to handle adding an answer
const handleAddAnswer = () => {
  store.commit('addAnswer', answer.value);
  answer.value = ''; // Clear input after adding
};

// Function to update an answer
const updateAnswer = (index: number, newText: string) => {
  store.commit('updateAnswer', { index, newText });
};

// Function to reset question and answers
const handleResetBtn = () => {
  store.commit('reset');
  answer.value = ''; // Clear input after reset
};

// Function to remove an answer
const removeAnswer = (index: number) => {
  store.commit('removeAnswer', index);
};

</script>
