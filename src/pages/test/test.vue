<template>
    <div>
        <p>当前时间：{{time}}</p>
        <img src="https://wx.qlogo.cn/mmopen/vi_32/DYAIOgq83eoY623OoB3iblavHibLJ8iaInvtuBjyegEIF4G46688wJtzmlmCR0OSByX4uRzYDP3l4TeTD3WHDicsxw/132"
            alt="">
        <button @tap="getdata">123</button>
    </div>
</template>

<script>
    var util = require('../../utils/util.js');
    export default {
        data() {
            return {
                time: ""
            }
        },
        methods: {
            getSociety_data() {
                const db = wx.cloud.database({});
                const table = db.collection('society_data');
                table.where({
                    _openid: 'o1-hd5TzQXNzWO3MbYwD4IFNiN40'
                }).get({
                    success: function (res) {
                        console.log(res.data)
                    }
                })
            },
            getdata() {
                var that = this;
                const db = wx.cloud.database();
                db.collection('society_data').get({
                    success: function (res) {
                        /* this.coachdata=res.data*/
                        console.log(res)
                        that.coachdata = res.data
                    }
                })
            },
            onLoad: function () {
                // 调用函数时，传入new Date()参数，返回值是日期和时间  
                var time = util.formatTime(new Date());
                // 再通过setData更改Page()里面的data，动态更新页面的数据  
                this.time = time
            },
            created() {

            }
        }
    }
</script>