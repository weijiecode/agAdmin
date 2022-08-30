<template>
    <div class="content">
        <el-menu :default-active="typepage" class="el-menu-demo" mode="horizontal">
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

        <div class="shopbox" v-if="typepage == 1">
            <!-- 添加产品 -->
            <el-button type="primary">添加产品</el-button>
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
                <el-table-column prop="photo" label="图片">
                </el-table-column>
                <el-table-column label="所属店铺">
                    <template slot-scope="scope">
                        {{ shopname }}
                    </template>
                </el-table-column>
                <el-table-column label="操作" width="100">
                    <template slot-scope="scope">
                        <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
                        <el-button type="text" size="small">编辑</el-button>
                    </template>
                </el-table-column>
            </el-table>
        </div>
        <!-- 订单管理列表 -->
        <div class="shopbox" v-if="typepage == 2">
        </div>
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
            shopname: ''
        }
    },
    // 页面一加载执行下面方法
    mounted() {
        this.shopname = localStorage.getItem('shopname')
        this.getShop()
    },
    methods: {
        handleClick(row) {
            console.log(row);
        },
        // 获取产品数据
        async getShop() {
            const res = await this.$http.post("shop/commodity", {
                merchantid: localStorage.getItem('merchantid') * 1
            })
            console.log(res)
            if (res.data.code === 200) {
                this.shopData = res.data.data
            }
            console.log(this.shopData)
        },
        // 退出登录
        outlogin() {
            console.log('123')
            localStorage.removeItem('merchantid')
            localStorage.removeItem('shopname')
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
</style>