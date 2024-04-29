<!--
 * @Email: rumosky@163.com
 * @Author: rumosky
 * @Github: https://github.com/rumosky
 * @Date: 2019-07-19 09:32:08
 * @Description: 封装分页组件
 -->
<template>
  <div class="page-wrap" v-if="total>0">
    <ul>
      <li
        class="li-page"
        :class="{
          disable: !prePage,
        }"
        @click="goPrePage"
        style="font-weight: 700"
      >
        <i class="el-icon-arrow-left"></i>
      </li>

      <li
        v-for="(i, index) in showPageBtn"
        :key="index"
        :class="{
          active: i === currentPage,
          pointer: i,
          hover: i && i !== currentPage,
        }"
        @click="pageOffset(i)"
      >
        <span class="pageNo" v-if="i">{{ i }}</span>
        <span class="pageNo" v-else>···</span>
      </li>

      <li
        class="li-page"
        :class="{
          disable: !nextPage,
        }"
        @click="goNextPage"
      >
        <i class="el-icon-arrow-right"></i>
      </li>
    </ul>
  </div>
</template>

<script>
import mdapi from "/src/zion-config/request_outer.js";
// 引入
import { messageControl } from "/src/components/message.js";

export default {
  name: "pagination",
  props: ["globalData", "url", "actionflow_id", "table_name", "where", "limit"],
  mounted() {
    console.log("props:", this.$props);

    this.message = new messageControl();

    // 初始化配置
    if (!this.$props.url && !this.$props.actionflow_id) {
      this.message.message({
        type: "error",
        content: "url或者actionflow_id为空",
      });
      console.error("url或者actionflow_id为空");
    } else {
      mdapi.init(this.$props.url, this.$props.actionflow_id);
    }

    if (!this.$props.table_name) {
      this.message.message({ type: "error", content: "table_name不能为空" });
      console.error("table_name不能为空");
    }

    if (!this.$props.limit) {
      this.message.message({ type: "error", content: "limit" });
      console.error("limit");
    }

    this.queryTableDataCount(this.table_name, this.where);
  },
  data: () => ({
    offset: 0,
    total: 0,
    message: null,
  }),
  computed: {
    prePage() {
      return this.offset !== 0 && this.total;
    },
    nextPage() {
      return this.offset + this.limit < this.total && this.total;
    },
    totalPage() {
      return Math.ceil(this.total / this.limit);
    },
    currentPage() {
      return Math.ceil(this.offset / this.limit) + 1;
    },
    showPageBtn() {
      const pagetotal = this.totalPage;
      const index = this.currentPage;
      if (pagetotal <= 5) return [...new Array(pagetotal)].map((v, i) => i + 1);
      if (index <= 2) return [1, 2, 3, 0, pagetotal];
      if (index >= pagetotal - 1)
        return [1, 0, pagetotal - 2, pagetotal - 1, pagetotal];

      if (index === 3) return [1, 2, 3, 4, 0, pagetotal];
      if (index === pagetotal - 2)
        return [1, 0, pagetotal - 3, pagetotal - 2, pagetotal - 1, pagetotal];
      return [1, 0, index - 1, index, index + 1, 0, pagetotal];
    },
  },
  methods: {
    //查询表格数据总数
    queryTableDataCount(table_name, whereTxt) {
      let where = eval(`({${whereTxt}})`);

      let params = {
        model: `${table_name}_aggregate`,
        field_string: "aggregate {count}",
        where,
      };

      console.log("rs", params);

      mdapi.query(params).then((res) => {
        this.total=res.aggregate.count
      });


    },

    pageOffset(i) {
      if (i === 0 || i === this.currentPage) return;
      this.offset = (i - 1) * this.limit;
      this.changePage();
    },
    goPrePage() {
      if (!this.prePage) {
        return;
      }
      this.offset -= this.limit;
      this.changePage();
    },
    goNextPage() {
      if (!this.nextPage) {
        return;
      }
      this.offset += this.limit;
      this.changePage();
    },
    //页码改变时
    changePage() {
      this.$props.globalData.currentPage=this.currentPage
      console.log("props:", this.$props);
    },
  },
};
</script>

<style scoped>
@import url("//unpkg.com/element-ui@2.13.2/lib/theme-chalk/index.css");

* {
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
.li-page {
  /* line-height: 1.5; */
  cursor: pointer;
  color: #000000;
  background-color: #f4f4f5;
}

.active {
  border-color: #409eff;
  background-color: #409eff;
}

.pointer {
  cursor: pointer;
}

.hover {
  color: #e01818;
  background-color: #f4f4f5;
}

.active a {
  color: #c4acac;
}

.page-wrap {
  font-size: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.page-wrap ul {
  display: inline-block;
  list-style: none;
  overflow: hidden;
  padding-inline-start: 0px;
}

.page-wrap li {
  display: inline-block;
  text-align: center;
  font-weight: 550;
  min-width: 25px;

  /* color: #c2c4cc; */
  padding: 5px 5px;
  margin: 0 5px;
  border-radius: 5px;
  user-select: none;
  border: 1px solid transparent;
}

.pageNo {
  /* color: #606266; */
  /* min-width: 30px; */
  color: #303133;
}

.disable {
  color: #c0c4cc;
}
</style>
