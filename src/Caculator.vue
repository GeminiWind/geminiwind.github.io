<template>
    <div id="app">
            <CaculatorDisplay :displayContext="displayContext"></CaculatorDisplay>
            <div class="caculator-context">
                <el-row :gutter="15">
                  <CaculatorBtn btnValue="add" btnLabel="+" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="substract" btnLabel="-" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="multiple" btnLabel="x" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="divide" btnLabel="/" span=6 @press="handleInput($event)"></CaculatorBtn>
                </el-row>
                <el-row :gutter="15">
                  <CaculatorBtn btnValue="7" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="8" span=6  @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="9" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="AC" span=6 @press="handleInput($event)"></CaculatorBtn>
                </el-row>
                <el-row :gutter="15">
                  <CaculatorBtn btnValue="4" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="5" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="6" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="DEL" span=6 @press="handleInput($event)"></CaculatorBtn>
                </el-row>
                <el-row :gutter="15">
                  <CaculatorBtn btnValue="1" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="2" span=6 @press="handleInput($event)"></CaculatorBtn>
                  <CaculatorBtn btnValue="3" span=6 @press="handleInput($event)"></CaculatorBtn>
                </el-row>
                <el-row :gutter="15">
                    <CaculatorBtn btnValue="0" span=6 @press="handleInput($event)"></CaculatorBtn>
                    <CaculatorBtn btnValue="." span=6 @press="handleInput($event)"></CaculatorBtn>
                    <CaculatorBtn btnValue="=" span=12 @press="handleInput($event)"></CaculatorBtn>
                </el-row>
            </div>
    </div>
</template>

<script>
import CaculatorBtn from "./CaculatorBtn";
import CaculatorDisplay from "./CaculatorDisplay";

export default {
  components: {
    CaculatorBtn,
    CaculatorDisplay
  },
  data() {
    return {
      firstNumber: undefined,
      secondNumber: undefined,
      operator: undefined,
      result: undefined,
      tmpVal: undefined,
      displayContext: undefined
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
      this.displayContext = displayContext;
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
        if (!(this.tmpVal.includes(".") && character === ".")) {
          this.tmpVal = this.tmpVal =
            character === "DEL"
              ? this.tmpVal.substring(0, this.tmpVal.length - 1)
              : this.tmpVal.concat(character);
        }
      } else {
        this.tmpVal = character !== "DEL" ? character : undefined;
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
  background-color: #e86f19;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
</style>
