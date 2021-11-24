<template>
    <div class="sidebar-page">
                <b-modal v-model="isFileUploadActive">
                    <b-field class="file is-primary" :class="{'has-name': !!file}">
                        <b-upload v-model="file" class="file-label">
                            <span class="file-cta">
                                <b-icon class="file-icon" icon="upload"></b-icon>
                                <span class="file-label">Upload a PDF</span>
                            </span>
                            <span class="file-name has-text-white" v-if="file">
                                {{ file.name }}
                            </span>
                        </b-upload>
                    </b-field>
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
                            <b-menu-item icon="file-upload" label="Upload a PDF" @click="handleUpload"></b-menu-item>
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
export default {

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
    handleUpload: function () {
      this.isFileUploadActive = true
    }
  }
}
</script>

<style lang="scss">
.file{
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
