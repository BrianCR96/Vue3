<template>
  <div v-if="isValidQuestion">
    <img :src='img' @load="handleImageLoad">
  </div>
  <div class='bg-dark'></div>
  
  <div class='indecision-container'>
    <input v-model='question' type='text' placeholder='Hazme una pregunta'>
    
    <div class="answer-container">
      <p v-if="!isValidQuestion">Recuerda terminar con un sigo de interrogación (?)</p>
      <div v-if="isValidQuestion && !error">
        <h2>{{ question }}</h2>
        <h1>{{ answer }}</h1>
      </div>
      <div v-else> 
        <!--Error-->
        <h2>{{ answer }}</h2>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'indecision',
  data() {
    return {
      question: null,
      answer: null,
      img: null,
      loading: true,
      isValidQuestion: false,
      error: false
    }
  },
  methods: {
    async getAnswer() {
      try {
          
          this.answer = 'Pensando...'
          this.img = null
          const { answer, image } = await fetch('https://yesno.wtf/api').then(response => response.json())
        /* {
          'answer': 'yes'/'no'/'maybe',
          'forced': true/false,
          'image': 'URL'
        } */

        this.answer = {'yes': 'Sí', 'no': 'No', 'maybe': 'Tal vez'}[answer]
        this.img = image
      } catch (err) {
        this.answer = 'Ha ocurrido un error, por favor inténtelo de nuevo más tarde.'
        this.error = true
      }
    },
    handleImageLoad() {
      return this.loading = false
    }
  },
  watch: {
    question(value, oldValue) {
      this.answer = null
      this.isValidQuestion = false
      this.error = false

      if (!value.includes('?')) return

      this.isValidQuestion = true
      this.getAnswer()
    }
  }
}

</script>

<style scoped>
  img, .bg-dark {
    height: 100vh;
    left: 0px;
    max-height: 100%;
    max-width: 100%;
    position: fixed;
    top: 0px;
    width: 100vw;
    overflow: hidden;
  }

  .bg-dark {
    background-color: rgba(0, 0, 0, 0.60);
  }
  .indecision-container {
    position: relative;
    z-index: 99;
    overflow: hidden;
  }

  .answer-container {
    margin-top: 1rem;
    position: relative;
  }
    
  input {
    width: 250px;
    padding: 10px 15px;
    border-radius: 5px;
    border: none;
  }
    
  input:focus {
    outline: none;
  }

  p {
    color: white;
    font-size: 20px;
    margin-top: 0px;
  }

  h1, h2 {
    color: white;
  }  
  
  h2 {
    margin-top: 150px;
  }
</style>