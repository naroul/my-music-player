<template>
  <div class="playlist-summary-wrapper">
    <div class="cover">
      <router-link :to="`/music/musiclist?id=${data.id}`">
        <img :src="data.picUrl || data.coverImgUrl" />
      </router-link>

      <div class="cover-bottom-bar">
        <i class="iconfont icon-service"></i>
        <span class="play-count">{{ playCount }}</span>
        <!-- 播放按钮 -->
        <i class="iconfont icon-play" @click="play(data.id)"></i>
      </div>
    </div>
    <p :class="['name', { elip: isElip }]">
      <router-link :to="`/music/musiclist?id=${data.id}`">
        {{ data.name }}
      </router-link>
    </p>
    <p class="author" v-if="isShowAuthor">
      <span>by</span>
      <router-link :to="`/user/info?id=${data.userId}`" class="author-name">
        {{ data.creator.nickname }}
      </router-link>
    </p>
  </div>
</template>

<script>
import { playerMixin } from "@/mixins";
import { getPlaylistDetail } from "@/apis/playlist";
import { formatPlayCount } from "^/formatPlayCount";

export default {
  mixins: [playerMixin],

  props: {
    /**
     * 推荐歌单的数据
     */
    data: {
      type: Object,
      required: true,
      default: {},
    },

    /**
     * 是否要显示歌单作者
     */
    isShowAuthor: {
      type: Boolean,
      required: false,
      default: false,
    },

    /**
     * 标题超出部分是否要显示省略号
     */
    isElip: {
      type: Boolean,
      required: false,
      default: false,
    },
  },

  computed: {
    /**
     * 歌单的播放数
     */
    playCount() {
      return formatPlayCount(this.data.playCount);
    },
  },

  methods: {
    /**
     * 将当前播放列表清空，并用歌单歌曲替代 并设置播放状态为 true
     */
    play(id) {
      /**
       * 播放歌单
       */
      this.plyPlaylist(id);

      /**
       * 设置播放状态为true
       */
      this.setPlayStatus(true);
      this.setVolConfigStatus(false);
    },
  },
};
</script>

<style lang="scss" scoped>
.playlist-summary-wrapper {
  display: inline-block;
  vertical-align: top;
  box-sizing: border-box;
  width: 140px;
  min-height: 200px;

  .cover {
    position: relative;
    width: 140px;
    height: 140px;
    font-size: 0;

    img {
      width: 140px;
      height: 140px;
    }

    .cover-bottom-bar {
      position: absolute;
      bottom: 0;
      padding: 5px 10px;
      box-sizing: border-box;
      width: 100%;
      height: 27px;
      display: flex;
      align-items: center;
      background: rgba(51, 51, 51, 0.6);

      .play-count {
        flex: 1;
        padding-left: 3px;
        height: 17px;
        line-height: 17px;
        text-align: left;
        color: rgba(255, 255, 255, 0.6);
        font-size: 12px;
      }

      .iconfont {
        color: rgba(255, 255, 255, 0.6);
      }

      .icon-play {
        font-size: 20px;

        &:hover {
          color: #fff;
        }
      }
    }
  }

  .name {
    margin: 8px 0 3px;
    line-height: 1.4;
    font-size: 14px;

    a {
      color: #505050;

      &:hover {
        text-decoration: underline;
      }
    }
  }

  .elip {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .author {
    margin-top: 0;
    margin-bottom: 20px;

    span,
    a {
      font-size: 12px;
      color: #999;
    }

    .author-name {
      color: #666;

      &:hover {
        text-decoration: underline;
      }
    }
  }
}
</style>
