<template>
  <div class="cart alert-primary">
    <div class="card-body test-left">
      <h2 class="card-title text-center">{{ title }}</h2>
      <p class="card-text h5">{{ message }}</p>
      <hr>
      <div>
        <div class="form-group">
          <label>Formula:</label>
          <textarea class="form-control mb-2" v-model="fomula"></textarea>
        </div>
        <div class="text-center">
          <button class="brn btn-primary" v-on:click="doAction">CALC</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calc',
  props: {
    title: String,
  },
  data() {
    return {
      message: 'Enter expression: ',
      fomula: '0',
    }
  },
  methods: {
    doAction() {
      let arr  = this.fomula.trim().split('\n') // fomulaのテキストを改行コードで分割し配列にする
      let last = arr.pop() // 配列の最後の項目を変数lastに取り出しておく（配列からは消える）
      let fn   = ''
      for (let n in arr) { // 配列の各要素について繰り返し処理をする
        if (arr[n].trim() != '') { // 取り出した項目のテキストが空でないなら「let テキスト ;」というテキストを作って変数fnに追加
          fn += 'let ' + arr[n] + ';'
        }
      }
      fn          += 'return ' + last + ';' // 繰り返しが終わったら「return 最後の項目 ;」とテキストを追加して完成
      let exp      = 'function f() {' + fn + '} f();' // 完成したテキストの前後に「function(){} f();」と付け足す。取り出した処理のテキストをfという関数にまとめて実行するスクリプトができあがる。
      let ans      = eval(exp); // eval関数を使ってスクリプトのテキストを実行する
      this.message = 'answer: ' + ans // 実行結果をメッセージにまとめてmessageに設定
      let re       = arr.join(';').trim() // 配列をセミコロンでつなげて１つのテキストにする
      if (re != '') { re += ';'} // 配列が空でなければセミコロンを付ける
      re          += last // 最後の項目を付け加える
      this.$emit('result-event', re, ans) // 作成したテキストを引数に指定し、$emitで親コンポーネントのresult-eventを呼び出す
    }
  }
}
</script>
