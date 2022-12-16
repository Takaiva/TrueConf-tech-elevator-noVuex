<template>
  <div
    class="elevator"
    :style="elevatorOptions?.style"
    :class="elevatorOptions?.class"
  >
    <div class="elevator__direction" v-if="queueStatus">
      <span class="elevator__direction__floor-num">
        {{ elevatorOptions?.targetFloor }}
      </span>
      <span
        class="elevator__direction__arrow"
        v-if="elevatorOptions?.direction === 'up'"
      >
        &#8593;
      </span>
      <span
        class="elevator__direction__arrow"
        v-if="elevatorOptions?.direction === 'down'"
      >
        &#8595;
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    elevatorOptions: Object,
    queueStatus: String || null,
  },
};
</script>

<style scoped>
.elevator {
  position: relative;
  width: 100%;
  height: 100%;
  background: #eb6841;
  border-radius: 14px;
  display: flex;
  justify-content: center;
}
.elevator__direction {
  margin-top: 10px;
  padding: 10px;
  background: #155263;
  border-radius: 10px;
  height: 30%;
  display: flex;
  /*justify-content: space-between;*/
  align-items: center;
  justify-content: center;
  gap: 5px;
  color: #eb6841;
  font-size: 30px;
  font-weight: bold;
}
.elevator__direction__arrow {
  font-size: 24px;
}

.elevator_resting {
  animation: blinker 1s linear infinite;
}

@keyframes blinker {
  50% {
    opacity: 0;
  }
}
</style>
