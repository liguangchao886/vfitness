<template>
    <div>
        <div class="upload_area">
            <textarea class="textarea" maxlength="140" v-model="text" />
            <div class="pic_choice" @tap="doUploadPhoto">
                <div class="addType">
                    <img :src="url">
                </div>
            </div>
        </div>
        <button @tap="onAdd">发布</button>
    </div>
</template>

<script>
    var util = require('../../utils/util.js');
    export default {
        data() {
            return {
                nickName: '',
                avatarUrl: '',
                text: '',
                openid: '',
                url: '/static/tab/add.png',
                time: '',
                time2: '',
                cloudPath: '',
                filePath: '',
                org_cloudPath: ''
            }
        },
        mounted() {
            this.url = '/static/tab/add.png' //  初始化输入框和图片
            this.text = ''
            this.openid = wx.getStorageSync('openid')
            console.log("openid：" + this.openid)
            this.handelGetUserInfo()

        },
        methods: {
            handelGetUserInfo() {
                wx.getUserInfo({
                    success: data => {
                        console.log(data);
                        this.nickName = data.userInfo.nickName;
                        this.avatarUrl = data.userInfo.avatarUrl;
                    },
                    fail: () => {
                        console.log("获取失败");
                        wx.showToast({
                            title: '请先获取授权',
                            icon: 'none',
                            duration: 2000
                        })
                    }
                });
            },
            doUploadPhoto() {
                // 选择图片
                var that = this;
                wx.chooseImage({
                    count: 2,
                    sizeType: ['compressed'],
                    sourceType: ['album', 'camera'],
                    success: function (res) {
                        const filePath = res.tempFilePaths[0]
                        that.url = filePath
                        console.log('图片地址：' + that.url)
                        that.getTime()
                        // 上传图片
                        const org_cloudPath = filePath.match(/\.[^.]+?$/)[0]
                        const cloudPath = 'society/' + that.openid + '/' + that.time + filePath.match(/\.[^.]+?$/)[0]
                        that.filePath = filePath
                        that.org_cloudPath = org_cloudPath
                        that.cloudPath = cloudPath
                        console.log(cloudPath)
                    },
                    fail: e => {
                        console.error(e)
                    }
                })
            },
            onAdd() {
                var that = this
                const db = wx.cloud.database()
                if (that.org_cloudPath === '') {

                    console.log('图片为空')
                    wx.showToast({
                        title: '图片不能为空',
                        icon: 'none',
                        duration: 10000
                    })

                    setTimeout(function () {
                        wx.hideToast()
                    }, 2000)
                    return;

                }
                db.collection('society_data').add({
                    //  openid为数据库_id

                    data: {
                        _id: this.openid + ' ' + this.time,
                        text: this.text,
                        time: this.time2,
                        nickName: this.nickName,
                        avatarUrl: this.avatarUrl,
                        url: 'cloud://liguangchao-u14ya.6c69-liguangchao-u14ya-1300277699/' + this.cloudPath
                    },
                    success: res => {

                        console.log('发布成功')
                    },

                }),
                    wx.cloud.uploadFile({
                        cloudPath: this.cloudPath,
                        filePath: this.filePath,
                        success: res => {
                            console.log('[上传文件] 成功：', res)
                            app.globalData.fileID = res.fileID
                            app.globalData.cloudPath = cloudPath
                            app.globalData.imagePath = filePath

                        },
                        fail: e => {
                            if (cloudPath == '') {
                                console.error('[上传文件] 失败：', e)
                            }
                            wx.showToast({
                                icon: 'none',
                                title: '上传失败',
                            })
                        }
                    })
                wx.showToast({
                    title: '发布成功',
                    icon: 'success',
                    duration: 2000
                })
                setTimeout(function () {
                    //  保存后返回上一页
                    wx.navigateBack({
                        delta: 1,
                    })
                }, 2000)
            },
            getTime() {
                // 调用函数时，传入new Date()参数，返回值是日期和时间  
                var time = util.formatTime(new Date());
                // 再通过setData更改Page()里面的data，动态更新页面的数据  
                this.time = time

                var time2 = util.formatTime2(new Date());
                this.time2 = time2

            }

        }
    }

</script>

<style>
    .upload_area{
        height: 400rpx;
        width: 100%;
    }
    .textarea{
        margin:20rpx auto 0 auto;
        width: 95%;
        height: 200rpx;
    }

    .pic_choice{
        height: 150rpx;
        width: 150rpx;
        background-color: #eee;
        position: absolute;
        margin-left: 30rpx;
        border-radius: 20rpx;
    }

    .addType{
        height: 100rpx;
        width: 100rpx;
        position: relative;
        margin:0 auto;
        top: 50%;
        transform: translateY(-50%);
    }

    .addType img{
        height: 100rpx;
        width: 100rpx;
    }

</style>