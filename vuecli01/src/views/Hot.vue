<template>
  <section class="hot">
    <section class="img">
      <img
        src="https://s3.music.126.net/mobile-new/img/hot_music_bg_2x.jpg?f01a252389c26bcf016816242eaa6aee="
        alt=""
      />
      <div class="img-1">
        <p>云音乐</p>
        <p>热歌榜</p>
      </div>
    </section>
    <ul>
      <SongList
        v-for="(item, index) in hotSong"
        :key="item.id"
        :item="item"
        @change-current-song="
          $emit('change-current-song', item);
          $emit('change-current-play-list', hotSong);
        "
        :currentSongId="currentSongId"
        :playing="playing"
        :class="{ lt3: index < 3 }"
        >{{ index + 1 }}</SongList
      >
    </ul>
  </section>
</template>

<script>
// splice
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
      hotSong: null,
    };
  },
  // 组件创建完成后获取数据
  created(){
    this.getHotSongsData();
  },
  methods: {
    getHotSongsData: function () {
      this.axios
        .get("http://apis.netstart.cn/music/playlist/detail?id=3778678"
        )
        .then((res) => {
          console.log(res);//结果 分析结果里面的data的数据结构
          this.hotSong = res.data.playlist.tracks;
        });
    },
  },
};
</script>

<style lang="less" scoped>
.hot {
  .img {
    position: relative;
    vertical-align: bottom;
    .img-1 {
      width: 142px;
      height: 67px;
      position: absolute;
      top: 20px;
      left: 20px;
      p:first-child {
        color: #eddedb;
      }
      p:last-child {
        color: #fff;
        font-size: 34px;
      }
    }
  }
}
</style>
