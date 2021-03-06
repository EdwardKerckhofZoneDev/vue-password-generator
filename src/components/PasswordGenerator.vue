<template>
  <form class="form-gen-password" @submit.prevent="generatePassword">
    <div class="form-group">
      <label for="passwordLength">Password length:</label>
      <input id="passwordLength" v-model="passwordLength" type="number" />
    </div>

    <div class="form-group">
      <label for="includeSymbols">Include Symbols (&@#$%):</label>
      <input id="includeSymbols" v-model="includeSymbols" type="checkbox" />
    </div>

    <div class="form-group">
      <label for="includeNumbers">Include Numbers (123456):</label>
      <input id="includeNumbers" v-model="includeNumbers" type="checkbox" />
    </div>

    <div class="form-group">
      <label for="includeLowerCase"
        >Include Lowercase Characters (abcdef):</label
      >
      <input id="includeLowerCase" v-model="includeLowerCase" type="checkbox" />
    </div>

    <div class="form-group">
      <label for="includeUpperCase"
        >Include Uppercase Characters (ABCDEF):</label
      >
      <input id="includeUpperCase" v-model="includeUpperCase" type="checkbox" />
    </div>

    <div class="form-group">
      <label for="excludeAmbitious"
        >Exclude Ambiguous Characters ({ } [ ] ( ) / \ ' " ` ~ , ; : .):</label
      >
      <input id="excludeAmbitious" v-model="excludeAmbitious" type="checkbox" />
    </div>

    <div class="form-group">
      <label for="savePreferences">Save My Preferences:</label>
      <input id="savePreferences" v-model="savePreferences" type="checkbox" />
    </div>

    <button type="submit">Generate Password</button>
  </form>

  <div class="generated">
    <form class="form-copy-password" @submit.prevent="copyPassword">
      <p>Your newly generated password:</p>
      <input id="createdPasswordId" v-model="createdPassword" type="text" />
      <button type="submit" @mouseout="outFocus">
        <span class="tooltiptext">{{ tooltipText }}</span
        >Copy
      </button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue'

interface Preference {
  passwordLength: number
  includeSymbols: boolean
  includeNumbers: boolean
  includeLowerCase: boolean
  includeUpperCase: boolean
  excludeAmbitious: boolean
}

export default defineComponent({
  name: 'PasswordGenerator',

  setup() {
    const passwordLength = ref(16)
    const createdPassword = ref('')
    const tooltipText = ref('Copy to clipboard')

    const includeSymbols = ref(true)
    const includeNumbers = ref(true)
    const includeLowerCase = ref(true)
    const includeUpperCase = ref(true)
    const excludeAmbitious = ref(false)

    const SAVED_NAME = 'selectedPasswordOptions'

    const savePreferences = ref(false)

    const arrayFromLowToHigh = (low: number, high: number): Array<number> => {
      const arr: number[] = []
      for (let i = low; i <= high; i++) {
        arr.push(i)
      }
      return arr
    }

    const UPPERCASE_CHAR_CODES = arrayFromLowToHigh(65, 90)
    const LOWERCASE_CHAR_CODES = arrayFromLowToHigh(97, 122)
    const NUMBER_CHAR_CODES = arrayFromLowToHigh(48, 57)
    const SYMBOL_CHAR_CODES = arrayFromLowToHigh(35, 38)
      .concat(arrayFromLowToHigh(42, 43))
      .concat(arrayFromLowToHigh(63, 64))
    const AMBITIOUS_CHAR_CODES = arrayFromLowToHigh(33, 34)
      .concat(arrayFromLowToHigh(39, 41))
      .concat(arrayFromLowToHigh(43, 47))
      .concat(arrayFromLowToHigh(58, 62))
      .concat(arrayFromLowToHigh(91, 96))
      .concat(arrayFromLowToHigh(123, 126))

    const generatePassword = () => {
      let charCodes = LOWERCASE_CHAR_CODES.concat(SYMBOL_CHAR_CODES)
        .concat(NUMBER_CHAR_CODES)
        .concat(UPPERCASE_CHAR_CODES)
        .concat(AMBITIOUS_CHAR_CODES)

      if (!includeSymbols.value)
        charCodes = charCodes.filter((el) => {
          return SYMBOL_CHAR_CODES.indexOf(el) < 0
        })

      if (!includeNumbers.value)
        charCodes = charCodes.filter((el) => {
          return NUMBER_CHAR_CODES.indexOf(el) < 0
        })

      if (!includeUpperCase.value)
        charCodes = charCodes.filter((el) => {
          return UPPERCASE_CHAR_CODES.indexOf(el) < 0
        })

      if (excludeAmbitious.value)
        charCodes = charCodes.filter((el) => {
          return AMBITIOUS_CHAR_CODES.indexOf(el) < 0
        })

      if (savePreferences.value) {
        localStorage.removeItem(SAVED_NAME)
        localStorage.setItem(
          SAVED_NAME,
          JSON.stringify([
            { passwordLength: passwordLength.value },
            { includeSymbols: includeSymbols.value },
            { includeNumbers: includeNumbers.value },
            { includeLowerCase: includeLowerCase.value },
            { includeUpperCase: includeUpperCase.value },
            { excludeAmbitious: excludeAmbitious.value },
          ]),
        )
      }

      const passwordChars = []
      for (let i = 0; i < passwordLength.value; i++) {
        const charCode = charCodes[Math.floor(Math.random() * charCodes.length)]
        passwordChars.push(String.fromCharCode(charCode))
      }

      createdPassword.value = passwordChars.join('')
    }

    const copyPassword = () => {
      if (createdPassword.value !== '') {
        const textToCopy = document.getElementById(
          'createdPasswordId',
        ) as HTMLInputElement
        textToCopy.select()
        textToCopy.setSelectionRange(0, passwordLength.value + 1)
        document.execCommand('copy')

        tooltipText.value = 'Password coppied!'
      } else {
        tooltipText.value = 'No password to copy!'
      }
    }

    const outFocus = () => {
      tooltipText.value = 'Copy to clipboard'
    }

    onMounted(() => {
      let preferences = localStorage.getItem(SAVED_NAME)
      if (preferences) {
        preferences = JSON.parse(preferences)
        ;(preferences as any).forEach((pref: Preference) => {
          if (Object.keys(pref)[0] === 'passwordLength') {
            passwordLength.value = pref.passwordLength
          }
          if (Object.keys(pref)[0] === 'includeSymbols') {
            includeSymbols.value = pref.includeSymbols
          }
          if (Object.keys(pref)[0] === 'includeNumbers') {
            includeNumbers.value = pref.includeNumbers
          }
          if (Object.keys(pref)[0] === 'includeLowerCase') {
            includeLowerCase.value = pref.includeLowerCase
          }
          if (Object.keys(pref)[0] === 'includeUpperCase') {
            includeUpperCase.value = pref.includeUpperCase
          }
          if (Object.keys(pref)[0] === 'excludeAmbitious') {
            excludeAmbitious.value = pref.excludeAmbitious
          }
        })
      }
    })

    return {
      passwordLength,
      includeSymbols,
      includeNumbers,
      includeLowerCase,
      includeUpperCase,
      excludeAmbitious,
      savePreferences,
      generatePassword,
      copyPassword,
      createdPassword,
      tooltipText,
      outFocus,
    }
  },
})
</script>

<style lang="scss" scoped>
.form-gen-password {
  width: 70%;
  max-width: 800px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: left;

  div.form-group {
    display: flex;
    align-items: center;
    margin-bottom: 15px;

    label {
      flex: 0 0 85%;
    }
    input {
      text-align: right;
    }
    input[type='number'] {
      width: 50px;
    }
  }

  button[type='submit'] {
    width: 200px;
    margin: 25px auto;
    background: none;
    border: 2px solid $clr-primary;
    font-size: 16px;
    color: $clr-primary;
    cursor: pointer;
    padding: 10px 0;
    border-radius: 20px;
    transition: all 0.2s ease;
    outline: none;

    &:hover {
      background: $clr-primary;
      color: white;
    }
  }
}

div.generated {
  width: 70%;
  max-width: 800px;
  margin: 0 auto;
  text-align: left;
  margin-top: 40px;

  form {
    display: flex;
    justify-content: space-between;
    align-items: center;

    p {
      display: inline-block;
      margin-right: 30px;
    }

    input {
      flex: 1;
      margin-right: 16px;
      padding: 5px 8px;
    }

    button {
      font-size: 15px;
      padding: 5px 8px;
      cursor: pointer;
      background: none;
      border: none;
      border-radius: 5px;
      text-transform: uppercase;
      position: relative;

      &:hover {
        .tooltiptext {
          visibility: visible;
          opacity: 1;
        }
      }

      .tooltiptext {
        visibility: hidden;
        width: 140px;
        background-color: #555;
        color: #fff;
        text-align: center;
        border-radius: 6px;
        padding: 5px;
        position: absolute;
        z-index: 1;
        bottom: 150%;
        left: 50%;
        margin-left: -75px;
        opacity: 0;
        transition: opacity 0.3s;

        &::after {
          content: '';
          position: absolute;
          top: 100%;
          left: 50%;
          margin-left: -5px;
          border-width: 5px;
          border-style: solid;
          border-color: #555 transparent transparent transparent;
        }
      }
    }
  }
}
</style>
