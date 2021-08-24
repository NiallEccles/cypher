<template>
  <div class="banner">
    <h1>Cypher</h1>
  </div>
  <fieldset>
    <p>{{ password }}</p>
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
            @change="password = genPassword(passwordLength)"
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
import { ref, defineComponent, reactive } from "vue";
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
    return {
      minLength: 8,
      passwordLength: 8,
      maxLength: 100,
      password: "",
      preferences: {
        symbols: localStorage.getItem("pref-symbols")
          ? localStorage.getItem("pref-symbols")
          : false,
      },
    };
  },
  methods: {
    pwLengthHandler: (event: HTMLElementEvent<HTMLInputElement>) => {
      console.log(event.target.value);
    },
    changeSetting: () => {},
    genPassword: (length: number) => {
      let charset =
        "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
      let chars = charset;
      if (localStorage.getItem("pref-symbols") === "true") {
        chars += "!@€£#$$%^&*()_+{}[];,.";
      }

      let retVal = "";
      for (var i = 0, n = chars.length; i < length; ++i) {
        retVal += chars.charAt(Math.floor(Math.random() * n));
      }
      return retVal;
    },
    setPreference: (preference: string, value: string | number) => {
      localStorage.setItem(preference, value.toString());
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
/* #0BCF3E */
/* #095C23 */

.banner{
  padding: 1em;
  background-image: linear-gradient(25deg, #026544, #158e63, #28ba84, #3be8a7);
}

h1{
  margin: 0;
}

h1,
a {
  color: #FFEFFE;
  /* -webkit-background-clip: text;
  -webkit-text-fill-color: transparent; */
}

fieldset {
  border: none;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
  color: #095C23;
}

code {
  background-color: #eee;
  padding: 2px 4px;
  border-radius: 4px;
  color: #304455;
}
</style>
