// 所有学生成绩查询

<template>
  <div class="all">
    <!-- <el-table :data="pagination.records" border>
      <el-table-column fixed="left" prop="studentName" label="姓名" width="180"></el-table-column>
      <el-table-column prop="institute" label="学院" width="200"></el-table-column>
      <el-table-column prop="major" label="专业" width="200"></el-table-column>
      <el-table-column prop="grade" label="年级" width="200"></el-table-column>
      <el-table-column prop="subject" label="考试科目" width="200"></el-table-column>
      <el-table-column prop="clazz" label="班级" width="100"></el-table-column>
      <el-table-column fixed="right" label="查看成绩" width="150">
        <template slot-scope="scope">
          <el-button @click="checkGrade(scope.row.studentId)" type="primary" size="small">查看成绩</el-button>
        </template>
      </el-table-column>
    </el-table> -->
    <el-form size="">
      <el-form-item class="searchSub">
        <el-input
                v-model="search"
                size="big"
                placeholder="输入关键字搜索"
                class="search-input"
                />
          <el-button type="primary" @click="searchSelect()">搜索科目</el-button>
        </el-form-item>
    </el-form>
     <el-table
      :data="pagination.records"
      border
      style="width: 100%">
      <el-table-column fixed="left" prop="studentName" label="姓名" width="180"></el-table-column>
      <el-table-column prop="institute" label="学院" width="200"></el-table-column>
      <el-table-column prop="major" label="专业" width="200"></el-table-column>
      <el-table-column prop="grade" label="年级" width="200"></el-table-column>
      <el-table-column prop="subject" label="考试科目" width="200"></el-table-column>
      <el-table-column prop="clazz" label="班级" width="100"></el-table-column>
      <el-table-column prop="studentId" label="学号" width="200"></el-table-column>
      <el-table-column fixed="right" prop="etScore" label="查看成绩" width="150" >
       
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagination.current"
      :page-sizes="[6, 10]"
      :page-size="pagination.size"
      layout="total, sizes, prev, pager, next, jumper"
      :total="pagination.total"
      class="page"
    ></el-pagination>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pagination: {
        //分页后的考试信息
        current: 1, //当前页
        total: null, //记录条数
        size: 6 //每页条数
      },
      search:'',
      tableData:{}
    };
  },
  mounted() {
    this.getAnswerInfo();
  },
  methods: {
    getAnswerInfo() {


      this.$axios(`api/scores/${this.pagination.current}/${this.pagination.size}?key=${this.search}`).then(res => {
        this.pagination = res.data.data;     //这里面只能把学生学号、考试科目和成绩给赋值 
        let stu = {};
        console.log(res)
        for(let i = 0; i < this.pagination.total; i++)
        {
          stu = this.pagination.records[i].student;
            this.pagination.records[i].studentId = stu.studentId;
            this.pagination.records[i].institute = stu.institute;
            this.pagination.records[i].major = stu.major;
            this.pagination.records[i].grade = stu.grade;
            this.pagination.records[i].clazz = stu.clazz;
            this.pagination.records[i].studentName = stu.studentName;
        }
      }).catch(error => {});
      
    },
    //改变当前记录条数
    handleSizeChange(val) {
      this.pagination.size = val;
      this.getAnswerInfo();
    },
    //改变当前页码，重新发送请求
    handleCurrentChange(val) {
      this.pagination.current = val;
      this.getAnswerInfo();
    },
    checkGrade(studentId) {
      this.$router.push({ path: "/grade", query: { studentId: studentId } });
    },
    searchSelect(){
      this.getAnswerInfo();
    }
  }
};
</script>
<style lang="scss" scoped>
.all {
  padding: 0px 40px;
  .page {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .edit {
    margin-left: 20px;
  }

  .searchSub{
    width:400px;
    float: right;
    margin-bottom: 30px;
  }
  .search-input{
    display: block;
    width: 200px;
    float: left;
  }
  .el-table tr {
    background-color: #dd5862 !important;
  }
}
.el-table .warning-row {
  background: #000 !important;
}

.el-table .success-row {
  background: #dd5862;
}
</style>
