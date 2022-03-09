<template>
  <div class="home">
    <b-container>
      <b-row align-h="center">
        <b-card tag="article" class="mb-2">
          <b-card-text> 25:00 </b-card-text>
          <b-button pill href="#" variant="outline-secondary" size="lg"
            ><strong>START</strong></b-button
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
export default class HomeView extends Vue {}
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
  font-size: 2rem;
}
.card-text {
  font-size: 10rem;
  font-weight: bold;
}
</style>
