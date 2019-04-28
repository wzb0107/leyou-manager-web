<template>
  <div>
    <v-card>
      <v-card-title>
        <v-text-field label="请输入关键字" v-model="search" append-icon="search" hide-details></v-text-field>
        <v-spacer/>
        <v-btn color="primary">
          添加
        </v-btn>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="brands"
        :pagination.sync="pagination"
        :total-items="totalBrands"
        :loading="loading"
        class="elevation-1"
      >
        <template slot="items" slot-scope="props">
          <td class="text-xs-center">{{ props.item.id }}</td>
          <td class="text-xs-center">{{ props.item.name }}</td>
          <td class="text-xs-center"><img v-if="props.item.image" :src="props.item.image" width="130" height="40"/></td>
          <td class="text-xs-center">{{ props.item.letter }}</td>

          <!--删除 修改按钮-->
          <td class="text-xs-center">
            <v-icon small class="mr-2" @click="editItem(props.item)">
              edit
            </v-icon>
            <v-icon small @click="deleteItem(props.item)">
              delete
            </v-icon>
          </td>

        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
  export default {
    name: "myBrand",
    data() {
      return {
        totalBrands: 0, // 总条数
        brands: [], // 当前页品牌数据
        loading: true, // 是否在加载中
        pagination: {}, // 分页信息
        search: "",
        headers: [ // 头信息
          {text: 'ID', align: 'center', value: 'id'},
          {text: '品牌', align: 'center', value: 'name', sortable: false},
          {text: 'LOGO', align: 'center', value: 'image', sortable: false},
          {text: '首字母', align: 'center', value: 'letter'},
          {text: '操作', align: 'center', value: 'id', sortable: false},
        ]
      }
    },
    methods: {
      // 从服务端加载数据的函数
      getDataFromServer() {
        this.loading = true; // 加载数据
        this.$http.get("/item/brand/page", {
          params: {
            page: this.pagination.page,
            rows: this.pagination.rowsPerPage,
            sortBy: this.pagination.sortBy,
            desc: this.pagination.descending,
            key: this.search
          }
        }).then(resp => {
            this.totalBrands = resp.data.total;
            this.brands = resp.data.items;
            this.loading = false;
        });
      }
    },
    // 渲染后执行
    mounted() {
      this.getDataFromServer() // 调用数据初始化函数
    }
  }
</script>

<!-- scoped:当前样式只作用于当前组件的节点 -->
<style scoped>

</style>
