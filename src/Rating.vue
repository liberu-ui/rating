<template>
    <div class="rating"
        @mouseover="hover = !readonly"
        @mouseleave="hover = false; hoverValue = 0">
        <div class="is-inline"
            v-for="rate in max"
            :key="rate">
            <span class="icon"
                :class="{'active': active(rate)}"
                v-if="readonly">
                <fa icon="star"/>
            </span>
            <a class="icon"
                @mouseover="hoverValue = rate"
                @click="$emit('input', rate)"
                :class="{'active': active(rate)}"
                v-else>
                <fa icon="star"/>
            </a>
        </div>
    </div>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faStar } from '@fortawesome/free-solid-svg-icons';

library.add(faStar);

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
        active(rate) {
            return this.hover
                ? rate <= this.hoverValue
                : rate <= this.value;
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
