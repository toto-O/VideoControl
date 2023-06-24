<template>
  <!-- <HelloWorld /> -->

  <video
    src="@/assets/recording.webm"
    height="400"
    controls
    @loadedmetadata="check_full_playback_time($event)"
  />
</template>

<script lang="ts" setup>
// import { ref } from "vue";

interface Props {
  frame_rate: number;
}
interface Emits {
  (e: "set_play_range", play_range: number[]): void;
  (e: "set_maximum_play_time", maximum_play_time: number): void;
}
const props = defineProps<Props>();
const emits = defineEmits<Emits>();

function check_full_playback_time(params: any) {
  params.target.currentTime = 60000;
  params.target.onseeked = () => {
    params.target.onseeked = undefined;
    params.target.currentTime = 0;
    return check_element(params);
  };
}
function check_element(params: any) {
  console.log(params);
  emits("set_maximum_play_time", params.target.duration);
  // * props.frame_rate
  emits("set_play_range", [0, params.target.duration]);
}
</script>
