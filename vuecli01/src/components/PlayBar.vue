<template>
  <!-- <footer class="playbar" :class="{ playing: playing }"> -->
  <section
    class="playbar"
    :class="{ playing: playing }"
    @click="$emit('toggle-show-play-page', true)"
  >
    <img
      :src="`${
        currentSong.song ? currentSong.picUrl : currentSong.al.picUrl
      }?imageView=1&type=webp&thumbnail=80x0`"
      alt=""
    />
    <h3>
      {{ currentSong.name }}
      <span>
        -
        {{
          currentSong.song
            ? currentSong.song.artists[0].name
            : currentSong.ar[0].name
        }}</span
      >
    </h3>
    <div class="progress" @click.stop="$emit('toggle-playing-state')">
      <canvas width="40" height="40" ref="canvas"></canvas>
      <div class="icon" :class="[playing ? 'pause' : 'play']"></div>
    </div>
    <button  class="btn" @click.stop="$emit('toggle-show-play-list', true)"></button>
  </section>
  <!-- </footer> -->
</template>

<script>
export default {
  props: {
    currentSong: Object,
    playing: Boolean,
    currentTime: Number,
    duration: Number,
    currentPalyList: Array,
  },

  mounted: function () {
    // console.log(this.$refs.canvas);
  },
  computed: {
    percentage: function () {
      return this.currentTime / this.duration;
    },
  },
  watch: {
    percentage: function (n) {
      var context = this.$refs.canvas.getContext("2d");
      context.clearRect(0, 0, 40, 40);

      context.beginPath();
      context.arc(
        20,
        20,
        18,
        (Math.PI / 180) * (0 - 90),
        (Math.PI / 180) * (360 * n - 90)
      );
      context.strokeStyle = "#000";
      context.lineWidth = 1;
      context.stroke();

      context.beginPath();
      context.arc(
        20,
        20,
        18,
        (Math.PI / 180) * (360 * n - 90),
        (Math.PI / 180) * (360 - 90)
      );
      context.strokeStyle = "lightgray";
      context.lineWidth = 1;
      context.stroke();
    },
  },
};
</script>

<style lang="less" scoped>
.playbar {
  width: 100%;
  height: 40px;
  padding-bottom: 10px;
  position: fixed;
  bottom: 0;
  left: 0;
  background: #fff;
  display: flex;
  align-items: center;
  padding: 0 12px;
  box-shadow: 0 0 10px 3px inset rgba(0, 0, 0, 0.3);
  .btn {
    border: none;
    outline: none;
    width: 30px;
    height: 30px;
    // 背景图片、背景是否平铺、背景位置、背景大小
    background-image: url(../image/列表.png)  ;
    background-repeat: no-repeat;
    background-position: center center;
    background-size: contain;
  }
  img {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    animation: rotate 6s linear infinite;
    animation-play-state: paused;
  }
  h3 {
    padding: 0 15px;
    flex: 1;
    font-size: 12px;
    display: -webkit-box;
    -webkit-line-clamp: 1;
    -webkit-box-orient: vertical;
    overflow: hidden;
    span {
      color: #888;
    }
  }
  .progress  {
    width: 30px;
    height: 30px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    canvas  {
      width: 100%;
      height: 100%;
      position: absolute;
      top: 0;
      left: 0;
    }
    .icon  {
      width: 12px;
      height: 12px;
      display: flex;
      justify-content: space-around;
      align-items: center;
      &.play  {
        border-top: 6px solid transparent;
        border-bottom: 6px solid transparent;
        border-left: 10px solid #000;
        transform: translateX(3px);
      }
       &.pause  {
        width: 12px;
        height: 10px;
        display: flex;
        justify-content: space-around;
        align-items: center;
            &::before,
            &::after  {
          content: "";
          display: block;
          height: 100%;
          width: 20%;
          background: black;
        }
      }
    }
  }
  &.playing {
    img {
      animation-play-state: running;
    }
  }
}
</style>