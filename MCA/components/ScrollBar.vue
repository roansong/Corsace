<template>
    <div class="scroll">
        <div class="scroll__bar" />
        <div 
            class="scroll__thumb triangle"
            :class="`triangle-active--${selectedMode}`"
            :style="{'top':relativePos}"
        />
    </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from "vue-property-decorator";
import { State } from "vuex-class";

@Component
export default class ScrollBar extends Vue {

    @Prop({ type: String, default: "" }) readonly selector!: string;

    @State selectedMode!: string;

    scrollPos = 0;
    scrollSize = 1;
    pos = 0;
    bottom = false;

    async mounted () {
        const list = document.querySelector(this.selector);

        if (list) {
            list.addEventListener("scroll", this.handleScroll);
            this.scrollSize = list.scrollHeight - list.clientHeight;
        }
    }
    
    get relativePos (): string {
        const percent = this.scrollPos / this.scrollSize * 100;
        let currentlyBottom = false;
        if (percent >= 99)
            currentlyBottom = true;
        this.emit(currentlyBottom);
            
        return percent - 2 + "%";
    }

    handleScroll (event) {
        if (event.target) {
            this.scrollPos = event.target.scrollTop;
            this.scrollSize = event.target.scrollHeight - event.target.clientHeight; // U know... just in case the window size changes Lol
        }
    }

    emit (currentlyBottom: boolean): void {
        if (currentlyBottom !== this.bottom) {
            this.bottom = currentlyBottom;
            if (currentlyBottom === true)
                this.$emit("bottom");
        }
    }

}
</script>

<style lang="scss">
.scroll {
    height: 95%;

    position: absolute;
    right: 0;
    top: 0;
}

.scroll__bar {
    background-image: repeating-linear-gradient(white, white 1px, transparent 1px, transparent 10px);

    height: 100%;
    width: 7px;
    margin-right: 25px;
}

.scroll__thumb {
    position: absolute;

    transform: rotate(-90deg);

    right: 13px;
}
</style>
