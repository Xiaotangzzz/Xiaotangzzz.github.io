<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Recommend>编辑推荐</Recommend>
    <ul class="home-cardlist">
      <CardList
        v-for="item in personalizeds"
        :key="item.id"
        :item="item"
      ></CardList>
    </ul>
    <!-- <ul>
      <li
        v-for="item in personalizeds"
        :key="item.id"
        @click="$router.push('/playlist?id=' + item.id)"
      >
        {{ item.name }}
      </li>
    </ul> -->
    <Recommend>最新音乐</Recommend>
    <ul class="home-SongList">
      <SongList
        v-for="item in newsongs"
        :key="item.id"
        :item="item"
        @change-current-song="
          $emit('change-current-song', $event);
          $emit('change-current-play-list', newsongs);
        "
        :currentSongId="currentSongId"
        :playing="playing"
      ></SongList>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src
import Recommend from "@/components/Recommend.vue";
import CardList from "@/components/CardList.vue";
import SongList from "@/components/SongList.vue";

export default {
  name: "Home",
  components: {
    Recommend,
    CardList,
    SongList,
  },
  props: {
    currentSongId: {
      type: Number,
    },
    playing: Boolean,
  },
  data: function () {
    return {
      personalizeds: [],
      newsongs: [],
    };
  },
  created: function () {
    //发起ajax请求
    this.axios
      .get("http://apis.netstart.cn/music/personalized?limit=6")
      .then((res) => {
        // console.log(this,res);
        this.personalizeds = res.data.result;
      });

    this.axios
      .get("http://apis.netstart.cn/music/personalized/newsong")
      .then((res) => {
        // console.log(this,res);
        this.newsongs = res.data.result;
      });
  },
};
</script>

<style lang="less">
.home-cardlist {
  display: flex;
  flex-wrap: wrap;
}
</style>
