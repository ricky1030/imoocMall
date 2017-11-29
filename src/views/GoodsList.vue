<template>
  <div>
    <nav-header>
    </nav-header>
    <nav-bread>
    </nav-bread>

    <div class="accessory-result-page accessory-page">
      <div class="container">
        <div class="filter-nav">
          <span class="sortby">Sort by:</span>
          <a href="javascript:void(0)" class="default cur">Default</a>
          <a @click="sortGoods" href="javascript:;" class="price">Price
            <svg class="icon icon-arrow-short">
              <use xlink:href="#icon-arrow-short"></use>
            </svg>
          </a>
          <a href="javascript:void(0)" class="filterby stopPop">Filter by</a>
        </div>
        <div class="accessory-result">
          <!-- filter -->
          <div class="filter stopPop" id="filter">
            <dl class="filter-price">
              <dt>Price:</dt>
              <dd>
                <a href="javascript:void(0)">All</a>
              </dd>
              <dd>
                <a href="javascript:void(0)">0 - 100</a>
              </dd>
              <dd>
                <a href="javascript:void(0)">100 - 500</a>
              </dd>
              <dd>
                <a href="javascript:void(0)">500 - 1000</a>
              </dd>
              <dd>
                <a href="javascript:void(0)">1000 - 2000</a>
              </dd>
            </dl>
          </div>

          <!-- search result accessories list -->
          <div class="accessory-list-wrap">
            <div class="accessory-list col-4">
              <ul>
                <li v-for="item in goodsList">
                  <div class="pic">
                    <a href="#">
                      <img :src="'static/'+item.productImage" alt="">

                    </a>
                  </div>
                  <div class="main">
                    <div class="name">{{item.productName}}</div>

                    <div class="price">{{item.salePrice}}</div>

                    <div class="btn-area">
                      <a href="javascript:;" class="btn btn--m">加入购物车</a>
                    </div>
                  </div>
                </li>
              </ul>
              <div v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="10">
                加載中
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>


    <nav-footer>
    </nav-Footer>

  </div>
</template>


<script>
import "./../assets/css/base.css";
import "./../assets/css/checkout.css";
import "./../assets/css/login.css";
import "./../assets/css/product.css";
import NavHeader from "../components/Header";
import NavFooter from "../components/Footer.vue";
import NavBread from "../components/Bread.vue";
import axios from "axios";

export default {
  data() {
    return {
      goodsList: [],
      sortFlag: true,
      page: 1,
      pageSize: 8,
      busy: true
    };
  },
  mounted() {
    this.getGoodsList();
  },
  components: {
    NavHeader: NavHeader,
    NavFooter: NavFooter,
    NavBread: NavBread
  },
  methods: {
    getGoodsList(flag) {
      var param = {
        page: this.page,
        pageSize: this.pageSize,
        sort: this.sortFlag ? 1 : -1
      };
      axios
        .get("/goods", {
          params: param
        })
        .then(response => {
          let res = response.data;
          if (res.status == "0") {
            if (flag) {
              this.goodsList = this.goodsList.concat(res.result.list);

              if (res.result.count == 0) {
                this.busy = true;
              } else {
                this.busy = false;
              }
            } else {
              this.goodsList = res.result.list;
              this.busy = false;
            }
          } else {
            this.goodsList = [];
          }
        });
    },
    sortGoods() {
      this.sortFlag = !this.sortFlag;
      this.page = 1;
      this.getGoodsList();
    },
    loadMore() {
      this.busy = true;
      setTimeout(() => {
        this.page++;
        this.getGoodsList(true);
      }, 500);
    }
  }
};
</script>

<style scoped>

</style>
