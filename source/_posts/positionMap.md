---
title: 定位当前地理位置
---

#### 使用 HTML5 Geolocation API 来构建基于地理位置的应用～
各种浏览器对HTML5 Geolocation 的支持
浏览器 | 版本 | 只支持HTTPS版本

| 浏览器 | 版本 | 只支持HTTPS版本 |
| ---------------------- | --- | ---- |
| IE | 9+  | - |
| Edge | 12+  | - | 
| Firefox | 3.5+  | - | 
| Chrome | 5+  | 50 |
| Safari | 5+  | 39 |
| iOS Safari | 3.2+  | 10.2 |
| Android Browser | 2.1+  | 56 |
| Chrome for Android | 57+  | 5 |
| UC Browser for Android | 11.4+  | - | 


> 出于安全考虑，部分浏览器只允许通过HTTPS协议使用 Geolocation API。在HTTP协议下使用Geolocation API 浏览器会抛出异常，在开发阶段，127.0.0.1和localhost 等本地域在两种协议下均可使用。

Geolocation API 通过 navigator.geolocation 全局对象进行访问，第一次访问的时候会询问用户是否允许共享位置。

判断浏览器是否支持 Geolocation API
```
        // 判断浏览器属否支持获取位置
        if(navigator.geolocation){
            console.log("可以获取");
        }else{
            console.log("不支持");
        }
```
实例代码如下：
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    获取用户地理位置
    <input type="button" value="点击获取位置" id="btn">
    <script>
        let btnBtn = document.getElementById('btn')
        btnBtn.onclick = () => { // 点击事件
                getAdd()
            }
            // 成功回调
        let success = (position) => {
                console.log(`获取位置成功：${position.coords}`);
                console.log(position.coords); // 获取坐标信息
                // coords的常用属性
                console.log(position.coords.latitude); // 获取坐标纬度
                console.log(position.coords.longitude); // 获取坐标经度
                console.log(position.coords.accuracy); // 获取坐标精度，单位为米
                console.log(position.timestamp); // 获取位置的时间戳

            }
            // 失败回调
        let error = (positionErr) => {
            console.log(`获取位置失败：${positionErr.code}+${positionErr.message}`);
        }

        let options = {
            enableHightAccuracy: false, // 获取高精度的位置信息，可能会增加响应时间，默认是false
            timeout: 30000, // 设置超时时间，单位毫秒，如果到达设定的时间没获取到信息则回触发失败回调，默认值为0，无限大
            maximumAge: 0 // 设置用户位置信息缓存的最大时间
        }
        let getAdd = () => {
            navigator.geolocation.getCurrentPosition(success, error, options)
        }


        // 判断浏览器属否支持获取位置
        if(navigator.geolocation){
            console.log("可以获取");
        }else{
            console.log("不支持");
        }
    </script>
</body>

</html>
```
当获取位置失败时，会调用失败回调(error函数)。返回的参数<positionErr.code 标识错误的原因><positionErr.message错误信息描述>
positionErr.code 值
- UNKNOWN_ERROR(0): 其他错误
- PERMISSION_DENIED(1): 用户拒绝分享位置信息
- POSITION_UNAVAILABLE(2): 获取用户位置信息失败
- TIMEOUT(3): 获取用户位置信息超时


