<template>
    <div class="Code">
        <button @tap="getQRcode" class="code" v-if="!isShow">获取二维码</button>
        <div class="code" v-if="isShow">
            <img :src="qrcodeURL">
        </div>
    </div>
</template>

<script>
    const QR = require('../../utils/weapp-qrcode.js')
    export default {
        data() {
            return {
                ID: '',
                qrcodeURL: '',
                isShow: false
            }
        },
        mounted() {
            this.isShow=false
            this.ID=''
            this.qrcodeURL=''
            this.getUserInfo()
        },
        created() {
        },
        methods: {
            getUserInfo() {
                var that = this;
                const db = wx.cloud.database();
                db.collection('user_data').get({
                    success: function (res) {
                        that.ID = res.data[0].ID
                        console.log('ID:' + that.ID)
                    }
                })
            },
            getQRcode() {
                this.isShow = true
                if (this.ID === '') {
                    wx.showToast({
                        title: 'ID不能为空',
                        icon: 'none',
                        duration: 10000
                    })

                    setTimeout(function () {
                        //  保存后返回上一页
                        wx.navigateBack({
                            delta: 1,
                        })
                    }, 2000)
                    return;
                } else {
                    var imgData = QR.drawImg(this.ID, {
                        typeNumber: 8,
                        errorCorrectLevel: 'M',
                        size: 500
                    })
                    this.qrcodeURL = imgData;
                }
            }
        }
    }
</script>

<style>
    .Code {
        position: absolute;
        height: 90%;
        width: 100%;
        margin: 0 auto;
    }

    .code {
        position: relative;
        height: 400rpx;
        width: 400rpx;
        margin: auto;
        top: 50%;
        transform: translateY(-50%);
        line-height: 400rpx;
    }

    .code img {
        height: 400rpx;
        width: 400rpx;
    }
</style>