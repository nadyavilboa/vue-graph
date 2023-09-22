<template>
    <div class="v-graph graph-wrapper">
        <div class="v-graph__bottom-ring">
            <div class="v-graph__value v-graph__max-value text">{{ maxValue }}</div>
            <div
                v-if="getLack"
                class="v-graph__current-value ring-diagram ring-diagram--target animate" 
                :style="{ '--p': targetValue, '--c': 'transparent' }"
                :number="targetValue"
            >
            </div>
            <div
                class="v-graph__current-value ring-diagram animate" 
                :style="{ '--p': value, '--c': getLack ? '' : '#38A651' }"
                :number="value"
            >
            </div>
            <div class="v-graph__middle-ring text text--middle-ring">
                <span v-if="getLack" class="v-graph__text">Ещё продать на завтра</span>
                <span v-else class="v-graph__text">Продано на завтра</span>
                <span class="v-graph__lack">
                    <span v-if="getLack" class="v-graph__result-val text--orange">+{{ getLack }}</span>
                    <span v-else class="v-graph__result-val text--green">{{ value }}</span>
                    <span>ч</span>
                </span>
            </div>
        </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'v-graph',
    components: {
    },
    props: {
        maxValue: {
            type: Number,
            default() {
                return 100
            }
        },
        value: {
            type: Number,
            default() {
                return 30
            }
        },
        targetValue: {
            type: Number,
            default() {
                return 80
            }
        }
    },
    data() {
        return {
        }
    },
    computed: {
        getLack() {
            return this.targetValue > this.value ? this.targetValue - this.value : '';
        }
    },
    methods: {},
    mounted() {
        console.log(this.maxValue, this.value, this.targetValue, this.getLack);
    }
  }
  </script>
  
  <style lang="scss" scoped>

    $page-background: #2e3640;
    $bottom-ring: #282F37;
    $bottom-ring-deep: #3E4B5C;
    $middle-ring: linear-gradient(315deg, #505B69 -13.06%, #353D47 114.17%);
    $text-color: #778AA6;
    $middle-ring-text: #B5C3D7;
    $orange-text: #FF8B02;
    $green-text: #38A651;
    $orange-diagram: #F96C00;

    .v-graph {
        position: relative;
        font-family: 'Poppins', sans-serif;
        box-sizing: border-box;

        &__value {
            position: absolute;
        }

        &__max-value {
            top: 15px;
            left: 30%;
            font-size: 12px;
        }
    }

    .v-graph__bottom-ring {
        width: 240px;
        height: 240px;
        border-radius: 50%;
        background: $bottom-ring-deep;
        filter: 
            drop-shadow(60px 60px 138px rgba(0, 0, 0, 0.20))
            drop-shadow(-24px -24px 40px rgba(253, 253, 253, 0.15));
        display: flex;
        justify-content: center;
        align-items: center;

        &::before {
            content: '';
            width: 240px;
            height: 240px;
            display: block;
            background: $bottom-ring;
            filter: 
                drop-shadow(60px 60px 138px rgba(0, 0, 0, 0.20))
                drop-shadow(-24px -24px 40px rgba(253, 253, 253, 0.15));
            z-index: -1;
            position: absolute;
            border-radius: 50%;
        }
    }

    .v-graph__middle-ring {
        width: 153px;
        height: 153px;
        border-radius: 50%;
        background: $middle-ring;
        filter: 
            drop-shadow(3px 3px 5px rgba(22, 27, 29, 0.20))
            drop-shadow(-3px -3px 5px rgba(250, 251, 255, 0.20));
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 3.61px;
        padding: 25px;
    }

    .v-graph__result-val {
        font-family: Roboto, sans-serif;
        font-size: 38px;
        font-weight: 900;
        line-height: 104%;
    }

    .v-graph__current-value {
        position: absolute;
    }

    .text {
        text-align: center;
        font-size: 14px;
        font-style: normal;
        font-weight: 400;
        line-height: 18px;
        color: $text-color;

        &--middle-ring {
            color: $middle-ring-text;
        }

        &--orange {
            color: $orange-text;
        }

        &--green {
            color: $green-text;
        }
    }

    @property --p {
        syntax: '<number>';
        inherits: true;
        initial-value: 0;
    }
  
    .ring-diagram {
        --p: 50;
        --b: 38px;
        --c: #F96C00;
        --w: 240px;

        width: var(--w);
        aspect-ratio: 1;
        display: inline-grid;
        margin: 5px;
        place-content: center;
    }

    .ring-diagram::before {
        content: "";
    }

    .ring-diagram::after {
        content: attr(number);
    }

    .ring-diagram::before,
    .ring-diagram::after {
        position: absolute;
        border-radius: 50%;
    }

    .ring-diagram::before {
        inset: 0;
        background:
        radial-gradient(farthest-side,var(--c) 98%,#0000) top/var(--b) var(--b) no-repeat,
        conic-gradient(var(--c) calc(var(--p)*1%),#0000 0);
        -webkit-mask:radial-gradient(farthest-side,#0000 calc(99% - var(--b)),#000 calc(100% - var(--b)));
                mask:radial-gradient(farthest-side,#0000 calc(99% - var(--b)),#000 calc(100% - var(--b)));
    }

    .ring-diagram::after {
        inset: calc(50% - var(--b)/2);
        background: $bottom-ring-deep;
        color: var(--c);
        font-size: 12px;
        font-weight: 700;
        border: 5px solid var(--c);
        transform: rotate(calc(var(--p)*3.6deg)) translateY(calc(50% - var(--w)/2));
        padding: 7px;
    }

    .ring-diagram--target::after {
        background: transparent;
        color: $text-color;
    }

    .animate {
        animation: diagram 1s .5s both;
    }

    @keyframes diagram {
        from{--p:0}
    }
  </style>
  