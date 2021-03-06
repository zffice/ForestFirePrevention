<template>
  <div class="app-container">

    <div>
      <FilenameOption v-model="filename" />
      <AutoWidthOption v-model="autoWidth" />
      <BookTypeOption v-model="bookType" />
      <el-button :loading="downloadLoading" style="margin:0 0 20px 20px;" type="primary" icon="el-icon-document" @click="handleDownload">
        Export Excel
      </el-button>
      <a href="https://panjiachen.github.io/vue-element-admin-site/feature/component/excel.html" target="_blank" style="margin-left:15px;">
        <el-tag type="info">Documentation</el-tag>
      </a>
    </div>
<!--报警信息表格制作   data:"用于存放请求数据回来的数组-->
    <el-table v-loading="listLoading" :data="list" element-loading-text="Loading..." border fit highlight-current-row>
      <el-table-column align="center" label="Id" width="95">
        <template slot-scope="scope"><!--这里取到当前单元格-->
          {{ scope.$index }}<!--直接取到该单元格值-->
        </template>
      </el-table-column>
      <el-table-column  prop="Title" label="Title">
        
          <!-- <template >
         
        </template>scope.row 直接取到该单元格对象{{ scope.row.title }}  slot-scope="scope"  
          -->
       
      </el-table-column>
       <el-table-column  prop="type" label="type">
       
      </el-table-column>
       <el-table-column  prop="level" label="level">
        
      </el-table-column>
      <el-table-column prop="Author" label="Author">
        <template >
          <!--<el-tag>{{ scope.row.author }}</el-tag>-->
        </template>
      </el-table-column>
      <el-table-column prop="content" label="content">
        
      </el-table-column>
      <el-table-column align="center"  prop="Date" label="Date" width="220">
        <template >
          
         <!--<span>{{ scope.row.timestamp | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>   <i class="el-icon-time" />-->
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { fetchList } from '@/api/article'
import { parseTime } from '@/utils'
// options components
import FilenameOption from './components/FilenameOption'
import AutoWidthOption from './components/AutoWidthOption'
import BookTypeOption from './components/BookTypeOption'

export default {
  name: 'ExportExcel',
  components: { FilenameOption, AutoWidthOption, BookTypeOption },
  data() {
    return {
      value: true,
      list: [
        {
          Title:'火险报警',
          type:'红色预警',
          level:'1级',
          content:'林场东南方式2号站点',
          Date:'2020年2月1日17点23分',
          Author:'测试提交'
        },
        {
          Title:'火险报警',
          type:'蓝色预警',
          level:'2级',
          content:'林场西南方式3号站点',
          Date:'2020年9月1日18点23分',
          Author:'三号检测员谢豆'
        },
        {
          Title:'火险报警',
          type:'橙色预警',
          level:'3级',
          content:'林场东北方式1号站点',
          Date:'2020年4月1日10点23分',
          Author:'二号检测员郑嘉怡'
        } ,
        {
          Title:'火险报警',
          type:'黄色预警',
          level:'2级',
          content:'林场东南方式2号站点',
          Date:'2020年2月2日20点23分',
          Author:'四号检测员郑菲'
        },
        {
          Title:'火险报警',
          type:'蓝色预警',
          level:'2级',
          content:'林场西南方式3号站点',
          Date:'2020年9月1日18点23分',
          Author:'三号检测员林泽坪'
        },
        {
          Title:'火险报警',
          type:'橙色预警',
          level:'3级',
          content:'林场东北方式1号站点',
          Date:'2020年4月1日10点23分',
          Author:'二号检测员张瑞'
        }
      ]
      /*listLoading: true,
      downloadLoading: false,
      filename: '',
      autoWidth: true,
      bookType: 'xlsx'*/
    }
  }/*,
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
      fetchList().then(response => {
        this.list = response.data.items
        this.listLoading = false
      })
    },
    handleDownload() {
      this.downloadLoading = true
      import('@/vendor/Export2Excel').then(excel => {
        const tHeader = ['Id', 'Title','type', 'level','Author', 'content', 'Date']
        const filterVal = ['id', 'title', 'author', 'pageviews', 'display_time']
        const list = this.list
        const data = this.formatJson(filterVal, list)
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename: this.filename,
          autoWidth: this.autoWidth,
          bookType: this.bookType
        })
        this.downloadLoading = false
      })
    }*/,
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => {
        if (j === 'timestamp') {
          return parseTime(v[j])
        } else {
          return v[j]
        }
      }))
    }
  }
/*}*/
</script>

<style>
.radio-label {
  font-size: 14px;
  color: #606266;
  line-height: 40px;
  padding: 0 12px 0 30px;
}
</style>
