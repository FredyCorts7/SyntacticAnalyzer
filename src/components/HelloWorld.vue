<template>
  <b-container fluid>
    <br>
    <b-row class="justify-content-md-center" align-v="center" cols="12">
      <b-col sm="6" lg="6" xl="5">
        <p>Te recomendamos aprender Python desde cero, de una forma sencilla y muy práctica.</p>
        <iframe width="95%" height="260" src="https://www.youtube-nocookie.com/embed/G2FCfQj-9ig" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
      </b-col>
      <b-col sm="6" lg="6" xl="6">
        <b-form-textarea
          ref="refcode"
          :state=accepted
          id="txtcode"
          v-model="code"
          class="white-color"
          placeholder="#Start to write code"
          rows="15"
        />
      </b-col>
    </b-row>
    <br>
    <p>&#169;Copyright</p>
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
  #txtcode {
    font-family: "varela round";
    font-size: 14px;
    background-color: #161616 !important
  }
  .red-color {
    color: #ff4b4b !important;
  }
  .white-color {
    color: white !important;
  }
</style>

<script>
  const letters = [ //letras minusculas
    'q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p', 'a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l', 'ñ', 'z', 'x', 'c', 'v', 'b',
    'n', 'm', 'Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P', 'A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L', 'Ñ', 'Z', 'X', 'C',
    'V', 'B', 'N', 'M', '_'
  ]
  const numbers = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
  const simbols = ['=', '"']
  const simbolscondition = ['=', '<', '>', '!']
  const T = [ //alfabeto de simbolos terminales
    letters, numbers, simbols
  ]
  export default {
    data () {
      return {
        code: '',
        cont: 0,
        stack: [],
        accepted: false
      }
    },
    created () {
      this.$toastr.info('You can begin to program in Python', 'Welcome')
      this.debouncedisAccepted = _.debounce(this.isAccepted, 100)
    },
    watch: {
        code: function (newCode, oldCode) {
            this.debouncedisAccepted()
        }
    },
    methods: {
      isAccepted: function() {
        this.cont = 0
        this.accepted = false
        this.stack = ['Z']
        this.q0()
        if (this.accepted) {
          this.$refs.refcode.$el.classList.remove('red-color')
          this.$refs.refcode.$el.classList.add('white-color')
          this.$toastr.success("Ready to compile", "Excellent")
        } else {
          this.$refs.refcode.$el.classList.remove('white-color')
          this.$refs.refcode.$el.classList.add('red-color')
          this.$toastr.error("Still not ready to compile", "Faile")
        }

        /*Probando que el salto de linea se registre
        for (let i = 0; i < this.code.length; i++) {
          if(this.code[i] == "/n") this.$toastr.info("Acabas de hacer un /n", "Information")
        }*/
      },
      q0 () {
        if (this.cont <= this.code.length) {
          let s = this.stack.slice()
          let char = this.code[this.cont]
          this.cont++
          let pop = s.pop()
          if (letters.indexOf(char) > -1 && pop == 'Z' && char != 'i') {
            this.stack.pop()
            this.stack.push('A')
            this.stack.push('V')
            this.q0()
          } else if (letters.indexOf(char) > -1 && pop == 'V') {
            this.q0()
          } else if (letters.indexOf(char) > -1 && pop == 'C') {
            this.stack.pop()
            this.stack.push('O')
            this.q0()
          } else if (letters.indexOf(char) > -1 && pop == 'O') {
            this.q0()
          } else if (numbers.indexOf(char) > -1 && pop == 'O') {
            this.q0()
          } else if (numbers.indexOf(char) > -1 && pop == 'V') {
            this.q0()
          } else if (numbers.indexOf(char) > -1 && pop == 'C') {
            this.stack.pop()
            this.stack.push('N')
            this.q0()
          } else if (numbers.indexOf(char) > -1 && pop == 'N') {
            this.q0()
          } else if (char == ' ' && pop == 'Z') {
            this.q0()
          } else if (char == ' ' && pop == 'V') {
            this.stack.pop()
            this.stack.push('B')
            this.q0()
          } else if (char == ' ' && pop == 'B') {
            this.q0()
          } else if (char == ' ' && pop == 'C') {
            this.stack.pop()
            this.stack.push('O')
            this.q0()
          } else if (char == ' ' && pop == 'N') {
            this.stack.pop()
            this.stack.push('O')
            this.q0()
          } else if (char == ' ' && pop == 'O') {
            this.q0()
          } else if (char == 'i' && pop == 'Z') {
            this.stack.pop()
            this.stack.push('I')
            this.q0()
          } else if (letters.indexOf(char) > -1 && pop == 'I' && char != 'f') {
            this.stack.pop()
            this.stack.push('A')
            this.stack.push('V')
            this.q0()
          } else if (char == 'f' && pop == 'I') {
            this.stack.pop()
            this.stack.push('C')
            this.stack.push('F')
            this.q2()
          } else if (char == '<' && pop == 'N') {
            this.stack.pop()
            this.stack.push('I')
            this.q1()
          } else if (char == '>' && pop == 'N') {
            this.stack.pop()
            this.stack.push('D')
            this.q1()
          } else if (char == '!' && pop == 'N') {
            this.stack.pop()
            this.stack.push('O')
            this.q1()
          } else if (char == '=' && pop == 'N') {
            this.stack.pop()
            this.stack.push('O')
            this.q1()
          } else if (char == '!' && pop == 'O') {
            this.q1()
          } else if (char == '=' && pop == 'B') {
            this.stack.pop()
            this.stack.push('S')
            this.q1()
          } else if (char == '=' && pop == 'V') {
            this.stack.pop()
            this.stack.push('S')
            this.q1()
          } else if (char == '=' && pop == 'O') {
            this.q1()
          } else if (char == '>' && pop == 'O') {
            this.stack.pop()
            this.stack.push('D')
            this.q1()
          } else if (char == '<' && pop == 'O') {
            this.stack.pop()
            this.stack.push('I')
            this.q1()
          }
        }
      },
      q1 () {
        if (this.cont <= this.code.length) {
          let s = this.stack.slice()
          let char = this.code[this.cont]
          this.cont++
          let pop = s.pop()
          if (char == '=' && pop == 'I') {
            this.stack.pop()
            this.stack.push('D')
            this.q1()
          } else if (char == '=' && pop == 'D') {
            this.q1()
          } else if (char == '=' && pop == 'O') {
            this.stack.pop()
            this.stack.push('S')
            this.q1()
          } else if (char == ' ' && pop == 'S') {
            this.stack.pop()
            this.stack.push('B')
            this.q1()
          } else if (char == ' ' && pop == 'B') {
            this.q1()
          } else if (char == ' ' && pop == 'D') {
            this.stack.pop()
            this.stack.push('B')
            this.q1()
          } else if (numbers.indexOf(char) > -1 && pop == 'D') {
            this.stack.pop()
            this.stack.push('E')
            this.q3()
          } else if (numbers.indexOf(char) > -1 && pop == 'I') {
            this.stack.pop()
            this.stack.push('E')
            this.q3()
          } else if (numbers.indexOf(char) > -1 && pop == 'S') {
            this.stack.pop()
            this.stack.push('E')
            this.q3()
          } else if (numbers.indexOf(char) > -1 && pop == 'B') {
            this.stack.pop()
            this.stack.push('E')
            this.q3()
          } else if (char == '"' && pop == 'B') {
            this.stack.pop()
            this.stack.push('C')
            this.q4()
          } else if (char == '"' && pop == 'S') {
            this.stack.pop()
            this.stack.push('C')
            this.q4()
          } else if (char == "'" && pop == 'B') {
            this.stack.pop()
            this.stack.push('C')
            this.q4()
          } else if (char == "'" && pop == 'S') {
            this.stack.pop()
            this.stack.push('C')
            this.q4()
          } else if (char == 'T' && pop == 'S') {
            this.stack.pop()
            this.stack.push('T')
            this.q5()
          } else if (char == 'T' && pop == 'B') {
            this.stack.pop()
            this.stack.push('T')
            this.q5()
          } else if (char == 'F' && pop == 'S') {
            this.stack.pop()
            this.stack.push('F')
            this.q5()
          } else if (char == 'F' && pop == 'B') {
            this.stack.pop()
            this.stack.push('F')
            this.q5()
          } else if (char == 'F' && pop == 'D') {
            this.stack.pop()
            this.stack.push('F')
            this.q5()
          } else if (char == 'T' && pop == 'D') {
            this.stack.pop()
            this.stack.push('T')
            this.q5()
          }
        }
      },
      q2 () {
        if (this.cont <= this.code.length) {
          let s = this.stack.slice()
          let char = this.code[this.cont]
          this.cont++
          let pop = s.pop()
          if (char == ' ' && pop == 'F') {
            this.stack.pop()
            this.stack.push('C')
            this.q0()
          }
        }
      },
      q3 () {
        if (this.cont <= this.code.length) {
          let s = this.stack.slice()
          let char = this.code[this.cont]
          this.cont++
          let pop = s.pop()
          let pop2 = s.pop()
          if (numbers.indexOf(char) > -1 && pop == 'E') {
            this.q3()
          } else if (char == ' ' && pop == 'E') {
            this.stack.pop()
            this.stack.push('A')
            this.q3()
          } else if (char == ' ' && pop == 'A') {
            this.q3()
          } else if (char == '\n' && pop == 'E' && pop2 == 'A') {
            this.stack.pop()
            this.stack.pop()
            this.stack.push('Z')
            this.q0()
          } else if (char == '\n' && pop == 'A' && pop2 == 'A') {
            this.stack.pop()
            this.stack.pop()
            this.stack.push('Z')
            this.q0()
          } else if (char == undefined && pop == 'E' && pop2 == "A") {
            this.stack.pop()
            this.stack.pop()
            this.q6()
          } else if (char == undefined && pop == 'A' && pop2 == "A") {
            this.stack.pop()
            this.stack.pop()
            this.q6()
          } else if (char == ':' && pop == 'E' && pop2 == "C") {
            this.stack.pop()
            this.stack.pop()
            this.stack.push('C')
            this.q9()
          } else if (char == ':' && pop == 'A' && pop2 == "C") {
            this.stack.pop()
            this.stack.pop()
            this.stack.push('C')
            this.q9()
          }
        }
      },
      q4 () {
        if (this.cont <= this.code.length) {
          let s = this.stack.slice()
          let char = this.code[this.cont]
          this.cont++
          let pop = s.pop()
          let pop2 = s.pop()
          if (char == ' ' && pop == 'E') {
            this.q4()
          } else if (char != "'" && char != '"' && pop == 'C') {
            this.q4()
          } else if (char == '"' && pop == 'C') {
            this.stack.pop()
            this.stack.push('E')
            this.q4()
          } else if (char == "'" && pop == 'C') {
            this.stack.pop()
            this.stack.push('E')
            this.q4()
          } else if (char == undefined && pop == 'E' && pop2 == 'A') {
            this.stack.pop()
            this.stack.pop()
            this.q6()
          } else if (char == ':' && pop == 'E' && pop2 == 'C') {
            this.stack.pop()
            this.stack.pop()
            this.stack.push('C')
            this.q7()
          } else if (char == '\n' && pop == 'E' && pop2 == 'A') {
            this.stack.pop()
            this.stack.pop()
            this.stack.push('Z')
            this.q0()
          }
        }
      },
      q5 () {
        if (this.cont <= this.code.length) {
          let s = this.stack.slice()
          let char = this.code[this.cont]
          this.cont++
          let pop = s.pop()
          let pop2 = s.pop()
          if (char == 'r' && pop == 'T') {
            this.stack.pop()
            this.stack.push('R')
            this.q5()
          } else if (char == 'u' && pop == 'R') {
            this.stack.pop()
            this.stack.push('U')
            this.q5()
          } else if (char == 'e' && pop == 'U') {
            this.stack.pop()
            this.stack.push('E')
            this.q5()
          } else if (char == 'a' && pop == 'F') {
            this.stack.pop()
            this.stack.push('A')
            this.q5()
          } else if (char == 'l' && pop == 'A') {
            this.stack.pop()
            this.stack.push('L')
            this.q5()
          } else if (char == 's' && pop == 'L') {
            this.stack.pop()
            this.stack.push('S')
            this.q5()
          } else if (char == 'e' && pop == 'S') {
            this.stack.pop()
            this.stack.push('E')
            this.q5()
          } else if (char == ' ' && pop == 'E') {
            this.stack.pop()
            this.stack.push('E')
            this.q5()
          } else if (char == undefined && pop == 'E' && pop2 == 'A') {
            this.stack.pop()
            this.stack.pop()
            this.q6()
          } else if (char == '\n' && pop == 'E' && pop2 == 'A') {
            this.stack.pop()
            this.stack.pop()
            this.stack.push('Z')
            this.q0()
          }
        }
      },
      q6 () {
        //Este es el estado de aceptación
        this.accepted = true
      },
      q7 () {
        if (this.cont <= this.code.length) {
          let s = this.stack.slice()
          let char = this.code[this.cont]
          this.cont++
          let pop = s.pop()
          if (char == '\n' && pop == 'C') {
            this.q8()
          }
        }
      },
      q8 () {
        if (this.cont <= this.code.length) {
          let s = this.stack.slice()
          let char = this.code[this.cont]
          this.cont++
          let pop = s.pop()
          if (char == ' ' && pop == 'C') {
            this.stack.pop()
            this.stack.push('Z')
            this.q0()
          }
        }
      },
      q9 () {
        if (this.cont <= this.code.length) {
          let s = this.stack.slice()
          let char = this.code[this.cont]
          this.cont++
          let pop = s.pop()
          if (char == '\n' && pop == 'C') {
            this.q8()
          }
        }
      }
    }
  }
</script> 

<!-- Add "scoped" attribute to limit CSS to this component only -->