<template>
  <li
    v-if="item.song"
    class="songlistitem"
    @click="$emit('change-current-song', item)"
  >
    <div class="left">
      <div class="songname">
        {{ item.name }}
        <span v-for="n in item.song.alias" :key="n">{{ n }}</span>
      </div>
      <div class="info">
        <span v-if="item.song.exclusive"></span>
        <i v-for="artist in item.song.artists" :key="artist.id">{{
          artist.name
        }}</i>
        <b>{{ item.song.album.name }}</b>
      </div>
    </div>
    <div class="icon">
      <div
        class="play"
        :class="{ current: currentSongId === item.id, playing: playing }"
      >
        <i></i>
        <i></i>
        <i></i>
        <i></i>
      </div>
    </div>
  </li>
  <li v-else class="songlistitem" @click="$emit('change-current-song', item)">
    <div class="num"><slot></slot></div>
    <div class="left">
      <div class="songname">
        {{ item.name }}<span v-for="n in item.alia" :key="n">{{ n }}</span>
      </div>
      <div class="info">
        <!-- <span v-if="item.song.exclusive"></span> -->
        <i v-for="artist in item.ar" :key="artist.id">{{ artist.name }}</i>
        <b>{{ item.al.name }}</b>
      </div>
    </div>
    <div class="icon">
      <div
        class="play"
        :class="{ current: currentSongId === item.id, playing: playing }"
      >
        <i></i>
        <i></i>
        <i></i>
        <i></i>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true,
    },
    currentSongId: {
      type: Number,
    },
    playing: Boolean,
  },
};
</script>

<style lang="less" scoped>
.songlistitem {
  padding: 5px 12px 5px 0;
  display: flex;
  align-items: center;
  height: 54px;
  // border-bottom: 1px solid gray;
  box-shadow: 0 1px 0 0px rgb(231, 231, 231);
  margin-left: 12px;
  .num {
    padding: 0 12px 0 0;
  }
  &.lt3 {
    .num {
      color: #d43c33;
    }
  }
  .left {
    flex: 1;
    // margin-right: 10px;
    .songname {
      font-size: 17px;
      line-height: 1.5;
      // padding-left: 10px;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
      overflow: hidden;
      span {
        color: #888;
        &::before {
          content: "(";
        }
        &::after {
          content: ")";
        }
      }
    }
    .info {
      font-size: 10px;
      color: #888;
      line-height: 1.5;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
      overflow: hidden;
      span {
        background-image: url("https://s3.music.126.net/mobile-new/img/index_icon_2x.png");
        background-repeat: no-repeat;
        background-size: 166px auto;
        display: inline-block;
        width: 12px;
        height: 8px;
        margin-right: 4px;
      }
      i {
        font-style: normal;
        &:after {
          content: "/";
          margin: 0 3px;
        }
        &:last-of-type {
          &::after {
            content: "-";
            margin: 0 5px;
          }
        }
      }
      b {
        font-weight: normal;
      }
    }
  }
  .icon {
    width: 22px;
    height: 22px;
    // margin-left: 10px;
    // background: red;
    position: relative;
    display: flex;
    justify-content: center;
    align-content: center;
    .play {
      // position: absolute;
      // top: 0;
      // left: 0;
      width: 100%;
      height: 100%;
      background-image: url("https://s3.music.126.net/mobile-new/img/index_icon_2x.png");
      background-repeat: no-repeat;
      background-size: 166px auto;
      background-position: -24px 0;
    }
    .current {
      width: 16px;
      height: 16px;
      background: red;
      background: none;
      display: flex;
      justify-content: space-around;
      i {
        width: 2px;
        height: 100%;
        background: #d43c33;
        transform-origin: bottom;
        animation: playing 0.5s linear 0s infinite alternate;
        animation-play-state: paused;
        &:nth-child(1) {
          animation-delay: -0.5s;
        }
        &:nth-child(2) {
          animation-delay: -0.7s;
        }
        &:nth-child(3) {
          animation-delay: -0.3s;
        }
      }
      &.playing {
        i {
          animation-play-state: running;
        }
      }
    }
  }
  @keyframes playing {
    from {
      transform: scaleY(0.2);
    }
    to {
      transform: scaleY(1);
    }
  }
}
</style>