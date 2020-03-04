<template>
    <div class="Container">
        <form>
            <div class="form_imput">
                <span class="form_left">会员卡号:</span>
                <input class="form_right" type="text" v-model="ID" />
            </div>
            <div class="form_imput">
                <span class="form_left">姓名:</span>
                <input class="form_right" type="text" v-model="name" />
            </div>
            <div class="form_imput">
                <label class="form_left">性别：</label>
                <radio-group class="form_right" @change="sexChange">
                    <label class="radio" v-for="(item, index) in items" :key="item.sex">
                        <radio color='#999' :value="item.sex" :checked="item.checked" />
                        {{item.value}}
                    </label>
                </radio-group>
            </div>
            <div class="form_imput">
                <span class="form_left">出生日期:</span>
                <picker mode="multiSelector" @change="bindMultiPickerChange" :value="multiIndex" :range="newMultiArray">
                    <span class="form_right">{{time}}</span>
                </picker>
            </div>
            <button @tap="onAdd">保存</button>
        </form>





    </div>
</template>

<script>
    export default {
        data() {
            return {
                user_data: '',
                openid: '',
                ID: '',
                name: '',
                items: [
                    { sex: '男', value: '男', checked: '' },
                    { sex: '女', value: '女', checked: '' }
                ],
                time: '',
                multiArray: [],
                multiIndex: [0, 0, 0, 0, 0]
            }
        },
        computed: {
            newMultiArray: () => {
                let array = [];
                const date = new Date();
                const years = [];
                const months = [];
                const days = [];

                for (let i = 2019; i <= date.getFullYear() + 10; i++) {
                    years.push("" + i);
                }
                array.push(years);

                for (let i = 1; i <= 12; i++) {
                    if (i < 10) {
                        i = "0" + i;
                    }
                    months.push("" + i);
                }
                array.push(months);

                for (let i = 1; i <= 31; i++) {
                    if (i < 10) {
                        i = "0" + i;
                    }
                    days.push("" + i);
                }
                array.push(days);
                return array;
            }
        },
        mounted() {
            //  本地缓存读取openid
            this.openid = wx.getStorageSync('openid')
            console.log(this.openid)
            sex_type: '男'
            this.getUserInfo()
        },
        methods: {
            getUserInfo() {
                var that = this;
                const db = wx.cloud.database();
                db.collection('user_data').get({
                    success: function (res) {
                        console.log(res)
                        that.user_data = res.data[0]
                        that.ID = res.data[0].ID
                        that.name = res.data[0].name
                        that.sex = res.data[0].sex
                        if (res.data[0].sex === '男') {
                            that.sex_type = '男'
                            that.items[0].checked = 'true'
                        } else
                            if (res.data[0].sex === '女') {
                                that.sex_type = '女'
                                that.items[1].checked = 'true'
                            }
                        that.time = res.data[0].date
                    }
                })
            },
            sexChange(e) {
                this.sex_type = e.target.value
                if (this.sex_type === '女') {
                    this.items[0].checked = ''
                    this.items[1].checked = 'true'
                } else {
                    this.items[0].checked = 'true'
                    this.items[1].checked = ''
                }
                console.log('性别：', this.sex_type)
            },
            bindMultiPickerChange(e) {
                this.multiIndex = e.target.value;
                const index = this.multiIndex;
                const year = this.newMultiArray[0][index[0]];
                const month = this.newMultiArray[1][index[1]];
                const day = this.newMultiArray[2][index[2]];
                this.time = year + "-" + month + "-" + day
                console.log(this.time)
            },
            disp() {
                console.log('会员卡号：' + this.ID + ' 姓名：' + this.name + ' 性别：' + this.sex_type + ' 出生日期：' + this.time)
            },
            onAdd() {
                const db = wx.cloud.database()
                db.collection('user_data').doc(this.openid).set({
                    //  openid为数据库_id
                    data: {
                        ID: this.ID,
                        name: this.name,
                        sex: this.sex_type,
                        date: this.time,
                    },
                    success: res => {
                        wx.showToast({
                            title: '保存成功',
                            icon: 'success',
                            duration: 2000
                        })
                        setTimeout(function () {
                            //  保存后返回上一页
                            wx.navigateBack({
                                delta: 2,
                            })
                        }, 1000)

                    }
                })

            },
        }
    }

</script>

<style>
    .form_imput {
        height: 150rpx;
        width: 100%;
        border-bottom: 1rpx solid #eee;
        line-height: 150rpx;
    }

    .form_left {
        padding-left: 20rpx;
        width: 40%;
        float: left;
        line-height: 150rpx;

    }

    .form_right {
        float: left;
        width: 50%;
        line-height: 150rpx;
        height: 150rpx;
        text-align: right;
    }
</style>