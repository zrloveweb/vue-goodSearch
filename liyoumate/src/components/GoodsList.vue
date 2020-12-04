<template>
  <div>
    <el-row type="flex" justify="center">
      <el-col :span="12 ">
        <el-input v-model="input" placeholder="请输入需要搜索的内容" @keyup.enter.native="enter"></el-input>
      </el-col>
      <el-button type="success" class="rightMove">点击搜索</el-button>
    </el-row>
    <el-row type="flex">

      <el-col :span="5" v-for="(item ,index) in items" :key="index">
        <div class="grid-content"><img :src="item.img"></img></div>
        <div v-html="item.name"></div>
      </el-col>
    </el-row>

    <div class="block">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page.sync="currentPage"
        :page-size="pageSize"
        layout="prev, pager, next, jumper"
        :total="total">
      </el-pagination>
      <div>
        <div id="example">
          {{ reversedMessage }}
          <el-input v-model="message" placeholder="请输入需要搜索的内容" @keyup.enter.native="enter"></el-input>

        </div>


      </div>
    </div>
  </div>

</template>

<script>
  export default {
    name: "GoodsList",
    methods: {
      enter(val) {
        //如果重新搜索别的内容 当前页设置为1
        if (val.key == 'Enter') {
          this.currentPage = 1;
        }
        this.items = [];
        this.$fetchGet(`http://localhost:8085/searchGoods/${this.input}/${this.currentPage}/${this.pageSize}`, null).then(data => {
          data.data.forEach(item => {
            this.items.push(item)
          })
          this.total = data.total;
        });
      },
      handleSizeChange(val) {
      },
      handleCurrentChange(val) {
        this.enter(val);
      },
      handleClick() {
        alert('button click');
      }
    },
    data() {
      return {
        items: [],
        input: '',
        currentPage: 1,
        pageSize: 10,
        total: 0,
        message: 'Hello'
      }
    },
    computed: {
      // 计算属性的 getter
      reversedMessage :{
        // getter
        get: function () {
          this.input='set'
          return this.message
        },
        // setter
        set: function (newValue) {
         this.input='set'
        }
      }

    }
  }

</script>

<style scoped>
  .el-row--flex {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    flex-wrap: wrap;
  }

  .el-col-5 {
    width: 25%;
  }

  .rightMove {
    margin-left: 20px;
  }


  .el-dropdown {
    vertical-align: top;
  }

  .el-dropdown + .el-dropdown {
    margin-left: 15px;
  }

  .el-icon-arrow-down {
    font-size: 12px;
  }
</style>
