<template>
    <div id="app">
            <div class="container-displayContext">
                <el-row style="text-align: right; font-size: 4em">
                    <el-col :span="24">
                        <div class="grid-content bg-purple-dark displayContext">
                            {{displayContext || 0}}
                        </div>
                    </el-col>
                </el-row>
            </div>
            <div class="caculator-context">
                <el-row :gutter="15">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('add')">+</el-button>
                    </el-col>
                    <el-col :span="6">
                        <el-button plain @click="handleInput('substract')">-</el-button>
                    </el-col>
                    <el-col :span="6">
                        <el-button plain @click="handleInput('multiple')">x</el-button>
                    </el-col>
                     <el-col :span="6">
                        <el-button plain @click="handleInput('divide')">/</el-button>
                    </el-col>
                </el-row>
                <el-row :gutter="15">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('7')">7</el-button>
                    </el-col>
                    <el-col :span="6">
                          <el-button plain @click="handleInput('8')">8</el-button>
                    </el-col>
                    <el-col :span="6">
                         <el-button plain @click="handleInput('9')">9</el-button>
                    </el-col>
                    <el-col :span="6">
                            <el-button style="background-color: #f4a064" plain @click="handleInput('AC')">AC</el-button>
                    </el-col>
                </el-row>
                <el-row :gutter="15">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('4')">4</el-button>
                    </el-col>
                    <el-col :span="6">
                          <el-button plain @click="handleInput('5')">5</el-button>
                    </el-col>
                    <el-col :span="6">
                         <el-button plain @click="handleInput('6')">6</el-button>
                    </el-col>
                    <el-col :span="6">
                         <el-button plain style="background-color: #f4a064" @click="handleInput('DEL')">DEL</el-button>
                    </el-col>
                </el-row>
                <el-row :gutter="15">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('1')">1</el-button>
                    </el-col>
                    <el-col :span="6">
                          <el-button plain @click="handleInput('2')">2</el-button>
                    </el-col>
                    <el-col :span="6">
                         <el-button plain @click="handleInput('3')">3</el-button>
                    </el-col>
                </el-row>
                <el-row :gutter="15">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('0')">0</el-button>
                    </el-col>
                    <el-col :span="6">
                          <el-button plain @click="handleInput('.')">.</el-button>
                    </el-col>
                    <el-col :span="12">
                            <el-button class="btn-red-light" plain @click="handleInput('=')">=</el-button>
                    </el-col>
                </el-row>
            </div>
    </div>

</template>

<script>
export default {
  data() {
    return {
      firstNumber: undefined,
      secondNumber: undefined,
      operator: undefined,
      result: undefined,
      tmpVal: undefined,
      displayContext: undefined,
    };
  },
  methods: {
    caculate(firstNumber, secondNumber, operator) {
      let result;

      if (firstNumber && secondNumber && operator) {
        switch (operator) {
          case "add":
            result = firstNumber + secondNumber;
            break;
          case "substract":
            result = firstNumber - secondNumber;
            break;
          case "multiple":
            result = firstNumber * secondNumber;
            break;
          case "divide":
            result = firstNumber / secondNumber;
            break;
          default:
            break;
        }
      }

      return result;
    },
    parseInputToNumber(input) {
      let output;

      if (input.includes(".")) {
        output = parseFloat(input);
      } else {
        output = parseInt(input);
      }

      return output;
    },
    handleInput(character) {
      const isOperator = inputString =>
        ["add", "substract", "multiple", "divide"].includes(inputString);

      const operatorMap = {
        add: "+",
        substract: "-",
        multiple: "x",
        divide: "/"
      };

      switch (character) {
        case "AC":
          this.resetContext();
          break;

        case "=":
          if (this.firstNumber && this.secondNumber && this.operator) {
            this.result = this.caculate(
              this.firstNumber,
              this.secondNumber,
              this.operator
            );
            this.setDisplayContext(this.result);
            console.log("result", this.result);
          }
          break;

        default:
          // if the last result is available
          if (this.result) {
            // and user input a operator
            // then assign last result to first number
            // and continue to caculated
            if (isOperator(character)) {
                this.firstNumber = this.result;
                this.secondNumber = undefined;
                this.operator = undefined;
                this.result = undefined;
                this.tmpVal = undefined;
                this.setDisplayContext(this.firstNumber);
            } else {
              // if new number is inputted
              // reset caculator context
              // then start new caculation
                this.resetContext();
            }
          }

          if (!this.operator && !this.secondNumber) {
            // set value for firstNumber
            if (!isOperator(character)) {
              this.firstNumber = this.getInputNumber(character);
              this.setDisplayContext(this.firstNumber);
              console.log("first number", this.firstNumber);
            } else if (this.firstNumber) {
              this.operator = character;
              this.tmpVal = undefined;
              this.setDisplayContext(operatorMap[this.operator]);
              console.log("operator", this.operator);
            }
          } else {
            // set value for secondNumber
            this.secondNumber = this.getInputNumber(character);
            console.log("second number", this.secondNumber);
            this.setDisplayContext(this.secondNumber);
          }
      }
    },
    setDisplayContext(displayContext) {
        this.displayContext = displayContext
    },
    resetContext() {
      this.firstNumber = undefined;
      this.secondNumber = undefined;
      this.operator = undefined;
      this.result = undefined;
      this.tmpVal = undefined;
      this.displayContext = undefined;
    },
    // use tmp variable to store input number
    getInputNumber(character) {
      if (this.tmpVal) {
        if (!(this.tmpVal.includes('.') && character === '.')) {
         this.tmpVal =
            this.tmpVal = (character === 'DEL') ? this.tmpVal.substring(0, this.tmpVal.length - 1) : this.tmpVal.concat(character);
        }
      } else {
        this.tmpVal = (character !== 'DEL') ? character : undefined;
      }

      const outputNumber = this.parseInputToNumber(this.tmpVal);

      return outputNumber;
    }
  }
};
</script>

<style>
  .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .el-button {
    width: 100%;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .btn-red-light {
    background-color:#e86f19;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
</style>
