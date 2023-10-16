<template>
  <div>
    <el-input
        v-model="searchKeyword"
        placeholder="输入关键字进行搜索"
        @keyup.enter.native="searchperson"
        style="margin-bottom: 10px;"

    >
    </el-input>

    <el-table
        :data="filteredReaderList"
        style="width: 100%"
        :default-sort="{ prop: 'readerName', order: 'descending' }"
    >
      <!--      readerName: '',-->
      <!--      phone: '',-->
      <!--      email: '',-->
      <!--      borrowTimes: 0,-->
      <!--      ovdTimes: 0-->

      <el-table-column type="expand">
        <template slot-scope="props">
          <el-form label-position="left" class="demo-table-expand">
            <el-form-item label="姓名">
              <span>{{ props.row.readerName }}</span>&nbsp;
              <el-button @click="changePersonName(props.row)"
                         type="text" style="float: right" size="mini"
                         icon="el-icon-edit">修改
              </el-button>
            </el-form-item>
            <el-form-item label="电话">
              <span>{{ props.row.phone }}</span>&nbsp;
              <el-button @click="changePersonPhone(props.row)"
                         type="text" style="float: right" size="mini"
                         icon="el-icon-edit">修改
              </el-button>
            </el-form-item>
            <el-form-item label="邮箱">
              <span>{{ props.row.email }}</span>&nbsp;
              <el-button @click="changePersonEmail(props.row)" type="text"
                         style="float: right" size="mini"
                         icon="el-icon-edit">修改
              </el-button>
            </el-form-item>
            <el-form-item label="借阅次数">
              <span>{{ props.row.borrowTimes }}</span>&nbsp;
              <el-button
                  @click="changePersonBorrowTimes(props.row)" type="text"
                  style="float: right" size="mini"
                  icon="el-icon-edit">修改
              </el-button>
            </el-form-item>
            <el-form-item label="逾期次数">
              <span>{{ props.row.ovdTimes }}</span>&nbsp;
              <el-button
                  @click="changePersonOvdTimes(props.row)" type="text"
                  style="float: right" size="mini"
                  icon="el-icon-edit">修改
              </el-button>
            </el-form-item>
          </el-form>
        </template>
      </el-table-column>
      <!-- ...已有的表格列... -->
      <el-table-column prop="readerName" label="姓名" sortable width=80>
      </el-table-column>
      <el-table-column prop="phone" sortable label="电话" width=120>
      </el-table-column>
      <el-table-column prop="email" sortable label="邮箱" width=180>
      </el-table-column>
      <el-table-column prop="borrowTimes" sortable label="借阅次数" width=110>
      </el-table-column>
      <el-table-column prop="ovdTimes" sortable label="逾期次数" width=110>
      </el-table-column>

      <el-table-column label="操作">
        <template slot-scope="scope">
          <!--           删除按钮 -->
          <el-popconfirm
              title="确认删除该人员吗？"
              @confirm="delPerson(scope.$index, scope.row)"
          >
            <el-button size="mini" style="margin-right: 5px" type="primary" plain slot="reference">
              删除人员
            </el-button>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import {mapState} from 'vuex'
import {changePersonInfo, delPerson, searchPerson} from '@/api'
import qs from 'qs'
// import {editPerson} from '../EditPerson'

export default {
  name: 'AdminPerson',
  computed: {
    ...mapState({
      readerList(state) {
        return state.User.readerList
      }
    })
  },
  data() {
    return {
      searchKeyword: '', // 搜索关键字
      filteredReaderList: [], // 筛选后的数据
      readerName: '',
      phone: '',
      email: '',
      borrowTimes: 0,
      ovdTimes: 0,
      readerId: ''
    };
  },
  methods: {
    changePersonName(row) {
      // console.log(row);
      var readerName = row.readerName;
      var phone = row.phone;
      var email = row.email;
      var borrowTimes = row.borrowTimes;
      var ovdTimes = row.ovdTimes;
      var readerId = row.readerId;

      this.$prompt("请输入姓名", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        inputValue: row.readerName,
      })
          .then(({value}) => {
            this.$message({
              type: "success",
              message: "你修改的姓名是: " + value,
            });
            // 修改的信息
            readerName = value;
            var infoObj = {readerId, readerName, phone, email, borrowTimes, ovdTimes};
            changePersonInfo(qs.stringify(infoObj)).then(
                (res) => {
                  this.$store.dispatch("initReaderList");
                  this.filteredReaderList = this.readerList;
                  this.reload();
                },
                (err) => {
                  console.log(err.message);
                }
            );
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "取消输入",
            });
          });
    },

    changePersonPhone(row) {
      console.log(row);
      var readerName = row.readerName;
      var phone = row.phone;
      var email = row.email;
      var borrowTimes = row.borrowTimes;
      var ovdTimes = row.ovdTimes;
      var readerId = row.readerId;

      this.$prompt("请输入电话", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        inputValue: row.phone,
      })
          .then(({value}) => {
            this.$message({
              type: "success",
              message: "你修改的电话是: " + value,
            });
            // 修改的信息
            phone = value;
            var infoObj = {readerId, readerName, phone, email, borrowTimes, ovdTimes};
            changePersonInfo(qs.stringify(infoObj)).then(
                (res) => {
                  this.$store.dispatch("initReaderList");
                  this.filteredReaderList = this.readerList;
                },
                (err) => {
                  console.log(err.message);
                }
            );
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "取消输入",
            });
          });
    },
    changePersonEmail(row) {
      console.log(row);
      var readerName = row.readerName;
      var phone = row.phone;
      var email = row.email;
      var borrowTimes = row.borrowTimes;
      var ovdTimes = row.ovdTimes;
      var readerId = row.readerId;

      this.$prompt("请输入邮箱", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        inputValue: row.email,
      })
          .then(({value}) => {
            this.$message({
              type: "success",
              message: "你修改的邮箱是: " + value,
            });
            // 修改的信息
            phone = value;
            var infoObj = {readerId, readerName, phone, email, borrowTimes, ovdTimes};
            changePersonInfo(qs.stringify(infoObj)).then(
                (res) => {
                  this.$store.dispatch("initReaderList");
                  this.filteredReaderList = this.readerList;
                },
                (err) => {
                  console.log(err.message);
                }
            );
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "取消输入",
            });
          });
    },

    changePersonBorrowTimes(row) {
      console.log(row);
      var readerName = row.readerName;
      var phone = row.phone;
      var email = row.email;
      var borrowTimes = row.borrowTimes;
      var ovdTimes = row.ovdTimes;
      var readerId = row.readerId;

      this.$prompt("请输入借阅次数", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        inputValue: row.borrowTimes,
      })
          .then(({value}) => {
            this.$message({
              type: "success",
              message: "你修改的借阅次数是: " + value,
            });
            // 修改的信息
            borrowTimes = value;
            var infoObj = {readerId, readerName, phone, email, borrowTimes, ovdTimes};
            changePersonInfo(qs.stringify(infoObj)).then(
                (res) => {
                  this.$store.dispatch("initReaderList");
                  this.filteredReaderList = this.readerList;
                },
                (err) => {
                  console.log(err.message);
                }
            );
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "取消输入",
            });
          });
    },
    changePersonOvdTimes(row) {
      console.log(row);
      var readerName = row.readerName;
      var phone = row.phone;
      var email = row.email;
      var borrowTimes = row.borrowTimes;
      var ovdTimes = row.ovdTimes;
      var readerId = row.readerId;

      this.$prompt("请输入逾期次数", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        inputValue: row.ovdTimes,
      })
          .then(({value}) => {
            this.$message({
              type: "success",
              message: "你修改的逾期次数是: " + value,
            });
            // 修改的信息
            ovdTimes = value;
            var infoObj = {readerId, readerName, phone, email, borrowTimes, ovdTimes};
            changePersonInfo(qs.stringify(infoObj)).then(
                (res) => {
                  this.$store.dispatch("initReaderList");
                  this.filteredReaderList = this.readerList;
                },
                (err) => {
                  console.log(err.message);
                }
            );
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "取消输入",
            });
          });
    },

    delPerson(index, row) {
      console.log(index, row);
      let infoObj = {readerId: row.readerId}
      delPerson(qs.stringify(infoObj)).then(res => {
        console.log(res);
        if (res.status == 200) {
          this.$message({
            showClose: true,
            message: "删除人员成功！",
            type: "success",
          });
        }
        // 删除后，你可能想要刷新人员列表
        this.$store.dispatch('initReaderList');
        this.filteredReaderList = this.readerList;
      });
    },

    searchperson(e) {
      this.loading = true;
      console.log(this.searchKeyword)
      searchPerson(qs.stringify({data: this.searchKeyword})).then(
          (res) => {
            this.loading = false;
            e.target.blur();
            this.flag = 1;
            this.filteredReaderList = res.data;
            console.log(res);
            if (res.status == 0) {
              this.$message({
                showClose: true,
                message: "未找到相关用户！",
                type: "error",
              });
            }
          },
          (err) => {
            this.loading = false;
            console.log(err.message);
          }
      );
    },

  },
  mounted() {
    this.$store.dispatch('initReaderList');
    this.filteredReaderList = this.readerList; // 初始化筛选后的数据为全部数据

  }
}

</script>

<style lang="less" scoped>
.demo-table-expand {
  font-size: 0;
}

.demo-table-expand label {
  width: 90px;
  color: #99a9bf;
}

.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 50%;
}
</style>