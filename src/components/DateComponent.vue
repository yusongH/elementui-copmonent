<template>
    <div>
        <el-date-picker
        popper-class="now-data-picker"
        v-model="myTime"
        :picker-options="{
            disabledDate(time) {
                return time.getTime() < Date.now() - 1 * 24 * 3600 * 1000;
            },
            selectableRange: startTimeRange
        }"
        :editable="false"
        type="datetime"
        placeholder="请选择时间"
        format="yyyy-MM-dd HH:mm:ss"
        value-format="yyyy-MM-dd HH:mm:ss"
        ></el-date-picker>
    </div>
</template>

<script>
import moment from 'moment'

export default {
    name: "DateComponent",
    data() {
        return {
            // 时间
            myTime: '',
            // 可选的时间范围
            startTimeRange: ''
        }
    },
    watch: {
        myTime:{
            handler(newValue, oldValue) {
                if(newValue){
                    let nowDate = moment().format('YYYY-MM-DD HH:mm:ss');
                    let dt = nowDate.split(" ");
                    let st = '';

                    if(newValue.split(" ")[0] == dt[0]){
                        // 如果是当天,选择的时间开始为此刻的时分秒
                        st = dt[1];
                        // 如果是当天（只有当日期改变时）,默认选中时间为当前时间
                        if (newValue.split(" ")[0] != oldValue.split(" ")[0]) {
                            this.myTime = nowDate;
                        }
                    }else{
                        // 明天以及后面的时间从0时开始
                        st = '00:00:00';
                    }
                    
                    this.startTimeRange =  st + ' - 23:59:59'; 
                    //例如：如果今天此刻时间为10:41:40 则选择时间范围为： 10:41:40 - 23:59:59  
                    //否则为：00:00:00 - 23:59:59
                } 
            }
        }
    },
};
</script>

<style>
</style>