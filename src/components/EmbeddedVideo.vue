<template>
  <!-- <HelloWorld /> -->

  <video
    src="@/assets/recording.webm"
    height="400"
    controls
    @loadedmetadata="check_full_playback_time($event)"
    @timeupdate="set_restart_position($event)"
  />
</template>

<script lang="ts" setup>
// import { ref } from "vue";

interface Props {
  playback_range: number[];
  // frame_rate: number;
}
interface Emits {
  (e: "set_playback_range", playback_range: number[]): void;
  (e: "set_maximum_play_time", maximum_play_time: number): void;
}
const props = defineProps<Props>();
const emits = defineEmits<Emits>();

function set_restart_position(params: any) {
  if (params.target.currentTime > props.playback_range[1]) {
    params.target.pause();
    params.target.onplay = () => {
      if (params.target.currentTime > props.playback_range[1]) {
        params.target.currentTime = props.playback_range[0];
      }
    };
  }
}

function check_full_playback_time(params: any) {
  if (params.target.duration !== Infinity) return check_element(params);
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
  emits("set_playback_range", [0, params.target.duration]);
}
</script>
