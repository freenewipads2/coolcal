<template>
<div id="app">
    <div class="calculator">
    <div class="btn display">{{current || 0}}</div>
    <div class="btn" @click="ac">AC</div>
    <div class="btn" @click="invert">+/-</div>
    <div class="btn" @click="precent">%</div>
    <div class="btn operator" @click="divide">÷</div>
    <div class="btn number" @click="append('7')">7</div>
    <div class="btn number" @click="append('8')">8</div>
    <div class="btn number" @click="append('9')">9</div>
    <div class="btn operator" @click="multiply">x</div>
    <div class="btn number" @click="append('4')">4</div>
    <div class="btn number" @click="append('5')">5</div>
    <div class="btn number" @click="append('6')">6</div>
    <div class="btn operator" @click="subtract">-</div>
    <div class="btn number" @click="append('3')">3</div>
    <div class="btn number" @click="append('2')">2</div>
    <div class="btn number" @click="append('1')">1</div>
    <div class="btn operator" @click="add">+</div>
    <div class="btn zero" @click="append('0')">0</div>
    <div class="btn" @click="dot">.</div>
    <div class="btn operator" id="equals" @click="equals">=</div>
    </div>
  {{gameObj.points}}

</div>
</template>

<script>
import { Howl, Howler } from 'howler'

export default {
  name: 'app',
data () {
return {
    current: '',
    previous: '',
    operator: null,
    ticker : null,
    sounds: [],
    gameObj: {currentNumber : null, points: 0, break: false}
  }
},

mounted: function () {
  this.$nextTick(function () {
  let btns = document.getElementsByClassName("number");
  let colors = ["highlight-red","highlight-blue","highlight-green"];
  this.ticker = setInterval( x => {
    let colorIndex = Math.floor(Math.random() * (colors.length ));
    let btnIndex = Math.floor(Math.random() * (btns.length ));
    if(!this.gameObj.break){
      btns[btnIndex].classList.add(colors[colorIndex]);

      this.gameObj.currentNumber = btns[btnIndex].innerHTML;
  /*     console.log("color",colors[colorIndex]);
      console.log("btn", btns[btnIndex]); */

      setTimeout( x =>{
      btns[btnIndex].classList.remove(colors[colorIndex]);
      },1000);
    }
  },1000);

this.background =  new Howl({
  src: [require('./assets/bg_01.mp3')],
    autoplay: true,
  loop: true,
  volume: 1,
});

this.drop = ( new Howl({
  src: [require('./assets/equals.mp3')],
    volume: 2
})
);

this.sounds.push( new Howl({
  src: [require('./assets/01.mp3')],
    volume: 2
})
);

this.sounds.push( new Howl({
  src: [require('./assets/02.mp3')],
      volume: 2
})
);

this.sounds.push( new Howl({
  src: [require('./assets/03.mp3')],
      volume: 2
})
);

this.sounds.push( new Howl({
  src: [require('./assets/04.mp3')],
      volume: 2
})
);

this.sounds.push( new Howl({
  src: [require('./assets/05.mp3')],
      volume: 2
})
);
this.sounds.push( new Howl({
  src: [require('./assets/06.mp3')],
      volume: 2
})
);

this.sounds.push( new Howl({
  src: [require('./assets/07.mp3')],
      volume: 2
})
);

this.sounds.push( new Howl({
  src: [require('./assets/08.mp3')],
      volume: 2
})
);

this.sounds.push( new Howl({
  src: [require('./assets/09.mp3')],
      volume: 2
})
);
document.addEventListener('keydown', (e) => {
  console.log("heee");
    switch(e.keyCode) {
        case 49: // left
        this.append('1');
        break;
        case 50: // left
        this.append('2');
        break;
        case 51: // left
        this.append('3');
        break;
        case 52: // left
        this.append('4');
        break;
        case 53: // left
        this.append('5');
        break;
        case 54: // left
        this.append('6');
        break;
        case 55: // left
        this.append('7');
        break;
        case 56: // left
        this.append('8');
        break;
        case 57: // left
        this.append('9');
        break;

        default: return; // exit this handler for other keys
    }
    //e.preventDefault(); // prevent the default action (scroll / move caret)
});



  })
},
beforeDestroy: function(){
  clearInterval(this.ticker);
    this.background.stop();

},
methods: {
    setPrevious(){
      this.previous = this.current;
      this.current = '';
    },
    ac(){
      this.current = '';
    },
    invert(){
      this.current = `${-parseFloat(this.current)}`;
    },
    precent(){
      this.current = `${parseFloat(this.current)/100}`;
    },
    append(n){
      if(this.current.charAt(0) == '0'){
        return;
      }
      let breakIndex = Math.floor(Math.random() * 2);
      if(breakIndex == 1){
        this.gameObj.break = true;
        this.drop.play();
        document.getElementById("equals").classList.add("glow");
      }
      if(this.gameObj.currentNumber == n){
        this.gameObj.points++;
        let i = Math.floor(Math.random() * 3);
        if(i == 1){
          this.sounds[parseInt(this.gameObj.currentNumber) - 1].play();
        }
      }
      this.current = `${this.current}` + `${n}`;
    },
    dot(){
      if(this.current.indexOf(".") !== -1){
        return;
      }
      this.append(".");
    },
    divide(){
      this.setPrevious();
      this.operator = (a,b) => { return a / b }
    },
    multiply(){
      this.setPrevious();
      this.operator = (a,b) => { return a * b }
    },
    subtract(){
      this.setPrevious();
      this.operator = (a,b) => { return a - b }
    },
    add(){
      this.setPrevious();
      this.operator = (a,b) => { return a + b }
    },
    equals(){
      this.current = `${this.operator(parseFloat(this.current),parseFloat(this.previous))}`;
    }
}
}
</script>

<style lang="scss">
*{
  padding: 0px;
  margin:0px;
  font-family: sans-serif;
}
.calculator{
  margin: auto auto;
  width: 300px;
  font-size: 16pt;
   display: grid;
    grid-template-columns: auto auto auto;
    grid-template-columns: repeat(4, 1fr);
}
.display{
  grid-column-start: 1;
  grid-column-end: 5;
  background: #3d3d3d!important;
  color: white;
  text-align: right!important;
  font-size: 32pt;
}
.operator{
  background: orange!important;
  color: white;
}
.zero{
  grid-column-start: 1;
  grid-column-end: 3;
}
.btn{
  transition: 0.25s background ease-in-out;
  background: #efefef;
  padding: 10px;
  border: 1px solid #3d3d3d;
  text-align: center;
  cursor: pointer;
}
.btn:hover{
   -moz-box-shadow:    inset 0 0 10px rgba(0,0,0,0.5);
   -webkit-box-shadow: inset 0 0 10px rgba(0,0,0,0.5);
   box-shadow:         inset 0 0 10px rgba(0,0,0,0.5);

}
.highlight-red{
     background: rgba(255,0,0,0.2)!important;
}
.highlight-blue{
     background: rgba(0,255,0,0.2)!important;
}
.highlight-green{
     background: rgba(0,0,255,0.2)!important;
}

.glow{
   animation: glow 3s infinite;
}

@keyframes example {
    0%   {background-color: red;}
    50%  {background-color: blue;}
    100% {background-color: green;}
}

@keyframes glow {
    0%   {background-color: red;}
    33%  {background-color: blue;}
    66% {background-color: green;}
    100% {background-color: red;}

}
</style>
