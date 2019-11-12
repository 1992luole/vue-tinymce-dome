<template>
    <div>
        <p>tinymce的使用</p>
        <editor id="tinymce" v-model="tinyValue" :init="init"></editor>
    </div>
</template>
<script>
import tinymce from 'tinymce/tinymce'
import Editor from '@tinymce/tinymce-vue'
import 'tinymce/themes/silver'
// 插件
import 'tinymce/plugins/image'
import 'tinymce/plugins/link'
import 'tinymce/plugins/code'
import 'tinymce/plugins/table'
import 'tinymce/plugins/lists'
import 'tinymce/plugins/contextmenu'
import 'tinymce/plugins/wordcount'
import 'tinymce/plugins/colorpicker'
import 'tinymce/plugins/textcolor'
export default {
    data(){
        return{
            tinyValue:"",
            init: {
                language_url: '/tinymce/zh_CN.js',// 语言包的路径  pulic打包后就直接 在根目录下
                language: 'zh_CN',//语言
                skin_url: '/tinymce/skins/ui/oxide',// skin路径，注意这个，很多网上教程都是skins/lightgray，但是发现其实并没有这个玩意儿
                height: 300,//编辑器高度
                branding: false,//是否禁用“Powered by TinyMCE”
                menubar: false,//顶部菜单栏显示
                plugins: 'link lists image code table colorpicker textcolor wordcount contextmenu',
                toolbar:
                'bold italic underline strikethrough | fontsizeselect | forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist | outdent indent blockquote | undo redo | link unlink image code | removeformat',//工具栏
                images_upload_handler :function (blobInfo, success, failure) {//自定义上传
                    this.handleImgUpload(blobInfo, success, failure)
                }
            }
        }
    },
    methods: {
        handleImgUpload (blobInfo, success, failure) {
            let formdata = new FormData()
            formdata.set('upload_file', blobInfo.blob())
            axios.post('/api/upload', formdata).then(res => {
                success(res.data.data.src)
            }).catch(res => {
                failure('error')
            })
        }
    },
    mounted() {
        // 需要初始化 默认空对象
        tinymce.init({})
    },
    components:{
        Editor
    }
}
</script>