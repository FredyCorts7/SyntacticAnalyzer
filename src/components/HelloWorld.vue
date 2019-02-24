<template>
  <b-container>
    <b-row class="justify-content-md-center" align-v="center">
      <b-col sm="6" lg="6" xl="6">
        <b-form-textarea
          id="txtcode"
          v-model="code"
          placeholder="#Start to write code"
          rows="15"
        />
      </b-col>
      <b-col sm="5" lg="5" xl="5">
        <p>Te recomendamos aprender Python desde cero, de una forma sencilla y muy práctica.</p>
        <iframe width="100%" height="300" src="https://www.youtube-nocookie.com/embed/G2FCfQj-9ig" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
      </b-col>
      <p>&#169;Copyright</p>
    </b-row>
  </b-container>
</template>

<style>
  @font-face {
      font-family: "varela round";
      src: url(../../public/fonts/Varela_Round/VarelaRound-Regular.ttf) format("truetype")
  }
  body {
    font-family: "varela round"
  }
  h1, p {
    font-weight: bolder;
    font-family: "varela round"
  }
  #txtcode {
    font-family: "varela round" !important;
    font-size: 12px !important;
    background-color: #161616 !important;
    color: white !important;
    border-radius: 15px !important;
  }
</style>

<script>
const letters = [
  //letras minusculas
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
  "ñ",
  "z",
  "x",
  "c",
  "v",
  "b",
  "n",
  "m",
  "Q",
  "W",
  "E",
  "R",
  "T",
  "Y",
  "U",
  "I",
  "O",
  "P",
  "A",
  "S",
  "D",
  "F",
  "G",
  "H",
  "J",
  "K",
  "L",
  "Ñ",
  "Z",
  "X",
  "C",
  "V",
  "B",
  "N",
  "M",
  "_"
]
const numbers = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"]
const simbols = ["=", '"']
const T = [
  //alfabeto de simbolos terminales
  letters,
  numbers,
  simbols
];
const NT = [
  //alfabeto de simbolos no terminales
  "<Z>",
  "VAR",
  "EXP",
  "SIM",
  "CAD",
  "<T>",
  "<R>",
  "<U>",
  "<A>",
  "<L>",
  "<S>"
];
export default {
  data() {
    return {
      code: "",
      code2: "",
      cont: 0,
      stack: [],
      accepted: false
    };
  },
  created() {
    this.$toastr.success("You can begin to program in Python", "Welcome")
    this.debouncedisAccepted = _.debounce(this.isAccepted(), 800)
  },
  watch: {
    code: function(newCode, oldCode) {
      this.isAccepted()
      this.debouncedisAccepted()
    }
  },
  methods: {
    isAccepted() {
      this.cont = 0
      this.accepted = false
      this.stack = [NT[0]]
      this.code2 = this.code
      this.q6()
      /*let lines = this.code2.split("\n")
      console.log(lines)
      for (let i = 0; i < lines.length; i++) {
        this.code2 = lines[i]
        this.stack = [NT[0]]
        this.cont = 0
        this.q0()
      }*/
      if (this.accepted) document.getElementById("txtcode").style.color = "white !important";
      else {
        document.getElementById("txtcode").style.color = "red !important";
        this.$toastr.error("You can that have a error", "Faile");
      }
    },
    q0() {
      if (this.cont < this.code2.length) {
        let s = this.stack.slice()
        let char = this.code2[this.cont]
        this.cont++
        let pop = s.pop()
        if (letters.indexOf(char) > -1 && pop == "<Z>") {
          this.stack.pop()
          this.stack.push("VAR")
          this.q0()
        } else if (letters.indexOf(char) > -1 && pop == "VAR") {
          this.q0()
        } else if (numbers.indexOf(char) > -1 && pop == "VAR") {
          this.q0()
        } else if (char == "=" && pop == "VAR") {
          this.stack.pop()
          this.stack.push("SIM")
          this.q1()
        }else if (char == "=" && pop == "<>") {
          this.stack.pop()
          this.stack.push("SIM")
          this.q1()
        } else if (char == " " && pop == "VAR") {
          this.stack.pop()
          this.stack.push("<>")
          this.q0()
        }else if (char == " " && pop == "SIM") {
          this.stack.pop()
          this.stack.push("<>")
          this.q1()
        } else if (char == " " && pop == "<>") {
          this.q0()
        } else if (char == "f" && pop == "V") {
          this.stack.pop()
          this.stack.push("<F>")
          this.q6()
        }
      }
    },
    q1() {
      if (this.cont < this.code2.length) {
        let s = this.stack.slice()
        let char = this.code2[this.cont]
        this.cont++
        let pop = s.pop()
        if (pop == "SIM" || pop == "<>") {
          if (numbers.indexOf(char) > -1) {
            this.stack.pop()
            this.stack.push("EXP");
            this.q2()
          } else if (char == '"') {
            this.stack.pop()
            this.stack.push("CAD");
            this.q3()
          } else if (char == "T" || char == "F") {
            this.stack.pop()
            if (char == "T") this.stack.push("<T>")
            if (char == "F") this.stack.push("<F>")
            this.q4()
          } else if (char == " ") {
            this.q1()
          }
        }
      }
    },
    q2() {
      this.accepted = true
      if (this.cont < this.code2.length) {
        let s = this.stack.slice()
        let char = this.code2[this.cont]
        this.cont++
        let pop = s.pop()
        if (numbers.indexOf(char) > -1 && pop == "EXP") {
          this.stack.pop()
          this.stack.push("EXP")
          this.q2()
        } else if (numbers.indexOf(char) == -1) {
          this.accepted = false
        }
      }
    },
    q3() {
      if (this.cont < this.code2.length) {
        let s = this.stack.slice()
        let char = this.code2[this.cont]
        this.cont++
        let pop = s.pop()
        if (char == '"' && pop == "CAD") {
          this.stack.pop()
          this.stack.push("EXP")
          if (this.cont - 1 == this.code2.length - 1) this.q5()
        } else if (letters.indexOf(char) > -1 && pop == "CAD") this.q3()
        else if (numbers.indexOf(char) > -1 && pop == "CAD") this.q3()
      }
    },
    q4() {
      if (this.cont < this.code2.length) {
        let s = this.stack.slice()
        let char = this.code2[this.cont]
        this.cont++
        let pop = s.pop()
        if (char == "r" && pop == "<T>") {
          this.stack.pop()
          this.stack.push("<R>")
          this.q4()
        } else if (char == "u" && pop == "<R>") {
          this.stack.pop()
          this.stack.push("<U>")
          this.q4()
        } else if (char == "e" && pop == "<U>") {
          this.stack.pop()
          this.stack.push("EXP")
          if (this.cont - 1 == this.code2.length - 1) this.q5()
        } else if (char == "a" && pop == "<F>") {
          this.stack.pop()
          this.stack.push("<A>")
          this.q4()
        } else if (char == "l" && pop == "<A>") {
          this.stack.pop()
          this.stack.push("<L>")
          this.q4()
        } else if (char == "s" && pop == "<L>") {
          this.stack.pop()
          this.stack.push("<S>")
          this.q4()
        } else if (char == "e" && pop == "<S>") {
          this.stack.pop()
          this.stack.push("EXP")
          if (this.cont - 1 == this.code2.length - 1) this.q5()
        }
      }
    },
    q5() {
      let s = this.stack.slice()
      let char = this.code2[this.cont]
      if (s.pop("EXP")) this.accepted = true
    },
    q6() {
      if (this.cont < this.code2.length) {
        let s = this.stack.slice()
        let char = this.code2[this.cont]
        this.cont++
        let pop = s.pop()
        if (char == " " && pop == "<F>") {
          this.stack.pop()
          this.stack.push("<>")
          this.q0()
        }
      }
    },
    q7() {
      if (this.cont < this.code2.length) {
        
      }
    }
  }
}
</script> 

<!-- Add "scoped" attribute to limit CSS to this component only -->