<template>
  <el-table :data="getbableInfo" stripe style="width: 100%;padding-top: 15px;">
    <el-table-column prop="content" label="预警林地" min-width="200"></el-table-column>
    <el-table-column prop="level" label="预警等级" width="195" align="center"></el-table-column>
    <el-table-column prop="people" label="发布预警人" width="100" align="center"></el-table-column>
  </el-table>
</template>

<script>
import { transactionList } from "@/api/remote-search";

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        success: "success",
        pending: "danger"
      };
      return statusMap[status];
    },
    orderNoFilter(str) {
      return str.substring(0, 30);
    }
  },
  data() {
    return {
      
      getbableInfo: []
    };
  },
  mounted() {
    this.getInfo();
  },
  methods: {
    getInfo() {
      var self = this;
      self.$http
        .get(this.baseUrl + "/indexfunction/info")
        .then(function(response) {
          var dd = [];
          var res = response.data;
          console.log(res);
          self.getbableInfo = dd;
          for (var i = 0; i < res.length; i++) {
            dd.push({
              content: res[i].content,
              level: res[i].level,
              people: res[i].userId
            });
          }
         
        });
    },
    //change,play实现表格自动滚动
    change() {
      //把第一条数据插入数组最后一条
      this.getInfo.push(this.getInfo[0]);
      //删除数组中第一条数据
      this.getInfo.shift();
    }
  }
};
</script>
