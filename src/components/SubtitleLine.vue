<template>
  <div class="subtitle-line" :class="isActive ? 'active' : ''">
    <div class="subtitle-left">
      <Icon
        size="26"
        type="ios-play"
        @click="$emit('start-play-from', item.from)"
      />
      <Icon
        size="22"
        :type="isMarked ? 'md-heart' : 'md-heart-outline'"
        @click="$emit('mark-line', item)"
      />
    </div>
    <div :id="item.from" :index="index">
      <div class="major-subtitle">
        {{ item.text2 }}
      </div>
      <div class="minor-subtitle">
        {{ item.text1 }}({{ index + 1 }}/{{ subtitlesNum }})
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "SubtitleLine",
  computed: {
    isActive() {
      return (
        this.curPlaySecs > this.item.from && this.curPlaySecs < this.item.to
      );
    },
    isMarked() {
      return (
        this.remarks.findIndex(
          remark => remark.from === this.item.from && remark.to === this.item.to
        ) !== -1
      );
    }
  },
  props: {
    item: {
      type: Object
    },
    index: {
      type: Number
    },
    curPlaySecs: {
      type: Number
    },
    subtitlesNum: {
      type: Number
    },
    remarks: {
      type: Array
    }
  },
  watch: {
    isActive(val) {
      if (val)
        this.$el.scrollIntoView({
          behavior: "smooth",
          block: "center",
          inline: "nearest"
        });
    }
  }
};
</script>
<style lang="less">
.subtitle-line {
  padding: 15px 0;
  margin: 0 15px;
  border-bottom: 1px #eee solid;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

.subtitle-left {
  flex: 0 0 70px;
  & > * {
    margin-right: 5px;
    &:hover {
      background-color: #ddd;
      border-radius: 4px;
    }
  }
}

.subtitle-line {
  .major-subtitle {
    font-size: 18px;
  }
  .minor-subtitle {
    color: #999;
  }
}
.active {
  .major-subtitle {
    font-weight: bold;
    color: #40a9ff;
  }
}
</style>
