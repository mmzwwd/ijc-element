<template>
  <div class="pdf">
    <div v-show="fileType === 'pdf'" class="pdf">
      <p class="arrow">
        // 上一页
      </p>
      // 自己引入就可以使用,这里我的需求是做了分页功能,如果不需要分页功能,只要src就可以了
      <!-- <pdf
      :src="src" // src需要展示的PDF地址
      :page="currentPage" // 当前展示的PDF页码
      @num-pages="pageCount=$event" // PDF文件总页码
      @page-loaded="currentPage=$event" // 一开始加载的页面
      @loaded="loadPdfHandler"> // 加载事件
    </pdf> -->
      <iframe :src="src" width="100%" height="800" allowfullscreen webkitallowfullscreen />
    </div>
  </div>
</template>

<script>
import pdf from 'vue-pdf'
import axios from 'axios'
export default {
  components: { pdf },
  data() {
    return {
      currentPage: 0, // pdf文件页码
      pageCount: 0, // pdf文件总页数
      fileType: 'pdf', // 文件类型

      src: '' // pdf文件地址
    }
  },

  mounted() {
    this.aaa()
  },
  methods: {
    aaa() {
      const then = this
      axios({
        method: 'get',
        type: 'application/pdf;charset=UTF-8',
        url: 'http://api.yingheit.com/tutorBase/downLoadPdf',
        headers: { 'token': 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjdXJyZW50VGltZSI6MTYxMjI0NTM4NTE2NSwiaXNzIjoiYXV0aDAiLCJleHAiOjE2MTIyNDU2ODUsImFjY291bnQiOiIzNzA4MjkxOTk2MDUyOTczMTQifQ.ugka4M5y85MIyHxdk5dsv7NVQreX-5EwKRhInfOixY0' },
        responseType: 'arraybuffer'
      }).then(function(response) {
        const a = 'http://www.windriver.com.cn/downloads/pdfviewer/web/viewer.aspx?pdfurl=/downloads/files/wp-requirements-virtualization-next-gen-industrial-cs-white-paper-cn.pdf'
        const b = 'https://file.keking.cn/onlinePreview?url=https%3A%2F%2Ffile.keking.cn%2Fdemo%2F3707.pdf.pdf'
        const pdfUrl = then.getObjectURL(response.data)
        then.src = './static/web/viewer.html'
        sessionStorage.setItem('_imgUrl', response.data)
        console.log(encodeURIComponent(pdfUrl))
      })
    },
    getObjectURL(data) {
      let url = null
      const file = new Blob([data], { type: 'application/pdf;charset=UTF-8' })
      console.log(file)
      console.log(window.URL.createObjectURL(file))
      if (window.createObjectURL != undefined) {
        // 通用
        url = window.createObjectURL(file)
        console.log('通用')
      } else if (window.webkitURL != undefined) {
        console.log('兼容谷歌', file)
        // 兼容谷歌
        try {
          url = window.webkitURL.createObjectURL(file)
        } catch (error) {}
      } else if (window.URL != undefined) {
        console.log('兼容其他')
        // 兼容其他
        try {
          url = window.URL.createObjectURL(file)
        } catch (error) {}
      }
      console.log(url)
      // 将转化后url赋值
      return url
    }
  }

}

</script>
<style  scoped>
</style>
