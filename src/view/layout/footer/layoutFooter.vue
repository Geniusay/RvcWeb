<template>
  <footer class="footer2-wrap" :class="{ hidden: y > 10 }">
    <div class="footer-copy">© RVC 2024</div>
    <div class="footer-center">
      <a class="mantine-1innaxq" type="button" data-button="true" href="/join-us">
        <div class="mantine-3xbgk5 mantine-Button-inner">
          <span class="mantine-qo1k2 mantine-Button-label">加入我们 💼</span>
        </div>
      </a>
      <a class="mantine-x6buan" type="button" data-button="true"  @click="userAgreementDialogVisible = true">
        <div class="mantine-3xbgk5 mantine-Button-inner">
          <span class="mantine-qo1k2 mantine-Button-label">用户协议</span>
        </div>
      </a>
    </div>
    <el-dialog v-model="userAgreementDialogVisible" style="background-color: rgba(26,27,30);color: white;" title="用户协议" width="30%">
      <span></span>
      <template #footer>
        <span class="dialog-footer" style="display: flex;justify-content: right;">
          <span @click="userAgreementDialogVisible = false" class="dialog-footer__cancel">关闭</span>
        </span>
      </template>
    </el-dialog>
    <div class="footer-right">
      <a class="mantine-96znl7" type="button" data-button="true" href="javascript:void(0);" target="_blank">
        <div class="mantine-3xbgk5 mantine-Button-inner">
          <span class="mantine-qo1k2 mantine-Button-label">
            <router-link to="" target="_blank">关于我们❓</router-link>
          </span>
        </div>
      </a>
    </div>

  </footer>
  <div class="scroll-to-top-wrap" style="transition: bottom 300ms linear 0s" @click="scrollToTop" v-show="y > 50">
    <button class="scroll-to-top-btn">
      <div class="scroll-to-top-btn-content">
        <span class="scroll-to-top__icon">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none"
            stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class=" ">
            <path d="M12 5l0 14"></path>
            <path d="M18 11l-6 -6"></path>
            <path d="M6 11l6 -6"></path>
          </svg>
        </span>
        <span class="scroll-to-top__text">回到顶部</span>
      </div>
    </button>
  </div>
</template>

<script lang="ts" setup>
import { useWindowScroll } from '@vueuse/core';
import { ref } from 'vue';
const { x, y } = useWindowScroll();
console.log(x)
const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth',
  });
};
let userAgreementDialogVisible = ref(false)
</script>

<style scoped lang="scss">
.dialog-footer__cancel {
    color: white;
    height: 35px;
    padding: 0 20px;
    line-height: 35px;
    border-radius: 5px;
    border: rgba(55, 58, 64) 1px solid;
    background-color: rgba(37, 38, 43);
    cursor: pointer;
}

.dialog-footer__cancel:hover {
    background-color: rgba(44, 46, 51);
}
.footer2-wrap {
  min-width: 1111px;
  width: 100%;
  bottom: 0;
  left: 0;
  z-index: 100;

  position: fixed;
  box-sizing: border-box;
  background-color: #1a1b1e;
  border-top: 1px solid #2c2e33;

  padding: 12px;

  box-sizing: border-box;
  display: flex;
  flex-direction: row;
  align-items: center;
  flex-wrap: wrap;
  justify-content: flex-start;
  gap: 20px;
  flex-wrap: nowrap;
  transition: bottom 0.5s ease-in-out;

  &.hidden {
    bottom: -62px;
  }
}

.footer-copy {
  font-family:
    -apple-system,
    BlinkMacSystemFont,
    Segoe UI,
    Roboto,
    Helvetica,
    Arial,
    sans-serif,
    Apple Color Emoji,
    Segoe UI Emoji;
  color: rgb(193, 194, 197);
  font-size: inherit;
  line-height: 1.55;
  text-decoration: none;
  font-weight: 700;
  white-space: nowrap;
  user-select: none;
}

.footer-center {
  box-sizing: border-box;
  display: flex;
  flex-direction: row;
  align-items: center;
  flex-wrap: wrap;
  justify-content: flex-start;
  gap: 0;
  flex-wrap: nowrap;

  .mantine-12d6g0a {
    -webkit-tap-highlight-color: transparent;
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;
    cursor: pointer;
    border: 0;
    padding: 0;

    appearance: none;
    font-size: 16px;
    background-color: transparent;
    text-align: left;
    color: #c1c2c5;

    text-decoration: none;
    box-sizing: border-box;
    height: 30px;
    padding-left: 14px;
    padding-right: 14px;
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;

    display: inline-block;
    width: auto;
    border-radius: 4px;
    font-weight: 600;
    position: relative;
    line-height: 30px;
    font-size: 12px;

    user-select: none;
    cursor: pointer;
    border: 1px solid transparent;
    background-color: transparent;
    color: #fcc2d7;
    padding-right: 10px;
    padding-left: 10px;
  }

  .mantine-1innaxq {
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;
    cursor: pointer;
    border: 0;
    padding: 0;

    appearance: none;
    font-size: 16px;
    background-color: transparent;
    text-align: left;
    color: #c1c2c5;
    text-decoration: none;
    box-sizing: border-box;
    height: 30px;
    padding-left: 14px;
    padding-right: 14px;
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;
    -webkit-tap-highlight-color: transparent;
    display: inline-block;
    width: auto;
    border-radius: 4px;
    font-weight: 600;
    position: relative;
    line-height: 30px;
    font-size: 12px;

    user-select: none;
    cursor: pointer;
    border: 1px solid transparent;
    background-color: transparent;
    color: #b2f2bb;
    padding-right: 10px;
    padding-left: 10px;
  }

  .mantine-x6buan {
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;
    cursor: pointer;
    border: 0;
    padding: 0;
    appearance: none;
    font-size: 16px;
    background-color: transparent;
    text-align: left;
    color: #c1c2c5;
    text-decoration: none;
    box-sizing: border-box;
    height: 30px;
    padding-left: 14px;
    padding-right: 14px;
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;
    display: inline-block;
    width: auto;
    border-radius: 4px;
    font-weight: 600;
    position: relative;
    line-height: 30px;
    font-size: 12px;
    user-select: none;
    cursor: pointer;
    border: 1px solid transparent;
    background-color: transparent;
    color: #e9ecef;
    padding-right: 10px;
    padding-left: 10px;
  }
}

.footer-right {
  box-sizing: border-box;

  display: flex;
  flex-direction: row;

  align-items: center;

  flex-wrap: wrap;

  justify-content: flex-start;
  gap: 4px;

  flex-wrap: nowrap;
  margin-left: auto;

  .mantine-96znl7 {
    -webkit-tap-highlight-color: transparent;
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;
    cursor: pointer;
    border: 0;
    padding: 0;

    appearance: none;
    font-size: 16px;
    background-color: transparent;
    text-align: left;
    color: #c1c2c5;
    text-decoration: none;
    box-sizing: border-box;
    height: 30px;
    padding-left: 14px;
    padding-right: 14px;
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;

    display: inline-block;
    width: auto;
    border-radius: 4px;
    font-weight: 600;
    position: relative;
    line-height: 30px;
    font-size: 12px;

    user-select: none;
    cursor: pointer;
    border: 1px solid transparent;
    background-color: transparent;
    color: #e9ecef;
    padding-left: 10px;
    padding-right: 10px;
  }

  .mantine-ua08er {
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;
    cursor: pointer;
    border: 0;
    padding: 0;

    appearance: none;
    font-size: 16px;
    background-color: transparent;
    text-align: left;
    color: #c1c2c5;

    text-decoration: none;
    box-sizing: border-box;
    height: 36px;
    padding-left: 18px;
    padding-right: 18px;
    font-family:
      -apple-system,
      BlinkMacSystemFont,
      Segoe UI,
      Roboto,
      Helvetica,
      Arial,
      sans-serif,
      Apple Color Emoji,
      Segoe UI Emoji;

    display: inline-block;
    width: auto;
    border-radius: 4px;
    font-weight: 600;
    position: relative;
    line-height: 30px;
    font-size: 14px;

    user-select: none;
    cursor: pointer;
    border: 1px solid transparent;
    background-color: rgba(240, 140, 0, 0.2);
    color: #ffec99;
    padding-left: 4px;
    padding-right: 10px;

    .mantine-3xbgk5 {
      display: flex;

      align-items: center;

      justify-content: center;
      height: 100%;
      overflow: visible;

      a {
        text-decoration: none;
      }
    }
  }
}

.scroll-to-top-wrap {
  position: fixed;
  z-index: 111;
  bottom: 12px;
  right: 12px;

  .scroll-to-top-btn {
    transition-property: transform, opacity;
    transition-duration: 250ms;
    transition-timing-function: ease;
    transform-origin: center bottom;
    opacity: 1;
    transform: translateY(0px);

    padding: 0px 18px 0px 12px;
    appearance: none;
    text-align: left;
    text-decoration: none;
    box-sizing: border-box;
    height: 36px;

    display: inline-block;
    width: auto;
    border-radius: 4px;
    font-weight: 600;
    position: relative;
    line-height: 1;
    font-size: 14px;
    user-select: none;
    cursor: pointer;
    border: 1px solid transparent;

    background-color: var(--el-color-primary);

    color: rgb(255, 255, 255);

    .scroll-to-top-btn-content {
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100%;
      overflow: visible;

      .scroll-to-top__icon {
        display: flex;

        align-items: center;
        margin-right: 10px;
      }

      .scroll-to-top__text {
        white-space: nowrap;
        height: 100%;
        overflow: hidden;

        display: flex;

        align-items: center;
      }
    }
  }
}
</style>
