<template>
  <section class="play-page">
    <div
      class="mask"
      :style="{
        backgroundImage: `url(${
          currentSong.song ? currentSong.picUrl : currentSong.al.picUrl
        }?imageView=1&type=webp&thumbnail=246x0)`,
      }"
    ></div>
    <button class="btn" @click="$emit('toggle-show-play-page', false)">
      <img src="../image/返回.png" alt="" />
    </button>
    <section class="rotate">
      <img
        class="needle"
        :class="{ paused: !playing }"
        src="https://s3.music.126.net/mobile-new/img/needle-ab.png"
        alt=""
      />
      <section class="record" :class="{ playing: playing }">
        <img
          class="thumb"
          :src="currentSong.song ? currentSong.picUrl : currentSong.al.picUrl"
          alt=""
        />
        <img
          class="disc"
          src="https://s3.music.126.net/mobile-new/img/disc.png"
          alt=""
        />
      </section>
    </section>
    <section class="controls">
      <span @click="$emit('toggle-play-model')"></span>
      <span @click="$emit('prev-song')"></span>
      <span @click="$emit('toggle-playing-state')"></span>
      <span @click="$emit('next-song')"></span>
      <span @click.stop="$emit('toggle-show-play-list', true)"></span>
    </section>
  </section>
</template>
<script>
export default {
  props: {
    currentSong: Object,
    playing: Boolean,
    currentTime: Number,
    duration: Number,
  },
};
</script>

<style lang="less" seoped>
.pos-ab {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.play-page {
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  //   background: red;
  overflow: hidden;
  &::before {
    content: "";
    display: block;
    .pos-ab();
    z-index: -2;
    background-color: #333;
  }
  .btn {
    border: none;
    background: rgba(0, 0, 0, 0);
    transform: rotate(-90deg);
  }
  .mask {
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    filter: blur(25px);
    .pos-ab();
    z-index: -1;
    transform: scale(1.5);
    transition: all 0.2s;
  }
  .rotate {
    position: relative;
    padding-top: 22vw;
    .needle {
      z-index: 5;
      height: 40vw;
      position: absolute;
      top: 0;
      left: 50%;
      margin-left: -15px;
      transform-origin: 13px 13px;
      transform: rotate(0deg);
      transition: all 0.3s;
      &.paused {
        transform: rotate(-25deg);
      }
    }
    .record {
      position: relative;
      width: 80vw;
      height: 80vw;
      margin: 0 auto;
      animation: rotate 6s linear infinite;
      animation-play-state: paused;
      img {
        .pos-ab();
        border-radius: 50%;
        &.thumb {
          transform: scale(0.8);
        }
      }

      &.playing {
        animation-play-state: running;
      }
    }
  }
  .controls {
    margin-top: 44%;
    display: flex;
    justify-content: space-around;
    color: #fff;
    span:first-child {
      .img();
      background-image: url(../image/随机播放.png);
    }
    span:nth-child(2) {
      .img();
      background-image: url(../image/上一曲.png);
    }
    span:nth-child(3) {
      .img();
      background-image: url(../image/暂停.png);
    }
    span:nth-child(4) {
      .img();
      background-image: url(../image/下一曲.png);
    }
    span:last-child {
      .img();
      background-image: url(../image/列表.png);
    }
    .img {
      width: 30px;
      height: 30px;
      // 背景图片、背景是否平铺、背景位置、背景大小

      background-repeat: no-repeat;
      background-position: center center;
      background-size: contain;
    }
  }
}
</style>