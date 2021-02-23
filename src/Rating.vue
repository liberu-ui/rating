<template>
    <div class="rating is-inline-block"
        :class="{'readonly': readonly}"
        @mouseover="hover= !readonly"
        @mouseleave="hover=false; hoverValue=0">
        <div v-for="rate in max"
            style="display: inline-block;"
            :key="rate">
            <a class="icon"
                @mouseover="hoverValue=rate"
                @click="!readonly && $emit('input', rate)"
                :class="{'active': active(rate)}">
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
        value: {
            required: true,
        },
        max: {
            type: Number,
            requred: false,
            default: 5,
        },
        readonly: {
            type: Boolean,
            requred: false,
            default: false,
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
    a {
        color: grey;

        &.active {
            color: gold;

            &:hover {
                color: gold;
            }
        }

        &:hover {
            color: grey;
        }
    }

    &.readonly a {
        cursor: auto;
    }
}
</style>
