<template>
  <div class="screen">
    <h1>BREN.OOO</h1>

    <div class="container">
      <div class="gameplay">
        <div v-on:keyup.enter="onEnter()" v-for="x in 6" v-bind:key="x">
          <div class="inputContainer">
            <input
              v-for="n in 5"
              v-bind:key="n"
              :id="'input' + x + n"
              type="text"
              @keydown="focusHandler($event)"
              v-on:keyup.backspace="deleteHandler()"
              maxlength="1"
              pattern="[A-Za-z]"
            />
          </div>
        </div>
      </div>
    </div>
    <h2 v-if="acertou">Você acertou!</h2>
    <div class="keyboard">
      <div class="keyboardContent">
        <div class="keyboardDiv" v-for="t in 9" :id="t" v-bind:key="t">
          {{ keyboardContent[t] }}
        </div>
      </div>
      <div class="keyboardContent">
        <div class="keyboardDiv" v-for="t in 9" :id="t + 9" v-bind:key="t">
          {{ keyboardContent[t + 9] }}
        </div>
      </div>
      <div class="keyboardContentLast">
        <div class="keyboardDiv" v-for="t in 7" :id="t + 18" v-bind:key="t">
          {{ keyboardContent[t + 18] }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      answer: "breno",
      auxX: 1,
      auxN: 0,
      auxConcat: 0,
      letter: ["", "", "", "", ""],
      correctLetter: false,
      acertou: false,
      keyboardContent: [
        "q",
        "w",
        "e",
        "r",
        "t",
        "y",
        "u",
        "i",
        "o",
        "p",
        "a",
        "s",
        "d",
        "f",
        "g",
        "h",
        "j",
        "k",
        "l",
        "z",
        "x",
        "c",
        "v",
        "b",
        "n",
        "m",
      ],
    };
  },
  mounted() {
    document.getElementById("input11").focus();
    for (let i = 2; i <= 6; i++) {
      for (let j = 1; j < 6; j++) {
        let toDisable = document.getElementById("input" + i + j);
        toDisable.disabled = true;
      }
    }
  },

  methods: {
    focusHandler(e) {
      const value = e.target.value;

      if (e.key === "Backspace") {
        return null;
      } else {
        if (this.auxN < 5) {
          this.letter.splice(this.auxN - 1, 1, value);

          this.auxN++;
        } else if (this.auxN === 5) {
          this.letter.splice(this.auxN - 1, 1, value);
        }
        this.auxConcat = `${this.auxX}${this.auxN}`;
        document.getElementById("input" + this.auxConcat).focus();
      }
    },
    deleteHandler() {
      if (this.auxN === 5) {
        this.letter.splice(this.auxN - 1, 1, "");
      } else {
        this.letter.splice(this.auxN, 1, "");
      }
      if (this.auxN < 1) {
        return null;
      } else {
        this.auxN--;
        if (this.auxN === 0) {
          this.auxConcat = `${this.auxX}${this.auxN + 1}`;
        } else {
          this.auxConcat = `${this.auxX}${this.auxN}`;
        }
        document.getElementById("input" + this.auxConcat).focus();
      }
    },

    onEnter() {
      const answerSplitted = this.answer.split("");

      for (let i = 0; i < answerSplitted.length; i++) {
        let aux = i + 1;
        let isIncluded = this.answer.includes(this.letter[i]);

        if (answerSplitted[i] != this.letter[i] && isIncluded == false) {
          let inputContent = document.getElementById("input" + this.auxX + aux);
          inputContent.style.backgroundColor = "#2c3e50";
        } else if (answerSplitted[i] == this.letter[i]) {
          this.correctLetter = true;

          let aux = i + 1;
          let inputContent = document.getElementById("input" + this.auxX + aux);
          inputContent.style.backgroundColor = "#2ecc71";
        } else if (answerSplitted[i] != this.letter[i] && isIncluded == true) {
          let inputContent = document.getElementById("input" + this.auxX + aux);
          inputContent.style.backgroundColor = "#f0932b";
        }
      }
      for (let j = 1; j < 6; j++) {
        let toDisableAgain = document.getElementById("input" + this.auxX + j);
        toDisableAgain.disabled = true;
      }
      this.auxX++;
      this.auxN = 0;

      try {
        if (this.acertou != true) {
          for (let j = 1; j < 6; j++) {
            let toDisableAgain = document.getElementById(
              "input" + this.auxX + j
            );
            toDisableAgain.disabled = true;
            let toEnlable = document.getElementById("input" + this.auxX + j);
            toEnlable.disabled = false;
          }
        }
      } catch (e) {
        return null;
      }

      this.auxConcat = `${this.auxX}${this.auxN + 1}`;
      document.getElementById("input" + this.auxConcat).focus();
      this.letter = ["", "", "", "", ""];

      console.log(this.correctLetter);
    },
  },
};
</script>

<style>
@import "../assets/game.css";
</style>
