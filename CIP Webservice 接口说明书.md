# CIP Webservice 接口说明书
Revison History

| Version   | Date   | Description   | Publisher   | 
|:----|:----|:----|:----|
| 0.01   | 2019.08.28   | 1st version   | liaodingbai   | 
|    |    |    |    | 

1. 使用说明

对服务接口调用HTTP API 接口，图片路径支持 HTTP POST方法发送请求，其他处理请求参数需要包含在请求的URL中，某些请求工具不支持接口参数中的特殊字符，可以使用浏览器进行调用测试。根据请求的处理情况，系统响应数据格式统一为 JSON 数据格式或者图片数据。

**公用请求Header信息**

| 字段   | 值   | 说明   | 
|:----:|:----:|:----|
| Content-Type | application/x-www-form-urlencoded | 内容类型   | 

**POST请求参数信息**

| 字段   | 类型 | 必选   | 说明   | 
|:----:|:----:|:----|:----:|
| dtype | int | Y   | 0:图片url, 1:图片base64编码字符串 | 
| image_url   | string   | N   | 图片url   | 
| content   | string   | N   | 图片base64编码字符串   | 

测试原图如下，大小为1024x768的JPEG文件：

![图片](https://uploader.shimo.im/f/jWwYmCk3alEabPVV.png!thumbnail)

2. 图片基本处理

imageView2 是图片基本处理的接口，简单，功能强大的图像处理接口，包括裁剪、压缩、格式转换等。

## imageView2

>注意： 请忽略以下内容中的空格与换行符。

imageView2/<mode>/w/<LongEdge>

                    /h/<ShortEdge>

                    /format/<Format>

                    /q/<Quality>

                    /fpga/<Fpga>

## 参数说明

| Item   | Value   | Description   | Optional   | 
|:----|:----|:----|:----|
| imageView2/   | 0, 1, 2, 3, 4, 5   | 参考mode参数详细说明， 包括一下w和h参数说明。   | M   | 
| /format/   | jpg，png，webp等， 默认为原图输出格式   | 新图的输出格式   | O   | 
| /q/   | 取值范围1-100， 默认值75   | 新图的输出质量   | O   | 
| /fpga/   | 是否使用cip加速， 默认为true   | 使用fpga加速卡   | O   | 

关于mode说明：

| mode   | description   | 
|:----|:----|
| /0/w/<LongEdge>/h/<ShortEdge>   | 限定缩略图的长边最多为<LongEdge>，短边最多为<ShortEdge>，进行等比缩放，不裁剪。如果只指定 w 参数则表示限定长边（短边自适应），只指定 h参数则表示限定短边（长边自适应）。   | 
| /1/w/<Width>/h/<Height>   | 限定缩略图的宽最少为<Width>，高最少为<Height>，进行等比缩放，居中裁剪。转后的缩略图通常恰好是 <Width>x<Height> 的大小（有一个边缩放的时候会因为超出矩形框而被裁剪掉多余部分）。如果只指定 w 参数或只指定 h参数，代表限定为长宽相等的正方图。   | 
| /2/w/<Width>/h/<Height>   | 限定缩略图的宽最多为<Width>，高最多为<Height>，进行等比缩放，不裁剪。如果只指定 w 参数则表示限定宽（长自适应），只指定 h 参数则表示限定长（宽自适应）。它和模式0类似，区别只是限定宽和高，不是限定长边和短边。从应用场景来说，模式0适合移动设备上做缩略图，模式2适合PC上做缩略图。   | 
| /3/w/<Width>/h/<Height>   | 限定缩略图的宽最少为<Width>，高最少为<Height>，进行等比缩放，不裁剪。如果只指定 w 参数或只指定 h参数，代表长宽限定为同样的值。你可以理解为模式1是模式3的结果再做居中裁剪得到的。   | 
| /4/w/<LongEdge>/h/<ShortEdge>   | 限定缩略图的长边最少为<LongEdge>，短边最少为<ShortEdge>，进行等比缩放，不裁剪。如果只指定 w 参数或只指定 h 参数，表示长边短边限定为同样的值。这个模式很适合在手持设备做图片的全屏查看（把这里的长边短边分别设为手机屏幕的分辨率即可），生成的图片尺寸刚好充满整个屏幕（某一个边可能会超出屏幕）。   | 
| /5/w/<LongEdge>/h/<ShortEdge>   | 限定缩略图的长边最少为<LongEdge>，短边最少为<ShortEdge>，进行等比缩放，居中裁剪。如果只指定 w 参数或只指定 h 参数，表示长边短边限定为同样的值。同上模式4，但超出限定的矩形部分会被裁剪。   | 

## 示例

### mode=0, w/400/h400变换成 400x300

POST /api/v1?imageView2/0/w/400/h/400

![图片](https://uploader.shimo.im/f/pYvEkIvRy2sgOZOm.png!thumbnail)

### mode=1, w/400/h/400变换成 400x400

POST /api/v1?imageView2/1/w/400/h/400

![图片](https://uploader.shimo.im/f/r5XxIU3vp6UV0B3g.png!thumbnail)

### mode=2, w/400/h/400变换成400x300 

POST /api/v1?imageView2/2/w/400/h/400

![图片](https://uploader.shimo.im/f/oUxbUZxIOF80fhsb.png!thumbnail)

### mode=3, w/400/h/400变换成533x400

POST /api/v1?imageView2/3/w/400/h/400

![图片](https://uploader.shimo.im/f/laVkEK89L6ERENK5.png!thumbnail)

### mode=4, w/400/h/400变换成533x400

POST /api/v1?imageView2/4/w/400/h/400

![图片](https://uploader.shimo.im/f/AiEmJhtppioRqJLc.png!thumbnail)

### mode=5, w/400/h/400变换成400x400

POST /api/v1?imageView2/5/w/400/h/400

![图片](https://uploader.shimo.im/f/T6ZhvfhYFrgHhuXu.png!thumbnail)

### mode=0, w/400, 表示限定长边为400， 短边自适应。变换成400x300的图。

POST /api/v1?imageView2/0/w/400

![图片](https://uploader.shimo.im/f/vW5KA7DtEkslARIy.png!thumbnail)

### mode=1, w/400，只指定w或h， 按比例缩放， 然后按中心裁剪为长宽相等的正方图， 变换成400x400的图。

POST /api/v1?imageView2/1/w/400

![图片](https://uploader.shimo.im/f/vnnJAGbj9tcUNjla.png!thumbnail)

### mode=2, w/400， 宽最多为400， 高度自适应， 变换成400x300的图。

POST /api/v1?imageView2/2/w/400

![图片](https://uploader.shimo.im/f/wqaocTsCas0u4bYz.png!thumbnail)

### mode=3, w/400， 宽至少为400， 高也至少为400， 变换成533x400的图。

POST /api/v1?imageView2/3/w/400

![图片](https://uploader.shimo.im/f/BjoGpYKDbBAhQqRv.png!thumbnail)

### mode=4, w/400， 长边至少为400， 短边至少为400， 进行等比缩放。变换成533x400的图。

POST /api/v1?imageView2/4/w/400

![图片](https://uploader.shimo.im/f/mGpvKJK3qdI0Yff7.png!thumbnail)

### mode=5, w/400， 长边至少为400， 短边至少为400， 进行等比缩放。居中裁剪为400x400的图。 

POST /api/v1?imageView2/5/w/400

![图片](https://uploader.shimo.im/f/7dhSbrDijY42AB8R.png!thumbnail)

### mode=1, w/400/h/400/format/png/q/70/fpga/false, 居中裁剪为400x400的图。输出格式为png。

POST /api/v1?imageView2/1/w/400/h/400/format/png/q/70/fpga/false

![图片](https://uploader.shimo.im/f/k3kl14xLDHgN5BwL.png!thumbnail)

3. 图片高级处理
## imageMogr2

>注意： 请忽略以下内容中的空格与换行符。

imageMogr2/auto-orient

            /thumbnail/<imageSizeGeometry>

            /gravity/<gravityType>

            /crop/<imageSizeAndOffsetGeometry>

            /rotate/<rotateDegree>

            /format/<destinationImageFormat>

            /blur/<radius>x<sigma>

            /background/<ecodedBackgroundColor>

      		/quality/<quality>

            /sharpen/<sharpen>

            /roundpic/<radiusX>x<radiusY>

    	    /iradius/<iradius>

## 参数说明

| Item   | Value   | Description   | Optional   | 
|:----|:----|:----|:----|
| /auto-orient   | 无   | 建议放在首位，根据原图EXIF信息自动旋正，便于后续处理。   | O   | 
| /thumbnail/   | -   | 参考 [thumbnail说明](https://github.com/ctaccel/cip-docs/blob/master/image_advanced_process.md#Thumbnail)   | O   | 
| /gravity/   | -   | 参考 [gravity说明](https://github.com/ctaccel/cip-docs/blob/master/image_advanced_process.md#Gravity)   | O   | 
| /crop/   | -   | 参考 [crop说明](https://github.com/ctaccel/cip-docs/blob/master/image_advanced_process.md#Crop)   | O   | 
| /rotate/   | 取值范围为1-360，默认为不旋转   | 图片旋转角度   | O   | 
| /format/   | 支持jpg、gif、png、webp等，默认为原图格式   | 新图的输出格式   | O   | 
| /blur/x   | radius是模糊半径，取值范围为1-50。sigma是正态分布的标准差，必须大于0。   | 高斯模糊参数   | O   | 
| /background/   | -   | 背景填充颜色,参考color说明   | O   | 
| /quality/   | 取值范围1-100， 默认值75   | 新图的输出质量   | O   | 
| /sharpen/   | 锐化参数值，取值范围为10-300间的整数。参数值越大，锐化效果越明显。   | 图片是否锐化   | O   | 
| /roundpic/x   | 水平和垂直的值相同， 则可以写成类似/roundpic/200这样， 否则/roundpic/200x300。 支持/roundpic/!50p和/roundpic/50p这种形式， 意思是原图的宽和高的50%作为radiusX和radiusY值。   | 圆角大小的参数   | O   | 
| /iradius/   | radius是内切圆的半径，取值范围为大于0、小于原图最小边一半的整数。内切圆的圆心为图片的中心。图片格式为gif时，不支持该参数。   | 内切圆裁剪功能   | O   | 

**thumbnail说明**

| Item   | Description   | 
|:----|:----|
| /thumbnail/!<Scale>p   | 指定图片的宽高为原图的 Scale%。   | 
| /thumbnail/!<Scale>px   | 指定图片的宽为原图的 Scale% ，高度不变。   | 
| /thumbnail/!x<Scale>p   | 指定图片的高为原图的 Scale% ，宽度不变。   | 
| /thumbnail/<Width>x   | 指定目标图片宽度为 Width ，高度等比压缩。   | 
| /thumbnail/x<Height>   | 指定目标图片高度为 Height ，宽度等比压缩。   | 
| /thumbnail/<Width>x<Height>   | 限定缩略图的宽度和高度的最大值分别为 Width 和 Height，进行等比缩放。   | 
| /thumbnail/!<Width>x<Height>r   | 限定缩略图的宽度和高度的最小值分别为 Width 和 Height，进行等比缩放 。   | 
| /thumbnail/<Width>x<Height>!   | 忽略原图宽高比例，指定图片宽度为 Width ，高度为 Height ，强行缩放图片，可能导致目标图片变形。   | 
| /thumbnail/<Area>@   | 等比缩放图片，缩放后的图像，总像素数量不超过 Area。   | 

**gravity说明**

图片处理重心参数表 在图片高级处理现有的功能中只影响其后的裁剪操作参数表，即裁剪操作以 gravity 为原点开始偏移后，进行裁剪操作。 

![图片](https://uploader.shimo.im/f/4Dc9J4fdCKM2yJbM.png!thumbnail)**crop说明**

裁剪操作参数表 (cropsize)

| 参数名称   | 必填   | 说明   | 
|:----|:----|:----|
| /crop/<Width>x   |    | 指定目标图片宽度，高度不变。取值范围为0-10000。   | 
| /crop/x<Height>   |    | 指定目标图片高度，宽度不变。取值范围为0-10000。   | 
| /crop/<Width>x<Height>   |    | 同时指定目标图片宽高。取值范围为0-10000。   | 

裁剪偏移参数表 (cropoffset)

| 参数名称   | 必填   | 说明   | 
|:----|:----|:----|
| /crop/!{cropsize}a<dx>a<dy>   |    | 相对于偏移锚点，向右偏移dx个像素，同时向下偏移dy个像素。取值范围不限，小于原图宽高即可。   | 
| /crop/!{cropsize}-<dx>a<dy>   |    | 相对于偏移锚点，从指定宽度中减去dx个像素，同时向下偏移dy个像素。取值范围不限，小于原图宽高即可。   | 
| /crop/!{cropsize}a<dx>-<dy>   |    | 相对于偏移锚点，向右偏移dx个像素，同时从指定高度中减去dy个像素。取值范围不限，小于原图宽高即可。   | 
| /crop/!{cropsize}-<dx>-<dy>   |    | 相对于偏移锚点，从指定宽度中减去dx个像素，同时从指定高度中减去dy个像素。取值范围不限，小于原图宽高即可。   | 

**color说明**

可以是颜色名称（比如red）或十六进制颜色（比如#FF0000）的URL安全的Base64编码。我们支持的颜色名称有transparent（#00000000）、none（#00000000）、white（#FFFFFF）、black（#000000）、red（#FF0000）、orange（#FFA500）、yellow（#FFFF00）、green（#008000）、blue（#0000FF）、purple（#800080）、gray（#7E7E7E）、pink（#FFC0CB），其中none与transparent均为透明背景色，另外十六进制颜色不区分大小写，具体颜色请参考颜色编码表。缺省背景色为white（#FFFFFF）。

### 
## 示例

### 缩略图

* 等比缩小75%

POST /api/v1?imageMogr2/thumbnail/!75p

![图片](https://uploader.shimo.im/f/4UYOOns6BDwAoIp1.png!thumbnail)

* 按原宽度75%等比缩小， 高度不变。

POST /api/v1?imageMogr2/thumbnail/!75px

* 按原高度75%等比缩小， 宽度不变。

POST /api/v1?imageMogr2/thumbnail/!x75p

* 指定新宽度 1536px， 等比缩放。

POST /api/v1?imageMogr2/thumbnail/1536x

![图片](https://uploader.shimo.im/f/EHlCvgRyw5UyqQnb.png!thumbnail)

* 指定新高度 1152px， 等比缩放。

POST /api/v1?imageMogr2/thumbnail/x1152

![图片](https://uploader.shimo.im/f/RO7L3FzpPMoqhc2S.png!thumbnail)

* 限定长边， 生成不超过 400x400 的缩略图

POST /api/v1?imageMogr2/thumbnail/400x400

![图片](https://uploader.shimo.im/f/5OHnA8ySQgwPLEH6.png!thumbnail)

* 限定短边， 生成不小于 400x400的缩略图

POST /api/v1?imageMogr2/thumbnail/!400x400r

![图片](https://uploader.shimo.im/f/eW112Zg4NUogaw5e.png!thumbnail)

* 强制生成400x600的图

POST /api/v1?imageMogr2/thumbnail/400x600!

![图片](https://uploader.shimo.im/f/Qz0KHPezIdE0Jv6L.png!thumbnail)

* 原图大于指定长宽矩形， 按宽缩放比和高缩放比的较大值进行缩放成400x300。

POST /api/v1?imageMogr2/thumbnail/400x600>

![图片](https://uploader.shimo.im/f/RqzUddVtiRUjgXkn.png!thumbnail)

* 原图小于指定长宽矩形， 按宽缩放比和高缩放比的较小值缩放成1467x1100。

POST /api/v1?imageMogr2/thumbnail/1500x1100<

![图片](https://uploader.shimo.im/f/7PFtbgMQo9QvWnKT.png!thumbnail)

* 按原图高宽比例等比缩放， 缩放后的像素数量不超过900000。

POST /api/v1?imageMogr2/thumbnail/900000@

![图片](https://uploader.shimo.im/f/PLzHKtFDoVg7iRJg.png!thumbnail)

### 裁剪

* 生成 400x768裁剪图

POST /api/v1?imageMogr2/crop/400x

![图片](https://uploader.shimo.im/f/mEQzdHyJkqU35x2u.png!thumbnail)

* 生成1024x400的裁剪图

POST /api/v1?imageMogr2/crop/x400

![图片](https://uploader.shimo.im/f/FO1LzLxkQekjWA07.png!thumbnail)

* 生成400x400的裁剪图

POST /api/v1?imageMogr2/crop/400x400

![图片](https://uploader.shimo.im/f/r4JmYWTFgqs7M2Ry.png!thumbnail)

* 生成300x300裁剪图， 偏移距离30x100

POST /api/v1?imageMogr2/crop/!400x400a30a100

![图片](https://uploader.shimo.im/f/vhHEYTq0XiE7DWla.png!thumbnail)

* 生成400x300裁剪图， 偏移距离30x0

POST /api/v1?imageMogr2/crop/!400x400a30-100

![图片](https://uploader.shimo.im/f/Es36Hw9X328kcMSJ.png!thumbnail)

* 生成370*400裁剪图，偏移距离0x100

POST /api/v1?imageMogr2/crop/!400x400-30a100

![图片](https://uploader.shimo.im/f/uLuWoJ8ERmI6R1Ha.png!thumbnail)

* 生成370x300裁剪图， 偏移距离0x0

POST /api/v1?imageMogr2/crop/!400x400-30-100

![图片](https://uploader.shimo.im/f/yKFMTpeDwY83OgBc.png!thumbnail)

* 锚点在左上角（northwest）， 生成300x300裁剪图

POST /api/v1?imageMogr2/gravity/northwest/crop/400x400

![图片](https://uploader.shimo.im/f/SfLeMdITx4YdrzVv.png!thumbnail)

* 锚点在正上方（north）， 生成400x400裁剪图

POST /api/v1?imageMogr2/gravity/north/crop/400x400

![图片](https://uploader.shimo.im/f/MkmMxNqasJwtNPsd.png!thumbnail)

* 锚点在右上方（northeast）， 生成400x400裁剪图

POST /api/v1?imageMogr2/gravity/northeast/crop/400x400

![图片](https://uploader.shimo.im/f/qUedWlN6YZsPRmYc.png!thumbnail)

* 锚点在正左方（west），　生成400x400裁剪图

POST /api/v1?imageMogr2/gravity/west/crop/400x400

![图片](https://uploader.shimo.im/f/czJldRvn9dk6htc1.png!thumbnail)

* 锚点在正中（center），　生成400x400裁剪图

POST /api/v1?imageMogr2/gravity/center/crop/400x400

![图片](https://uploader.shimo.im/f/SbX70My0h5Mx1fgW.png!thumbnail)

* 锚点在正右方 （east）， 生成400x400裁剪图

POST /api/v1?imageMogr2/gravity/east/crop/400x400

![图片](https://uploader.shimo.im/f/GgjWH29V1gEKzNdT.png!thumbnail)

* 锚点在左下方 （southwest）， 生成400x400裁剪图

POST /api/v1?imageMogr2/gravity/southwest/crop/400x400

![图片](https://uploader.shimo.im/f/iMoEw6UzF0sQdqgE.png!thumbnail)

* 锚点在正下方 （south）， 生成400x400裁剪图

POST /api/v1?imageMogr2/gravity/south/crop/400x400

![图片](https://uploader.shimo.im/f/lCq2q6vcQT0ql9ZY.png!thumbnail)

* 锚点在右下方 （southeast）， 生成400x400裁剪图

POST /api/v1?imageMogr2/gravity/southeast/crop/400x400

![图片](https://uploader.shimo.im/f/LSXgrDlDsyw4AafQ.png!thumbnail)

### 旋转

* 顺时针旋转45度

POST /api/v1?imageMogr2/rotate/45

![图片](https://uploader.shimo.im/f/XIZiBesUipgSTmte.png!thumbnail)

* 旋转并添加背景色，裁剪以及高斯模糊，输出质量为80%

POST /api/v1?imageMogr2/auto-orient/thumbnail/!256x256r/background/b3Jhbmdl/gravity/center/crop/256x256/blur/3x9/quality/80/rotate/45

![图片](https://uploader.shimo.im/f/hFJCEcuBQZU8ltfx.png!thumbnail)

### 高斯模糊

* 半径为5， sigma值为9。

POST /api/v1?imageMogr2/blur/5x9

![图片](https://uploader.shimo.im/f/jZ7Q7nYQNysDJyaN.png!thumbnail)

### 椭圆圆角

* 水平方向和垂直方向都为300像素

POST /api/v1?imageMogr2/roundpic/300

![图片](https://uploader.shimo.im/f/oDpsmagiTDAxrrZL.png!thumbnail)

* 水平方向为300px，　垂直方向为400px

POST /api/v1?imageMogr2/roundpic/300x400

![图片](https://uploader.shimo.im/f/aN9LJ6AZ4VYhG91n.png!thumbnail)

* 水平方向和垂直方向都为原图片的30%大小

POST /api/v1?imageMogr2/roundpic/!30p

![图片](https://uploader.shimo.im/f/lZnbt74Bb9IdAbE1.png!thumbnail)

### 中心原图

* 中心原图半径为300px

POST /api/v1?imageMogr2/iradius/300

![图片](https://uploader.shimo.im/f/M3p8frOG9SIL5zvR.png!thumbnail)

* 中心原图直径为原宽高最小值的60%

POST /api/v1?imageMogr2/iradius/!60p

![图片](https://uploader.shimo.im/f/NPqYfmB8sN8jhOoq.png!thumbnail)

4. 添加水印

image watermark 是将logo图片或者文字叠加到源图片上的功能。提供四种水印接口：图片水印、文字水印，文字平铺水印、混合水印。

## 图片水印

>注意： 请忽略以下内容中的空格与换行符。

watermark/1

         /image/<encodedImageURL>

         /dissolve/<dissolve>

         /gravity/<gravity>

         /dx/<distanceX>

         /dy/<distanceY>

         /ws/<watermarkScale>

         /wst/<watermarkScaleType>

### 参数说明

| 参数名称   | 说明   | 
|:----|:----|
| /image/<encodedImageURL>   | 水印源图片网址（经过URL安全的Base64编码），必须有效且返回一张图片。   | 
| /dissolve/<dissolve>   | 透明度，取值范围1-100，默认值为100（完全不透明）。   | 
| /gravity/<gravity>   | 水印位置，默认值为SouthEast（右下角）。   | 
| /dx/<distanceX>   | 横轴边距，单位:像素(px)，默认值为10。   | 
| /dy/<distanceY>   | 纵轴边距，单位:像素(px)，默认值为10。   | 
| /ws/<watermarkScale>   | 水印图片自适应原图的短边比例，ws的取值范围为0-1。具体是指水印图片保持原比例，并短边缩放到原图短边＊ws。   | 
| /wst/<watermarkScaleType>   | 水印图片自适应原图的类型，取值0、1、2、3分别表示为自适应原图的短边、长边、宽、高，默认值为0   | 

例如：原图大小为250x250，水印图片大小为91x61，如果ws=1，那么最终水印图片的大小为：372x250。

### 示例

POST /api/v1?watermark/1/image/aHR0cDovL3d3dy5wbmdhbGwuY29tL3dwLWNvbnRlbnQvdXBsb2Fkcy8yMDE2LzA1L1B5dGhvbi1Mb2dvLUZyZWUtRG93bmxvYWQtUE5HLnBuZw==/dissolve/80/gravity/center/dx/200/dy/100/ws/1/wst/0

![图片](https://uploader.shimo.im/f/x2JEDlN4ztME9EqB.jpeg!thumbnail)

## 文字水印

>注意： 请忽略以下内容中的空格与换行符。

watermark/2

         /text/<encodedText>

         /font/<encodedFontName>

         /fontsize/<fontSize>

         /fill/<encodedTextColor>

         /dissolve/<dissolve>

         /gravity/<gravity>

         /dx/<distanceX>

         /dy/<distanceY>

### 参数说明

| 参数名称   | 说明   | 
|:----|:----|
| /text/   | 水印文字内容（经过[URL安全的Base64编码）   | 
| /font/   | 水印文字字体（经过URL安全的Base64编码），默认为黑体   | 
| /fontsize/   | 水印文字大小，单位: 缇 ，等于1/20磅，默认值是240缇，参考DPI为72。   | 
| /fill/   | 水印文字颜色，RGB格式，可以是颜色名称（例如 red）或十六进制（例如 #FF0000）。默认为黑色。经过URL安全的Base64编码。   | 
| /dissolve/   | 透明度，取值范围1-100，默认值为100（完全不透明）。   | 
| /gravity/   | 水印位置，默认值为SouthEast（右下角）。   | 
| /dx/   | 横轴边距，单位:像素(px)，默认值为10。   | 
| /dy/   | 纵轴边距，单位:像素(px)，默认值为10。   | 

### 示例

POST /api/v1?watermark/2/text/5rWLQ1RBYWFhVHh0/dy/50/dx/50/fill/UmVk/dissolve/50/fontsize/900/gravity/center

![图片](https://uploader.shimo.im/f/JAcbCAHSjrMrDeSX.jpeg!thumbnail)

## 文字平铺水印

>注意： 请忽略以下内容中的空格与换行符。

watermark/4

         /text/<encodedText>

         /font/<encodedFontName>

         /fontsize/<fontSize>

         /fill/<encodedTextColor>

         /dissolve/<dissolve>

         /rotate/<rotate>

         /uw/<unitW>

         /uh/<unitH>

         /resize/<resize>

### 参数说明

| 参数名称   | 说明   | 
|:----|:----|
| /text/   | 水印文字内容（经过[URL安全的Base64编码）   | 
| /font/   | 水印文字字体（经过URL安全的Base64编码），默认为黑体   | 
| /fontsize/   | 水印文字大小，单位: 缇 ，等于1/20磅，默认值是240缇，参考DPI为72。   | 
| /fill/   | 水印文字颜色，RGB格式，可以是颜色名称（例如 red）或十六进制（例如 #FF0000）。默认为黑色。经过URL安全的Base64编码。   | 
| /dissolve/   | 透明度，取值范围1-100，默认值为100（完全不透明）。   | 
| /rotate/   | 水印文字旋转角度，[-180, 180]，默认为０。   | 
| /uw/   | 水印文字填充单元宽度，默认值为 100。   | 
| /uh/   | 水印文字填充单元高度，默认值为 100。   | 
| /resize/   | 水印文字填充单元缩放比例，[0.1, 10]，　默认为１ （不缩放）。   | 

### 示例

POST /api/v1?watermark/4/text/5rWLQ1RBYWFhVHh0/uw/160/resize/1/uh/50/fill/UmVk/rotate/180/fontsize/500/font/5b6u6L2v6ZuF6buR/dissolve/100

![图片](https://uploader.shimo.im/f/rOKyjadJTewM2HgV.jpeg!thumbnail)

## 混合水印

混合水印用于同时在一个原图上打多个不同类型的水印。

>注意： 请忽略以下内容中的空格与换行符。

watermark/３

         /text/<textWaterMarkParams1>

         /image/<imageWaterMarkParams1>

         /image/<imageWaterMarkParams2>

         /text/<textWaterMarkParams2>

### 参数说明

| 参数名称   | 说明   | 
|:----|:----|
| /image/<imageWaterMarkParams>   | 参考图片水印参数   | 
| /text/<textWaterMarkParams>   | 参考文字水印参数   | 

### 示例

POST /api/v1?imageMogr2/auto-orient/format/png|watermark/3/image/aHR0cDovLzd4bHY0Ny5jb20wLnowLmdsYi5jbG91ZGRuLmNvbS94aWFvamkucG5n/gravity/North/dy/-10/dx/0/text/5ZCD6L-H54yr5bGx546L77yM5YW25LuW5qa06I6y55qG6Lev5Lq6/gravity/SouthWest/dx/10/dy/180/fontsize/500/text/5LuF6ZmQN-WkqSAgMjAxOS4wNC4wMS0yMDE5LjA0LjA3/gravity/SouthWest/dx/30/dy/130/fontsize/300/image/aHR0cDovLzd4bHY0Ny5jb20wLnowLmdsYi5jbG91ZGRuLmNvbS9xdWFuLnBuZw==/gravity/SouthWest/dx/80/dy/30/image/aHR0cDovLzd4bHY0Ny5jb20wLnowLmdsYi5jbG91ZGRuLmNvbS_kuoznu7TnoIEucG5n/gravity/SouthEast/dx/10/dy/30/text/5omr56CB6aKG5Y-W5LyY5oOg5Yi4/gravity/SouthEast/dx/20/dy/10/fontsize/300/fill/UmVk

注意：　image_url = http://7xlv47.com0.z0.glb.clouddn.com/baidi.png

![图片](https://uploader.shimo.im/f/BWXtUVberKwbpck4.png!thumbnail)


5. 图片基本信息

图片基本信息包括图片格式、图片大小、色彩模型。

## imageInfo

## 参数说明

| Item   | Value   | Description   | Optional   | 
|:----|:----|:----|:----|
| -   | -   | -   | -   | 

## JSON返回值说明

| name   | description   | 
|:----|:----|
| size   | 文件大小，单位：Bytes   | 
| format   | 图片类型，如png、jpeg、bmp等。   | 
| width   | 图片宽度，单位：像素(px)。   | 
| height   | 图片高度，单位：像素(px)。   | 

## 示例

POST /api/v1?imageInfo

| {      "format": "jpeg",      "height": 768,      "size": 135455,      "width": 1024  }   | 
|----|
6. 图片主色调

imageAve接口用于计算一幅图片的平均色调，并以0xRRGGBB形式返回。

## imageAve

## 参数说明

| Item   | Value   | Description   | Optional   | 
|:----|:----|:----|:----|
| -   | -   | -   | -   | 

## JSON返回值说明

| name   | description   | 
|:----|:----|
| RGB   | 16进制颜色值   | 

## 示例

POST /api/v1?imageAve

| {      "RGB": "0x767b7f"  }   | 
|----|
1. 常见问题
1. 错误码
