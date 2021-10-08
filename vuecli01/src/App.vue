<template>
  <div id="app">
    <ul id="nav" v-if="$route.meta.show">
      <li><router-link to="/">推荐音乐</router-link></li>
      <li><router-link to="/hot">热歌榜</router-link></li>
      <li><router-link to="/search">搜索</router-link></li>
    </ul>
    <section class="routes">
      <!-- <transition
    name="custom-classes-transition"
    enter-active-class="animate__animated animate__slideInRight"
    leave-active-class="animate__animated animate__slideOutLeft"
    mode="out-in" -->
      <router-view
        @change-current-song="changeCurrentSong"
        @change-current-play-list="changeCurrentPalyList"
        :currentSongId="currentSong ? currentSong.id : null"
        :playing="playing"
      />
      <!-- </transition> -->
    </section>
    <audio
      ref="audio"
      :src="currentSongUrl"
      controls
      style="height: 0"
      autoplay
      @playing="playing = true"
      @pause="playing = false"
      @timeupdate="timeupdate"
      @durationchange="durationchange"
    ></audio>
    <Play
      v-if="currentSong"
      :currentSong="currentSong"
      :playing="playing"
      @toggle-playing-state="togglePlayingState"
      :currentTime="currentTime"
      :duration="duration"
      :currentPalyList="currentPalyList"
      @change-current-song="changeCurrentSong"
      @next-song="nextSong"
      @prev-song="prevSong"
    ></Play>
  </div>
</template>
<script>
import Play from "@/components/Play.vue";
export default {
  components: {
    Play,
  },
  data: function () {
    return {
      currentSong: null,
      currentPalyList: [],
      playing: false,
      currentTime: 0,
      duration: 0,
    };
  },
  computed: {
    currentSongUrl: function () {
      if (this.currentSong) {
        return ` https://music.163.com/song/media/outer/url?id=${this.currentSong.id}.mp3`;
      } else {
        return null;
      }
    },
  },
  methods: {
    changeCurrentSong: function (song) {
      // console.log(song);
      this.currentSong = song;
    },
    changeCurrentPalyList: function (list) {
      this.currentPalyList = list;
    },
    togglePlayingState: function () {
      if (this.playing) {
        this.$refs.audio.pause();
      } else {
        this.$refs.audio.play();
      }
    },
    timeupdate: function (event) {
      this.currentTime = event.target.currentTime;
    },
    durationchange: function (event) {
      this.duration = event.target.duration;
    },
    nextSong:function(){
      // console.log(123);
      var index = this.currentPalyList.findIndex((item)=>{
        return item.id === this.currentSong.id;
      });
      index++;
      index = index >= this.currentPalyList.length - 1 ? 0 : index;
      index = index < 0 ? this.currentPalyList.length - 1 : index;
      // console.log(index);
      this.changeCurrentSong(this.currentPalyList[index]);
    },
    prevSong:function(){
      // console.log(456);
      var index = this.currentPalyList.findIndex((item)=>{
        return item.id === this.currentSong.id;
      });
      index--;
      index = index >= this.currentPalyList.length - 1 ? 0 : index;
      index = index <= 0 ? this.currentPalyList.length - 1 : index;
      // console.log(index);
      this.changeCurrentSong(this.currentPalyList[index]);
    }
  },
};
</script>

<style lang="less">
@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
.animate__animated {
  animation-duration: 0.3s;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  color: #2c3e50;
  audio {
    margin-bottom: 40px;
  }
}

#nav {
  display: flex;
  // height: 42px;
  // border-bottom: 1px solid rgba(235, 232, 232, 0.685);
  box-shadow: 0 -1px 0 0px rgb(231, 231, 231) inset;
  li {
    flex: 1;
    text-align: center;

    a {
      text-decoration: none;
      // font-weight: bold;
      color: #2c3e50;
      line-height: 40px;
      display: inline-block;
      font-size: 15px;
      padding: 0 5px; //为了让下面的想比文字长出一点

      &.router-link-exact-active {
        color: #d43c33;
        border-bottom: 2px solid #d43c33;
      }
    }
  }
}
</style>
