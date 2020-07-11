<template>
  <div class="container mx-auto">
    <div class>
      <div class="grid grid-rows-2 bg-white ml-24 md:mr-24 md:pr-12 mr-6">
        <div class="h-12 text-5xl text-gray-900 pt-6 pb-3 pr-12 text-right align-middle">{{display}}</div>
        <div class="h-12 text-3xl pt-6 pb-12 pr-12 text-right align-middle">{{total}}</div>
      </div>
      <div class="grid grid-cols-7 col-gap-0 text-2xl text-white">
        <div class="md:col-span-1"></div>
        <div
          class="grid grid-flow-row grid-cols-3 grid-rows-3 col-span-4 gap-8 pt-12 pb-12 bg-gray-700"
        >
          <div class="action" @click="append('7')">7</div>
          <div class="action" @click="append('8')">8</div>
          <div class="action" @click="append('9')">9</div>
          <div class="action" @click="append('4')">4</div>
          <div class="action" @click="append('5')">5</div>
          <div class="action" @click="append('6')">6</div>
          <div class="action" @click="append('1')">1</div>
          <div class="action" @click="append('2')">2</div>
          <div class="action" @click="append('3')">3</div>
          <div class="action" @click="appendDot('.')">.</div>
          <div class="action" @click="append('0')">0</div>
          <div class="action" @click="getTotal()">=</div>
        </div>
        <div class="grid grid-rows-4 col-span-1 pt-12 pb-12 bg-gray-500">
          <div class="action text-300" @click="clear">C</div>
          <div class="action" @click="appendSign('-')">-</div>
          <div class="action" @click="appendSign('x')">x</div>
          <div class="action" @click="appendSign('+')">+</div>
          <div class="action" @click="appendSign('/')">/</div>
        </div>
        <div class="grid col-span-1 bg-yellow-600 lg:mr-24 mr-8"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      display: "",
      work_area: "",
      total: "",
      activeOperator: false,
      previous: "",
      activeOperation: ""
    };
  },
  methods: {
    clear() {
      this.work_area = "";
      this.display = "";
      this.previous = "";
      this.total = "";
      this.activeOperation = "";
      this.activeOperator = false;
    },
    append(number) {
      if(!this.activeOperation || parseFloat(number)){

        this.display = this.display + number;
        if (!this.activeOperator) {
          this.work_area = this.work_area + number;
          this.operation();
        }
      }
    },
    appendDot(symbol) {
      if (this.work_area.indexOf(".") === -1) {
        this.append(symbol);
      }
    },
    appendSign(sign) {
      this.previous = this.total !== ""  ? this.total : this.work_area;
      this.work_area = "";
      this.activeOperator = true;
      this.append(sign);
      this.activeOperation = sign;
      this.activeOperator = false;
    },
    operation() {
      switch (this.activeOperation) {
        case "x":
          this.multiply();
          break;
        case "+":
          this.add();
          break;
        case "-":
          this.subtract();
          break;
        case "/":
          this.divide();
          break;
        default:
          break;
      }
    },
    multiply() {
      this.total = parseFloat(this.previous || 0) * parseFloat(this.work_area);
      this.activeOperation = "";
    },
    add() {
      this.total = parseFloat(this.previous || 0) + parseFloat(this.work_area);
      this.activeOperation = "";
    },
    subtract() {
      this.total = parseFloat(this.previous || 0) - parseFloat(this.work_area);
      this.activeOperation = "";
    },
    divide() {
      this.total = parseFloat(this.previous || 0) / parseFloat(this.work_area);
      this.activeOperation = "";
    },
    getTotal() {
      if(this.total === "") {
        return;
      }
      else {
        this.display = this.total;
        this.work_area = this.total;
        this.total = "";
      }
    }
  }
};
</script>

<style scoped>
  .action {
    cursor: pointer;
}
</style>