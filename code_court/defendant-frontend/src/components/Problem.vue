<template>
  <div v-if="problem">
    <h1 class="title is-1">{{ problem.name }}</h1>
    <div v-html="convertToMarkdown(problem.problem_statement)"></div>
    </br>

    <h3 class="subtitle is-3">Sample Input</h3>
    <pre><code>{{ problem.sample_input }}</code></pre>
    </br>

    <h3 class="subtitle is-3">Sample Output</h3>
    <pre><code>{{ problem.sample_output }}</code></pre>
    <br/>

    <h3 class="subtitle is-3">Code</h3>
    <Editor v-model="source_code"
            id="main-editor"
            theme="solarized_light"
            :height="500" />
    <br/>

    <div>
      <button v-on:click="submitCode(false)" class="button is-info">Run</button>
      <button v-on:click="submitCode(true)" class="button is-warning">Submit</button>
    </div>
    <br/>

    <h3 class="subtitle is-3">Runs</h3>
    <RunCollapse v-for="(run, i) in problem.runs.slice().reverse()"
                 :key="run.id"
                 :run="run"
                 :init-is-toggled="i == 0 ? true : false"/>

  </div>
</template>

<script>
import marked from 'marked'
import axios from 'axios'

import Editor from '@/components/Editor'
import RunCollapse from '@/components/RunCollapse'

export default {
  data () {
    return {
      source_code: ''
    }
  },
  computed: {
    problem () {
      return this.$store.state.problems[this.$route.params.slug]
    }
  },
  methods: {
    convertToMarkdown: function (s) {
      return marked(s)
    },
    submitCode: function (isSubmission) {
      console.log(this.source_code)
      axios.post('http://localhost:9191/api/submit-run', {
        lang: 'python',
        problem_slug: this.problem.slug,
        source_code: this.source_code,
        is_submission: isSubmission
      }).then((response) => {
      }).catch(function (error) {
        console.log(error)
      })
    }
  },
  components: {
    Editor,
    RunCollapse
  }
}
</script>

<style scoped>
</style>