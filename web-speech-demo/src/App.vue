<template>
  <header>
    <div class="wrapper">
      <h1>Web Speech API Demo</h1>
      <button @click="record">
        {{ isRecording ? "End Recording" : "Start Recording" }}
      </button>
      <ul v-for="(item, index) in recordedWords" :key="index">
        <li>
          {{ item }}
        </li>
      </ul>
    </div>
  </header>
</template>

<script setup>
import { ref } from "vue";
const SpeechRecognition =
  window.SpeechRecognition || window.webkitSpeechRecognition;
const recognition = SpeechRecognition ? new SpeechRecognition() : false;

const isRecording = ref(false);
const recordedWords = ref([]);
recognition.continuous = true;
recognition.lang = "fr-CAN";

function record() {
  if (!isRecording.value) {
    recognition.start();
    isRecording.value = true;
    console.log("recording started");
  } else {
    recognition.stop();
    isRecording.value = false;
    console.log("recording stopped");
  }

  recognition.onresult = function (event) {
    console.log(event.results);
    recordedWords.value.push(
      event.results[event.results.length - 1][0].transcript
    );
    console.log(
      "Confidence: ",
      event.results[event.results.length - 1][0].confidence
    );
  };
}
</script>

<style scoped></style>
