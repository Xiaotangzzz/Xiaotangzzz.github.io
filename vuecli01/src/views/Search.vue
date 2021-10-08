<template>
  <div class="search" @scroll="scroll">
    <div class="inp">
      <input
        type="text"
        placeholder="搜索歌曲、歌手、专辑"
        v-model.trim="value"
        @keyup.enter="value && (searching = true)"
        @focus="inputing = true"
        @blur="inputing = false"
      />
    </div>
    <section class="hots" v-if="!value && !searching">
      <h5>热门搜索</h5>
      <ul>
        <li
          v-for="hot in hots"
          :key="hot.first"
          @click="
            searching = true;
            value = hot.first;
          "
        >
          {{ hot.first }}
        </li>
      </ul>
      <ol v-if="history.length !=0">
        <li
          v-for="(h,index) in history"
          :key="h"
          
        >
        
          <img src="../image/histy.png" alt="" />
          <span @click="
            searching = true;
            value = h;
          ">{{ h }}</span>
          <span><img class="xx" src="../image/地图找房_交叉.png" alt="" @click="deleteData(index)"/></span>
        </li>
      </ol>
    </section>
    <section class="suggests" v-if="value && !searching">
      <h5>搜索"{{ value }}"</h5>
      <ul>
        <li v-for="(item, index) in suggests" :key="index">
          {{ item.keyword }}
        </li>
      </ul>
    </section>

    <section class="suggests" v-if="searching">
      <h5>搜索结果</h5>
      <ul>
        <li v-for="(item, index) in searchResults" :key="index" >
          {{ item.name }}
        </li>
      </ul>
      <p v-if="!hasMore">没有更多了</p>
    </section>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      hots: [],
      suggests: [],
      //   搜索结果
      searchResults: [],
      value: "",
      //   搜索框输入状态
      searching: false,
      inputing: false,
      page: 0,
      hasMore: false,
      history: JSON.parse(window.localStorage.getItem("history")) || [],
      // history:[],
    };
  },
  methods: {
    scroll: function (event) {
      // console.log(event);
      if (this.hasMore) {
        if (
          event.target.offsetHeight + event.target.scrollTop ===
          event.target.scrollHeight
        ) {
          // console.log("触底");
          this.getSearch();
        }
      } else {
        // console.log("没有更多了");
      }
    },
    getSearch: function () {
      this.axios
        .get("http://apis.netstart.cn/music/search", {
          params: {
            keywords: this.value,
            limit: 30,
            offset: this.page * 30,
          },
        })
        .then((res) => {
          this.searchResults.push(...res.data.result.songs);
          this.page++;
          this.hasMore = res.data.result.hasMore;
        });
      //搜索记录
      // this.history.push(this.value);
      //搜索记录去从
      this.history = [...new Set([...this.history, this.value])];
      window.localStorage.setItem("history", JSON.stringify(this.history));
    },

    //删除
    deleteData(index){
      // console.log(index);
      // console.log(this.history);
       this.history.splice(index,1);
      window.localStorage.setItem("history", JSON.stringify(this.history));
    } 
  },
  created: function () {
    this.axios.get("http://apis.netstart.cn/music/search/hot").then((res) => {
      this.hots = res.data.result.hots;
    });
  },
  watch: {
    value: function (n) {
      // 监听搜索状态，只要搜索框状态发生改变，就把搜索结果干掉
      if (this.inputing) {
        this.searching = false;
      }
      if (n && !this.searching) {
        this.axios
          .get("http://apis.netstart.cn/music/search/suggest", {
            params: {
              keywords: n,
              type: "mobile",
            },
          })
          .then((res) => {
            this.suggests = res.data.result.allMatch;
          });
      } else {
        this.suggests = [];
      }
    },
    // 监听搜索状态，只要搜索框状态发生改变，就把搜索结果干掉，重新显示搜索建议
    searching: function (n) {
      if (n && this.value) {
        this.getSearch();
      } else {
        this.searchResults = [];
      }
    },
  },
};
</script>

<style lang="less" scoped>
.search {
  position: absolute;
  top: 42px;
  left: 0;
  overflow: auto;
  width: 100vw;
  height: 92vh;

  .suggests {
    h5 {
      color: #507daf;
    }
  }
  .inp {
    box-shadow: 0 -1px 0 0px rgb(231, 231, 231) inset;
    input {
      width: 340px;
      // height: 30px;
      padding: 6px 0 6px 10px;
      border: 1px solid #ebecec;
      border-radius: 20px;
      background: #ebecec;
      margin: 15px 10px;
      // 去掉边框高亮
      outline: none;
      
    }
  }
  .hots {
    margin: 15px 10px;
    ol {
      list-style: none;
      margin-top: 15px;
      li {
        // background: skyblue;
        height: 46px;
        line-height: 46px;
        margin: 0 5px;
        box-shadow: 0 1px 0 0px rgb(231, 231, 231);
        display: flex;
        align-items: center;
        position: relative;
        top: 0;
        left: 0;
        img {
          width: 18px;
          height: 18px;
          margin: 0 10px;
        }
        .xx {
          position: absolute;
          top: 13px;
          right: 5px;
        }
      }
    }

    h5 {
      font-size: 8px;
    }
    ul {
      overflow: hidden;
      margin-top: 6px;
      li {
        float: left;
        border: 1px solid rgba(202, 196, 196, 0.555);
        border-radius: 20px;
        padding: 4px 12px;
        margin: 6px 4px;
      }
    }
  }
}
</style>