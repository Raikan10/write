<template>
<div>
      <div><canvas ref="write" v-on:pointermove="handleMouseMove" v-on:pointerdown="handlePointerDown"></canvas></div>
      <side-bar :style="{ marginTop:height+'px'}" class="side-bar"></side-bar>
</div>
</template>

<script>
import { getStroke } from 'perfect-freehand'
import SideBar from '../components/SideBar.vue'
export default {
  components: {

    SideBar

  },
  data: function () {
    return {
      points: [],
      color: '',
      isWrite: true,
      writeOptions: {
        size: 6,
        smoothing: 0.99,
        thinning: 0.28,
        streamline: 0.78,
        easing: (t) => t,
        start: {
          taper: 53,
          cap: true
        },
        end: {
          taper: 58,
          cap: true
        },
        simulatePressure: true
      },
      eraserOptions: {
        size: 27,
        smoothing: 0.01,
        thinning: -0.31,
        streamline: 0.05,
        easing: (t) => t,
        start: {
          taper: 0,
          cap: true
        },
        end: {
          taper: 0,
          cap: true
        }
      },
      width: 0

    }
  },
  computed: {
    options: function () {
      if (this.isWrite) {
        this.changeColor()
        return this.writeOptions
      }
      this.changeColor()
      return this.eraserOptions
    },
    height: function () {
      return window.innerHeight / 2
    }
  },
  watch: {
    points: function () {
      const outlinePoints = getStroke(this.points, this.options)
      const pathData = this.getSvgPathFromStroke(outlinePoints)
      const myPath = new Path2D(pathData)
      this.ctx.fillStyle = this.color
      this.ctx.fill(myPath)
    }
  },
  methods: {
    handlePointerDown: function (event) {
      event.target.setPointerCapture(event.pointerId)
      this.points = [[event.offsetX, event.offsetY, event.pressure]]
    },
    handleMouseMove: function (event) {
      if (event.buttons !== 1) return
      this.points = [...this.points, [event.offsetX, event.offsetY, event.pressure]]
    //   this.draw(event.offsetX, event.offsetY)
    },
    draw: function (x, y) {
      console.log(x, y)
      this.ctx.beginPath()
      this.ctx.arc(x, y, 2, 0, 2 * Math.PI)
      this.ctx.fill()
    },
    getSvgPathFromStroke: function (stroke) {
      if (!stroke.length) return ''

      const d = stroke.reduce(
        (acc, [x0, y0], i, arr) => {
          const [x1, y1] = arr[(i + 1) % arr.length]
          acc.push(x0, y0, (x0 + x1) / 2, (y0 + y1) / 2)
          return acc
        },
        ['M', ...stroke[0], 'Q']
      )

      d.push('Z')
      return d.join(' ')
    },
    changeColor: function () {
      if (this.isWrite) this.color = '#000000'
      else this.color = '#ffffff'
    }

  },
  mounted () {
    this.canvas = this.$refs.write
    this.width = window.innerWidth
    this.canvas.width = window.innerWidth * 0.99
    this.canvas.height = window.innerHeight * 0.99
    this.ctx = this.canvas.getContext('2d')
  }
}
</script>

<style scoped lang="scss">
.side-bar {
  position: fixed;
  z-index: 2;
  margin-top: var(--height);
}
canvas{
    position: fixed;
    border: 10px;
    border-color: #000000;
    border-style: solid;
    touch-action: none;
    display: flex;
    justify-content: center;
    align-items: center;

}

</style>
