<template>
  <div class="container_box">
    <p class="title">等待放假ing</p>
    <picker class="weui-btn"
            @change="PickerChange"
            :value="number"
            :range="array">
      <button type="warn">{{array[number]}}年</button>
    </picker>
    <div class="grid-content">
      <div class="box">
        <div class="list"
             v-for="(item, index) in data"
             :key="index">
          <p>{{item.name}}</p>
          <p>{{item.time}}</p>
          <p class="color_red"
             v-if="item.dayTime === 0">今天</p>
          <p class="color_red"
             v-else>{{item.dayTime}}天</p>
        </div>
      </div>
    </div>
    <mp-toast type="error"
              v-model="showToast"
              content="今年没有假期了哦"
              :duration="3000"></mp-toast>
  </div>
</template>

<script>
import { timeToTimestamp } from '../../utils/index'
import mpToast from 'mpvue-weui/src/toast';
import { date } from '../../utils/date'
export default {
  components: {
    mpToast
  },
  data () {
    return {
      Data: date,
      array: ['2020', '2021', '2022', '2023', '2024', '2025'],
      data: [],
      today: new Date().getTime(),
      day: 86400000,
      number: 1,
      showToast: false
    }
  },

  methods: {
    filter (arr) {
      this.data = []
      if (arr !== undefined) {
        arr.forEach(item => {
          if ((item.timestamp + this.day) >= this.today) {
            item.dayTime = Math.ceil((item.timestamp - this.today) / this.day)
            this.data.push(item)
          }
        });
        setTimeout(() => {
          if (this.data.length == 0) {
            this.showToast = true
            setTimeout(() => {
              this.showToast = false
            }, 3000);
          }
        }, 0);
      }
    },
    PickerChange (e) {
      var year = this.array[e.mp.detail.value]
      switch (year) {
        case '2020':
          this.number = 0
          break;
        case '2021':
          this.number = 1
          break;
        case '2022':
          this.number = 2
          break;
        case '2023':
          this.number = 3
          break;
        case '2024':
          this.number = 4
          break;
        case '2025':
          this.number = 5
          break;
        default:
          break;
      }
      var arr = this.Data[this.number]
      this.$nextTick(() => {
        this.filter(arr)
      })

    }
  },

  created () {
    let now = new Date().getFullYear().toString();
    this.number = now.substring(now.length-1)
    var dateArray = this.Data[this.number]
    this.filter(dateArray)

    // 获取用户信息
    // wx.login({
    //   success (res) {
    //     if (res.code) {
    //       //发起网络请求
    //       console.log(res)
    //     } else {
    //       console.log('登录失败！' + res.errMsg)
    //     }
    //   }
    // })

    // 开启权限
    // wx.authorize({
    //   // 通过scope指明申请获取哪种类型的权限
    //   scope: 'scope.record',
    //   success () {
    //     // 用户已经同意小程序使用录音功能，后续调用 wx.startRecord 接口不会弹窗询问
    //     wx.startRecord()
    //   }
    // })

    // 扫码
    // wx.scanCode({
    //   success (res) {
    //     console.log(res)
    //   }
    // })
  }
}
</script>

<style scoped>
.title {
  text-align: center;
  font-family: Georgia, "Times New Roman", Times, serif;
  visibility: hidden;
}
.box {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}
.weui-btn {
  width: 60%;
  margin: 0 auto;
  padding-bottom: 20px;
}
.list {
  float: left;
  display: flex;
  width: 30%;
  flex-direction: column;
  align-items: center;
  border: 1px solid #ccc;
  padding: 15px;
  margin: 10px 0;
}
.color_red {
  color: red;
}
</style>
