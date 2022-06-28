
<template>
    <el-dialog
    :close-on-click-modal="false"
    :visible.sync="visible">
    <div>
        测试
    </div>
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
          console.log('detail' + data.detail.jobId)
          this.visible = true
          this.$nextTick(() => {
            this.$refs['dataForm'].resetFields()
          })
        })
      }
    }
  }
</script>