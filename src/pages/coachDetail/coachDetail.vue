<template>
    <div class="coachDetailContainer" v-if="coachdata[index_id]">
        <div class="coach_photo">
            <img :src="link" alt="" mode="aspectFit">
        </div>
        <p class="coach_name">姓名：{{coachdata[index_id].coach_name}}</p>
        <div class="detail_content">
            <span>介绍：{{coachdata[index_id].coach_cot}}</span>
            <span>格言：{{coachdata[index_id].coach_motto}}</span>
        </div>
        <button @tap="goOrder">我要预约</button>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                coachdata: [],
                index_id: '',
                link:''
            }
        },
        onLoad(options) {
            console.log('第' + options.pID + '位教练')
            var i_id = options.pID
            this.index_id = i_id
        },
        mounted() {
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
        beforeMount() {
            this.link='cloud://liguangchao-u14ya.6c69-liguangchao-u14ya/coach_photo/coach_0'+this.index_id+'.jpg'
        },
        methods:{
            goOrder(){
                wx.navigateTo({
                    url: '/pages/order/main?coachname=' + this.coachdata[this.index_id].coach_name
                })
            }

        }
    }

</script>

<style>
    .coachDetailContainer {
        display: flex;
        flex-direction: column;
    }

    .coach_photo{
        margin: 50rpx auto;
    }

    .coach_photo img {
        max-height: 700rpx;
    }

    .coach_name {
        font-size: 40rpx;
        text-align: center;
        font-weight: bold;
    }

    .detail_content {
        display: flex;
        flex-direction: column;
        margin-left: 20%;
        margin-top: 30rpx;
        line-height: 50rpx;
        font-size: 28rpx;
    }

    button {
        width: 70%;
        height: 80rpx;
        line-height: 80rpx;
        font-size: 28rpx;
        margin-top: 20rpx;
    }
</style>