<template>
    <div>
        <div class="coachContainer" @tap="tocoachDetail(index)" v-for="(item, index) in coachdata" :key="index">
            <img class="coach_img" :src="item.coach_photo" alt="" mode="aspectFit">
            <div class="coachinfo">
                <p class="coach_name">姓名：{{item.coach_name}}</p>
                <p class="coach_year">年龄：{{item.coach_year}}</p>
                <p class="coach_cot">简介：{{item.coach_cot}}</p>
            </div>
            <p class="coach_point">></p>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                coachdata: [],
            }
        },
        mounted() {
            this.getCoachData()
        },

        methods: {
            getCoachData() {
                var that = this;
                const db = wx.cloud.database();
                db.collection('coach').get({
                    success: function (res) {
                        /* this.coachdata=res.data*/
                        console.log(res)
                        that.coachdata = res.data
                    }
                })
            },
            tocoachDetail(id) {
                wx.navigateTo({
                    url: '/pages/coachDetail/main?pID=' + id
                })
            }
        }
    }

</script>

<style>
    .coachContainer {
        padding: 20rpx;
        display: flex;
        border-bottom: 1rpx solid #eee;
    }

    .coach_img {
        padding-left: 20rpx;
        height: 128rpx;
        width: 128rpx;
        margin-right: 40rpx;
    }

    .coachinfo {
        width: 50%
    }

    .coach_name {
        font-size: 32rpx;
        color: #333;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }

    .coach_year {
        font-size: 28rpx;
        color: #999;
        margin: 10rpx 0;
    }

    .coach_cot {
        font-size: 30rpx;
        color: #666;
    }

    .coach_point {
        font-size: 50rpx;
        line-height: 128rpx;
        padding-left: 100rpx
    }
</style>