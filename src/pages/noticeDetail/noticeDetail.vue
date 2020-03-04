<template>
    <div class="noticeDetailContainer" v-if="noticedata[index_id]">
        <div class="detail_content">
            <span>{{noticedata[index_id].text}}</span>
            <span class="date">{{noticedata[index_id].date}}</span>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                noticedata: [],
                index_id: '',
                link: ''
            }
        },
        onLoad(options) {
            console.log('第' + options.pID + '条公告')
            var i_id = options.pID
            this.index_id = i_id
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
            }
        }
    }

</script>

<style>
    .noticeDetailContainer {
        border: 1rpx #eee dashed;
        width: 80%;
        margin: 200rpx auto 0 auto;
    }


    .detail_content {
        display: flex;
        flex-direction: column;
        margin-top: 100rpx;
        line-height: 50rpx;
        text-align: center;
        font-size: 32rpx;
        color: #333;
    }

    .date {
        margin-top: 80rpx;
        text-align: right;
        font-size: 28rpx;
        color: #666;
    }
</style>