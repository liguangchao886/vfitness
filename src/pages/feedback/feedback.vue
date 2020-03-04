<template>
    <div>
        <div class="textArea">
            <textarea class="textarea" maxlength="-1" v-model="feedback_text" placeholder="请输入反馈信息..."></textarea>
        </div>
        <div class="form_imput">
            <span class="form_left">联系方式:</span>
            <input class="form_right" type="text" v-model="phone" placeholder="请输入联系方式..." />
        </div>
        <button @tap="onAddFeedback">确 定</button>
    </div>
</template>

<script>
    var util = require('../../utils/util.js');
    export default {
        data() {
            return {
                openid: '',
                feedback_text: '',
                phone: '',
                date: '',
                name: ''
            }
        },
        onLoad(options) {
            console.log('openid:' + options._openid)
            this.openid = options.openid
        },
        mounted() {
            this.feedback_text = ''
            this.phone = ''
            this.getUserInfo()
        },
        methods: {
            getUserInfo() {
                var that = this;
                const db = wx.cloud.database();
                db.collection('user_data').get({
                    success: function (res) {
                        that.name = res.data[0].name
                    }
                })
            },
            onAddFeedback() {
                this.getTime()
                const db = wx.cloud.database()
                db.collection('feedback').add({
                    //  openid为数据库_id
                    data: {
                        _openid: this.openid,
                        text: this.feedback_text,
                        phone: this.phone,
                        date: this.time,
                        name: this.name
                    },
                    success: res => {
                        wx.showToast({
                            title: '反馈成功',
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
                        that.name = res.data[0].name
                    }
                })
            },
            getTime() {
                // 调用函数时，传入new Date()参数，返回值是日期和时间  
                // 再通过setData更改Page()里面的data，动态更新页面的数据  
                var time = util.formatTime2(new Date());
                this.time = time

            }
        }
    }
</script>

<style>
    .textArea{
        border: #666 1rpx solid;
        margin: 20rpx auto 0 auto;
        width: 96%;
        
    }
    .textarea {
        margin: 20rpx auto 20rpx auto;
        width: 95%;
        height: 400rpx;
        letter-spacing: 3rpx;
    }

    .form_imput {
        margin: 20rpx auto 0 auto;
        width: 96%;
        height: 100rpx;
        line-height: 100rpx;
    }

    .form_left {
        padding-left: 20rpx;
        width: 20%;
        float: left;
        line-height: 100rpx;
    }

    .form_right {
        float: right;
        width: 73%;
        line-height: 100rpx;
        height: 100rpx;
        text-align: left;
        border-bottom: #666 1rpx solid;
        letter-spacing: 2rpx;
    }

    button {
        width: 95%;
        margin: 50rpx auto 0 auto;
    }
</style>