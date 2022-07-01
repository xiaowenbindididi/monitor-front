
<template>
    <el-dialog
    :title="'任务详情'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" ref="dataForm" label-width="150px">
      <el-form-item label="任务标识" prop="jobId">
        <el-input v-model="dataForm.jobId" placeholder="任务标识"></el-input>
      </el-form-item>
      <el-form-item label="关键采集成功数" prop="keyParseSuc">
        <el-input v-model="dataForm.keyParseSuc" placeholder="关键采集成功数量"></el-input>
      </el-form-item>
      <el-form-item label="写入kafka成功数" prop="writeToKafkaSuc">
        <el-input v-model="dataForm.writeToKafkaSuc" placeholder="写入kafka成功数量"></el-input>
      </el-form-item>
      <el-form-item label="任务开始时间"  prop="startTime">
        <el-input v-model="dataForm.startTime" placeholder="任务开始时间"></el-input>
      </el-form-item>
      <el-form-item label="任务结束时间" prop="endTime">
        <el-input v-model="dataForm.endTime" placeholder="任务结束时间"></el-input>
      </el-form-item>
      <el-form-item label="扩展指标" prop="metrics">
        <el-input v-model="dataForm.metrics" placeholder="任务运行情况衡量指标"></el-input>
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
        //   jobId: null,
        //   startTime: null,
        //   endTime: null,
        //   metrics: null
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
            this.dataForm.keyParseSuc = data.detail.keyParseSuc
            this.dataForm.writeToKafkaSuc = data.detail.writeToKafkaSuc
          } else {
            this.dataForm.jobId = ''
            console.log(this.dataForm.jobId)
            this.dataForm.startTime = null
            this.dataForm.endTime = null
            this.dataForm.metrics = ''
            this.dataForm.keyParseSuc = null
            this.dataForm.writeToKafkaSuc = null
          }
          this.visible = true
          this.$nextTick(() => {
            // this.$refs['dataForm'].resetFields()
          })
        })
      }
    }
  }
</script>