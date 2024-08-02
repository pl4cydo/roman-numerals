<script setup lang="ts">
import { ref } from 'vue'

class Roman {
  constructor() {
    this.romanNumerals = [
      ['I', 'II', 'III', 'IV', 'V', 'VI', 'VII', 'VIII', 'IX'],
      ['X', 'XX', 'XXX', 'XL', 'L', 'LX', 'LXX', 'LXXX', 'XC'],
      ['C', 'CC', 'CCC', 'CD', 'D', 'DC', 'DCC', 'DCCC', 'CM'],
      ['M']
    ]
  }

  numberForRoman(num) {
    let result = ''
    switch (String(num).length) {
      case 1:
        result = this.romanNumerals[0][num - 1]
        break
      case 2:
        let dezena = Math.floor(num / 10)
        let unidade = num % 10
        if (dezena > 0) result += this.romanNumerals[1][dezena - 1]
        if (unidade > 0) result += this.romanNumerals[0][unidade - 1]
        break
      case 3:
        let centena = Math.floor(num / 100)
        let unidade2 = (num % 100) % 10
        let dezena2 = ((num % 100) - unidade2) / 10
        if (centena > 0) result += this.romanNumerals[2][centena - 1]
        if (dezena2 > 0) result += this.romanNumerals[1][dezena2 - 1]
        if (unidade2 > 0) result += this.romanNumerals[0][unidade2 - 1]
        break
    }
    return result
  }

  romanForNumber(roman) {
    const saves = roman.split('')
    let result = 0
    for (let i = 0; i < saves.length; i++) {
      switch (saves[i]) {
        case 'I':
          if (saves[i + 1] == 'V' || saves[i + 1] == 'X') result = result - 1
          else result = result + 1
          break
        case 'V':
          result = result + 5
          break
        case 'X':
          if (saves[i + 1] == 'L' || saves[i + 1] == 'C') result = result - 10
          else result = result + 10
          break
        case 'L':
          result = result + 50
          break
        case 'C':
          if (saves[i + 1] == 'D' || saves[i + 1] == 'M') result = result - 100
          else result = result + 100
          break
        case 'D':
          result = result + 500
          break
        case 'M':
          result = result + 1000
          break
      }
    }
    return result
  }
}

const romanInstance = new Roman()

const resultado = ref()
const first_input = ref()
const second_input = ref()

function converter(a) {
  resultado.value = romanInstance.numberForRoman(a)
}
function converter2(a) {
  resultado.value = romanInstance.romanForNumber(a)
}
</script>

<template>
  <div class="container">
    <div class="insid">
      <h1>Conversor</h1>
      <span>Conversor número para algarismo romano</span>
      <input type="text" v-model="first_input" placeholder="Input 1" class="input-field" />
      <button @click="converter(first_input)">Converter</button>
      <span>Conversor algarismo romano para número</span>
      <input type="text" v-model="second_input" placeholder="Input 2" class="input-field" />
      <button @click="converter2(second_input)">Converter</button>
      <div>
        <h2>Resultado: {{ resultado }}</h2>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.insid {
  width: 25%;
  height: 50%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #2e2e2e;
  border-radius: 5px;
  display: flex;
  align-items: flex-start;
  flex-direction: column;
}

h1 {
  margin-bottom: 25px;
}

span {
  margin-top: 25px;
}

.input-field {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  margin-bottom: 5px;
  border: 1px solid #2e2e2e;
  border-radius: 5px;
  box-sizing: border-box;
  font-size: 16px;
  transition:
    border-color 0.3s,
    box-shadow 0.3s;
}

.input-field:focus {
  border-color: #007bff;
  box-shadow: 0 0 8px rgba(0, 123, 255, 0.25);
  outline: none;
}
</style>
