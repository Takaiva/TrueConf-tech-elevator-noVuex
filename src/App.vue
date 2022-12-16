<template>
  <div class="wrapper">
    <AppOptions
      :floorsCount="floorsCount"
      @changeNumOfFloors="(value) => (this.floorsCount = value)"
    />
    <Shaft :floors="floorsCount" />
  </div>
</template>

<script>
import Shaft from "./components/ShaftComponent.vue";
import AppOptions from "@/components/AppOptions.vue";
export default {
  components: {
    Shaft,
    AppOptions,
  },
  data() {
    return {
      floorsCount: 5, // default
    };
  },
  mounted() {
    // save floors count on page refresh
    if (Object.prototype.hasOwnProperty.call(localStorage, "floorsCount")) {
      this.floorsCount = Number(localStorage.getItem("floorsCount"));
    }
  },
  watch: {
    floorsCount(newVal) {
      localStorage.setItem("floorsCount", newVal);
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  min-height: 100vh;
  width: 100%;
  background: #393e46;
}

.wrapper {
  margin: 10px;
}
</style>
