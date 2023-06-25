<template>
  <video
    ref="video"
    src="@/assets/recording.webm"
    height="400"
    controls
    @loadedmetadata="check_full_playback_time($event)"
    @timeupdate="set_restart_position($event)"
    @play="play_video($event)"
    @pause="pause_video($event)"
  />
</template>

<script lang="ts" setup>
import { ref, watch } from "vue";
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
  if (params.target.currentTime >= props.playback_range[1]) {
    params.target.pause();
    // 指定の再生終了地点まで到達したら、再生開始地点まで戻る
  }
}

function check_full_playback_time(params: any) {
  if (params.target.duration === Infinity) return pass_video_duration(params);
  else return set_playback_range(params);
}

function pass_video_duration(params: any) {
  pass_current_playback_position(60000);
  params.target.onseeked = () => {
    params.target.onseeked = undefined;
    pass_current_playback_position(0);
    return set_playback_range(params);
  };
}

function set_playback_range(params: any) {
  emits("set_maximum_play_time", params.target.duration);
  emits("set_playback_range", [0, params.target.duration]);
}

function play_video(params: any) {
  if (params.target.currentTime >= props.playback_range[1]) {
    return pass_current_playback_position(props.playback_range[0]);
  }
  // pass_current_playback_position(params.target);
}
function pause_video(params: any) {
  console.log(params.target.currentTime);
}

function pass_current_playback_position(current_time: number) {
  video.value!.currentTime = current_time;
}

const video = ref<HTMLVideoElement>();
watch(
  () => props.playback_range,
  (old_value, new_value) => {
    if (old_value[1] !== new_value[1]) {
      pass_current_playback_position(props.playback_range[1]);
    } else if (old_value[0] !== new_value[0]) {
      pass_current_playback_position(props.playback_range[0]);
    }
  },
  { deep: true }
);

defineExpose({
  pass_current_playback_position,
});
</script>
