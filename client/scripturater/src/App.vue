<script>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup

export default {

  data() {
    return {
      word: {id:0, word:"hebrews"},
      numberWordsRated: -1
    }
  },
  methods:{
    async append() {
      var node = document.createElement('li');
      node.appendChild(document.createTextNode(word.word));
 
      document.querySelector('ul').appendChild(node);
    },
    async loadNewWord() {

      const result = await fetch("http://localhost:3000/api/words/random");
      const word = await result.json();



      this.numberWordsRated++;
      //alert(word.word);

      this.word = word;
    },

    async submitRating(wordId, rating) {
      const result = await fetch(`http://localhost:3000/api/words/${wordId}/ratings`, 
      {
        method:"post", 
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify( {rating: rating} )
      });


    }
  },
  mounted() {
    this.loadNewWord();
  }
}

</script>

<template>

  <div>
    <h1>Scripturater</h1>

    <h2>{{word.word}}</h2>

    <h3></h3>

    <h3>Words rated: {{numberWordsRated}}</h3>

    <a href="" @click="loadNewWord" @click.prevent="submitRating(word.id, 'like')">🙁</a>
    <a href="" @click="loadNewWord" @click.prevent="submitRating(word.id, 'neutral')">😐</a>
    <a href="" @click="loadNewWord" @click.prevent="submitRating(word.id, 'dislike')">🙂</a>

    <br>

    <button @click="loadNewWord">Get Another Word</button>

    
    <div class="list">
      <ul class="ratedList">
        <li>Proverb</li>
        <li>Hebrews</li>
      </ul>
    </div>
    
  </div>

</template>

<style>
*{
  background-color: #121212;
  color: white;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

a {
  font-size: 8em;
  text-decoration: none;
  
}

a:hover{
   transform: scale(1.2);
   
}

.ratedList{
  border: white 3px solid;
  list-style-type: none;
  width: 20vw;
  margin: auto;
  padding: 10px;
  
  
}

.list{
  margin: 20px;
  
}


</style>
