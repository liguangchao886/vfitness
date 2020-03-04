<template>
    <div>
        <div class="noticeContainer" @tap="tonoticeDetail(index)" v-for="(item, index) in noticedata" :key="index">
            <div class="nitoceinfo">
                <p class="notice_text">公告：{{item.text}}</p>
                <p class="notice_date">时间：{{item.date}}</p>
            </div>
            <p class="notice_point">></p>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                noticedata: []
            }
        },
        mounted() {
            this.getNoticeData()
        },
        methods: {
            getNoticeData() {
                var that = this;
                const db = wx.cloud.database();
                db.collection('notice').get({
                    success: function (res) {
                        /* this.noticedata=res.data*/
                        console.log(res)
                        that.noticedata = res.data
                    }
                })
            },
            tonoticeDetail(id) {
                wx.navigateTo({
                    url: '/pages/noticeDetail/main?pID=' + id
                })
            }
        }
    }

</script>

<style>
    .noticeContainer {
        padding: 20rpx;
        display: flex;
        border-bottom: 1rpx solid #eee;
        height: 100rpx;
    }

    .nitoceinfo {
        width: 90%
    }

    .notice_text {
        width: 80%;
        font-size: 32rpx;
        color: #333;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        margin: 10rpx 0 0 30rpx;
    }

    .notice_date {
        font-size: 28rpx;
        color: #999;
        margin: 10rpx 0 0 30rpx;
    }

    .notice_point {
        font-size: 50rpx;
        line-height: 100rpx;
    }
</style>