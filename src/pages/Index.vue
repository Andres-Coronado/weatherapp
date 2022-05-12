<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input rounded standout v-model="text" label="Search" dark>
        <template v-slot:prepend>
          <q-icon name="my_location" />
        </template>
        <template v-slot:append>
          <q-icon name="search" @click="text = ''" class="cursor-pointer" />
        </template>
      </q-input>
    </div>
    <div></div>
    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">Monterrey</div>
        <div class="text-h6 text-weight-light">Rain</div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>8</span>
          <span class="text-weight-thin text-h3 relative-position degree">
            &deg;
          </span>
          <div class="col text-center">
            <img
              class="col"
              src="https://www.fillmurray.com/100/100"
              alt=""
              srcset=""
            />
          </div>
        </div>
      </div>
    </template>
    <template v-else>
      <div class="col column text-center text-black">
        <div class="col text-h2 text-weight-thin">Quasar <br />Weather</div>
        <q-btn class="col btn" flat color="white">
          <q-icon left size="3em" name="my_location" />
          <div>Find my location</div>
        </q-btn>
      </div>
    </template>
    <div class="col skyline">
      <img src="" alt="" />
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "PageIndex",
  data() {
    return {
      search: "",
      weatherData: null,
    };
  },
});
</script>

<style lang="sass">
.q-page
  background-color: #11111f
  background-image: linear-gradient(180deg, #11111f 0%, #28b3d6 46%, #ce9329 100%)

.degree
  top: -40px
  right: 18px

.skyline
  flex: 0 0 100px
  background: url("../assets/skyline.png")
  background-size: contain
  background-position: center bottom

.btn
  background-color: #003049

</style>
