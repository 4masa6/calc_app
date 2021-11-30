<template>
  <div id="app">
    <Calc v-bind:title="message" v-on:result-event="appAction" />
    <div class="mt-3 text-left">
      <table class="table text-blue" v-html="log"></table>
    </div>
    <div>
      <button class="btn btn-danger" v-on:click="doClear">Clear Log</button>
    </div>
  </div>
</template>

<script>
import Calc from './components/Calc.vue'

export default {
  name: 'App',
  components: {
    Calc
  },
  data() {
    return {
      message: 'CALC',
      result: [],
    }
  },
  computed: {
    log() {
      let table = '<tr><th>Expression</th><th>Value</th></tr>'
      if (this.result.length > 0) {
        for (var i in this.result) {
          table += '<tr><td>' + this.result[i][0] + '</td><th>' + this.result[i][1] + '</th></tr>'
        }
      }
      return table
    }
  },
  created() {
    console.log("created")
    let items = localStorage.getItem('log')
    let logs = JSON.parse(items)
    if (logs != null) {this.result = logs}
  },
  methods: {
    appAction(exp, res) {
      this.result.unshift([exp, res]) // 引数（子コンポーネントから渡ってきた計算式のテキストと答え）を配列resultに追加する
      var log = JSON.stringify(this.result) // 配列resultをJSONテキストに変換する
      localStorage.setItem('log', log) // ローカルストレージに保存する
    },
    doClear() {
      if (confirm('ログを全て消去する')) { // confirmはOKならtrue、キャンセルならfalseを返す
        localStorage.removeItem('log')
        this.result = []
      }
    }
  }
}
</script>
