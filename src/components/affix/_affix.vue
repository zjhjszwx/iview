<template lang="">
    <div>
        <div ref="point" :class="classes" :style="styles">
            <slot></slot>
        </div>
        <div v-show="slot" :style="slotStyle"></div>
    </div>
</template>
<script>
const prefixCls = 'ivu-affix';
import { on, off } from '../../utils/dom';

// 获取窗口滚动距离
function getScroll(target){
    return target.pageYOffset;

}

function getOffset(element) {
    const rect = element.getBoundingClientRect();
    return {
        top: element.offsetTop
    };
}

export default {
    props: {
        offsetTop: {
            type: Number,
            default: 0
        },
        offsetBottom: {
            type: Number
        },
        useCapture: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            affix: false,
            styles: {},
            slot: false,
            slotStyle: {}
        };
    },
    computed: {
        offsetType() {
            let type = 'top';
            if(this.offsetBottom > 0) {
                type = 'bottom';
            }
            return type;
        },
        classes() {
            return [
                {
                    [`${prefixCls}`]: this.affix
                }
            ];
        }
    },
    mounted() {
        on(window, 'scroll', this.handleScroll);
    },
    beforeDestroy() {
        off(window, 'scroll', this.handleScroll);
    },
    methods: {
        handleScroll() {
            const scrollTop = getScroll(window);
            const elOffset = getOffset(this.$el);
            // console.log(scrollTop, elOffset.top);
            if(scrollTop > elOffset.top) {
                this.affix = true;
                this.styles = {
                    top: `${this.offsetTop}px`,
                };
            } else if(scrollTop < elOffset.top) {
                this.affix = false;
                this.styles = null;
            }


        }
    }
};
</script>
<style lang="">

</style>
