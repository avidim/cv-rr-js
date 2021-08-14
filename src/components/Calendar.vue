<template>
    <div>
        <div class="cal-wrapper">
            <div class="cal-wrapper-header">
                <span @click="prevMonth">&#8701;</span>
                <span>{{ properTodayValue }}</span>
                <span @click="nextMonth">&#8702;</span>
            </div>
            <div class="cal-wrapper-content">
                <div class="content-week">
                    <div>Mon</div>
                    <div>Tue</div>
                    <div>Wed</div>
                    <div>Thu</div>
                    <div>Fri</div>
                    <div>Sat</div>
                    <div>Sun</div>
                </div>
                <div class="content-days">
                    <div v-for="value in getMonthDayAmount"
                         :key="value" 
                         :class="{ 'active': value == day }" 
                         @click="chooseDate(value)" 
                         v-html="value">
                    </div>
                </div>
            </div>
            <input type="text" :value="choosedDate" readonly/>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            date: {
                type: String,
                required: false,
            }
        },
        data() {
            return {
                data: {
                    months: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
                    monthDayPair: {
                        'Jan': 31, 
                        'Feb': 28, 
                        'Mar': 31, 
                        'Apr': 30, 
                        'May': 31, 
                        'Jun': 30, 
                        'Jul': 31, 
                        'Aug': 31, 
                        'Sep': 30, 
                        'Oct': 31, 
                        'Nov': 30, 
                        'Dec': 31
                    }
                },
                day: null,
                month: null,
                year: null,
                choosedDate: '',
            }
        },
        created() {
            let arr = this.date || this.todayDate();
            arr = arr.split('-');
            this.day = arr[2];
            this.month = arr[1] - 1;
            this.year = arr[0];
        },
        methods: {
            todayDate() {
                let today = new Date();
                return today.getFullYear() + '-' + (today.getMonth() + 1) + '-' + today.getDate();
            },
            prevMonth() {
                if (this.month == 0) {
                    this.year--;
                    this.month = 11;
                    return;
                }   
                this.month--;
            },
            nextMonth() {
                if (this.month == 11) {
                    this.year++;
                    this.month = 0;
                    return;
                }  
                this.month++;
            },
            chooseDate(value) {
                this.day = value;
                this.choosedDate = `${this.day}-${this.month}-${this.year}`
            }
        },
        computed: {
            properTodayValue() {
                return this.data.months[this.month] + ' ' + this.year;
            },
            getMonthDayAmount() {
                return this.data.monthDayPair[this.data.months[this.month]];
            }
        }
    }
</script>

<style lang="css" scoped>
.cal-wrapper {
    margin: 0 auto;
    width: 250px;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.cal-wrapper input {
    width: 80px;
    margin: 10px auto;
    text-align: center;
}

.cal-wrapper-header {
    display: flex;
    justify-content: space-evenly;
}

.cal-wrapper-header span:not(:nth-child(2)) {
    cursor: pointer;
}

.cal-wrapper-content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-size: 12px;
}

.content-week {
    display: flex;
    margin: 10px 0;
}

.content-week div {
    width: 35px;
}

.content-days {
    display: flex;
    flex-wrap: wrap;
}

.content-days div {
    width: 35px;
    cursor: pointer;
    margin: 5px 0;
}

.active {
    border: 1px solid black;
}
</style>