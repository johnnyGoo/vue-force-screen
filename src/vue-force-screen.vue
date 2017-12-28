<template>
    <div ref="parent" style="left: 50%;top: 50%;position: absolute;width: 0px;height: 0px">
        <div ref="child" style="position: absolute" :style="{'width':width,'height':height}">
            <slot name="background">

            </slot>
        </div>
        <div ref="content" style="left: 0px;top:0px;position: absolute" >
            <slot name="content">

            </slot>
            <!--<div  style="position: absolute;top:200px;right:0px;color: #fff">{{   info.width}}:{{   info.height}}</div>-->
        </div>
    </div>
</template>
<style>

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
      function bindEvent (dom, event, cb, useCapture) {
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
            if (window.orientation == 90 || window.orientation == -90) {
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
            }
        },
        data: function () {
            return {
                child: null, parent: null,content:null,windowsize:{width:0,height:0},removeEvent:function () {
                }
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
                let content_rotation=0;

                switch (this.type) {
                    case LANDSCAPE:
                        if (viewType() === PORTRAIT) {
                            rotation = 90;
                            content_rotation=-90
                        }
                        break;
                    default:
                        if (viewType() === LANDSCAPE) {
                            rotation = -90;
                            content_rotation=90
                        }
                        break
                }
                let cssObj;
                this.windowsize = {width: window.innerWidth, height: window.innerHeight};
                let windowsize=this.windowsize;
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


                if(rotation===0){
                    this.content.style.width=windowsize.width+'px'
                    this.content.style.height=windowsize.height+'px'
                    this.content.style.left = -windowsize.width/2 + 'px';
                    this.content.style.top = -windowsize.height/2 + 'px';
                    _cssToDom(this.content, fixCss('transform', ' rotate(' + (rotation) + 'deg) scale(' + 1/scale + ')'));
                }else{
                    this.content.style.width=windowsize.height+'px'
                    this.content.style.height=windowsize.width+'px'
                    this.content.style.left = -windowsize.height/2 + 'px';
                    this.content.style.top = -windowsize.width/2 + 'px';
                    _cssToDom(this.content, fixCss('transform', ' rotate(' + (content_rotation+rotation) + 'deg) scale(' + 1/scale + ')'));

                }

                // this.info.width=this.child.style.left;
                // this.info.height=this.child.style.top;









                _cssToDom(this.parent, fixCss('transform-origin', '50% 50%'));
                _cssToDom(this.parent, fixCss('transform', ' rotate(' + rotation + 'deg) scale(' + scale + ')'));

            }
        },

        beforeDestroy:function () {
            this.removeEvent()
        },

        mounted: function () {
            let self = this;

            this.parent = this.$refs.parent;
            this.child = this.$refs.child;
            this.content=this.$refs.content;
            self.update();
            this.removeEvent=bindEvent(window, 'resize', function () {
                self.update();
                setTimeout(self.update, 200)
            });

            if (false === this.scroll) {
                this.parent.setAttribute('ontouchmove', 'event.preventDefault();');
            }


        }


    }


</script>