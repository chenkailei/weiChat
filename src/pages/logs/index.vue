<template>
  <div class="container_box">
    <p class="title">逝去的假期</p>
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
          <p class="color_ccc"
             v-if="item.dayTime === 1">昨天</p>
          <p class="color_ccc"
             v-else-if="item.dayTime === 2">前天</p>
          <p class="color_ccc"
             v-else>已经过去{{item.dayTime}}天</p>
        </div>
      </div>
    </div>
    <mp-toast type="error"
              v-model="showToast"
              content="今年假期还没到哦"
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
      data: [],
      array: ['2019', '2020', '2021', '2022'],
      today: new Date().getTime(),
      day: 86400000,
      showToast: false,
      number: 0,
    }
  },

  methods: {
    filter (arr) {
      this.data = []
      arr.forEach(item => {
        if ((item.timestamp + this.day) < this.today) {
          item.dayTime = Math.ceil((item.timestamp - this.today) / this.day) * -1
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
    },
    PickerChange (e) {
      var year = this.array[e.mp.detail.value]
      switch (year) {
        case '2019':
          this.number = 0
          break;
        case '2020':
          this.number = 1
          break;
        case '2021':
          this.number = 2
          break;
        case '2022':
          this.number = 3
          break;
        default:
          break;
      }
      var arr = date[this.number]
      this.$nextTick(() => {
        this.filter(arr)
      })

    }
  },

  created () {
    var dateArray = date[this.number]
    this.filter(dateArray)
  }
}
</script>

<style scoped>
page {
  height: 100%;
}

.container_box {
  height: 100%;
}
.title {
  text-align: center;
  font-family: Georgia, "Times New Roman", Times, serif;
}
.box {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
}
.weui-btn {
  width: 60%;
  margin: 0 auto;
  padding-bottom: 20px;
}
.list {
  float: left;
  display: flex;
  width: 35%;
  flex-direction: column;
  align-items: center;
  border: 1px solid #ccc;
  padding: 15px;
  margin: 10px 0;
}
.color_ccc {
  color: #ccc;
}
</style>
