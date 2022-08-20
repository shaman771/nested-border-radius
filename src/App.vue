<template>
  <h1>Nested Border Radius</h1>
  <div class="app-content">
    <header class="row">
      <app-slider
        v-model="outerRadius"
        :max="50"
        label="Outer border radius, %"
        hint="Set border radius value for the outermost box in % to its width"
        start-icon="⬜"
        end-icon="⚪"
      />
      <app-slider
        v-model="outerBorderWidth"
        :max="50"
        :min="1"
        :step="1"
        label="Outer border width, px"
        hint="Set border width for the outermost box, px"
      />
      <app-slider
        v-model="boxSize"
        :max="800"
        :min="150"
        label="Box size, px"
        hint="Set box size in pixels"
      />
    </header>

    <div class="demo-boxes">
      <DemoBox
        :style="{
          borderRadius: `${outerRadius}%`,
          width: `${boxSize}px`,
          height: `${boxSize}px`,
        }"
        :border-width="`${outerBorderWidth}px`"
        color="blue"
      >
        <DemoBox
          :border-width="`${nestedBorderWidthOne}px`"
          :style="{ borderRadius: nestedRadiusOne }"
          color="yellow"
        >
          <DemoBox
            :border-width="`${nestedBorderWidthTwo}px`"
            :style="{ borderRadius: nestedRadiusTwo }"
            color="blue"
          >
            <DemoBox
              :border-width="`${nestedBorderWidthThree}px`"
              :style="{
                borderRadius: nestedRadiusThree,
                backgroundColor: 'blue',
              }"
              color="yellow"
            ></DemoBox>
          </DemoBox>
        </DemoBox>
      </DemoBox>
    </div>
    <div class="row width-controls">
      <p>
        You can also dynamically change the border width of all nested boxes:
      </p>
      <app-slider
        v-model="nestedBorderWidthOne"
        :max="50"
        :min="0"
        :step="1"
        label="Box 1 border width, px"
        hint="Set border width for the first nested box, px"
      />
      <app-slider
        v-model="nestedBorderWidthTwo"
        :max="50"
        :min="0"
        :step="1"
        label="Box 2 border width, px"
        hint="Set border width for the second nested box, px"
      />
      <app-slider
        v-model="nestedBorderWidthThree"
        :max="50"
        :min="0"
        :step="1"
        label="Box 3 border width, px"
        hint="Set border width for the third nested box, px"
      />
    </div>

    <footer>
      <p>
        To understand the nature of the problem, turn off the nested border
        radius calculation and see the result.
      </p>
      <input id="calcSwitch" v-model="isCalcOn" type="checkbox" />
      <label for="calcSwitch" title="Toggle nested border radius calculation">
        {{ `Nested radius calculation: ${isCalcOn ? 'ON' : 'OFF'}` }}
      </label>
    </footer>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import AppSlider from './components/AppSlider.vue';
import DemoBox from './components/DemoBox.vue';

export default {
  name: 'App',
  components: {
    AppSlider,
    DemoBox,
  },
  setup() {
    const isCalcOn = ref(true);
    const outerRadius = ref(20);
    const outerBorderWidth = ref(15);
    const boxSize = ref(400);
    const nestedBorderWidthOne = ref(15);
    const nestedBorderWidthTwo = ref(15);
    const nestedBorderWidthThree = ref(15);

    const outerRadiusPx = computed(
      () => (boxSize.value * outerRadius.value) / 100
    );

    const nestedSizeOne = computed(
      () => boxSize.value - outerBorderWidth.value * 2
    );
    const nestedSizeTwo = computed(
      () => nestedSizeOne.value - nestedBorderWidthOne.value * 2
    );
    const nestedSizeThree = computed(
      () => nestedSizeTwo.value - nestedBorderWidthTwo.value * 2
    );

    const nestedRadiusOne = computed(() => {
      return isCalcOn.value
        ? `${
            ((outerRadiusPx.value - outerBorderWidth.value) * 100) /
            nestedSizeOne.value
          }%`
        : `${outerRadius.value}%`;
    });

    const nestedRadiusTwo = computed(() => {
      const parentRadiusPx =
        (Number.parseInt(nestedRadiusOne.value) * nestedSizeOne.value) / 100;
      return isCalcOn.value
        ? `${
            ((parentRadiusPx - nestedBorderWidthOne.value) * 100) /
            nestedSizeTwo.value
          }%`
        : `${outerRadius.value}%`;
    });

    const nestedRadiusThree = computed(() => {
      const parentRadiusPx =
        (Number.parseInt(nestedRadiusTwo.value) * nestedSizeTwo.value) / 100;
      return isCalcOn.value
        ? `${
            ((parentRadiusPx - nestedBorderWidthTwo.value) * 100) /
            nestedSizeThree.value
          }%`
        : `${outerRadius.value}%`;
    });

    return {
      isCalcOn,
      outerRadius,
      outerBorderWidth,
      boxSize,
      nestedBorderWidthOne,
      nestedBorderWidthTwo,
      nestedBorderWidthThree,
      nestedRadiusOne,
      nestedRadiusTwo,
      nestedRadiusThree,
    };
  },
};
</script>

<style lang="scss">
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  height: 100vh;

  * {
    box-sizing: border-box;
  }
}
.row {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;

  > :not(:first-child) {
    margin-left: 1.5em;
  }
}
.demo-boxes {
  margin: 1em;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.width-controls {
  margin-bottom: 2em;

  > :first-child {
    width: 100%;
    margin-bottom: 1em;
    font-size: 0.875em;
  }

  > * {
    margin-bottom: 0.5em;
  }
}
.app-content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
}

footer {
  background-color: #eee;
  font-size: 0.875em;
  padding: 0 0 2em;
  input {
    margin-right: 1em;
  }
}
</style>
