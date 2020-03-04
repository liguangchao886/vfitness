<template>
    <div>
        <div class="form_input">
            <span class="form_left">教练姓名:</span>
            <input class="form_right" type="text" v-model="coach_name" disabled="disabled" />
        </div>
        <div class="form_input">
            <span class="form_left">日期:</span>
            <picker mode="multiSelector" @change="bindMultiPickerChange" :value="multiIndex" :range="newMultiArray">
                <span class="form_right">{{time}}</span>
            </picker>
        </div>
        <div class="form_input">
            <span class="form_left">备注:</span>
            <input class="form_right" type="text" v-model="remark" placeholder="请输入备注..." />
        </div>
        <button @tap="onAddFeedback">确 定</button>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                username: '',
                coach_name: '',
                time: '',
                remark: '',
                multiArray: [],
                multiIndex: [0, 0, 0, 0, 0]
            }
        },
        onLoad(options) {
            this.coach_name = options.coachname
        },
        mounted() {
            this.time = ''
        },
        computed: {
            newMultiArray: () => {
                let array = [];
                const date = new Date();
                const years = [];
                const months = [];
                const days = [];

                for (let i = 2019; i <= date.getFullYear() + 10; i++) {
                    years.push("" + i);
                }
                array.push(years);

                for (let i = 1; i <= 12; i++) {
                    if (i < 10) {
                        i = "0" + i;
                    }
                    months.push("" + i);
                }
                array.push(months);

                for (let i = 1; i <= 31; i++) {
                    if (i < 10) {
                        i = "0" + i;
                    }
                    days.push("" + i);
                }
                array.push(days);
                return array;
            }
        },
        methods: {
            bindMultiPickerChange(e) {
                this.multiIndex = e.target.value;
                const index = this.multiIndex;
                const year = this.newMultiArray[0][index[0]];
                const month = this.newMultiArray[1][index[1]];
                const day = this.newMultiArray[2][index[2]];
                this.time = year + "-" + month + "-" + day
                console.log(this.time)
            },
            onAddFeedback() {
                const db = wx.cloud.database()
                db.collection('class_order').add({
                    //  openid为数据库_id
                    data: {
                        coach_name: this.coach_name,
                        time: this.time,
                        remark: this.remark,
                        name: this.username
                    },
                    success: res => {
                        wx.showToast({
                            title: '预约成功',
                            icon: 'success',
                            duration: 2000
                        })
                        setTimeout(function () {
                            //  保存后返回上一页
                            wx.navigateBack({
                                delta: 2,
                            })
                        }, 1000)

                    }
                })
            },
            getUserName() {
                var that = this;
                const db = wx.cloud.database();
                db.collection('user_data').get({
                    success: function (res) {
                        that.username = res.data[0].name
                    }
                })
            }
        },

    }

</script>

<style>
    .form_input {
        margin: 20rpx auto 0 auto;
        width: 96%;
        height: 100rpx;
        line-height: 100rpx;
        border-bottom: 1rpx #eee solid
    }

    .form_left {
        padding-left: 20rpx;
        width: 20%;
        float: left;
        line-height: 100rpx;
    }

    .form_right {
        float: left;
        width: 73%;
        line-height: 100rpx;
        height: 100rpx;
        text-align: right;
        letter-spacing: 2rpx;
    }

    button {
        width: 95%;
        margin: 50rpx auto 0 auto;
    }
</style>