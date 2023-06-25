<template>
  <v-range-slider
    v-model="play_back_range"
    strict
    :min="0"
    :max="maximum_play_frames"
    :step="1 / frame_rate"
    @end="reset_playback_start_position($event)"
  />
  <!-- @update:modelValue="pass_current_playback_position($event)" -->
</template>

<script lang="ts" setup>
import { computed } from "vue";
interface Props {
  modelValue: number[];
  frame_rate: number;
  maximum_play_time: number;
}
interface Emits {
  (e: "update:modelValue", modelValue: number[]): void;
  (e: "seeked_playback_start_position", start_position: number): void;
}
const props = defineProps<Props>();
const emits = defineEmits<Emits>();

const maximum_play_frames = computed(() => {
  return props.maximum_play_time;
});

const play_back_range = computed<number[]>({
  get() {
    return props.modelValue;
    // * props.frame_rate
  },
  set(newVal) {
    emits("update:modelValue", newVal);
  },
});

function reset_playback_start_position(range_data: number[]) {
  emits("seeked_playback_start_position", range_data[0]);
}
</script>
