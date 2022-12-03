<template>
  <div class="min-h-screen flex flex-col justify-center space-y-4">
    <h1 class="text-center text-4xl font-bold flex-shrink-0">Haiku Bot</h1>
    <div class="flex space-x-2 w-full max-w-xl mx-auto flex-shrink-0">
      <ul class="text-xl py-2 border border-indigo-200/0">
        <li v-for="(count, i) of lineCount" :key="i">{{ count }}</li>
      </ul>
      <textarea
        class="flex-grow text-xl border rounded py-2 px-2 overflow-hidden"
        v-model="input"
      />
    </div>
    <div class="text-center py-4">
      I {{ isValid ? "am" : "am not" }} a haiku.
    </div>
    <div class="text-center">
      <button type="button" class="p-2 border rounded" @click="copyInput()">
        Copy to Clipboard <span v-if="copied" v-html="icons.VALID" />
      </button>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { syllable } from "syllable";
function compareArrays(a: any[], b: any[]): boolean {
  if (a.length !== b.length) {
    return false;
  }
  return typeof a.find((v, i) => b[i] !== v) === "undefined";
}
const icons = ref({
  VALID: "&#x2714;",
  INVALID: "&#x2718;",
});

const copied = ref<boolean>(false);

const input = ref<string>("");
const lineCount = computed(() =>
  input.value.split("\n").map((line) => syllable(line))
);
const isValid = computed<boolean>(() => {
  return (
    compareArrays(lineCount.value, [5, 7, 5]) ||
    compareArrays(lineCount.value, [1, 3, 1])
  );
});
function copyInput() {
  try {
    navigator.clipboard.writeText(input.value);
    copied.value = true;
  } catch (err) {
    console.error(err);
  }
}
</script>
