<template>
  <div class="home">
    <el-container style="width: 90%; border: 1px solid #eee; margin: auto">
      <el-main>
        <img
          alt="Vue logo"
          src="../assets/logo.png"
          style="width: 90%; height: 300px"
        />
        <div class="demo-input-size" sytle="width:90%;marin: auto">
          <el-row>
            <div class="demo-input-size">
              <br />
              <el-input
                size="medium"
                suffix-icon="el-icon-s-custom"
                style="width: 16%; margin-right: 25px"
                placeholder="姓名"
                v-model="query.name"
              >
              </el-input>
              <el-input
                size="medium"
                style="width: 16%; margin-right: 25px"
                placeholder="学校"
                suffix-icon="el-icon-office-building"
                v-model="query.company"
              >
              </el-input>
              <el-input
                size="medium"
                style="width: 16%; margin-right: 25px"
                placeholder="职称"
                suffix-icon="el-icon-medal"
                v-model="query.title"
              >
              </el-input>
              <el-input
                size="medium"
                style="width: 16%; margin-right: 25px"
                placeholder="研究方向"
                suffix-icon="el-icon-position"
                v-model="query.direction"
              >
              </el-input>
              <el-button
                type="primary"
                @click="fuzzyQuery()"
                style="margin-left: 10px"
                >查询</el-button
              >
              <el-button
                type="primary"
                @click="resetQuery()"
                style="margin-left: 20px"
                >重置</el-button
              >
            </div>
          </el-row>
        </div>
        <br />
        <el-table
          :data="tableData"
          :stripe="true"
          :lazy="true"
          style="width: 90%; margin: auto"
        >
          <el-table-column type="expand">
            <template slot-scope="props">
              <el-form label-position="left" inline class="demo-table-expand">
                <el-form-item label="照片">
                  <div class="demo-image__preview">
                    <el-image
                      style="width: 120px; height: 150px"
                      :src="props.row.image"
                    >
                    </el-image>
                  </div>
                </el-form-item>
                <el-form-item label="姓名">
                  <span>{{ props.row.name }}</span>
                </el-form-item>
                <el-form-item label="学校">
                  <span>{{ props.row.company }}</span>
                </el-form-item>
                <el-form-item label="职称">
                  <span>{{ props.row.title }}</span>
                </el-form-item>
                <el-form-item label="专业">
                  <span>{{ props.row.major }}</span>
                </el-form-item>
                <el-form-item label="研究方向">
                  <span>{{ props.row.direction }}</span>
                </el-form-item>
                <el-form-item label="电话">
                  <span>{{ props.row.phone }}</span>
                </el-form-item>
                <el-form-item label="邮箱">
                  <span>{{ props.row.email }}</span>
                </el-form-item>
                <el-form-item label="个人主页">
                  <el-link
                    type="primary"
                    v-bind:href="props.row.homepage"
                    target="_blank"
                    >{{ props.row.homepage }}</el-link
                  >
                </el-form-item>
              </el-form>
            </template>
          </el-table-column>
          <el-table-column label="姓名" prop="name" width="100%">
          </el-table-column>
          <el-table-column label="学校" prop="company" width="100%">
          </el-table-column>
          <el-table-column label="职称" prop="title" width="100%">
          </el-table-column>
          <el-table-column label="专业" prop="major" width="160%">
          </el-table-column>
          <el-table-column label="研究方向" prop="direction" min-width="0">
          </el-table-column> </el-table
        ><br />
        <div  style=" width:90%;text-align:right">
          <el-pagination
            background
            layout="sizes, prev, pager, next"
            @prev-click="preclick"
            @next-click="nextclick"
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :page-sizes="[10, 20, 30, 40, 50, 100]"
            :current-page="currentpage"
            :page-size="this.pagesize"
            :total="this.total"
          >
          </el-pagination>
        </div>
      </el-main>
      <el-footer>
        <div class="demo-input-size" sytle="width:90%;marin: auto">
          <el-row>
            <h3 style="display:inline">信息统计:</h3>
            <el-select v-model="companyvalue" clearable placeholder="请选择学校" style="width: 16%; margin-left:50px;margin-right: 25px">
              <el-option
                v-for="item in companies"
                :key="item.value"
                :label="item.lable"
                :value="item.value">
              </el-option>
            </el-select>
            <el-select v-model="titlevalue" clearable placeholder="请选择职称" style="width: 16%; margin-right: 25px">
              <el-option
                v-for="item in titles"
                :key="item.value"
                :label="item.lable"
                :value="item.value">
              </el-option>
            </el-select>
            <el-select v-model="majorvalue" clearable placeholder="请选择专业" style="width: 16%; margin-right: 25px">
              <el-option
                v-for="item in majors"
                :key="item.value"
                :label="item.lable"
                :value="item.value">
              </el-option>
            </el-select>
            <el-input
              style="width: 20%; margin-right: 25px"
              placeholder="统计结果"
              v-model="statisticResult"
              :disabled="true">
            </el-input>
            <el-button
                type="primary"
                @click="statistic()"
                style="margin-left: 10px"
                >统计</el-button
              >
          </el-row>
        </div>
      </el-footer>
    </el-container>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  data() {
    return {
      companies: [{
          value: '黄金糕',
          lable: '黄金糕'
        }, {
          value: '双皮奶',
          lable: '双皮奶'
        }, {
          value: '蚵仔煎',
          lable: '蚵仔煎'
        }, {
          value: '龙须面',
          lable: '龙须面'
        }, {
          value: '北京烤鸭',
          lable: '北京烤鸭'
        }],
        majors:[],
        titles:[],
        companyvalue: '清华大学',
        titlevalue: '教授',
        majorvalue: '计算机科学与技术',
        statisticResult: '',
      query: {
        name: "",
        company: "",
        title: "",
        direction: "",
      },
      currentpage: 0,
      nextpage: "1",
      prepage: "1",
      // 每页条数
      pagesize: 10,
      hasPreviousPage: false,
      hasNextPage: true,
      // 总页数
      pages: 0,
      // 数据总条数
      total: 0,
      tableData: [],
    };
  },
  mounted() {
    this.nextclick();
    this.staticQueryMajors();
    this.staticQueryTitles();
    this.staticQueryCompanies();
  },
  methods: {
    // 统计信息查询
    staticQueryMajors(){
      this.$http
        .get("/selectMajors")
        .then((ret) => {
          this.majors = ret.data.data;
        });
    },
    staticQueryCompanies(){
      this.$http
        .get("/selectCompanies")
        .then((ret) => {
          this.companies = ret.data.data;
        });
    },
    staticQueryTitles(){
      this.$http
        .get("/selectTitles")
        .then((ret) => {
          this.titles = ret.data.data;
        });
    },
    // 条件查询
    fuzzyQuery() {
      this.$http
        .post("/fuzzyQuery", {
          name: this.query.name,
          company: this.query.company,
          title: this.query.title,
          direction: this.query.direction,
        })
        .then((ret) => {
          this.tableData = ret.data.data;
        });
    },
    // 查询重置
    resetQuery() {
      this.query = {
        name: null,
        company: null,
        title: null,
        direction: null,
      };
      this.nextclick();
    },
    nextclick() {
      this.$http
        .get("/search/" + this.nextpage + "/" + this.pagesize)
        .then((ret) => {
          this.tableData = ret.data.data.list;
          this.currentpage = ret.data.data.pageNum;
          this.nextpage = ret.data.data.nextPage;
          this.prepage = ret.data.data.prePage;
          this.hasPreviousPage = ret.data.data.hasPreviousPage;
          this.hasNextPage = ret.data.data.hasNextPage;
          this.pages = ret.data.data.pages;
          this.total = ret.data.data.total;
        });
    },
    handleCurrentChange(val) {
      this.currentpage = val;
      this.$http
        .get("/search/" + this.currentpage + "/" + this.pagesize)
        .then((ret) => {
          this.tableData = ret.data.data.list;
          this.currentpage = ret.data.data.pageNum;
          this.nextpage = ret.data.data.nextPage;
          this.prepage = ret.data.data.prePage;
          this.hasPreviousPage = ret.data.data.hasPreviousPage;
          this.hasNextPage = ret.data.data.hasNextPage;
          this.pages = ret.data.data.pages;
          this.total = ret.data.data.total;
        });
    },
    handleSizeChange(val) {
      this.pagesize = val;
      this.nextpage = 1;
      this.$http
        .get("/search/" + this.nextpage + "/" + this.pagesize)
        .then((ret) => {
          this.tableData = ret.data.data.list;
          this.currentpage = ret.data.data.pageNum;
          this.nextpage = ret.data.data.nextPage;
          this.prepage = ret.data.data.prePage;
          this.hasPreviousPage = ret.data.data.hasPreviousPage;
          this.hasNextPage = ret.data.data.hasNextPage;
          this.pages = ret.data.data.pages;
          this.total = ret.data.data.total;
        });
    },
    preclick() {
      this.$http
        .get("/search/" + this.prepage + "/" + this.pagesize)
        .then((ret) => {
          this.tableData = ret.data.data.list;
          this.currentpage = ret.data.data.pageNum;
          this.nextpage = ret.data.data.nextPage;
          this.prepage = ret.data.data.prePage;
          this.hasPreviousPage = ret.data.data.hasPreviousPage;
          this.hasNextPage = ret.data.data.hasNextPage;
          this.pages = ret.data.data.pages;
          this.total = ret.data.data.total;
        });
    },
    statistic(){
       console.log(this.companyvalue+ this.titlevalue+this.majorvalue+"----");
       this.$http
        .post("/statistic", {
          company: this.companyvalue,
          title: this.titlevalue,
          major: this.majorvalue
        })
        .then((ret) => {
          this.statisticResult = ret.data.data;
        });
    }
  },
};
</script>

<style>
.el-main{
  background-color: #e9eef3;
  color: rgb(51, 51, 51);
  text-align: center;
  /* line-height: 200px; */
}
.el-footer {
  background-color: #B3C0D1;
  color: #333;
  text-align: center;
  line-height: 60px;
}
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
  width: 70%;
}
</style>
