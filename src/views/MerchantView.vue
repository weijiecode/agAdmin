<template>
    <div class="content">
        <el-menu :default-active="typepage" @select="handleSelect" class="el-menu-demo" mode="horizontal">
            <el-menu-item index="1">商品管理</el-menu-item>
            <el-menu-item index="2">订单管理</el-menu-item>
            <el-dropdown @command="outlogin" style="position: absolute;right: 15px;line-height: 61px;cursor: pointer;">
                <span class="el-dropdown-link">
                    alice<i class="el-icon-arrow-down el-icon--right"></i>
                </span>
                <el-dropdown-menu slot="dropdown">
                    <el-dropdown-item>退出登录</el-dropdown-item>
                </el-dropdown-menu>
            </el-dropdown>
        </el-menu>
        <div class="shopbox" v-if="typepage == '1'">
            <!-- 添加产品 -->
            <el-button @click="shopVisible = true" type="primary">添加产品</el-button>
            <!-- 商品管理列表 -->
            <el-table :data="shopData" border style="width: 100%;margin-top: 5px;">
                <el-table-column prop="id" label="id" width="150">
                </el-table-column>
                <el-table-column prop="title" label="标题">
                </el-table-column>
                <el-table-column prop="content" label="内容">
                </el-table-column>
                <el-table-column prop="price" label="价格">
                </el-table-column>
                <el-table-column prop="photo" label="图片" width="150">
                    <template slot-scope="scope">
                        <img :src="scope.row.photo" class="shopimg">
                    </template>
                </el-table-column>
                <el-table-column label="所属店铺">
                    <template slot-scope="scope">
                        {{ shopname }}
                    </template>
                </el-table-column>
                <el-table-column prop="status" label="审核状态">
                </el-table-column>
                <el-table-column label="操作" width="100">
                    <template slot-scope="scope">
                        <el-button @click="handleClick(scope.row)" type="text" size="small">编辑</el-button>
                        <el-button type="text" size="small">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>
        </div>
        <!-- 订单管理列表 -->
        <div class="shopbox" v-if="typepage == '2'">
            <!-- 订单管理列表 -->
            <el-table :data="pay" border style="width: 100%;margin-top: 5px;">
                <el-table-column prop="id" label="id">
                </el-table-column>
                <el-table-column prop="username" label="买家用户名">
                </el-table-column>
                <el-table-column prop="title" label="标题">
                </el-table-column>
                <el-table-column prop="price" label="价格">
                </el-table-column>
                <el-table-column prop="price" label="价格">
                </el-table-column>
                <el-table-column prop="photo" label="图片" width="150">
                    <template slot-scope="scope">
                        <img :src="scope.row.photo" class="shopimg">
                    </template>
                </el-table-column>
                <el-table-column label="所属店铺">
                    <template slot-scope="scope">
                        {{ shopname }}
                    </template>
                </el-table-column>
                <el-table-column prop="address" label="地址">
                </el-table-column>
                <el-table-column prop="remark" label="备注">
                </el-table-column>
                <el-table-column label="操作" width="150">
                    <template slot-scope="scope">
                        <el-button @click="payHandleClick(scope.row)" type="text" size="small">编辑</el-button>
                        <el-button @click="delpay(scope.row)" type="text" size="small">取消订单</el-button>
                    </template>
                </el-table-column>
            </el-table>
        </div>
        <!-- 添加商品弹框 -->
        <el-dialog title="添加商品" :visible.sync="shopVisible" width="60%">
            <el-form status-icon label-width="100px" class="demo-ruleForm">
                <el-form-item label="标题">
                    <el-input placeholder="请输入产品标题" v-model="title"></el-input>
                </el-form-item>
                <el-form-item label="内容">
                    <el-input type="textarea" :rows="2" placeholder="请输入产品内容" v-model="content">
                    </el-input>
                </el-form-item>
                <el-form-item label="价格">
                    <el-input style="width: 30%" placeholder="产品价格" v-model="price">
                        <template slot="append">¥</template>
                    </el-input>
                </el-form-item>
                <el-form-item label="产品图片">
                    <el-upload class="avatar-uploader" action="http://localhost:5001/shop/shopphotouploadurl"
                        :show-file-list="false" :on-success="handleAvatarSuccess" :before-upload="beforeAvatarUpload">
                        <img v-if="photo" :src="photo" class="avatar">
                        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                    </el-upload>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="shopVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveshop">上传审核</el-button>
            </span>
        </el-dialog>

        <!-- 修改商品弹框 -->
        <el-dialog title="修改商品" :visible.sync="ushopVisible" width="60%">
            <el-form status-icon label-width="100px" class="demo-ruleForm">
                <el-form-item label="标题">
                    <el-input placeholder="请输入产品标题" v-model="utitle"></el-input>
                </el-form-item>
                <el-form-item label="内容">
                    <el-input type="textarea" :rows="2" placeholder="请输入产品内容" v-model="ucontent">
                    </el-input>
                </el-form-item>
                <el-form-item label="价格">
                    <el-input style="width: 30%" placeholder="产品价格" v-model="uprice">
                        <template slot="append">¥</template>
                    </el-input>
                </el-form-item>
                <el-form-item label="产品图片">
                    <el-upload class="avatar-uploader" action="http://localhost:5001/shop/shopphotouploadurl"
                        :show-file-list="false" :on-success="uhandleAvatarSuccess" :before-upload="beforeAvatarUpload">
                        <img v-if="uphoto" :src="uphoto" class="avatar">
                        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                    </el-upload>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="ushopVisible = false">取 消</el-button>
                <el-button type="primary" @click="usaveshop">修改产品</el-button>
            </span>
        </el-dialog>

        <!-- 修改订单记录 -->
        <el-dialog title="修改订单信息" :visible.sync="payVisible" width="60%">
            <el-form status-icon label-width="100px" class="demo-ruleForm">
                <el-form-item label="地址">
                    <el-input placeholder="请输入地址" v-model="address"></el-input>
                </el-form-item>
                <el-form-item label="备注">
                    <el-input type="textarea" :rows="2" placeholder="请输入备注" v-model="remark">
                    </el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="payVisible = false">取 消</el-button>
                <el-button type="primary" @click="editpay">修改信息</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
export default {
    name: 'MerchantView',
    data() {
        return {
            // 控制菜单显示内容
            typepage: '1',
            // 产品数据
            shopData: [],
            // 店铺名称
            shopname: '',
            // 控制添加商品弹框显示
            shopVisible: false,
            // 控制修改弹框显示
            payVisible: false,
            // 控制修改商品弹框
            ushopVisible: false,
            // 产品标题
            title: '',
            // 产品内容
            content: '',
            // 产品价格
            price: '',
            // 产品图片
            photo: '',
            // 修改产品标题
            utitle: '',
            // 修改产品内容
            ucontent: '',
            // 修改产品价格
            uprice: '',
            // 修改产品图片
            uphoto: '',
            // 指定编辑产品的id
            shopid: '',
            // 购物车数据
            shopCartData: [],
            // 原始支付数据
            oldpay: [],
            // 筛选过滤后渲染的支付数据
            pay: [],
            // 所有用户名称和id
            iduser: [],
            // 地址
            address: '',
            // 备注
            remark: ''
        }
    },
    // 页面一加载执行下面方法
    mounted() {
        this.shopname = localStorage.getItem('shopname')
        this.getShop()
    },
    methods: {
        // 点击编辑
        async handleClick(row) {
            this.ushopVisible = true
            this.utitle = row.title
            this.ucontent = row.content
            this.uphoto = row.photo
            this.uprice = row.price
            this.shopid = row.id
        },
        // 提交修改
        async usaveshop() {
            const res = await this.$http.post("shop/updateshop", {
                title: this.utitle,
                content: this.ucontent,
                photo: this.uphoto,
                price: this.uprice,
                id: this.shopid
            })
            console.log(res)
            if (res.data.code === 200) {
                this.$message.success("修改产品成功")
                // 添加成功后刷新列表数据
                this.getShop()
                this.ushopVisible = false
            } else {
                this.$message.error("修改失败，请重试")
            }
        },
        // 获取产品数据
        async getShop() {
            const res = await this.$http.post("shop/commodity", {
                merchantid: localStorage.getItem('merchantid') * 1
            })
            console.log(res)
            if (res.data.code === 200) {
                this.shopData = res.data.data
                this.shopData.forEach((item, index) => {
                    if (item.status === '0') {
                        this.shopData[index].status = '待审核'
                    } else if (item.status === '1') {
                        this.shopData[index].status = '审核通过'
                    } else if (item.status === '2') {
                        this.shopData[index].status = '审核未通过'
                    }
                })
            }
            console.log(this.shopData)


            const res2 = await this.$http.post('shop/selectpay')
            console.log(res2)
            if(res2.data.code === 200){
                this.oldpay = res2.data.data
                // 获取用户名
                this.$http.post('account/iduser').then(res1 => {
                    console.log(res1)
                    if(res2.data.code === 200){
                    this.iduser = res1.data.data
                    console.log(this.oldpay)
                    this.pay = []
                    this.oldpay.forEach((item,index) => {
                        this.shopData.forEach(subitem => {
                            if(item.commodityId == subitem.id){
                                this.pay[index] = {
                                    id: item.id,
                                    title: subitem.title,
                                    price: subitem.price,
                                    photo: subitem.photo,
                                    userId: item.userId,
                                    remark: item.remark,
                                    address: item.address
                                }
                            }
                        })
                    })
                    console.log(this.pay)
                    this.pay.forEach((subitem1,subindex1) => {
                        this.iduser.forEach(subitem2 => {
                            if(subitem1.userId == subitem2.id){
                                this.pay[subindex1].username = subitem2.username
                            }
                        })
                    })
                    console.log(this.pay)
                    }
                })

            }
        },
        // 保存修改订单信息
        async editpay() {
            const res = await this.$http.post('shop/updatepay',{
                remark: this.remark,
                address: this.address,
                id: this.id
            })
            console.log(res)
            if(res.data.code === 200) {
                this.payVisible = false
                this.$message.success('修改成功')
                this.getShop()
            }else {
                this.$message.error('修改失败，请重试')
            }
        },
        // 取消订单
        delpay(row) {
            this.$http.post('shop/delpay',{
                id: row.id
            }).then(res => {
                console.log(res)
                if(res.data.code === 200) {
                    this.$message.success('该订单已取消')
                    this.getShop()
                }else {
                    this.$message.error('订单取消失败，请重试')
                }
            })
            
        },
        handleSelect(item) {
            this.typepage = item
        },
        // 点击编辑按钮
        payHandleClick(row) {
            this.payVisible = true
            this.id = row.id
            this.address = row.address
            this.remark = row.remark

        },
        // 退出登录
        outlogin() {
            localStorage.removeItem('merchantid')
            localStorage.removeItem('shopname')
            this.$router.push('login')
            this.$message("账号已退出登录!");
        },
        // 添加上传图片方法
        handleAvatarSuccess(res, file) {
            // 后端返回url赋值给photo
            this.photo = res;
        },
        // 编辑上传图片方法
        uhandleAvatarSuccess(res, file) {
            // 后端返回url赋值给photo
            this.uphoto = res;
        },
        // 图片上传之前的监测操作
        beforeAvatarUpload(file) {
            const isJPG = file.type === 'image/jpeg';
            const isLt2M = file.size / 1024 / 1024 < 2;

            if (!isJPG) {
                this.$message.error('上传头像图片只能是 JPG 格式!');
            }
            if (!isLt2M) {
                this.$message.error('上传头像图片大小不能超过 2MB!');
            }
            return isJPG && isLt2M;
        },
        // 提交审核商品数据
        async saveshop() {
            const res = await this.$http.post("shop/addshop", {
                title: this.title,
                content: this.content,
                photo: this.photo,
                price: this.price,
                merchantid: localStorage.getItem('merchantid') * 1,
                status: 0
            })
            console.log(res)
            if (res.data.code === 200) {
                this.$message.success("添加产品成功")
                // 添加成功后刷新列表数据
                this.getShop()
                this.shopVisible = false
                this.title = ''
                this.content = ''
                this.photo = ''
                this.price = ''
            } else {
                this.$message.error("添加失败，请重试")
            }
        }
    }
}
</script>

<style scoped>
.shopbox {
    margin-top: 20px;
    padding: 0 20px;
}

/* 图片css */
::v-deep .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
}

::v-deep .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
}

::v-deep .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
}

::v-deep .avatar {
    width: 178px;
    height: 178px;
    display: block;
}

.shopimg {
    width: 120px;
    height: 80px;
}
</style>