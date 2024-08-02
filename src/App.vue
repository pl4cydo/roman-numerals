<script setup lang="ts">
import { ref, type Ref } from "vue";

//TODO: Colocar verificação

interface RomanIntereface {
  numberForRoman(num: number): string;
  romanForNumber(roman: string): number;
}

class Roman implements RomanIntereface {
  private static readonly romanNumerals: string[][] = [
    ["I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"],
    ["X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"],
    ["C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"],
    ["M"],
  ];

  public numberForRoman(num: number): string {
    let result: string = "";
    switch (String(num).length) {
      case 1:
        result = Roman.romanNumerals[0][num - 1];
        break;
      case 2:
        const case2Tens: number = Math.floor(num / 10);
        const case2Unit: number = num % 10;
        if (case2Tens > 0) result += Roman.romanNumerals[1][case2Tens - 1];
        if (case2Unit > 0) result += Roman.romanNumerals[0][case2Unit - 1];
        break;
      case 3:
        const case3Hundred: number = Math.floor(num / 100);
        const case3Tens: number = Math.floor((num % 100) / 10);
        const case3Unit: number = Math.floor(num % 100) % 10;
        if (case3Hundred > 0)
          result += Roman.romanNumerals[2][case3Hundred - 1];
        if (case3Tens > 0) result += Roman.romanNumerals[1][case3Tens - 1];
        if (case3Unit > 0) result += Roman.romanNumerals[0][case3Unit - 1];
        break;
      case 4:
        const case4Thousands: number = Math.floor(num / 1000);
        const case4Hundred: number = Math.floor((num % 1000) / 100);
        const case4Tens: number = Math.floor(((num % 1000) % 100) / 10);
        const case4Unit: number = Math.floor(((num % 1000) % 100) % 10);

        if (case4Thousands > 0)
          result += Roman.romanNumerals[3][case4Thousands - 1];
        if (case4Hundred > 0)
          result += Roman.romanNumerals[2][case4Hundred - 1];
        if (case4Tens > 0) result += Roman.romanNumerals[1][case4Tens - 1];
        if (case4Unit > 0) result += Roman.romanNumerals[0][case4Unit - 1];
    }
    return result;
  }

  public romanForNumber(roman: string): number {
    const saves: string[] = roman.split("");
    let result: number = 0;
    for (let i = 0; i < saves.length; i++) {
      switch (saves[i]) {
        case "I":
          if (saves[i + 1] == "V" || saves[i + 1] == "X") result = result - 1;
          else result = result + 1;
          break;
        case "V":
          result = result + 5;
          break;
        case "X":
          if (saves[i + 1] == "L" || saves[i + 1] == "C") result = result - 10;
          else result = result + 10;
          break;
        case "L":
          result = result + 50;
          break;
        case "C":
          if (saves[i + 1] == "D" || saves[i + 1] == "M") result = result - 100;
          else result = result + 100;
          break;
        case "D":
          result = result + 500;
          break;
        case "M":
          result = result + 1000;
          break;
      }
    }
    return result;
  }
}

const result: Ref<number | string | undefined> = ref();
const first_input: Ref<number | undefined> = ref();
const second_input: Ref<string | undefined> = ref();

function convert(input: number | string) {
  const romanInstance = new Roman();
  if (input && typeof input === "string") {
    result.value = romanInstance.romanForNumber(input);
  } else if (input && typeof input === "number") {
    result.value = romanInstance.numberForRoman(input);
  }
}
</script>

<template>
  <div class="container">
    <div class="insid">
      <h1>Conversor</h1>
      <span>Conversor número para algarismo romano</span>
      <input
        type="number"
        v-model="first_input"
        placeholder="Número"
        class="input-field"
      />
      <button @click="convert(first_input as number)">Converter</button>
      <span>Conversor algarismo romano para número</span>
      <input
        type="text"
        v-model="second_input"
        placeholder="Texto"
        class="input-field"
      />
      <button @click="convert(second_input as string)">Converter</button>
      <div>
        <h2>Resultado: {{ result }}</h2>
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

button {
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #2e2e2e;
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
