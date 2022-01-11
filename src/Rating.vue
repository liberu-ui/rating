<template>
    <div class="rating"
        @mouseover="hover = !readonly"
        @mouseleave="hover = false; hoverValue = 0">
        <div class="is-inline"
            v-if="clearControl && modelValue">
            <span class="icon has-text-muted is-clickable"
                :class="{ 'is-small': small }"
                @click="$emit('update:modelValue', null)">
                <fa icon="star"
                    :size="small ? 'sm': null"/>
            </span>
        </div>
        <div class="is-inline stars"
            v-for="step in max"
            :key="step">
            <span class="icon"
                :class="{ 'is-small': small }"
                v-if="readonly">
                <fa :icon="icon(step)"
                    :size="small ? 'sm': null"/>
            </span>
            <span class="icon is-clickable"
                :class="{ 'is-small': small }"
                @mouseover="hoverValue = step"
                @click="$emit('update:modelValue', step)"
                v-else>
                <fa :icon="icon(step)"
                    :size="small ? 'sm': null"/>
            </span>
        </div>
    </div>
</template>

<script>
import { FontAwesomeIcon as Fa } from '@fortawesome/vue-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faStar, faStarHalfAlt } from '@fortawesome/free-solid-svg-icons';
import { faStar as faStarAlt } from '@fortawesome/free-regular-svg-icons';

library.add(faStar, faStarAlt, faStarHalfAlt);

export default {
    name: 'Rating',

    components: { Fa },

    props: {
        clearControl: {
            type: Boolean,
            default: false,
        },
        max: {
            type: Number,
            default: 5,
        },
        readonly: {
            type: Boolean,
            default: false,
        },
        small: {
            type: Boolean,
            default: false,
        },
        modelValue: {
            type: Number,
            default: null,
        },
    },

    emits: ['update:modelValue'],

    data: () => ({
        hover: false,
        hoverValue: 0,
    }),

    computed: {
        currentValue() {
            return this.hover ? this.hoverValue : this.modelValue;
        },
    },

    methods: {
        icon(step) {
            if (this.isHalf(step)) {
                return faStarHalfAlt;
            }

            return step <= this.currentValue ? faStar : faStarAlt;
        },
        isHalf(step) {
            return step > this.currentValue && step - 1 < this.currentValue;
        },
    },
};
</script>

<style lang="scss">
    .rating {
        .stars > span.icon {
            color: gold;
        }
    }
</style>
