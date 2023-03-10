<template>
  <main>
    <p class="title">SPLI</p>
    <p class="title">TTER</p>
    <div class="calculator-box">
      <div class="left-box">
        <NumberInput 
        title="Bill" 
        :billNumber="billValue" 
        @change-bill-value="changeBillValue"
        :key="reload"
        />
        <div class="tip-select-box upper-margin-lg">
          <p>Select tip %</p>
          <div class="tip-select-grid upper-margin-sm">
            <button class="tip-select tip-button-active" id="5" @click="setActive($event.target.id)">5%</button>
            <button class="tip-select tip-button-inactive" id="10" @click="setActive($event.target.id)">10%</button>
            <button class="tip-select tip-button-inactive" id="15" @click="setActive($event.target.id)">15%</button>
            <button class="tip-select tip-button-inactive" id="25" @click="setActive($event.target.id)">25%</button>
            <button class="tip-select tip-button-inactive" id="50" @click="setActive($event.target.id)">50%</button>
            <!-- <button class="tip-select">10%</button> -->
            <input type="text" class="tip-select tip-input" placeholder="Custom" id="custom" @click="setActive($event.target.id)">
          </div>
        </div>
        <PeopleInput 
        title="Number of people" 
        :peopleNumber="peopleValue" 
        @change-people-value="changePeopleValue" 
        class="upper-margin-lg"
        :key="reload"
        />
      </div>
      <div class="right-box">
        <div class="right-upper-box">
          <div class="summary-box">
            <div class="summary-left">
              <p class="summary-title">Tip Amount</p>
              <p class="summary-subcaption">/ person</p>
            </div>
            <div class="summary-right">
              <h1>${{ tipAmount.toFixed(2) }}</h1>
            </div>
          </div>
          <div class="summary-box upper-margin-lg">
              <div class="summary-left">
                <p class="summary-title">Total</p>
                <p class="summary-subcaption">/ person</p>
              </div>
              <div class="summary-right">
                <h1>${{ total.toFixed(2) }}</h1>
              </div>
            </div>
        </div>
        <div class="right-lower-box">
          <button class="reset-btn" @click="resetCalculator">Reset</button>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
document.title = 'Tip Calculator';

// @ is an alias to /src
import NumberInput from '@/components/NumberInput.vue';
import PeopleInput from '@/components/PeopleInput.vue';

export default {
  data(){
    return{
      activeBtn: 5,
      billValue: 0,
      peopleValue: 0,
      tipAmount: 0,
      total: 0,
      reload: 0
    }
  },
  components: {
    NumberInput,
    PeopleInput
  },
  methods: {
    setActive(id){
      this.activeBtn = id;

      let tipButtons = document.querySelectorAll(".tip-select");
      tipButtons.forEach((button)=>{
        if(button.id == id){
          button.classList.add('tip-button-active');
          button.classList.remove('tip-button-inactive');
        }else{
          button.classList.add('tip-button-inactive');
          button.classList.remove('tip-button-active');
        }
      })
    },
    changeBillValue(newValue){
      this.billValue = newValue;
    },
    changePeopleValue(newValue){
      this.peopleValue = newValue;
    },
    resetCalculator(){
      this.activeBtn = 5;
      this.billValue = 0;
      this.peopleValue = 0;
      this.tipAmount = 0;
      this.total = 0;
      this.reload = this.reload + 1; // reload people and bill components
    },
    calculateValue(){
      console.log('calculate'); // <------ calculate value function (detect NOnumbers in inputs!)
    }
  },
  watch: {
    activeBtn(){
      this.calculateValue();
    },
    billValue(){
      this.calculateValue();
    },
    peopleValue(){
      this.calculateValue();
    }
  }
}

</script>

<style>

  p{
    font-weight: var(--font-weight);
    font-size: var(--font-desc);
    color: var(--neutral-2);
  }

  main{
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .title{
    letter-spacing: 10px;
    font-size: var(--font-input);
    color: var(--neutral-1);
  }

  .calculator-box{
    background-color: var(--white );
    width: 60vw;
    height: auto;
    /* height: 400px; */
    padding: var(--big-padding);
    border-radius: var(--border-radius);
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    margin: 60px 0px;
  }

  .right-box, .left-box{
    width: 50%;
  }

  .right-box{
    background-color: var(--neutral-1);
    border-radius: var(--border-radius);
    padding: 50px 30px 30px 30px;
    margin-left: 20px;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
  }

  .left-box{
    margin-right: 20px;
  }

  .tip-select-grid{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 10px 10px;
  }

  button{
        font-size: var(--font-input);
        font-weight: var(--font-weight);
        /* background-color: var(--neutral-1);
        color: var(--neutral-4); */
        padding: var(--input-padding);
        border-radius: var(--input-radius);
        width: 100%;
        outline: none;
        border: none;
        transition: var(--main-transition);
        cursor: pointer;
    }

    button:hover{
        background-color: var(--primary);
        color: var(--neutral-4);
    }

    .tip-input{
      text-align: center !important;
    }

    .tip-button-active{
      background-color: var(--primary);
      color: var(--neutral-1);
    }

    .tip-button-inactive{
      background-color: var(--neutral-1);
      color: var(--neutral-4);
    }

    .right-upper-box{
      width: 100%;
    }

    .summary-box{
      width: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .summary-title{
      color: var(--white);
    }

    .summary-subcaption{
      color: var(--neutral-2);
      font-size: 16px;
    }

    .summary-right h1{
      font-size: 42px;
      color: var(--primary)
    }

    .summary-left{
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: flex-start;
      height: 100%;
    }

    .right-lower-box{
      width: 100%;
    }

    .reset-btn{
      background-color: var(--primary);
      color: var(--neutral-1);
      text-transform: uppercase;
      width: 100%;
    }

    .reset-btn:hover{
      background-color: var(--neutral-4);
      color: var(--neutral-1);
    }


</style>
