<template>
  <div class="container">
    <div v-for="count in floorsCount" :key="count" class="floor">
      <div class="floor__container">
        <Floor>
          <Elevator
            v-if="count === 1"
            :elevatorOptions="elevatorOptions"
            :queueStatus="queueStatus"
          />
        </Floor>
      </div>
      <FloorControl
        :count="count"
        :targetFloor="targetFloor"
        @changeFloor="changeFloor"
      />
    </div>
  </div>
</template>

<script>
import Floor from "@/components/FloorComponent.vue";
import Elevator from "@/components/ElevatorComponent.vue";
import FloorControl from "@/components/FloorControl";
export default {
  components: {
    Floor,
    Elevator,
    FloorControl,
  },
  props: {
    floors: Number,
  },
  data() {
    return {
      floorsCount: "",
      targetFloor: 1, // default
      elevatorOptions: {},
      eventQueue: [],
      queueStatus: null,
    };
  },
  methods: {
    changeFloor(floor, controlsApi) {
      const diff = Math.abs(floor - this.targetFloor);
      const direction = floor > this.targetFloor ? "up" : "down";
      this.targetFloor = floor;
      const options = {
        style: {
          transition: `transform ${diff}s linear 0s`,
          transform: `translateY(-${121 * (floor - 1)}px)`, // 121 because of 1px border
        },
        timeForMove: diff * 1000,
        targetFloor: floor,
        direction,
        controlsApi,
      };
      this.eventQueue.push(options);
      if (!this.queueStatus) {
        this.performTasks();
        this.queueStatus = "performing";
      }
    },

    performTasks() {
      if (this.eventQueue.length === 0) {
        this.queueStatus = null;
        return;
      }

      const timeForResting = 3000;
      const {
        style,
        timeForMove,
        direction,
        targetFloor,
        controlsApi
      } = this.eventQueue.shift();

      this.elevatorOptions = { style, direction, targetFloor };
      // highlight the floor to which the elevator is heading
      controlsApi.highlightCurrentTarget();

      setTimeout(() => {
        // elevator blink while resting
        this.elevatorOptions.class = "elevator_resting";
        // unhighlight the floor that the elevator visited
        controlsApi.unhighlightCurrentTarget();
        setTimeout(() => {
          this.elevatorOptions.class = null;
          // remove indication to show that the floor is out of queue
          controlsApi.refreshControl();
          this.performTasks();
        }, timeForResting);
      }, timeForMove);
    },
  },
  watch: {
    floors(newVal) {
      this.floorsCount = newVal;
    },
  },
  mounted() {
    this.floorsCount = this.floors;
  },
};
</script>

<style scoped>
.container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column-reverse;
}
.floor {
  height: 100%;
  width: 100%;
  background: floralwhite;
  display: flex;
  flex-direction: row;
  gap: 20px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 8px;
}
.floor__container {
  display: flex;
  border-radius: 8px;
}
</style>
