<template>
    <div class="content">
        <el-menu :default-active="typepage" class="el-menu-demo" mode="horizontal">
            <el-menu-item index="1">商品审核</el-menu-item>
            <el-menu-item index="2">用户管理</el-menu-item>
            <el-dropdown @command="outlogin" style="position: absolute;right: 15px;line-height: 61px;cursor: pointer;">
                <span class="el-dropdown-link">
                    管理员<i class="el-icon-arrow-down el-icon--right"></i>
                </span>
                <el-dropdown-menu slot="dropdown">
                    <el-dropdown-item>退出登录</el-dropdown-item>
                </el-dropdown-menu>
            </el-dropdown>
        </el-menu>

        <!-- 商品审核 -->
        <div class="shopbox" v-if="typepage == 1">
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
                <el-table-column prop="status" label="审核状态">
                </el-table-column>
                <el-table-column label="操作" width="100">
                    <template slot-scope="scope">
                        <el-button @click="handleClick(scope.row)" type="text" size="small">审核产品</el-button>
                    </template>
                </el-table-column>
            </el-table>
        </div>

        <!-- 审核商品弹框 -->
        <el-dialog title="审核商品" :visible.sync="shopVisible" width="45%">
            <el-radio-group v-model="radio">
                <el-radio :label="0">待审核</el-radio>
                <el-radio :label="1">审核通过</el-radio>
                <el-radio :label="2">审核通过</el-radio>
            </el-radio-group>
            <span slot="footer" class="dialog-footer">
                <el-button @click="shopVisible = false">取 消</el-button>
                <el-button type="primary" @click="saveshop">提交审核</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
export default {
    name: 'AdminView',
    data() {
        return {
            // 控制菜单显示内容
            typepage: '1',
            // 产品数据
            shopData: [],
            // 控制添加商品弹框
            shopVisible: false,
            // 产品标题
            title: '',
            // 产品内容
            content: '',
            // 产品价格
            price: '',
            // 产品图片
            photo: '',
            // 审核状态
            radio: 0,
            // 审核产品的id
            id: ''
        }
    },
    // 页面一加载执行下面方法
    mounted() {
        this.getShop()
    },
    methods: {
        // 点击审核
        handleClick(row) {
            this.shopVisible = true
            this.id = row.id
            if(row.status == '待审核'){
                this.radio = 0
            }else if(row.status == '审核通过'){
                this.radio = 1
            }
            else if (row.status == '审核未通过'){
                this.radio = 2
            }
            console.log(row);
        },
        // 获取产品数据
        async getShop() {
            const res = await this.$http.post("shop/admincommodity")
            console.log(res)
            if (res.data.code === 200) {
                this.shopData = res.data.data
                // 产品状态为0 1 2  进行数据转换为文字
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
        },
        // 提交审核
        async saveshop() {
            const res = await this.$http.post("shop/updatestatus",{
                status: this.radio,
                id: this.id
            })
            console.log(res)
            if(res.data.code === 200) {
                this.shopVisible = false
                this.$message.success("审核成功")
                this.getShop()
            }else {
                this.$message.error("审核失败，请重试")
            }
        },
        // 退出登录
        outlogin() {
            console.log('123')
            localStorage.removeItem('adminid')
            this.$router.push('login')
            this.$message("账号已退出登录!");
        }
    }
}
</script>

<style scoped>
.shopbox {
    margin-top: 20px;
    padding: 0 20px;
}

.shopimg {
    width: 120px;
    height: 80px;
}
</style>