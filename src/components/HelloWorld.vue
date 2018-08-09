<template>
   <div class="calendar" id="calendar">
        <button class="month-less" @click="prevMonth"></button>
        <h4>{{year}}年{{month}}月</h4>
        <button class="month-add" @click="nextMonth"></button>
        <table class="sign_tab" border="0px" cellpadding="0px" cellspacing="0px">
            <thead>
                <tr>
                    <th>日</th>
                    <th>一</th>
                    <th>二</th>
                    <th>三</th>
                    <th>四</th>
                    <th>五</th>
                    <th>六</th>
                </tr>
            </thead>
            <tbody>
                <tr :key="item.id" v-for="item in dateArr" v-if="contains(item)">
                    <td :key="index" v-for="(data,index) in item" :class="{'check_end':data.endOnoff,'check_today':data.todayOnoff,'check_miss':data.missOnoff}">
                        {{data.day}}
                        </td>
                    <!--<td v-else :class="{'check_miss':missOnoff}">
                        {{data}}
                    </td> -->
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script type="text/javascript">
    export default {
        data () {
            return {
                year: '',
                month: '',
                dateArr: []
            }
        },
        mounted () {
            let date = new Date()
            this.year = date.getFullYear()
            this.month = date.getMonth() + 1
            this.getCalendar()
        },
        methods: {
            getLen() {
                const month = this.month;
                if (month == 2) {
                    if (this.isLeap) {
                        return 29;
                    } else {
                        return 28;
                    }
                } else {
                    if (month < 8) {
                        if (month % 2 > 0) {
                            return 31;
                        } else {
                            return 30;
                        }
                    } else {
                        if (month % 2 > 0) {
                            return 30;
                        } else {
                            return 31;
                        }
                    }
                }
            },
            contains(arr) { // 判断这一周以内是否在日期范围内
                if (
                    arr[0] == "" &&
                    arr[1] == "" &&
                    arr[2] == "" &&
                    arr[3] == "" &&
                    arr[4] == "" &&
                    arr[5] == "" &&
                    arr[6] == ""
                ) {
                    return false;
                } else {
                    return true;
                }
            },
            prevMonth() {
                if (this.month == 1) {
                    this.year--;
                    this.month = 12;
                } else {
                    this.month--;
                    this.omonth =
                        this.month < 10 ? "0" + "" + this.month : this.month;
                    this.prevSinTime = this.year + "" + this.omonth;
                }
                this.getCalendar()
            },
            nextMonth() {
                if (this.month == 12) {
                    this.year++;
                    this.month = 1;
                } else {
                    this.month++;
                    this.omonth =
                        this.month < 10 ? "0" + "" + this.month : this.month;
                    this.prevSinTime = this.year + "" + this.omonth;
                }
                this.getCalendar()
            },
            getCalendar() {
                var len = this.getLen();
                var d = new Date(this.year, this.month - 1, 1); // 获取当月的第一天的时间
                var dfw = d.getDay(); // 获取当天是星期几
                var arr = new Array();
                var tem = 0;
                for (var i = 0; i < 6; i++) {
                    arr[i] = new Array(); // 日历一个月显示六个礼拜
                    for (var j = 0; j < 7; j++) { // 一个礼拜七天
                        tem++;
                        if (tem - dfw > 0 && tem - dfw <= len) { // 判断是否在一个月内
                            // 判断当前日期
                            if (this.month == new Date().getMonth() + 1 && tem - dfw == new Date().getDate()) {
                                var obj = {
                                    day: tem - dfw,
                                    missOnoff: false,
                                    endOnoff: false,
                                    todayOnoff: true
                                }
                            } else if (
                                (this.month == new Date().getMonth() + 1 && tem - dfw >= new Date().getDate()) 
                                || this.month >= new Date().getMonth() 
                                || this.year >= new Date().getFullYear()
                            ){ 
                                 var obj = {
                                    day: tem - dfw,
                                    missOnoff: false,
                                    endOnoff: true
                                }
                            } else {
                                var obj = {
                                    day: tem - dfw,
                                    missOnoff: true,
                                    endOnoff: false
                                }
                            }
                            arr[i][j] = obj
                        } else { // 不在这个月内
                            arr[i][j] = "";
                        }
                    }
                }
                this.dateArr = arr;
            },
        }
    }
</script>

<style scoped>
.calendar {
    width: 610px;
    height: 602px;
    position: absolute;
    padding: 20px 40px 28px 40px;
    background: indianred;
}
/* 今日签到 */
.check_today {
    background: url(../assets/findImg/check_today.png) no-repeat 12px 0;
    background-size: 74px 70px;
    color: #f74c4c;
    font-size: 34px;
}
/* 过日期未签到 */
.check_miss {
    color: #ffc7c7;
}

/* 插件css */
.calendar h4 {
    margin-left: 40px;
    color: #f4f4f4;
    text-align: center;
    font-size: 36px;
    font-weight: 700;
    margin-top: 10px;
}
.calendar button {
    position: absolute;
    width: 24px;
    height: 24px;
    top: 15px;
    border: none;
}
.calendar button:focus {
    border: none;
    outline: none;
}
.calendar button.month-less {
    top: 40px;
    left: 200px;
    width: 18px;
    height: 34px;
    background: url(../assets/findImg/prev_next.png) no-repeat;
    background-size: 18px 68px;
    cursor: pointer;
}
.calendar button.month-add {
    top: 40px;
    right: 150px;
    width: 18px;
    height: 34px;
    background: url(../assets/findImg/prev_next.png) no-repeat 0 -35px;
    background-size: 18px 68px;
    cursor: pointer;
}
.calendar .sign_tab {
    width: 100%;
    height: 505px;
    border-collapse: collapse;
    border-top: 0;
    table-layout: fixed;
    color: #fff;
}
.calendar .sign_tab th {
    text-align: center;
    height: 72px;
    font-weight: 700;
    color: #ffc7c7;
}
.calendar .sign_tab td {
    text-align: center;
    width: 97px;
    height: 72px;
    font-size: 30px;
}
.calendar .check_end {
    background: url(../assets/findImg/check_end.png) no-repeat 16px 15px;
    background-size: 54px 54px;
    color: #f74c4c;
    font-size: 34px;
}
</style>
