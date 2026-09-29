<!doctype html>

<html lang="zh-CN">

<head>

<meta charset="utf-8">

<meta name="viewport" content="width=device-width,initial-scale=1">

<title>股票雷达｜每日5-10只</title>

<style>

*{box-sizing:border-box}

body{

    margin:0;

    background:#f5f7fa;

    color:#18202a;

    font-family:-apple-system,BlinkMacSystemFont,

    "Segoe UI","PingFang SC","Microsoft YaHei",sans-serif;

}

.container{

    max-width:1100px;

    margin:auto;

    padding:18px;

}

header{

    display:flex;

    justify-content:space-between;

    align-items:center;

    margin-bottom:15px;

}

h1{

    margin:0;

    font-size:25px;

}

.subtitle{

    color:#718096;

    font-size:13px;

    margin-top:5px;

}

button{

    border:0;

    background:#1677ff;

    color:white;

    padding:10px 18px;

    border-radius:9px;

    cursor:pointer;

    font-size:14px;

}

.card{

    background:white;

    border:1px solid #e8edf3;

    border-radius:14px;

    padding:16px;

    margin-bottom:14px;

}

.tags{

    display:flex;

    gap:8px;

    flex-wrap:wrap;

}

.tag{

    background:#f0f5ff;

    color:#2563eb;

    padding:7px 11px;

    border-radius:20px;

    font-size:13px;

}

.table-box{

    overflow:auto;

}

table{

    width:100%;

    min-width:900px;

    border-collapse:collapse;

}

th,td{

    text-align:left;

    padding:13px 9px;

    border-bottom:1px solid #edf0f4;

}

th{

    color:#718096;

    font-size:12px;

}

td{

    font-size:14px;

}

.red{

    color:#e23b3b;

    font-weight:bold;

}

.money{

    color:#c66a00;

    font-weight:bold;

}

.news{

    display:inline-block;

    background:#fff3e8;

    color:#c65b00;

    padding:4px 8px;

    border-radius:6px;

    font-size:12px;

}

.strong{

    color:#e23b3b;

    font-weight:bold;

}

.good{

    color:#1677ff;

    font-weight:bold;

}

.small{

    color:#718096;

    font-size:12px;

    line-height:1.7;

}

.reason{

    margin-top:15px;

    padding:12px;

    background:#f8fafc;

    border-radius:9px;

    font-size:13px;

}

@media(max-width:600px){

    h1{font-size:20px}

}

</style>

</head>

<body>

<div class="container">

<header>

<div>

<h1>📡 股票雷达</h1>

<div class="subtitle">

每天只看5～10只｜利好＋主力资金＋5日＋10日＋趋势

</div>

</div>

<button onclick="refresh()">刷新</button>

</header>

<div class="card">

<div class="tags">

<div class="tag">🔥 今日利好</div>

<div class="tag">💰 主力净流入</div>

<div class="tag">📈 5日净流入</div>

<div class="tag">📈 10日净流入</div>

<div class="tag">📊 今日涨幅</div>

<div class="tag">🔎 趋势</div>

</div>

<p class="small">

筛选目标：优先观察有明确消息催化、资金持续流入、

涨幅不过度、技术趋势改善的股票。

</p>

</div>

<div class="card">

<div style="

display:flex;

justify-content:space-between;

margin-bottom:10px;

">

<b>🔥 今日重点观察</b>

<span class="small" id="date"></span>

</div>

<div class="table-box">

<table>

<thead>

<tr>

<th>股票</th>

<th>今日涨幅</th>

<th>主力净流入</th>

<th>5日净流入</th>

<th>10日净流入</th>

<th>今日利好</th>

<th>趋势</th>

</tr>

</thead>

<tbody id="stocks"></tbody>

</table>

</div>

</div>

<div class="card">

<b>为什么入选？</b>

<div class="reason">

<strong>筛选逻辑：</strong>

<br><br>

1️⃣ 今天出现明确利好或行业催化

<br>

2️⃣ 主力资金当天净流入

<br>

3️⃣ 5日资金保持正流入

<br>

4️⃣ 10日资金保持正流入

<br>

5️⃣ 涨幅没有明显脱离合理区间

<br>

6️⃣ 成交量、均线、突破情况支持趋势判断

</div>

</div>

<div class="card">

<b>⚠️ 数据说明</b>

<p class="small">

当前网页为界面原型，表格中的股票数据是示例数据。

正式版需要连接实时行情、资金流向和新闻数据接口，

才能每天自动产生真实的5～10只股票名单。

</p>

</div>

</div>

<script>

const stocks = [

{

name:"示例A",

change:"+6.8%",

main:"+2.40亿",

five:"+6.10亿",

ten:"+10.30亿",

news:"订单/业绩催化",

trend:"强"

},

{

name:"示例B",

change:"+4.2%",

main:"+1.70亿",

five:"+4.80亿",

ten:"+7.20亿",

news:"行业景气/政策",

trend:"偏强"

},

{

name:"示例C",

change:"+3.1%",

main:"+1.20亿",

five:"+3.50亿",

ten:"+5.90亿",

news:"公司公告",

trend:"偏强"

},

{

name:"示例D",

change:"+7.5%",

main:"+3.00亿",

five:"+2.10亿",

ten:"+4.00亿",

news:"重大合同",

trend:"观察"

},

{

name:"示例E",

change:"+2.6%",

main:"+0.90亿",

five:"+2.70亿",

ten:"+4.60亿",

news:"行业涨价",

trend:"偏强"

}

];

function render(){

const tbody=document.getElementById("stocks");

tbody.innerHTML="";

stocks.forEach(stock=>{

const tr=document.createElement("tr");

tr.innerHTML=`

<td><b>${stock.name}</b></td>

<td class="red">

${stock.change}

</td>

<td class="money">

${stock.main}

</td>

<td class="money">

${stock.five}

</td>

<td class="money">

${stock.ten}

</td>

<td>

<span class="news">

${stock.news}

</span>

</td>

<td class="${

stock.trend==="强"

?"strong"

:"good"

}">

${stock.trend}

</td>

`;

tbody.appendChild(tr);

});

document.getElementById("date").innerText =

new Date().toLocaleString("zh-CN");

}

function refresh(){

render();

}

render();

</script>

</body>

</html>
