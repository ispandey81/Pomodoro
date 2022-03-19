<template>
  <div class="home">
    <b-container>
      <b-row align-h="center">
        <b-card tag="article" class="mb-2">
          <b-card-text>
            <span id="minuteForTimeCalculation">{{ minuteHTMLValue }}</span
            >:<span id="second">{{ secondHTMLValue }}</span>
          </b-card-text>
          <b-button
            v-on:click="startClicked"
            v-if="!timerStarted"
            pill
            href="#"
            variant="outline-secondary"
            size="lg"
            ><strong>START</strong></b-button
          >
          <b-button
            class="ml-2"
            v-if="timerStarted"
            v-on:click="stopClicked"
            pill
            href="#"
            variant="outline-secondary"
            size="lg"
            ><strong>STOP</strong></b-button
          >
          <b-button
            class="ml-2"
            v-if="timerStarted"
            v-on:click="resetClicked"
            pill
            href="#"
            variant="outline-secondary"
            size="lg"
            ><strong>RESET</strong></b-button
          >
        </b-card>
      </b-row>
    </b-container>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src

@Component({
  components: {
    HelloWorld,
  },
})
export default class HomeView extends Vue {
  private timerStarted = false;
  private minuteForTimeCalculation = 25;
  private secondForTimeCalculation = 60;
  private intervalId!: number;
  private minuteHTMLValue = "25";
  private secondHTMLValue = "00";

  startClicked() {
    this.stopClicked();
    this.timerStarted = true;
    this.minuteHTMLValue = "24";
    this.minuteForTimeCalculation = 24;
    this.secondHTMLValue = "59";
    this.secondForTimeCalculation = 59;
    this.intervalId = setInterval(() => this.timer(), 1000); // repeat every second
  }

  resetClicked() {
    this.timerStarted = false;
    this.minuteForTimeCalculation = 25;
    this.secondForTimeCalculation = 60;
    this.minuteHTMLValue = "25";
    this.secondHTMLValue = "00";
    this.stopClicked();
  }

  stopClicked() {
    clearInterval(this.intervalId);
    this.timerStarted = false;
  }

  timer() {
    if (
      this.secondForTimeCalculation == 60 ||
      this.secondForTimeCalculation > 0
    ) {
      this.secondForTimeCalculation--;
    }
    if (this.secondForTimeCalculation == 0) {
      this.secondForTimeCalculation = 60;
      this.minuteForTimeCalculation--;
    }
    if (this.minuteForTimeCalculation == 0) {
      clearInterval(this.intervalId);
    }
    this.minuteHTMLValue = this.returnData(this.minuteForTimeCalculation);
    this.secondHTMLValue = this.returnData(this.secondForTimeCalculation);
  }

  returnData(input: number) {
    return input >= 10 && input !== 60
      ? `${input}`
      : input === 60
      ? "00"
      : `0${input}`;
  }
}
</script>
<style scoped>
.card {
  background-color: var(--blue-sapphire);
  max-width: 40rem;
  color: white;
}
.btn-outline-secondary,
.btn-outline-secondary:hover {
  color: var(--rich-black-fogra-29);
  border-color: var(--ghost-white);
  background-color: var(--ghost-white);
  font-size: 1.5rem;
}
.card-text {
  font-size: 10rem;
  font-weight: bold;
}
</style>
