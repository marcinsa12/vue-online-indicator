<template>
  <div ref="OnlineIndicator" :class="`show-${positionClass} online-indicator ${$attrs.class}`" :style="styles">
        <span :style="spanStyles" :class="`${spanClass} indicator-text`">
            {{isOnline ? onlineMsg : offlineMsg}}    
        </span>
  </div>
</template>

<script>
export default {
    name: "online-indicator",
    props: {
        position: {
            type: String,
            validator: value => {
               return ['top-righ', 'top-left','top-center','bottom-right', 'bottom-left', 'bottom-center'].includes(value)
            },
            required: false,
            default: 'top-right'
        },
        timeout: {
            type: Number,
            required: false,
            default: 3000
        },
        spanClass: {
            type: String,
            required: false,
            default: ''
        },
        styles: {
            type: Object,
            required: false,
            default: () => ({})
        },
        spanStyles: {
            type: Object,
            required: false,
            default: () => ({})
        },
    },
    data: () => ({
        isOnline: !!window.navigator.onLine,
        timeoutHandle: null,
        onlineMsg: "You're Online!",
        offlineMsg: "You're Offline!"
    }),
    computed: {
        positionClass() {
            return ['top-righ', 'top-left','top-center','bottom-right', 'bottom-left', 'bottom-center'].includes(this.position) && this.position || 'top-right'
        }
    },
    methods: {
        wentOffline() {
            this.isOnline = false
            this.$emit('offline')
            this.showStatus()
            console.warn('Offline event detected from browsing context')
        },
        wentOnline() {
            this.isOnline = true;
            this.$emit('online')
            this.showStatus()
            console.log('Online event detected from browsing context')
        },
        showStatus() {
          this.$refs.OnlineIndicator.classList.add('indicator-visible')
          if(this.timeout) {
              window.clearTimeout(this.timeoutHandle)
              this.timeoutHandle = setTimeout(() => {
                  this.hideStatus()
              }, this.timeout);
          }
        },
        hideStatus() {
            this.$refs.OnlineIndicator.classList.remove('indicator-visible')
        },
        hasAttr(attr) {
            return this.$attrs.hasOwnProperty(attr)
        }
    },
    mounted() {
        window.removeEventListener('offline', this.wentOffline)
        window.removeEventListener('online', this.wentOnline)
        window.addEventListener('offline', this.wentOffline)
        window.addEventListener('online', this.wentOnline)
    },
    install(Vue) {
        if (this.install.installed) return;
        this.install.installed = true;
        Vue.component('online-indicator', this);
    }
}

</script>

<style scoped>
:root {
    
}

.online-indicator {
    width: 125px;
    z-index: 5;
    position: fixed;
    border-radius: 5%;
    transition: 500ms;
    opacity: 0;
    background: #1976D2;
    padding: 8px 12px;
}

.show-top-right {
    top: -5%;
    right: 5%;
}

.show-top-left {
    top: -5%;
    left: 5%;
}

.show-top-center {
    top: -5%;
    left: -50%;
    transform: translateX(-50%);
}

.show-top-center.indicator-visible, .show-top-left.indicator-visible, .show-top-right.indicator-visible {
    top: 5%;
    opacity: 1;
}

.show-bottom-right {
    bottom: -5%;
    right: 5%;
}

.show-bottom-left {
    bottom: -5%;
    left: 5%;
}
.show-bottom-center {
    bottom: -5%;
    left: -50%;
    transform: translateX(-50%);
}

.show-bottom-center.indicator-visible, .show-bottom-left.indicator-visible, .show-bottom-right.indicator-visible {
    bottom: 5%;
    opacity: 1;
}

.indicator-text {
    color: #fff
}
</style>