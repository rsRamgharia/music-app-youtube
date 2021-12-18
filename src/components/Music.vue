<template>
  <div class="player">
    <div class="cover">
      <img src="../assets/logo.jpg" alt="" />
    </div>
    <div class="info">
      <div class="title">Song Title</div>
      <div class="singer">Singer name</div>
    </div>
    <div class="volume-box" v-bind:class="{ active: volume.active }">
      <span class="volume-down" @click="handleVolumeDown()"
        ><i class="material-icons">remove</i></span
      >
      <input
        type="range"
        class="volume-range"
        step="1"
        :value="volume.range"
        min="0"
        max="100"
      />
      <span class="volume-up" @click="handleVolumeUp()"
        ><i class="material-icons">add</i></span
      >
    </div>
    <div class="btn-box">
      <i
        class="material-icons repeat"
        v-bind:class="{ active: music.loop }"
        @click="handleRepeat()"
        >repeat</i
      >
      <i
        class="material-icons favorite"
        v-bind:class="{ active: music.favorite }"
        @click="handleFavorite()"
      >
        favorite
      </i>
      <i
        class="material-icons volume"
        v-bind:class="{ active: volume.active }"
        @click="handleVolume()"
      >
        volume_up
      </i>
    </div>
    <div class="music-box">
      <input
        type="range"
        step="1"
        class="seekbar"
        v-model="seekbar.value"
        min="0"
        :max="seekbar.max"
      />

      <audio class="music-element">
        <source
          src="https://hosseinghanbari.ir/other/music-player/autumn.mp3"
          type="audio/mp3"
        />
      </audio>
      <span class="current-time">{{ currentTime }}</span
      ><span class="duration">{{ duration }}</span>
      <span class="play" @click="handlePlay()">
        <i class="material-icons">
          {{ !music.paused ? 'pause' : 'play_arrow' }}
        </i>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      music: {
        element: HTMLMediaElement,
        paused: true,
        loop: false,
        volume: 50,
        favourite: false,
      },
      volume: {
        active: false,
        range: 80,
      },
      seekbar: {
        max: 100,
        value: 0,
      },
      duration: '0:00',
      currentTime: '0:00',
    };
  },
  mounted() {
    this.music.element = document.querySelector('.music-element');
    this.music.element.addEventListener('ended', () => {
      this.currentTime = '0:00';
      this.music.element.currentTime = 0;
    });

    this.music.element.onloadeddata = () => {
      this.seekbar.max = this.music.element.duration;
      this.duration =
        (parseInt(this.seekbar.max / 60) % 60) +
        ':' +
        parseInt(this.seekbar.max % 60);
    };

    this.music.element.ontimeupdate = () => {
      this.seekbar.value = this.music.element.currentTime;
    };

    this.music.element.addEventListener(
      'timeupdate',
      () => {
        let cs = parseInt(this.music.element.currentTime % 60);
        let cm = parseInt((this.music.element.currentTime / 60) % 60);

        if (cs <= 9) {
          cs = `0${cs}`;
        }

        this.currentTime = cm + ':' + cs;
      },
      false
    );
  },
  methods: {
    handlePlay() {
      this.music.paused = !this.music.paused;
      if (this.music.paused) {
        this.music.element.pause();
      } else {
        this.music.element.play();
      }
    },
    handleRepeat() {
      this.music.loop = !this.music.loop;
    },
    handleVolume() {
      this.volume.active = !this.volume.active;
    },
    handleVolumeDown() {
      this.volume.range = Number(this.volume.range) - 20;
      this.music.element.volume = Number(this.volume.range) / 100;
    },
    handleVolumeUp() {
      this.volume.range = Number(this.volume.range) + 20;
      this.music.element.volume = Number(this.volume.range) / 100;
    },
    handleFavourite() {
      this.music.favourite = !this.music.favourite;
    },
  },
};
</script>

<style lang="scss">
html {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  height: 100%;
  background-image: url('../assets/bg.jpg');
  background-position: center;
  background-size: cover;
  position: relative;
  font-family: 'Roboto', sans-serif;
}
body {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  height: 100%;
  background-image: url('../assets/bg.jpg');
  background-position: center;
  background-size: cover;
  position: relative;
  font-family: 'Roboto', sans-serif;
}
* {
  &::selection {
    background-color: unset;
  }
}
.player {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 330px;
  height: 530px;
  border-radius: 15px;
  backdrop-filter: blur(7px) saturate(200%);
  -webkit-backdrop-filter: blur(7px) saturate(200%);
  background-color: rgba(255, 255, 255, 0.37);
  border-radius: 12px;
  border: 1px solid rgba(209, 213, 219, 0.3);
  input[type='range'] {
    -webkit-appearance: none !important;
    margin: 0px;
    padding: 0px;
    background: #f2eae4;
    height: 5px;
    width: 150px;
    outline: none;
    cursor: pointer;
    overflow: hidden;
    border-radius: 5px;
    &::-ms-fill-lower {
      background: #f2eae4;
    }
    &::-ms-fill-upper {
      background: #f2eae4;
    }
    &::-moz-range-track {
      border: none;
      background: #f2eae4;
    }
    &::-webkit-slider-thumb {
      -webkit-appearance: none !important;
      background: #ff3677;
      height: 5px;
      width: 5px;
      border-radius: 50%;
      box-shadow: -100vw 0 0 100vw #f7d9b9;
    }
    &::-moz-range-thumb {
      background: #ff3677;
      height: 8px;
      width: 8px;
      border-radius: 100%;
    }
    &::-ms-thumb {
      -webkit-appearance: none !important;
      background: #ff3677;
      height: 8px;
      width: 8px;
      border-radius: 100%;
    }
  }
  .cover {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    overflow: hidden;
    position: absolute;
    left: 50%;
    top: 50px;
    transform: translateX(-50%);
    box-shadow: 0 5px 20px 0 #9e00d56d;
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
  .info {
    position: absolute;
    left: 50%;
    top: 240px;
    transform: translateX(-50%);
    text-align: center;
    .title {
      font-size: 20px;
      font-weight: 700;
      color: #444;
      margin-bottom: 2px;
    }
    .singer {
      font-size: 12px;
      color: #72646f;
    }
  }
  .btn-box {
    position: absolute;
    top: 330px;
    width: 100%;
    display: flex;
    justify-content: center;
    i {
      font-size: 24px;
      color: #72646f;
      margin: 0 30px;
      cursor: pointer;
    }
    i.active {
      color: #ff3677;
    }
  }
  .volume-box {
    display: none;
    position: absolute;
    left: 50%;
    top: 295px;
    transform: translateX(-50%);
    z-index: 1;
    padding: 0 20px;
    .volume-down {
      position: absolute;
      left: -15px;
      top: 50%;
      transform: translateY(-50%);
      cursor: pointer;
      color: #72646f;
    }
    .volume-up {
      position: absolute;
      right: -15px;
      top: 50%;
      transform: translateY(-50%);
      cursor: pointer;
      color: #72646f;
      &::selection {
        background-color: unset;
      }
    }
    input[type='range'] {
      height: 5px;
      width: 150px;
      margin: 0 0 15px 0;
    }
  }
  .volume-box.active {
    display: block;
  }
  .music-box {
    position: absolute;
    left: 50%;
    top: 385px;
    transform: translateX(-50%);
    input[type='range'] {
      height: 5px;
      width: 230px;
      margin: 0 0 10px 0;
      &::-webkit-slider-thumb {
        height: 5px;
        width: 7px;
      }
    }
    .current-time {
      position: absolute;
      left: -35px;
      top: 50%;
      transform: translateY(-50%);
      font-size: 12px;
      color: #72646f;
    }
    .duration {
      position: absolute;
      right: -35px;
      top: 50%;
      transform: translateY(-50%);
      font-size: 12px;
      color: #72646f;
    }
    .play,
    .pause {
      position: absolute;
      left: 50%;
      top: 55px;
      transform: translateX(-50%);
      width: 50px;
      height: 50px;
      border-radius: 50px;
      background-color: #c8054e;
      cursor: pointer;
      transition: all 0.4s;
      i {
        font-size: 36px;
        color: #fff;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-48%, -50%);
      }
    }
  }
}
</style>
