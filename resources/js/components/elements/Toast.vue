<template>
    <transition name="fade">
        <div v-if="show" class="toast fixed right-0 bottom-0 mr-4 mb-4"
             @mouseover="hoverStart"
             @mouseleave="hoverEnd"
             @touchstart="hoverStart"
             @touchend="hoverEnd"
             @touchcancel="hoverEnd">
            <alert :type="success ? 'success' : 'error'">
                <component v-bind:is="render()"></component>
            </alert>
        </div>
    </transition>
</template>

<script>
import Helpers from "../../services/Helpers";

export default {
    data() {
        return {
            message: '',
            show: false,
            success: true,
            timer: null,
            seconds: 5
        }
    },
    mounted() {
        this.$bus.$on('toast', this.open)
    },
    destroyed() {
        this.$bus.$off('toast', this.open)
    },
    methods: {
        render() {
            return {
                template: `<span>${this.message}</span>`
            };
        },
        open(message, isSuccess = true) {
            this.message = message;
            this.success = isSuccess;
            this.show = true;
            this.hoverStart();
            this.hoverEnd();
        },
        hoverStart() {
            if (this.timer) {
                clearTimeout(this.timer);
            }
        },
        hoverEnd() {
            this.timer = setTimeout(() => {
                this.show = false;
            }, this.seconds * 1000);
        }
    }
}
</script>
