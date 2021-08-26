<template>
  <div class="banner">
    <h1>Cypher</h1>
  </div>
  <div class="container">
    <fieldset>
      <div class="password-display">
        <input @click="highlightInput($event)" type="text" v-model="password" />
        <!-- <button v-clipboard="() => password">Copy</button> -->
        <button @click="password = genPassword(passwordLength)">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
            <g data-name="Layer 2">
              <g data-name="refresh">
                <rect width="24" height="24" opacity="0" />
                <path
                  d="M20.3 13.43a1 1 0 0 0-1.25.65A7.14 7.14 0 0 1 12.18 19 7.1 7.1 0 0 1 5 12a7.1 7.1 0 0 1 7.18-7 7.26 7.26 0 0 1 4.65 1.67l-2.17-.36a1 1 0 0 0-1.15.83 1 1 0 0 0 .83 1.15l4.24.7h.17a1 1 0 0 0 .34-.06.33.33 0 0 0 .1-.06.78.78 0 0 0 .2-.11l.09-.11c0-.05.09-.09.13-.15s0-.1.05-.14a1.34 1.34 0 0 0 .07-.18l.75-4a1 1 0 0 0-2-.38l-.27 1.45A9.21 9.21 0 0 0 12.18 3 9.1 9.1 0 0 0 3 12a9.1 9.1 0 0 0 9.18 9A9.12 9.12 0 0 0 21 14.68a1 1 0 0 0-.7-1.25z"
                />
              </g>
            </g>
          </svg>
        </button>
      </div>
      <div class="fieldset-item">
        <div class="input-stack">
          <div class="password-length-display">
            <label
              for="password-length"
              id="password-length"
              aria-hidden="true"
            >
              Length
            </label>
            <span>{{ passwordLength }}</span>
          </div>
          <!-- <input type="text" v-model="passwordLength" /> -->
          <div class="password-length">
            <span>{{ minLength }}</span>
            <input
              v-model="passwordLength"
              name="password-length"
              aria-labelledby="password-length"
              type="range"
              min="8"
              max="100"
              style="--track-fill: 30%"
              @change="password = genPassword(passwordLength)"
            />
            <span>{{ maxLength }}</span>
          </div>
        </div>
      </div>
      <div class="fieldset-item preference">
        <div class="preference-row">
          <div class="input-stack">
            <label for="pref-symbols"> Include Symbols </label>
          </div>
          <input
            type="checkbox"
            checked
            id="pref-symbols"
            name="pref-symbols"
            v-model="preferences.symbols"
            @click="setPreference('pref-symbols', $event.target.checked)"
          />
        </div>
        <div class="preference-row">
          <div class="input-stack">
            <label for="pref-symbols"> Include Hyphens </label>
          </div>
          <input
            type="checkbox"
            checked
            id="pref-hyphens"
            name="pref-hyphens"
            v-model="preferences.hyphens"
            @click="setPreference('pref-hyphens', $event.target.checked)"
          />
        </div>
      </div>
    </fieldset>
  </div>
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
        hyphens: localStorage.getItem("pref-hyphens")
          ? localStorage.getItem("pref-hyphens")
          : false,
      },
    };
  },
  methods: {
    pwLengthHandler: (event: HTMLElementEvent<HTMLInputElement>) => {
      console.log(event.target.value);
    },
    highlightInput: ($event) => {
      $event.srcElement.select();
    },
    genPassword: (length: number) => {
      let charset =
        "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
      let chars = charset;
      if (localStorage.getItem("pref-symbols") === "true") {
        chars += "!@€£#$%^&*";
      }
      let retVal = "";
      for (var i = 0, n = chars.length; i < length; ++i) {
        retVal += chars.charAt(Math.floor(Math.random() * n));
      }
      if (localStorage.getItem("pref-hyphens") === "true") {
        retVal = retVal
          .match(/.{1,8}/g)
          .concat()
          .toString()
          .replaceAll(",", "-");
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

.container {
  max-width: 500px;
  margin: auto;
}

.password-length-display {
  display: flex;
  justify-content: space-between;
  margin: 0.5em 0;
}
.password-display {
  max-width: 500px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.password-display input {
  width: 100%;
  height: 2em;
  font-size: 2em;
  padding: 0 0.5em;
  margin: 0 0.25em 0 0;
  font-family: monospace;
  background-color: #eee;
  border: none;
  color: #304455;
}

.password-display button {
  background: #075d23;
  padding: 1em;
  border: none;
  color: #ffeffe;
  height: 100%;
  font-family: PlusJakartaSans-Medium;
  border-radius: 0.5em;
  cursor: pointer;
}

.password-display button svg {
  width: 2em;
  color: #ffeffe;
  fill: #ffeffe;
}

.password-length {
  padding: 1em;
  display: flex;
  background: #075d23;
  justify-content: space-between;
  color: #ffeffe;
  border-radius: 0.5em;
}

.password-length input {
  flex: 1;
  margin: 0 1em;
}

.preference {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 1em 0 0 0;
  flex-direction: column;
}

.preference-row {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.banner {
  padding: 1em;
  background-image: linear-gradient(25deg, #026544, #158e63, #28ba84, #3be8a7);
  text-align: center;
  margin-bottom: 2em;
}

h1 {
  margin: 0;
}

h1,
a {
  color: #ffeffe;
  /* -webkit-background-clip: text;
  -webkit-text-fill-color: transparent; */
}

fieldset {
  border: none;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
  color: #095c23;
}

code {
  font-size: 2em;
  background-color: #eee;
  padding: 2px 4px;
  border-radius: 4px;
  color: #304455;
}
</style>
