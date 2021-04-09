<template>
  <div id="app">
    <div id="capture">
      <img class="absolute-img" width="500" src="/cat.jpg" alt="">
      <div class="capture-container">
        <h1 class="target-text">Hello world!</h1>
      </div>

    </div>
  </div>
</template>

<script>
import html2canvas from 'html2canvas';

export default {
  mounted() {
    html2canvas(document.querySelector("#capture"), {
      allowTaint: false,
    }).then(canvas => {
      canvas.setAttribute('id', 'renderedCanvas')
      document.querySelector("#capture").appendChild(canvas)
      const blurTarget = document.querySelector('.target-text')
      let ctx = canvas.getContext('2d');

      let rect = {
        startX: blurTarget.offsetLeft - 5,
        startY: blurTarget.offsetTop - 5,
        w: blurTarget.clientWidth + 10,
        h: blurTarget.clientHeight + 10
      };

      let imageObj = new Image();

      imageObj.onload = function () {
        ctx.filter = 'blur(5px)';
        ctx.drawImage(imageObj, 0, 0);

        let imgDraw = ctx.getImageData(rect.startX, rect.startY, rect.w, rect.h);
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        ctx.filter = 'none';
        ctx.drawImage(imageObj, 0, 0);
        ctx.putImageData(imgDraw, rect.startX, rect.startY);
      };
      imageObj.src = canvas.toDataURL();
    })
  }
}
</script>

<style>
body {
  padding: 0;
  margin: 0;
}
#capture {
  position: relative;
  width: 500px;
  height: 281px;
}
.absolute-img	{
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
}
#renderedCanvas {
  position: absolute;
  top: 0px;
  left: 0;
  z-index: 2;
}
.target-text {
  line-height: 50px;
  position: fixed;
  color: white;
  margin-left: 150px;
  margin-top: 100px;
}
.capture-container {
  position: inherit;
  display: flex;
  width: 100%;
  height: 100%;
  z-index: 3;
}
</style>
