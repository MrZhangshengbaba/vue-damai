<template>
  <div class="telephone">
    <transition name="fade">
      <!-- 动画里的元素必须有切换效果，才能出发动画效果 -->
      <!-- 绑定 v-show 用于触发动画效果-->
      <div class="selected" v-show="toggleShow" key="1">
        <div class="title border-1px">
          选择国家和地区
          <i class="title-icon iconfont icon-bacha" @click.stop="hide()"></i>
        </div>

        <div class="cell border-1px" @click="selectTel(1)">
          中国大陆 +86
          <i class="cell-icon iconfont icon-quanquan" v-show="telIsShow !== 1"></i>
          <svg class="cell-icon" aria-hidden="true" v-show="telIsShow == 1">
            <use xlink:href="#icon-gouxuan2" />
          </svg>
        </div>

        <div class="cell border-1px" @click="selectTel(2)">
          中国香港 +852
          <i class="cell-icon iconfont icon-quanquan" v-show="telIsShow !== 2"></i>
          <svg class="cell-icon" aria-hidden="true" v-show="telIsShow == 2">
            <use xlink:href="#icon-gouxuan2" />
          </svg>
        </div>

        <div class="cell border-1px" @click="selectTel(3)">
          中国澳门 +853
          <i class="cell-icon iconfont icon-quanquan" v-show="telIsShow !== 3"></i>
          <svg class="cell-icon" aria-hidden="true" v-show="telIsShow == 3">
            <use xlink:href="#icon-gouxuan2" />
          </svg>
        </div>

        <div class="cell border-1px" @click="selectTel(4)">
          中国台湾 +886
          <i class="cell-icon iconfont icon-quanquan" v-show="telIsShow !== 4"></i>
          <svg class="cell-icon" aria-hidden="true" v-show="telIsShow == 4">
            <use xlink:href="#icon-gouxuan2" />
          </svg>
        </div>
      </div>
    </transition>
    <Myloading v-show="false" />
  </div>
</template>

<script>
import Myloading from "../Myloading";

export default {
  data: () => ({
    telIsShow: 1,
    type: null,
    toggleShow: false  // 用于切换上下滑动的状态
  }),
  components: {
    Myloading
  },
  props: ["isCheck"],
  watch: {
    // isCheck为true，页面显示。
    // 侦听变化，改变控制上下滑动切换的状态值
    isCheck(newVal) {
      if (newVal) {
        this.toggleShow = true;
      } else {
        this.toggleShow = false;
      }
    }
  },
  methods: {
    selectTel(n) {
      this.telIsShow = n;
      switch (n) {
        case 2:
          this.type = 852;
          break;
        case 3:
          this.type = 853;
          break;
        case 4:
          this.type = 886;
          break;
        default:
          this.type = 86;
      }
      setTimeout(() => {
        this.$emit("tel-type", this.type);
      }, 300);
      this.toggleShow = false;
    },
    hide() {
      // 异步执行定时器，等待动画效果完成才隐藏页面
      setTimeout(() => {
        this.$emit("hide");
      }, 300);
      this.toggleShow = false;
    }

    // js钩子  无法实现效果
    // beforeEnter(el) {
    //   el.style.transition = "all 1s linear";
    // },
    // enter(el, done) {
    //   let step = 0;
    //   el.style.transform = "translateY(100%)";
    //   let animate = function() {
    //     el.offsetHeight;
    //     step++;
    //     if (step === 1) {
    //       el.style.transition = "all 1s linear";
    //       el.style.transform = "translateY(50%)";
    //     } else if (step === 2) {
    //       el.style.transition = "all 1s linear";
    //       el.style.transform = "translateY(0)";
    //     } else {
    //       el.removeEventListener("transitionend", animate);
    //       done();
    //     }
    //   };
    //   el.addEventListener("transitionend", animate);
    // },
    // afterEnter(el) {
    //   el.style.transition = "none";
    // },

    // beforeLeave(el) {
    //   el.style.transition = "all 1s linear";
    // },
    // leave(el, done) {
    //   let step = 0;
    //   el.style.transform = "translateY(100%)";
    //   let animate = function() {
    //     step++;
    //     if (step === 1) {
    //       el.style.transition = "all 1s linear";
    //       el.style.transform = "translateY(50%)";
    //     } else if (step === 2) {
    //       el.style.transition = "all 1s linear";
    //       el.style.transform = "translateY(0)";
    //     } else {
    //       el.removeEventListener("transitionend", animate);
    //       done();
    //     }
    //   };
    //   el.addEventListener("transitionend", animate);
    // },
    // afterLeave(el) {
    //   el.style.transition = "none";
    // }
    // @before-enter="beforeEnter"
    // @enter="enter"
    // @after-enter="afterEnter"
    // @before-leave="beforeLeave"
    // @leave="leave"
    // @after-leave="afterLeave"
  }
};
</script>

<style scoped lang="scss">
@import "../../../public/scss/index.scss";
@import "../../../public/scss/mixin.scss";

.telephone {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 10;
}

.selected {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 10rem;
  border-radius: 8px 8px 0 0;
  background-color: #fff;

  .title {
    font-size: 0.36rem;
    height: 1.2rem;
    line-height: 1.2rem;
    text-align: center;
    margin: 0 0.3rem;
    position: relative;
    @include scale-border-bottom(rgba(7, 17, 27, 0.2));

    .title-icon {
      display: inline-block;
      position: absolute;
      top: 0;
      right: 0;
      width: 0.5rem;
      height: 1.2rem;
      line-height: 1.2rem;
      font-size: 0.22rem;
      text-align: right;
      margin-right: 0.2rem;
    }
  }

  .cell {
    font-size: 0.3rem;
    height: 1.1rem;
    line-height: 1.1rem;
    margin: 0 0.3rem;
    @include scale-border-bottom(rgba(7, 17, 27, 0.2));

    .cell-icon {
      position: absolute;
      top: 0;
      right: 0;
      width: 0.6rem;
      height: 1.1rem;
      line-height: 1.1rem;
      display: inline-block;
      font-size: 0.4rem;
      text-align: center;
    }
  }
}

.fade-enter,
.fade-leave-to {
  // height: 0;
  transform: translateY(100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: all .2s linear;
  // animation: slideUp 1s linear;
}

.fade-enter-to,
.fade-leave {
  // height: 10rem;
  transform: translateY(0);
}
</style>
