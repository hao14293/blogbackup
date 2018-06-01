---
title: SVG之Animation
tags: css
categories: web
abbrlink: 64093
date: 2018-04-18 23:53:40
---
<code>&lt;animate></code>元素用于实现动画效果（动画截图比较麻烦，本文中的例子最好直接写demo看效果）

## 基本动画
将<animate>元素嵌入到元素内，来实现该元素的动画效果。
```sh
<rect x="10" y="10" width="200" height="20" stroke="black" fill="none">
    <animate
    attributeName="width"
    attributeType="XML"
    from="200" to="20"
    begin="0s" dur="5s"
    fill="freeze" />
</rect>```
以上代码会一个200*20的长方形，在5秒内渐变成一个20*20的正方形，并且在动画结束时停留在正方形的状态。
<!--more-->
<animate>元素的基本属性：
* <b>attributeName</b>
定义发生变化的元素属性名
* <b>attributeType</b>


当attributeType="XML"时，attributeName被认为是XML的属性；当attributeType="CSS"时，attributeName被认为是css的属性；不指定attributeType时，默认为"auto"，会先将attributeName作为css的属性，如果无效，再将attributeName作为XML的属性。
* <b>from & to & by</b>


from和to分别定义发生变化的属性的初始值和终止值。from可缺省，表示初始值即为<animate>父元素相应的属性值。可用by替换to，表示变化偏移量。可以理解为to = from + by。
* <b>begin & dur & end</b>


begin定义动画开始时间；dur定义动画所需时间；end定义动画终止时间。时间单位h：小时；min：分钟；s：秒；ms：毫秒。默认时间单位为s
* <b>fill</b>


当fill="freeze"时，动画终止时，发生变化的元素属性值停留在动画终止时的状态；当fill="remove"时，动画终止时，发生变化的元素属性值回复到动画起始时的状态。fill属性默认值为remove。
允许在同一个元素内嵌入多个<animate>
```sh
<rect x="10" y="10" width="20" height="20" style="stroke: black; fill: #cfc;">
    <animate attributeName="width" attributeType="XML" begin="0s" dur="2s" from="20" to="120" fill="freeze"/>
    <animate attributeName="height" attributeType="XML" begin="0s" dur="2s" from="20" by="100" fill="freeze"/>
</rect>```
## 动画时间
当begin设置为一个具体时间，比如2s，svg会在元素加载完毕后，过2秒开始执行动画。
begin还可以指定一个其他<animate>的begin或者end，比如：
```sh
<circle cx="60" cy="60" r="30" style="fill: #f9f; stroke: gray;">
    <animate id="c1" attributeName="r" attributeType="XML" begin="0s" dur="4s" from="30"  to="10" fill="freeze"/>
</circle>
<circle cx="120" cy="60" r="10" style="fill: #9f9; stroke: gray;">
    <animate attributeName="r" attributeType="XML" begin="c1.end" dur="4s" from="10" to="30" fill="freeze"/>
</circle>```
第二个圆的动画执行起始时间为第一个圆动画执行完毕时间。
begin属性还可以进行简单计算：
begin="c1.end+1.5s"：表示动画执行起始时间为第一个圆执行完毕后的1.5秒。

当end设置为一个具体时间，比如2s，svg会在元素加载完毕后，过2秒即停止执行动画，不管这个元素的动画是否执行完毕。如果end设置的比begin小，则动画根本不会执行。

end同样可以指定一个其他<animate>的begin或者end，同样支持计算。

## 重复动画
通过设置repeatDur或者repeatCount属性，让动画重复执行。
* <b>repeatDur</b>


设置动画执行的总时长。在repeatDur设置的时间内，动画一直会重复执行。如果repeatDur小于dur，repeatDur的作用与end一样。
* <b>repeatCount</b>


设置动画重复执行的次数。
repeatDur和repeatCount都可以通过设置为indefinit实现无限循环动画。
当repeatDur和repeatCount同时作用于同一个<animate>时，动画终止时间取两者中较小值。
repeat可作为begin和end中的参数使用：
```sh
<circle cx="60" cy="60" r="15" style="fill: none; stroke: red;">
    <animate id="circleAnim" attributeName="cx" attributeType="XML" begin="0s" dur="5s" repeatCount="3" from="60" to="260" fill="freeze"/>
</circle>
<rect x="230" y="80" width="30" height="30" style="fill: #ccf; stroke: black;">
    <animate attributeName="x" attributeType="XML" begin="circleAnim.repeat(1)+2.5s" dur="5s" from="230" to="30" fill="freeze"/>
</rect>```
长方形的动画会在圆形动画执行过一遍后延迟2.5秒后开始执行。
repeat(n)中的n需大于0。

## 复杂属性的动画
动画还可作用于颜色、paths、d等非纯数字属性。

颜色的动画过程可以分解成r,g,b,a四个数字的渐变。比如从#f00变化到#0f0，红色部分从1渐变到0，绿色部分同时从0渐变到1。
paths和d要实现动画有一个前提，其参数个数不能变。变化前后参数一一对应，所有参数同时渐变。
```sh
<polygon points="30 30 70 30 90 70 10 70" style="fill:#fcc; stroke:black">
    <animate id="animation"
             attributeName="points"
             attributeType="XML"
             to="50 30 70 50 50 90 30 50"
             begin="0s" dur="5s" fill="freeze"/>
</polygon>
<path d="M15 50 Q 40 15, 50 50, 65 32, 100 40" style="fill:none; stroke: black" transform="translate(0,50)">
    <animate attributeName="d"
             attributeType="XML"
             to="M50 15 Q 15 40, 50 50, 32 65, 40 100"
             begin="0s" dur="5s" fill="freeze"/>
</path>```
## 多节点动画
实现一个属性的连续变化有两种方式：
* 多个<animate>组合
* 使用value + keyTimes + calcMode属性

基础动画中提到过多<animate>组合方式；这里说下values + keyTimes + calcMode。
* <b>values</b>


values属性值表示一个动画经过的节点数值，数值间以分号分割。
```sh
<circle cx="175" cy="75" r="20" fill="red">
    <animate attributeName="r"
             attributeType="XML"
             values="20;50;20"
             begin="0" dur="1" repeatCount="indefinite"
             fill="freeze"></animate>
</circle>```
上例中1秒内，圆的半径由20变为50，再由50变为20。
* <b>keyTimes</b>


keyTimes属性值与values属性值一一对应，第一个数值永远是0（表示起始时间点），最后一个数值永远是1（表示终止时间点）,中间的数值表示变化到对应values属性值时所处时间点百分比(0~1之间)。
```sh
<circle cx="175" cy="75" r="20" fill="red">
    <animate attributeName="r"
             attributeType="XML"
             values="20;50;20"
             keyTimes="0;0.2;1"
             begin="0" dur="1" repeatCount="indefinite"
             fill="freeze"></animate>
</circle>```
上例中0.2秒时圆的半径由20变为50，在之后的0.8秒又从50变为20。
* <b>calcMode</b>


calcMode可以影响动画各阶段的表现。calcMode有四种属性值：paced, linear, discrete, spline

calcMode="paced"时，动画会忽略keyTimes属性，根据values数值以匀速变化。
calcMode="linear"时，动画根据values和keyTimes属性，在每个时间段内匀速变化。linear为calcMode的默认属性值。
calcMode="discrete"时，动画根据values和keyTimes属性，去掉过度动画，到了keyTimes的某个节点，属性值直接变为values对应数值。
calcMode="spline"时，需要配合keySplines属性，设置每个时间段内的三次贝塞尔变化曲线。
```sh
<circle cx="175" cy="75" r="20" fill="red">
    <animate attributeName="r"
             attributeType="XML"
             values="20;50;20"
             keyTimes="0;.15;1"
             calcMode="spline"  keySplines=".5 0 .5 1;.5 0 .5 1"
             begin="0" dur="1" repeatCount="indefinite"
             fill="freeze"></animate>
</circle>```
上例中加上贝塞尔曲线后圆形的变化有点类似心跳的节奏。

## &lt;set>
<set>元素也可设置属性变化的动画，但与<animate>有明显区别：

* 不需要from属性，起始状态即为父节点属性值。
* 一到begin属性设置的时点，指定的attributeName属性值即改为to指定的属性值，没有过度动画。
* attributeName可以是属性值非数字的属性，如style="visibility: hidden;"

```sh
<text text-anchor="middle" x="60" y="60" style="visibility: hidden;">
    <set attributeName="visibility" attributeType="CSS"
         to="visible" begin="1s" dur="10s" fill="freeze"></set>
    <set attributeName="x" attributeType="XML"
         to="120" begin="2s" dur="10s" fill="freeze"></set>
    All gone!
</text>```
上例中1秒后显示All gone!，再过1秒后文字移动至（120,60）。

## &lt;animateTransform>
要实现transform属性改变的动画，需要使用<animateTransform>来替代<animate>。
```sh
<rect x="-10" y="-10" width="20" height="20" style="fill: #ff9; stroke: black;">
    <animateTransform id="a1" attributeName="transform" attributeType="XML" type="scale" from="1" to="4 2" additive="sum" begin="0s" dur="4s" fill="freeze"></animateTransform>
    <animateTransform attributeName="transform" attributeType="XML" type="rotate" from="0" to="45" additive="sum" begin="a1.end" dur="4s" fill="freeze"></animateTransform>
</rect>```
<animateTransform>的attributeName指定为transform（个人感觉有点多余，animateTransform本身就是应用于改变transform属性值的）。用type属性指定transform需要改变的属性（translate, scale, rotate, skewX, skewY）。

<animateTransform>还有个additive属性。上例中两个<animateTransform>同时作用于一个<rect>元素，默认情况下additive属性值为replace，表示当前<animateTransform>的初始状态与之前的<animateTransform>变化结果无关。如果additive="sum"，表示当前<animateTransform>的变化基于之前的<animateTransform>变化之上。






(刚看到一个长方形使用rotate会变成一个菱形感觉哪里不对，再回想了一下translate的本质和形变的次序，就明了了)

## &lt;animateMotion>
<animateMotion>是个很强大的元素，之前所有动画功能在css里都可以用animation实现，但<animateMotion>能实现的功能是单凭css无法实现的。
<animateMotion>可以让父元素沿着指定的路径运动！
直线路径可以简单使用from + to属性来指定起点和终点：
```sh
<g>
    <rect x="0" y="0" width="50" height="30" style="fill: #ccc;"/>
    <circle cx="40" cy="30" r="10" style="fill: #cfc; stroke: green;"/>
    <circle cx="10" cy="30" r="10" style="fill: #cfc; stroke: green;"/>
    <animateMotion from="0,0" to="60,30" dur="4s" fill="freeze"/>
</g>```
也可以实用path指定复杂的路径
```sh
<g>
    <rect x="0" y="0" width="50" height="30" style="fill: #ccc;"/>
    <circle cx="40" cy="30" r="10" style="fill: #cfc; stroke: green;"/>
    <circle cx="10" cy="30" r="10" style="fill: #cfc; stroke: green;"/>
    <animateMotion path="M50,125 C 100,25 150,225, 200, 125" dur="4s" fill="freeze"/>
</g>```
也可以指定<path>元素作为自己的路径
```sh
<path id="cubicCurve" d="M50,125 C 100,25 150,225, 200, 125"/>
<g>
    <rect x="0" y="0" width="50" height="30" style="fill: #ccc;"/>
    <circle cx="40" cy="30" r="10" style="fill: #cfc; stroke: green;"/>
    <circle cx="10" cy="30" r="10" style="fill: #cfc; stroke: green;"/>
    <animateMotion dur="6s" fill="freeze">
        <mpath xlink:href="#cubicCurve"/>
    </animateMotion>
</g> ```
<animateMotion>有个rotate属性，默认为0，元素在运动时不会旋转。当设置为auto时，元素对应的水平轴会始终与path路径保持水平，上图中加上rotate="auto"后的效果就像是车子开过山坡。

## &lt;animateMotion>节点
与多节点动画类似，<animateMotion>也有三个属性用于控制动画的节点。
* <b>keyPoints</b>


第一个属性值为0（表示path路径的起始位置），最后一个属性值为1（表示path路径的终点位置），各属性值用分号分割，每个属性值与keyTimes的属性值一一对应，表示到某个时点运动到相对于路径的哪个位置。
* <b>keyTimes</b>


第一个属性值为0（表示动画起始时间），最后一个属性值为1（表示动画终止时间），各属性值用分号分割，每个属性值与keyPoints的属性值一一对应，表示运动到某个位置需要的总动画时常占比。
* <b>calcMode</b>


与多节点动画中的calcMode属性值及作用完全一致。
```sh
<path d="M-10,-3 L10,-3 L0,-25z" style="fill: yellow; stroke: red;" >
    <animateMotion
    path="M50,125 C 100,25 150,225, 200, 125"
    rotate="auto"
    keyPoints="0;0.2;0.8;1"
    keyTimes="0;0.33;0.66;1"
    calcMode="linear"
    dur="6s" fill="freeze"/>
</path>```
## CSS动画
css中有两种实现动画的方式：transition和animation。
transition用于实现较简单的两点动画，即根据属性的起始状态和终止状态，完整中间过渡动画。

### transition
transition动画相关的css属性如下
* <b>transition-property</b>


定义动画属性，可定义多个属性，逗号分隔
* <b>transition-duration</b>


定义动画执行时间，单位(s,ms)，可定义多个属性，与transition-property一一对应，逗号分隔
* <b>transition-timing-function</b>


与calcMode类似，影响动画的表现。默认值为ease。可选项有：ease | linear | ease-in | ease-out | ease-in-out | step-start | step-end | steps(<integer>[, [ start | end ] ]?) | cubic-bezier(<number>, <number>, <number>, <number>)
* <b>transition-delay</b>


定义动画延迟多少时间后开始执行

其中transition-timing-function中的step-start、step-end和steps有点类似calcMode中的discrete模式，状态之间切换没有中间过渡。具体来说stepts(<integer>[, [start | end]])的第一个参数必须为正整数，表示分几步变化，第二个参数设置为start时，表示在动画开始执行时就进行变化，设置为end表示在动画经过transition-duration设置的时间才开始变化，第二个参数允许省略，缺省值为end。step-start相当于steps(1, start)即属性一开始就变为最终状态；step-end相当于steps(1,end)即属性经过transition-duration设置的时间后立刻变为终止状态。
```sh
#car{
    transition-property:width, height;
    transition-duration:1s, 3s;
    transition-timing-function: ease-in, cubic-bezier(1,0,0,1);
    transition-delay:2s, 0.5s;
}```
可使用transition缩写简化上例中的css，直接从W3C规范里抄了下缩写组成部分。

```sh
transition:<single-transition> [ ‘,’ <single-transition> ]*

<single-transition> = [ none | <single-transition-property> ] || <time> || <single-transition-timing-function> || <time>```
第一个<time>对应transition-duration，第二个<time>对应transition-delay。
如果有一个<single-transition>里的第一个参数设置为none，则整个transition无效。也就是说none只能这么用：transition:none
```sh
#car{
    transition:width 1s ease-in 2s, height 3s cubic-bezier(1,0,0,1) .5s;
}```
### animation
animation可以说是transition的高级版，配合@keyframes可以实现对动画过程的多点控制。除了svg中延指定path路径运动的动画实现不了，其他都可以用animation来实现。其很多属性与svg的<animate>元素属性作用也能一一对应。
先看下animation的属性
* <b>animation-name</b>


对应<animate>的attributeName属性，属性值为@keyframes名对应，后面再说@keyframes
* <b>animation-duration</b>


对应<animate>的dur属性

* <b>animation-timing-function</b>


对应<animate>的calcMode属性，可用三次贝塞尔曲线来设置动画运行的效果
* <b>animation-iteration-count</b>


对应<animate>的repeatCount属性。默认为1。infinite表示无限重复（不同于repeatCount的indefinte）

* <b>animation-delay</b>


动画延迟执行时间。有点类似<animate>的start属性的作用，用start加上一些事件和时间的计算，也可以实现延迟执行动画的效果

* <b>animation-fill-mode</b>


类似<animate>的fill属性，但有区别。animation-fill-mode有四个属性值可选：none | forwards | backwards | both。
<pre>
 - **forwards**
 类似fill的freeze，动画结束后保持最后一帧时的状态。
 - **backwards**
 这个功能svg里好像没有。backwards是配合animation-delay一起使用的，如果动画延迟执行，那么在开始执行之前处于延迟的这段时间内，动画对象元素的默认状态是其原始状态，而非@keyframes的起始状态。当animation-fill-mode设置成backwards时，动画对象元素在延迟的这段时间内，将处于@keyframes设置的起始状态。
 - **both**
 相当于同时设置了forwards和backwards两个属性值。
 - **none**
 none是animation-fill-mode的默认属性值，即动画开始前和动画结束后，对象元素均处于其原始状态。</pre>
 * <b>animation-play-state</b>


animation-play-state顾名思义控制动画状态的，两个属性值也好理解。
<pre>
- **running**
表示让动画执行
- **paused**
表示暂停动画
可以通过改变这个属性来控制动画的播放和暂停，pause改为running后不会让元素重头再执行一遍动画，而是接着暂停时的状态继续变化。</pre>
* <b>animation-direction</b>


这个属性用于控制动画是正向执行，或者反向执行，如果设置了animation-iteration-count重复执行次数，还能控制一次动画执行完毕后，以何种方式执行接下去的一次动画。有四个属性值normal | reverse | alternate | alternate-reverse
<pre>
 - **normal**
 默认属性值，动画正向执行，重复执行动画时，每次都从起始状态开始变化。
 - **reverse**
 动画反向执行，即从指定的终止状态变化到起始状态，重复执行动画时，每次都从终止状态开始变化。其中animation-timing-function设置的运动曲线也会被颠倒，原来的ease-in会变成ease-out的效果。
 - **alternate**
 第一次执行动画时，从起始状态开始变化。重复执行动画时，单次动画结束后，下一次动画以当前状态作为起始状态开始变化，以上一次动画的起始状态作为该次动画的终止状态，相当于执行一次reverse的动画效果。
 - **alternate-reverse**
 和alternate类似，但是第一次执行动画时，从指定的结束状态开始向起始状态变化。</pre>
可以用animation来缩写以上所有属性：
```sh
animation:<single-animation> [ ‘,’ <single-animation> ]*

<single-animation> = <single-animation-name> || <time> || <single-animation-timing-function> || <time> || <single-animation-iteration-count> || <single-animation-direction> || <single-animation-fill-mode> || <single-animation-play-state>```
其中第一个<time>为animation-duration，第二个<time>为animation-delay。

再来看与animation紧密相关的@keyframes
@keyframes的作用类似values + keyTimes，可精细设置动画过程中每个变化节点。
@keyframes必须包含一个动画起始状态信息，和一个动画终止状态信息。可以用from + to，也可以用0% + 100%：
```sh
@keyframes rotate1{
    from {
        left: 0;
        top: 0;
    }
    to {
        left: 100px;
        top: 100px;
    }
}
@keyframes rotate2{
    0% {
        left: 0;
        top: 0;
    }
    100%{
        left: 100px;
        top: 100px;
    }
}```
以上两段代码等价，都表示起始状态的left和top属性为0，终止状态的left和top属性为100px。
对于中间动画节点，比如在动画进行到四分之一时，希望left为50px：
```sh
@keyframes rotate2{
    0% {
        left: 0;
        top: 0;
    }
    50%{
        left: 50px;
    }
    100%{
        left: 100px;
        top: 100px;
    }
}```
另外，还可以对每段动画的变化曲线做单独设置animation-timing-function属性，属性需要设置在起始状态信息中：
```sh
@keyframes rotate2{
    0% {
        left: 0;
        top: 0;
        animation-timing-function: ease-out;
    }
    50%{
        left: 50px; 
        animation-timing-function: ease-in;
    }
    100%{
        left: 100px;
        top: 100px;
    }
}```