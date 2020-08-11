<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><h1>BizzyQuiz</h1></b-nav-item>
      <b-nav-item disabled class="mt-3">Counter: {{ numTotal }}/{{ numQuestions}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: [
    'numQuestions',
    'numTotal'
  ]
}
</script>
