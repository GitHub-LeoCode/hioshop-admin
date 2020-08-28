<template>
    <div class="content-page">
        <div class="content-nav">
            <el-breadcrumb class="breadcrumb" separator="/">
                <el-breadcrumb-item :to="{ name: 'manufactor' }">厂商</el-breadcrumb-item>
                <el-breadcrumb-item>{{infoForm.id ? '编辑' : '新增'}}</el-breadcrumb-item>
            </el-breadcrumb>
            <div class="operation-nav">
                <el-button type="primary" @click="onSubmitInfo">确定保存</el-button>
                <el-button @click="goBackPage" icon="arrow-left">返回列表</el-button>
            </div>
        </div>
        <div class="content-main">
            <div class="form-table-box">
                <el-form ref="infoForm" :rules="infoRules" :model="infoForm" label-width="120px">
                    <el-form-item label="厂商名称" prop="name">
                        <el-input v-model="infoForm.name"></el-input>
                    </el-form-item>
                    <el-form-item label="厂商地址" prop="address">
                        <el-input v-model="infoForm.address"></el-input>
                    </el-form-item>
                    <el-form-item label="厂商电话" prop="phone">
                        <el-input v-model="infoForm.phone"></el-input>
                    </el-form-item>
                    <el-form-item label="状态" prop="is_delete">
                        <el-input v-model="infoForm.is_delete"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="onSubmitInfo">确定保存</el-button>
                        <el-button @click="goBackPage">返回列表</el-button>
                        <el-button type="danger" class="float-right" @click="onCopyGood">复制厂商</el-button>
                    </el-form-item>
                </el-form>
            </div>
        </div>
    </div>
</template>

<script>
    import api from '@/config/api';
    import {quillEditor} from 'vue-quill-editor'
    import ElForm from "../../../../node_modules/element-ui/packages/form/src/form.vue";

    export default {
        data() {
            return {
                infoForm: {
                    name: "",
                    address: "",
                    phone: "",
                    is_delete: 0
                },
                infoRules: {
                    name: [
                        {required: true, message: '请输入名称', trigger: 'blur'},
                    ],
                    address: [
                        {required: true, message: '请输入地址', trigger: 'blur'},
                    ],
                    phone: [
                        {required: true, message: '请输入电话', trigger: 'blur'},
                    ]
                },
            }
        },
        methods: {
            goBackPage() {
                this.$router.go(-1);
            },
            onSubmitInfo() {
                this.$refs['infoForm'].validate((valid) => {
                    if (valid) {
                        this.axios.post('manufactor/store',
                            {
                                info: this.infoForm,
                            }).then((response) => {
                            if (response.data.errno === 0) {
                                this.$message({
                                    type: 'success',
                                    message: '保存成功'
                                });
                                this.$router.go(-1);
                            } else {
                                this.$message({
                                    type: 'error',
                                    message: '保存失败'
                                })
                            }
                        })
                    } else {
                        return false;
                    }
                });
            },
            getInfo() {
                if (this.infoForm.id <= 0) {
                    return false
                }
                let that = this
                this.axios.get('manufactor/info', {
                    params: {
                        id: that.infoForm.id
                    }
                }).then((response) => {
                    that.infoForm = response.data.data.data;
                })
            },
            mounted() {
                this.infoForm.id = this.$route.query.id || 0;
                this.getInfo();
                this.root = api.rootUrl;
            }
        },
    }

</script>

<style scoped>
    /* .edit_container{ */
    /*.avatar-uploader .el-upload {*/
    /*display: none;*/
    /*}*/
    .video-wrap {
        width: 300px;
    }

    .dialog-header {
        display: flex;
        justify-content: flex-start;
        margin-bottom: 10px;
    }

    .dialog-header .value {
        width: 150px;
        margin-right: 14px;
    }

    .input-wrap .el-input {
        width: 200px;
        float: left;
        margin: 0 20px 20px 0;
    }

    .input-wrap .el-input input {
        background-color: #fff !important;
        color: #233445 !important;
    }

    .specFormDialig .el-input {
        width: 150px;
        margin-right: 10px;
    }

    .el-select-class {
        width: 200px;
        margin-right: 20px;
    }

    .sepc-form {
        display: flex;
        justify-content: flex-start;
        margin-bottom: 10px;
    }

    .spec-form-wrap {
        margin-top: 0 !important;
    }

    .add-spec {
        margin-top: 10px;
    }

    .spec-form-wrap .header {
        display: flex;
        justify-content: flex-start;
    }

    .spec-form-wrap .header .l {
        width: 200px;
        margin-right: 20px;
    }

    .spec-form-wrap .header .m {
        width: 200px;
        margin-right: 20px;
    }

    .spec-form-wrap .header .m {
        width: 200px;
        margin-right: 20px;
    }

    /*.sepc-form div{*/
    /*margin-left: 0;*/
    /*}*/

    .float-right {
        float: right;
    }

    .sepc-form .el-input {
        width: 200px;
        margin-right: 20px;
    }

    .marginTop20 {
        margin-top: 20px;
    }

    .checkbox-wrap .checkbox-list {
        float: left;
        margin-right: 20px;
    }

    .upload_ad {
        display: none;
    }

    .upload_ad .el-upload--picture-card {
        display: none;
    }

    .ql-container {
        min-height: 200px;
        max-height: 400px;
        overflow-y: auto;
    }

    .image-uploader-diy {
        /*height: 200px;*/
        position: relative;
    }

    /*.dele-list-pic {*/
    /*position: absolute;*/
    /*left: 380px;*/
    /*top: 0px;*/
    /*}*/

    .image-uploader-diy .el-upload {
        border: 1px solid #d9d9d9;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }

    .image-uploader-diy .el-upload:hover {
        border-color: #20a0ff;
    }

    .image-uploader-diy .image-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 200px;
        height: 200px;
        line-height: 200px;
        text-align: center;
    }

    .image-uploader-diy .image-show {
        min-width: 105px;
        height: 105px;
        display: block;
    }

    .image-uploader-diy .new-image-uploader {
        font-size: 28px;
        color: #8c939d;
        width: 165px;
        height: 105px;
        line-height: 105px;
        text-align: center;
    }

    .image-uploader-diy .new-image-uploader .image-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 165px;
        height: 105px;
        line-height: 105px;
        text-align: center;
    }

    .image-uploader-diy .new-image-uploader .image-show {
        width: 165px;
        height: 105px;
        display: block;
    }

    .item-url-image-fuzhu .el-input {
        width: 260px;
    }

    .hidden {
        display: none;
    }
</style>
