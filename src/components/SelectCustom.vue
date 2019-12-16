<template>
    <div class="mr-select-default__wrapper">

        <div class="mr-select-default-toggle__wrapper" @click="toggleSelect" :class="stateClass" ref="dropdownSelect">
            <div class="mr-select-default-input__wrapper" @click="isVisible = !isVisible" >
                {{ current.value }}

                <span class="rn-select-arrow">
                    <svg xmlns="http://www.w3.org/2000/svg" width="14" height="10" role="presentation" class="vs__open-indicator"><path d="M9.211364 7.59931l4.48338-4.867229c.407008-.441854.407008-1.158247 0-1.60046l-.73712-.80023c-.407008-.441854-1.066904-.441854-1.474243 0L7 5.198617 2.51662.33139c-.407008-.441853-1.066904-.441853-1.474243 0l-.737121.80023c-.407008.441854-.407008 1.158248 0 1.600461l4.48338 4.867228L7 10l2.211364-2.40069z"></path></svg>
                </span>
            </div>

            <div class="mr-select-default-list__wrapper" v-if="isVisible">
                <div class="mr-select-default-list" v-for="item in options" @click="selectValue(item.id, item.value)" :key="`mr-select-default-list_${item.id}`">{{item.value}}</div>
            </div>
        </div>

    </div>
</template>

<script>

    export default {
        props: {
            /**
             * Contains the currently selected value. Very similar to a
             * `value` attribute on an <input>. You can listen for changes
             * using 'change' event using v-on
             * @type {Object||String||null}
             */
            value: {},
            options: {
                type: Object,
                default: () => ({
                    0: {
                        id: 0,
                        value: 'Default',
                    },
                    1: {
                        id: 1,
                        value: 'Value'
                    }
                })
            },
            current: {
                type: Object,
                default: () => ({
                    id: 0,
                    value: 'Default',
                })
            },
            selected: {
                type: String,
                default: 'Default'
            },
            /**
             * Tells vue-select what key to use when generating option
             * labels when each `option` is an object.
             * @type {String}
             */
            label: {
                type: String,
                default: 'label'
            },
            // getOptionLabel: {
            //     type: Function,
            //     default(option) {
            //         if (typeof option === 'object') {
            //             if (!option.hasOwnProperty(this.label)) {
            //                 return console.warn(
            //                     `[vue-select warn]: Label key "option.${this.label}" does not` +
            //                     ` exist in options object ${JSON.stringify(option)}.\n` +
            //                     'https://vue-select.org/api/props.html#getoptionlabel'
            //                 )
            //             }
            //             return option[this.label]
            //         }
            //         return option;
            //     }
            // },
        },
        data() {
            return {
                isVisible: false,
                stateClass: '',
                classOpen: 'open',
            }
        },
        methods: {
            documentClick(e) {
                let el = this.$refs.dropdownSelect;
                let target = e.target;

                if (this.isVisible === true) {
                    if (el !== target && !el.contains(target)) {
                        this.closeState();
                    }
                }
            },
            closeState() {
                this.isVisible = false;
                this.stateClass = '';
            },
            toggleSelect() {
                this.stateClass = this.stateClass === this.classOpen ? '' : this.classOpen;
            },
            selectValue(id, value) {
                this.isVisible = false;
                this.$emit('update-room-selected', {id, value});
            }
        },
        created() {
            document.body.addEventListener('click', this.documentClick);
        },
        updated() {
            document.body.addEventListener('click', this.documentClick);
        },
        destroyed() {
            document.body.addEventListener('click', this.documentClick);
        },
        watch: {
            current () {
            },
        },
    }
</script>

<style lang="scss">

    .mr-select-default__wrapper {
        cursor: pointer;
        width: 100%;
        color: #05070A;
    }

    .mr-select-default-toggle__wrapper {
        position: relative;
        width: 100%;

        &.open {
            .mr-select-default-input__wrapper:after {
                transform: translateY(-50%) rotate(180deg);
                transition: all .1s linear;
            }
        }
    }

    .mr-select-default-input__wrapper {
        position: relative;
        padding: 11.5px 16px;
        background: #fff;
        border: 1px solid #e3e3e3;
        user-select: none;

        .rn-select-arrow {
            position: absolute;
            right: 12px;
            top: 50%;
            transform: translateY(-50%);
            width: 20px;
            height: 20px;
            transition: all .1s linear;
        }

        &:hover {

        }
    }

    .mr-select-default-list__wrapper {
        position: absolute;
        background-color: #fff;
        top: 100%;
        left: 0;
        right: 0;
        z-index: 99;
        min-height: 88px;
        height: auto;
        max-height: 132px;
        overflow-y: auto;
        overflow-x: hidden;
        box-shadow: 0 3px 6px rgba(15, 11, 7, 0.25);
    }

    .mr-select-default-list {
        display: block;
        width: 100%;
        padding: 11px 16px 10px 16px;
        cursor: pointer;
        user-select: none;

        &:hover {
            color: #05070A;
            background-color: #eee;
        }
    }

</style>