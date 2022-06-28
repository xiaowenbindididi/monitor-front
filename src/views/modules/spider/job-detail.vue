
<template>
    <el-dialog
    :title="'任务详情'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" ref="dataForm" label-width="80px">
      <el-form-item label="任务标识" prop="jobId">
        <el-input v-model="dataForm.jobId" placeholder="任务标识"></el-input>
      </el-form-item>
      <el-form-item label="任务开始时间"  prop="startTime">
        <el-input v-model="dataForm.startTime" placeholder="任务开始时间"></el-input>
      </el-form-item>
      <el-form-item label="任务结束时间" prop="endTime">
        <el-input v-model="dataForm.endTime" placeholder="任务结束时间"></el-input>
      </el-form-item>
      <el-form-item label="指标" prop="metrics">
        <el-input v-model="dataForm.metrics" placeholder="指标"></el-input>
      </el-form-item>
    </el-form>
    </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        roleList: [],
        dataForm: {
          id: ''
          // jobId: '',
          // startTime: null,
          // endTime: null,
          // metrics: null
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || ''
        this.$http({
          url: this.$http.adornUrl('/spider/getJD'),
          method: 'post',
          params: {jobId: this.dataForm.id}
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataForm.jobId = data.detail.jobId
            console.log(this.dataForm.jobId)
            this.dataForm.startTime = data.detail.startTime
            this.dataForm.endTime = data.detail.endTime
            this.dataForm.metrics = data.detail.metrics
          } else {
            this.dataForm.jobId = ''
            console.log(this.dataForm.jobId)
            this.dataForm.startTime = null
            this.dataForm.endTime = null
            this.dataForm.metrics = ''
          }
          this.visible = true
          this.$nextTick(() => {
            this.$refs['dataForm'].resetFields()
          })
        })
      }
    }
  }
</script>