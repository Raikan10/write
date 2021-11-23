<template>
<div class="container">
      <div class="  columns"><b-button @click="isWrite=!isWrite">Eraser</b-button></div>
      <div class="  columns"><canvas ref="write" width="1000" height="700" v-on:pointermove="handleMouseMove" v-on:pointerdown="handlePointerDown"></canvas></div>
</div>
</template>

<script>
import { getStroke } from 'perfect-freehand'

export default {
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
      }

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
    this.ctx = this.canvas.getContext('2d')
  }
}
</script>

<style scoped>
canvas{
    border: 10px;
    border-color: #000000;
    border-style: solid;
}
</style>
