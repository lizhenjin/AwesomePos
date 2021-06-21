<template>
  <el-row>
    <el-col :span="7" class="pos_order">
      <div class="pos_order_content">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width: 100%">
              <el-table-column prop="goodsName" label="商品名称">
              </el-table-column>
              <el-table-column prop="count" label="数量"> </el-table-column>
              <el-table-column prop="price" label="金额"> </el-table-column>
              <el-table-column label="操作">
                <template slot-scope="scope">
                  <el-button
                    @click="handleDeleteGoods(scope.row)"
                    type="text"
                    size="small"
                    >删除</el-button
                  >
                  <el-button
                    type="text"
                    size="small"
                    @click="addOrderLisr(scope.row)"
                    >增加</el-button
                  >
                </template>
              </el-table-column>
            </el-table>
            <el-row style="margin-top: 20px" class="calculate">
              <span>数量:</span><span>{{ shopingCarNum }}</span
              ><span style="margin-left: 50px">总价:</span
              ><span>{{ shopingCarMoney }}</span>
            </el-row>
            <el-row style="margin-top: 20px">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="deleteAllGoods">删除</el-button>
              <el-button type="success" @click="toOrder">结账</el-button>
            </el-row>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </div>
    </el-col>
    <el-col :span="17" class="pos_goods">
      <div class="pos_goods_content">
        <h3>常用商品</h3>
        <div class="goods_show">
          <el-button
            v-for="item in oftenGoods"
            :key="item.goodsId"
            effect="plain"
            class="oftenGoods"
            @click="addOrderLisr(item)"
          >
            <span>{{ item.goodsName }} </span><span>￥{{ item.price }}</span>
          </el-button>
        </div>
        <div>
          <el-tabs>
            <el-tab-pane label="汉堡" class="details_goods">
              <el-button
                v-for="item in type0Goods"
                :key="item.goodsId"
                effect="plain"
                class="oftenGoods"
                @click="addOrderLisr(item)"
              >
                <span>{{ item.goodsName }} </span
                ><span>￥{{ item.price }}</span>
              </el-button>
            </el-tab-pane>
            <el-tab-pane label="小食" class="details_goods"
              ><el-button
                v-for="item in type1Goods"
                :key="item.goodsId"
                effect="plain"
                class="oftenGoods"
                @click="addOrderLisr(item)"
              >
                <span>{{ item.goodsName }} </span
                ><span>￥{{ item.price }}</span>
              </el-button></el-tab-pane
            >
            <el-tab-pane label="饮料" class="details_goods"
              ><el-button
                v-for="item in type2Goods"
                :key="item.goodsId"
                effect="plain"
                class="oftenGoods"
                @click="addOrderLisr(item)"
              >
                <span>{{ item.goodsName }} </span
                ><span>￥{{ item.price }}</span>
              </el-button></el-tab-pane
            >
            <el-tab-pane label="套餐" class="details_goods"
              ><el-button
                v-for="item in type3Goods"
                :key="item.goodsId"
                effect="plain"
                class="oftenGoods"
                @click="addOrderLisr(item)"
              >
                <span>{{ item.goodsName }} </span
                ><span>￥{{ item.price }}</span>
              </el-button></el-tab-pane
            >
          </el-tabs>
        </div>
      </div>
    </el-col>
  </el-row>
</template>

<script>
import axios from "axios";
import _ from "lodash";
export default {
  name: "pos",
  data() {
    return {
      //购物车数量
      shopingCarNum: 0,
      //购物车金额
      shopingCarMoney: 0,
      //常用商品
      oftenGoods: [],
      //购物车商品
      tableData: [],
      //汉堡
      type0Goods: [],
      //小食
      type1Goods: [],
      //饮料
      type2Goods: [],
      //套餐
      type3Goods: [],
    };
  },
  created() {
    axios
      .get(
        "https://www.fastmock.site/mock/6c12811163be28570b91f7380e49cf0c/awesomepos/oftenGoods"
      )
      .then((response) => {
        this.oftenGoods = response.data.rspBody.oftenGoods;
      })
      .catch((error) => {
        alert("网络错误，不能访问");
      });
    axios
      .get(
        "https://www.fastmock.site/mock/6c12811163be28570b91f7380e49cf0c/awesomepos/tableData"
      )
      .then((response) => {
        this.type0Goods = response.data.rspBody.tableData.filter((item) => {
          return item.type === 1;
        });
        this.type1Goods = response.data.rspBody.tableData.filter((item) => {
          return item.type === 2;
        });
        this.type2Goods = response.data.rspBody.tableData.filter((item) => {
          return item.type === 3;
        });
        this.type3Goods = response.data.rspBody.tableData.filter((item) => {
          return item.type === 4;
        });
      })
      .catch((error) => {
        alert("网络错误，不能访问");
      });
  },
  methods: {
    handleClick(row) {
      console.log(row);
    },
    //常用商品添加
    addOrderLisr(goods) {
      let isInShoppingCar = false;
      isInShoppingCar = this.tableData.some((item) => {
        return item.goodsId === goods.goodsId;
      });
      if (isInShoppingCar) {
        this.tableData = this.tableData.map((item) => {
          if (item.goodsId === goods.goodsId) {
            item.count += 1;
          }
          return item;
        });
      } else {
        goods.count = 1;
        this.tableData.push(goods);
      }
      //算数量
      this.shopingCarNum = this.tableData.reduce((prev, cur) => {
        return prev + cur.count;
      }, 0);
      //算总价
      this.shopingCarMoney = this.tableData.reduce((prev, cur) => {
        return prev + cur.count * cur.price;
      }, 0);
    },
    //删除单个商品
    handleDeleteGoods(row) {
      this.tableData = this.tableData.filter((item) => {
        return item.goodsId !== row.goodsId;
      });
      //算数量
      this.shopingCarNum = this.tableData.reduce((prev, cur) => {
        return prev + cur.count;
      }, 0);
      //算总价
      this.shopingCarMoney = this.tableData.reduce((prev, cur) => {
        return prev + cur.count * cur.price;
      }, 0);
    },
    //删除所有商品
    deleteAllGoods() {
      this.tableData = [];
      //购物车数量
      this.shopingCarNum = 0;
      //购物车金额
      this.shopingCarMoney = 0; 
    },
    //结账
    toOrder() {
      if (!_.isEmpty(this.tableData)) {
         this.$message({
          message: '结账成功哦！',
          type: 'success'
        });
        this.tableData = [];
      //购物车数量
      this.shopingCarNum = 0;
      //购物车金额
      this.shopingCarMoney = 0;
      } else {
        this.$message.error("购物车不能为空哦！");
      }
    },
  },
};
</script>

<style lang="less" scoped>
.pos_order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
  &_content {
    height: 100vh;
    padding: 10px;
  }
}
.pos_order_content {
  .calculate {
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }
}
.pos_goods {
  &_content {
    height: 100vh;
    padding: 10px;
    .goods_show {
      height: 200px;
      background-color: #f9fafc;
      display: flex;
      flex-flow: wrap;
      justify-content: flex-start;
      align-items: center;
      .oftenGoods {
        width: 150px;
        height: 50px;
      }
      .oftenGoods:first-of-type {
        margin-left: 10px;
      }
    }
    .details_goods {
      display: flex;
      flex-flow: wrap;
      .oftenGoods {
        width: 150px;
        height: 50px;
        margin-top: 10px;
      }
      .oftenGoods:first-of-type {
        margin-left: 10px;
      }
    }
  }
}
</style>