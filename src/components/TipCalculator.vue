<template>
  <div class="tipHolder">
    <!-- svg logo here -->
    <div class="tipContent">
      <div class="tipSection">
        <!-- Bill Input -->
        <label class="label"> Bill </label>
        <div class="customInput">
          <img
            src="@/assets/images/icon-dollar.svg"
            class="icon"
            alt="dollar-icon"
          />
          <input
            type="number"
            v-model.number="billInput"
            class="input"
            placeholder="0"
          />
        </div>

        <!-- Select Tip Section -->
        <label class="label"> Select Tip % </label>
        <div class="tips-section">
          <div
            v-for="(tip, index) in tips"
            class="tip"
            :class="activeTip == index ? 'selected' : ''"
            v-bind:key="index"
            @click="onTipClick(index)"
          >
            <span v-if="tip != ''"> {{ tip }}% </span>
            <input
              v-else
              class="input"
              v-model.number="customTip"
              placeholder="Custom"
              type="number"
            />
          </div>
        </div>

        <!-- Number of People Input -->
        <div class="labelError">
          <label class="label"> Number of People </label>
          <label
            v-if="!isValid(peopleNumber)"
            class="label error"
          >
            Can't be zero
          </label>
        </div>
        <div class="customInput">
          <img
            src="@/assets/images/icon-person.svg"
            class="icon"
            alt="person-icon"
          />
          <input
            type="number"
            v-model.number="peopleNumber"
            class="input"
            :class="!isValid(peopleNumber) ? 'error' : ''"
            placeholder="0"
            min="0"
          />
        </div>
      </div>
      <div class="tipSection">
        <div class="tipCalculatorSection">
          <div class="tipAmount">
            <div class="tipText">
              <span class="tipLabel">
                Tip Amount
              </span>
              <span class="tipLabelSecond">
                / person
              </span>
            </div>
            <span class="tipPrice">
              ${{calculator('tip')}}
            </span>
          </div>
          <div class="tipAmount">
            <div class="tipText">
              <span class="tipLabel">
                Tip Amount
              </span>
              <span class="tipLabelSecond">
                / person
              </span>
            </div>
            <span class="tipPrice">
              ${{calculator('total')}}
            </span>
          </div>
          <input type="button" value="Reset" :disabled="checkInputs()" class="resetBtn" @click="reset()">
            
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TipCalculator",
  components: {},
  props: {},
  data() {
    return {
      tips: ["5", "10", "15", "25", "50", ""],
      activeTip: null,
      billInput: null,
      customTip: null,
      peopleNumber: null,
      min: 0,
      max: 100,
      tipAmount: null
    };
  },
  methods: {
    onTipClick(index) {
      if (this.activeTip === index) {
        this.activeTip = null;
      } else {
        this.activeTip = index;
      }
    },
    isValid(value) {      
        return (value !== 0);
    },
    reset(){
      this.activeTip = null;
      this.customTip = null;
      this.billInput = null;
      this.peopleNumber = null;
      this.tipAmount = null;
    },
    checkInputs(){
      if((this.activeTip || this.customTip ) && this.billInput && this.peopleNumber){
        return false;
      }
      else {
        return true;
      }
    },
    calculator(type){
      var amount;
      if(type == 'tip') {
        if(!this.checkInputs()){
          var tipPercentage = parseInt(this.tips[this.activeTip],10);
          amount = ((this.billInput / this.peopleNumber) * (this.customTip || tipPercentage)) / 100;
          this.tipAmount = amount;
        }
        else {
          amount = '0.00';
        }
      }
      else {
        if(!this.checkInputs() && this.tipAmount){
          amount = (this.billInput / this.peopleNumber) + this.tipAmount ;
        }
        else {
          amount = '0.00';
        }
      }

      amount = amount.toString();
      amount = amount.slice(0, (amount.indexOf("."))+3)

      return amount;
    },
  },
};
</script>

<style scoped lang="scss">
.tipHolder {
  background-color: #ffffff;
  border-radius: 6px;
  width: 60%;
  display: flex;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0px 0px 28px 6px #88888824;

  @media (max-width:768px) {
      width: 100%;
    border-radius: 30px;
    position: unset;
    transform: translate(0);
  }

  .tipContent {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    padding: 20px;

    .tipSection {
        flex: 1 1 0;
        padding: 10px;
        margin: 20px;
    }
  }

  .label {
    color: hsl(186, 14%, 43%);
    font-weight: 700;
    font-size: 14px;
    display: inline-block;
    margin-top: 20px;

    &.error {
      color: #b67e75;
    }
  }

  .labelError {
    display: flex;
    justify-content: space-between;
  }

  .customInput {
    position: relative;
    display: flex;
    margin: 10px 0;

    .icon {
      position: absolute;
      left: 10px;
      top: 50%;
      z-index: 10;
      transform: translate(0%, -50%);
    }
  }

  .input:focus,
  .input:focus-visible {
    outline: none;
    border: none;
  }

  .input[type="number"]::-webkit-inner-spin-button,
  .input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  .input {
    // margin: 10px auto;
    border: 2px solid transparent;
    border-radius: 4px;
    background-color: hsl(189, 41%, 97%);
    width: 100%;
    padding: 5px 15px 5px 30px;
    font-size: 20px;
    font-weight: 700;
    font-family: "Space Mono", monospace;
    text-align: right;
    box-sizing: border-box;
    color: hsl(183, 100%, 15%);
    caret-color: hsl(185, 41%, 84%);

    &:hover,
    &:focus,
    &:focus-visible {
      border: 2px solid hsl(172, 67%, 45%) !important;
      border-radius: 4px !important;
    }

    &::placeholder {
      color: hsl(184, 14%, 56%);
    }

    &.error {
      border: 2px solid #b67e75 !important;
    }
  }

  .tips-section {
    display: flex;
    flex-wrap: wrap;
    width: 100%;

    .tip {
      flex: 30%;
      cursor: pointer;
      height: 40px;
      display: flex;
      margin: 5px;
      background-color: #00474b;
      border-radius: 4px;
      color: #f8ffff;
      text-align: center;
      font-weight: 700;
      font-size: 20px;

      @media (max-width:768px) {
        flex: 40%;
      }

      span {
        width: 100%;
        align-self: center;
      }

      &:hover {
        background-color: #9fe8df;
        color: #00474b;
      }

      &.selected {
        background-color: #27c1ad;
        color: #005254;
      }

      .tip-input {
        width: 100%;
        padding: 0;
        border: 0;
        height: 100%;
      }

      .input {
        width: 100%;
        padding: 0 10px;
        border: 2px solid transparent;
        height: 100%;
        margin: 0;
        border-radius: 0;
      }
    }
  }

  .tipCalculatorSection {
    height: 100%;
    background-color: #00474B;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    font-weight: 700;

    .tipAmount,.resetBtn {
      margin: 30px;
    }

    .tipAmount {
      display: flex;
      justify-content: space-between;
      align-items: center;

      .tipText {
        display: flex;
        flex-direction: column;
        font-size: 14px;

        .tipLabel {
          color: hsl(0, 0%, 100%);
        }

        .tipLabelSecond {
          color: hsl(186, 14%, 43%);
        }

      }
      
      .tipPrice {
        color: hsl(172, 67%, 45%);
        font-size: 38px;
      }

    }
    .resetBtn {
      background-color: #27c1ad;
      color: #005254;
      padding: 10px 20px;
      text-align: center;
      border-radius: 6px;
      text-transform: uppercase;
      font-family: "Space Mono", monospace;
      font-weight: 700;
      border: 0;
      font-size: 16px;

      &:not(:disabled) {
        cursor: pointer;
      }

      &:not(:disabled):hover {
        background-color: #9fe8df;
        color: #00474b;
      }

      &:disabled {
        opacity: 0.3;
      }
    }
  }
}
</style>