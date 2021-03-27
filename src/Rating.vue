<template>
    <div class="rating"
        @mouseover="hover = !readonly"
        @mouseleave="hover = false; hoverValue = 0">
        <div class="is-inline"
            v-for="step in max"
            :key="step">
            <span class="icon"
                :class="{'active': active(step) || isHalf(step)}"
                v-if="readonly">
                <fa :icon="icon(step)"/>
            </span>
            <a class="icon"
                @mouseover="hoverValue = step"
                @click="$emit('input', step)"
                :class="{'active': active(step)}"
                v-else>
                <fa icon="star"/>
            </a>
        </div>
    </div>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faStar, faStarHalfAlt, faStartHalfAlt } from '@fortawesome/free-solid-svg-icons';

library.add(faStar, faStarHalfAlt);

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
            return this.isHalf(step) ? faStarHalfAlt : faStar;
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

            &.active {
                color: gold;
            }
        }
    }
</style>
