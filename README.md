# Lanshan寒假作业

## 1.拆分作业
  
  1. 第一个页面
  -  顶部：推荐歌单，推荐歌单列表 歌曲搜索
  -  轮播图
  - 侧边栏： 我的歌单 个人详情（登录+注册）
 -  底部页面：播放控制（暂停、播放、上一首、下一首、进度拖拽，音量控制)
  2. 第二个页面 播放页面 音乐播放
  - 播放音乐
  - 播放控制（暂停、播放、上一首、下一首、进度拖拽，音量控制)
  - 歌单

## 2.主页面

  ### 2.1.轮播图介绍
>一个8页的活动轮播图
```JavaScript
const data = [
                { url: './images/slider01.jpg', title: '低空飞行，首发单曲', color: 'rgb(100, 67, 68)' },
                { url: './images/slider02.jpg', title: '出发寻找属于自己的正解', color: 'rgb(43, 35, 26)' },
                { url: './images/slider03.jpg', title: '暗恋神作传达汹涌爱意', color: 'rgb(36, 31, 33)' },
                { url: './images/slider04.jpg', title: '原神启动', color: 'rgb(139, 98, 66)' },
                { url: './images/slider05.jpg', title: '新年上上签', color: 'rgb(67, 90, 92)' },
                { url: './images/slider06.jpg', title: '全明星街球派对主题曲', color: 'rgb(166, 131, 143)' },
                { url: './images/slider07.jpg', title: '海贼王25周年经典版', color: 'rgb(53, 29, 25)' },
                { url: './images/slider08.jpg', title: '跟我做自己', color: 'rgb(53, 29, 25)' }
            ]
            // 获取元素
            const img = document.querySelector('.slider-wrapper img')
            const p = document.querySelector('.slider-footer p')
            const footer = document.querySelector('.slider-footer')
            // 1. 右按钮业务
            // 1.1 获取右侧按钮 
            const next = document.querySelector('.next')
            let i = 0  // 信号量 控制播放图片张数
            // 1.2 注册点击事件

            next.addEventListener('click', function () {
                // console.log(11)
                i++
                // 1.6判断条件  如果大于8 就复原为 0
                // if (i >= 8) {
                //   i = 0
                // }
                i = i >= data.length ? 0 : i
                // 1.3 得到对应的对象
                // console.log(data[i])
                // 调用函数
                toggle()
            })

            // 2. 左侧按钮业务
            // 2.1 获取左侧按钮 
            const prev = document.querySelector('.prev')
            // 1.2 注册点击事件
            prev.addEventListener('click', function () {
                i--
                // 判断条件  如果小于0  则爬到最后一张图片索引号是 7
                // if (i < 0) {
                //   i = 7
                // }
                i = i < 0 ? data.length - 1 : i
                // 1.3 得到对应的对象
                // console.log(data[i])
                // 调用函数
                toggle()
            })

            // 声明一个渲染的函数作为复用
            function toggle() {
                // 1.4 渲染对应的数据
                img.src = data[i].url
                p.innerHTML = data[i].title
                footer.style.backgroundColor = data[i].color
                // 1.5 更换小圆点    先移除原来的类名， 当前li再添加 这个 类名
                document.querySelector('.slider-indicator .active').classList.remove('active')
                document.querySelector(`.slider-indicator li:nth-child(${i + 1})`).classList.add('active')
            }


            // 1、 获取第一个小li
            const li1 = document.querySelector('.xtx_navs li:first-child')
            const li2 = li1.nextElementSibling
            // 2. 最好做个渲染函数 因为退出登录需要重新渲染
            function render() {
                // 2.1 读取本地存储的用户名
                const uname = localStorage.getItem('xtx-uname')
                // console.log(uname)
                if (uname) {
                    li1.innerHTML = `<a href="javascript:;"><i class="iconfont icon-user">${uname
                        }</i></a>
        `
                    li2.innerHTML = '<a href="javascript:;">退出登录</a>'
                } else {
                    li1.innerHTML = '<a href="./login.html">请先登录</a>'
                    li2.innerHTML = '<a href="./register.html">免费注册</a>'
                }
            }
            render()  // 调用函数

            // 2. 点击退出登录模块
            li2.addEventListener('click', function () {
                // 删除本地存储的数据
                localStorage.removeItem('xtx-uname')
                // 重新渲染
                render()
            })
```

### 2.2 注册和登录页面
#### 2.2.1注册页面
>- 应该包含“设置用户名”，“手机号，验证码”，“确定输入”，“同意服务约定”等功能，且有一处有问题该注册都不能成功。
>- 此处多次使用事件监听来实现
  ```JavaScript
 
 
      // 1. 发送短信验证码模块
      const code = document.querySelector('.code')
      let flag = true  // 通过一个变量来控制   节流阀 
      //  1.1 点击事件
      code.addEventListener('click', function () {
        if (flag) {
          // 取反了，不能马上第二次点击
          flag = false
          let i = 5
          // 点击完毕之后立马触发
          code.innerHTML = `0${i}秒后重新获取`
          // 开启定时器
          let timerId = setInterval(function () {
            i--
            code.innerHTML = `0${i}秒后重新获取`
            if (i === 0) {
              // 清除定时器
              clearInterval(timerId)
              // 从新获取
              code.innerHTML = `重新获取`
              // 到时间了，可以开启 flag了
              flag = true
            }
          }, 1000)
        }
      })
    })();


    // 2. 验证的是用户名
    // 2.1 获取用户名表单
    const username = document.querySelector('[name=username]')
    // 2.2 使用change事件  值发生变化的时候
    username.addEventListener('change', verifyName)
    // 2.3 封装verifyName函数
    function verifyName() {
      // console.log(11)
      const span = username.nextElementSibling
      // 2.4 定规则  用户名
      const reg = /^[a-zA-Z0-9-_]{6,10}$/
      if (!reg.test(username.value)) {
        // console.log(11)
        span.innerText = '输入不合法,请输入6~10位'
        return false
      }
      // 2.5 合法的 就清空span
      span.innerText = ''
      return true
    }



    // 3. 验证的是手机号
    // 2.1 获取手机表单
    const phone = document.querySelector('[name=phone]')
    // 2.2 使用change事件  值发生变化的时候
    phone.addEventListener('change', verifyPhone)
    // 2.3 verifyPhone
    function verifyPhone() {
      // console.log(11)
      const span = phone.nextElementSibling
      // 2.4 定规则  用户名
      const reg = /^1(3\d|4[5-9]|5[0-35-9]|6[567]|7[0-8]|8\d|9[0-35-9])\d{8}$/
      if (!reg.test(phone.value)) {
        // console.log(11)
        span.innerText = '输入不合法,请输入正确的11位手机号码'
        return false
      }
      // 2.5 合法的 就清空span
      span.innerText = ''
      return true
    }


    // 4. 验证的是验证码
    // 4.1 获取验证码表单
    const codeInput = document.querySelector('[name=code]')
    //4.2 使用change事件  值发生变化的时候
    codeInput.addEventListener('change', verifyCode)
    // 4.3 verifyPhone
    function verifyCode() {
      // console.log(11)
      const span = codeInput.nextElementSibling
      // 4.4 定规则  验证码
      const reg = /^\d{6}$/
      if (!reg.test(codeInput.value)) {
        // console.log(11)
        span.innerText = '输入不合法,6 位数字'
        return false
      }
      // 4.5 合法的 就清空span
      span.innerText = ''
      return true
    }

    // 5. 验证的是密码框
    // 5.1 获取密码表单
    const password = document.querySelector('[name=password]')
    //5.2 使用change事件  值发生变化的时候
    password.addEventListener('change', verifyPwd)
    // 5.3 verifyPhone
    function verifyPwd() {
      // console.log(11)
      const span = password.nextElementSibling
      // 5.4 定规则  密码
      const reg = /^[a-zA-Z0-9-_]{6,20}$/
      if (!reg.test(password.value)) {
        // console.log(11)
        span.innerText = '输入不合法,6~20位数字字母符号组成'
        return false
      }
      // 5.5 合法的 就清空span
      span.innerText = ''
      return true
    }



    // 6. 密码的再次验证
    // 6.1 获取再次验证表单
    const confirm = document.querySelector('[name=confirm]')
    //6.2 使用change事件  值发生变化的时候
    confirm.addEventListener('change', verifyConfirm)
    // 6.3 verifyPhone
    function verifyConfirm() {
      // console.log(11)
      const span = confirm.nextElementSibling
      // 6.4 当前表单的值不等于 密码框的值就是错误的
      if (confirm.value !== password.value) {
        // console.log(11)
        span.innerText = '两次密码输入不一致'
        return false
      }
      // 6.5 合法的 就清空span
      span.innerText = ''
      return true
    }

    // 7. 我同意
    const queren = document.querySelector('.icon-queren')
    queren.addEventListener('click', function () {
      // 切换类  原来有的就删掉，原来没有就添加
      this.classList.toggle('icon-queren2')
    })

    // 8. 提交模块
    const form = document.querySelector('form')
    form.addEventListener('submit', function (e) {
      // 判断是否勾选我同意模块 ，如果有 icon-queren2说明就勾选了，否则没勾选
      if (!queren.classList.contains('icon-queren2')) {
        alert('请勾选同意协议')
        // 阻止提交
        e.preventDefault()
      }
      // 依次判断上面的每个框框 是否通过，只要有一个没有通过的就阻止
      // console.log(verifyName())
      if (!verifyName()) e.preventDefault()
      if (!verifyPhone()) e.preventDefault()
      if (!verifyCode()) e.preventDefault()
      if (!verifyPwd()) e.preventDefault()
      if (!verifyConfirm()) e.preventDefault()
    })

```
 #### 2.2.2 登录页面
 - 登录页面中可以选择正常的密码登录和二维码登录
 - 其中密码登录中包含 用户名，密码，同意服务条款（不勾选就不能登录）
>其中应该附带 忘记密码 和 注册页面的选择
-登录后的昵称和头像修改没做出来
```JavaScript
 // 1. tab栏切换  事件委托
    const tab_nav = document.querySelector('.tab-nav')
    const pane = document.querySelectorAll('.tab-pane')
    // 1.1 事件监听
    tab_nav.addEventListener('click', function (e) {
      if (e.target.tagName === 'A') {
        // 取消上一个active
        tab_nav.querySelector('.active').classList.remove('active')
        // 当前元素添加active
        e.target.classList.add('active')

        // 先干掉所有人  for循环
        for (let i = 0; i < pane.length; i++) {
          pane[i].style.display = 'none'
        }
        // 让对应序号的 大pane 显示 
        pane[e.target.dataset.id].style.display = 'block'
      }
    })

    // 点击提交模块
    const form = document.querySelector('form')
    const agree = document.querySelector('[name=agree]')
    const username = document.querySelector('[name=username]')
    form.addEventListener('submit', function (e) {
      e.preventDefault()
      // 判断是否勾选同意协议
      if (!agree.checked) {
        return alert('请勾选同意协议')
      }

      // 记录用户名到本地存储
      localStorage.setItem('xtx-uname', username.value)
      // 跳转到首页
      location.href = './index.html'
    })

```
### 2.3 歌单，推荐曲目
- 由于不知道点击图片和切换歌曲如何搭配着写，所以在该栏目仅仅添加了一点鼠标移动到图片上的动画


### 2.4 底部栏
#### 2.4.1底部栏歌曲
- 底部配备了一首歌

#### 2.4.2播放，切歌，进度条
- 播放：播放和暂停时切换图标
```JavaScript
// 播放开始与暂停以及相关的图标字体修改
function bofang() {
    if (audio.paused) {
        audio.play()
        _audio.classList.remove('icon-bofang')
        _audio.classList.add('icon-zanting')
    } else {
        audio.pause()
        _audio.classList.remove('icon-zanting')
        _audio.classList.add('icon-bofang')
    }
}

// 是否静音与相关的图标字体修改
_voice.addEventListener('click', () => {
    if (audio.muted) {
        audio.muted = false
        _voice.classList.remove('icon-yinliangguanbi')
        _voice.classList.add('icon-yinliangkai')
    } else {
        audio.muted = true
        _voice.classList.remove('icon-yinliangkai')
        _voice.classList.add('icon-yinliangguanbi')
    }
})

```
- 切歌：此处只有一首歌，后面的纯净式播放器里面有该功能。
- 进度条：未播放时进度条在最开始处，随着音乐播放进度条缓慢移动。
>先要将歌曲时间放入合适位置
  ```JavaScript
  // 时长进度条
    const progress = document.querySelector(".progress");
    const slide = document.querySelector(".slide");
    const fill = document.querySelector(".fill")
    audio.ontimeupdate = function () {
        let l = (audio.currentTime / audio.duration) * 100;
        slide.style.left = l + "%";
        fill.style.width = l + "%";
        if (audio.currentTime == 0) {
            slide.style.left = "0%";
        }
        const currentTime = document.querySelector(".currentTime");
        currentTime.innerHTML = transTime(parseInt(audio.currentTime));
        const duraTime = document.querySelector(".duraTime");
        duraTime.innerHTML = transTime(audio.duration);
    };

    // 进度条拖动
    slide.onmousedown = function (e) {
        let x = e.clientX - this.offsetLeft
        document.onmousemove = function (e) {
            let jlx = ((e.clientX - x) / progress.clientWidth) * 100
            if (jlx <= 100 && jlx >= 0) {
                slide.style.left = jlx + "%"
            }
            audio.currentTime = (jlx / 100) * audio.duration
        }
        document.onmouseup = function () {
            document.onmousemove = null
            document.onmouseup = null
        }
    }
    slide.ontouchstart = function (e) {
        let x = e.targetTouches[0].clientX - this.offsetLeft
        document.ontouchmove = function (e) {
            let jlx = ((e.targetTouches[0].clientX - x) / progress.clientWidth) * 100
            if (jlx <= 100 && jlx >= 0) {
                slide.style.left = jlx + '%'
            }
            audio.currentTime = (jlx / 100) * audio.duration
        }
        document.ontouchend = function (e) {
            document.ontouchmove = null
            document.ontouchend = null
        }
    }
}

```

  ## 3.音乐播放界面
  - 包含了简约风格的四个键位，暂停播放，上一首，下一首，歌单
  - 在播放时图片会慢慢旋转，暂停后停止，接着播放时继续旋转
    >旋转和该页面大部分功能是在B站上听讲解后仿做的，无法做过多解释,见谅


  
  
