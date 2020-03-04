<template>
    <div class="page_table">
        <div class="userContainer">
            <div class="header">
                <img v-if="isShow" :src="userInfo.avatarUrl" alt="" />
                <Button class="btn" v-else open-type="getUserInfo" @getuserinfo="getUserInfo">点击获取信息</Button>
            </div>
            <div class="userContainer_Name" v-if="isShow">hello&nbsp;&nbsp;{{userInfo.nickName}}</div>
        </div>
        <div class="Container" :class="{'noActive':noactive}">
            <div class="down_userContainer" @tap="goQrcodeDetail">
                <img class="down_userContainer_left" src="/static/tab/home.png" alt="">
                <span class="down_userContainer_text">扫码入场</span>
                <span class="down_userContainer_text_right">></span>
            </div>
            <div class="down_userContainer" @tap="goPhotography">
                <img class="down_userContainer_left" src="/static/tab/home.png" alt="">
                <span class="down_userContainer_text">相册</span>
                <span class="down_userContainer_text_right">></span>
            </div>
            <div class="down_userContainer" @tap="goUserData">
                <img class="down_userContainer_left" src="/static/tab/home.png" alt="">
                <span class="down_userContainer_text">个人资料</span>
                <span class="down_userContainer_text_right">></span>
            </div>
            <div class="down_userContainer" @tap="goFeedback" style="border-top: 15rpx #eee solid">
                <img class="down_userContainer_left" src="/static/tab/home.png" alt="">
                <span class="down_userContainer_text">反馈信息</span>
                <span class="down_userContainer_text_right">></span>
            </div>
        </div>
    </div>

    </div>
</template>

<script>
    export default {
        data() {
            return {
                userInfo: {},
                nickName: '',
                avatarUrl: '',
                isShow: false,
                noactive: true,
                ID: '',
                openid: '',
            };
        },
        beforeMount() {
            this.handelGetUserInfo();

        },
        mounted() {

        },
        methods: {
            handelGetUserInfo() {
                wx.getUserInfo({
                    success: data => {
                        console.log(data);
                        this.userInfo = data.userInfo;
                        this.nickName = data.userInfo.nickName;
                        this.avatarUrl = data.userInfo.avatarUrl;
                        this.isShow = true;
                        this.getUserID()
                        this.onGetOpenid()
                        this.noactive = false
                        this.saveUserInfo()
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
            onGetOpenid() {
                var that = this
                wx.cloud.callFunction({
                    name: 'login',
                    data: {},
                    success: res => {
                        console.log('[云函数] [login] user openid:', res.result.openid)
                        wx.setStorage({
                            key: "openid",
                            data: res.result.openid,
                            success: function () {
                                console.log('user openid:', res.result.openid + ' 缓存成功！')
                                that.openid = res.result.openid
                            }
                        })
                    },
                    fail: err => {
                        console.error('[云函数] [login] 调用失败', err)
                    }
                })
            },
            getUserInfo(data) {
                //  如果授权 重新获取
                if (data.mp.detail.rawData) {
                    this.handelGetUserInfo();
                }
            },
            goUserData() {
                wx.navigateTo({
                    url: '/pages/userData/main'
                })
            },
            goQrcodeDetail() {
                wx.navigateTo({
                    url: '/pages/scanCode/main'
                })
            },
            goPhotography(userInfo) {
                wx.navigateTo({
                    url: '/pages/photography/main?nickName=' + this.userInfo.nickName + '&avatarUrl=' + this.userInfo.avatarUrl + '&openid=' + this.openid
                })
            },
            goFeedback(openid) {
                wx.navigateTo({
                    url: '/pages/feedback/main?_openid=' + this.openid
                })
            },
            getUserID() {
                var that = this;
                const db = wx.cloud.database();
                db.collection('user_data').get({
                    success: function (res) {
                        console.log(res)
                        that.ID = res.data[0].ID
                    }
                })
            },/* 
            getIsShow() {
                var that = this
                const db = wx.cloud.database({});
                const table = db.collection('society_data');
                table.where({
                    _openid: this.openid
                }).get({
                    success: function (res) {
                        console.log('+++' + res.data)

                    }
                })
            }, */
            saveUserInfo() {
                const db = wx.cloud.database()
                db.collection('user_info').doc(this.openid).set({
                    //  openid为数据库_id
                    data: {
                        nickName: this.userInfo.nickName,
                        avatarUrl: this.userInfo.avatarUrl
                    },
                    success: res => {
                        console.log('userInfo上传成功')
                    },
                    fail: e => {
                        console.error(e)
                    }
                })
            },
            refresh() {
                var pages = getCurrentPages();
                if (pages.length > 1) {
                    //上一个页面实例对象
                    var prePage = pages[pages.length - 2];
                    //关键在这里
                    prePage.onLoad()
                }
            },/* 
            changeIsShow() {
                if (this.s_isShow) {
                    this.s_isShow = false
                } else if (!this.s_isShow) {
                    this.s_isShow = true
                }
            }, */

        }
    }

</script>

<style>
    .page_table {
        background-color: #eee;
    }

    .userContainer {
        background-color: white;
        border-bottom: 1rpx #eee solid;
        height: 40%;
    }

    .header {
        margin: 0 auto;
        width: 200rpx;
        height: 250rpx;
        margin-top: 80rpx;
    }

    .header img {
        width: 200rpx;
        height: 200rpx;
        border-radius: 200rpx;
        margin: 0 auto;
    }

    .btn {
        width: 200rpx;
        height: 200rpx;
        border-radius: 30rpx;
        margin: 0 auto;
    }

    .userContainer_Name {
        width: 300rpx;
        height: 100rpx;
        font-size: 40rpx;
        margin: 0 auto;
        text-align: center;
    }

    .Container {
        background-color: white;
        margin-top: 2rpx;
        border-top: 15rpx #eee solid;
    }

    .down_userContainer {
        background-color: white;
        padding: 30rpx 70rpx;
        border-bottom: 1rpx #eee solid;
        display: flex;
    }

    .noActive {
        pointer-events: none
    }

    .down_userContainer_left {
        width: 50rpx;
        height: 50rpx;
    }

    .down_userContainer_text {
        margin-left: 70rpx;
        line-height: 50rpx;
        font-size: 30rpx;
        width: 70%;
        letter-spacing: 4rpx;
        font-size: 28rpx;
    }

    .userContainer_text_right {
        font-size: 35rpx;
        line-height: 50rpx;
    }

    .down_userContainer_text_right_button {
        width: 120rpx;
        height: 60rpx;
        border-radius: 60rpx;
        border: #444 1rpx solid;
        position: relative
    }

    .down_userContainer_text_right_buttonActive {
        background-color: #eee;
    }

    .circular {
        position: absolute;
        width: 50rpx;
        height: 50rpx;
        border-radius: 50rpx;
        border: #666 solid 1rpx;
        top: 5rpx;
        left: 5rpx;
    }

    .circularActive {
        left: 50rpx;
    }
</style>