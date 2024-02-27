<template>
  <div>
    <div class="buttons">
      <button 
        v-for="button in buttons" 
        :key="button.title" class="buttons_button" 
        :class="[button.title, button.active ? 'active' : '']"
        @click="setUserButtons(button)"  
      >
      </button>
    </div>
    <h2>Раунд {{ round }}</h2>
    <div>Уровень сложности</div>
    <div>
      <label for="delay1500" style="cursor: pointer;">Легкий</label>
      <input 
        value="1500"
        id="delay1500"
        name="delay"
        checked
        @change="setDelay($event.target.value)"
        type="radio"
        style="margin-left: 5px; cursor: pointer;">
    </div>
    <div>
      <label for="delay1000" style="cursor: pointer;">Средний</label>
      <input 
        value="1000"
        id="delay1000"
        name="delay"
        @change="setDelay($event.target.value)"
        type="radio"
        style="margin-left: 5px; cursor: pointer;">
    </div>
    <div>
      <label for="delay400" style="cursor: pointer;">Сложный</label>
      <input 
        value="400"
        id="delay400"
        name="delay"
        @change="setDelay($event.target.value)"
        type="radio"
        style="margin-left: 5px; cursor: pointer;">
    </div>
    <button @click="start()" style="padding: 15px; background-color: lightgrey; cursor: pointer;">Старт</button>
    <h2 v-if="!success" style="color: red">Вы проиграли</h2>
  </div>
</template>

<script>
import clickBlue from './clickBlue.mp3'
import clickGreen from './clickGreen.mp3'
import clickOrange from './clickOrange.mp3'
import clickRed from './clickRed.mp3'

  export default {
    name: "App",
    data() {
      return {
        buttons: [{title: 'blue', active: false}, {title: 'green', active: false}, {title: 'orange', active: false}, {title: 'red', active: false}],
        gameButtons: [],
        userButtons: [],
        success: true,
        round: 0,
        delay: 1500,
        timeOutIds: []
      }
    },
    methods: {
      setUserButtons(pickedButton) {
        if(this.gameButtons.length !== 0) {
          this.userButtons.push(pickedButton)
            for(let i = 0; i < this.userButtons.length; i++) {
              if(this.gameButtons[i].title == this.userButtons[i].title) {
                // в случае правильного нажатия идет проверка: если, проверяемый индекс в массиве игрока равен последнему индеку массива игры(то есть массивы одинаковы по длине), то игра снова пушит новый элемент в свой массив, а у пользователя массив опустошается т.к ему надо с начала все повторять
                if(i === this.gameButtons.length - 1) {
                  const randomButtonIndex = Math.floor(Math.random() * this.buttons.length)
                  this.gameButtons.push(this.buttons[randomButtonIndex])
                  this.userButtons = []
                  this.round += 1
  
                  setTimeout(() => {
                    for(let i = 0; i < this.gameButtons.length; i++) {
                      const gameButton = this.gameButtons[i]
                      const currentButtonIndex = this.buttons.findIndex( button => button.title === gameButton.title)

                      const firstTimeOutId = setTimeout(() => {
                        this.buttons[currentButtonIndex].active = true    // каждая последующая кнопка будет загораться с задережкой i * this.delay
                        if(this.buttons[currentButtonIndex].title === 'blue') {
                          const clickBlueButton = new Audio(clickBlue)
                          clickBlueButton.play()
                        }
                        if(this.buttons[currentButtonIndex].title === 'green') {
                          const clickGreenButton = new Audio(clickGreen)
                          clickGreenButton.play()
                        }
                        if(this.buttons[currentButtonIndex].title === 'orange') {
                          const clickOrangeButton = new Audio(clickOrange)
                          clickOrangeButton.play()
                        }
                        if(this.buttons[currentButtonIndex].title === 'red') {
                          const clickRedButton = new Audio(clickRed)
                          clickRedButton.play()
                        }
                      }, i * this.delay);

                      this.timeOutIds.push(firstTimeOutId)  // сохраняем id каждого setTimeOut, чтобы отписаться от него, если пользователь ошибся - нужно сбросить подсвечивание кнопок
                      setTimeout( () => {
                        this.buttons[currentButtonIndex].active = false  // каждая последующая кнопка будет потухать с задережкой i * this.delay + 500 - 500 сколько светится кнопка
                      }, i * this.delay + 500);
                    }
                  }, 500);
                }
              } else {
                this.success = false
                this.round = 0
                // если пользователь ошибся, то очищаем сохраненные setTimeOut, чтобы сбросить подсвечивание кнопок
                if(!this.success) {
                for(let i = 0; i < this.timeOutIds.length; i++) {
                  clearTimeout(this.timeOutIds[i])
                }
              }
              }
            }
        }
        if(pickedButton.title === 'blue') {
          const clickBlueButton = new Audio(clickBlue)
          clickBlueButton.play()
        }
        if(pickedButton.title === 'green') {
          const clickGreenButton = new Audio(clickGreen)
          clickGreenButton.play()
        }
        if(pickedButton.title === 'orange') {
          const clickOrangeButton = new Audio(clickOrange)
          clickOrangeButton.play()
        }
        if(pickedButton.title === 'red') {
          const clickRedButton = new Audio(clickRed)
          clickRedButton.play()
        }
      },
      start() {
        this.success = true
        this.gameButtons = []
        this.userButtons = []
        this.timeOutIds = []
        const randomButtonIndex = Math.floor(Math.random() * this.buttons.length)
        this.gameButtons.push(this.buttons[randomButtonIndex])
        this.round = 1

        this.buttons[randomButtonIndex].active = true
        if(this.buttons[randomButtonIndex].title === 'blue') {
          const clickBlueButton = new Audio(clickBlue)
          clickBlueButton.play()
        }
        if(this.buttons[randomButtonIndex].title === 'green') {
          const clickGreenButton = new Audio(clickGreen)
          clickGreenButton.play()
        }
        if(this.buttons[randomButtonIndex].title === 'orange') {
          const clickOrangeButton = new Audio(clickOrange)
          clickOrangeButton.play()
        }
        if(this.buttons[randomButtonIndex].title === 'red') {
          const clickRedButton = new Audio(clickRed)
          clickRedButton.play()
        }
        setTimeout(() => {
          this.buttons[randomButtonIndex].active = false
        }, 500);    // 500 - сколько светится кнопка
      },
      setDelay(delay) {
        if(Number(delay) === 400) {
          this.delay = 900
        } 
        if(Number(delay) === 1000) {
          this.delay = 1500
        } 
        if(Number(delay) === 1500) {
          this.delay = 2000
        }
      },
    }
  }
</script>

<style lang="sass">
  body 
    font-family: sans-serif

  .buttons
    display: flex
    gap: 40px

    .buttons_button
      padding: 40px
      border: none
      cursor: pointer
      border: 3px solid transparent

      &.blue
        background-color: #6262ff
        &:hover
          border: 3px solid black
        &:active
          background-color: blue 
        &.active 
          background-color: blue
          border: 3px solid black 

      &.green 
        background-color: #349d34
        &:hover
          border: 3px solid black
        &:active
          background-color: green     
        &.active
          background-color: green
          border: 3px solid black

      &.orange 
        background-color: #ffe493
        &:hover
          border: 3px solid black
        &:active
          background-color: #ffbf00
        &.active
          background-color: #ffbf00
          border: 3px solid black

      &.red
        background-color: #ff5555
        &:hover
          border: 3px solid black
        &:active
          background-color: red 
        &.active
          background-color: red
          border: 3px solid black
</style>