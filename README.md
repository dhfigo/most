<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>팀/파트 매출 통계 | PS사업부</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&family=JetBrains+Mono:wght@400;600&display=swap');
:root{
  --dark2:#2c3e50;--white:#fff;--bg:#f4f5f7;--card:#fff;--border:#dde1e7;
  --text:#1a202c;--muted:#6b7280;--hint:#9ca3af;
  --green:#0f6e56;--blue:#1558a8;--grayb:#3d4451;
  --amber:#854f0b;--oran:#c05010;--red:#a32d2d;
  --team-bg:#e8eaf6;--team-text:#26215c;
  --part-bg:#f3f4fb;--part-text:#3c3489;
  --sum-bg:#f5f5ec;--tot-bg:#fff8e1;
  --mo-bg:#fff9f0;--mo-text:#7a4500;
  --yr-bg:#fdf0f4;--yr-text:#6a0f35;
  --fix-bg:#e3e6ea;--fix-b:#9baabf;--yr-b:#c8a0b4;
  --unit-bg:#fffbea;--unit-border:#e6a817;--unit-text:#7a4500;
  --rh:24px;--hd1h:26px;--hd2h:22px;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:'Noto Sans KR',sans-serif;background:var(--bg);color:var(--text);font-size:13px;}

.page-header{background:var(--dark2);color:#fff;padding:10px 18px;display:flex;align-items:center;gap:12px;box-shadow:0 2px 8px rgba(0,0,0,.2);position:sticky;top:0;z-index:200;}
.logo{font-size:15px;font-weight:700;letter-spacing:-.3px;}.logo span{color:#7ec8b0;}
.header-spacer{flex:1;}
.header-date{font-size:11px;color:rgba(255,255,255,.5);font-family:'JetBrains Mono',monospace;}

.unit-bar{background:var(--unit-bg);border-bottom:1.5px solid var(--unit-border);padding:6px 18px;display:flex;align-items:center;gap:14px;flex-wrap:wrap;}
.year-select-wrap{display:flex;align-items:center;gap:7px;}
.year-select-wrap label{font-size:12px;font-weight:500;color:var(--unit-text);}
.year-select{font-size:12px;font-weight:600;padding:3px 28px 3px 9px;border:1.5px solid var(--unit-border);border-radius:5px;
  background:#fff url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%23e6a817'/%3E%3C/svg%3E") no-repeat right 8px center;
  color:var(--unit-text);appearance:none;cursor:pointer;font-family:'JetBrains Mono',monospace;}
.unit-notice{font-size:12px;font-weight:500;color:var(--unit-text);}
.unit-notice strong{font-weight:700;}
.spacer{flex:1;}
.period-badge{font-size:11px;color:var(--muted);background:rgba(0,0,0,.04);border-radius:4px;padding:3px 8px;font-family:'JetBrains Mono',monospace;}

.container{padding:14px 18px;}
.section-card{background:var(--card);border:1px solid var(--border);border-radius:8px;box-shadow:0 1px 3px rgba(0,0,0,.08);margin-bottom:16px;overflow:hidden;}
.section-hd{background:var(--dark2);color:#fff;padding:8px 14px;display:flex;align-items:center;gap:10px;flex-wrap:wrap;}
.section-hd .title{font-size:13px;font-weight:500;flex:1;min-width:160px;}

.btn{font-size:11px;font-weight:500;padding:4px 12px;border:none;border-radius:5px;cursor:pointer;white-space:nowrap;font-family:'Noto Sans KR',sans-serif;transition:filter .15s,transform .1s;display:inline-flex;align-items:center;gap:4px;}
.btn:hover{filter:brightness(1.12);}.btn:active{transform:scale(.97);}
.btn-green{background:var(--green);color:#fff;}.btn-blue{background:var(--blue);color:#fff;}.btn-gray{background:var(--grayb);color:#fff;}
.btn-sm{font-size:10px;padding:3px 9px;}

/* ═══ 틀고정 핵심 ═══
   .freeze-outer: position:relative; overflow:hidden; height 고정
     ├ .fp-left:  좌측 고정 div (position:absolute; left:0)
     ├ .fp-right: 우측 고정 div (position:absolute; right:0)  
     └ .fp-scroll: 중앙 스크롤 div (position:absolute; overflow:scroll)
   
   모든 테이블은 border-separate + 동일한 행 height(px) 고정
   → tr height가 동일하므로 translateY(-scrollTop)으로 좌/우 패널 행 위치 완벽 동기화
   thead는 각 패널 내부에 position:sticky top:0으로 독립적으로 고정
*/
.freeze-outer{position:relative;overflow:hidden;background:var(--card);}
.fp-left{position:absolute;top:0;left:0;bottom:0;overflow:hidden;z-index:12;border-right:2px solid var(--fix-b);}
.fp-right{position:absolute;top:0;right:0;bottom:0;overflow:hidden;z-index:12;border-left:2px solid var(--yr-b);}
.fp-scroll{position:absolute;top:0;bottom:0;overflow:scroll;}
.fp-scroll::-webkit-scrollbar{width:7px;height:7px;}
.fp-scroll::-webkit-scrollbar-track{background:#eee;}
.fp-scroll::-webkit-scrollbar-thumb{background:#bbb;border-radius:4px;}

/* 패널 내 테이블 */
.fp-left table,.fp-right table,.fp-scroll table{
  border-collapse:separate;border-spacing:0;white-space:nowrap;font-size:11px;
}
/* 공통 th */
.fp-left th,.fp-right th,.fp-scroll th{
  font-size:10px;font-weight:500;text-align:center;padding:0 4px;
  background:var(--fix-bg);color:#263238;
  border-bottom:1px solid #c0c8d0;border-right:0.5px solid #c8d0d8;
  position:sticky;top:0;z-index:6;
}
.fp-scroll th.mo{background:var(--mo-bg);color:var(--mo-text);}
.fp-right th{background:var(--yr-bg);color:var(--yr-text);border-bottom:1px solid #d4a0b4;}
/* 행 높이 — 모든 th/td 동일하게 고정 */
.fp-left th,.fp-right th,.fp-scroll th{height:var(--hd2h);}
.fp-left th.h1,.fp-right th.h1,.fp-scroll th.h1{height:var(--hd1h);}
.fp-left td,.fp-right td,.fp-scroll td{height:var(--rh);}
/* 공통 td */
.fp-left td,.fp-right td,.fp-scroll td{
  font-size:11px;text-align:right;padding:0 5px;
  border-bottom:0.5px solid #e0e4ea;border-right:0.5px solid #e8ecf0;
  font-family:'JetBrains Mono',monospace;
}
.fp-right td{background:var(--yr-bg);}
/* 좌측 라벨 */
.fp-left td.c-lbl{text-align:left;font-family:'Noto Sans KR',sans-serif;font-weight:500;padding-left:7px;}
.fp-left td.c-sub{text-align:left;font-family:'Noto Sans KR',sans-serif;padding-left:11px;color:var(--muted);}
.fp-left td.c-tsub{text-align:left;font-family:'Noto Sans KR',sans-serif;padding-left:11px;color:var(--part-text);font-weight:600;}
.fp-left td.c-tpsn{text-align:left;font-family:'Noto Sans KR',sans-serif;padding-left:17px;color:var(--muted);}
/* 행 배경 — 모든 패널에 class로 동일 적용 */
tr.R-sum td{background:var(--sum-bg)!important;font-weight:600;}
tr.R-tot td{background:var(--tot-bg)!important;font-weight:700;}
tr.R-team td{background:var(--team-bg)!important;font-weight:700;}
tr.R-part td{background:var(--part-bg)!important;}
tr.R-pers td{background:#fff!important;}
/* 우측 패널 행배경 보정 */
.fp-right tr.R-team td{background:#dce0f0!important;}
.fp-right tr.R-part td{background:#e8e8f5!important;}
.fp-right tr.R-pers td{background:var(--yr-bg)!important;}
.fp-right tr.R-sum  td{background:#f0e6ec!important;}
.fp-right tr.R-tot  td{background:#f5e8c8!important;}

/* 달성률 */
.g{color:var(--green)!important;font-weight:600;}.y{color:var(--amber)!important;font-weight:600;}
.o{color:var(--oran)!important;font-weight:600;}.r{color:var(--red)!important;font-weight:600;}
.dash{color:#bbb;font-size:10px;}

/* 아코디언 */
.detail-hd{background:var(--dark2);color:#fff;padding:8px 12px;display:flex;align-items:center;gap:8px;flex-wrap:wrap;}
.detail-hd .title{font-size:13px;font-weight:500;}
.team-filter{font-size:11px;font-weight:500;padding:3px 24px 3px 8px;border:1px solid rgba(255,255,255,.4);border-radius:5px;
  background:rgba(255,255,255,.12) url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='8' height='5'%3E%3Cpath d='M0 0l4 5 4-5z' fill='rgba(255,255,255,.8)'/%3E%3C/svg%3E") no-repeat right 7px center;
  color:#fff;appearance:none;cursor:pointer;font-family:'Noto Sans KR',sans-serif;}
.team-filter option{color:#222;background:#fff;}
.hd-spacer{flex:1;}.hd-btns{display:flex;gap:5px;align-items:center;}
.team-row{display:flex;align-items:center;gap:8px;padding:7px 12px;background:var(--team-bg);cursor:pointer;border-bottom:1px solid #c5c9e8;user-select:none;transition:background .12s;}
.team-row:hover{background:#dfe2f5;}
.team-nm{font-size:12px;font-weight:700;color:var(--team-text);flex:1;}
.team-badge{font-size:10px;background:#7f77dd;color:#fff;padding:1px 7px;border-radius:10px;font-weight:500;}
.team-content{display:none;}.team-content.open{display:block;}

.legend{padding:8px 14px;background:#fafbfc;border-top:1px solid var(--border);display:flex;gap:14px;flex-wrap:wrap;align-items:center;font-size:11px;color:var(--muted);}
.lg-item{display:flex;align-items:center;gap:4px;}
.lg-dot{width:9px;height:9px;border-radius:2px;}
.role-badge{display:inline-block;font-size:9px;font-weight:700;padding:0 5px;border-radius:3px;margin-left:4px;vertical-align:middle;line-height:14px;}
.role-tl{background:#2c3e50;color:#fff;}.role-pl{background:#3c3489;color:#fff;}
*{scrollbar-width:thin;scrollbar-color:#bbb #eee;}
</style>
</head>
<body>
<div class="page-header">
  <div class="logo"><span>팀/파트</span> 매출 통계</div>
  <div class="header-spacer"></div>
  <div class="header-date" id="hdrDate"></div>
</div>
<div class="unit-bar">
  <div class="year-select-wrap">
    <label for="yearSel">기준연도</label>
    <select id="yearSel" class="year-select" onchange="onYearChange(this.value)">
      <option value="2024">2024년</option>
      <option value="2025">2025년</option>
      <option value="2026" selected>2026년</option>
    </select>
  </div>
  <div class="unit-notice">※ 금액 단위: <strong>천원</strong></div>
  <div class="spacer"></div>
  <div class="period-badge" id="periodBadge">금주: 2026-04-06 ~ 2026-04-12</div>
</div>
<div class="container">

<div class="section-card">
  <div class="section-hd">
    <div class="title">□ 실적 현황 (<span id="yr1">2026</span>년) — 1월~12월 월별</div>
    <button class="btn btn-green btn-sm" onclick="downloadExcel('summary')">↓ 엑셀 다운로드 (실적현황 전체)</button>
  </div>
  <div class="freeze-outer" id="sumOuter">
    <div class="fp-left"  id="sumL"></div>
    <div class="fp-right" id="sumR"></div>
    <div class="fp-scroll" id="sumS"></div>
  </div>
  <div class="legend">
    <span style="font-weight:500;color:#555;">달성률:</span>
    <span class="lg-item"><span class="lg-dot" style="background:var(--green)"></span>100% 이상</span>
    <span class="lg-item"><span class="lg-dot" style="background:var(--amber)"></span>70~99%</span>
    <span class="lg-item"><span class="lg-dot" style="background:var(--oran)"></span>50~69%</span>
    <span class="lg-item"><span class="lg-dot" style="background:var(--red)"></span>50% 미만</span>
    <span style="margin-left:auto;font-size:10px;color:var(--hint);">— = 미래 월</span>
  </div>
</div>

<div class="section-card">
  <div class="detail-hd">
    <div class="title">□ 팀별 상세 (펼침/접힘)</div>
    <select class="team-filter" id="teamFilter" onchange="filterTeam(this.value)">
      <option value="all">전체 팀</option>
      <option value="TS1">TS1팀</option><option value="TS2">TS2팀</option>
      <option value="TS3">TS3팀</option><option value="TD">TD팀</option>
    </select>
    <div class="hd-spacer"></div>
    <div class="hd-btns">
      <button class="btn btn-gray btn-sm" onclick="toggleAll(true)">전체 펼치기</button>
      <button class="btn btn-gray btn-sm" onclick="toggleAll(false)">전체 접기</button>
      <button class="btn btn-green btn-sm" onclick="downloadExcel('all')">↓ 전체 엑셀</button>
    </div>
  </div>
  <div class="accordion" id="accordion"></div>
  <div class="legend">
    <span class="lg-item"><span class="lg-dot" style="background:var(--team-bg);border:1px solid #aab"></span>팀 합계행</span>
    <span class="lg-item"><span class="lg-dot" style="background:var(--part-bg);border:1px solid #ccd"></span>파트 소계행</span>
    <span class="lg-item"><span class="lg-dot" style="background:#fff;border:1px solid #dde"></span>개인행</span>
    <span class="lg-item"><span class="role-badge role-tl" style="font-size:9px">팀장</span>팀장</span>
    <span class="lg-item"><span class="role-badge role-pl" style="font-size:9px">파트장</span>파트장</span>
    <span style="margin-left:auto;font-size:10px;color:var(--hint);">팀 엑셀 버튼: 해당 팀 팀·파트·개인 × 1~12월 다운로드</span>
  </div>
</div>
</div>

<script>
const MO=['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'];
const RH=24,HD1=26,HD2=22,HDH=HD1+HD2;

function genData(y){
  const D={
    2026:[
      {team:'TS1',part:'IT/Tech',   mo:[{g:50750,a:56417},{g:50750,a:41083},{g:50750,a:28466},{g:175000,a:0},0,0,0,0,0,0,0,0],yg:700000,ya:125966},
      {team:'TS1',part:'AI/반도체', mo:[{g:30450,a:18290},{g:30450,a:8240},{g:30450,a:6110},{g:105000,a:0},0,0,0,0,0,0,0,0],yg:420000,ya:32640},
      {team:'TS2',part:'전기/전자', mo:[{g:70325,a:541867},{g:70325,a:396810},{g:70325,a:116653},{g:242500,a:60147},0,0,0,0,0,0,0,0],yg:970000,ya:1115478},
      {team:'TS2',part:'메카닉스',  mo:[{g:55825,a:63417},{g:55825,a:31240},{g:55825,a:14397},{g:192500,a:0},0,0,0,0,0,0,0,0],yg:770000,ya:109054},
      {team:'TS3',part:'소비재/헬스',mo:[{g:52200,a:86215},{g:52200,a:56408},{g:52200,a:23166},{g:180000,a:27475},0,0,0,0,0,0,0,0],yg:720000,ya:193265},
      {team:'TS3',part:'BizService',mo:[{g:35525,a:38680},{g:35525,a:26450},{g:35525,a:8500},{g:122500,a:0},0,0,0,0,0,0,0,0],yg:490000,ya:73630},
      {team:'TD', part:'리크루팅',  mo:[{g:35625,a:47523},{g:35625,a:32875},{g:35625,a:13240},{g:160850,a:0},0,0,0,0,0,0,0,0],yg:624000,ya:93638},
      {team:'TD', part:'TD1',       mo:[{g:5333,a:0},{g:5333,a:0},{g:5333,a:0},{g:34000,a:0},0,0,0,0,0,0,0,0],yg:170000,ya:0},
      {team:'TD', part:'TD2',       mo:[{g:29400,a:51230},{g:29400,a:26880},{g:29400,a:11534},{g:96000,a:0},0,0,0,0,0,0,0,0],yg:336000,ya:89644},
    ],
    2025:[
      {team:'TS1',part:'IT/Tech',   mo:[{g:48000,a:52100},{g:48000,a:39000},{g:48000,a:45200},{g:48000,a:61000},{g:48000,a:38000},{g:48000,a:55000},{g:48000,a:49000},{g:48000,a:42000},{g:48000,a:58000},{g:48000,a:61000},{g:48000,a:55000},{g:48000,a:62000}],yg:576000,ya:617300},
      {team:'TS1',part:'AI/반도체', mo:[{g:28000,a:15000},{g:28000,a:22000},{g:28000,a:19000},{g:28000,a:31000},{g:28000,a:25000},{g:28000,a:18000},{g:28000,a:27000},{g:28000,a:22000},{g:28000,a:30000},{g:28000,a:28000},{g:28000,a:24000},{g:28000,a:35000}],yg:336000,ya:296000},
      {team:'TS2',part:'전기/전자', mo:[{g:65000,a:480000},{g:65000,a:350000},{g:65000,a:100000},{g:65000,a:55000},{g:65000,a:70000},{g:65000,a:90000},{g:65000,a:45000},{g:65000,a:60000},{g:65000,a:80000},{g:65000,a:75000},{g:65000,a:65000},{g:65000,a:90000}],yg:780000,ya:1510000},
      {team:'TS2',part:'메카닉스',  mo:[{g:52000,a:58000},{g:52000,a:28000},{g:52000,a:12000},{g:52000,a:0},{g:52000,a:45000},{g:52000,a:60000},{g:52000,a:38000},{g:52000,a:42000},{g:52000,a:55000},{g:52000,a:48000},{g:52000,a:40000},{g:52000,a:65000}],yg:624000,ya:491000},
      {team:'TS3',part:'소비재/헬스',mo:[{g:50000,a:79000},{g:50000,a:51000},{g:50000,a:21000},{g:50000,a:25000},{g:50000,a:62000},{g:50000,a:58000},{g:50000,a:44000},{g:50000,a:51000},{g:50000,a:67000},{g:50000,a:55000},{g:50000,a:48000},{g:50000,a:72000}],yg:600000,ya:633000},
      {team:'TS3',part:'BizService',mo:[{g:33000,a:35000},{g:33000,a:24000},{g:33000,a:8000},{g:33000,a:0},{g:33000,a:30000},{g:33000,a:38000},{g:33000,a:25000},{g:33000,a:28000},{g:33000,a:40000},{g:33000,a:35000},{g:33000,a:29000},{g:33000,a:45000}],yg:396000,ya:337000},
      {team:'TD', part:'리크루팅',  mo:[{g:32000,a:42000},{g:32000,a:29000},{g:32000,a:12000},{g:32000,a:0},{g:32000,a:35000},{g:32000,a:45000},{g:32000,a:28000},{g:32000,a:35000},{g:32000,a:48000},{g:32000,a:40000},{g:32000,a:32000},{g:32000,a:52000}],yg:384000,ya:398000},
      {team:'TD', part:'TD1',       mo:[{g:5000,a:0},{g:5000,a:0},{g:5000,a:0},{g:5000,a:0},{g:5000,a:8000},{g:5000,a:5000},{g:5000,a:3000},{g:5000,a:0},{g:5000,a:7000},{g:5000,a:9000},{g:5000,a:4000},{g:5000,a:11000}],yg:60000,ya:47000},
      {team:'TD', part:'TD2',       mo:[{g:27000,a:46000},{g:27000,a:24000},{g:27000,a:10000},{g:27000,a:0},{g:27000,a:30000},{g:27000,a:40000},{g:27000,a:25000},{g:27000,a:30000},{g:27000,a:42000},{g:27000,a:35000},{g:27000,a:28000},{g:27000,a:48000}],yg:324000,ya:358000},
    ],
    2024:[
      {team:'TS1',part:'IT/Tech',   mo:[{g:44000,a:48000},{g:44000,a:36000},{g:44000,a:42000},{g:44000,a:56000},{g:44000,a:35000},{g:44000,a:50000},{g:44000,a:45000},{g:44000,a:39000},{g:44000,a:53000},{g:44000,a:56000},{g:44000,a:50000},{g:44000,a:57000}],yg:528000,ya:567000},
      {team:'TS1',part:'AI/반도체', mo:[{g:25000,a:12000},{g:25000,a:19000},{g:25000,a:16000},{g:25000,a:27000},{g:25000,a:22000},{g:25000,a:15000},{g:25000,a:24000},{g:25000,a:19000},{g:25000,a:27000},{g:25000,a:25000},{g:25000,a:21000},{g:25000,a:31000}],yg:300000,ya:258000},
      {team:'TS2',part:'전기/전자', mo:[{g:60000,a:430000},{g:60000,a:310000},{g:60000,a:90000},{g:60000,a:50000},{g:60000,a:64000},{g:60000,a:82000},{g:60000,a:40000},{g:60000,a:55000},{g:60000,a:73000},{g:60000,a:68000},{g:60000,a:60000},{g:60000,a:84000}],yg:720000,ya:1406000},
      {team:'TS2',part:'메카닉스',  mo:[{g:48000,a:53000},{g:48000,a:25000},{g:48000,a:11000},{g:48000,a:0},{g:48000,a:41000},{g:48000,a:55000},{g:48000,a:35000},{g:48000,a:38000},{g:48000,a:50000},{g:48000,a:44000},{g:48000,a:37000},{g:48000,a:60000}],yg:576000,ya:449000},
      {team:'TS3',part:'소비재/헬스',mo:[{g:46000,a:72000},{g:46000,a:47000},{g:46000,a:19000},{g:46000,a:23000},{g:46000,a:57000},{g:46000,a:53000},{g:46000,a:40000},{g:46000,a:47000},{g:46000,a:61000},{g:46000,a:50000},{g:46000,a:44000},{g:46000,a:66000}],yg:552000,ya:579000},
      {team:'TS3',part:'BizService',mo:[{g:30000,a:32000},{g:30000,a:21000},{g:30000,a:7000},{g:30000,a:0},{g:30000,a:27000},{g:30000,a:35000},{g:30000,a:22000},{g:30000,a:25000},{g:30000,a:37000},{g:30000,a:32000},{g:30000,a:26000},{g:30000,a:41000}],yg:360000,ya:305000},
      {team:'TD', part:'리크루팅',  mo:[{g:29000,a:38000},{g:29000,a:26000},{g:29000,a:11000},{g:29000,a:0},{g:29000,a:32000},{g:29000,a:41000},{g:29000,a:25000},{g:29000,a:32000},{g:29000,a:44000},{g:29000,a:37000},{g:29000,a:29000},{g:29000,a:48000}],yg:348000,ya:363000},
      {team:'TD', part:'TD1',       mo:[{g:4500,a:0},{g:4500,a:0},{g:4500,a:0},{g:4500,a:0},{g:4500,a:7000},{g:4500,a:4500},{g:4500,a:2500},{g:4500,a:0},{g:4500,a:6500},{g:4500,a:8000},{g:4500,a:3500},{g:4500,a:10000}],yg:54000,ya:42000},
      {team:'TD', part:'TD2',       mo:[{g:24000,a:41000},{g:24000,a:22000},{g:24000,a:9000},{g:24000,a:0},{g:24000,a:27000},{g:24000,a:37000},{g:24000,a:22000},{g:24000,a:27000},{g:24000,a:38000},{g:24000,a:32000},{g:24000,a:25000},{g:24000,a:44000}],yg:288000,ya:324000},
    ],
  };
  return D[y]||D[2026];
}

const PER={
  'TS1_IT/Tech':[{name:'오숙현 (이사)',role:'teamLeader',r:0.123},{name:'김대근 (사원)',role:'member',r:0.257},{name:'음영태 (부장)',role:'member',r:0.254},{name:'최용훈 (대리)',role:'member',r:0.114},{name:'최형빈 (과장)',role:'member',r:0.252}],
  'TS1_AI/반도체':[{name:'홍진철 (부장)',role:'partLeader',r:0.50},{name:'김종윤 (부장)',role:'member',r:0.50}],
  'TS2_전기/전자':[{name:'이형래 (이사)',role:'partLeader',r:0.42},{name:'박승현 (부장)',role:'member',r:0.33},{name:'정민우 (과장)',role:'member',r:0.25}],
  'TS2_메카닉스':[{name:'강태영 (부장)',role:'partLeader',r:0.55},{name:'윤성민 (대리)',role:'member',r:0.45}],
  'TS3_소비재/헬스':[{name:'한지현 (부장)',role:'partLeader',r:0.48},{name:'오세훈 (과장)',role:'member',r:0.32},{name:'임채원 (사원)',role:'member',r:0.20}],
  'TS3_BizService':[{name:'정수진 (부장)',role:'partLeader',r:0.60},{name:'박민지 (대리)',role:'member',r:0.40}],
  'TD_리크루팅':[{name:'김철수 (부장)',role:'partLeader',r:0.45},{name:'이미영 (과장)',role:'member',r:0.35},{name:'박지훈 (사원)',role:'member',r:0.20}],
  'TD_TD1':[{name:'최동훈 (이사)',role:'member',r:1.0}],
  'TD_TD2':[{name:'장혜진 (부장)',role:'partLeader',r:0.55},{name:'류성호 (과장)',role:'member',r:0.45}],
};
const RO={teamLeader:0,partLeader:1,member:2};
const srt=p=>[...p].sort((a,b)=>{const d=RO[a.role]-RO[b.role];return d||a.name.localeCompare(b.name,'ko');});
const rb=r=>r==='teamLeader'?'<span class="role-badge role-tl">팀장</span>':r==='partLeader'?'<span class="role-badge role-pl">파트장</span>':'';
const f=n=>n==null?'<span class="dash">—</span>':Math.round(n).toLocaleString('ko-KR');
function pc(g,a){
  if(!g||g==null)return '<span class="dash">—</span>';
  const v=Math.round(a/g*100);
  return `<span class="${v>=100?'g':v>=70?'y':v>=50?'o':'r'}">${v}%</span>`;
}
// 월 3셀 (미래=0 처리)
function mc(m,rc=''){
  if(!m||m===0)return `<td class="${rc}"><span class="dash">—</span></td><td class="${rc}"><span class="dash">—</span></td><td class="${rc}"><span class="dash">—</span></td>`;
  return `<td class="${rc}">${f(m.g)}</td><td class="${rc}">${f(m.a)}</td><td class="${rc}">${pc(m.g,m.a)}</td>`;
}

// ═══ 핵심: 틀고정 테이블 렌더러 ═══
// 원리: 좌/우 패널은 overflow:hidden + thead sticky
//       translateY(-scrollTop)으로 tbody 행 위치 동기화
//       모든 tr height 완전 동일 → 행이 정확히 맞음
function buildFT(outerEl, LW, RW, H, lHdr, rHdr, mHdr, rows){
  const L=outerEl.querySelector('.fp-left');
  const R=outerEl.querySelector('.fp-right');
  const S=outerEl.querySelector('.fp-scroll');

  outerEl.style.height=H+'px';
  L.style.width=LW+'px';
  R.style.width=RW+'px';
  S.style.left=(LW+2)+'px';
  S.style.right=(RW+2)+'px';

  // tbody HTML
  const lRows=rows.map(r=>`<tr class="${r.c}">${r.l}</tr>`).join('');
  const rRows=rows.map(r=>`<tr class="${r.c}">${r.r}</tr>`).join('');
  const mRows=rows.map(r=>`<tr class="${r.c}">${r.m}</tr>`).join('');

  L.innerHTML=`<table style="width:${LW}px;border-collapse:separate;border-spacing:0;">
    <thead style="position:sticky;top:0;z-index:6;">${lHdr}</thead>
    <tbody id="${L.id}_b">${lRows}</tbody></table>`;
  R.innerHTML=`<table style="width:${RW}px;border-collapse:separate;border-spacing:0;">
    <thead style="position:sticky;top:0;z-index:6;">${rHdr}</thead>
    <tbody id="${R.id}_b">${rRows}</tbody></table>`;
  S.innerHTML=`<table style="border-collapse:separate;border-spacing:0;">
    <thead style="position:sticky;top:0;z-index:6;">${mHdr}</thead>
    <tbody>${mRows}</tbody></table>`;

  // 좌/우 tbody DOM ref
  const Lb=L.querySelector('tbody');
  const Rb=R.querySelector('tbody');

  // 스크롤 동기화
  S.onscroll=()=>{
    const st=S.scrollTop;
    Lb.style.transform=`translateY(-${st}px)`;
    Rb.style.transform=`translateY(-${st}px)`;
  };
}

// 월 헤더 HTML (공통)
function moHdr(){
  let h1='<tr>',h2='<tr>';
  MO.forEach(m=>{
    h1+=`<th class="h1 mo" colspan="3" style="min-width:148px">${m}</th>`;
    h2+=`<th class="mo" style="width:52px">목표</th><th class="mo" style="width:52px">실적</th><th class="mo" style="width:44px">달성</th>`;
  });
  return h1+'</tr>'+h2+'</tr>';
}
function yrHdr(label='연간'){
  return `<tr><th class="h1" colspan="3" style="width:174px;background:var(--yr-bg);color:var(--yr-text)">${label}</th></tr>`+
    `<tr><th style="width:58px;background:var(--yr-bg);color:var(--yr-text)">목표</th>`+
    `<th style="width:58px;background:var(--yr-bg);color:var(--yr-text)">실적</th>`+
    `<th style="width:58px;background:var(--yr-bg);color:var(--yr-text)">달성</th></tr>`;
}

let CY=2026, CD=[];

// ── 실적현황 ──
function buildSum(data){
  const teams=[...new Set(data.map(d=>d.team))];
  const rows=[];
  for(const t of teams){
    const ps=data.filter(d=>d.team===t);
    ps.forEach(p=>{
      rows.push({c:'',
        l:`<td class="c-lbl">${p.team}</td><td class="c-sub">${p.part}</td>`,
        m:p.mo.map(m=>mc(m)).join(''),
        r:`<td>${f(p.yg)}</td><td>${f(p.ya)}</td><td>${pc(p.yg,p.ya)}</td>`
      });
    });
    // 소계
    const s=agg12(ps.map(p=>p.mo));
    const sy=ps.reduce((a,p)=>a+p.yg,0),sa=ps.reduce((a,p)=>a+p.ya,0);
    rows.push({c:'R-sum',
      l:`<td class="c-lbl" colspan="2" style="font-weight:600">${t} 소계</td>`,
      m:s.map(m=>mc(m,'R-sum')).join(''),
      r:`<td>${f(sy)}</td><td>${f(sa)}</td><td>${pc(sy,sa)}</td>`
    });
  }
  // 합계
  const a=agg12(data.map(p=>p.mo));
  const ty=data.reduce((s,p)=>s+p.yg,0),ta=data.reduce((s,p)=>s+p.ya,0);
  rows.push({c:'R-tot',
    l:`<td class="c-lbl" colspan="2" style="font-weight:700">합계</td>`,
    m:a.map(m=>mc(m,'R-tot')).join(''),
    r:`<td>${f(ty)}</td><td>${f(ta)}</td><td>${pc(ty,ta)}</td>`
  });

  const lHdr=`<tr><th class="h1" style="width:44px" rowspan="2">팀</th><th class="h1" style="width:82px" rowspan="2">파트</th></tr><tr></tr>`;
  buildFT(document.getElementById('sumOuter'),126,174,380,lHdr,yrHdr(),moHdr(),rows);
}

function agg12(moArrays){
  return Array.from({length:12},(_,i)=>{
    const arr=moArrays.map(m=>m[i]);
    if(arr.every(v=>!v||v===0))return 0;
    return{g:arr.reduce((s,m)=>s+(m&&m!==0?m.g:0),0),a:arr.reduce((s,m)=>s+(m&&m!==0?m.a:0),0)};
  });
}

// ── 팀별 상세 ──
function buildAcc(data){
  const acc=document.getElementById('accordion');
  acc.innerHTML='';
  const teams=[...new Set(data.map(d=>d.team))];
  for(const t of teams){
    const ps=data.filter(d=>d.team===t);
    const total=Object.entries(PER).filter(([k])=>k.startsWith(t+'_')).reduce((s,[,v])=>s+v.length,0);
    const block=document.createElement('div');
    block.className='team-block';block.dataset.team=t;
    const hd=document.createElement('div');hd.className='team-row';
    hd.innerHTML=`<span class="ti">▼</span><span class="team-nm">${t}팀</span><span class="team-badge">${total}명</span><span style="flex:1"></span><button class="btn btn-blue btn-sm" onclick="event.stopPropagation();downloadExcel('team','${t}')">↓ ${t} 엑셀</button>`;
    hd.addEventListener('click',()=>toggleTeam(t));
    const ct=document.createElement('div');ct.className='team-content open';ct.id='tc_'+t;
    // 행 생성
    const rows=[];
    const ts=agg12(ps.map(p=>p.mo));
    const ty2=ps.reduce((a,p)=>a+p.yg,0),ta2=ps.reduce((a,p)=>a+p.ya,0);
    rows.push({c:'R-team',
      l:`<td class="c-lbl">${t}</td><td class="c-lbl">전체</td><td class="c-lbl">팀 합계</td>`,
      m:ts.map(m=>mc(m)).join(''),
      r:`<td>${f(ty2)}</td><td>${f(ta2)}</td><td>${pc(ty2,ta2)}</td>`
    });
    for(const p of ps){
      const k=`${t}_${p.part}`;const prs=PER[k]||[];
      rows.push({c:'R-part',
        l:`<td class="c-lbl">${t}</td><td class="c-tsub">${p.part}</td><td class="c-tsub">파트 소계</td>`,
        m:p.mo.map(m=>mc(m)).join(''),
        r:`<td style="font-weight:600">${f(p.yg)}</td><td style="font-weight:600">${f(p.ya)}</td><td>${pc(p.yg,p.ya)}</td>`
      });
      for(const pr of srt(prs)){
        const pm=p.mo.map(m=>m&&m!==0?{g:Math.round(m.g*pr.r),a:Math.round(m.a*pr.r)}:0);
        const pyg=Math.round(p.yg*pr.r),pya=Math.round(p.ya*pr.r);
        rows.push({c:'R-pers',
          l:`<td class="c-lbl" style="color:var(--muted)">${t}</td><td class="c-tpsn">${p.part}</td><td class="c-tpsn">${pr.name}${rb(pr.role)}</td>`,
          m:pm.map(m=>mc(m)).join(''),
          r:`<td>${f(pyg)}</td><td>${f(pya)}</td><td>${pc(pyg,pya)}</td>`
        });
      }
    }
    const outer=document.createElement('div');
    outer.className='freeze-outer';
    outer.innerHTML=`<div class="fp-left" id="fl_${t}"></div><div class="fp-right" id="fr_${t}"></div><div class="fp-scroll" id="fs_${t}"></div>`;
    const lHdr=`<tr><th class="h1" style="width:40px" rowspan="2">팀</th><th class="h1" style="width:80px" rowspan="2">파트</th><th class="h1" style="width:100px" rowspan="2">성명</th></tr><tr></tr>`;
    buildFT(outer,220,174,360,lHdr,yrHdr(),moHdr(),rows);
    ct.appendChild(outer);block.appendChild(hd);block.appendChild(ct);acc.appendChild(block);
  }
}

function toggleTeam(t){
  const c=document.getElementById('tc_'+t);
  const i=c.closest('.team-block').querySelector('.ti');
  const o=c.classList.toggle('open');
  i.textContent=o?'▼':'▶';
}
function toggleAll(o){
  document.querySelectorAll('.team-content').forEach(e=>e.classList.toggle('open',o));
  document.querySelectorAll('.ti').forEach(i=>i.textContent=o?'▼':'▶');
}
function filterTeam(v){
  document.querySelectorAll('.team-block').forEach(b=>{b.style.display=(v==='all'||b.dataset.team===v)?'':'none';});
}
function onYearChange(y){
  CY=parseInt(y);CD=genData(CY);
  document.getElementById('yr1').textContent=y;
  document.getElementById('periodBadge').textContent=y==='2026'?'금주: 2026-04-06 ~ 2026-04-12':`기준: ${y}년 연간`;
  buildSum(CD);buildAcc(CD);
}
function downloadExcel(type,team){
  const yr=CY;
  let csv=`팀,파트,성명,역할`;
  MO.forEach(m=>{csv+=`,${m}목표(원),${m}실적(원),${m}달성률`;});
  csv+=`,연간목표(원),연간실적(원),연간달성률\n`;
  const rows=type==='team'?CD.filter(d=>d.team===team):CD;
  const rl={teamLeader:'팀장',partLeader:'파트장',member:''};
  for(const p of rows){
    const prs=PER[`${p.team}_${p.part}`]||[];
    let row=`${p.team},${p.part},파트소계,`;
    p.mo.forEach(m=>{if(!m||m===0)row+=',,,';else row+=`,${m.g*1000},${m.a*1000},${m.g?Math.round(m.a/m.g*100)+'%':''}`;});
    row+=`,${p.yg*1000},${p.ya*1000},${p.yg?Math.round(p.ya/p.yg*100)+'%':''}`;
    csv+=row+'\n';
    for(const pr of srt(prs)){
      const pyg=Math.round(p.yg*pr.r),pya=Math.round(p.ya*pr.r);
      let r2=`${p.team},${p.part},${pr.name},${rl[pr.role]||''}`;
      p.mo.forEach(m=>{if(!m||m===0)r2+=',,,';else{const pg=Math.round(m.g*pr.r),pa=Math.round(m.a*pr.r);r2+=`,${pg*1000},${pa*1000},${pg?Math.round(pa/pg*100)+'%':''}`;}});
      r2+=`,${pyg*1000},${pya*1000},${pyg?Math.round(pya/pyg*100)+'%':''}`;
      csv+=r2+'\n';
    }
  }
  const blob=new Blob(['\uFEFF'+csv],{type:'text/csv;charset=utf-8'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download=type==='summary'?`PS_실적현황_${yr}.csv`:type==='team'?`PS_${team}팀_${yr}.csv`:`PS_전체팀_${yr}.csv`;
  a.click();
}
window.addEventListener('DOMContentLoaded',()=>{
  const n=new Date();
  document.getElementById('hdrDate').textContent=`${n.getFullYear()}-${String(n.getMonth()+1).padStart(2,'0')}-${String(n.getDate()).padStart(2,'0')}`;
  onYearChange(2026);
});
</script>
</body>
</html>
