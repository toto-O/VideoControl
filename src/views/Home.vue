<template>
  <v-row>
    <v-col align="center">
      <embedded-video
        :playback_range="value"
        @set_maximum_play_time="set_maximum_play_time"
        @set_playback_range="set_playback_range"
      />
      <play-back-range-slider
        v-model="value"
        :frame_rate="frame_rate"
        :maximum_play_time="maximum_play_time"
      />
    </v-col>
  </v-row>
</template>

<script lang="ts" setup>
import EmbeddedVideo from "@/components/EmbeddedVideo.vue";
import PlayBackRangeSlider from "@/components/PlayBackRangeSlider.vue";

import { ref } from "vue";
const value = ref<number[]>([0, 0]);
const frame_rate = ref<number>(30);
const maximum_play_time = ref<number>(0);

function set_maximum_play_time(params: number) {
  maximum_play_time.value = params;
}
function set_playback_range(playback_range: number[]) {
  value.value = playback_range;
}
</script>
