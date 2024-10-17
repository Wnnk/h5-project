<template>
  <view id="box"></view>
</template>

<script setup>
  onMounted(() => {
    const items = getItems(10)
    console.log(items)
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

  const createImgs = (items, imgWidth) => {
    for (let i = 0; i < array.length; i++) {
      const element = array[i];
      
    }
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
