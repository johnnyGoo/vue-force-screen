<template>
    <div style="width: 100%;height: 100%;position: relative" ref="main">
        <div ref="parent" style="left: 50%;top: 50%;position: absolute;width: 0px;height: 0px">
            <div ref="child" style="position: absolute" :style="{'width':width,'height':height}">
                <slot name="background">

                </slot>
            </div>
            <div ref="content" style="left: 0px;top:0px;position: absolute">
                <slot name="content">

                </slot>
                <!--<div  style="position: absolute;top:200px;right:0px;color: #fff">{{   info.width}}:{{   info.height}}</div>-->
            </div>


        </div>
        <div v-if="displayInfo&&block===true"
             style="left: 0px;top:0px;width:100%;height:100%;position: absolute;background: rgba(0,0,0,1)">
            <div style="left: 50%;top: 50%;position: absolute;width: 0px;height: 0px">
                <div class="view_blocker_rotate_icon" style="margin-left: -33px;margin-top: -85px">

                </div>
                <div style="color: #fff;height: 20px;width: 500px;text-align: center;margin-top: 20px;margin-left: -250px">
                    {{blockMessage}}
                </div>
            </div>
        </div>
    </div>
</template>
<style>
    @keyframes view_blocker_rotate_icon_rotation {
        10% {
            transform: rotate(90deg);
            -webkit-transform: rotate(90deg);
        }
        50% {
            transform: rotate(0deg);
            -webkit-transform: rotate(0deg);
        }
        60% {
            transform: rotate(0deg);
            -webkit-transform: rotate(0deg);
        }
        90% {
            transform: rotate(90deg);
            -webkit-transform: rotate(90deg);
        }
        100% {
            transform: rotate(90deg);
            -webkit-transform: rotate(90deg);
        }
    }

    @-webkit-keyframes view_blocker_rotate_icon_rotation {
        10% {
            transform: rotate(90deg);
            -webkit-transform: rotate(90deg);
        }
        50% {
            transform: rotate(0deg);
            -webkit-transform: rotate(0deg);
        }
        60% {
            transform: rotate(0deg);
            -webkit-transform: rotate(0deg);
        }
        90% {
            transform: rotate(90deg);
            -webkit-transform: rotate(90deg);
        }
        100% {
            transform: rotate(90deg);
            -webkit-transform: rotate(90deg);
        }
    }

    .view_blocker_rotate_icon {
        display: inline-block;
        width: 67px;
        height: 109px;
        background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIYAAADaCAMAAABU68ovAAAAXVBMVEUAAAD29vb////x8fH////////x8fH5+fn29vby8vL////5+fn39/f6+vr////x8fH////////+/v7////09PT////x8fH39/f////////////////////x8fH///+WLTLGAAAAHXRSTlMAIpML+gb4ZhHWn1c2gvHBvq1uKJcC6k8b187lQ9yhhboAAAQYSURBVHja7d3blpowFIDhTUIAOchZDkre/zE7ycySrbUUpsRN2/1fzO18KzEqxEVgTiZNfgmmtxRc8iaR8HNe8x4BtjQePKayYCIoyBSgvNNE1AkNSHqZyLqk97EgUCCHBzZ5mkg7ScvIJuIyOyXBRFxgpqWZyGsAZLB1KjsJi8nutHU4JCRbFRH8tmirI9k8Jx2sqNs8K/m0LQkrktO2crgcgXGB4AiTEsB0hJfo9MGgX7CGcYiYwQxmMOOvZwRhBG8tCoMXjBDeXvWCEcHbi14wgCBmMIMZzGAGM5jxETNwzMAxA8cMHDNwzMAxA8cMHDNwzMAxA8cMHDNwzMAxY6E2rUQxnH2tz9cirlJFwFBJedaPnUv0M7++egPDE8iAJcIDmxwH5wwv9vUviw2kLbVO3TJU5uul/EyB0FoLp4x60PdGUd3qPurrWyjGGTc05u+1dcgI7/+tCCPARWGhH7o5Y7RCf+bH9ctXLp6v2BVDxfqz0oPXeSVaNtINo/1SXDv4dck8IIkbhtC2ol+iouEonTBCbYvVMnXOjxww6s/RFrBUpXHh/gw1rHj5d/qhYn9Gpk2FWh6xRBRX5Oj3Znh2Sq49/L6+y8pB26q9GbE2dbA2mVbx6I+7MfBglLCttm73ZQi7AD3iL4HqjFYJHSPRppqaUaJ3ATpGa+ckpGak2hRRMyqjGMkvl+xyFeSMwjAqcsZgGDdyhl0oNTnDN4yenJGZFGxNChP5/Y3efh6SM2rDOJMzboYxkDMqwyjIGcIw6F+io2FU1IxIm1JqRmgXSkvNKNCXeTpGrU0JNSO2c6LIGPgCS8AuDHz9ta0SXWDtxoDRH+MqlbC2Dt2G2JFRadtQZt2qq/orGowdGb2euxYiqWEpVWhTBnszoNAPdStuQwxqf0aocdWKW4Z+DfszIh8pxJqbuCE4YAC+4bm0evtipjpgJHeFnyyt1Ku2xa0bhjxr27p75rECNwyI9ZwvXkHq+7aTaMEV44YYy/spfgjgjNHaWW+GeUhGEX7tLlVinIFDDSgnOwhi1V6bU0b6tVS9eAERe863g4dRrtiHdc6o+nn5vtyVVgR79Cqt4uL6gfHPQyGqtP2vf7HADGbcYwaOGThm4JiBYwaOGThm4JiBYwaOGThm4JiBYwaOGThm4JiBYwaOGThm4JjhtOM+J/AgT008yDMkN/dPP9hzS8zAMQN3OEYeekp5YU7KOKXwVXqiY+QS7smcinGKABWdiBgpPJTSMHJ4KidhhPBUSMLw4CmPhKHgKUXCkHsygum71ftNSgCX6bsl8FQyfbcL5EdYsDk0R3j7aiA5wpt5AjKg/2gLJEBD/0Hf2OOf/vRrj6z/7GtP4B3nMKyjHA12kIPSjnJs3FEO0TvKkYJHOWCR+rjJH0Vn6fI5PjNbAAAAAElFTkSuQmCC');
        transform: rotate(90deg);
        -webkit-transform: rotate(90deg);
        -webkit-animation: view_blocker_rotate_icon_rotation infinite 1.5s ease-in-out;
        animation: view_blocker_rotate_icon_rotation infinite 1.5s ease-in-out;
        -webkit-background-size: 67px;
        background-size: 67px
    }
</style>


<script>
    function Size(x, y, width, height) {
        this.x = x || 0;
        this.y = y || 0;
        this.width = width || 0;
        this.height = height || 0;
    }

    Size.prototype.fixInRec = function (recBig) {
        var rec = this;
        var obj = {width: 0, height: 0};
        var rad = rec.width / rec.height;
        var radbig = recBig.width / recBig.height;
        if (rad > radbig) {
            obj.width = recBig.width;
            obj.height = obj.width / rad;
        } else {
            obj.height = recBig.height;
            obj.width = obj.height * rad;
        }
        obj.x = (recBig.width - obj.width) / 2;
        obj.y = (recBig.height - obj.height) / 2;
        obj.scale = obj.width / rec.width;
        return obj;
    };
    Size.prototype.fillInRec = function (recBig) {
        var rec = this;
        var obj = {width: 0, height: 0};
        var rad = rec.width / rec.height;
        var radbig = recBig.width / recBig.height;
        if (rad < radbig) {
            obj.width = recBig.width;
            obj.height = obj.width / rad;
        } else {
            obj.height = recBig.height;
            obj.width = obj.height * rad;

        }
        obj.x = (recBig.width - obj.width) / 2;
        obj.y = (recBig.height - obj.height) / 2;
        obj.scale = obj.width / rec.width;
        return obj;
    };

    function bindEvent(dom, event, cb, useCapture) {
        function remove() {
            dom.removeEventListener(event, icc, useCapture)
        }

        function icc(e) {
            if (cb(e) === true) {
                remove();
            }
        }

        dom.addEventListener(event, icc, useCapture);
        return remove;
    };


    function _cssToDom(el, obj) {
        if (!el) {
            return
        }
        for (let i in obj) {
            if (el.style) {
                el.style[i] = obj[i];
            }
        }
    };

    function fixCss(name, attr) {
        let cssObj = {};
        if (!attr || attr === '') {
            return cssObj;
        }

        cssObj[name] = attr;
        cssObj['-webkit-' + name] = attr;
        cssObj['-moz-' + name] = attr;
        cssObj['-ms-' + name] = attr;
        cssObj['-o-' + name] = attr;
        return cssObj;
    }

    const LANDSCAPE = 'landscape';
    const PORTRAIT = 'portrait';

    function viewType() {

        if (window.innerWidth > window.innerHeight) {
            return LANDSCAPE
        } else {
            return PORTRAIT
        }
        if (window.hasOwnProperty('orientation')) {
            console.log(window.orientation);
            if (window.orientation === 90 || window.orientation === -90) {
                return LANDSCAPE
            } else {
                return PORTRAIT
            }
        } else {

            if (window.innerWidth > window.innerHeight) {
                return LANDSCAPE
            } else {
                return PORTRAIT
            }

        }
    }

    // 注册
    export default {
        name: 'vue-fullscreen-video',
        // 声明 props
        props: {
            align: {
                type: String,
                default: 'default'
            },
            scroll: {
                type: Boolean,
                default: false
            },
            type: {
                type: String,
                default: PORTRAIT
            },
            width: {
                type: String,
                default: '20px'
            },
            height: {
                type: String,
                default: '20px'
            },
            transitionTime: {
                type: String,
                default: '0s'
            },
            block: {
                type: Boolean,
                default: false
            }
            , blockMessage: {
                type: String,
                default: ''
            }
        },
        data: function () {
            return {
                child: null, parent: null, content: null, windowsize: {width: 0, height: 0}, removeEvent: function () {
                }, displayInfo: false
            }
        },
        watch: {
            align: function (ov, nv) {
                this.update();
            },
            width: function (ov, nv) {
                this.update();
            },
            height: function (ov, nv) {
                this.update();
            }

        },
        methods: {
            update: function () {

                let rotation = 0;
                let content_rotation = 0;

                switch (this.type) {
                    case LANDSCAPE:
                        if (viewType() === PORTRAIT) {
                            rotation = 90;
                            content_rotation = -90;
                            this.displayInfo = true;
                            if (this.block === true) {
                                return;
                            }
                        } else {
                            this.displayInfo = false;
                        }
                        break;
                    default:
                        if (viewType() === LANDSCAPE) {
                            rotation = -90;
                            content_rotation = 90
                            this.displayInfo = true;
                            if (this.block === true) {
                                return;
                            }
                        } else {
                            this.displayInfo = false;
                        }
                        break
                }
                let cssObj;
                this.windowsize = {width: window.innerWidth, height: window.innerHeight};
                let windowsize = this.windowsize;
                let selfSize = {
                    width: Number(this.width.replace('px', '')),
                    height: Number(this.height.replace('px', ''))
                };

                if (rotation === 0) {

                    if (this.align === 'crop') {
                        cssObj = new Size(0, 0, selfSize.width, selfSize.height).fillInRec(windowsize);
                    } else {
                        cssObj = new Size(0, 0, selfSize.width, selfSize.height).fixInRec(windowsize);

                    }


                } else {

                    if (this.align === 'crop') {
                        cssObj = new Size(0, 0, selfSize.height, selfSize.width).fillInRec(windowsize);

                    } else {
                        cssObj = new Size(0, 0, selfSize.height, selfSize.width).fixInRec(windowsize);
                    }


                }

                this.child.style.left = -selfSize.width / 2 + 'px';
                this.child.style.top = -selfSize.height / 2 + 'px';
                let scale = cssObj.scale


                if (rotation === 0) {
                    this.content.style.width = windowsize.width + 'px'
                    this.content.style.height = windowsize.height + 'px'
                    this.content.style.left = -windowsize.width / 2 + 'px';
                    this.content.style.top = -windowsize.height / 2 + 'px';
                    _cssToDom(this.content, fixCss('transform', ' rotate(' + (rotation) + 'deg) scale(' + 1 / scale + ')'));
                } else {
                    this.content.style.width = windowsize.height + 'px'
                    this.content.style.height = windowsize.width + 'px'
                    this.content.style.left = -windowsize.height / 2 + 'px';
                    this.content.style.top = -windowsize.width / 2 + 'px';
                    _cssToDom(this.content, fixCss('transform', ' rotate(' + (content_rotation + rotation) + 'deg) scale(' + 1 / scale + ')'));

                }

                // this.info.width=this.child.style.left;
                // this.info.height=this.child.style.top;


                _cssToDom(this.parent, fixCss('transform-origin', '50% 50%'));
                _cssToDom(this.parent, fixCss('transform', ' rotate(' + rotation + 'deg) scale(' + scale + ')'));

            }
        },

        beforeDestroy: function () {
            this.removeEvent()
        },

        mounted: function () {
            let self = this;
            if (this.blockMessage === '') {
                this.blockMessage = this.type === LANDSCAPE ? "请横屏浏览" : "请竖屏浏览";
            }

            this.parent = this.$refs.parent;
            this.child = this.$refs.child;
            this.content = this.$refs.content;
            self.update();
            this.removeEvent = bindEvent(window, 'resize', function () {
                self.update();
                setTimeout(self.update, 200)
            });

            if (false === this.scroll) {
                this.$refs.child.setAttribute('ontouchmove', 'event.preventDefault();');
            }
            this.$refs.main.setAttribute('ontouchmove', 'event.preventDefault();');


        }


    }


</script>