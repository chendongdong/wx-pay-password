<template>
  <div class="pay-password">
    <div v-if="!isSucceed">
      <p style="font-size: 16px;margin-left: 20px">请输入支付密码</p>
      <div class="content">
        <input v-for="(item, idx) in 6" @keyup="onKeyup" :id="'myInput' + idx" type="tel" maxlength="1">
      </div>
    </div>
    <div v-else>
      <div style="text-align: center;">
        <img src="../assets/ic_pay_succeed.png" style="margin-top: 60px">
        <p style="font-size: 22px;color: #333">恭喜您，支付成功！</p>
      </div>
      <div style="padding: 40px 20px;">
        <button @click="isSucceed=false" type="primary" style="height: 40px;font-size: 16px;">重新输入</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default{
    methods: {
      initInput () {
        // 进入页面设置第一个元素聚焦
        for (let i = 0; i < 6; i++) {
          if (i === 0) {
            document.getElementById('myInput' + i).disabled = false
            document.getElementById('myInput' + i).focus()
          } else {
            document.getElementById('myInput' + i).disabled = true
          }
          document.getElementById('myInput' + i).value = ''
        }
        this.passwords = ''
      },
      onKeyup (e) {  // 默认返回的键盘监听事件event
        let key = e.keyCode
        let val = e.target.value
        let next = e.target.nextElementSibling
        let prev = e.target.previousElementSibling
        if (val.length === 0) { // 处理删除后再输入的情况，还原type为text,否则的话还是保持上次的password类型
          e.target.type = 'text'
        } else { // 输入完成350毫秒后设置type为password
          setTimeout(() => {
            e.target.type = 'password'
          }, 350)
        }
        // 处理删除的情况，设置上一个元素聚焦   8：Backspace键  46：delete键
        if ((key === 8 || key === 46) && val.length === 0 && prev) {
          prev.disabled = false
          prev.value = ''
          prev.focus()
        }
        // 输入的情况，设置下一个(如果还有)聚焦
        if (val.length >= 1) {
          this.passwords += e.target.value
          if (next) {
            e.target.disabled = true
            next.disabled = false
            next.focus()
          }
        }
        if (!next) {
          this.isSucceed = true
          console.log('密码输入完成了')
        }
      }
    },
    data () {
      return {
        isSucceed: false,
        passwords: ''
      }
    },
    mounted: function () {
      this.$nextTick(function () {
        this.initInput()
      })
    }
  }
</script>

<style lang="scss">
  @import "../style/pay-password.scss";
</style>
