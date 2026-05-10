<script setup>
import { ref } from 'vue'

const count = ref(0)
const text = ref('')
// Define the boolean for your v-if
const awesome = ref(true) 

function increment() {
  count.value++
}

function onInput(e) {
  text.value = e.target.value
}

</script>

<template>
  <div class="p-8 space-y-4">
    <input :value="text" @input="onInput" placeholder="Type here" class="border p-2">
    <p>Input text: {{ text }}</p>

    <!-- The button now works with the increment function -->
    <button 
      @click="increment" 
      class="bg-blue-500 text-white px-4 py-2 rounded"
    >
      Count is: {{ count }}
    </button>

    <!-- Fixed spelling from 'awasome' to 'awesome' -->
    <h1 v-if="awesome" class="text-green-600">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>

    <h1 class="text-3xl font-bold underline">
      Hello world!
    </h1>
  </div>

  
</template>
