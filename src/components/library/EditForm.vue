<template>
    <div>
        <i class="el-icon-circle-plus-outline" @click="dialogFormVisible=true"></i>
        <el-dialog
         title="添加/修改图书"
         :visible.sync="dialogFormVisible"
         @close="clear"
        >
         <el-form v-model="form" style="text-align:left" ref="form" :rules="formRules">
             <el-form-item label="书名" :label-width="formLabelWidth" prop="title">
                <el-input v-model="form.title" clearable autocomplete="off" placeholder="不加《》"></el-input>
             </el-form-item>
             <el-form-item label="作者" :label-width="formLabelWidth" prop="author">
                <el-input v-model="form.author" clearable autocomplete="off"></el-input>
             </el-form-item>
             <el-form-item label="出版日期" :label-width="formLabelWidth" prop="date">
                <!-- <el-input v-model="form.date" autocomplete="off"></el-input> -->
                <el-date-picker v-model="form.date" clearable type="date" placeholder="选择日期"></el-date-picker>
             </el-form-item>
             <el-form-item label="出版社" :label-width="formLabelWidth" prop="press">
                <el-input v-model="form.press" clearable autocomplete="off"></el-input>
             </el-form-item>
             <el-form-item label="封面" :label-width="formLabelWidth" prop="cover">
                <el-input v-model="form.cover" autocomplete="off" placeholder="图片 URL"></el-input>
                <img-upload @onUpload="uploadImg" ref="imgUpload"></img-upload>
             </el-form-item>
             <el-form-item label="简介" :label-width="formLabelWidth" prop="abs">
                <el-input type="textarea" v-model="form.abs" clearable autocomplete="off"></el-input>
             </el-form-item>
             <el-form-item label="分类" :label-width="formLabelWidth" prop="cid">
                <el-select v-model="form.category.id" clearable placeholder="请选择分类">
                    <el-option label="文学" value="1"></el-option>
                    <el-option label="流行" value="2"></el-option>
                    <el-option label="文化" value="3"></el-option>
                    <el-option label="生活" value="4"></el-option>
                    <el-option label="经管" value="5"></el-option>
                    <el-option label="科技" value="6"></el-option>
                </el-select>
             </el-form-item>
             <el-form-item prop="id" style="height:0">
                 <el-input type="hidden" v-model="form.id" autocomplete="off"></el-input>
             </el-form-item>
         </el-form>
         <div slot="footer" class="dialog-footer">
             <el-button @click="dialogFormVisible=false">取消</el-button>
             <el-button type="primary" @click="onSubmit">确定</el-button>
         </div>
         </el-dialog>
    </div>
</template>

<script>
import ImgUpload from './ImgUpload'
    export default {
  components: { ImgUpload },
        name: 'EditForm',
        data () {
            return {
                dialogFormVisible: false,
                form: {
                    id: '',
                    title: '',
                    author: '',
                    date:'',
                    press:'',
                    cover:'',
                    abs:'',
                    category:{
                        id:'',
                        name:''
                    }
                },
                formRules: {
                    title:[{required: true, message: "不能为空", trigger: "blur"}],
                    date:[{required: true, message: "不能为空", trigger: "blur"}],
                },
                formLabelWidth:'120px'
            }
        },
        methods: {
            uploadImg () {
                this.form.cover=this.$refs.imgUpload.url
            },
            clear(){
                this.form ={
                    id:'',
                    title:'',
                    author:'',
                    date:'',
                    press:'',
                    cover:'',
                    abs:'',
                    category:''
                }
            },
            onSubmit(){
                this.$axios
                .post('/books',{
                    id: this.form.id,
                    cover: this.form.cover,
                    title: this.form.title,
                    author: this.form.author,
                    date:this.form.date,
                    press: this.form.press,
                    abs: this.form.abs,
                    category: this.form.category
                }).then(resp=>{
                    if(resp && resp.status === 200){
                        this.dialogFormVisible = false
                        this.$emit('onSubmit')
                        // 触发提交事件
                    }
                })
            }

        }
    }
</script>
<style scoped>
.el-icon-circle-plus-outline{
    margin: 50px 0 0 20px;
    font-size: 100px;
    float: left;
    cursor:pointer;
}
</style>