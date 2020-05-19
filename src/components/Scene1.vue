<template>
  <div class="scene1">
    <nickname-header></nickname-header>
    <input-text></input-text>
    <input-button></input-button>
  </div>
</template>

<script>
// importする
import Vue from 'vue'

// デフォルト値定義
export default {
  data () {
    return {

    }
  }
}

var PropertyStore = {
  debug: true,
  state: {
    property: {
      type: '1',
      nickname: ''
    }
  },
  validate () {
    this.debug && console.log('setMessageAction triggered with', this.state.property.nickname)

    console.log(this.state.property.nickname.length)
    if (this.state.property.nickname.length <= 3) {
      throw new Error('3文字以上入力してください')
    }
    return true
  }
}

// ニックネーム表示用のヘッダーコンポーネントを作成
var NickNameHeader = Vue.extend({
  name: 'nickename_header',
  template: '<div class="nickname">{{ sharedState.state.property.nickname }}</div>',
  data: function () {
    return {
      privateState: {},
      sharedState: PropertyStore
    }
  }
})

// コンポーネントを登録
Vue.component('nickname-header', NickNameHeader)

// インプットフォーム用のコンポーネントを作成
var InputText = Vue.extend({
  name: 'nickname_input',
  template: '<input class="input" v-model="sharedState.state.property.nickname" placeholder="NICKNAMEを入力してください。">',
  data: function () {
    return {
      property: {},
      sharedState: PropertyStore
    }
  }
})

Vue.component('input-text', InputText)

// ボタン用のコンポーネントを作成
var InputButton = Vue.extend({
  name: 'input-button',
  template: '<button class="button" v-on:click="validate">START</button>',
  data: function () {
    return {
      privateState: {},
      sharedState: PropertyStore
    }
  },
  methods: {
    validate: function (event) {
      console.log(this.$el)
      var valid = false
      try {
        valid = this.$data.sharedState.validate()
        if (valid) {
          this.$router.push('/scene2')
        }
      } catch (e) {
        alert(e.message)
      }
    }
  }
})
Vue.component('input-button', InputButton)
</script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.scene1{
  font-size: 1.8rem;
  text-align: center;
  a div p{
    display: block;
    margin: 0 auto;
    height:30pt;
  }
}
.input{
  width: 130pt;
  height:30pt;
}
.button {
  display: block;
  position: relative;
  margin: 0 auto;
  width: 70pt;
  border: solid 1px silver;
  border-radius: 0.5rem 0.5rem;
  padding: 0.5rem 1.5rem;
  margin-top: 1rem;
  text-decoration: none;
}
</style>
