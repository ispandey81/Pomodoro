<template>
  <div class="home">
    <b-container>
      <audio src="../assets/notification.wav"></audio>
      <b-row align-h="center">
        <b-card tag="article" class="mb-2" :class="backgroundColor">
          <div>
            <b-button-group>
              <button
                @click="onBtnClick('Pomodoro')"
                :class="{ activeBtn: typeOfBtnClicked === 'Pomodoro' }"
              >
                Pomodoro
              </button>
              <button
                @click="onBtnClick('Short Break')"
                :class="{ activeBtn: typeOfBtnClicked === 'Short Break' }"
              >
                Short Break
              </button>
              <button
                @click="onBtnClick('Long Break')"
                :class="{ activeBtn: typeOfBtnClicked === 'Long Break' }"
              >
                Long Break
              </button>
            </b-button-group>
          </div>
          <b-card-text>
            <span id="minuteForTimeCalculation">{{ minuteHTMLValue }}</span
            >:<span id="second">{{ secondHTMLValue }}</span>
          </b-card-text>
          <b-button
            @click="startClicked(intervalId)"
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
            @click="stopClicked"
            pill
            href="#"
            variant="outline-secondary"
            size="lg"
            ><strong>STOP</strong></b-button
          >
          <b-button
            class="ml-2"
            v-if="timerStarted"
            @click="resetClicked"
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
  private static readonly PLACEHOLDER_TEXT = "Pomodoro";
  private timerStarted = false;
  private minuteForTimeCalculation = 25;
  private secondForTimeCalculation = 60;
  private intervalId = NaN;
  private minuteHTMLValue = "25";
  private secondHTMLValue = "00";
  private typeOfBtnClicked = HomeView.PLACEHOLDER_TEXT;
  private backgroundColor = {
    pomodoro: true,
    shortBreak: false,
    longBreak: false,
  };

  startClicked(intervalId: number) {
    this.stopClicked();
    this.timerStarted = true;
    if (!intervalId) {
      this.minuteForTimeCalculation = this.minuteForTimeCalculation - 1;
      this.minuteHTMLValue = this.returnData(this.minuteForTimeCalculation);
      this.secondForTimeCalculation = this.secondForTimeCalculation - 1;
      this.secondHTMLValue = this.returnData(this.secondForTimeCalculation);
    }
    this.intervalId = setInterval(() => this.timer(), 1000); // repeat every second
    this.playAudio();
  }

  playAudio() {
    const audioElement = document.querySelector("audio")!;
    audioElement.play();
  }

  resetClicked() {
    this.onBtnClick(this.typeOfBtnClicked);
    this.stopClicked();
    this.intervalId = NaN;
  }

  stopClicked() {
    clearInterval(this.intervalId);
    this.timerStarted = false;
  }

  onBtnClick(typeOfBtnClicked: string) {
    window.document.title = HomeView.PLACEHOLDER_TEXT;
    this.timerStarted = false;
    this.stopClicked();
    this.intervalId = NaN;
    this.typeOfBtnClicked = typeOfBtnClicked;
    this.secondForTimeCalculation = 60;
    this.secondHTMLValue = "00";
    if (typeOfBtnClicked === HomeView.PLACEHOLDER_TEXT) {
      this.minuteForTimeCalculation = 25;
      this.minuteHTMLValue = "25";
      this.backgroundColor = {
        pomodoro: true,
        shortBreak: false,
        longBreak: false,
      };
    } else if (typeOfBtnClicked === "Short Break") {
      this.minuteForTimeCalculation = 5;
      this.minuteHTMLValue = "05";
      this.backgroundColor = {
        pomodoro: false,
        shortBreak: true,
        longBreak: false,
      };
    } else if (typeOfBtnClicked === "Long Break") {
      this.minuteForTimeCalculation = 15;
      this.minuteHTMLValue = "15";
      this.backgroundColor = {
        pomodoro: false,
        shortBreak: false,
        longBreak: true,
      };
    }
  }

  timer() {
    if (
      this.secondForTimeCalculation == 60 ||
      this.secondForTimeCalculation > 0
    ) {
      this.secondForTimeCalculation--;
    }
    if (
      this.secondForTimeCalculation == 0 &&
      this.minuteForTimeCalculation !== 0
    ) {
      this.secondForTimeCalculation = 60;
      this.minuteForTimeCalculation--;
    }
    if (
      this.minuteForTimeCalculation == 0 &&
      this.secondForTimeCalculation == 0
    ) {
      if (this.typeOfBtnClicked === HomeView.PLACEHOLDER_TEXT) {
        this.typeOfBtnClicked = "Short Break";
      } else {
        this.typeOfBtnClicked = HomeView.PLACEHOLDER_TEXT;
      }
      this.resetClicked();
      this.playAudio();
    }
    this.minuteHTMLValue = this.returnData(this.minuteForTimeCalculation);
    this.secondHTMLValue = this.returnData(this.secondForTimeCalculation);
    window.document.title = `${this.minuteHTMLValue}:${this.secondHTMLValue}`;
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
  background-color: var(--international-orange-engineering);
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
.activeBtn {
  background-color: var(--rich-black-fogra-29);
  color: white;
  border-radius: 0.5rem;
}
.pomodoro {
  background-color: var(--international-orange-engineering);
}
.shortBreak {
  background-color: var(--blue-sapphire);
}
.longBreak {
  background-color: forestgreen;
}
</style>
