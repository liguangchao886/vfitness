<template>
    <div>

        <div class="photography" v-for="(item, index) in society_data_Arr">
            <div class="userinfo">
                <img :src="item.avatarUrl" class="avatar" alt="">
                <span class="nickname">{{item.nickName}}</span>
                <span class="date">{{item.time}}</span>
            </div>
            <div class="photo_box">
                <img class="photo" :src="item.url" :data-src="item.url" alt="" mode="aspectFit" @tap='previewMoreImage'>
            </div>
            <p class="text">{{item.text}}</p>
        </div>
        <div class="btn" @tap="goUpload">
            <img src="../../../static/tab/orders.png" alt="" mode="aspectFit">
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                society_data_Arr: [],
            }
        },
        mounted() {
            this.getSociety_data()

        },
        onPullDownRefresh() {
            var that = this
            wx.showNavigationBarLoading() //在标题栏中显示加载
            this.getSociety_data()
            wx.showToast({
                title: 'loading....',
                icon: 'loading'
            })
            console.log('loading...')
            wx.hideNavigationBarLoading() //完成停止加载
            wx.stopPullDownRefresh() //停止下拉刷新
        },
        methods: {
            goUpload() {
                wx.navigateTo({
                    url: '/pages/upload/main'
                })
            },
            getSociety_data() {
                var that = this
                const db = wx.cloud.database({});
                const table = db.collection('society_data');
                table.get({
                    success: function (res) {
                        console.log(res.data)
                        that.society_data_Arr = res.data.reverse()
                    }
                })
            },
            previewMoreImage(e) {
                let src = e.currentTarget.dataset.src;
                let urlarr = [];
                urlarr.push(src)
                wx.previewImage({
                    current: src,
                    urls: urlarr
                })
            },
        }
    }
</script>

<style>
    .btn {
        height: 80rpx;
        width: 80rpx;
        border-top-right-radius: 50rpx;
        border-bottom-right-radius: 50rpx;
        position: sticky;
        bottom: 400rpx;
        z-index: +1;
        background-color: rgba(119, 119, 119, 0.904);
    }

    .btn img {
        float: right;
        height: 50rpx;
        width: 50rpx;
        margin: 15rpx 18rpx 0 0
    }

    .photography {
        margin: 0 auto;
        margin-top: 5rpx;
    }

    .userinfo {
        height: 75rpx;
        width: 100%;
        background: #eee;
        margin: 0 auto;
    }

    .avatar {
        height: 69rpx;
        width: 69rpx;
        border-radius: 69rpx;
        float: left;
        margin-left: 50rpx;
        margin-top: 3rpx;
    }

    .nickname {
        height: 50rpx;
        width: 100rpx;
        float: left;
        margin-left: 20rpx;
        line-height: 75rpx;
        font-size: 32rpx;
        margin-left: 50rpx;
    }

    .date {
        height: 50rpx;
        width: 35%;
        float: right;
        margin-right: 30rpx;
        line-height: 75rpx;
        font-size: 28rpx;
        color: #666;
    }

    .photo_p_d {
        padding-left: 25rpx;

    }

    .photo_p {
        height: 100rpx;
        width: 350rpx;
        line-height: 45px;
        font-size: 32rpx;
        color: #333;
        overflow: hidden;
        text-overflow: ellipsis;
        letter-spacing: 3rpx;
    }


    .photo_box {
        position: relative;
        width: 100%;
        height: 500rpx;
    }

    .photo {
        max-width: 100%;
        max-height: 100%;
        position: absolute;
        left: 0;
        right: 0;
        top: 30rpx;
        bottom: 0;
        margin: auto;

    }

    .text {
        width: 80%;
        margin: 50rpx auto 30rpx auto;
        word-wrap: break-word;
        letter-spacing: 3rpx;
        line-height: 36rpx;
        font-size: 32rpx;
        border-bottom: 1px solid #eee;
    }
</style>