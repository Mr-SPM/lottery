<template>
    <div>
        <h1>Lottery</h1>
        <div class="content">
            <table :class="{'finish':tempCircle === numbers.length && tempCircle !== 0}">
                <tr>
                    <td :class="{'current':tempIndex == 1}">1</td>
                    <td :class="{'current':tempIndex == 2}">2</td>
                    <td :class="{'current':tempIndex == 3}">3</td>
                </tr>
                <tr>
                    <td :class="{'current':tempIndex == 0}">0</td>
                    <td rowspan="2" @click="start()">
                        Start
                    </td>
                    <td :class="{'current':tempIndex == 4}">4</td>
                </tr>
                <tr>
                    <td :class="{'current':tempIndex == 9}">9</td>
                    <td :class="{'current':tempIndex == 5}">5</td>
                </tr>
                <tr>
                    <td :class="{'current':tempIndex == 8}">8</td>
                    <td :class="{'current':tempIndex == 7}">7</td>
                    <td :class="{'current':tempIndex == 6}">6</td>
                </tr>
            </table>
        </div>
        <div>
            <span>人数：</span><input type="text" placeholder="人数" v-model="max" maxlength="3">
        </div>
        <p>中奖人：{{message}}</p>
        <p v-if="tempCircle === numbers.length && tempCircle !== 0">恭喜中奖!!!</p>
        <div>
            <p>中奖列表</p>
            <ul>
                <li v-for="item in array" :key="item">{{item}}</li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Lottery',
    data() {
        return {
            message: '',
            max: '100',
            numbers: [],
            target: '',
            tempIndex: '',
            tempCircle: 0,
            array: []
        };
    },
    methods: {
        start: function () {
            if (!this.max) {
                window.alert('人数设置不能为空');
            }
            this.message = '';
            this.target = Math.round(Math.random() * this.max).toString();
            this.numbers = this.target.split('');
            this.tempCircle = 0;
            this.render();
        },
        choose: function () {
            let temp = Math.round(Math.random() * this.max).toString();
            if (this.array.indexOf(temp) === -1) {
                return temp;
            } else {
                this.choose();
            }
        },
        render: function () {
            this.tempIndex = 0;
            let circle = 0;
            let sum = 20 + parseInt(this.numbers[this.tempCircle]);
            let st =
                setInterval(() => {
                    this.tempIndex += 1;
                    circle += 1;
                    this.tempIndex = circle % 10;
                    if (circle === sum) {
                        this.message += this.numbers[this.tempCircle];
                        window.clearInterval(st);
                        if (this.tempCircle < this.numbers.length - 1) {
                            setTimeout(() => {
                                this.tempCircle += 1;
                                this.render();
                            }, 1000);
                        } else {
                            setTimeout(() => {
                                this.tempCircle += 1;
                                this.tempIndex = 0;
                                this.array.push(this.target);
                            }, 1000);
                        }
                    }
                }, 100);
        }
    }
};
</script>

<style lang="scss" scoped>
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  table {
    margin: 0;
    padding: 0;
    border: 1px solid #ccc;
    // border-collapse: collapse;
    border-spacing: 0;
    border-radius: 5px;
    color: #333;
    &.finish {
      td {
        background-color: rgb(241, 95, 95) !important;
        border-color: rgb(216, 77, 77);
        color: #333 !important;
      }
    }
    tr {
      td {
        width: 80px;
        height: 60px;
        border: 1px solid #ccc;
        &.current {
          background-color: aqua;
          color: #fff;
        }
      }
    }
  }
}
</style>
