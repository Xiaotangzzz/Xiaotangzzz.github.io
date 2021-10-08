<template>
  <!-- 因为ajax是异步的  初始化的时候数据还没拿到   所以一开始需要判断只有detail存在的时候才开始 -->
  <div v-if="detail">
    <!-- <h3>{{ $route.query.id }}</h3> -->
    <ul>
      <!-- <li v-for="item in detail.tracks" :key="item.id">{{ item.name }}</li> -->
      <SongList
        v-for="(item, index) in detail.tracks"
        :key="item.id"
        :item="item"
        @change-current-song="
          $emit('change-current-song', item);
          $emit('change-current-play-list', detail.tracks);
        "
        :currentSongId="currentSongId"
        :playing="playing"
        :class="{ lt3: index < 3 }"
        >{{ index + 1 }}</SongList
      >
    </ul>
  </div>
</template>

<script>
import SongList from "@/components/SongList.vue";
export default {
  components: {
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
      detail: null,
    };
  },
  created: function () {
    this.axios
      .get("http://apis.netstart.cn/music/playlist/detail?id=3778678")
      .then((res) => {
        this.hotsong = res.dt.playlist.tracks;
      });
  },
};
</script>

<style lang="less" scoped>
</style>
