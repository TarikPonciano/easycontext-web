<template>
  <v-card max-width="500px" min-height="500px">
    <v-card-title>
      <v-layout wrap>
        <v-flex sm2>
          <v-icon>vibration</v-icon>
        </v-flex>
        <v-flex sm4>
          <span>Vibrate</span>
        </v-flex>
        <v-flex sm5>
          <v-layout wrap>
            <v-flex sm10></v-flex>
            <v-flex sm2>
              <v-btn icon small color="red" @click="performDelete">
                <v-icon small>delete</v-icon>
              </v-btn>
            </v-flex>
          </v-layout>
        </v-flex>
      </v-layout>
    </v-card-title>
    <v-card-text>
      <v-row class="mt-12">
      <v-slider thumb-label="always" thumb-size="48" min="100" max="2000" label="Time (ms)" v-model="vibrateTime"
      @end="sendData"><template v-slot:thumb-label="{ value }">
            {{ value+"ms" }}
          </template></v-slider>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
import VibrateAction from "../../model/actions/VibrateAction"
export default {
    
  props: {
    id: String,
    onDelete: Function,
    onChange: Function
  },
  mounted() {
    const prevState = this.$store.getters.componentState(this.id);
    if (prevState) {
      this.vibrateTime = prevState.vibrateTime || this.vibrateTime;
    }
    this.sendData();
  },
  data() {
    return {
      vibrateTime: 500
    };
  },
  methods: {
    performDelete() {
      this.onDelete(this.id);
    },
    sendData() {
      let action = new VibrateAction(this.vibrateTime);
      let componentState = {};
      componentState.vibrateTime = this.vibrateTime;
      componentState.id = this.id;
      this.onChange(this.id, {
        action: action,
        componentState: componentState
      });
    }
  }
};
</script>

<style>
</style>