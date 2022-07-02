<template>
  <div>
    <h3 id="wordToPlay"></h3>
    <div>
      <input v-model="testAnswer" type="text" id="vocTest">
    </div>
    <div>
      <button id="submit" @click="checkTheWord">Submit</button>
    </div>
    <div>
      <button id="play-again" @click="pickFiveWords">Play again</button>
    </div>
    <div id="right-answer" v-bind:style="{display: displayRight}">Congra! correct answer</div>
    <div id="false-answer" v-bind:style="{display: displayWrong}">Sorry! Wrong answer</div>
  </div>
</template>

<script>

export default {
  name: 'PlayView',
  components: ['VocLog'],
  props: [''],
  data () {
    return {
      wordTotranslate: '',
      testAnswer: '',
      displayRight: '',
      displayWrong: '',
      wordsList: [],
      howManySubmit: 0,
      wordForPlay: [],
      updatedObject: {},
      probability: 0
    }
  },
  methods: {
    checkTheWord () {
      this.howManySubmit = this.howManySubmit + 1
      if (this.howManySubmit === 3) {
        this.howManySubmit = 0
      }
      if (this.wordTotranslate === this.testAnswer.trim()) {
        this.displayRight = 'inline'
        if (this.probability <= 1) {
          this.probability = this.probability + 0.2
          this.updateProbability()
        }
      } else {
        this.displayWrong = 'inline'
        if (this.probability >= 0) {
          this.probability = this.probability - 0.2
          this.updateProbability()
        }
      }
    },
    playAgain () {
      this.displayRight = ''
      this.displayWrong = ''
    },
    pickFiveWords () {
      console.log(this.wordsList)
      let i = 0
      let weight = 0
      const words = []
      while (i <= 5) {
        const randomNumber = Math.random()
        if (randomNumber <= 1 && randomNumber > 0.8) {
          weight = 1.0
        } else if (randomNumber <= 0.8 && randomNumber > 0.5) {
          weight = 0.8
        } else {
          // eslint-disable-next-line no-unused-vars
          weight = 0.5
        }
        for (let j = 0; j < this.wordsList.length; j) {
          if (this.wordsList[j].uebersetzungSet[0].wahscheinlichkeit === 1) {
            console.log(this.wordsList[j])
            words.push(this.wordsList[j])
            break
          }
        }
        i = i + 1
        console.log(i)
      }
      console.log(words)
    }
  },
  playing () {
    const number = 0
    document.getElementById('wordToPlay').innerHTML = this.wordForPlay[number].wort
  },
  updateProbability () {
    const myHeader = new Headers()
    myHeader.append('Accept', 'application/json')
    myHeader.append('Content-type', 'application/json')

    const requestOptions = {
      methods: 'POST',
      headers: myHeader,
      body: this.probability,
      redirect: 'follow'
    }

    fetch('http://localhost:8080/api/v1/uebersetzung', requestOptions)
      .then(response => response.json())
      .catch(error => console.log('error', error))
  },
  beforeMount () {
    const myHeader = new Headers()
    myHeader.append('Accept', 'application/json')
    myHeader.append('Content-type', 'application/json')

    const requestOptions = {
      methods: 'GET',
      headers: myHeader,
      redirect: 'follow'
    }
    fetch('http://localhost:8080/api/v1/words', requestOptions)
      .then(response => response.json())
      // eslint-disable-next-line no-return-assign
      .then(result => result.forEach(data => {
        this.wordsList.push(data)
      }))
      .catch(error => console.log('error', error))
  }
}
</script>

<style scoped>
#right-answer,#false-answer{
  display: none;
}

#right-answer{
  color: green;
}
#false-answer{
  color: red;
}
</style>
