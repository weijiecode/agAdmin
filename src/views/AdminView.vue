<template>
    <div class="content">
        <el-menu @select="handleSelect" :default-active="typepage" class="el-menu-demo" mode="horizontal">
            <el-menu-item index="1">商品审核</el-menu-item>
            <el-menu-item index="2">用户管理</el-menu-item>
            <el-menu-item index="3">资讯管理</el-menu-item>
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
        <div class="shopbox" v-if="typepage == '1'">
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

        <!-- 用户管理 -->
        <div class="shopbox" v-if="typepage == '2'">
            <!-- 用户管理列表 -->
            <el-table :data="usersData" border style="width: 100%;margin-top: 5px;">
                <el-table-column prop="id" label="id" width="150">
                </el-table-column>
                <el-table-column prop="username" label="用户名">
                </el-table-column>
                <el-table-column prop="nickname" label="昵称">
                </el-table-column>
                <el-table-column prop="sex" label="性别">
                </el-table-column>
                <el-table-column prop="phone" label="手机号">
                </el-table-column>
                <el-table-column prop="email" label="邮箱">
                </el-table-column>
                <el-table-column prop="introduction" label="个人简介">
                </el-table-column>
                <el-table-column label="操作" width="100">
                    <template slot-scope="scope">
                        <el-button @click="editHandleClick(scope.row)" type="text" size="small">编辑</el-button>
                        <el-button @click="delHandleClick(scope.row)" type="text" size="small">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>
        </div>
        <!-- 发布资讯 -->
        <div class="shopbox" v-if="typepage == '3'">
            <el-button @click="shopVisibleInfo = true" type="primary">发布资讯</el-button>
            <!-- 资讯管理列表 -->
            <el-table :data="informationData" border style="width: 100%;margin-top: 5px;">
                <el-table-column prop="id" label="id" width="150">
                </el-table-column>
                <el-table-column prop="title" label="标题">
                </el-table-column>
                <el-table-column prop="content" label="内容">
                </el-table-column>
                <el-table-column prop="picture" label="图片" width="150">
                    <template slot-scope="scope">
                        <img :src="scope.row.picture" class="shopimg">
                    </template>
                </el-table-column>
                <el-table-column label="操作" width="100">
                    <template slot-scope="scope">
                        <el-button @click="editHandleClickInfo(scope.row)" type="text" size="small">编辑</el-button>
                        <el-button @click="delHandleClickInfo(scope.row)" type="text" size="small">删除</el-button>
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

        <!-- 修改用户信息弹框 -->
        <el-dialog title="修改用户信息" :visible.sync="ushopVisible" width="60%">
            <el-form status-icon label-width="100px" class="demo-ruleForm">
                <el-form-item label="用户名">
                    <el-input placeholder="请输入用户名" v-model="username"></el-input>
                </el-form-item>
                <el-form-item label="昵称">
                    <el-input placeholder="请输入昵称" v-model="nickname">
                    </el-input>
                </el-form-item>
                <el-form-item label="性别">
                    <el-radio v-model="sex" label="男">男</el-radio>
                    <el-radio v-model="sex" label="女">女</el-radio>
                </el-form-item>
                <el-form-item label="电话">
                    <el-input placeholder="请输入号码" v-model="phone">
                    </el-input>
                </el-form-item>
                <el-form-item label="邮箱">
                    <el-input placeholder="请输入邮箱" v-model="email">
                    </el-input>
                </el-form-item>
                <el-form-item label="简介">
                    <el-input type="textarea" :rows="2" placeholder="请输入简介" v-model="introduction">
                    </el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="shopVisible = false">取 消</el-button>
                <el-button type="primary" @click="edituser">修改信息</el-button>
            </span>
        </el-dialog>

        <!-- 发布资讯弹框 -->
        <el-dialog title="发布资讯" :visible.sync="shopVisibleInfo" width="60%">
            <el-form status-icon label-width="100px" class="demo-ruleForm">
                <el-form-item label="标题">
                    <el-input placeholder="请输入资讯标题" v-model="infoTitle"></el-input>
                </el-form-item>
                <el-form-item label="资讯图片">
                    <el-upload class="avatar-uploader" action="http://localhost:5001/information/infophotouploadurl"
                        :show-file-list="false" :on-success="handleAvatarSuccess" :before-upload="beforeAvatarUpload">
                        <img v-if="infoPicture" :src="infoPicture" class="avatar">
                        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                    </el-upload>
                </el-form-item>
                <el-form-item label="内容">
                    <el-input type="textarea" :rows="5" placeholder="请输入资讯内容" v-model="infoContent">
                    </el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="shopVisibleInfo = false">取 消</el-button>
                <el-button type="primary" @click="saveinfo">发布资讯</el-button>
            </span>
        </el-dialog>

        <!-- 修改咨询弹框 -->
        <el-dialog title="修改商品" :visible.sync="infoEditVisibleInfo" width="60%">
            <el-form status-icon label-width="100px" class="demo-ruleForm">
                <el-form-item label="标题">
                    <el-input placeholder="请输入咨询标题" v-model="uinfoTitle"></el-input>
                </el-form-item>
                <el-form-item label="产品图片">
                    <el-upload class="avatar-uploader" action="http://localhost:5001/information/infophotouploadurl"
                        :show-file-list="false" :on-success="uhandleAvatarSuccess" :before-upload="beforeAvatarUpload">
                        <img v-if="uinfoPicture" :src="uinfoPicture" class="avatar">
                        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                    </el-upload>
                </el-form-item>
                <el-form-item label="内容">
                    <el-input type="textarea" :rows="5" placeholder="请输入咨询内容" v-model="uinfoContent">
                    </el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="infoEditVisibleInfo = false">取 消</el-button>
                <el-button type="primary" @click="usaveinformation">修改咨询</el-button>
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
            id: '',
            // 用户数据
            usersData: [],
            // 用户id
            userid: '',
            // 用户名称
            username: '',
            // 用户昵称
            nickname: '',
            // 用户性别
            sex: 0,
            // 用户手机号
            phone: '',
            // 用户邮箱
            email: '',
            // 用户简介
            introduction: '',
            // 控制显示修改用户弹框
            ushopVisible: false,
            // 资讯列表数据
            informationData: [],
            // 控制发布资讯弹框显示
            shopVisibleInfo: false,
            // 咨询标题
            infoTitle: '',
            // 咨询内容
            infoContent: '',
            // 咨询图片
            infoPicture: '',
            // 咨询时间
            infoDatetime: '',
            // 控制修改资讯弹框显示
            infoEditVisibleInfo: false,
            // 编辑咨询标题
            uinfoTitle: '',
            // 编辑咨询内容
            uinfoContent: '',
            // 编辑咨询图片
            uinfoPicture: '',
            // 咨询id
            informationid: ''
        }
    },
    // 页面一加载执行下面方法
    mounted() {
        this.getShop()
        this.getUsers()
        this.getInformation()
    },
    methods: {
        // 点击审核
        handleClick(row) {
            this.shopVisible = true
            this.id = row.id
            if (row.status == '待审核') {
                this.radio = 0
            } else if (row.status == '审核通过') {
                this.radio = 1
            }
            else if (row.status == '审核未通过') {
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
        // 获取用户数据
        async getUsers() {
            const res = await this.$http.post("account/allusers")
            console.log(res)
            if (res.data.code === 200) {
                this.usersData = res.data.data
                this.usersData.forEach((item, index) => {
                    if (item.sex === '0') {
                        this.usersData[index].sex = '女'
                    } else {
                        this.usersData[index].sex = '男'
                    }
                })
            }
        },
        // 获取资讯数据
        async getInformation() {
            const res = await this.$http.get('information/getinformation')
            console.log(res)
            if (res.data.code === 200) {
                this.informationData = res.data.data
            }
        },
        // 发布资讯
        async saveinfo() {
            const res = await this.$http.post('information/addinformation', {
                title: this.infoTitle,
                content: this.infoContent,
                picture: this.infoPicture
            })
            console.log(res)
            if (res.data.code === 200) {
                this.getInformation()
                this.shopVisibleInfo = false
                this.$message.success('发布成功')
            }else {
                this.$message.error('发布失败，请重试')
            }
        },
        // 点击咨询编辑
        editHandleClickInfo(row) {
            this.infoEditVisibleInfo = true
            console.log(row)
            this.uinfoTitle = row.title
            this.uinfoContent = row.content
            this.uinfoPicture = row.picture
            this.informationid = row.id
        },
        // 点击咨询删除
        async delHandleClickInfo(row) {
            console.log(row)
            const res = await this.$http.post('information/delinformation', {
                id: row.id
            })
            console.log(res)
            if (res.data.code === 200) {
                this.getInformation()
                this.$message.success('删除成功')
            }
        },
        // 编辑咨询
        async usaveinformation() {
            const res = await this.$http.post('information/editinformation', {
                title: this.uinfoTitle,
                content: this.uinfoContent,
                picture: this.uinfoPicture,
                id: this.informationid
            })
            console.log(res)
            if (res.data.code === 200) {
                this.infoEditVisibleInfo = false
                this.getInformation()
                this.$message.success('修改成功')
            }else {
                this.$message.error('修改失败，请重试')
            }
        },
        // 添加上传图片方法
        handleAvatarSuccess(res, file) {
            // 后端返回url赋值给picture
            this.infoPicture = res;
        },
        // 编辑上传图片方法
        uhandleAvatarSuccess(res, file) {
            // 后端返回url赋值给picture
            this.uinfoPicture = res;
        },
        // 图片上传之前的监测操作
        beforeAvatarUpload(file) {
            const isLt2M = file.size / 1024 / 1024 < 2;
            if (!isLt2M) {
                this.$message.error('上传头像图片大小不能超过 2MB!');
            }
            return isLt2M;
        },
        // 切换菜单
        handleSelect(item) {
            console.log(item)
            this.typepage = item
        },
        // 编辑用户
        async editHandleClick(row) {
            console.log(row)
            this.ushopVisible = true
            this.usersData.forEach(item => {
                if (item.id == row.id) {
                    this.userid = row.id
                    this.username = item.username
                    this.nickname = item.nickname
                    this.phone = item.phone
                    this.sex = item.sex
                    this.email = item.email
                    this.introduction = item.introduction
                }
            })

        },
        // 删除用户
        async delHandleClick(row) {
            console.log(row.id)
            const res = await this.$http.post('account/deluser', {
                id: row.id
            })
            if (res.data.code === 200) {
                this.$message.success('删除成功')
                this.getUsers()
            } else {
                this.$message.error('删除用户失败，请重试')
            }
        },
        // 提交修改用户数据
        async edituser() {
            if (this.sex == '男') {
                this.sex = '1'
            } else {
                this.sex = '0'
            }
            const res = await this.$http.post('account/edituser', {
                username: this.username,
                nickname: this.nickname,
                sex: this.sex,
                phone: this.phone,
                email: this.email,
                introduction: this.introduction,
                id: this.userid
            })
            console.log(res)
            if (res.data.code === 200) {
                this.ushopVisible = false
                this.$message.success('修改用户信息成功')
                this.getUsers()
            }

        },
        // 提交审核
        async saveshop() {
            const res = await this.$http.post("shop/updatestatus", {
                status: this.radio,
                id: this.id
            })
            console.log(res)
            if (res.data.code === 200) {
                this.shopVisible = false
                this.$message.success("审核成功")
                this.getShop()
            } else {
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