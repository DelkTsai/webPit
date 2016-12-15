# webPit
这里收集了许多web上遇到的各种坑与相对解决方案

##移动开发事件
[填坑一：ajax传递给后台数组参数方式(同样适用移动端)](https://github.com/sosout/webPit/blob/master/demos/201612151116.html)

```js
// 这里代码依赖jQuery v1.12.1
$(function() {
    var params = {
        name: 'admin',
        address: {
            province: '浙江',
            city: '杭州'
        },
        fids: [1, 2],
        friends: [{
            name: '张三',
            age: 10
        }, {
            name: '李四',
            age: 15
        }]
    };
    $.ajax({
        url: '后台接口地址',
        type: 'GET',
        data: $.param(serializeObjects(params)),
        traditional: true,
        success: function(result) {},
        error: function() {
            console.log('error');
        }
    });
    // 对参数进行特殊转化
    function serializeObjects(params) {
        var obj = {};
        for (var k in params) {
            var o = params[k];
            if ('[object Array]' === Object.prototype.toString.call(o))
                for (var i = 0; i < o.length; i++) {
                    var o1 = o[i];
                    if ('[object Object]' === Object.prototype.toString.call(o1))
                        for (var k1 in o1) obj[(k + '[' + i + '].' + k1).toString()] = o1[k1];
                    else obj[(k + '[' + i + ']').toString()] = o1;
                } else if ('[object Object]' === Object.prototype.toString.call(o))
                    for (var k2 in o) obj[(k + '.' + k2).toString()] = o[k2];
                else obj[k.toString()] = o;
        }
        return obj;
    }
})
```
