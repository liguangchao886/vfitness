<template>
    <div class="con">
        <div class="text">
            <p>地址：广东省东莞市麻涌镇沿江西一路7号 </p>
            <p>联系电话：020-12345678</p>
            <p>营业时间：10:00-22:00</p>
            <img @tap='previewMoreImage' :src="link" alt="" mode="aspectFit">
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                link: 'cloud://liguangchao-u14ya.6c69-liguangchao-u14ya/1.png'
            }
        },
        methods: {
            previewMoreImage(e) {   //  点击查看大图
                let src = this.link;
                let urlarr = [];
                urlarr.push(src)
                wx.previewImage({
                    current: src,
                    urls: urlarr
                })
            }
        }
    }

</script>

<style>
    .con {
        width: 80%;
        margin: 100rpx auto;
        border: 1rpx dashed #eee;
    }

    .text {
        width: 98%;
        margin: 0 auto;
        text-align: left;
    }

    .text p {
        font-size: 36rpx;
        line-height: 70rpx;
    }

    .text img{
        width: 98%;
        margin: 0 auto;
    }
</style>