<template>
  <fieldset>
    <div class="fieldset-item">
      <div class="input-stack">
        <label for="password-length" id="password-length" aria-hidden="true">
          Length
        </label>
        <div class="password-length">
          <span>{{ minLength }}</span>
          <input
            v-model="passwordLength"
            name="password-length"
            aria-labelledby="password-length"
            type="range"
            min="0"
            max="100"
            style="--track-fill: 30%"
          />
          <span>{{ maxLength }}</span>
        </div>
        <div class="fieldset-item">
          <input
            type="checkbox"
            checked
            id="text-notifications"
            name="text-notifications"
          />
          <div class="input-stack">
            <label for="text-notifications">
              <h3>Include Symbols</h3>
              <small
                >Get notified about all text messages sent to your device</small
              >
            </label>
          </div>
        </div>
        <br />
        <input type="text" v-model="passwordLength" />
      </div>
    </div>
  </fieldset>

  <button type="button" @click="count++">count is: {{ count }}</button>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
export default defineComponent({
  name: "Password",
  props: {
    msg: {
      type: String,
      required: true,
    },
  },
  data: () => {
    return {
      minLength: 8,
      passwordLength: 8,
      maxLength: 100,
    };
  },
  methods: {
    changeSetting: () => {},
  },
  setup: () => {
    const count = ref(0);
    return { count };
  },
});

// const sliders = document.querySelectorAll('input[type="range"]')

// const rangeToPercent = (slider: unknown) => {
//   const max = slider.getAttribute('max') || 10;
//   const percent = slider.value / max * 100;

//   return `${parseInt(percent)}%`
// }

// sliders.forEach(slider => {
//   slider.style.setProperty('--track-fill', rangeToPercent(slider));

//   slider.addEventListener('input', e => {
//     e.target.style.setProperty('--track-fill', rangeToPercent(e.target));
//   })
// })
</script>

<style scoped>
a {
  color: #42b983;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
}

code {
  background-color: #eee;
  padding: 2px 4px;
  border-radius: 4px;
  color: #304455;
}
</style>
