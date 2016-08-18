<template>
  <form  enctype="multipart/form-data" method="post">
    <a class="btn btn-sm btn-default btn-upload">
      <span>选择文件</span>
      <input type="file" id="file" class="btn btn-primary" :name="name" @change = "inputfile()"  :multiple="mutifile" v-el:file>
    </a> 
  　<input type="button" value="上传"  @click = "fileUpload()" class="btn btn-sm btn-primary setmargin" :disabled="disabled"/>
    <span v-for = "item in nameArr">{{item.name}}</span>
    <span class="text-red">{{errorMsg}}</span>
  </form>
</template>
<script>
export default {
  components: {},
  data () {
    return {
      file:[],
      nameArr:[],
      errorMsg:'',
      disabled:false
    }
  },
  methods:{
    //选择文件后所触发的事件
    inputfile(){
      let name = this.file.name ;
      let _arr = [];
      let _this = this ;
      this.$set('nameArr', _arr) ;
      this.disabled = false;
      this.errorMsg = "";
      this.file = this.$els.file.files;
      this.type = this.type.join('|');
      let RegTest = new RegExp(`.${this.type}$`,'i');
      //检验文件后缀
      for(var i = 0 ; i < this.file.length ; i++ ){
        if (!RegTest.test(this.file[i].name )  ) {
          this.errorMsg = "请选择正确后缀的文件";
          this.disabled=true;
          return ;
        }
        _arr[i] = {} ; 
        _arr[i].name = this.file[i].name ;
      }
      this.$set('nameArr', _arr) 
      this.inputCallFun(this.file);
    },
    judgeSuffix(){
      let this = _this ;
      if(this.file.length == 0){
        this.errorMsg = "请选择文件后再上传！";
          return ;
      }
      this.file.forEach( function(element,index){
        if (element.name == ""  ) {
          _this.errorMsg = "请选择文件后再上传！";
          return false;
        }
      })
    },
    fileUpload(){
      let _this = this ;
      if(!this.judgeSuffix()){ return };
      if(typeof FormData !=== 'undefined') {　
        let formData = new FormData();
        let xhr = new XMLHttpRequest();
        // 建立一个upload表单项，值为上传的文件
        for(var i = 0 ; i < this.file.length ; i++ ){
        if (this.file[i].name == ""  ) {
          this.errorMsg = "请选择文件后再上传！";
          return ;
        }
      }
        xhr.open('POST', this.address,true);
        // 定义上传完成后的回调函数
        xhr.onload = function () {
        if (xhr.status === 200) {
          _this.file = [];
          _this.nameArr = [];
          _this.sucCallFun();
          } else {
          _this.falCallFun();
          }
        };
      xhr.send(formData);
      }
    }
  },
  props:{
    name :{
      type:String
    },
    mutifile:{
      type:Boolean,
      default: false
    },
    autoupload:{
      type:Boolean,
      default: false
    },
    type:{
      type:Array
    },
    address:{
      type:String
    },
    inputCallFun:{
      type:Function,
      default:function(){}
    },
    sucCallFun:{
      type:Function,
      default:function(){alert('上传成功！')}
    },
    falCallFun:{
      type:Function,
      default:function(){alert('上传失败，请重新上传一次！')}
    }
  }

}
</script>
<style lang="less">
 .btn-upload {
    position: relative;
    overflow: hidden;
}
.btn-upload input {
    position: absolute;
    top: 0;
    right: 0;
    margin: 0;
    padding: 0;
    font-size: 20px;
    cursor: pointer;
    opacity: 0;
}
.setmargin{
  margin-right:10px;
}
</style>
