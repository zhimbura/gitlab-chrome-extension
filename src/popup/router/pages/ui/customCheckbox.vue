<template>
    <ul class="tg-list">
        <li class="tg-list-item" v-if="type === 'light'">
            <input class="tgl tgl-light" :id="groupName" @change="change"  v-model="defaultValue" type="checkbox"/>
            <label class="tgl-btn" :for="groupName"></label>
        </li>
        <li class="tg-list-item"  v-if="type === 'ios'">
            <input class="tgl tgl-ios" :id="groupName" v-model="defaultValue" type="checkbox"/>
            <label class="tgl-btn" :for="groupName"></label>
        </li>
        <li class="tg-list-item"  v-if="type === 'skewed'">
            <input class="tgl tgl-skewed" :id="groupName" v-model="defaultValue" type="checkbox"/>
            <label class="tgl-btn" data-tg-off="OFF" data-tg-on="ON" :for="groupName"></label>
        </li>
        <li class="tg-list-item"  v-if="type === 'flat'">
            <input class="tgl tgl-flat" :id="groupName" v-model="defaultValue" type="checkbox"/>
            <label class="tgl-btn" :for="groupName"></label>
        </li>
        <li class="tg-list-item"  v-if="type === 'flip'">
            <input class="tgl tgl-flip" :id="groupName" v-model="defaultValue" type="checkbox"/>
            <label class="tgl-btn" data-tg-off="Nope" data-tg-on="Yeah!" :for="groupName"></label>
        </li>
    </ul>
</template>

<script>
  export default {
    name: "custom-checkbox",
    model: {
      event: 'change'
    },
    props: {
      type: {
        type: String,
        default: 'light'
      },
      defaultValue: {
        type: Boolean,
        default: false
      },
      groupName: {
        type: String,
        default: 'cb' + Date.now()
      }
    },
    methods: {
      change () {
        this.$emit('change', this.defaultValue)
      }
    }
  }
</script>

<style lang="scss" scoped>
    ul,
    li {
        list-style: none;
        margin: 0;
        padding: 0;
    }

    .tg-list {
        text-align: center;
        display: flex;
        align-items: center;
    }

    .tg-list-item {
        margin: 0;
    }
    .tgl {
        display: none;

        // add default box-sizing for this scope
        &,
        &:after,
        &:before,
        & *,
        & *:after,
        & *:before,
        & + .tgl-btn {
            box-sizing: border-box;
            &::selection {
                background: none;
            }
        }

        + .tgl-btn {
            outline: 0;
            display: block;
            width: 3em;
            height: 1.5em;
            position: relative;
            cursor: pointer;
            user-select: none;
            &:after,
            &:before {
                position: relative;
                display: block;
                content: "";
                width: 50%;
                height: 100%;
            }

            &:after {
                left: 0;
            }

            &:before {
                display: none;
            }
        }

        &:checked + .tgl-btn:after {
            left: 50%;
        }
    }

    // themes
    .tgl-light {
        + .tgl-btn {
            background: #f0f0f0;
            border-radius: 2em;
            padding: 2px;
            transition: all .4s ease;
            &:after {
                border-radius: 50%;
                background: #fff;
                transition: all .2s ease;
            }
        }

        &:checked + .tgl-btn {
            background: #9FD6AE;
        }
    }

    .tgl-ios {
        + .tgl-btn {
            background: #fbfbfb;
            border-radius: 2em;
            padding: 2px;
            transition: all .4s ease;
            border: 1px solid #e8eae9;
            &:after {
                border-radius: 2em;
                background: #fbfbfb;
                transition:
                        left .3s cubic-bezier(
                                        0.175, 0.885, 0.320, 1.275
                        ),
                        padding .3s ease, margin .3s ease;
                box-shadow:
                        0 0 0 1px rgba(0,0,0,.1),
                        0 4px 0 rgba(0,0,0,.08);
            }

            &:hover:after {
                will-change: padding;
            }

            &:active {
                box-shadow: inset 0 0 0 2em #e8eae9;
                &:after {
                    padding-right: .8em;
                }
            }
        }

        &:checked + .tgl-btn {
            background: #86d993;
            &:active {
                box-shadow: none;
                &:after {
                    margin-left: -.8em;
                }
            }
        }
    }

    .tgl-skewed {
        + .tgl-btn {
            overflow: hidden;
            transform: skew(-10deg);
            backface-visibility: hidden;
            transition: all .2s ease;
            font-family: sans-serif;
            background: #888;
            &:after,
            &:before {
                transform: skew(10deg);
                display: inline-block;
                transition: all .2s ease;
                width: 100%;
                text-align: center;
                position: absolute;
                line-height: 2em;
                font-weight: bold;
                color: #fff;
                text-shadow: 0 1px 0 rgba(0,0,0,.4);
            }

            &:after {
                left: 100%;
                content: attr(data-tg-on);
            }

            &:before {
                left: 0;
                content: attr(data-tg-off);
            }

            &:active {
                background: #888;
                &:before {
                    left: -10%;
                }
            }
        }

        &:checked + .tgl-btn {
            background: #86d993;
            &:before {
                left: -100%;
            }

            &:after {
                left: 0;
            }

            &:active:after {
                left: 10%;
            }
        }
    }

    .tgl-flat {
        + .tgl-btn {
            padding: 2px;
            transition: all .2s ease;
            background: #fff;
            border: 4px solid #f2f2f2;
            border-radius: 2em;
            &:after {
                transition: all .2s ease;
                background: #f2f2f2;
                content: "";
                border-radius: 1em;
            }
        }

        &:checked + .tgl-btn {
            border: 4px solid #21E76E;
            &:after {
                left: 50%;
                background: #21E76E;
            }
        }
    }

    .tgl-flip {
        + .tgl-btn {
            padding: 2px;
            transition: all .2s ease;
            font-family: sans-serif;
            perspective: 100px;
            &:after,
            &:before {
                display: inline-block;
                transition: all .4s ease;
                width: 100%;
                text-align: center;
                position: absolute;
                line-height: 2em;
                font-weight: bold;
                color: #fff;
                position: absolute;
                top: 0;
                left: 0;
                backface-visibility: hidden;
                border-radius: 4px;
            }

            &:after {
                content: attr(data-tg-on);
                background: #02C66F;
                transform: rotateY(-180deg);
            }

            &:before {
                background: #FF3A19;
                content: attr(data-tg-off);
            }

            &:active:before {
                transform: rotateY(-20deg);
            }
        }

        &:checked + .tgl-btn {
            &:before {
                transform: rotateY(180deg);
            }

            &:after {
                transform: rotateY(0);
                left: 0;
                background: #21E76E;
            }

            &:active:after {
                transform: rotateY(20deg);
            }
        }
    }
</style>
