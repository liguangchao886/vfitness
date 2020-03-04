<template>
    <div class="timetable">
        <img :src="link" alt="" mode="aspectFit" @tap="previewMoreImage">
    </div>
</template>

<script>
    export default{
        data(){
            return{
                link:'cloud://liguangchao-u14ya.6c69-liguangchao-u14ya-1300277699/timetable.jpg'
            }
        },
        methods:{
            previewMoreImage() {   //  点击查看大图
                let src = this.link
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
    .timetable{
        margin-top: 100rpx;
        width: 100%;

    }

    .timetable img{
        width: 100%;
    }
</style>