[team_stats (3).html](https://github.com/user-attachments/files/26528234/team_stats.3.html)
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>팀/파트 매출 통계 | PS사업부</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&family=JetBrains+Mono:wght@400;600&display=swap');

:root {
  --dark: #1e2832;
  --dark2: #2c3e50;
  --dark3: #34495e;
  --white: #ffffff;
  --bg: #f4f5f7;
  --bg2: #ebedf0;
  --card: #ffffff;
  --border: #dde1e7;
  --text: #1a202c;
  --muted: #6b7280;
  --hint: #9ca3af;
  --green: #0f6e56;
  --green-bg: #e6f4f0;
  --blue: #1558a8;
  --blue-bg: #e8f0fc;
  --grayb: #3d4451;
  --amber: #854f0b;
  --oran: #c05010;
  --red: #a32d2d;
  --team-bg: #e8eaf6;
  --team-text: #26215c;
  --part-bg: #f3f4fb;
  --part-text: #3c3489;
  --sum-bg: #f5f5ec;
  --tot-bg: #fff8e1;
  --mo-bg: #fff9f0;
  --mo-text: #7a4500;
  --yr-bg: #fdf0f4;
  --yr-text: #6a0f35;
  --unit-bg: #fffbea;
  --unit-border: #e6a817;
  --unit-text: #7a4500;
  --accent: #2563eb;
  --radius: 8px;
  --shadow: 0 1px 3px rgba(0,0,0,0.08);
}

* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: 'Noto Sans KR', sans-serif;
  background: var(--bg);
  color: var(--text);
  font-size: 13px;
  min-height: 100vh;
}

/* ─── 헤더 ─────────────────────────────── */
.page-header {
  background: var(--dark2);
  color: var(--white);
  padding: 10px 18px;
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
  position: sticky; top: 0; z-index: 100;
}
.page-header .logo {
  font-size: 15px; font-weight: 700; letter-spacing: -0.3px;
  display: flex; align-items: center; gap: 6px;
}
.page-header .logo span { color: #7ec8b0; }
.page-header .sub { font-size: 11px; color: rgba(255,255,255,0.55); margin-top:1px; }
.header-spacer { flex: 1; }
.header-date { font-size: 11px; color: rgba(255,255,255,0.5); font-family: 'JetBrains Mono', monospace; }

/* ─── 단위/연도바 ────────────────────────── */
.unit-bar {
  background: var(--unit-bg);
  border-bottom: 1.5px solid var(--unit-border);
  padding: 7px 18px;
  display: flex;
  align-items: center;
  gap: 14px;
  flex-wrap: wrap;
}
.year-select-wrap {
  display: flex; align-items: center; gap: 7px;
}
.year-select-wrap label { font-size: 12px; font-weight: 500; color: var(--unit-text); }
.year-select {
  font-size: 12px; font-weight: 600;
  padding: 3px 28px 3px 9px;
  border: 1.5px solid var(--unit-border);
  border-radius: 5px;
  background: #fff url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%23e6a817'/%3E%3C/svg%3E") no-repeat right 8px center;
  color: var(--unit-text);
  appearance: none; cursor: pointer;
  font-family: 'JetBrains Mono', monospace;
}
.unit-notice {
  font-size: 12px; font-weight: 500; color: var(--unit-text);
}
.unit-notice strong { font-weight: 700; }
.unit-bar .spacer { flex:1; }
.period-badge {
  font-size: 11px; color: var(--muted);
  background: rgba(0,0,0,0.04); border-radius: 4px; padding: 3px 8px;
  font-family: 'JetBrains Mono', monospace;
}

/* ─── 컨테이너 ─────────────────────────── */
.container { padding: 14px 18px; }

/* ─── 섹션 카드 ────────────────────────── */
.section-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  margin-bottom: 16px;
  overflow: hidden;
}
.section-hd {
  background: var(--dark2);
  color: var(--white);
  padding: 8px 14px;
  display: flex; align-items: center; gap: 10px;
  flex-wrap: wrap;
}
.section-hd .title { font-size: 13px; font-weight: 500; flex:1; min-width: 180px; }

/* ─── 버튼들 ───────────────────────────── */
.btn {
  font-size: 11px; font-weight: 500;
  padding: 4px 12px; border: none; border-radius: 5px;
  cursor: pointer; white-space: nowrap;
  font-family: 'Noto Sans KR', sans-serif;
  transition: filter 0.15s, transform 0.1s;
  display: inline-flex; align-items: center; gap: 4px;
}
.btn:hover { filter: brightness(1.12); }
.btn:active { transform: scale(0.97); }
.btn-green { background: var(--green); color: #fff; }
.btn-blue  { background: var(--blue);  color: #fff; }
.btn-gray  { background: var(--grayb); color: #fff; }
.btn-sm { font-size: 10px; padding: 3px 9px; }

/* ─── 테이블 래퍼 ─────────────────────── */
.tbl-wrap {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}
.tbl-wrap::-webkit-scrollbar { height: 6px; }
.tbl-wrap::-webkit-scrollbar-track { background: #f0f0f0; }
.tbl-wrap::-webkit-scrollbar-thumb { background: #c0c0c0; border-radius: 3px; }

table {
  border-collapse: collapse;
  white-space: nowrap;
  font-size: 11px;
}
th, td {
  border: 0.5px solid #dde1e7;
  padding: 0 5px;
  vertical-align: middle;
}
th {
  background: #eceff1; color: #37474f;
  font-weight: 500; text-align: center;
  height: 26px; font-size: 10px;
}
th.mo  { background: var(--mo-bg);  color: var(--mo-text); }
th.yr  { background: var(--yr-bg);  color: var(--yr-text); }
th.fix { background: #e3e6ea; color: #263238; text-align: left; padding-left: 8px; }
td {
  height: 24px; text-align: right; color: var(--text);
  font-size: 11px; font-family: 'JetBrains Mono', monospace;
}
td.lbl { text-align: left; font-family: 'Noto Sans KR', sans-serif; font-weight: 500; padding-left: 8px; color: var(--text); }
td.sub { text-align: left; font-family: 'Noto Sans KR', sans-serif; padding-left: 12px; color: var(--muted); }
td.sum { background: var(--sum-bg); font-weight: 600; }
td.tot { background: var(--tot-bg); font-weight: 700; }
td.sum.lbl, td.tot.lbl { text-align: left; }

/* 달성률 색상 */
.g { color: var(--green) !important; font-weight: 600; }
.y { color: var(--amber) !important; font-weight: 600; }
.o { color: var(--oran)  !important; font-weight: 600; }
.r { color: var(--red)   !important; font-weight: 600; }
.dash { color: #bbb; font-size: 10px; }

/* ─── 팀별 상세 헤더 ─────────────────── */
.detail-hd {
  background: var(--dark2);
  color: var(--white);
  padding: 8px 12px;
  display: flex; align-items: center; gap: 8px; flex-wrap: wrap;
}
.detail-hd .title { font-size: 13px; font-weight: 500; }
.team-filter {
  font-size: 11px; font-weight: 500;
  padding: 3px 24px 3px 8px;
  border: 1px solid rgba(255,255,255,0.4);
  border-radius: 5px;
  background: rgba(255,255,255,0.12) url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='8' height='5'%3E%3Cpath d='M0 0l4 5 4-5z' fill='rgba(255,255,255,0.8)'/%3E%3C/svg%3E") no-repeat right 7px center;
  color: #fff;
  appearance: none; cursor: pointer;
  font-family: 'Noto Sans KR', sans-serif;
}
.team-filter option { color: #222; background: #fff; }
.hd-spacer { flex: 1; }
.hd-btns { display: flex; gap: 5px; align-items: center; }

/* ─── 아코디언 ─────────────────────────── */
.accordion { border-top: none; }
.team-row {
  display: flex; align-items: center; gap: 8px;
  padding: 7px 12px;
  background: var(--team-bg);
  cursor: pointer;
  border-bottom: 1px solid #c5c9e8;
  user-select: none;
  transition: background 0.12s;
}
.team-row:hover { background: #dfe2f5; }
.team-ico { font-size: 10px; color: var(--team-text); min-width: 12px; transition: transform 0.2s; }
.team-ico.open { transform: rotate(0deg); }
.team-ico.closed { transform: rotate(-90deg); }
.team-nm { font-size: 12px; font-weight: 700; color: var(--team-text); flex:1; }
.team-badge {
  font-size: 10px; background: #7f77dd; color: #fff;
  padding: 1px 7px; border-radius: 10px; font-weight: 500;
}
.team-content { display: none; border-bottom: 1px solid #dde1e7; }
.team-content.open { display: block; }

/* ─── 팀별상세 테이블 ─────────────────── */
.dtbl th { font-size: 9px; height: 22px; }
.dtbl td { height: 22px; font-size: 10px; }
.dtbl td.tlbl { text-align:left; font-family:'Noto Sans KR',sans-serif; padding-left:6px; font-weight:500; }
.dtbl td.tsub { text-align:left; font-family:'Noto Sans KR',sans-serif; padding-left:10px; color:var(--part-text); font-weight:600; }
.dtbl td.tpsn { text-align:left; font-family:'Noto Sans KR',sans-serif; padding-left:16px; color:var(--muted); }
.dtbl tr.t-team td { background: var(--team-bg) !important; font-weight:700; }
.dtbl tr.t-team td.tlbl { color: var(--team-text); }
.dtbl tr.t-part td { background: var(--part-bg) !important; }
.dtbl tr.t-pers td { background: #fff; }
.dtbl tr:hover td { filter: brightness(0.97); }

/* ─── 범례 ─────────────────────────────── */
.legend {
  padding: 8px 14px;
  background: #fafbfc;
  border-top: 1px solid var(--border);
  display: flex; gap: 14px; flex-wrap: wrap; align-items: center;
  font-size: 11px; color: var(--muted);
}
.lg-item { display: flex; align-items: center; gap: 4px; }
.lg-dot { width: 9px; height: 9px; border-radius: 2px; }

/* ─── 역할 뱃지 ────────────────────────── */
.role-badge {
  display: inline-block;
  font-size: 9px; font-weight: 700;
  padding: 0px 5px; border-radius: 3px;
  margin-left: 4px; vertical-align: middle;
  line-height: 14px;
}
.role-tl { background: #2c3e50; color: #ffffff; }
.role-pl { background: #3c3489; color: #ffffff; }

/* 스크롤바 thin */
* { scrollbar-width: thin; scrollbar-color: #c0c0c0 #f0f0f0; }
</style>
</head>
<body>

<!-- 상단 헤더 -->
<div class="page-header">
  <div>
    <div class="logo"><span>팀/파트</span> 매출 통계</div>
  </div>
  <div class="header-spacer"></div>
  <div class="header-date" id="hdrDate"></div>
</div>

<!-- 단위 + 연도 선택 바 -->
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

<!-- ① 실적 현황 -->
<div class="section-card">
  <div class="section-hd">
    <div class="title">□ 실적 현황 (<span id="yr1">2026</span>년) — 1월~12월 월별</div>
    <button class="btn btn-green btn-sm" onclick="downloadExcel('summary')">↓ 엑셀 다운로드 (실적현황 전체)</button>
  </div>
  <div class="tbl-wrap">
    <table id="summaryTable" style="min-width:2200px;">
      <thead>
        <tr>
          <th class="fix" rowspan="2" style="width:44px;min-width:44px;">팀</th>
          <th class="fix" rowspan="2" style="width:82px;min-width:82px;">파트</th>
          <th class="mo" colspan="3">1월</th><th class="mo" colspan="3">2월</th><th class="mo" colspan="3">3월</th>
          <th class="mo" colspan="3">4월</th><th class="mo" colspan="3">5월</th><th class="mo" colspan="3">6월</th>
          <th class="mo" colspan="3">7월</th><th class="mo" colspan="3">8월</th><th class="mo" colspan="3">9월</th>
          <th class="mo" colspan="3">10월</th><th class="mo" colspan="3">11월</th><th class="mo" colspan="3">12월</th>
          <th class="yr" colspan="3">연간</th>
        </tr>
        <tr id="moHdRow"></tr>
      </thead>
      <tbody id="summaryBody"></tbody>
    </table>
  </div>
  <div class="legend">
    <span style="font-weight:500;color:#555;">달성률:</span>
    <span class="lg-item"><span class="lg-dot" style="background:var(--green)"></span>100% 이상</span>
    <span class="lg-item"><span class="lg-dot" style="background:var(--amber)"></span>70~99%</span>
    <span class="lg-item"><span class="lg-dot" style="background:var(--oran)"></span>50~69%</span>
    <span class="lg-item"><span class="lg-dot" style="background:var(--red)"></span>50% 미만</span>
    <span style="margin-left:auto;font-size:10px;color:var(--hint);">— = 미래 월 (데이터 없음)</span>
  </div>
</div>

<!-- ② 팀별 상세 -->
<div class="section-card">
  <div class="detail-hd">
    <div class="title">□ 팀별 상세 (펼침/접힘)</div>
    <select class="team-filter" id="teamFilter" onchange="filterTeam(this.value)">
      <option value="all">전체 팀</option>
      <option value="TS1">TS1팀</option>
      <option value="TS2">TS2팀</option>
      <option value="TS3">TS3팀</option>
      <option value="TD">TD팀</option>
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

</div><!-- /container -->

<script>
// ─── 데이터 정의 ──────────────────────────────────────────────────
const MONTHS = ['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'];

// 데이터: 연도별로 분리
// 구조: { team, part, name(optional), monthly:[{goal,actual}×12], yearGoal, yearActual }
// monthly의 null = 미래월
function genData(year) {
  const base = {
    2026: [
      {type:'part',team:'TS1',part:'IT/Tech',monthly:[{g:50750,a:56417},{g:50750,a:41083},{g:50750,a:28466},{g:175000,a:0},null,null,null,null,null,null,null,null],yg:700000,ya:125966},
      {type:'part',team:'TS1',part:'AI/반도체',monthly:[{g:30450,a:18290},{g:30450,a:8240},{g:30450,a:6110},{g:105000,a:0},null,null,null,null,null,null,null,null],yg:420000,ya:32640},
      {type:'part',team:'TS2',part:'전기/전자',monthly:[{g:70325,a:541867},{g:70325,a:396810},{g:70325,a:116653},{g:242500,a:60147},null,null,null,null,null,null,null,null],yg:970000,ya:1115478},
      {type:'part',team:'TS2',part:'메카닉스',monthly:[{g:55825,a:63417},{g:55825,a:31240},{g:55825,a:14397},{g:192500,a:0},null,null,null,null,null,null,null,null],yg:770000,ya:109054},
      {type:'part',team:'TS3',part:'소비재/헬스',monthly:[{g:52200,a:86215},{g:52200,a:56408},{g:52200,a:23166},{g:180000,a:27475},null,null,null,null,null,null,null,null],yg:720000,ya:193265},
      {type:'part',team:'TS3',part:'BizService',monthly:[{g:35525,a:38680},{g:35525,a:26450},{g:35525,a:8500},{g:122500,a:0},null,null,null,null,null,null,null,null],yg:490000,ya:73630},
      {type:'part',team:'TD',part:'리크루팅',monthly:[{g:35625,a:47523},{g:35625,a:32875},{g:35625,a:13240},{g:160850,a:0},null,null,null,null,null,null,null,null],yg:624000,ya:93638},
      {type:'part',team:'TD',part:'TD1',monthly:[{g:5333,a:0},{g:5333,a:0},{g:5333,a:0},{g:34000,a:0},null,null,null,null,null,null,null,null],yg:170000,ya:0},
      {type:'part',team:'TD',part:'TD2',monthly:[{g:29400,a:51230},{g:29400,a:26880},{g:29400,a:11534},{g:96000,a:0},null,null,null,null,null,null,null,null],yg:336000,ya:89644},
    ],
    2025: [
      {type:'part',team:'TS1',part:'IT/Tech',monthly:[{g:48000,a:52100},{g:48000,a:39000},{g:48000,a:45200},{g:48000,a:61000},{g:48000,a:38000},{g:48000,a:55000},{g:48000,a:49000},{g:48000,a:42000},{g:48000,a:58000},{g:48000,a:61000},{g:48000,a:55000},{g:48000,a:62000}],yg:576000,ya:617300},
      {type:'part',team:'TS1',part:'AI/반도체',monthly:[{g:28000,a:15000},{g:28000,a:22000},{g:28000,a:19000},{g:28000,a:31000},{g:28000,a:25000},{g:28000,a:18000},{g:28000,a:27000},{g:28000,a:22000},{g:28000,a:30000},{g:28000,a:28000},{g:28000,a:24000},{g:28000,a:35000}],yg:336000,ya:296000},
      {type:'part',team:'TS2',part:'전기/전자',monthly:[{g:65000,a:480000},{g:65000,a:350000},{g:65000,a:100000},{g:65000,a:55000},{g:65000,a:70000},{g:65000,a:90000},{g:65000,a:45000},{g:65000,a:60000},{g:65000,a:80000},{g:65000,a:75000},{g:65000,a:65000},{g:65000,a:90000}],yg:780000,ya:1510000},
      {type:'part',team:'TS2',part:'메카닉스',monthly:[{g:52000,a:58000},{g:52000,a:28000},{g:52000,a:12000},{g:52000,a:0},{g:52000,a:45000},{g:52000,a:60000},{g:52000,a:38000},{g:52000,a:42000},{g:52000,a:55000},{g:52000,a:48000},{g:52000,a:40000},{g:52000,a:65000}],yg:624000,ya:491000},
      {type:'part',team:'TS3',part:'소비재/헬스',monthly:[{g:50000,a:79000},{g:50000,a:51000},{g:50000,a:21000},{g:50000,a:25000},{g:50000,a:62000},{g:50000,a:58000},{g:50000,a:44000},{g:50000,a:51000},{g:50000,a:67000},{g:50000,a:55000},{g:50000,a:48000},{g:50000,a:72000}],yg:600000,ya:633000},
      {type:'part',team:'TS3',part:'BizService',monthly:[{g:33000,a:35000},{g:33000,a:24000},{g:33000,a:8000},{g:33000,a:0},{g:33000,a:30000},{g:33000,a:38000},{g:33000,a:25000},{g:33000,a:28000},{g:33000,a:40000},{g:33000,a:35000},{g:33000,a:29000},{g:33000,a:45000}],yg:396000,ya:337000},
      {type:'part',team:'TD',part:'리크루팅',monthly:[{g:32000,a:42000},{g:32000,a:29000},{g:32000,a:12000},{g:32000,a:0},{g:32000,a:35000},{g:32000,a:45000},{g:32000,a:28000},{g:32000,a:35000},{g:32000,a:48000},{g:32000,a:40000},{g:32000,a:32000},{g:32000,a:52000}],yg:384000,ya:398000},
      {type:'part',team:'TD',part:'TD1',monthly:[{g:5000,a:0},{g:5000,a:0},{g:5000,a:0},{g:5000,a:0},{g:5000,a:8000},{g:5000,a:5000},{g:5000,a:3000},{g:5000,a:0},{g:5000,a:7000},{g:5000,a:9000},{g:5000,a:4000},{g:5000,a:11000}],yg:60000,ya:47000},
      {type:'part',team:'TD',part:'TD2',monthly:[{g:27000,a:46000},{g:27000,a:24000},{g:27000,a:10000},{g:27000,a:0},{g:27000,a:30000},{g:27000,a:40000},{g:27000,a:25000},{g:27000,a:30000},{g:27000,a:42000},{g:27000,a:35000},{g:27000,a:28000},{g:27000,a:48000}],yg:324000,ya:358000},
    ],
    2024: [
      {type:'part',team:'TS1',part:'IT/Tech',monthly:[{g:44000,a:48000},{g:44000,a:36000},{g:44000,a:42000},{g:44000,a:56000},{g:44000,a:35000},{g:44000,a:50000},{g:44000,a:45000},{g:44000,a:39000},{g:44000,a:53000},{g:44000,a:56000},{g:44000,a:50000},{g:44000,a:57000}],yg:528000,ya:567000},
      {type:'part',team:'TS1',part:'AI/반도체',monthly:[{g:25000,a:12000},{g:25000,a:19000},{g:25000,a:16000},{g:25000,a:27000},{g:25000,a:22000},{g:25000,a:15000},{g:25000,a:24000},{g:25000,a:19000},{g:25000,a:27000},{g:25000,a:25000},{g:25000,a:21000},{g:25000,a:31000}],yg:300000,ya:258000},
      {type:'part',team:'TS2',part:'전기/전자',monthly:[{g:60000,a:430000},{g:60000,a:310000},{g:60000,a:90000},{g:60000,a:50000},{g:60000,a:64000},{g:60000,a:82000},{g:60000,a:40000},{g:60000,a:55000},{g:60000,a:73000},{g:60000,a:68000},{g:60000,a:60000},{g:60000,a:84000}],yg:720000,ya:1406000},
      {type:'part',team:'TS2',part:'메카닉스',monthly:[{g:48000,a:53000},{g:48000,a:25000},{g:48000,a:11000},{g:48000,a:0},{g:48000,a:41000},{g:48000,a:55000},{g:48000,a:35000},{g:48000,a:38000},{g:48000,a:50000},{g:48000,a:44000},{g:48000,a:37000},{g:48000,a:60000}],yg:576000,ya:449000},
      {type:'part',team:'TS3',part:'소비재/헬스',monthly:[{g:46000,a:72000},{g:46000,a:47000},{g:46000,a:19000},{g:46000,a:23000},{g:46000,a:57000},{g:46000,a:53000},{g:46000,a:40000},{g:46000,a:47000},{g:46000,a:61000},{g:46000,a:50000},{g:46000,a:44000},{g:46000,a:66000}],yg:552000,ya:579000},
      {type:'part',team:'TS3',part:'BizService',monthly:[{g:30000,a:32000},{g:30000,a:21000},{g:30000,a:7000},{g:30000,a:0},{g:30000,a:27000},{g:30000,a:35000},{g:30000,a:22000},{g:30000,a:25000},{g:30000,a:37000},{g:30000,a:32000},{g:30000,a:26000},{g:30000,a:41000}],yg:360000,ya:305000},
      {type:'part',team:'TD',part:'리크루팅',monthly:[{g:29000,a:38000},{g:29000,a:26000},{g:29000,a:11000},{g:29000,a:0},{g:29000,a:32000},{g:29000,a:41000},{g:29000,a:25000},{g:29000,a:32000},{g:29000,a:44000},{g:29000,a:37000},{g:29000,a:29000},{g:29000,a:48000}],yg:348000,ya:363000},
      {type:'part',team:'TD',part:'TD1',monthly:[{g:4500,a:0},{g:4500,a:0},{g:4500,a:0},{g:4500,a:0},{g:4500,a:7000},{g:4500,a:4500},{g:4500,a:2500},{g:4500,a:0},{g:4500,a:6500},{g:4500,a:8000},{g:4500,a:3500},{g:4500,a:10000}],yg:54000,ya:42000},
      {type:'part',team:'TD',part:'TD2',monthly:[{g:24000,a:41000},{g:24000,a:22000},{g:24000,a:9000},{g:24000,a:0},{g:24000,a:27000},{g:24000,a:37000},{g:24000,a:22000},{g:24000,a:27000},{g:24000,a:38000},{g:24000,a:32000},{g:24000,a:25000},{g:24000,a:44000}],yg:288000,ya:324000},
    ],
  };
  return base[year] || base[2026];
}

// 개인 데이터 — role: 'teamLeader' | 'partLeader' | 'member'
// 정렬 기준: teamLeader 최상단 → partLeader → member(성 ㄱㄴㄷ순)
// 전산에서 teamLeader/partLeader는 별도 플래그로 관리
const PERSONS = {
  'TS1_IT/Tech': [
    {name:'오숙현 (이사)',  role:'teamLeader', ratio:0.123},  // TS1 팀장
    {name:'김대근 (사원)',  role:'member',     ratio:0.257},
    {name:'음영태 (부장)',  role:'member',     ratio:0.254},
    {name:'최용훈 (대리)', role:'member',     ratio:0.114},
    {name:'최형빈 (과장)', role:'member',     ratio:0.252},
  ],
  'TS1_AI/반도체': [
    {name:'홍진철 (부장)', role:'partLeader', ratio:0.50},   // AI/반도체 파트장
    {name:'김종윤 (부장)', role:'member',     ratio:0.50},
  ],
  'TS2_전기/전자': [
    {name:'이형래 (이사)', role:'partLeader', ratio:0.42},
    {name:'박승현 (부장)', role:'member',     ratio:0.33},
    {name:'정민우 (과장)', role:'member',     ratio:0.25},
  ],
  'TS2_메카닉스': [
    {name:'강태영 (부장)', role:'partLeader', ratio:0.55},
    {name:'윤성민 (대리)', role:'member',     ratio:0.45},
  ],
  'TS3_소비재/헬스': [
    {name:'한지현 (부장)', role:'partLeader', ratio:0.48},
    {name:'오세훈 (과장)', role:'member',     ratio:0.32},
    {name:'임채원 (사원)', role:'member',     ratio:0.20},
  ],
  'TS3_BizService': [
    {name:'정수진 (부장)', role:'partLeader', ratio:0.60},
    {name:'박민지 (대리)', role:'member',     ratio:0.40},
  ],
  'TD_리크루팅': [
    {name:'김철수 (부장)', role:'partLeader', ratio:0.45},
    {name:'이미영 (과장)', role:'member',     ratio:0.35},
    {name:'박지훈 (사원)', role:'member',     ratio:0.20},
  ],
  'TD_TD1': [
    // 팀장/파트장 없음 — 성 ㄱㄴㄷ순
    {name:'최동훈 (이사)', role:'member', ratio:1.0},
  ],
  'TD_TD2': [
    {name:'장혜진 (부장)', role:'partLeader', ratio:0.55},
    {name:'류성호 (과장)', role:'member',     ratio:0.45},
  ],
};

// 역할 우선순위: teamLeader=0, partLeader=1, member=2
const ROLE_ORDER = {teamLeader:0, partLeader:1, member:2};

// 성 ㄱㄴㄷ 정렬 (성만 추출해 localeCompare)
function sortPersons(persons) {
  return [...persons].sort((a, b) => {
    const ro = ROLE_ORDER[a.role] - ROLE_ORDER[b.role];
    if (ro !== 0) return ro;
    // 같은 역할끼리는 성(첫 글자) ㄱㄴㄷ순
    return a.name.localeCompare(b.name, 'ko');
  });
}

// 역할 뱃지 HTML
function roleBadge(role) {
  if (role === 'teamLeader')  return '<span class="role-badge role-tl">팀장</span>';
  if (role === 'partLeader')  return '<span class="role-badge role-pl">파트장</span>';
  return '';
}

// ─── 유틸 함수 ────────────────────────────────────────────────────
function fmt(n) {
  if (n == null) return '<span class="dash">—</span>';
  return Math.round(n).toLocaleString('ko-KR');
}
function pctClass(v) {
  if (v === null) return 'dash';
  if (v >= 100) return 'g';
  if (v >= 70)  return 'y';
  if (v >= 50)  return 'o';
  return 'r';
}
function pctStr(g, a) {
  if (g == null || a == null) return '<span class="dash">—</span>';
  if (g === 0) return '<span class="dash">—</span>';
  const v = Math.round(a / g * 100);
  const cls = pctClass(v);
  return `<span class="${cls}">${v}%</span>`;
}
function moCell(m, cls='') {
  if (!m) return `<td class="${cls}"><span class="dash">—</span></td><td class="${cls}"><span class="dash">—</span></td><td class="${cls}"><span class="dash">—</span></td>`;
  return `<td class="${cls}" style="text-align:right">${fmt(m.g)}</td><td class="${cls}" style="text-align:right">${fmt(m.a)}</td><td class="${cls}" style="text-align:right">${pctStr(m.g,m.a)}</td>`;
}

let currentYear = 2026;
let currentData = [];

// ─── 실적현황 테이블 빌드 ──────────────────────────────────────────
function buildSummary(data) {
  // 월 헤더 2행
  const hdRow = document.getElementById('moHdRow');
  let hdHTML = '';
  for(let i=0;i<12;i++) hdHTML += `<th class="mo">목표</th><th class="mo">실적</th><th class="mo">달성</th>`;
  hdHTML += `<th class="yr">목표</th><th class="yr">실적</th><th class="yr">달성</th>`;
  hdRow.innerHTML = hdHTML;

  const teams = [...new Set(data.map(d=>d.team))];
  let html = '';

  for (const team of teams) {
    const parts = data.filter(d=>d.team===team);

    // 파트별 행
    for (const p of parts) {
      html += `<tr>`;
      html += `<td class="lbl">${p.team}</td><td class="sub">${p.part}</td>`;
      for (const m of p.monthly) html += moCell(m);
      html += `<td style="text-align:right">${fmt(p.yg)}</td><td style="text-align:right">${fmt(p.ya)}</td><td style="text-align:right">${pctStr(p.yg,p.ya)}</td>`;
      html += `</tr>`;
    }

    // 팀 소계
    const sum12 = Array.from({length:12},(_,i)=>{
      const mArr = parts.map(p=>p.monthly[i]);
      if(mArr.every(m=>m===null)) return null;
      return {g:mArr.reduce((s,m)=>s+(m?m.g:0),0), a:mArr.reduce((s,m)=>s+(m?m.a:0),0)};
    });
    const syg = parts.reduce((s,p)=>s+p.yg,0);
    const sya = parts.reduce((s,p)=>s+p.ya,0);
    html += `<tr>`;
    html += `<td class="lbl sum" colspan="2">${team} 소계</td>`;
    for (const m of sum12) html += moCell(m,'sum');
    html += `<td class="sum" style="text-align:right">${fmt(syg)}</td><td class="sum" style="text-align:right">${fmt(sya)}</td><td class="sum" style="text-align:right">${pctStr(syg,sya)}</td>`;
    html += `</tr>`;
  }

  // 전체 합계
  const all12 = Array.from({length:12},(_,i)=>{
    const mArr = data.map(p=>p.monthly[i]);
    if(mArr.every(m=>m===null)) return null;
    return {g:mArr.reduce((s,m)=>s+(m?m.g:0),0), a:mArr.reduce((s,m)=>s+(m?m.a:0),0)};
  });
  const tyg = data.reduce((s,p)=>s+p.yg,0);
  const tya = data.reduce((s,p)=>s+p.ya,0);
  html += `<tr>`;
  html += `<td class="lbl tot" colspan="2">합계</td>`;
  for (const m of all12) html += moCell(m,'tot');
  html += `<td class="tot" style="text-align:right">${fmt(tyg)}</td><td class="tot" style="text-align:right">${fmt(tya)}</td><td class="tot" style="text-align:right">${pctStr(tyg,tya)}</td>`;
  html += `</tr>`;

  document.getElementById('summaryBody').innerHTML = html;
}

// ─── 팀별 상세 아코디언 빌드 ──────────────────────────────────────
function buildAccordion(data) {
  const teams = [...new Set(data.map(d=>d.team))];
  const acc = document.getElementById('accordion');
  acc.innerHTML = '';

  for (const team of teams) {
    const parts = data.filter(d=>d.team===team);
    const totalPersons = Object.entries(PERSONS)
      .filter(([k])=>k.startsWith(team+'_'))
      .reduce((s,[,v])=>s+v.length,0);

    const teamBlock = document.createElement('div');
    teamBlock.className = 'team-block';
    teamBlock.dataset.team = team;

    // 팀 헤더
    const teamHd = document.createElement('div');
    teamHd.className = 'team-row';
    teamHd.innerHTML = `
      <span class="team-ico open">▼</span>
      <span class="team-nm">${team}팀</span>
      <span class="team-badge">${totalPersons}명</span>
      <span style="flex:1"></span>
      <button class="btn btn-blue btn-sm" onclick="event.stopPropagation();downloadExcel('team','${team}')">↓ ${team} 엑셀</button>
    `;
    teamHd.addEventListener('click', () => toggleTeam(team));

    // 팀 콘텐츠
    const teamContent = document.createElement('div');
    teamContent.className = 'team-content open';
    teamContent.id = 'tc_' + team;

    // 상세 테이블
    const wrap = document.createElement('div');
    wrap.className = 'tbl-wrap';

    let tblHTML = `<table class="dtbl" style="min-width:2200px;"><thead>
      <tr>
        <th style="width:40px">팀</th><th style="width:80px">파트</th><th style="width:100px">성명</th>`;
    for(let i=0;i<12;i++) tblHTML += `<th class="mo" colspan="3">${MONTHS[i]}</th>`;
    tblHTML += `<th class="yr" colspan="3">연간</th></tr><tr><th></th><th></th><th></th>`;
    for(let i=0;i<12;i++) tblHTML += `<th class="mo">목표</th><th class="mo">실적</th><th class="mo">달성</th>`;
    tblHTML += `<th class="yr">목표</th><th class="yr">실적</th><th class="yr">달성</th></tr></thead><tbody>`;

    // 팀 합계행
    const teamSum12 = Array.from({length:12},(_,i)=>{
      const mArr = parts.map(p=>p.monthly[i]);
      if(mArr.every(m=>m===null)) return null;
      return {g:mArr.reduce((s,m)=>s+(m?m.g:0),0), a:mArr.reduce((s,m)=>s+(m?m.a:0),0)};
    });
    const tsyg = parts.reduce((s,p)=>s+p.yg,0);
    const tsya = parts.reduce((s,p)=>s+p.ya,0);
    tblHTML += `<tr class="t-team"><td class="tlbl">${team}</td><td class="tlbl">전체</td><td class="tlbl">팀 합계</td>`;
    for(const m of teamSum12) tblHTML += moCell(m);
    tblHTML += `<td style="text-align:right">${fmt(tsyg)}</td><td style="text-align:right">${fmt(tsya)}</td><td style="text-align:right">${pctStr(tsyg,tsya)}</td></tr>`;

    // 파트별
    for (const part of parts) {
      const key = `${team}_${part.part}`;
      const persons = PERSONS[key] || [];

      // 파트 소계
      tblHTML += `<tr class="t-part"><td class="tlbl">${team}</td><td class="tsub">${part.part}</td><td class="tsub">파트 소계</td>`;
      for(const m of part.monthly) tblHTML += moCell(m);
      tblHTML += `<td style="text-align:right;font-weight:600">${fmt(part.yg)}</td><td style="text-align:right;font-weight:600">${fmt(part.ya)}</td><td style="text-align:right">${pctStr(part.yg,part.ya)}</td></tr>`;

      // 개인별 — sortPersons로 팀장→파트장→성ㄱㄴㄷ 정렬
      for (const person of sortPersons(persons)) {
        const pm12 = part.monthly.map(m => m ? {g:Math.round(m.g*person.ratio), a:Math.round(m.a*person.ratio)} : null);
        const pyg = Math.round(part.yg * person.ratio);
        const pya = Math.round(part.ya * person.ratio);
        tblHTML += `<tr class="t-pers"><td class="tlbl" style="color:var(--muted)">${team}</td><td class="tpsn">${part.part}</td><td class="tpsn">${person.name}${roleBadge(person.role)}</td>`;
        for(const m of pm12) tblHTML += moCell(m);
        tblHTML += `<td style="text-align:right">${fmt(pyg)}</td><td style="text-align:right">${fmt(pya)}</td><td style="text-align:right">${pctStr(pyg,pya)}</td></tr>`;
      }
    }

    tblHTML += `</tbody></table>`;
    wrap.innerHTML = tblHTML;
    teamContent.appendChild(wrap);
    teamBlock.appendChild(teamHd);
    teamBlock.appendChild(teamContent);
    acc.appendChild(teamBlock);
  }
}

// ─── 인터랙션 ──────────────────────────────────────────────────────
function toggleTeam(team) {
  const content = document.getElementById('tc_' + team);
  const block = content.closest('.team-block');
  const ico = block.querySelector('.team-ico');
  const isOpen = content.classList.contains('open');
  content.classList.toggle('open', !isOpen);
  ico.textContent = isOpen ? '▶' : '▼';
}

function toggleAll(open) {
  document.querySelectorAll('.team-content').forEach(el => {
    el.classList.toggle('open', open);
  });
  document.querySelectorAll('.team-ico').forEach(ico => {
    ico.textContent = open ? '▼' : '▶';
  });
}

function filterTeam(val) {
  document.querySelectorAll('.team-block').forEach(block => {
    block.style.display = (val === 'all' || block.dataset.team === val) ? '' : 'none';
  });
}

function onYearChange(year) {
  currentYear = parseInt(year);
  currentData = genData(currentYear);
  document.getElementById('yr1').textContent = year;
  document.getElementById('periodBadge').textContent =
    year === '2026' ? '금주: 2026-04-06 ~ 2026-04-12' : `기준: ${year}년 연간`;
  buildSummary(currentData);
  buildAccordion(currentData);
}

// 엑셀 다운로드 — 화면: 천원 표시 / 다운로드: 원(₩) 원본값 그대로 출력
// 데이터 원본이 이미 원 단위이므로 별도 변환 없이 저장
function downloadExcel(type, team) {
  const yr = currentYear;
  let csv = `팀,파트,성명,역할`;
  for (const m of MONTHS) csv += `,${m}목표(원),${m}실적(원),${m}달성률`;
  csv += `,연간목표(원),연간실적(원),연간달성률\n`;

  const data = currentData;
  const rows = type === 'team'
    ? data.filter(d => d.team === team)
    : data;

  for (const p of rows) {
    const persons = PERSONS[`${p.team}_${p.part}`] || [];

    // 파트 소계 — 원본값(원 단위) 그대로
    let row = `${p.team},${p.part},파트소계,`;
    for (const m of p.monthly) {
      if (!m) row += ',,,';
      else row += `,${m.g * 1000},${m.a * 1000},${m.g ? Math.round(m.a / m.g * 100) + '%' : ''}`;
    }
    row += `,${p.yg * 1000},${p.ya * 1000},${p.yg ? Math.round(p.ya / p.yg * 100) + '%' : ''}`;
    csv += row + '\n';

    // 개인 — 정렬 적용 + 역할 컬럼 포함
    const roleLabel = {teamLeader:'팀장', partLeader:'파트장', member:''};
    for (const person of sortPersons(persons)) {
      const pyg = Math.round(p.yg * person.ratio);
      const pya = Math.round(p.ya * person.ratio);
      let prow = `${p.team},${p.part},${person.name},${roleLabel[person.role]||''}`;
      for (const m of p.monthly) {
        if (!m) prow += ',,,';
        else {
          const pg = Math.round(m.g * person.ratio);
          const pa = Math.round(m.a * person.ratio);
          prow += `,${pg * 1000},${pa * 1000},${pg ? Math.round(pa / pg * 100) + '%' : ''}`;
        }
      }
      prow += `,${pyg * 1000},${pya * 1000},${pyg ? Math.round(pya / pyg * 100) + '%' : ''}`;
      csv += prow + '\n';
    }
  }

  const bom = '\uFEFF';
  const blob = new Blob([bom+csv], {type:'text/csv;charset=utf-8'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  const fname = type==='summary' ? `PS_실적현황_${yr}.csv`
    : type==='team' ? `PS_${team}팀_${yr}.csv`
    : `PS_전체팀_${yr}.csv`;
  a.href = url; a.download = fname; a.click();
  URL.revokeObjectURL(url);
}

// ─── 초기화 ────────────────────────────────────────────────────────
window.addEventListener('DOMContentLoaded', () => {
  const now = new Date();
  document.getElementById('hdrDate').textContent =
    `${now.getFullYear()}-${String(now.getMonth()+1).padStart(2,'0')}-${String(now.getDate()).padStart(2,'0')}  로드`;
  onYearChange(2026);
});
</script>
</body>
</html>
