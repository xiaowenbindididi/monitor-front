<template>
  <div class="mod-user">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <!-- <el-form-item>
        <el-input v-model="dataForm.userName" placeholder="还没做" clearable></el-input>
      </el-form-item> -->
      <!-- <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <el-button v-if="isAuth('sys:user:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>
        <el-button v-if="isAuth('sys:user:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>
      </el-form-item> -->
    </el-form>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <!-- <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column> -->
      <el-table-column
        prop="id"
        header-align="center"
        align="center"
        width="80"
        label="序号">
      </el-table-column>
      <el-table-column
        prop="jobId"
        header-align="center"
        align="center"
        width="80"
        label="任务ID">
      </el-table-column>
      <el-table-column
        prop="jobName"
        header-align="center"
        align="center"
        label="任务名">
      </el-table-column>
      <el-table-column
        prop="relatedProject"
        header-align="center"
        align="center"
        label="关联项目">
      </el-table-column>
      <el-table-column
        prop="productOwner"
        header-align="center"
        align="center"
        label="产品经理">
      </el-table-column>
      <el-table-column
        prop="developer"
        header-align="center"
        align="center"
        label="开发人员">
      </el-table-column>
      <el-table-column
        prop="maintainer"
        header-align="center"
        align="center"
        label="运维人员">
      </el-table-column>
      <el-table-column
        prop="jobType"
        header-align="center"
        align="center"
        label="任务类型">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.status === 0" size="small" type="danger">周期性</el-tag>
          <el-tag v-else size="small">单次</el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="config"
        header-align="center"
        align="center"
        label="cron配置">
      </el-table-column>
      <el-table-column
        prop="status"
        header-align="center"
        align="center"
        label="状态">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.status === 0" size="small" type="danger">异常</el-tag>
          <el-tag v-else size="small">正常</el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="createTime"
        header-align="center"
        align="center"
        width="180"
        label="创建时间">
      </el-table-column>
      <el-table-column
        prop="nextStartTime"
        header-align="center"
        align="center"
        width="180"
        label="下次开始时间">
      </el-table-column>
      <el-table-column
        prop="jobDesc"
        header-align="center"
        align="center"
        label="描述">
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="jump(scope.row.jobId)">详情</el-button>
          <!-- <button @click="jump(scope.row.jobId)">详情</button> -->
          <!-- <el-button v-if="isAuth('sys:user:update')" type="text" size="small" @click="addOrUpdateHandle(scope.row.userId)">修改</el-button> -->
          <!-- <el-button v-if="isAuth('sys:user:delete')" type="text" size="small" @click="deleteHandle(scope.row.userId)">删除</el-button> -->
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 弹窗, 新增 / 修改 -->
    <add-or-update v-if="addOrUpdateVisible" ref="AddOrUpdate" @refreshDataList="getDataList"></add-or-update>
  </div>
</template>

<script>
  import AddOrUpdate from './job-detail'
  export default {
    data () {
      return {
        dataForm: {
          jobName: '',
          id: 0
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false
      }
    },
    components: {
      AddOrUpdate
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/spider/getJobs'),
          method: 'post',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'limit': this.pageSize
          })
        }).then(({data}) => {
          console.log('alljobs' + data)
          if (data && data.code === 0) {
            this.dataList = data.page.list
            this.totalPage = data.page.totalCount
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      sizeChangeHandle (val) {
        this.pageSize = val
        this.pageIndex = 1
        this.getDataList()
      },
      // 当前页
      currentChangeHandle (val) {
        this.pageIndex = val
        this.getDataList()
      },
      jump (id) {
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.AddOrUpdate.init(id)
        })
      }
    }
  }
</script>
