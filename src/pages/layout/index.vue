<template>
  <view id="box" ref="box"></view>
</template>

<script setup>
  import { ref, onMounted } from 'vue'
  const box = ref(null)
  const colWidth = 180
  const items = getItems(30)

  onMounted(() => {
    const items = getItems(10)
    console.log(box.value.clientWidth)
    init()
  })
  function getItems(len) {
    const items = []
    for (let i = 0; i < len; i++) {
      const width = Math.floor(Math.random() * (400 - 300 + 1) + 300)
      const height = Math.floor(Math.random() * (500 - 250 + 1) + 250)
      let url = `
        data:image/svg+xml;charset=utf-8,
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="${width}"
          height="${height}"
          viewBox="0 0 ${width} ${height}"
        >
          <text
            x="50%"
            y="50%"
            dominant-baseline="middle"
            text-anchor="middle"
            font-size="20"
            fill="black"
          >
            ${width} * ${height}
          </text>
        </svg>
      `
      url = url
        .replace(/[\r\n]+/g, '')
        .replace(/\s+/g, ' ')
        .trim()
      items.push({ width, height, url })
    }
    return items
  }
  /**
    @description:创建图片
    @param {Array} items:图片信息数组
    @param {Number} imgWidth:每张图片的宽度
  */
  const createImgs = (items, imgWidth) => {
    for (let i = 0; i < items.length; i++) {
      const { url, width: w, height: h } = items[i]
      const img = document.createElement('img')
      const ratio = imgWidth / w
      const imgHeight = Math.floor(h * ratio)
      img.width = imgWidth
      img.height = imgHeight
      img.src = url
      box.value.appendChild(img)
    }
  }

  /**
   * @description:计算列数，以及每列的间隙
   * @param {Number} imgWidth:每张图片的宽度
   * @**/
  const getColumnInfo = (imgWidth) => {
    console.log(box.value)
    const boxWidth = box.value.clientWidth
    const colNumber = Math.floor(boxWidth / imgWidth)

    const spaceNumber = colNumber - 1
    const leftSpace = boxWidth - colNumber * imgWidth
    const colSpace = parseFloat((leftSpace / spaceNumber).toFixed(2, 10))
    return { colNumber, colSpace }
  }
  /**
   *  @description: 图片布局
   *  @param {Array} items:图片信息数组
   *  @param {Number} imgWidth:每张图片的宽度
   *  @param {Number} rowSpace:每行的间隙
   *  @param {Number} colSpace:每列的间隙
   */
  const setPosition = (items, colNumber, colSpace, rowSpace) => {
    const nextTops = new Array(colNumber).fill(0)
    for (let i = 0; i < items.length; i++) {
      const img = box.value.children[i]
      const top = Math.min.apply(null, nextTops)
      img.style.top = `${top}px`
      const index = nextTops.indexOf(top)
      nextTops[index] = nextTopsp[index] + img.height + rowSpace
      const left = index * colSpace + index * img.width
      img.style.left = `${left}px`
    }
    const max = Math.max.apply(null, nextTops)
    box.value.style.height = `${max}px`
  }

  const init = () => {
    const { colNumber: cNumber, colSpace: cSpace } = getColumnInfo(colWidth)
    const rSpace = cSpace
    setPosition(items, cNumber, cSpace, rSpace)
  }
</script>

<style scoped>
  #box {
    position: relative;
  }
  #box img {
    position: absolute;
    object-fit: cover;
    background-color: #eee;
  }
</style>
