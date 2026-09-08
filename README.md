<!DOCTYPE html>

<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Smart Farm Planner</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600;700&display=swap" rel="stylesheet">

<style>
*{box-sizing:border-box}

body{
    margin:0;
    font-family:'Prompt',sans-serif;
    background:#f4f7f4;
    color:#26352a;
}

button,select,input{font-family:inherit}
button{cursor:pointer}

.topbar{
    background:linear-gradient(135deg,#1b5e20,#43a047);
    color:white;
    padding:24px 20px;
}

.topbar-inner{
    max-width:1400px;
    margin:auto;
}

.topbar h1{
    margin:0;
    font-size:28px;
}

.topbar p{
    margin:6px 0 0;
    opacity:.9;
}

.container{
    max-width:1400px;
    margin:auto;
    padding:22px;
}

.section{
    background:white;
    border-radius:18px;
    padding:20px;
    margin-bottom:20px;
    box-shadow:0 5px 20px rgba(0,0,0,.06);
}

.section-title{
    font-size:21px;
    font-weight:600;
    color:#1b5e20;
    margin-bottom:15px;
}

.section-subtitle{
    color:#718078;
    font-size:14px;
    margin-top:-8px;
    margin-bottom:16px;
}

.dashboard{
    display:grid;
    grid-template-columns:repeat(4,1fr);
    gap:14px;
}

.stat-card{
    background:#f8fbf8;
    border:1px solid #e3eee4;
    border-radius:14px;
    padding:18px;
}

.stat-icon{font-size:27px}

.stat-title{
    color:#728078;
    font-size:13px;
    margin-top:7px;
}

.stat-value{
    font-size:25px;
    font-weight:600;
    color:#1b5e20;
}

/* FARM MAP */

.farm-layout{
    display:grid;
    grid-template-columns:minmax(500px,1fr) 310px;
    gap:20px;
}

.farm-wrapper{
    overflow:auto;
    padding:10px;
}

.farm{
    display:grid;
    grid-template-columns:repeat(10,1fr);
    gap:4px;
    width:min(700px,100%);
    aspect-ratio:1/1;
    margin:auto;
}

.plot{
    position:relative;
    background:#eef6ec;
    border:1px solid #c8ddc6;
    border-radius:5px;
    min-width:0;
    min-height:0;
    cursor:pointer;
    transition:.15s;
    overflow:hidden;
}

.plot:hover{
    transform:scale(1.04);
    z-index:2;
}

.plot.selected{
    border:3px solid #2e7d32;
    background:#e7f5e7;
}

.plot-number{
    position:absolute;
    top:2px;
    left:3px;
    font-size:8px;
    color:#769078;
    z-index:3;
}

.plot-plants{
    position:absolute;
    inset:15px 2px 2px;
    display:flex;
    justify-content:center;
    align-items:flex-end;
    gap:1px;
    flex-wrap:wrap;
}

.plant-mini{
    width:11px;
    height:11px;
    border-radius:50%;
    position:relative;
    border:1px solid rgba(0,0,0,.15);
}

.plant-mini.tall{
    width:13px;
    height:13px;
}

.plant-mini.medium{
    width:10px;
    height:10px;
}

.plant-mini.low{
    width:7px;
    height:7px;
}

/* SAMPLE MAP */

.plot.sample-preview{
    outline:3px solid #ff9800;
    background:#fff8e1;
}

.sample-label{
    position:absolute;
    bottom:2px;
    right:3px;
    font-size:7px;
    color:#8a5a00;
    z-index:4;
}

/* CONTROL */

.control-panel{
    background:#f8faf8;
    border:1px solid #e2ebe2;
    border-radius:15px;
    padding:17px;
    height:max-content;
}

.control-title{
    font-weight:600;
    color:#1b5e20;
    margin-bottom:12px;
}

.control-row{margin-bottom:12px}

.control-row label{
    display:block;
    font-size:13px;
    color:#66736a;
    margin-bottom:5px;
}

select,
input[type="number"],
input[type="text"]{
    width:100%;
    padding:10px 12px;
    border:1px solid #ccd8ce;
    border-radius:9px;
    background:white;
    outline:none;
}

select:focus,input:focus{
    border-color:#43a047;
}

.btn{
    border:0;
    border-radius:9px;
    padding:10px 14px;
    font-weight:500;
}

.btn-primary{
    background:#2e7d32;
    color:white;
}

.btn-primary:hover{
    background:#1b5e20;
}

.btn-light{
    background:#e8f2e9;
    color:#1b5e20;
}

.btn-danger{
    background:#ffebee;
    color:#c62828;
}

.btn-full{width:100%}

.legend{
    display:flex;
    flex-wrap:wrap;
    gap:10px;
    margin-bottom:12px;
    font-size:12px;
}

.legend-item{
    display:flex;
    align-items:center;
    gap:5px;
}

.legend-dot{
    width:11px;
    height:11px;
    border-radius:50%;
}

/* SELECTED */

.selected-crops{margin-top:15px}

.crop-row{
    display:flex;
    align-items:center;
    justify-content:space-between;
    background:white;
    border:1px solid #e2ebe2;
    border-radius:10px;
    padding:9px;
    margin-bottom:7px;
}

.crop-row-left{
    display:flex;
    align-items:center;
    gap:8px;
}

.crop-emoji{font-size:23px}

.crop-name{
    font-size:13px;
    font-weight:500;
}

.crop-meta{
    font-size:11px;
    color:#718078;
}

.qty-controls{
    display:flex;
    align-items:center;
    gap:5px;
}

.qty-btn{
    width:25px;
    height:25px;
    border:0;
    border-radius:6px;
    background:#e8f2e9;
    color:#1b5e20;
    font-weight:bold;
}

.qty-number{
    min-width:24px;
    text-align:center;
}

/* 2D */

.view-grid{
    display:grid;
    grid-template-columns:minmax(500px,1.4fr) 320px;
    gap:20px;
}

.scene-card{overflow:hidden}

.scene{
    position:relative;
    width:100%;
    height:430px;
    overflow:hidden;
    border-radius:16px;
    background:linear-gradient(#bfe6ff 0%,#e8f7ff 58%,#d5ebc8 58%,#c1dfb1 100%);
}

.scene-sun{
    position:absolute;
    width:65px;
    height:65px;
    background:#ffd54f;
    border-radius:50%;
    right:55px;
    top:35px;
    box-shadow:0 0 30px rgba(255,213,79,.7);
}

.scene-ground{
    position:absolute;
    bottom:0;
    left:0;
    right:0;
    height:42%;
    background:
        radial-gradient(circle at 10% 40%,rgba(55,100,45,.16) 0 2px,transparent 3px),
        radial-gradient(circle at 50% 60%,rgba(55,100,45,.12) 0 2px,transparent 3px),
        linear-gradient(#8fbd78,#6e9f58);
}

.scene-plants{
    position:absolute;
    left:20px;
    right:20px;
    bottom:65px;
    height:290px;
    display:flex;
    align-items:flex-end;
    justify-content:center;
    gap:12px;
    flex-wrap:wrap;
}

.big-plant{
    position:relative;
    width:52px;
    height:180px;
    display:flex;
    align-items:center;
    justify-content:flex-end;
    flex-direction:column;
}

.big-trunk{
    width:8px;
    height:65px;
    background:#795548;
    border-radius:4px;
}

.big-crown{
    width:55px;
    height:55px;
    background:#388e3c;
    border-radius:50%;
    position:relative;
    box-shadow:
        15px 3px 0 #43a047,
        -15px 7px 0 #2e7d32,
        4px -14px 0 #4caf50;
}

.crown-maryongchid,
.crown-mango{
    border-radius:48% 52% 42% 58%;
}

.crown-jackfruit{
    border-radius:45% 55% 48% 52%;
}

.crown-mangosteen{border-radius:50%}

.crown-mulberry{
    border-radius:35% 65% 50% 50%;
}

.crown-lime{border-radius:50%}

.crown-guava{
    border-radius:45% 55% 55% 45%;
}

.crown-chili{
    width:30px;
    height:45px;
    border-radius:50%;
}

.crown-stevia{
    width:32px;
    height:30px;
    border-radius:50%;
}

.crown-basil{
    width:35px;
    height:35px;
    border-radius:50%;
}

.crown-pandan{
    width:25px;
    height:55px;
    border-radius:50% 50% 15% 15%;
}

.big-plant.tall{height:270px}
.big-plant.medium{height:190px}
.big-plant.low{height:115px}

.scene-label{
    position:absolute;
    top:-25px;
    left:50%;
    transform:translateX(-50%);
    background:rgba(255,255,255,.92);
    border-radius:8px;
    padding:3px 6px;
    white-space:nowrap;
    font-size:10px;
    box-shadow:0 2px 6px rgba(0,0,0,.12);
}

.scene-info{
    background:#f8faf8;
    border:1px solid #e1ebe1;
    border-radius:15px;
    padding:18px;
}

.layer-card{
    border-radius:12px;
    padding:13px;
    margin-bottom:10px;
}

.layer-tall{
    background:#eaf4e8;
    border-left:5px solid #2e7d32;
}

.layer-medium{
    background:#f2f7e9;
    border-left:5px solid #7cb342;
}

.layer-low{
    background:#fff8df;
    border-left:5px solid #f9a825;
}

.layer-title{font-weight:600}

.layer-height{
    font-size:12px;
    color:#68756c;
}

/* BUSINESS */

.business-tabs{
    display:flex;
    gap:7px;
    overflow-x:auto;
    padding-bottom:7px;
    margin-bottom:15px;
}

.business-tab{
    white-space:nowrap;
    border:1px solid #d8e4da;
    background:white;
    color:#526057;
    padding:9px 15px;
    border-radius:20px;
}

.business-tab.active{
    background:#2e7d32;
    color:white;
    border-color:#2e7d32;
}

.insights{
    display:grid;
    grid-template-columns:repeat(4,1fr);
    gap:10px;
    margin-bottom:15px;
}

.insight{
    background:#f7faf7;
    border:1px solid #e1ebe2;
    border-radius:12px;
    padding:13px;
}

.insight-label{
    font-size:11px;
    color:#77847b;
}

.insight-value{
    font-size:18px;
    color:#1b5e20;
    font-weight:600;
    margin-top:3px;
}

.crop-tools{
    display:grid;
    grid-template-columns:1fr 160px;
    gap:10px;
    margin-bottom:14px;
}

.crop-grid{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:12px;
}

.crop-card{
    border:1px solid #e1e9e2;
    border-radius:14px;
    padding:15px;
    background:#fff;
    transition:.15s;
}

.crop-card:hover{
    border-color:#8fbc92;
    transform:translateY(-2px);
}

.crop-card-top{
    display:flex;
    align-items:center;
    gap:10px;
}

.crop-card-emoji{font-size:32px}

.crop-card-name{font-weight:600}

.crop-layer{
    font-size:11px;
    color:#6e7b72;
}

.crop-stat{
    display:flex;
    justify-content:space-between;
    border-top:1px solid #edf1ed;
    padding-top:7px;
    margin-top:9px;
    font-size:12px;
}

.crop-stat span:last-child{
    font-weight:500;
    color:#1b5e20;
}

.crop-buttons{
    display:flex;
    gap:6px;
    margin-top:11px;
}

.crop-buttons button{
    flex:1;
    font-size:11px;
    padding:7px;
}

.income-list{
    display:flex;
    flex-direction:column;
    gap:9px;
}

.income-item{
    display:grid;
    grid-template-columns:170px 1fr 100px;
    gap:12px;
    align-items:center;
}

.income-name{font-size:13px}

.income-bar-bg{
    height:15px;
    background:#edf2ed;
    border-radius:20px;
    overflow:hidden;
}

.income-bar{
    height:100%;
    background:#66bb6a;
    border-radius:20px;
}

.income-time{
    text-align:right;
    font-size:12px;
    color:#5e6b62;
}

.harvest-grid{
    display:grid;
    grid-template-columns:repeat(12,1fr);
    gap:5px;
    min-width:800px;
}

.harvest-month{
    background:#f5f8f5;
    border-radius:10px;
    padding:8px 5px;
    min-height:130px;
}

.harvest-month-title{
    text-align:center;
    font-size:12px;
    font-weight:600;
    color:#1b5e20;
    margin-bottom:7px;
}

.harvest-chip{
    width:100%;
    border:0;
    background:#e1f0e2;
    color:#315d34;
    border-radius:6px;
    padding:5px 2px;
    margin-bottom:4px;
    font-size:9px;
}

/* PRICE */

.price-layout{
    display:grid;
    grid-template-columns:250px 1fr;
    gap:18px;
    align-items:stretch;
}

.price-selector{
    background:#f6f9f6;
    border:1px solid #e1e9e1;
    border-radius:14px;
    padding:15px;
}

.price-current{
    margin-top:15px;
    background:white;
    border-radius:12px;
    padding:13px;
    border:1px solid #e1e9e1;
}

.price-current-number{
    font-size:27px;
    font-weight:600;
    color:#1b5e20;
}

.chart-container{
    position:relative;
    height:340px;
}

/* NEW YEAR CHARTS */

.year-chart-grid{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:14px;
}

.year-chart-card{
    background:#fafcf9;
    border:1px solid #e0e9e0;
    border-radius:14px;
    padding:14px;
}

.year-chart-title{
    font-size:17px;
    font-weight:600;
    color:#1b5e20;
    margin-bottom:8px;
}

.year-chart-wrap{
    position:relative;
    height:390px;
}

/* REVENUE */

.revenue-grid{
    display:grid;
    grid-template-columns:repeat(4,1fr);
    gap:12px;
    align-items:end;
}

.revenue-result{
    margin-top:18px;
    display:none;
    background:#e8f5e9;
    border:2px dashed #81c784;
    border-radius:14px;
    padding:20px;
    text-align:center;
}

.revenue-number{
    font-size:34px;
    color:#1b5e20;
    font-weight:600;
}

.revenue-detail{
    color:#68756c;
    font-size:13px;
}

/* NET */

.net-summary{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:12px;
    margin-top:15px;
}

.money-card{
    border-radius:14px;
    padding:18px;
    text-align:center;
    border:1px solid #e0e9e0;
    background:#f8faf8;
}

.money-card-title{
    font-size:12px;
    color:#718078;
}

.money-card-value{
    font-size:26px;
    font-weight:600;
    color:#1b5e20;
    margin-top:5px;
}

.cost-grid{
    display:grid;
    grid-template-columns:repeat(4,1fr);
    gap:10px;
}

.cost-box{
    background:#f8faf8;
    border:1px solid #e0e9e0;
    border-radius:12px;
    padding:12px;
}

.cost-box label{
    font-size:12px;
    color:#718078;
    display:block;
    margin-bottom:5px;
}

/* 10 YEAR */

.longterm-grid{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:15px;
}

.longterm-chart{
    position:relative;
    height:390px;
    background:#fafcf9;
    border:1px solid #e0e9e0;
    border-radius:14px;
    padding:12px;
}

.longterm-table{
    overflow:auto;
}

.longterm-table table{
    width:100%;
    border-collapse:collapse;
    font-size:12px;
}

.longterm-table th,
.longterm-table td{
    border-bottom:1px solid #e6ece6;
    padding:8px;
    text-align:right;
}

.longterm-table th:first-child,
.longterm-table td:first-child{
    text-align:center;
}

.longterm-table th{
    background:#f3f7f3;
    color:#1b5e20;
}

/* SAMPLE PLANS */

.sample-grid{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:14px;
}

.sample-card{
    border:1px solid #dfe9df;
    border-radius:14px;
    padding:16px;
    background:#fbfdfb;
}

.sample-card h3{
    margin:0 0 7px;
    color:#1b5e20;
    font-size:17px;
}

.sample-card p{
    font-size:12px;
    color:#718078;
    line-height:1.7;
}

.sample-count{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:5px;
    margin:10px 0;
}

.sample-count div{
    background:#f0f5f0;
    padding:7px;
    border-radius:8px;
    text-align:center;
    font-size:11px;
}

.sample-list{
    font-size:12px;
    line-height:1.8;
    margin-bottom:10px;
}

/* MODAL */

.modal{
    display:none;
    position:fixed;
    inset:0;
    background:rgba(20,35,23,.55);
    z-index:1000;
    padding:20px;
    overflow:auto;
}

.modal.show{
    display:flex;
    align-items:center;
    justify-content:center;
}

.modal-box{
    width:min(720px,100%);
    max-height:90vh;
    overflow:auto;
    background:white;
    border-radius:18px;
    padding:20px;
    position:relative;
}

.modal-close{
    position:absolute;
    right:15px;
    top:12px;
    border:0;
    background:#eef3ee;
    width:34px;
    height:34px;
    border-radius:50%;
    font-size:18px;
}

.modal-title{
    font-size:21px;
    font-weight:600;
    color:#1b5e20;
    padding-right:45px;
}

.detail-header{
    display:flex;
    align-items:center;
    gap:12px;
    margin-bottom:15px;
}

.detail-emoji{font-size:45px}

.detail-stats{
    display:grid;
    grid-template-columns:repeat(4,1fr);
    gap:8px;
    margin:15px 0;
}

.detail-stat{
    background:#f7faf7;
    border-radius:10px;
    padding:10px;
    text-align:center;
}

.detail-stat small{
    display:block;
    color:#7a867d;
    font-size:10px;
}

.detail-stat strong{
    color:#1b5e20;
    font-size:14px;
}

.detail-section{margin-top:15px}

.detail-section-title{
    font-weight:600;
    color:#355b3a;
    margin-bottom:7px;
}

.tag{
    display:inline-block;
    background:#e8f2e9;
    color:#35643b;
    border-radius:20px;
    padding:5px 9px;
    font-size:11px;
    margin:2px;
}

.plot-editor{
    display:grid;
    grid-template-columns:1fr 100px 100px;
    gap:8px;
    margin:15px 0;
}

.empty{
    text-align:center;
    padding:40px 20px;
    color:#78847b;
}

/* FOOTER */

.footer{
    text-align:center;
    color:#718078;
    font-size:12px;
    padding:10px 10px 30px;
}

.footer strong{
    color:#1b5e20;
}

/* RESPONSIVE */

@media(max-width:1100px){

    .year-chart-grid,
    .sample-grid{
        grid-template-columns:1fr;
    }

    .longterm-grid{
        grid-template-columns:1fr;
    }

    .farm-layout,
    .view-grid{
        grid-template-columns:1fr;
    }

    .crop-grid{
        grid-template-columns:repeat(2,1fr);
    }

    .market-grid{
        grid-template-columns:repeat(2,1fr);
    }

    .insights{
        grid-template-columns:repeat(2,1fr);
    }

    .dashboard{
        grid-template-columns:repeat(2,1fr);
    }

    .revenue-grid{
        grid-template-columns:repeat(2,1fr);
    }

    .price-layout{
        grid-template-columns:1fr;
    }

    .cost-grid{
        grid-template-columns:repeat(2,1fr);
    }
}

@media(max-width:650px){

    .container{padding:12px}

    .topbar h1{font-size:22px}

    .dashboard{
        grid-template-columns:repeat(2,1fr);
    }

    .crop-grid,
    .market-grid{
        grid-template-columns:1fr;
    }

    .crop-tools{
        grid-template-columns:1fr;
    }

    .income-item{
        grid-template-columns:100px 1fr 70px;
    }

    .revenue-grid{
        grid-template-columns:1fr;
    }

    .detail-stats{
        grid-template-columns:repeat(2,1fr);
    }

    .plot-editor{
        grid-template-columns:1fr 80px;
    }

    .plot-editor button{
        grid-column:1/-1;
    }

    .cost-grid,
    .net-summary{
        grid-template-columns:1fr;
    }

}
</style>

</head>

<body>

<header class="topbar">
    <div class="topbar-inner">
        <h1>🌱 Smart Farm Planner</h1>
        <p>วางแผนแปลงปลูก • วิเคราะห์พืช • ดูราคา • ประเมินรายได้ • วิเคราะห์ 10 ปี</p>
    </div>
</header>

<main class="container">

<section class="section">

```
<div class="dashboard">

    <div class="stat-card">
        <div class="stat-icon">📐</div>
        <div class="stat-title">พื้นที่ทั้งหมด</div>
        <div class="stat-value">1 ไร่</div>
    </div>

    <div class="stat-card">
        <div class="stat-icon">🟩</div>
        <div class="stat-title">จำนวนแปลง</div>
        <div class="stat-value">100 แปลง</div>
    </div>

    <div class="stat-card">
        <div class="stat-icon">🌳</div>
        <div class="stat-title">แปลงที่ใช้</div>
        <div class="stat-value" id="usedPlots">0</div>
    </div>

    <div class="stat-card">
        <div class="stat-icon">🌱</div>
        <div class="stat-title">ต้น / กอทั้งหมด</div>
        <div class="stat-value" id="totalPlants">0</div>
    </div>

</div>
```

</section>

<!-- FARM MAP -->

<section class="section">

```
<div class="section-title">🗺️ แผนผังการปลูก</div>

<div class="section-subtitle">
    1 ไร่ = 100 แปลงย่อย • คลิกแต่ละแปลงเพื่อดูหรือแก้ไขการปลูก
</div>

<div class="legend">

    <div class="legend-item">
        <span class="legend-dot" style="background:#2e7d32"></span>
        สูง
    </div>

    <div class="legend-item">
        <span class="legend-dot" style="background:#7cb342"></span>
        กลาง
    </div>

    <div class="legend-item">
        <span class="legend-dot" style="background:#f9a825"></span>
        ต่ำ
    </div>

</div>

<div class="farm-layout">

    <div class="farm-wrapper">
        <div class="farm" id="farmMap"></div>
    </div>

    <aside class="control-panel">

        <div class="control-title">
            🌱 จัดการแปลง
        </div>

        <div class="control-row">

            <label>แปลงที่เลือก</label>

            <input
                type="text"
                id="selectedPlotDisplay"
                value="ยังไม่ได้เลือก"
                readonly
            >

        </div>

        <div class="control-row">

            <label>เลือกพืช</label>

            <select id="cropSelect"></select>

        </div>

        <div class="control-row">

            <label>จำนวนต้น / กอ</label>

            <input
                type="number"
                id="quantityInput"
                value="1"
                min="1"
                step="1"
            >

        </div>

        <button
            class="btn btn-primary btn-full"
            onclick="addCropToPlot()">
            ＋ เพิ่มพืชในแปลง
        </button>

        <button
            class="btn btn-light btn-full"
            style="margin-top:7px"
            onclick="loadFullSampleFarm()">
            🌳 โหลดตัวอย่างครบ 100 แปลง
        </button>

        <button
            class="btn btn-danger btn-full"
            style="margin-top:7px"
            onclick="clearCurrentPlot()">
            🗑️ ล้างแปลงนี้
        </button>

        <div class="selected-crops">

            <div class="control-title">
                พืชในแปลง
            </div>

            <div id="selectedCrops">
                <div class="empty">
                    คลิกแปลงเพื่อเริ่มจัดวาง
                </div>
            </div>

        </div>

    </aside>

</div>
```

</section>

<!-- 2D -->

<section class="section">

```
<div class="section-title">🌄 ภาพ 2 มิติของแปลง</div>

<div class="section-subtitle">
    แสดงระดับความสูงของพืชเพื่อให้เห็นการแบ่งชั้นของแปลง
</div>

<div class="view-grid">

    <div class="scene-card">

        <div class="scene" id="hw3dnq">

            <div class="scene-sun"></div>

            <div class="scene-ground"></div>

            <div
                class="scene-plants"
                id="scenePlants">
            </div>

        </div>

    </div>

    <div class="scene-info" id="sceneInfo">

        <div class="layer-card layer-tall">

            <div class="layer-title">
                🌳 ชั้นสูง
            </div>

            <div class="layer-height">
                ประมาณ 4–12 เมตร
            </div>

            <small>
                มะยงชิด • มะม่วง • ขนุน • มังคุด
            </small>

        </div>

        <div class="layer-card layer-medium">

            <div class="layer-title">
                🌿 ชั้นกลาง
            </div>

            <div class="layer-height">
                ประมาณ 0.5–4 เมตร
            </div>

            <small>
                หม่อน • มะนาว • ฝรั่ง • พริก
            </small>

        </div>

        <div class="layer-card layer-low">

            <div class="layer-title">
                🌱 ชั้นต่ำ
            </div>

            <div class="layer-height">
                ประมาณ 0.3–1 เมตร
            </div>

            <small>
                หญ้าหวาน • กะเพรา • ใบเตย
            </small>

        </div>

        <div id="scenePlotSummary"></div>

    </div>

</div>
```

</section>

<!-- SAMPLE FARM -->

<section class="section">

```
<div class="section-title">
    🌳 ตัวอย่างการจัดสวนครบ 100 แปลง
</div>

<div class="section-subtitle">
    ตัวอย่างนี้เติมพืชลงครบทั้ง 100 ช่อง เพื่อให้เห็นภาพการใช้พื้นที่ทั้ง 1 ไร่
</div>

<div class="sample-grid">

    <div class="sample-card">

        <h3>🥭 แบบ A: สวนไม้ผลผสม</h3>

        <p>
            เน้นไม้ผลหลายชนิด กระจายตำแหน่งทั่วทั้ง 100 แปลง
            เพื่อไม่ให้เกิดการรวมพืชชนิดเดียวทั้งหมด
        </p>

        <div class="sample-count">
            <div>🌳 สูง<br><strong>25 ช่อง</strong></div>
            <div>🌿 กลาง<br><strong>35 ช่อง</strong></div>
            <div>🌱 ต่ำ<br><strong>40 ช่อง</strong></div>
        </div>

        <div class="sample-list">
            🥭 มะม่วงน้ำดอกไม้<br>
            🥭 มะยงชิด<br>
            🍈 ขนุน<br>
            🟣 มังคุด<br>
            🍋 มะนาว<br>
            🍐 ฝรั่ง<br>
            🌶️ พริกจินดาแดง<br>
            🌿 หญ้าหวาน / กะเพรา / ใบเตย
        </div>

        <button
            class="btn btn-primary btn-full"
            onclick="loadFullSampleFarm('A')">
            ดูตัวอย่าง 100 แปลง
        </button>

    </div>


    <div class="sample-card">

        <h3>🍋 แบบ B: เน้นพืชรายได้เร็ว</h3>

        <p>
            เพิ่มสัดส่วนพืชที่เริ่มให้ผลผลิตเร็ว
            พร้อมมีไม้ผลเป็นโครงสร้างระยะยาว
        </p>

        <div class="sample-count">
            <div>🌳 สูง<br><strong>20 ช่อง</strong></div>
            <div>🌿 กลาง<br><strong>30 ช่อง</strong></div>
            <div>🌱 ต่ำ<br><strong>50 ช่อง</strong></div>
        </div>

        <div class="sample-list">
            🥭 มะม่วงน้ำดอกไม้<br>
            🍈 ขนุน<br>
            🍋 มะนาว<br>
            🍐 ฝรั่ง<br>
            🌶️ พริกจินดาแดง<br>
            🌿 หญ้าหวาน<br>
            🌱 กะเพรา<br>
            🌾 ใบเตย
        </div>

        <button
            class="btn btn-primary btn-full"
            onclick="loadFullSampleFarm('B')">
            ดูตัวอย่าง 100 แปลง
        </button>

    </div>


    <div class="sample-card">

        <h3>🌱 แบบ C: เกษตรหลายชั้น</h3>

        <p>
            ใช้แนวคิดแบ่งพืชตามระดับความสูง
            โดยแยกตำแหน่งไม้สูง กลาง และต่ำ
        </p>

        <div class="sample-count">
            <div>🌳 สูง<br><strong>16 ช่อง</strong></div>
            <div>🌿 กลาง<br><strong>34 ช่อง</strong></div>
            <div>🌱 ต่ำ<br><strong>50 ช่อง</strong></div>
        </div>

        <div class="sample-list">
            🌳 มะยงชิด / มะม่วง / ขนุน / มังคุด<br>
            🌿 หม่อน / มะนาว / ฝรั่ง<br>
            🌶️ พริกจินดาแดง<br>
            🌱 หญ้าหวาน / กะเพรา / ใบเตย
        </div>

        <button
            class="btn btn-primary btn-full"
            onclick="loadFullSampleFarm('C')">
            ดูตัวอย่าง 100 แปลง
        </button>

    </div>

</div>

<div style="
    margin-top:14px;
    background:#fff8e1;
    border-radius:10px;
    padding:12px;
    font-size:12px;
    color:#725f20">

    ⚠️ ตัวอย่าง 100 แปลงเป็นแบบจำลองเพื่อแสดงแนวคิดการจัดพื้นที่
    ระยะปลูกจริงควรตรวจสอบตามพันธุ์ ทรงพุ่ม สภาพดิน
    ระบบน้ำ และเครื่องจักรที่ใช้ในพื้นที่จริง

</div>
```

</section>

<!-- BUSINESS -->

<section class="section" id="businessCenter">

```
<div class="section-title">
    📊 ศูนย์วิเคราะห์ฟาร์ม
</div>

<div class="section-subtitle">
    รวมข้อมูลพืช ราคา การเก็บเกี่ยว และรายได้ไว้ในหน้าเดียว
</div>

<div class="insights" id="quickInsights"></div>

<div class="business-tabs">

    <button
        class="business-tab active"
        data-tab="crops"
        onclick="setBusinessTab('crops',this)">
        🌱 พืช
    </button>

    <button
        class="business-tab"
        data-tab="revenue"
        onclick="setBusinessTab('revenue',this)">
        💰 รายได้
    </button>

    <button
        class="business-tab"
        data-tab="harvest"
        onclick="setBusinessTab('harvest',this)">
        📅 เก็บเกี่ยว
    </button>

    <button
        class="business-tab"
        data-tab="price"
        onclick="setBusinessTab('price',this)">
        💵 ราคา
    </button>

    <button
        class="business-tab"
        data-tab="market"
        onclick="setBusinessTab('market',this)">
        🏪 ตลาด
    </button>

    <button
        class="business-tab"
        data-tab="longterm"
        onclick="setBusinessTab('longterm',this)">
        📈 10 ปี
    </button>

</div>

<div id="businessContent"></div>
```

</section>

<!-- NET INCOME -->

<section class="section">

```
<div class="section-title">
    💰 วิเคราะห์รายได้สุทธิของฟาร์ม
</div>

<div class="section-subtitle">
    คำนวณจากตัวอย่างการปลูกครบ 100 แปลง
</div>

<div class="cost-grid">

    <div class="cost-box">
        <label>🌱 ต้นทุนกล้า / พันธุ์พืช (บาท/ปี)</label>
        <input type="number" id="costSeed" value="30000" min="0">
    </div>

    <div class="cost-box">
        <label>💧 ค่าน้ำและระบบน้ำ (บาท/ปี)</label>
        <input type="number" id="costWater" value="20000" min="0">
    </div>

    <div class="cost-box">
        <label>🧑‍🌾 ค่าแรง (บาท/ปี)</label>
        <input type="number" id="costLabor" value="40000" min="0">
    </div>

    <div class="cost-box">
        <label>🧪 ปุ๋ย / วัสดุ / อื่น ๆ (บาท/ปี)</label>
        <input type="number" id="costOther" value="30000" min="0">
    </div>

</div>

<button
    class="btn btn-primary"
    style="margin-top:12px"
    onclick="calculateFarmNetIncome()">
    🧮 คำนวณเงินได้สุทธิ
</button>

<div class="net-summary">

    <div class="money-card">

        <div class="money-card-title">
            รายได้รวม
        </div>

        <div
            class="money-card-value"
            id="farmGrossIncome">
            ฿0
        </div>

    </div>

    <div class="money-card">

        <div class="money-card-title">
            ต้นทุนรวม
        </div>

        <div
            class="money-card-value"
            id="farmTotalCost">
            ฿0
        </div>

    </div>

    <div class="money-card">

        <div class="money-card-title">
            เงินได้สุทธิ
        </div>

        <div
            class="money-card-value"
            id="farmNetIncome">
            ฿0
        </div>

    </div>

</div>
```

</section>

<!-- FOOTER -->

<footer class="footer">

```
จัดทำโดย<br>

<strong>นางสาวศุภัชญา หนูเล็ก</strong><br>

SCBT 6705043
```

</footer>

</main>

<!-- MODAL -->

<div
    class="modal"
    id="modal"
    onclick="closeModalOutside(event)">

```
<div class="modal-box">

    <button
        class="modal-close"
        onclick="closeModal()">
        ×
    </button>

    <div id="modalContent"></div>

</div>
```

</div>

<script>

/* =====================================================
   CROP DATABASE
===================================================== */

const agroData = [

{
id:"maryongchid",
name:"มะยงชิด",
emoji:"🥭",
layer:"tall",
layerName:"สูง",
height:"4–7 ม.",
firstIncome:"3–4 ปี",
incomeMonths:42,
harvest:[2,3,4],
price_2023:120,
price_2024:150,
price_2025:135,
yieldPerPlant:50,
note:"ผลผลิตตามฤดูกาล เกรดพรีเมียมสามารถมีราคาสูงกว่าราคากลาง",
channels:["ตลาด","ค้าส่ง","ส่งออก","แปรรูป"]
},

{
id:"mango",
name:"มะม่วงน้ำดอกไม้",
emoji:"🥭",
layer:"tall",
layerName:"สูง",
height:"5–10 ม.",
firstIncome:"3–4 ปี",
incomeMonths:42,
harvest:[1,2,3,4,5,11,12],
price_2023:45,
price_2024:50,
price_2025:48,
yieldPerPlant:40,
note:"ราคาขึ้นกับเกรด คุณภาพ และช่วงฤดูกาล",
channels:["ตลาด","ค้าส่ง","ส่งออก","แปรรูป"]
},

{
id:"jackfruit",
name:"ขนุน",
emoji:"🍈",
layer:"tall",
layerName:"สูง",
height:"5–12 ม.",
firstIncome:"2–3 ปี",
incomeMonths:30,
harvest:[1,2,3,4,5,6,7,8,9,10,11,12],
price_2023:20,
price_2024:22,
price_2025:25,
yieldPerPlant:80,
note:"สามารถให้ผลผลิตได้หลายช่วง ขึ้นกับพันธุ์และการจัดการ",
channels:["ตลาด","ค้าส่ง","แปรรูป"]
},

{
id:"mangosteen",
name:"มังคุด",
emoji:"🟣",
layer:"tall",
layerName:"สูง",
height:"6–12 ม.",
firstIncome:"5–7 ปี",
incomeMonths:72,
harvest:[5,6,7,8],
price_2023:55,
price_2024:60,
price_2025:65,
yieldPerPlant:50,
note:"ราคาผันผวนตามปริมาณผลผลิตและคุณภาพ",
channels:["ตลาด","ค้าส่ง","ส่งออก","แปรรูป"]
},

{
id:"mulberry",
name:"หม่อน",
emoji:"🫐",
layer:"medium",
layerName:"กลาง",
height:"2–4 ม.",
firstIncome:"1–2 ปี",
incomeMonths:18,
harvest:[1,2,3,4,5,6,7,8,9,10,11,12],
price_2023:120,
price_2024:125,
price_2025:130,
yieldPerPlant:5,
note:"สามารถใช้ผลหรือใบเป็นวัตถุดิบสำหรับการแปรรูป",
channels:["ตลาด","แปรรูป","เครื่องดื่ม"]
},

{
id:"lime",
name:"มะนาว",
emoji:"🍋",
layer:"medium",
layerName:"กลาง",
height:"2–4 ม.",
firstIncome:"1–2 ปี",
incomeMonths:18,
harvest:[1,2,3,4,5,6,7,8,9,10,11,12],
price_2023:40,
price_2024:45,
price_2025:50,
yieldPerPlant:40,
note:"ราคามักผันผวนมากในช่วงที่ผลผลิตขาดตลาด",
channels:["ตลาด","ค้าปลีก","แปรรูป"]
},

{
id:"guava",
name:"ฝรั่ง",
emoji:"🍐",
layer:"medium",
layerName:"กลาง",
height:"2–4 ม.",
firstIncome:"1–2 ปี",
incomeMonths:18,
harvest:[1,2,3,4,5,6,7,8,9,10,11,12],
price_2023:25,
price_2024:30,
price_2025:28,
yieldPerPlant:50,
note:"ราคาขึ้นกับพันธุ์ ขนาด และคุณภาพผล",
channels:["ตลาด","ค้าปลีก","แปรรูป"]
},

{
id:"chili",
name:"พริกจินดาแดง",
emoji:"🌶️",
layer:"medium",
layerName:"กลาง",
height:"0.5–1.5 ม.",
firstIncome:"3–4 เดือน",
incomeMonths:4,
harvest:[1,2,3,4,5,6,7,8,9,10,11,12],
price_2023:70,
price_2024:85,
price_2025:80,
yieldPerPlant:2,
note:"ราคาขึ้นลงตามสภาพอากาศ โรคแมลง และปริมาณผลผลิต",
channels:["ตลาด","ผู้รับซื้อ","แปรรูป"]
},

{
id:"stevia",
name:"หญ้าหวาน",
emoji:"🌿",
layer:"low",
layerName:"ต่ำ",
height:"0.3–0.8 ม.",
firstIncome:"3–4 เดือน",
incomeMonths:4,
harvest:[1,2,3,4,5,6,7,8,9,10,11,12],
price_2023:150,
price_2024:160,
price_2025:155,
yieldPerPlant:0.5,
note:"ราคาขึ้นกับรูปแบบสินค้า เช่น ใบสดหรือใบแห้ง",
channels:["แปรรูป","สมุนไพร","ค้าปลีก"]
},

{
id:"basil",
name:"กะเพรา",
emoji:"🌱",
layer:"low",
layerName:"ต่ำ",
height:"0.3–0.8 ม.",
firstIncome:"2–3 เดือน",
incomeMonths:3,
harvest:[1,2,3,4,5,6,7,8,9,10,11,12],
price_2023:30,
price_2024:35,
price_2025:40,
yieldPerPlant:2,
note:"เหมาะกับตลาดสด ร้านอาหาร และตลาดค้าปลีก",
channels:["ตลาด","ร้านอาหาร","ค้าปลีก"]
},

{
id:"pandan",
name:"ใบเตย",
emoji:"🌾",
layer:"low",
layerName:"ต่ำ",
height:"0.5–1 ม.",
firstIncome:"6–12 เดือน",
incomeMonths:9,
harvest:[1,2,3,4,5,6,7,8,9,10,11,12],
price_2023:25,
price_2024:28,
price_2025:30,
yieldPerPlant:3,
note:"สามารถจำหน่ายสดหรือใช้เป็นวัตถุดิบแปรรูป",
channels:["ตลาด","ร้านอาหาร","แปรรูป"]
}

];


/* =====================================================
   FARM DATA
===================================================== */

let farmData =
    Array.from(
        {length:100},
        () => ({crops:[]})
    );

let currentPlot = null;
let priceChart = null;
let yearlyBarChart = null;
let yearlyPointChart = null;


/* =====================================================
   STORAGE
===================================================== */

function saveData(){

    localStorage.setItem(
        "smartFarmFinal",
        JSON.stringify(farmData)
    );

}

function loadData(){

    const saved =
        localStorage.getItem("smartFarmFinal");

    if(!saved) return;

    try{

        const parsed = JSON.parse(saved);

        if(Array.isArray(parsed) && parsed.length === 100){

            farmData =
                parsed.map(plot=>{

                    if(!plot || !Array.isArray(plot.crops)){
                        return {crops:[]};
                    }

                    return plot;

                });

        }

    }catch(error){

        console.warn("ไม่สามารถอ่านข้อมูลเดิมได้");

    }

}


/* =====================================================
   HELPERS
===================================================== */

function getCrop(id){

    return agroData.find(
        crop=>crop.id===id
    );

}

function formatMoney(value){

    return Number(value).toLocaleString(
        "th-TH",
        {
            minimumFractionDigits:2,
            maximumFractionDigits:2
        }
    );

}

function getLatestPrice(crop){

    if(crop.price_2025 != null)
        return {year:2025,price:crop.price_2025};

    if(crop.price_2024 != null)
        return {year:2024,price:crop.price_2024};

    if(crop.price_2023 != null)
        return {year:2023,price:crop.price_2023};

    return null;

}

function getCropColor(crop){

    if(crop.layer==="tall") return "#2e7d32";
    if(crop.layer==="medium") return "#7cb342";

    return "#f9a825";

}


/* =====================================================
   FARM MAP
===================================================== */

function renderFarm(){

    const map =
        document.getElementById("farmMap");

    map.innerHTML = "";

    farmData.forEach((plot,index)=>{

        const div =
            document.createElement("div");

        div.className = "plot";

        if(index===currentPlot)
            div.classList.add("selected");

        div.onclick =
            ()=>openPlot(index);

        div.innerHTML = `
            <div class="plot-number">
                ${index+1}
            </div>

            <div class="plot-plants"></div>
        `;

        const plantContainer =
            div.querySelector(".plot-plants");

        plot.crops.forEach(item=>{

            const crop =
                getCrop(item.cropId);

            if(!crop) return;

            const count =
                Math.min(item.quantity,4);

            for(let i=0;i<count;i++){

                const plant =
                    document.createElement("div");

                plant.className =
                    `plant-mini ${crop.layer}`;

                plant.title =
                    `${crop.name} ${item.quantity} ต้น/กอ`;

                plant.style.background =
                    getCropColor(crop);

                plantContainer.appendChild(plant);

            }

        });

        map.appendChild(div);

    });

}


/* =====================================================
   OPEN PLOT
===================================================== */

function openPlot(index){

    currentPlot = index;

    document.getElementById(
        "selectedPlotDisplay"
    ).value =
        `แปลงที่ ${index+1}`;

    renderFarm();
    renderSelectedCrops();
    renderScene();
    renderPlotModal();

    document.getElementById("modal")
        .classList.add("show");

}


/* =====================================================
   ADD
===================================================== */

function addCropToPlot(){

    if(currentPlot===null){

        alert("กรุณาคลิกเลือกแปลงก่อน");
        return;

    }

    const cropId =
        document.getElementById(
            "cropSelect"
        ).value;

    const quantity =
        parseInt(
            document.getElementById(
                "quantityInput"
            ).value
        );

    if(!cropId || quantity<1){

        alert("กรุณาเลือกพืชและจำนวน");
        return;

    }

    const plot =
        farmData[currentPlot];

    const existing =
        plot.crops.find(
            c=>c.cropId===cropId
        );

    if(existing)
        existing.quantity += quantity;
    else
        plot.crops.push({
            cropId,
            quantity
        });

    saveData();

    renderFarm();
    renderSelectedCrops();
    renderScene();
    renderPlotModal();
    updateDashboard();
    renderQuickInsights();

}


/* =====================================================
   CHANGE QUANTITY
===================================================== */

function changeCropQty(cropId,delta){

    if(currentPlot===null) return;

    const plot =
        farmData[currentPlot];

    const item =
        plot.crops.find(
            c=>c.cropId===cropId
        );

    if(!item) return;

    item.quantity += delta;

    if(item.quantity<=0){

        plot.crops =
            plot.crops.filter(
                c=>c.cropId!==cropId
            );

    }

    saveData();

    renderFarm();
    renderSelectedCrops();
    renderScene();
    renderPlotModal();
    updateDashboard();
    renderQuickInsights();

}


/* =====================================================
   REMOVE
===================================================== */

function removeCrop(cropId){

    if(currentPlot===null) return;

    farmData[currentPlot].crops =
        farmData[currentPlot].crops.filter(
            c=>c.cropId!==cropId
        );

    saveData();

    renderFarm();
    renderSelectedCrops();
    renderScene();
    renderPlotModal();
    updateDashboard();
    renderQuickInsights();

}


/* =====================================================
   CLEAR
===================================================== */

function clearCurrentPlot(){

    if(currentPlot===null){

        alert("กรุณาเลือกแปลงก่อน");
        return;

    }

    if(!confirm(
        `ต้องการล้างข้อมูลแปลงที่ ${currentPlot+1} หรือไม่?`
    )) return;

    farmData[currentPlot]={
        crops:[]
    };

    saveData();

    renderFarm();
    renderSelectedCrops();
    renderScene();
    renderPlotModal();
    updateDashboard();
    renderQuickInsights();

}


/* =====================================================
   SELECTED CROPS
===================================================== */

function renderSelectedCrops(){

    const box =
        document.getElementById(
            "selectedCrops"
        );

    if(currentPlot===null){

        box.innerHTML =
            `<div class="empty">
                คลิกแปลงเพื่อเริ่มจัดวาง
            </div>`;

        return;

    }

    const crops =
        farmData[currentPlot].crops;

    if(crops.length===0){

        box.innerHTML =
            `<div class="empty">
                แปลงนี้ยังไม่มีพืช
            </div>`;

        return;

    }

    box.innerHTML = "";

    crops.forEach(item=>{

        const crop =
            getCrop(item.cropId);

        if(!crop) return;

        const row =
            document.createElement("div");

        row.className="crop-row";

        row.innerHTML = `

            <div class="crop-row-left">

                <div class="crop-emoji">
                    ${crop.emoji}
                </div>

                <div>

                    <div class="crop-name">
                        ${crop.name}
                    </div>

                    <div class="crop-meta">
                        ${crop.layerName}
                        • ${crop.height}
                    </div>

                </div>

            </div>

            <div class="qty-controls">

                <button
                    class="qty-btn"
                    onclick="changeCropQty('${crop.id}',-1)">
                    −
                </button>

                <span class="qty-number">
                    ${item.quantity}
                </span>

                <button
                    class="qty-btn"
                    onclick="changeCropQty('${crop.id}',1)">
                    +
                </button>

            </div>
        `;

        box.appendChild(row);

    });

}


/* =====================================================
   2D SCENE
===================================================== */

function renderScene(){

    const container =
        document.getElementById(
            "scenePlants"
        );

    container.innerHTML="";

    if(currentPlot===null){

        showSceneEmpty();
        return;

    }

    const crops =
        farmData[currentPlot].crops;

    if(crops.length===0){

        showSceneEmpty();
        return;

    }

    let totalShown=0;

    crops.forEach(item=>{

        const crop =
            getCrop(item.cropId);

        if(!crop) return;

        const remaining =
            30-totalShown;

        const count =
            Math.min(
                item.quantity,
                Math.max(0,remaining)
            );

        for(let i=0;i<count;i++){

            const plant =
                document.createElement("div");

            plant.className =
                `big-plant ${crop.layer}`;

            plant.innerHTML = `

                <div class="scene-label">

                    ${crop.name}<br>

                    ${crop.layerName}
                    • ${crop.height}

                </div>

                <div
                    class="big-crown crown-${crop.id}">
                </div>

                <div class="big-trunk"></div>
            `;

            container.appendChild(plant);

            totalShown++;

        }

    });

    renderSceneInfo();

}

function showSceneEmpty(){

    document.getElementById(
        "scenePlants"
    ).innerHTML = `
        <div class="empty"
             style="color:#49604e">
            🌱 เลือกแปลงเพื่อดูภาพ 2 มิติ
        </div>
    `;

    renderSceneInfo();

}

function renderSceneInfo(){

    const box =
        document.getElementById(
            "scenePlotSummary"
        );

    if(currentPlot===null){

        box.innerHTML="";
        return;

    }

    const crops =
        farmData[currentPlot].crops;

    if(crops.length===0){

        box.innerHTML = `
            <div class="detail-section">

                <strong>
                    แปลงที่ ${currentPlot+1}
                </strong>

                <p style="font-size:12px;color:#718078">
                    ยังไม่ได้จัดวางพืช
                </p>

            </div>
        `;

        return;

    }

    box.innerHTML = `
        <div class="detail-section">

            <div class="detail-section-title">
                แปลงที่ ${currentPlot+1}
            </div>

            ${crops.map(item=>{

                const crop =
                    getCrop(item.cropId);

                return `

                    <div style="
                        display:flex;
                        justify-content:space-between;
                        font-size:12px;
                        padding:5px 0;
                        border-bottom:1px solid #e7ece7;
                    ">

                        <span>
                            ${crop.emoji}
                            ${crop.name}
                        </span>

                        <strong>
                            ${item.quantity} ต้น/กอ
                        </strong>

                    </div>

                `;

            }).join("")}

        </div>
    `;

}


/* =====================================================
   MODAL
===================================================== */

function renderPlotModal(){

    if(currentPlot===null) return;

    const plot =
        farmData[currentPlot];

    document.getElementById(
        "modalContent"
    ).innerHTML = `

        <div class="detail-header">

            <div class="detail-emoji">
                🗺️
            </div>

            <div>

                <div class="modal-title">
                    แปลงที่ ${currentPlot+1}
                </div>

                <div style="
                    font-size:12px;
                    color:#718078">
                    จัดการพืชในแปลงนี้
                </div>

            </div>

        </div>

        <div class="plot-editor">

            <select id="modalCropSelect">

                ${agroData.map(crop=>`

                    <option value="${crop.id}">
                        ${crop.emoji}
                        ${crop.name}
                        (${crop.layerName})
                    </option>

                `).join("")}

            </select>

            <input
                type="number"
                id="modalQuantity"
                value="1"
                min="1"
            >

            <button
                class="btn btn-primary"
                onclick="addCropFromModal()">
                เพิ่ม
            </button>

        </div>

        <div class="detail-section-title">
            🌱 พืชที่ปลูกในแปลง
        </div>

        ${
            plot.crops.length===0
            ?
            `<div class="empty">
                ยังไม่มีพืชในแปลงนี้
            </div>`
            :
            plot.crops.map(item=>{

                const crop =
                    getCrop(item.cropId);

                return `

                    <div class="crop-row">

                        <div class="crop-row-left">

                            <div class="crop-emoji">
                                ${crop.emoji}
                            </div>

                            <div>

                                <div class="crop-name">
                                    ${crop.name}
                                </div>

                                <div class="crop-meta">
                                    ชั้น${crop.layerName}
                                    • สูง ${crop.height}
                                </div>

                            </div>

                        </div>

                        <div class="qty-controls">

                            <button
                                class="qty-btn"
                                onclick="changeCropQty('${crop.id}',-1)">
                                −
                            </button>

                            <span class="qty-number">
                                ${item.quantity}
                            </span>

                            <button
                                class="qty-btn"
                                onclick="changeCropQty('${crop.id}',1)">
                                +
                            </button>

                            <button
                                class="qty-btn"
                                style="
                                    background:#ffebee;
                                    color:#c62828"
                                onclick="removeCrop('${crop.id}')">
                                ×
                            </button>

                        </div>

                    </div>
                `;

            }).join("")
        }

    `;

}

function addCropFromModal(){

    const cropId =
        document.getElementById(
            "modalCropSelect"
        ).value;

    const quantity =
        parseInt(
            document.getElementById(
                "modalQuantity"
            ).value
        );

    if(!cropId || quantity<1) return;

    const existing =
        farmData[currentPlot].crops.find(
            c=>c.cropId===cropId
        );

    if(existing)
        existing.quantity += quantity;
    else
        farmData[currentPlot].crops.push({
            cropId,
            quantity
        });

    saveData();

    renderFarm();
    renderSelectedCrops();
    renderScene();
    renderPlotModal();
    updateDashboard();
    renderQuickInsights();

}

function closeModal(){

    document.getElementById(
        "modal"
    ).classList.remove("show");

}

function closeModalOutside(event){

    if(event.target.id==="modal")
        closeModal();

}

document.addEventListener(
    "keydown",
    event=>{

        if(event.key==="Escape")
            closeModal();

    }
);


/* =====================================================
   FULL 100 PLOT SAMPLE
===================================================== */

function resetFarm(){

    farmData =
        Array.from(
            {length:100},
            ()=>({crops:[]})
        );

}


/*
 * กระจายพืชลงครบทั้ง 100 ช่อง
 * แต่ละช่องมีพืชหลักเพียงชนิดเดียว
 * เพื่อให้แผนผังอ่านง่ายและไม่ยัดพืชซ้อนกัน
 */

function loadFullSampleFarm(type="A"){

    resetFarm();

    let sequence = [];

    if(type==="A"){

        sequence = [

            "mango",
            "maryongchid",
            "jackfruit",
            "mangosteen",

            "lime",
            "guava",
            "mulberry",

            "chili",
            "stevia",
            "basil",
            "pandan"

        ];

    }

    if(type==="B"){

        sequence = [

            "mango",
            "jackfruit",

            "lime",
            "guava",
            "chili",

            "stevia",
            "basil",
            "pandan",

            "maryongchid",
            "mulberry"

        ];

    }

    if(type==="C"){

        sequence = [

            "maryongchid",
            "mango",
            "jackfruit",
            "mangosteen",

            "mulberry",
            "lime",
            "guava",
            "chili",

            "stevia",
            "basil",
            "pandan"

        ];

    }

    /*
     * ทุกช่อง 1 ถึง 100 จะได้รับพืช
     */

    for(let i=0;i<100;i++){

        const cropId =
            sequence[i % sequence.length];

        farmData[i]={
            crops:[
                {
                    cropId,
                    quantity:getSampleQuantity(cropId,i)
                }
            ]
        };

    }

    saveData();

    currentPlot=0;

    document.getElementById(
        "selectedPlotDisplay"
    ).value="แปลงที่ 1";

    renderFarm();
    renderSelectedCrops();
    renderScene();
    renderPlotModal();
    updateDashboard();
    renderQuickInsights();

    calculateFarmNetIncome();

    closeModal();

}

function getSampleQuantity(cropId,index){

    const crop =
        getCrop(cropId);

    if(crop.layer==="tall")
        return 1;

    if(crop.layer==="medium")
        return 2;

    return 5;

}


/* =====================================================
   DASHBOARD
===================================================== */

function updateDashboard(){

    let used=0;
    let total=0;

    farmData.forEach(plot=>{

        if(plot.crops.length>0)
            used++;

        plot.crops.forEach(item=>{
            total+=Number(item.quantity);
        });

    });

    document.getElementById(
        "usedPlots"
    ).textContent=used;

    document.getElementById(
        "totalPlants"
    ).textContent=
        total.toLocaleString("th-TH");

}


/* =====================================================
   CROP SELECT
===================================================== */

function populateCropSelect(){

    const select =
        document.getElementById(
            "cropSelect"
        );

    select.innerHTML = `

        <option value="">
            -- เลือกพืช --
        </option>

        ${agroData.map(crop=>`

            <option value="${crop.id}">
                ${crop.emoji}
                ${crop.name}
                • ${crop.layerName}
            </option>

        `).join("")}

    `;

}


/* =====================================================
   BUSINESS
===================================================== */

let currentBusinessTab="crops";

function setBusinessTab(tab,button){

    currentBusinessTab=tab;

    document
        .querySelectorAll(".business-tab")
        .forEach(btn=>{
            btn.classList.remove("active");
        });

    if(button)
        button.classList.add("active");

    renderBusiness();

}

function renderBusiness(){

    renderQuickInsights();

    const content =
        document.getElementById(
            "businessContent"
        );

    if(currentBusinessTab==="crops")
        renderCropExplorer(content);

    if(currentBusinessTab==="revenue")
        renderIncomeBusiness(content);

    if(currentBusinessTab==="harvest")
        renderHarvestBusiness(content);

    if(currentBusinessTab==="price")
        renderPriceBusiness(content);

    if(currentBusinessTab==="market")
        renderMarketBusiness(content);

    if(currentBusinessTab==="longterm")
        renderLongTermBusiness(content);

}


/* =====================================================
   QUICK INSIGHTS
===================================================== */

function renderQuickInsights(){

    const box =
        document.getElementById(
            "quickInsights"
        );

    const planted =
        new Set();

    farmData.forEach(plot=>{

        plot.crops.forEach(item=>{
            planted.add(item.cropId);
        });

    });

    const fastest =
        [...agroData]
            .sort(
                (a,b)=>
                    a.incomeMonths-b.incomeMonths
            )[0];

    box.innerHTML = `

        <div class="insight">

            <div class="insight-label">
                พืชที่ปลูกแล้ว
            </div>

            <div class="insight-value">
                ${planted.size} ชนิด
            </div>

        </div>

        <div class="insight">

            <div class="insight-label">
                รายได้เร็วสุด
            </div>

            <div class="insight-value">
                ${fastest.emoji}
                ${fastest.name}
            </div>

        </div>

        <div class="insight">

            <div class="insight-label">
                แปลงที่มีพืช
            </div>

            <div class="insight-value">
                ${farmData.filter(p=>p.crops.length>0).length}/100
            </div>

        </div>

        <div class="insight">

            <div class="insight-label">
                พืชทั้งหมดในระบบ
            </div>

            <div class="insight-value">
                ${agroData.length} ชนิด
            </div>

        </div>

    `;

}


/* =====================================================
   CROP EXPLORER
===================================================== */

function renderCropExplorer(content){

    content.innerHTML = `

        <div class="crop-tools">

            <input
                type="text"
                id="cropSearch"
                placeholder="🔎 ค้นหาพืช..."
                oninput="filterCropCards()"
            >

            <select
                id="cropFilter"
                onchange="filterCropCards()">

                <option value="all">
                    พืชทั้งหมด
                </option>

                <option value="planted">
                    ปลูกแล้ว
                </option>

                <option value="fast">
                    รายได้เร็ว
                </option>

                <option value="price">
                    มีข้อมูลราคา
                </option>

            </select>

        </div>

        <div
            class="crop-grid"
            id="cropGrid">
        </div>

    `;

    renderCropCards();

}

function getPlantedCropIds(){

    const ids=new Set();

    farmData.forEach(plot=>{

        plot.crops.forEach(item=>{
            ids.add(item.cropId);
        });

    });

    return ids;

}

function renderCropCards(){

    const grid =
        document.getElementById(
            "cropGrid"
        );

    if(!grid) return;

    const search =
        (
            document.getElementById(
                "cropSearch"
            )?.value || ""
        ).toLowerCase();

    const filter =
        document.getElementById(
            "cropFilter"
        )?.value || "all";

    const planted =
        getPlantedCropIds();

    const filtered =
        agroData.filter(crop=>{

            const matchSearch =
                crop.name
                    .toLowerCase()
                    .includes(search);

            let matchFilter=true;

            if(filter==="planted")
                matchFilter=planted.has(crop.id);

            if(filter==="fast")
                matchFilter=crop.incomeMonths<=12;

            if(filter==="price")
                matchFilter=getLatestPrice(crop)!==null;

            return matchSearch && matchFilter;

        });

    grid.innerHTML =
        filtered.map(crop=>{

            const latest =
                getLatestPrice(crop);

            return `

                <div class="crop-card">

                    <div class="crop-card-top">

                        <div class="crop-card-emoji">
                            ${crop.emoji}
                        </div>

                        <div>

                            <div class="crop-card-name">
                                ${crop.name}
                            </div>

                            <div class="crop-layer">
                                ชั้น${crop.layerName}
                                • ${crop.height}
                            </div>

                        </div>

                    </div>

                    <div class="crop-stat">
                        <span>เริ่มมีรายได้</span>
                        <span>${crop.firstIncome}</span>
                    </div>

                    <div class="crop-stat">
                        <span>ผลผลิตแบบจำลอง/ต้น</span>
                        <span>${crop.yieldPerPlant} กก.</span>
                    </div>

                    <div class="crop-stat">
                        <span>ราคา 2025</span>
                        <span>
                            ${latest ? latest.price+" ฿/กก." : "-"}
                        </span>
                    </div>

                    <div class="crop-buttons">

                        <button
                            class="btn btn-light"
                            onclick="showCropDetail('${crop.id}')">
                            ดูรายละเอียด
                        </button>

                    </div>

                </div>

            `;

        }).join("");

}

function filterCropCards(){
    renderCropCards();
}


/* =====================================================
   CROP DETAIL
===================================================== */

function showCropDetail(cropId){

    const crop=getCrop(cropId);

    if(!crop) return;

    const latest=getLatestPrice(crop);

    document.getElementById(
        "modalContent"
    ).innerHTML = `

        <div class="detail-header">

            <div class="detail-emoji">
                ${crop.emoji}
            </div>

            <div>

                <div class="modal-title">
                    ${crop.name}
                </div>

                <div style="
                    color:#718078;
                    font-size:12px">
                    ชั้น${crop.layerName}
                    • ความสูง ${crop.height}
                </div>

            </div>

        </div>

        <div class="detail-stats">

            <div class="detail-stat">
                <small>เริ่มมีรายได้</small>
                <strong>${crop.firstIncome}</strong>
            </div>

            <div class="detail-stat">
                <small>เก็บเกี่ยว</small>
                <strong>${getHarvestText(crop)}</strong>
            </div>

            <div class="detail-stat">
                <small>ผลผลิตแบบจำลอง</small>
                <strong>${crop.yieldPerPlant} กก.</strong>
            </div>

            <div class="detail-stat">
                <small>ราคา 2025</small>
                <strong>${latest?.price ?? "-"}</strong>
            </div>

        </div>

        <div class="detail-section">

            <div class="detail-section-title">
                🏪 ช่องทางจำหน่าย
            </div>

            ${crop.channels.map(
                channel=>
                    `<span class="tag">${channel}</span>`
            ).join("")}

        </div>

        <div class="detail-section">

            <div class="detail-section-title">
                📌 หมายเหตุ
            </div>

            <div style="
                background:#f7faf7;
                border-radius:10px;
                padding:12px;
                font-size:13px;
                color:#66736a">

                ${crop.note}

            </div>

        </div>

        <div class="detail-section">

            <div class="detail-section-title">
                💵 ราคาอ้างอิงรายปี
            </div>

            <div style="
                display:grid;
                grid-template-columns:repeat(3,1fr);
                gap:8px">

                <div class="detail-stat">
                    <small>2023</small>
                    <strong>${crop.price_2023}</strong>
                </div>

                <div class="detail-stat">
                    <small>2024</small>
                    <strong>${crop.price_2024}</strong>
                </div>

                <div class="detail-stat">
                    <small>2025</small>
                    <strong>${crop.price_2025}</strong>
                </div>

            </div>

        </div>

    `;

    document.getElementById(
        "modal"
    ).classList.add("show");

}

function getHarvestText(crop){

    if(crop.harvest.length===12)
        return "ตลอดปี";

    const months=[
        "",
        "ม.ค.","ก.พ.","มี.ค.",
        "เม.ย.","พ.ค.","มิ.ย.",
        "ก.ค.","ส.ค.","ก.ย.",
        "ต.ค.","พ.ย.","ธ.ค."
    ];

    return crop.harvest
        .map(m=>months[m])
        .join(" ");

}


/* =====================================================
   INCOME TAB
===================================================== */

function renderIncomeBusiness(content){

    content.innerHTML = `

        <div style="
            background:#f7faf7;
            border-radius:12px;
            padding:15px;
            margin-bottom:15px">

            <div style="
                font-weight:600;
                color:#1b5e20">

                💰 คำนวณรายได้ด้วยตัวเอง

            </div>

            <div style="
                font-size:12px;
                color:#718078">

                ปริมาณผลผลิต × ราคาที่ใช้

            </div>

        </div>

        <div class="revenue-grid">

            <div>

                <label style="font-size:12px;color:#718078">
                    ผลผลิต
                </label>

                <select
                    id="revenueCrop"
                    onchange="updateRevenuePrice()">

                    ${agroData.map(crop=>`

                        <option value="${crop.id}">
                            ${crop.emoji}
                            ${crop.name}
                        </option>

                    `).join("")}

                </select>

            </div>

            <div>

                <label style="font-size:12px;color:#718078">
                    ปริมาณผลผลิต (กก.)
                </label>

                <input
                    type="number"
                    id="revenueYield"
                    min="0"
                    step="any"
                    placeholder="เช่น 1000">

            </div>

            <div>

                <label style="font-size:12px;color:#718078">
                    ราคาที่ใช้ (บาท/กก.)
                </label>

                <input
                    type="number"
                    id="revenuePrice"
                    min="0"
                    step="any">

            </div>

            <button
                class="btn btn-primary"
                onclick="calculateRevenue()">

                🧮 คำนวณ

            </button>

        </div>

        <div
            class="revenue-result"
            id="revenueResult">

            <div>รายได้ประเมิน</div>

            <div
                class="revenue-number"
                id="revenueOutput">
                ฿0
            </div>

            <div
                class="revenue-detail"
                id="revenueDetail">
            </div>

        </div>

        <div style="
            margin-top:20px;
            padding:13px;
            background:#fff8e1;
            border-radius:10px;
            font-size:12px;
            color:#725f20">

            ⚠️ ราคาที่ใช้เป็นข้อมูลตั้งต้นในระบบ
            ส่วนผลผลิต/ต้นเป็นค่าที่ใช้สำหรับแบบจำลอง
            สามารถเปลี่ยนข้อมูลได้ในโค้ดส่วน
            <strong>yieldPerPlant</strong>

        </div>

    `;

    updateRevenuePrice();

}

function updateRevenuePrice(){

    const cropSelect =
        document.getElementById(
            "revenueCrop"
        );

    const priceInput =
        document.getElementById(
            "revenuePrice"
        );

    if(!cropSelect || !priceInput) return;

    const crop =
        getCrop(cropSelect.value);

    const latest =
        getLatestPrice(crop);

    priceInput.value =
        latest ? latest.price : "";

}

function calculateRevenue(){

    const crop =
        getCrop(
            document.getElementById(
                "revenueCrop"
            ).value
        );

    const quantity =
        parseFloat(
            document.getElementById(
                "revenueYield"
            ).value
        );

    const price =
        parseFloat(
            document.getElementById(
                "revenuePrice"
            ).value
        );

    if(!crop || isNaN(quantity) || isNaN(price)){

        alert(
            "กรุณากรอกปริมาณผลผลิตและราคาให้ครบ"
        );

        return;

    }

    const revenue =
        quantity*price;

    document.getElementById(
        "revenueOutput"
    ).textContent =
        `฿${formatMoney(revenue)}`;

    document.getElementById(
        "revenueDetail"
    ).textContent =
        `${crop.name} ${quantity.toLocaleString("th-TH")} กก.
        × ${price.toLocaleString("th-TH")} บาท/กก.`;

    document.getElementById(
        "revenueResult"
    ).style.display="block";

}


/* =====================================================
   HARVEST
===================================================== */

function renderHarvestBusiness(content){

    const months=[
        "ม.ค.","ก.พ.","มี.ค.","เม.ย.",
        "พ.ค.","มิ.ย.","ก.ค.","ส.ค.",
        "ก.ย.","ต.ค.","พ.ย.","ธ.ค."
    ];

    content.innerHTML=`

        <div style="overflow-x:auto">

            <div class="harvest-grid">

                ${months.map(
                    (month,index)=>{

                        const monthNumber=index+1;

                        const crops =
                            agroData.filter(
                                crop=>
                                    crop.harvest.includes(
                                        monthNumber
                                    )
                            );

                        return `

                            <div class="harvest-month">

                                <div class="harvest-month-title">
                                    ${month}
                                </div>

                                ${
                                    crops.map(
                                        crop=>
                                        `
                                        <button
                                            class="harvest-chip"
                                            onclick="showCropDetail('${crop.id}')">

                                            ${crop.emoji}
                                            ${crop.name}

                                        </button>
                                        `
                                    ).join("")
                                }

                            </div>

                        `;

                    }
                ).join("")}

            </div>

        </div>

    `;

}


/* =====================================================
   PRICE
===================================================== */

function renderPriceBusiness(content){

    content.innerHTML=`

        <div class="price-layout">

            <div class="price-selector">

                <label style="
                    font-size:12px;
                    color:#6d796f">

                    เลือกพืชเพื่อดูกราฟราคา

                </label>

                <select
                    id="priceCropSelect"
                    onchange="updatePriceChart()"
                    style="margin-top:6px">

                    ${agroData.map(crop=>`

                        <option value="${crop.id}">
                            ${crop.emoji}
                            ${crop.name}
                        </option>

                    `).join("")}

                </select>

                <div
                    class="price-current"
                    id="priceCurrent">
                </div>

            </div>

            <div class="chart-container">

                <canvas id="priceChart"></canvas>

            </div>

        </div>

        <div style="
            margin-top:18px">

            <div style="
                font-weight:600;
                color:#1b5e20;
                margin-bottom:10px">

                📊 ราคาแยกตามปี

            </div>

            <div class="year-chart-grid">

                <div class="year-chart-card">

                    <div class="year-chart-title">
                        📊 ปี 2023
                    </div>

                    <div class="year-chart-wrap">

                        <canvas id="priceChart2023"></canvas>

                    </div>

                </div>

                <div class="year-chart-card">

                    <div class="year-chart-title">
                        📊 ปี 2024
                    </div>

                    <div class="year-chart-wrap">

                        <canvas id="priceChart2024"></canvas>

                    </div>

                </div>

                <div class="year-chart-card">

                    <div class="year-chart-title">
                        📊 ปี 2025
                    </div>

                    <div class="year-chart-wrap">

                        <canvas id="priceChart2025"></canvas>

                    </div>

                </div>

            </div>

        </div>

        <div style="
            margin-top:13px;
            background:#fff8e1;
            border-radius:10px;
            padding:11px;
            font-size:12px;
            color:#725f20">

            ⚠️ ราคาในระบบเป็นข้อมูลตั้งต้นเพื่อการวิเคราะห์
            ไม่ใช่การรับประกันราคาที่จะได้รับจริง

        </div>

    `;

    updatePriceChart();
    renderYearPriceCharts();

}

function updatePriceChart(){

    const select =
        document.getElementById(
            "priceCropSelect"
        );

    if(!select) return;

    const crop =
        getCrop(select.value);

    if(!crop) return;

    const canvas =
        document.getElementById(
            "priceChart"
        );

    const current =
        document.getElementById(
            "priceCurrent"
        );

    const latest =
        getLatestPrice(crop);

    current.innerHTML=`

        <div style="
            font-size:12px;
            color:#718078">

            ราคาล่าสุดในฐานข้อมูล

        </div>

        <div class="price-current-number">

            ${latest ? latest.price : "-"}

            <span style="
                font-size:13px;
                font-weight:400">

                บาท/กก.

            </span>

        </div>

        <div style="
            font-size:11px;
            color:#718078">

            ${
                latest
                ?
                `ข้อมูลปี ${latest.year}`
                :
                "ยังไม่มีข้อมูล"
            }

        </div>

    `;

    if(priceChart)
        priceChart.destroy();

    priceChart =
        new Chart(
            canvas.getContext("2d"),
            {
                type:"line",

                data:{

                    labels:[
                        "2023",
                        "2024",
                        "2025"
                    ],

                    datasets:[{

                        label:
                            `${crop.name} ราคา`,

                        data:[
                            crop.price_2023,
                            crop.price_2024,
                            crop.price_2025
                        ],

                        borderWidth:3,
                        tension:.3,
                        pointRadius:6,
                        pointHoverRadius:8,
                        fill:false

                    }]

                },

                options:{

                    responsive:true,
                    maintainAspectRatio:false,

                    scales:{

                        y:{
                            beginAtZero:true,
                            title:{
                                display:true,
                                text:"บาท / กก."
                            }
                        }

                    }

                }

            }
        );

}


/* =====================================================
   YEAR PRICE BAR CHARTS
===================================================== */

function renderYearPriceCharts(){

    const years=[2023,2024,2025];

    years.forEach(year=>{

        const canvas =
            document.getElementById(
                `priceChart${year}`
            );

        if(!canvas) return;

        const rows =
            [...agroData]
                .sort(
                    (a,b)=>
                        b[`price_${year}`]
                        -
                        a[`price_${year}`]
                );

        new Chart(
            canvas.getContext("2d"),
            {
                type:"bar",

                data:{

                    labels:
                        rows.map(
                            crop=>crop.name
                        ),

                    datasets:[{

                        label:
                            `ราคา ${year} บาท/กก.`,

                        data:
                            rows.map(
                                crop=>
                                    crop[`price_${year}`]
                            ),

                        borderWidth:1,
                        borderRadius:5

                    }]

                },

                options:{

                    indexAxis:"y",

                    responsive:true,

                    maintainAspectRatio:false,

                    plugins:{

                        legend:{
                            display:false
                        },

                        tooltip:{

                            callbacks:{

                                label:function(context){

                                    return ` ${context.raw} บาท/กก.`;

                                }

                            }

                        }

                    },

                    scales:{

                        x:{
                            beginAtZero:true,
                            title:{
                                display:true,
                                text:"บาท / กก."
                            }
                        }

                    }

                }

            }
        );

    });

}


/* =====================================================
   MARKET
===================================================== */

function renderMarketBusiness(content){

    const channels=new Set();

    agroData.forEach(
        crop=>
            crop.channels.forEach(
                channel=>
                    channels.add(channel)
            )
    );

    content.innerHTML=`

        <div class="market-grid">

            ${Array.from(channels).map(
                channel=>{

                    const crops =
                        agroData.filter(
                            crop=>
                                crop.channels
                                    .includes(channel)
                        );

                    return `

                        <div class="market-card">

                            <div class="market-title">
                                🏪 ${channel}
                            </div>

                            <div class="market-crops">

                                ${
                                    crops.map(
                                        crop=>
                                        `
                                        <button
                                            class="market-crop"
                                            onclick="showCropDetail('${crop.id}')">

                                            ${crop.emoji}
                                            ${crop.name}

                                        </button>
                                        `
                                    ).join("")
                                }

                            </div>

                        </div>

                    `;

                }
            ).join("")}

        </div>

    `;

}


/* =====================================================
   10 YEAR MODEL
===================================================== */

function getPlantingYearFactor(crop,year){

    const first =
        crop.incomeMonths;

    /*
     * แปลงเดือนเป็นปีโดยประมาณ
     */

    const firstYear =
        Math.ceil(first/12);

    if(year<firstYear)
        return 0;

    /*
     * หลังเริ่มมีรายได้
     * ผลผลิตค่อย ๆ เพิ่มขึ้น
     * จนถึงระดับเต็มแบบจำลอง
     */

    const yearsAfter =
        year-firstYear;

    return Math.min(
        1,
        0.25 + yearsAfter*0.15
    );

}


function calculateYearRevenue(year){

    let total=0;

    farmData.forEach(plot=>{

        plot.crops.forEach(item=>{

            const crop =
                getCrop(item.cropId);

            if(!crop) return;

            const factor =
                getPlantingYearFactor(
                    crop,
                    year
                );

            const price =
                crop.price_2025;

            const yieldKg =
                crop.yieldPerPlant *
                item.quantity *
                factor;

            total +=
                yieldKg*price;

        });

    });

    return total;

}


function calculateYearCost(year){

    const seed =
        Number(
            document.getElementById(
                "costSeed"
            )?.value || 30000
        );

    const water =
        Number(
            document.getElementById(
                "costWater"
            )?.value || 20000
        );

    const labor =
        Number(
            document.getElementById(
                "costLabor"
            )?.value || 40000
        );

    const other =
        Number(
            document.getElementById(
                "costOther"
            )?.value || 30000
        );

    /*
     * ต้นทุนประจำปี
     * ปีแรกมีต้นทุนกล้าสูงกว่า
     */

    let total =
        water+labor+other;

    if(year<=2)
        total += seed;

    return total;

}


function calculateTenYearData(){

    const data=[];

    for(let year=1;year<=10;year++){

        const revenue =
            calculateYearRevenue(year);

        const cost =
            calculateYearCost(year);

        const net =
            revenue-cost;

        const previous =
            data.length
            ?
            data[data.length-1].cumulative
            :
            0;

        data.push({

            year,

            revenue,

            cost,

            net,

            cumulative:
                previous+net

        });

    }

    return data;

}


function renderLongTermBusiness(content){

    const data =
        calculateTenYearData();

    content.innerHTML=`

        <div style="
            background:#f7faf7;
            padding:14px;
            border-radius:12px;
            margin-bottom:15px;
            font-size:13px">

            <strong>📈 แบบจำลองรายได้ 10 ปี</strong>

            <br>

            กราฟแสดงว่าเมื่อปลูกพืชแล้ว
            รายได้ของฟาร์มอาจเพิ่มขึ้นตามการเข้าสู่ช่วงให้ผลผลิต

            <br>

            <span style="color:#718078">
                * เป็นแบบจำลองจากข้อมูลในระบบ
                ไม่ใช่การคาดการณ์รายได้จริง
            </span>

        </div>

        <div class="longterm-grid">

            <div>

                <div style="
                    font-weight:600;
                    color:#1b5e20;
                    margin-bottom:8px">

                    📊 รายได้แต่ละปี

                </div>

                <div class="longterm-chart">

                    <canvas id="yearlyBarChart"></canvas>

                </div>

            </div>

            <div>

                <div style="
                    font-weight:600;
                    color:#1b5e20;
                    margin-bottom:8px">

                    📍 แนวโน้มรายได้ปีที่ 1–10

                </div>

                <div class="longterm-chart">

                    <canvas id="yearlyPointChart"></canvas>

                </div>

            </div>

        </div>

        <div style="
            margin-top:20px;
            font-weight:600;
            color:#1b5e20">

            📋 ตารางรายได้และเงินได้สุทธิ 10 ปี

        </div>

        <div class="longterm-table" style="margin-top:8px">

            <table>

                <thead>

                    <tr>

                        <th>ปี</th>
                        <th>รายได้</th>
                        <th>ต้นทุน</th>
                        <th>เงินได้สุทธิ</th>
                        <th>สะสม</th>

                    </tr>

                </thead>

                <tbody>

                    ${data.map(row=>`

                        <tr>

                            <td>
                                ปี ${row.year}
                            </td>

                            <td>
                                ฿${formatMoney(row.revenue)}
                            </td>

                            <td>
                                ฿${formatMoney(row.cost)}
                            </td>

                            <td>
                                ฿${formatMoney(row.net)}
                            </td>

                            <td>
                                ฿${formatMoney(row.cumulative)}
                            </td>

                        </tr>

                    `).join("")}

                </tbody>

            </table>

        </div>

        <div style="
            margin-top:15px;
            background:#fff8e1;
            padding:12px;
            border-radius:10px;
            font-size:12px;
            color:#725f20">

            💡 วิธีอ่าน:
            ปีแรก ๆ อาจมีรายได้ต่ำหรือยังไม่มีรายได้จากไม้ผลบางชนิด
            เมื่อพืชเริ่มเข้าสู่ช่วงให้ผลผลิต
            รายได้จึงมีแนวโน้มเพิ่มขึ้น

        </div>

    `;

    renderTenYearCharts(data);

}


function renderTenYearCharts(data){

    const barCanvas =
        document.getElementById(
            "yearlyBarChart"
        );

    const pointCanvas =
        document.getElementById(
            "yearlyPointChart"
        );

    if(yearlyBarChart)
        yearlyBarChart.destroy();

    if(yearlyPointChart)
        yearlyPointChart.destroy();

    yearlyBarChart =
        new Chart(
            barCanvas.getContext("2d"),
            {
                type:"bar",

                data:{

                    labels:
                        data.map(
                            row=>`ปี ${row.year}`
                        ),

                    datasets:[

                        {

                            label:"รายได้",

                            data:
                                data.map(
                                    row=>row.revenue
                                ),

                            borderWidth:1,
                            borderRadius:6

                        },

                        {

                            label:"เงินได้สุทธิ",

                            data:
                                data.map(
                                    row=>row.net
                                ),

                            borderWidth:1,
                            borderRadius:6

                        }

                    ]

                },

                options:{

                    responsive:true,
                    maintainAspectRatio:false,

                    scales:{

                        y:{
                            beginAtZero:true,

                            ticks:{

                                callback:function(value){

                                    return
                                        Number(value)
                                        .toLocaleString("th-TH");

                                }

                            },

                            title:{
                                display:true,
                                text:"บาท"
                            }

                        }

                    }

                }

            }
        );


    yearlyPointChart =
        new Chart(
            pointCanvas.getContext("2d"),
            {
                type:"line",

                data:{

                    labels:
                        data.map(
                            row=>`ปี ${row.year}`
                        ),

                    datasets:[{

                        label:
                            "รายได้ต่อปี",

                        data:
                            data.map(
                                row=>row.revenue
                            ),

                        borderWidth:3,
                        pointRadius:7,
                        pointHoverRadius:9,
                        tension:.25,
                        fill:false

                    }]

                },

                options:{

                    responsive:true,
                    maintainAspectRatio:false,

                    scales:{

                        y:{
                            beginAtZero:true,

                            title:{
                                display:true,
                                text:"บาท"
                            }

                        }

                    }

                }

            }
        );

}


/* =====================================================
   NET FARM INCOME
===================================================== */

function calculateFarmNetIncome(){

    /*
     * ใช้รายได้ปีที่ 10
     * เป็นภาพรายได้ของฟาร์มเมื่อพืช
     * ส่วนหนึ่งเข้าสู่ช่วงให้ผลผลิตแล้ว
     */

    const revenue =
        calculateYearRevenue(10);

    const cost =
        calculateYearCost(10);

    const net =
        revenue-cost;

    document.getElementById(
        "farmGrossIncome"
    ).textContent =
        `฿${formatMoney(revenue)}`;

    document.getElementById(
        "farmTotalCost"
    ).textContent =
        `฿${formatMoney(cost)}`;

    document.getElementById(
        "farmNetIncome"
    ).textContent =
        `฿${formatMoney(net)}`;

}


/* =====================================================
   INITIALIZE
===================================================== */

loadData();

populateCropSelect();

renderFarm();

updateDashboard();

renderBusiness();

calculateFarmNetIncome();

</script>

</body>
</html>
