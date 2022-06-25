<template>
  <VocLogo></VocLogo>
  <div>
    <div>
      <input v-model="word" type="text" placeholder="Enter new Word" id="word">
      <input v-model="translation" type="text" placeholder="Enter a Translation" id="translation">
    </div>
    <button id="save" @click="saveDataInDatabase">Save</button><br>
    <button id="delete">Delete</button>
  </div>
</template>

<script>
import VocLogo from '@/components/VocLog'
export default {
  name: 'VocView',
  components: { VocLogo },
  props: [''],
  data () {
    return {
      word: '',
      translation: ''
    }
  },
  methods: {
    saveDataInDatabase () {
      const myheader = new Headers()
      myheader.append('Content-type', 'application/json')
      const content = {
        word: 'run',
        language: 'english',
        translation: 'rennen',
        transLanguage: 'deutsch'
      }

      const requestOptions = {
        method: 'POST',
        headers: myheader,
        body: content,
        redirect: 'follow'
      }
      fetch('https://voclearner.herokuapp.com/api/v1/wordtranslation', requestOptions).catch(error => console.log('error', error))
    },
    deleteData () {
      const myheader = new Headers()
      myheader.append('Content-type', 'application/json')
      const content = {
        word: this.word
      }
      const requestOptions = {
        method: 'DELETE',
        headers: myheader,
        body: JSON.stringify(content),
        redirect: 'follow'
      }
      fetch('http://localhost:8080/api/v1/wordtranslation', requestOptions).catch(error => console.log('error', error))
    }
  }
}
</script>

<style scoped>
#word, #translation{
  border-style: groove;
  border-color: azure;
  border-radius: 5px;
  width: 100px;
  height: 50px
}
#save, #delete{
  width: 100px;
  height: 50px;
  text-align: center;
  backgound-color: green;
  color: darkgray;
}

</style>
