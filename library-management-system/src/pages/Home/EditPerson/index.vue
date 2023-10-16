<!--<template>-->
<!--  <div>-->
<!--    <el-form :model="person" label-width="100px" ref="form">-->
<!--      <el-form-item label="姓名">-->
<!--        <el-input v-model="person.readerName"></el-input>-->
<!--      </el-form-item>-->
<!--      <el-form-item label="电话">-->
<!--        <el-input v-model="person.phone"></el-input>-->
<!--      </el-form-item>-->
<!--      <el-form-item label="邮箱">-->
<!--        <el-input v-model="person.email"></el-input>-->
<!--      </el-form-item>-->
<!--      &lt;!&ndash; ...其他字段... &ndash;&gt;-->

<!--      <el-form-item>-->
<!--        <el-button type="primary" @click="updatePerson">保存</el-button>-->
<!--      </el-form-item>-->
<!--    </el-form>-->
<!--  </div>-->
<!--</template>-->

<!--<script>-->
<!--import { getPerson, updatePerson } from '@/api'-->

<!--export default {-->
<!--  name: 'EditPerson',-->
<!--  data() {-->
<!--    return {-->
<!--      person: {}-->
<!--    }-->
<!--  },-->
<!--  methods: {-->
<!--    updatePerson() {-->
<!--      this.$refs.form.validate(valid => {-->
<!--        if (valid) {-->
<!--          updatePerson(this.person)-->
<!--            .then(res => {-->
<!--              console.log(res)-->
<!--              if (res.status === 200) {-->
<!--                this.$message({-->
<!--                  showClose: true,-->
<!--                  message: '人员信息已更新！',-->
<!--                  type: 'success'-->
<!--                })-->
<!--                // 可选：导航回人员列表页面-->
<!--                this.$router.push({ name: 'AdminPerson' })-->
<!--              }-->
<!--            })-->
<!--            .catch(err => {-->
<!--              console.log(err.message)-->
<!--              this.$message({-->
<!--                showClose: true,-->
<!--                message: '更新人员信息失败！',-->
<!--                type: 'error'-->
<!--              })-->
<!--            })-->
<!--        } else {-->
<!--          this.$message.error('表单验证失败，请检查输入项！')-->
<!--        }-->
<!--      })-->
<!--    }-->
<!--  },-->
<!--  mounted() {-->
<!--    const readerId = this.$route.params.readerId-->
<!--    // 使用 readerId 参数获取人员数据-->
<!--    getPerson(readerId)-->
<!--      .then(res => {-->
<!--        this.person = res.data // 假设响应包含人员的数据-->
<!--      })-->
<!--      .catch(err => {-->
<!--        console.log(err.message)-->
<!--        this.$message.error('获取人员信息失败！')-->
<!--      })-->
<!--  }-->
<!--}-->
<!--</script>-->
