<template>
    <div class="rating"
        @mouseover="hover = !readonly"
        @mouseleave="hover = false; hoverValue = 0">
        <div class="is-inline"
            v-for="step in max"
            :key="step">
            <span class="icon"
                v-if="readonly">
                <fa :icon="icon(step)"/>
            </span>
            <a class="icon"
                @mouseover="hoverValue = step"
                @click="$emit('input', step)"
                v-else>
                <fa :icon="icon(step)"/>
            </a>
        </div>
    </div>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faStar, faStarHalfAlt, faStartHalfAlt } from '@fortawesome/free-solid-svg-icons';
import { faStar as faStarAlt } from '@fortawesome/free-regular-svg-icons';

library.add(faStar, faStarAlt, faStarHalfAlt);

export default {
    name: 'Rating',

    props: {
        max: {
            type: Number,
            default: 5,
        },
        readonly: {
            type: Boolean,
            default: false,
        },
        value: {
            type: Number,
            default: null,
        },
    },

    data: () => ({
        hover: false,
        hoverValue: 0,
    }),

    methods: {
        active(step) {
            return this.hover
                ? step <= this.hoverValue
                : step <= this.value;
        },
        icon(step) {
            if (this.isHalf(step)) {
                return faStarHalfAlt;
            }

            if (this.value === null) {
                return faStarAlt;
            }

            return step <= this.value ? faStar : faStarAlt;
        },
        isHalf(step) {
            return step > this.value && step - 1 < this.value;
        },
    },
};
</script>

<style lang="scss">
    .rating {
        a, span {
            color: grey;

            &.icon {
                color: gold;
            }
        }
    }
</style>
