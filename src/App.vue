<template>
  <div id="app">
    <div class="container">
      <iframe class="iframe" ref="iframe"></iframe>
    </div>
  </div>
</template>

<script>
import { RealseeSignals } from '@realsee/vr-signals';

export default {
  name: 'App',
  mounted() {
    const vrSignals = new RealseeSignals({
      // 请将下面的 vrLink 替换为您的 VR 链接
      vrLink: 'https://sandbox.realsee.cn/2VMM6QZN',
      // 传入 iframe 元素
      element: this.$refs.iframe,
    })

    // 请将所有的监听注册和方法调用都放在 onReady 回调中
    vrSignals.onReady(() => {
      console.log('onReady');

      const openTags = []
      let interval = null
      // 监听标签点击事件
      vrSignals.onEvent('tag.click', (data) => {
        console.log('tag.click', data);
        const { id } = data
        if (openTags.includes(id)) {
          // 关闭标签
          clearInterval(interval)
          openTags.splice(openTags.indexOf(id), 1)
        } else {
          // 开启标签
          // 设置标签的数据
          interval = setInterval(() => {
            vrSignals.send('tag.changeData', {
              id: 10896811,
              // 随机数
              description: Math.random(),
            })
          }, 1000)
          openTags.push(id)
        }

      })

      // 监听监控点开启事件
      vrSignals.onEvent('monitor.open', (data) => {
        console.log('monitor.open', data);
      })

      // 监听监控点关闭事件
      vrSignals.onEvent('monitor.close', (data) => {
        console.log('monitor.close', data);
      })



      // // 开启监控点
      // vrSignals.send('monitor.open', {
      //   id: 'xxxx',
      // })

      // // 关闭监控点
      // vrSignals.send('monitor.close', {
      //   id: 'xxxx',
      // })

    })
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
  width: 100%;
}

.container {
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f9f9f9;
  padding: 32px;
}

.iframe {
  flex-grow: 1;
  width: 100%;
  height: 100%;
  border: none;
  border-radius: 8px;
}
</style>
