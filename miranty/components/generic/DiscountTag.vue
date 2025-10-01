<template>
    <div
        v-if="value > 0"
        class="discount-tag"
    >
        <span>{{ formattedValue }}</span>
    </div>
</template>

<script>
export default {
    name: 'DiscountTag',

    props: {
        value: {
            type: Number,
            default: 0,
        },
    },

    computed: {
        formattedValue() {
            return Math.round(this.value);
        },
    },
};
</script>

<style lang="scss" scoped>
    .discount-tag {
        padding: 5px 6px;
        display: flex;
        align-items: center;
    }

    @media only screen and (max-width: 700px) {
        .discount-tag {
            position: absolute;
            top: 0;
            right: 0;
            z-index: 1;

            span {
                font-size: 11px;
            }
        }
    }
</style>
