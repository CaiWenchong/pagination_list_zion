<template>
  <div class="paginationClass">
    <div class="list-container">12</div>

    <div class="pagination">
      <el-pagination
        background
        layout="prev, pager, next"
        :total="pagination.count"
        :page-size="pagination.pageSize"
        :pager-count="4"
        :default-current-page="1"
        v-model:current-page="pagination.currentPage"
        class="mt-4"
        @current-change="handleCurrentChange"
      />
    </div>
  </div>
</template>

<script>
import mdapi from "/src/zion-config/request_outer.js";

// 引入
import { messageControl } from "/src/components/message.js";

export default {
  name: "pagination",
  props: [
    "globalData",
    "url",
    "actionflow_id",
    "company_id",
    "redirectUrl",
    "redirectUrlParams",
  ],

  data() {
    return {
      pagination: {
        count: 75,
        currentPage: 1,
        limit: 10,
        pageSize: 20,
      },

      message: null,
    };
  },
  mounted() {
    this.message = new messageControl();

    console.log("props:", this.$props);



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

    if (!this.$props.company_id) {
      this.message.message({ type: "error", content: "company_id不能为空" });
      console.error("company_id不能为空");
    }

    this.getList();

  },

  methods: {
    //分页页码改变时
    handleCurrentChange(val) {
      console.log(`current page: ${val}`);
    },

    getList() {
      let params = {
        model: "agreement",
        field_string: `id title created_at content`,
        where: {
          title: {
            _eq: "C2C个人支付风险提示",
          },
        },
      };
      mdapi.query(params).then((res) => {
        console.log("res:", res);
      });
    },

    // 保存服务器
    // saveImageInfo() {
    //   let b = this.canvas.toDataURL();

    //   this.local_umedia(b).then((imageRes) => {
    //     console.log("imageRes:", imageRes);
    //     if (imageRes.id) {
    //       this.insertSignPictureOne(imageRes.id).then((res) => {
    //         if (res.id) {
    //           this.$props.globalData.sign_image_id = imageRes.id;
    //           this.message.message({
    //             type: "success",
    //             content: "提交成功，请勿重复提交",
    //           });
    //         }
    //       });
    //     }
    //   });
    //   console.log("props:", this.$props);
    // },

    // insertSignPictureOne(signImageId) {
    //   // 初始化配置
    //   if (!this.$props.url && !this.$props.actionflow_id) {
    //     this.message.message({
    //       type: "error",
    //       content: "url或者actionflow_id为空",
    //     });
    //     console.error("提交失败:url或者actionflow_id为空");
    //     return;
    //   }

    //   if (!this.$props.table_name || !this.$props.table_fields) {
    //     this.message.message({
    //       type: "error",
    //       content: "提交失败,table_name和table_fields为空",
    //     });
    //     console.error("提交失败:table_name和table_fields为空");
    //     return;
    //   }

    //   if (!this.$props.image_field && !this.$props.image_id_field) {
    //     this.message.message({
    //       type: "error",
    //       content: "提交失败,image_field和image_id_field为空",
    //     });
    //     console.error("提交失败,image_field和image_id_field为空");
    //     return;
    //   }
    //   let image = this.$props.image_field;

    //   let imageId = this.$props.image_id_field;

    //   let obj = {};
    //   obj[image] = signImageId;
    //   obj[imageId] = signImageId;

    //   let params = {
    //     operation: `insert_${this.$props.table_name}_one`,
    //     object: obj,
    //     isloading: false,
    //     field_string: `${this.$props.table_fields}`,
    //   };
    //   return this.mutation(params);
    // },
  },
};
</script>
<style scoped>
.paginationClass {
  position: fixed;
  width: 100%;

  bottom: 0px;
  text-align: center;
}

.pagination {
  padding: 10px 0px;
  width: 100%;
  display: flex;
  justify-content: center;
}
</style>
