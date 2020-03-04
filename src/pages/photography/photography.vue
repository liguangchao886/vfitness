<template>
    <div>
        <div class="photography_container">
            <div class="avatar_line">
                <div class="avatar">
                    <span class="nickname">{{nickName}}</span>
                    <img :src="avatarUrl" alt="">
                </div>
            </div>
        </div>
        <div class="photography" v-for="(item, index) in society_data_Arr">
            <img class="photo_img" :src="item.url" :data-src="item.url" alt="" mode="aspectFit"
                @tap='previewMoreImage' />
            <div class="photo_p_d">
                <p class="photo_p">{{item.text}}</p>
                <span class="date">{{item.time}}</span>
                <span class="delete" @tap="onRemove(index)">删除</span>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                society_data_Arr: [],
                nickName: '',
                avatarUrl: '',
                time: '',
                openid: '',
                refresh: false
            }
        },
        onLoad(options) {
            console.log('nickName:' + options.nickName + ' url:' + options.avatarUrl)
            this.nickName = options.nickName
            this.avatarUrl = options.avatarUrl
            this.openid = options.openid
        },
        mounted() {
            this.getSociety_data()
            console.log(this.society_data_Arr)
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
            getSociety_data() {
                var that = this
                const db = wx.cloud.database({});
                const table = db.collection('society_data');
                table.where({
                    _openid: this.openid
                }).get({
                    success: function (res) {
                        console.log(res.data)
                        that.society_data_Arr = res.data.reverse()
                    }
                })
            },
            getImgUrl(item) {
                // var url = 'cloud://liguangchao-u14ya.6c69-liguangchao-u14ya-1300277699/'+ item.url
                console.log(url)
                return url

            },
            previewMoreImage(e) {   //  点击查看大图
                let src = e.currentTarget.dataset.src;
                let urlarr = [];
                urlarr.push(src)
                wx.previewImage({
                    current: src,
                    urls: urlarr
                })
            },
            onRemove(index) {
                var that = this
                wx.showModal({
                    title: '删除',
                    content: '确定要删除吗？',
                    success: function (res) {
                        if (res.confirm) {  //  如果选择确定
                            const db = wx.cloud.database()
                            db.collection('society_data').doc(that.society_data_Arr[index]._id).remove({
                                success: res => {
                                    console.log('hhh')
                                    that.getSociety_data()  //  重新加载数据=刷新
                                    wx.showToast({
                                        title: '删除成功',
                                    })
                                },
                                fail: err => {
                                    wx.showToast({
                                        icon: 'none',
                                        title: '删除失败',
                                    })
                                    console.error('[数据库] [删除记录] 失败：', err)
                                }
                            })
                        }
                    },
                })
            }
        }

    }
</script>

<style>
    .photography_container {
        width: 100%;
        height: 200rpx;
        border-bottom: 1rpx solid #eee
    }

    .avatar_line {
        position: relative;
        padding-top: 50rpx;
    }

    .avatar {
        float: right;
        margin-right: 30rpx;
    }

    .nickname {
        font-size: 48rpx;
        margin-right: 30rpx;
    }

    .avatar img {
        width: 200rpx;
        height: 200rpx;
        margin-right: 30rpx;
        border-radius: 175rpx;
        vertical-align: middle;
    }

    .line {
        position: absolute;
        top: 100rpx;
        margin-top: 30rpx;
        left: 5%;
        width: 40%;
        height: 4rpx;
        background: white
    }

    .photography {
        width: 80%;
        margin: 0 auto;
        margin-top: 80rpx;
        display: flex;
    }

    .photo_img {
        width: 150rpx;
        height: 150rpx;

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
        letter-spacing: 2rpx;
    }

    .date {
        height: 26rpx;
        font-size: 28rpx;
        color: #666;
    }

    .delete {
        float: right;
        height: 26rpx;
        font-size: 28rpx;
        color: rgb(101, 151, 218);
        letter-spacing: 1rpx;

    }
</style>