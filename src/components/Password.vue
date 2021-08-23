<template>
  <fieldset>
    <p>{{password}}</p>
    <div class="fieldset-item">
      <div class="input-stack">
        <label for="password-length" id="password-length" aria-hidden="true">
          Length
        </label>
        <br />
        <span>{{ passwordLength }}</span>
        <!-- <input type="text" v-model="passwordLength" /> -->
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
            @change="password = genPassword(passwordLength, true)"
          />
          <span>{{ maxLength }}</span>
        </div>
      </div>
    </div>
    <div class="fieldset-item">
      <input
        type="checkbox"
        checked
        id="pref-symbols"
        name="pref-symbols"
        v-model="preferences.symbols"
        @click="setPreference('pref-symbols', $event.target.checked)"
      />
      <div class="input-stack">
        <label for="pref-symbols">
          <h3>Include Symbols</h3>
        </label>
      </div>
    </div>
  </fieldset>

  <button type="button" @click="password = genPassword(passwordLength, true)">
    count is: {{ count }}
  </button>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
type HTMLElementEvent<T extends HTMLElement> = Event & {
  target: T;
  // probably you might want to add the currentTarget as well
  // currentTarget: T;
};
export default defineComponent({
  name: "Password",
  props: {
    msg: {
      type: String,
      required: true,
    },
  },
  data: () => {
    const initData = {
      minLength: 8,
      passwordLength: 8,
      maxLength: 100,
      password: "",
      preferences: {
        symbols: true,
      },
    };
    return initData;
  },
  methods: {
    pwLengthHandler: (event: HTMLElementEvent<HTMLInputElement>) => {
      console.log(event.target.value);
    },
    changeSetting: () => {},
    genPassword: (length: number, incSymbols = false) => {
      let charset =
        "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
      incSymbols ? charset = charset += '!@€£#$$%^&*()_+{}[];,.' : charset = charset;
      let retVal = "";
      for (var i = 0, n = charset.length; i < length; ++i) {
        retVal += charset.charAt(Math.floor(Math.random() * n));
      }
      return retVal;
    },
    setPreference: (preference: string, value: unknown) => {
      console.log(preference, value);
    },
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
