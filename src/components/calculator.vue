<template lang="html">
    <div class=calculator>

        <div class="display">
            <div class="display-text">
                <span ref="displayText">{{display}}</span>
            </div>
        </div>
    
        <div class="buttons">
            <div class="operators" v-for="operator in operators" :key="operator"
                v-on:click="operation(operator)">
                    {{operator}}
            </div>

            <div class="functions" v-for="funct in func" :key="funct"
            v-on:click="funksjon(funct)">
                {{funct}}
            </div>
        
            <div class="numbrs" v-for="numb in numbers" :key="numb"
            v-on:click="numberClicked(numb)"> 
                {{numb}}
            </div>
        </div>

        <div class="log">
            <h5>Log</h5>
                <ul class="expression" v-for="expression in calculated" :key="expression">
                {{expression}}
                </ul>
        </div>

    </div>
</template>


<script>

//Tilbakemelding

export default {
methods:{
    upDateDisplay(value){
        if((value+this.display).length>20){
            this.display='expression too large';
        }else{
        this.display= value;
        }
    },
    addExpressionToLog(value){
        this.currentExpression = this.currentExpression + '=' + value;
        console.log('the expression thats pushed:' + this.currentExpression);
        this.calculated.push(this.currentExpression);
        
        this.currentExpression='';
        this.previousValue=null;
        this.currentOperator='';
        this.isPressed=false;
    },
    calculate() {
        console.log('operatorbutton was clicked:' + this.currentOperator +
        'previous: ' + this.previousValue + ' display: ' + this.display);

        try{
            let value =  eval(this.currentExpression);
            this.upDateDisplay(value);
            this.comma=false;
            if(this.isPressed){
                console.log('We get into the push to log part');
                this.addExpressionToLog(value);
         }
        }catch{
            window.alert('invalid expression');
        }
    
    },
    numberClicked(numb){
        this.currentExpression = this.currentExpression + numb;
        if(this.currentOperator !== ''){
            console.log('changing numbr')
            this.upDateDisplay(' ');
        }
        if(this.previousValue===null){
            this.previousValue=numb;
            this.upDateDisplay(numb);
        }
        else{
            this.upDateDisplay(this.display + numb);
            }
    },
    operation(operator){ 
        this.currentExpression = this.currentExpression +  operator;
        this.upDateDisplay(' ');
        },
    funksjon(button) {
        if (button === 'Del') {
            this.upDateDisplay(this.display.slice(1));
        } else if (button === '+/-') {
          this.upDateDisplay(this.display * -1);
        } else if (button === '=') {
          this.isPressed=true;
          console.log(this.isPressed);
          this.calculate();

        } else if(button === '.'){
             if (!this.comma) {
                this.currentExpression += '.';
                this.display = this.display +'.';
                this.comma=true;
            }else{
                console.log('can only be one comma');
            }
        }
        else if (button === 'C'){
            this.upDateDisplay(' ');
            this.display='';
            this.previousValue=null;
            this.currentOperator='';
            this.comma=false;
            this.currentExpression='';
            console.log(this.previousValue + this.display+ this.currentOperator);
        }
    },
},
    data:() => ({
        display:' ',
        previousValue:'',
        currentOperator:'',
        currentExpression:'',
        isPressed: false,
        comma: false,
        

    //arrays with all the buttons
    operators: ['+','-','*','/'],
    numbers:[ '9','8','7','6','5','4','3','2','1','0'],
    func:['Del', '=', '-/+','C','.'], 
    calculated:[],
    })
}
</script>


<style>
body {
  width: 100vw;
  height: 100vh;
  font-size: 2.5vw;
  display: flex;
  justify-content: center;
  align-items: center;
}
.calculator {
  width: 30vw;
  height: calc(30vw * 1.4);
  font-family: bold sans-serif;
  border-radius: 5px;
  overflow: hidden;
  border: 0.5px solid grey;
}
.display {
  background: #989898;
  color: white;
  font-size: 4vw;
  height: 10%;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}
.display-text {
  padding: 0.5vw;
  text-align: right;
}
.buttons {
    outline: 0.5px solid black;
    background: #D0D0D0;
    align-items: center;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    width: 100%;
    text-align: center;
    border: 1px solid rgba(0, 0, 0, 0.8);
}
.functions{
    border: 1px solid rgba(0, 0, 0, 0.8);
}
.operators {
    color: black;
    background: #af5ec4;
    text-align: center;
    border: 1px solid rgba(0, 0, 0, 0.8);
}
.numbrs{
    border: 1px solid rgba(0, 0, 0, 0.8);
}
.expression {
    display: grid;
    grid-auto-columns: 2;
    font-size: 1.5vw;

}
</style>