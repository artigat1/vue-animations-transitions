<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <hr />
                <!--                <select @change="onAnimationChange" class="form-control">
                                    <option value="fade">Fade</option>
                                    <option value="slide">Slide</option>
                                </select>
                                <br /><br />
                                <button @click="show = !show" class="btn btn-primary">Show alert</button>
                                <br /><br />
                                <transition :name="alertAnimation" :type="transitionType">
                                    <div class="alert alert-info" v-if="show">This is some info</div>
                                </transition>
                                <transition name="slide" type="animation">
                                    <div class="alert alert-info" v-if="show">This is some info</div>
                                </transition>
                                <transition
                                        enter-active-class="animated bounce"
                                        leave-active-class="animated shake"
                                >
                                    <div class="alert alert-info" v-if="show">This is some info</div>
                                </transition>
                                <transition :name="alertAnimation" mode="out-in">
                                    <div class="alert alert-info" v-if="show">This is some info</div>>
                                    <div class="alert warning" v-if="!show">This is some warning</div>
                                </transition>-->
                <button @click="load = !load" class="btn btn-primary">Load / Remove element</button>
                <br /><br />
                <transition
                        :css="false"
                        @after-enter="afterEnter"
                        @after-leave="afterLeave"
                        @before-enter="beforeEnter"

                        @before-leave="beforeLeave"
                        @enter="enter"
                        @enter-cancelled="enterCancelled"
                        @leave="leave"
                        @leave-cancelled="leaveCancelled"
                >
                    <div style="width: 300px; height: 100px; background-color: lightgreen" v-if="load"></div>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                show: false,
                load: true,
                alertAnimation: 'fade',
                elementWidth: 100
            }
        },
        methods: {
            beforeEnter(el) {
                console.log('beforeEnter');
                this.elementWidth = 100;
                el.style.width = this.elementWidth + 'px';
            },
            enter(el, done) {
                console.log('enter');
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth + (round * 10)) + 'px';
                    round++;
                    if (round > 20) {
                        clearInterval(interval);
                        done();
                    }
                }, 20);
            },
            afterEnter(el) {
                console.log('afterEnter');
            },
            enterCancelled(el) {
                console.log('enterCancelled');
            },
            beforeLeave(el) {
                console.log('beforeLeave');
                this.elementWidth = 300;
                el.style.width = this.elementWidth + 'px'
            },
            leave(el, done) {
                console.log('leave');
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth - (round * 10)) + 'px';
                    round++;
                    if (round > 20) {
                        clearInterval(interval);
                        done();
                    }
                }, 20);
            },
            afterLeave(el) {
                console.log('afterLeave');
            },
            leaveCancelled(el) {
                console.log('leaveCancelled');
            }
        }
    }
</script>

<style>
    .fade-enter {
        opacity: 0;
    }

    .fade-enter-active {
        transition: opacity 1s;
    }

    .fade-leave {
        /* opacity 1 is the default, so don't actually need to set it*/
        /*opacity: 1;*/
    }

    .fade-leave-active {
        transition: opacity 1s;
        opacity: 0;
    }

    .slide-enter {
        /* start state setup in keyframe */
        opacity: 0;
    }

    .slide-enter-active {
        animation: slide-in 1s ease-out forwards;
        transition: opacity .5s;
    }

    .slide-leave {
        /* end state setup in keyframe */
    }

    .slide-leave-active {
        animation: slide-out 1s ease-out forwards;
        transition: opacity 1s;
        opacity: 0;
    }

    @keyframes slide-in {
        from {
            transform: translateY(20px);
        }
        to {
            transform: translateY(0);
        }
    }

    @keyframes slide-out {
        from {
            transform: translateY(0);
        }
        to {
            transform: translateY(20px);
        }
    }
</style>
