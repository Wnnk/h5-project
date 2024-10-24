<template>
  <div class="canvas-wrapper">
    <canvas
      id="myCanvas"
      style="width: 300px; height: 200px"
      ref="canvas"
      canvas-id="myCanvas"
    ></canvas>
  </div>
  <div>test</div>
</template>

<script setup>
  import { ref } from 'vue'
  let ctx = null
  onMounted(() => {
    ctx = uni.createCanvasContext('myCanvas')
    drawBall(100, 100, 100, 0, 98, 'red')

  })

  /**
   * @description 背景圆
   * @param {Number} beginPointX 开始坐标x
   * @param {Number} beginPointY 开始坐标y
   * @param {Number} radius 半径
   * @param {Number} startAngle 开始角度
   * @param {Number} endAngle 结束角度
   * @param {String} backgroundColor 背景颜色
   */
  const drawBall = (beginPointX, beginPointY, radius, startAngle, endAngle, backgroundColor) => {
    ctx.setStrokeStyle(backgroundColor)
    ctx.setLineWidth(2)
    ctx.arc(beginPointX, beginPointY, radius, startAngle, endAngle)
    ctx.stroke()
    ctx.setFillStyle(backgroundColor)
    ctx.fill()
    ctx.draw()
  }

  const drawRoundImage = (beginPointX, beginPointY, radius, startAngle, endAngle, image) => {
    const temp = Math.min(image.width, image.height)
    const scale = radius / temp
    ctx.rect(
      beginPointX - image.width * scale,
      beginPointY - image.height * scale,
      beginPointX + image.width * scale,
      beginPointY + image.height * scale
    )
    ctx.drawImage(
      image,
      beginPointX - (image.width * scale) / 2,
      beginPointY - (image.height * scale) / 2,
      image.width * scale,
      image.height * scale
    )
  }
</script>

<style></style>
