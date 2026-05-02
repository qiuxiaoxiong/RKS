<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RKS 秋小熊服务器</title>
<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "微软雅黑", sans-serif;
}
body {
    background: #0a0a1a;
    color: #fff;
    overflow: hidden;
}

/* 苹果纯白色静态灵动岛 无变色动画 */
.dynamic-island {
position: fixed;
top: 12px;
left: 50%;
transform: translateX(-50%);
width: 120px;
height: 35px;
border-radius: 20px;
z-index: 10000;
display: flex;
align-items: center;
justify-content: center;
background: #ffffff;
color: #000000;
font-size: 12px;
font-weight: bold;
transition: all 0.3s ease;
}
.dynamic-island.active {
width: 200px;
height: 40px;
}

/* 跑马灯边框 */
.marquee-wrap {
position: fixed;
top: 0;
left: 0;
width: 100vw;
height: 100vh;
z-index: 1;
pointer-events: none;
}
.marquee-line {
position: absolute;
background: linear-gradient(90deg,#3366ff,#22c55e,#ff6699,#3366ff);
background-size: 300% 300%;
animation: moveRun 3s linear infinite;
}
.top-line {
width: 100%;
height: 3px;
top: 0;
left: 0;
}
.bottom-line {
width: 100%;
height: 3px;
bottom: 0;
left: 0;
}
.left-line {
width: 3px;
height: 100%;
top: 0;
left: 0;
}
.right-line {
width: 3px;
height: 100%;
top: 0;
right: 0;
}
@keyframes moveRun {
0% { background-position: 0% 50%; }
100% { background-position: 300% 50%; }
}

/* 花瓣雨 */
#petalRain {
position: fixed;
top: 0;
left: 0;
width: 100vw;
height: 100vh;
pointer-events: none;
z-index: 2;
overflow: hidden;
}
.petal {
position: absolute;
top: -10%;
width: 20px;
height: 20px;
background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path fill="%23ffd6dd" d="M10 0C12 5 18 8 10 20C2 8 8 5 10 0Z"/></svg>');
background-size: contain;
background-repeat: no-repeat;
opacity: 0.9;
}

/* 页面布局 */
.page {
width: 100vw;
height: 100vh;
display: none;
flex-direction: column;
align-items: center;
justify-content: center;
position: relative;
z-index: 10;
}
.page.active {
display: flex;
}

/* 输入框区域 */
.input-box {
background: #151530;
padding: 40px 30px;
border-radius: 15px;
box-shadow: 0 0 30px #4169E1;
text-align: center;
border: 1px solid #3366ff;
}
.input-box h2 {
margin-bottom: 25px;
color: #58a6ff;
}
.input-box .tip {
color: #22c55e;
font-size: 18px;
margin-bottom: 25px;
}
.input-box input {
width: 280px;
height: 45px;
background: #0f0f25;
border: 1px solid #3366ff;
border-radius: 8px;
color: #fff;
text-align: center;
font-size: 18px;
outline: none;
padding: 0 10px;
}
.input-box button {
margin-top: 20px;
width: 280px;
height: 45px;
background: #3366ff;
border: none;
border-radius: 8px;
color: #fff;
font-size: 17px;
cursor: pointer;
transition: 0.3s;
}
.input-box button:hover {
background: #1a4cd8;
}

/* 悬浮提示 */
.tips-float {
position: fixed;
top: 30px;
left: 50%;
transform: translateX(-50%);
background: #1f2937;
padding: 15px 30px;
border-radius: 50px;
border: 1px solid #22c55e;
color: #22c55e;
font-size: 16px;
box-shadow: 0 0 15px #22c55e;
display: none;
z-index: 998;
}

/* 加载进度条 */
.load-ui {
width: 90%;
max-width: 400px;
}
.load-ui h3 {
text-align: center;
margin-bottom: 20px;
color: #58a6ff;
font-size: 18px;
}
.progress-bg {
width: 100%;
height: 20px;
background: #151530;
border-radius: 10px;
overflow: hidden;
border: 1px solid #3366ff;
}
.progress-bar {
height: 100%;
width: 0%;
background: linear-gradient(90deg,#3366ff,#22c55e);
border-radius: 10px;
transition: all 0.05s linear;
}

/* 苹果人脸识别框 + 背景RKS炫酷字体 */
.face-scan {
width: 260px;
height: 300px;
border: 3px solid #22c55e;
border-radius: 24px;
position: relative;
overflow: hidden;
margin-bottom: 30px;
box-shadow: 0 0 30px #22c55e;
display: flex;
align-items: center;
justify-content: center;
}
/* 人脸识别背景超大RKS炫酷字体 */
.face-rks-bg {
position: absolute;
font-size: 90px;
font-weight: 900;
letter-spacing: 8px;
color: rgba88, 166, 255, 0.15;
text-shadow: 0 0 20px #3366ff;
z-index: 1;
}
.scan-line {
position: absolute;
width: 100%;
height: 4px;
background: linear-gradient(90deg,transparent,#22c55e,transparent);
top: 0;
left: 0;
box-shadow: 0 0 15px #22c55e;
z-index: 2;
animation: scan 2s ease-in-out infinite;
}
@keyframes scan {
0%{top:0;}
50%{top:calc(100% - 4px);}
100%{top:0;}
}

/* 入侵成功 炫酷RKS发光字体 */
.success-big {
font-size: 46px;
font-weight: 900;
color: #22c55e;
margin-bottom: 15px;
letter-spacing: 6px;
text-shadow: 
0 0 5px #22c55e,
0 0 15px #22c55e,
0 0 30px #3366ff,
0 0 45px #ff6699;
}
.success-desc {
font-size: 16px;
color: #aaa;
margin-bottom: 40px;
}

/* 功能按钮组 */
.func-btn-group {
display: flex;
flex-direction: column;
gap: 15px;
width: 260px;
}
.func-btn {
width: 100%;
height: 50px;
background: #151530;
border: 1px solid #3366ff;
border-radius: 10px;
color: #fff;
font-size: 18px;
cursor: pointer;
transition: 0.3s;
}
.func-btn:hover {
background: #3366ff;
box-shadow: 0 0 15px #3366ff;
}
</style>
</head>
<body>

<!-- 苹果纯白色灵动岛 -->
<div class="dynamic-island" id="dynamicIsland">正在运行</div>

<!-- 四边跑马灯 -->
<div class="marquee-wrap">
    <div class="marquee-line top-line"></div>
    <div class="marquee-line bottom-line"></div>
    <div class="marquee-line left-line"></div>
    <div class="marquee-line right-line"></div>
</div>

<!-- 花瓣雨 -->
<div id="petalRain"></div>

<!-- 悬浮提示 -->
<div class="tips-float" id="tips">检测到是开发者用户已为你自动屏蔽</div>

<!-- 第一页 卡密输入 -->
<div class="page active" id="page1">
    <div class="input-box">
        <h2>秋小熊服务器 卡密验证</h2>
        <div class="tip">请输入卡密：RKSNBQJ</div>
        <input type="text" id="pwdInput" placeholder="请输入卡密" value="RKSNBQJ">
        <button onclick="checkPwd()">确认验证</button>
    </div>
</div>

<!-- 第二页 加载进度 -->
<div class="page" id="page2">
    <div class="load-ui">
        <h3>正在尝试入侵ACE服务器</h3>
        <div class="progress-bg">
            <div class="progress-bar" id="bar"></div>
        </div>
    </div>
</div>

<!-- 第三页 苹果人脸识别+背景RKS字体 -->
<div class="page" id="page3">
    <div class="face-scan">
        <!-- 人脸识别框背景RKS炫酷大字 -->
        <div class="face-rks-bg">RKS</div>
        <!-- 扫描线条 -->
        <div class="scan-line"></div>
    </div>

    <div class="success-big">入侵成功</div>
    <div class="success-desc">服务器连接正常</div>

    <div class="func-btn-group">
        <a href="https://1841782875.share.123pan.cn/123pan/FiqrTd-2nuQv" target="_blank" style="text-decoration:none;">
            <button class="func-btn">RKS公益直装链接</button>
        </a>
        <a href="https://fh10.zmfaka.cn/shop/H57UB9GK" target="_blank" style="text-decoration:none;">
            <button class="func-btn">八级号卡网链接</button>
        </a>
        <a href="tg://resolve?domain=RKSNBQJ" target="_blank" style="text-decoration:none;">
            <button class="func-btn">加入官方频道</button>
        </a>
    </div>
</div>

<script>
const rightPwd = "RKSNBQJ"; 
let tips = document.getElementById('tips'); 
let page1 = document.getElementById('page1'); 
let page2 = document.getElementById('page2'); 
let page3 = document.getElementById('page3'); 
let bar = document.getElementById('bar'); 
let island = document.getElementById('dynamicIsland'); 

// 灵动岛点击扩展
island.addEventListener('click', () => { 
    island.classList.toggle('active'); 
}); 

function checkPwd() { 
    let val = document.getElementById('pwdInput').value.trim(); 
    if (val === rightPwd) { 
        tips.style.display = 'block'; 
        setTimeout(() => { 
            tips.style.display = 'none'; 
            page1.classList.remove('active'); 
            page2.classList.add('active'); 
            startProgress(); 
        }, 2000); 
    } else { 
        alert('卡密错误，请重新输入！'); 
    } 
} 

function startProgress() { 
    let num = 0; 
    let timer = setInterval(() => { 
        num++; 
        bar.style.width = num + '%'; 
        if (num >= 100) { 
            clearInterval(timer); 
            setTimeout(() => { 
                page2.classList.remove('active'); 
                page3.classList.add('active'); 
            }, 500); 
        } 
    }, 30); 
} 

// 花瓣雨特效
const rainBox = document.getElementById('petalRain'); 
function createPetal(){ 
    let petal = document.createElement('div'); 
    petal.className = 'petal'; 
    let size = Math.random() * 16 + 8; 
    petal.style.width = size + 'px'; 
    petal.style.height = size + 'px'; 
    petal.style.left = Math.random() * 100 + 'vw'; 
    petal.style.transform = `rotate(${Math.random() * 360}deg)`; 
    let dura = Math.random() * 5 + 4; 
    petal.style.transition = `top ${dura}s linear, left ${dura}s ease-in-out, transform ${dura}s linear`; 
    rainBox.appendChild(petal); 
    setTimeout(()=>{ 
        petal.style.top = '105vh'; 
        petal.style.left = (parseFloat(petal.style.left) + (Math.random()*10 - 5)) + 'vw'; 
        petal.style.transform = `rotate(${Math.random() * 720}deg)`; 
    },100); 
    setTimeout(()=>{ 
        petal.remove(); 
    }, dura * 1000); 
} 
setInterval(createPetal, 100); 
</script>

</body>
</html>
