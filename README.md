# vue-office

支持多种文件(docx、pdf、excel)预览的vue组件套装。

[查看demo演示](https://501351981.github.io/vue-office/examples/dist/)

## 功能特色
- 一站式：提供docx、pdf、excel多种文档的在线预览方案，有它就够了
- 简单：只需提供文档的src(网络地址)即可完成文档预览
- 体验好：选择每个文档的最佳预览方案，保证用户体验和性能都达到最佳状态

## 安装
```
//docx文档预览组件
npm install @vue-office/docx

//excel文档预览组件
npm install @vue-office/excel

//pdf文档预览组件
npm install @vue-office/pdf
```

## 使用示例
文档预览场景大致可以分为两种：
- 有文档网络地址，比如 https://***.docx
- 文件上传时预览，此时可以获取文件的ArrayBuffer或Blob

### docx文档的预览
**使用网络地址预览**
```vue
<template>
  <vue-office-docx :src="docx" @rendered="rendered"/>
</template>

<script>
//引入VueOfficeDocx组件
import VueOfficeDocx from '@vue-office/docx'

export default {
  components:{
    VueOfficeDocx
  },
  data(){
    return {
      docx: 'http://static.shanhuxueyuan.com/test6.docx' //设置文档网络地址，可以是相对地址
    }
  },
  methods:{
    rendered(){
      console.log("渲染完成")
    }
  }
}
</script>
```

**上传文件预览**

我们使用element的上传组件作为示例，当然也可以使用普通的input type="file"，只要能获取文件的arrayBuffer即可
```vue
<template>
  <div id="docx-demo">
    <el-upload :limit="1" :file-list="fileList" accept=".docx" :beforeUpload="beforeUpload" action="">
      <el-button size="small" type="warning">点击上传</el-button>
    </el-upload>
    <vue-office-docx :src="src" />
  </div>
</template>

<script>
import VueOfficeDocx from '@vue-office/docx'
export default {
  components: {
    VueOfficeDocx
  },
  data(){
    return {
      src:'',
      fileList:[]
    }
  },
  methods:{

    beforeUpload(file){
      let reader = new FileReader();
      reader.readAsArrayBuffer(file);
      reader.onload = (loadEvent) => {
        let arrayBuffer = loadEvent.target.result;
        this.src = arrayBuffer
      };
      return false
    }
  }
}
</script>
```
如果是原生的input type="file"，也是类似的
```vue
<template>
  <div>
    <input type="file" @change="changeHandle"/>
    <vue-office-docx :src="src"/>
  </div>
</template>

<script>
import VueOfficeDocx from '@vue-office/docx'

export default {
  components: {
    VueOfficeDocx
  },
  data(){
    return {
      src: ''
    }
  },
  methods:{
    changeHandle(event){
      let file = event.target.files[0]
      let fileReader = new FileReader()
      fileReader.readAsArrayBuffer(file)
      fileReader.onload =  () => {
        this.src = fileReader.result
      }
    }
  }
}
</script>
```


### excel文档预览
```vue
<template>
  <vue-office-excel :src="excel" @rendered="rendered"/>
</template>

<script>
//引入VueOfficeExcel组件
import VueOfficeExcel from '@vue-office/excel'
//引入相关样式
import '@vue-office/excel/lib/index.css'

export default {
  components:{
    VueOfficeExcel
  },
  data(){
    return {
      excel: 'http://static.shanhuxueyuan.com/demo/excel.xlsx'//设置文档地址
    }
  },
  methods:{
    rendered(){
      console.log("渲染完成")
    }
  }
}
</script>
```


### pdf文档预览
```vue
<template>
  <vue-office-pdf :src="pdf" @rendered="rendered"/>
</template>

<script>
//引入VueOfficePdf组件
import VueOfficePdf from '@vue-office/pdf'

export default {
  components:{
    VueOfficePdf
  },
  data(){
    return {
      pdf: 'http://static.shanhuxueyuan.com/test.pdf' //设置文档地址
    }
  },
  methods:{
    rendered(){
      console.log("渲染完成")
    }
  }
}
</script>
``` 

excel和pdf，也同样支持通过文件上传进行预览，代码和docx的预览一样。

## API

为了使用简单，这几种组件的API尽量保持一致，如果是某个组件特有的，会单独注明

### 属性

| 属性              | 说明                                         | 类型                        | 可选值            | 默认值 |
|-----------------|--------------------------------------------|---------------------------|----------------|-----|
| src             | 文档地址                                       | String, ArrayBuffer, Blob | -              | -   |
| request-options | 请求参数，对应window.fetch的请求参数，可以用来设置header等请求信息 | Object                    | 参考window.fetch | {}  |


### 事件

| 事件名      | 说明                          | 参数        |
|----------|-----------------------------|-----------|
| rendered | 首次渲染完成及每次src变化之后渲染完成都会触发该事件 |           |
| error    | 各种失败，包括网络请求失败，渲染失败等         | errorInfo |


