<template>
    <div class="sidebar-page">
                <b-modal v-model="isFileUploadActive">
                    <div class="center">
    <div><b-field class="file is-primary" :class="{'has-name': !!file}">
        <b-upload v-model="file" class="file-label">
            <span class="file-cta">
                <b-icon class="file-icon" icon="upload"></b-icon>
                <span class="file-label">Upload a PDF</span>
            </span>
            <span class="file-name has-text-white" v-if="file">
                {{ file.name }}
            </span>
        </b-upload>
    </b-field></div>
    <br>
    <b-button @click="handleUpload">Done</b-button>
    </div>
                </b-modal>
             <b-sidebar
                position="static"
                :mobile="mobile"
                :expand-on-hover="false"
                :reduce="true"
                type="is-light"
                open
            >
                <div class="p-1">
                    <!-- <div class="block">
                    <img
                        src="https://raw.githubusercontent.com/buefy/buefy/dev/static/img/buefy-logo.png"
                        alt="Lightweight UI components for Vue.js based on Bulma"
                    />
                    </div> -->
                    <b-menu class="is-custom-mobile">
                        <b-menu-list label="Menu">
                            <b-menu-item icon="eraser" label="Eraser" @click="erase"></b-menu-item>
                            <b-menu-item icon="pen" label="Pen" @click="write"></b-menu-item>
                            <b-menu-item icon="file-upload" label="Upload a PDF" @click="showModal"></b-menu-item>
                            <b-menu-item icon="menu" label="Navigation">
                                <router-link to="/"><b-menu-item icon="home" label="Home"></b-menu-item></router-link>
                                <router-link to="/about"><b-menu-item icon="information" label="About"></b-menu-item></router-link>
                            </b-menu-item>
                        </b-menu-list>
                        <!-- <b-menu-list>
                            <b-menu-item label="Expo" icon="link"></b-menu-item>
                        </b-menu-list>
                        <b-menu-list label="Actions">
                            <b-menu-item icon="logout" label="Logout"></b-menu-item>
                        </b-menu-list> -->
                    </b-menu>
                </div>
            </b-sidebar>

    </div>
</template>

<script>
import * as pdfjs from 'pdfjs-dist/legacy/build/pdf.js'
pdfjs.GlobalWorkerOptions.workerSrc = '/js/pdf.worker.min.js'
const CMAP_URL = '../../node_modules/pdfjs-dist/cmaps/'
const CMAP_PACKED = true
const ENABLE_XFA = true
export default {
  props: ['ctx'],
  data () {
    return {
      mobile: 'reduce',
      isFileUploadActive: false,
      file: null

    }
  },
  methods: {
    erase: function () {
      this.$parent.$data.isWrite = false
    },
    write: function () {
      this.$parent.$data.isWrite = true
    },
    showModal: function () {
      this.isFileUploadActive = true
    },
    handleUpload: function () {
    //   console.log(this.file.name)
      var ctx = this.$parent.$data.ctx
      pdfjs.getDocument({
        url: 'https://raw.githubusercontent.com/mozilla/pdf.js/ba2edeae/examples/learning/helloworld.pdf',
        cMapUrl: CMAP_URL,
        cMapPacked: CMAP_PACKED,
        enableXfa: ENABLE_XFA
      }).promise.then(function (pdf) {
        pdf.getPage(1).then(function (page) {
          var scale = 1.5
          var viewport = page.getViewport({ scale: scale })
          // Support HiDPI-screens.
          var outputScale = window.devicePixelRatio || 1

          var transform = outputScale !== 1
            ? [outputScale, 0, 0, outputScale, 0, 0]
            : null
          var renderContext = {
            canvasContext: ctx,
            transform: transform,
            viewport: viewport
          }
          page.render(renderContext)
        })
      })
      this.isFileUploadActive = false
    }
  }
}
</script>

<style lang="scss">
.center{
    display: flex;
    justify-content: center !important;
    align-items: center;
}

.p-1 {
  padding: 1em;
}
.sidebar-page {
    width: 10%;

}
@media screen and (max-width: 1023px) {
    .b-sidebar {
        .sidebar-content {
            &.is-mini-mobile {
                &:not(.is-mini-expand),
                &.is-mini-expand:not(:hover):not(.is-mini-delayed) {
                    .menu-list {
                        li {
                            a {
                                span:nth-child(2) {
                                    display: none;
                                }
                            }
                            ul {
                                padding-left: 0;
                                li {
                                    a {
                                        display: inline-block;
                                    }
                                }
                            }
                        }
                    }
                    .menu-label:not(:last-child) {
                        margin-bottom: 0;
                    }
                }
            }
        }
    }
}
@media screen and (min-width: 1024px) {
    .b-sidebar {
        .sidebar-content {
            &.is-mini {
                &:not(.is-mini-expand),
                &.is-mini-expand:not(:hover):not(.is-mini-delayed) {
                    .menu-list {
                        li {
                            a {
                                span:nth-child(2) {
                                    display: none;
                                }
                            }
                            ul {
                                padding-left: 0;
                                li {
                                    a {
                                        display: inline-block;
                                    }
                                }
                            }
                        }
                    }
                    .menu-label:not(:last-child) {
                        margin-bottom: 0;
                    }
                }
            }
        }
    }
}
.is-mini-expand {
    .menu-list a {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
}
</style>
