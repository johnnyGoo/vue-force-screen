
###Usage
```
npm install vue-force-screen --save
```
```javascript
import VueForceScreen from 'vue-force-screen'
```

###Slot
```
 slot="background"  //适应部分
 slot="content"  //全屏显示部分(不会被裁切)
```



###props:
```
align:String 'crop'/'default'
type:String 'portrait'/'landscape'
width:String '*px' //只支持px后缀
height:String '*px' //只支持px后缀
scroll:Boolean false/true //是否允许浏览器默认滑动

```
###methods
```
update()

```
###properties
```
align
width
height

```

##注意
width height必须