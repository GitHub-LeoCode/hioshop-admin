<template>
    <div class="content-page">
        <div class="content-nav">
            <el-breadcrumb class="breadcrumb" separator="/">
                <el-breadcrumb-item>厂商列表</el-breadcrumb-item>
            </el-breadcrumb>
            <div class="operation-nav">
                <router-link to="/dashboard/manufactor/add">
                    <el-button type="primary" icon="plus" size="small">添加厂商</el-button>
                </router-link>
            </div>
        </div>
        <div class="content-main">
            <div class="filter-box">
                <el-form :inline="true" :model="filterForm" class="demo-form-inline">
                    <el-form-item label="厂商名称">
                        <el-input v-model="filterForm.name" placeholder="厂商名称"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="onSubmitFilter">查询</el-button>
                    </el-form-item>
                </el-form>
            </div>
            <div class="form-table-box" v-if="fake == 0">
                <el-table :data="tableData" style="width: 100%" border stripe>
                    <el-table-column prop="id" label="ID" width="60">
                    </el-table-column>
                    <el-table-column prop="name" label="厂商名称">
                        <template scope="scope">
                            <el-input v-model="scope.row.name" placeholder="厂商名称"
                                      @blur="submitNick(scope.$index, scope.row)"></el-input>
                        </template>
                    </el-table-column>
                    <el-table-column prop="address" label="厂商地址">
                    </el-table-column>
                    <el-table-column prop="phone" label="厂商电话">
                    </el-table-column>
                    <el-table-column prop="is_delete" label="状态">
                        <template scope="scope">
                            <label>{{scope.row.is_delete === 1? '已删除':'正常'}}</label>
                        </template>
                    </el-table-column>
                    <el-table-column label="操作">
                        <template scope="scope">
                            <el-button size="small" @click="handleRowEdit(scope.$index, scope.row)">编辑</el-button>
                            <el-button plain size="small" type="danger"
                                       @click="handleRowDelete(scope.$index, scope.row)">删除
                            </el-button>
                        </template>
                    </el-table-column>
                </el-table>
            </div>
            <div class="page-box" v-if="fake == 0">
                <el-pagination background @current-change="handlePageChange" :current-page.sync="page" :page-size="10"
                               layout="total, prev, pager, next, jumper" :total="total">
                </el-pagination>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        data() {
            return {
                page: 1,
                total: 0,
                filterForm: {
                    name: ''
                },
                tableData: [],
                fakeData: [],
                fake: 0,
                loginInfo: null,
                username: ''
            }
        },
        methods: {
            fakeShow() {
                this.fake = 1;
            },
            realShow() {
                this.fake = 0;
            },
            handlePageChange(val) {
                this.page = val;
                //保存到localStorage
                localStorage.setItem('manufactorPage', this.page)
                this.getList()
            },
            handleRowDelete(index, row) {
                this.$confirm('确定要删除?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    this.axios.post('manufactor/destory', {id: row.id}).then((response) => {
                        console.log(response.data)
                        if (response.data.errno === 0) {
                            this.$message({
                                type: 'success',
                                message: '删除成功!'
                            });
                            this.getList();
                        }
                    })
                });
            },
            onSubmitFilter() {
                this.page = 1
                this.getList()
            },
            getList() {
                this.axios.get('manufactor', {
                    params: {
                        page: this.page,
                        name: this.filterForm.name
                    }
                }).then((response) => {
                    console.log(response.data);
                    console.log(response);
                    this.tableData = response.data.data.manufactorData.data;
                    this.page = response.data.data.manufactorData.currentPage;
                    this.total = response.data.data.manufactorData.count;
                })
                if (!this.loginInfo) {
                    this.loginInfo = JSON.parse(window.localStorage.getItem('userInfo') || null);
                    this.username = this.loginInfo.username;
                }
            }, handleRowEdit(index, row) {
                this.$router.push({name: 'manufactor_add', query: {id: row.id}})
            },
        },
        components: {},
        mounted() {
            let thePage = localStorage.getItem('manufactorPage');
            if (thePage == null) {
                thePage = 1;
            }
            this.page = Number(thePage);
            console.log(this.page);
            this.getList();
        }
    }

</script>

<style scoped>

</style>
