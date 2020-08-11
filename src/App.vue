<template>
  <div id="app">
    <Header :numTotal="numTotal" :numQuestions="numQuestions" />
    <div class="mt-3">
      <div class="select-choice">
        <div class="mb-3"><label for="selected"><strong>Choose your questions</strong></label></div>
        <div><b-form-select v-model="selected" :options="options"></b-form-select></div>
        <div><b-form-input v-model="numQuestions" placeholder="Enter number of questions"></b-form-input></div>
        <div><b-form-select v-model="difficulty" :options="levels"></b-form-select></div>
        <b-button @click="applyCategory">Apply</b-button>
        <div v-if="index == numQuestions">
          <b-alert show variant="primary"><strong> Score: </strong>{{ numCorrect }} / {{ numTotal }}</b-alert>
        </div>
      </div>
    </div>
    <b-container class="bv-example-row">
      <b-row class="mt-3">
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      selected: null,
      options: [
        { value: null, text: 'Please select a category', disabled: true },
        { value: 27, text: 'Animals' },
        { value: 21, text: 'Sports' },
        { value: 9, text: 'General' }
      ],
      numQuestions: null,
      difficulty: null,
      levels: [
        { value: null, text: 'Please select a difficulty', disabled: true },
        { value: 'any', text: 'Any Difficulty' },
        { value: 'easy', text: 'Easy' },
        { value: 'medium', text: 'Medium' },
        { value: 'hard', text: 'Hard' }
      ],
      type: 'multiple',
      numCategory: null
    }
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
    mounted() {
      let url = '';

      if (this.difficulty == 'any') {
        url = `https://opentdb.com/api.php?amount=${ this.numQuestions }&category=${ this.selected }&type=${ this.type }`;
      } else {
        url = `https://opentdb.com/api.php?amount=${ this.numQuestions }&category=${ this.selected }&difficulty=${ this.difficulty }&type=${ this.type }`;
      }
    
      fetch(url, {
        method: "get"
      })
      .then(response => {
        return response.json();
      })
      .then(jsonData => {
        this.questions = jsonData.results;
      });
    },
    applyCategory() {
      this.mounted();
      this.index = 0;
      this.numCorrect = 0;
      this.numTotal = 0;
    }
//    var timer = setInterval(function() {
//     console.log('123');
//       this.next();
//    }, 2000);
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
/* .select-choice {
  align-items: center;
  display: flex;
  flex-direction: row;
  width: 15%;
  margin-bottom: 10px;
} */

/* Small devices (landscape phones, 576px and up) */
@media (min-width: 576px) {
  #app {
    display: block;
    margin-top: 10px;
  }

  .select-choice {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 30%;
    margin-bottom: 5px;
  }
}

/* Medium devices (tablets, 768px and up) */
/* @media (min-width: 768px) {
  #app {
    margin-top: 15px;
  }

  .select-choice {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 30%;
  }
} */

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) {
  .select-choice {
    align-items: center;
    display: flex;
    width: 15%;
    margin-bottom: 10px;
  }
}

/* Extra large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {
  .select-choice {
    align-items: center;
    display: flex;
    width: 35%;
    margin-bottom: 10px;
  }
}
</style>
