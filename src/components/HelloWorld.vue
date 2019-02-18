<template>
  <b-container>
    <h1>Syntactic analyzer of Python with Vue js</h1> 
    <div class="form-group">
      <textarea class="form-control txtcode" id="txtcode" v-model="code" rows="20" cols="100" spellcheck="false"></textarea>
    </div>
    <p>@Copyright --- Fredy Cortés</p>
  </b-container>
</template>

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
];
const numbers = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"];
const simbols = ["=", '"'];
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
      cont: 0,
      stack: [],
      accepted: false
    };
  },
  created() {
    this.$toastr.info("You can begin to program in Python", "Welcome");
    this.debouncedisAccepted = _.debounce(this.isAccepted(), 800);
  },
  watch: {
    code: function(newCode, oldCode) {
      this.isAccepted();
      this.debouncedisAccepted();
    }
  },
  methods: {
    isAccepted() {
      this.cont = 0;
      this.accepted = false;
      this.stack = [NT[0]];
      this.q0();
      if (this.accepted)
        document.getElementById("txtcode").style.color = "white";
      else {
        document.getElementById("txtcode").style.color = "red";
        this.$toastr.error("You can that have a error", "Faile");
      }
    },
    q0() {
      if (this.cont < this.code.length) {
        let s = this.stack.slice();
        let char = this.code[this.cont];
        this.cont++;
        let pop = s.pop();
        if (letters.indexOf(char) > -1 && pop == "<Z>") {
          this.stack.pop();
          this.stack.push("VAR");
          this.q0();
        } else if (letters.indexOf(char) > -1 && pop == "VAR") {
          this.q0();
        } else if (numbers.indexOf(char) > -1 && pop == "VAR") {
          this.q0();
        } else if (char == "=" && pop == "VAR") {
          this.stack.pop();
          this.stack.push("SIM");
          this.q1();
        }
      }
    },
    q1() {
      if (this.cont < this.code.length) {
        let s = this.stack.slice();
        let char = this.code[this.cont];
        this.cont++;
        let pop = s.pop();
        if (pop == "SIM") {
          if (numbers.indexOf(char) > -1) {
            this.stack.pop();
            this.stack.push("EXP");
            this.q2();
          } else if (char == '"') {
            this.stack.pop();
            this.stack.push("CAD");
            this.q3();
          } else if (char == "T" || char == "F") {
            this.stack.pop();
            if (char == "T") this.stack.push("<T>");
            if (char == "F") this.stack.push("<F>");
            this.q4();
          }
        }
      }
    },
    q2() {
      this.accepted = true;
      if (this.cont < this.code.length) {
        let s = this.stack.slice();
        let char = this.code[this.cont];
        this.cont++;
        let pop = s.pop();
        if (numbers.indexOf(char) > -1 && pop == "EXP") {
          this.stack.pop();
          this.stack.push("EXP");
          this.q2();
        } else if (numbers.indexOf(char) == -1) {
          this.accepted = false;
        }
      }
    },
    q3() {
      if (this.cont < this.code.length) {
        let s = this.stack.slice();
        let char = this.code[this.cont];
        this.cont++;
        let pop = s.pop();
        if (char == '"' && pop == "CAD") {
          this.stack.pop();
          this.stack.push("EXP");
          if (this.cont - 1 == this.code.length - 1) this.q5();
        } else if (letters.indexOf(char) > -1 && pop == "CAD") this.q3();
        else if (numbers.indexOf(char) > -1 && pop == "CAD") this.q3();
      }
    },
    q4() {
      if (this.cont < this.code.length) {
        let s = this.stack.slice();
        let char = this.code[this.cont];
        this.cont++;
        let pop = s.pop();
        if (char == "r" && pop == "<T>") {
          this.stack.pop();
          this.stack.push("<R>");
          this.q4();
        } else if (char == "u" && pop == "<R>") {
          this.stack.pop();
          this.stack.push("<U>");
          this.q4();
        } else if (char == "e" && pop == "<U>") {
          this.stack.pop();
          this.stack.push("EXP");
          if (this.cont - 1 == this.code.length - 1) this.q5();
        } else if (char == "a" && pop == "<F>") {
          this.stack.pop();
          this.stack.push("<A>");
          this.q4();
        } else if (char == "l" && pop == "<A>") {
          this.stack.pop();
          this.stack.push("<L>");
          this.q4();
        } else if (char == "s" && pop == "<L>") {
          this.stack.pop();
          this.stack.push("<S>");
          this.q4();
        } else if (char == "e" && pop == "<S>") {
          this.stack.pop();
          this.stack.push("EXP");
          if (this.cont - 1 == this.code.length - 1) this.q5();
        }
      }
    },
    q5() {
      let s = this.stack.slice();
      let char = this.code[this.cont];
      if (s.pop("EXP")) this.accepted = true;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @font-face {
      font-family: "varela round";
      src: url(../../public/fonts/Varela_Round/VarelaRound-Regular.ttf) format("truetype")
  }
  @font-face {
      font-family: "open sans";
      src: url(../../public/fonts/Open_Sans/OpenSans-ExtraBoldItalic.ttf) format("truetype")
  }
  body {
    font-family: "Varela Round";
  }
  h1, p {
    font-family: "Varela Round";
    font-weight: bolder;
  }
  h3 {
    margin: 30px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  .txtcode {
    background-color: #161616;
    color: white;
    border-radius: 8px;
    font-size: 16px;
  }
</style>
