# xss_jsfirewall
### 前端xss防火墙

#### 整个代码的思想是EherDream大牛在14年写的5篇xss前端防火墙的文章，在我们都在用正则去防御xss的时候，他就提出在前端利用前端的特性去防御，思想是领先时代的
#### 博客链接（http://www.cnblogs.com/index-html/p/xss-frontend-firewall-1.html）
#### 代码是参考另一位写的，这位作者也是参考EherDream的思想，相当于一个优化吧，git地址（https://github.com/chokcoco/httphijack）
#### 不少代码是都是把EherDream的demo直接拿过来用的，所以代码都差不多的，就是细节地方的处理不同
#### 代码的解释很详细了
#### 整个写下来发现前端防御的确有它的好处，也有局限性
#### 因为防御方法是把前端的各种函数勾住或者监听页面变化，取出值来判断，所以这里涉及到一个脚本的好坏的问题，如何把一个正常的脚本，和一个攻击的脚本区分开
#### 这个貌似想辨识度很高的区分开还是很困难的，但是按照xss的目的，可以有个思路,xss的目的有两种：1.获取到某些信息（窃取cookie或者password） 2.模拟某些操作(类似蠕虫或者利用xssgetshell)
#### 对于第一种，可以拦截请求，这里拦截了ajax,websocket,PostMessage这三种跨域发送请求的方式，验证目的地址是否是白名单，提一句ajax不支持跨域，但是它可以把消息发出去，只是收不到回复，所以也要拦截
#### 对于第二种，感觉还没有很好的方法区分，因为用js模拟用户操作正常的脚本也会，而攻击脚本也是模拟而已，也没有什么特别能跟正常操作区分开的地方，所以这里暂时只能用关键词黑名单匹配

#### 最后还有一点局限性就是虽然前端能获取到事件和脚本，但是如果攻击者利用'/"/script标签，成功的绕过了，那前端捕获到的就不准确了，比如
```markdown
`<img src="xxxx">`
```
#### 其中xxxx为我们可控的如果攻击者利用双引号逃逸了输入" onmouseover=“javascript:alert(1);
```markdown
`<img src="" onmouseover=“javascript:alert(1);”>`
```
#### 这样我们去取src中的值就是空，无法正确认为用户输入是"onmouseover=“javascript:alert(1);  虽然我们的脚本也可以拦截这种，因为拦截了事件
#### 所以这里最好是前后端配合起来用
#### 后端只用转义符号" ' < > ( ) /等等就ok了
#### 然后繁杂的各种on事件啊，各种请求，各种动态脚本都交给前端来处理
#### 这样可以前端后端各自发挥自己的优势
