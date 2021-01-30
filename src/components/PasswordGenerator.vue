<template>
  <form @submit.prevent="generatePassword">
    <div class="form-group">
      <label for="passwordLength">Password length:</label>
      <input type="number" v-model="passwordLength" id="passwordLength" />
    </div>

    <div class="form-group">
      <label for="includeSymbols">Include Symbols (&@#$%):</label>
      <input type="checkbox" v-model="includeSymbols" id="includeSymbols" />
    </div>

    <div class="form-group">
      <label for="includeNumbers">Include Numbers (123456):</label>
      <input type="checkbox" v-model="includeNumbers" id="includeNumbers" />
    </div>

    <div class="form-group">
      <label for="includeLowerCase"
        >Include Lowercase Characters (abcdef):</label
      >
      <input type="checkbox" v-model="includeLowerCase" id="includeLowerCase" />
    </div>

    <div class="form-group">
      <label for="includeUpperCase"
        >Include Uppercase Characters (ABCDEF):</label
      >
      <input type="checkbox" v-model="includeUpperCase" id="includeUpperCase" />
    </div>

    <div class="form-group">
      <label for="excludeAmbitious"
        >Exclude Ambiguous Characters ({ } [ ] ( ) / \ ' " ` ~ , ; : .):</label
      >
      <input type="checkbox" v-model="excludeAmbitious" id="excludeAmbitious" />
    </div>

    <div class="form-group">
      <label for="savePreferences">Save My Preferences:</label>
      <input type="checkbox" v-model="savePreferences" id="savePreferences" />
    </div>

    <button type="submit">Generate Password</button>
  </form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'PasswordGenerator',

  setup() {
    const passwordLength = ref(16)
    const includeSymbols = ref(true)
    const includeNumbers = ref(true)
    const includeLowerCase = ref(true)
    const includeUpperCase = ref(true)
    const excludeAmbitious = ref(false)
    const savePreferences = ref(false)

    const generatePassword = () => {}

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

    return {
      passwordLength,
      includeSymbols,
      includeNumbers,
      includeLowerCase,
      includeUpperCase,
      excludeAmbitious,
      savePreferences,
      generatePassword,
    }
  },
})
</script>

<style lang="scss" scoped></style>
