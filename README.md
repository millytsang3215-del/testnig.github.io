<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="淘汰賽產生器：單敗、雙敗、小組賽四晉二，點選晉級與圖表顯示">
<meta name="theme-color" content="#0f172a">
<title>淘汰賽產生器｜單敗 / 雙敗 / 小組賽四晉二</title>
<style>
:root {
  --bg:#0f172a; --card:#1e293b; --accent:#3b82f6; --accent2:#22c55e; --accent3:#f59e0b;
  --text:#f1f5f9; --muted:#94a3b8; --border:#334155; --win:#166534; --winb:#22c55e;
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:"Segoe UI","Noto Sans TC","Microsoft JhengHei",sans-serif;background:var(--bg);color:var(--text);min-height:100vh;padding:14px}
.container{max-width:1600px;margin:0 auto}
h1{text-align:center;font-size:1.7rem;margin-bottom:4px;background:linear-gradient(90deg,#3b82f6,#22c55e,#f59e0b);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.subtitle{text-align:center;color:var(--muted);margin-bottom:16px;font-size:13px}
.panel{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:14px 16px;margin-bottom:14px}
.panel h2{font-size:1.05rem;margin-bottom:8px;color:#93c5fd}
textarea{width:100%;height:130px;background:#0f172a;border:1px solid var(--border);border-radius:8px;color:var(--text);padding:10px;font-size:13px;resize:vertical;font-family:inherit}
.btn-row{display:flex;flex-wrap:wrap;gap:8px;margin-top:10px}
button{background:var(--accent);color:#fff;border:none;padding:8px 13px;border-radius:8px;cursor:pointer;font-size:13px;font-weight:600}
button:hover{filter:brightness(1.12)}
button.secondary{background:#475569}
button.success{background:var(--accent2)}
button.danger{background:#ef4444}
button.warning{background:var(--accent3);color:#1e293b}
.options{display:flex;flex-wrap:wrap;gap:10px 16px;align-items:center;margin-top:10px;font-size:13px}
.options label{display:flex;align-items:center;gap:5px;cursor:pointer}
select{background:#0f172a;border:1px solid var(--border);color:var(--text);padding:5px 8px;border-radius:6px}
#bracket-area{display:none}
.stats{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:10px;font-size:12px}
.stats span{background:#0f172a;padding:5px 10px;border-radius:6px;color:var(--muted)}
.hint{background:#1e3a5f;border:1px solid #3b82f6;border-radius:8px;padding:9px 12px;font-size:13px;margin-bottom:12px;color:#bfdbfe}
.progress-bar-wrap{background:#0f172a;border-radius:999px;height:8px;margin:8px 0 4px;overflow:hidden}
.progress-bar{height:100%;background:linear-gradient(90deg,#3b82f6,#22c55e);border-radius:999px;transition:width .3s}
.section-div{margin:18px 0 8px;padding:8px 12px;background:#0f172a;border-radius:8px;font-weight:700;font-size:14px}
.section-div.w{border-left:4px solid #22c55e}
.section-div.l{border-left:4px solid #ef4444}
.section-div.g{border-left:4px solid #f59e0b}
.section-div.grp{border-left:4px solid #3b82f6}
.round-title{font-size:13px;color:#93c5fd;margin:14px 0 8px;padding-bottom:4px;border-bottom:1px solid var(--border)}
.round-title.lr{color:#fca5a5}
.round-title.gf{color:#fcd34d}
.matches{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:8px}
.match-card{background:#0f172a;border:1px solid var(--border);border-radius:8px;overflow:hidden}
.match-card.decided{border-color:var(--winb)}
.match-card.bye-match{opacity:.75;border-style:dashed}
.slot-team{padding:8px 10px;font-size:13px;cursor:pointer;display:flex;align-items:center;gap:6px;border-bottom:1px solid var(--border);user-select:none;transition:background .15s}
.slot-team:last-child{border-bottom:none}
.slot-team:hover:not(.disabled):not(.winner){background:#1e3a5f}
.slot-team.winner{background:var(--win);color:#bbf7d0;font-weight:700}
.slot-team.loser{opacity:.4;text-decoration:line-through}
.slot-team.bye-team,.slot-team.tbd{color:var(--muted);cursor:default}
.slot-team.disabled{cursor:default}
.seed-tag{font-size:11px;color:var(--muted);min-width:22px}
.slot-team.winner .seed-tag{color:#86efac}
.win-icon{margin-left:auto;font-size:12px}
.champ-panel{text-align:center;padding:22px;background:linear-gradient(135deg,#1e3a5f,#14532d);border-radius:12px;margin-top:18px;border:2px solid #22c55e}
.champ-panel h3{font-size:1.25rem;margin-bottom:6px}
.champ-name{font-size:1.5rem;font-weight:800;color:#bbf7d0;margin-top:6px}
.note{font-size:12px;color:var(--muted);margin-top:6px;line-height:1.4}
.view-toggle{display:flex;gap:6px;margin-bottom:10px;flex-wrap:wrap}
.view-toggle button.active{outline:2px solid #93c5fd}
.bracket-scroll{overflow-x:auto;padding-bottom:16px;margin-bottom:8px}
.bracket-chart{display:flex;min-width:max-content;align-items:stretch;gap:0;padding:8px 4px}
.round-col{display:flex;flex-direction:column;min-width:200px;padding:0 14px;position:relative;border-right:1px dashed #334155}
.round-col:last-child{border-right:none}
.round-col-title{text-align:center;font-size:12px;font-weight:700;color:#93c5fd;margin-bottom:14px;padding:8px 6px;background:linear-gradient(180deg,#1e293b,#0f172a);border:1px solid #334155;border-radius:8px;white-space:nowrap;letter-spacing:.02em}
.round-col-title.final{color:#fcd34d;border-color:#b45309;background:linear-gradient(180deg,#451a03,#0f172a)}
.round-col-title.lr-title{color:#fca5a5;border-color:#7f1d1d;background:linear-gradient(180deg,#450a0a,#0f172a)}
.match-slot{display:flex;flex-direction:column;justify-content:center;position:relative}
.match-slot::after{content:"";position:absolute;right:-14px;top:50%;width:14px;height:2px;background:#475569}
.round-col:last-child .match-slot::after{display:none}
.match-card{box-shadow:0 1px 0 rgba(0,0,0,.25)}
.match-card .slot-team{min-height:32px}
.bracket-legend{display:flex;flex-wrap:wrap;gap:10px 16px;font-size:12px;color:var(--muted);margin:6px 0 12px;padding:8px 10px;background:#0f172a;border-radius:8px}
.bracket-legend span{display:flex;align-items:center;gap:6px}
.legend-dot{width:10px;height:10px;border-radius:50%;display:inline-block}
.legend-dot.w{background:#22c55e}
.legend-dot.l{background:#ef4444}
.legend-dot.g{background:#f59e0b}
.legend-dot.u{background:#64748b}
.ko-flow{font-size:12px;color:#94a3b8;margin:4px 0 10px;padding-left:4px}
/* 小組 */
.groups-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:14px}
.group-card{background:#0f172a;border:1px solid var(--border);border-radius:10px;padding:12px;overflow:hidden}
.group-card h3{font-size:14px;color:#93c5fd;margin-bottom:8px}
.standings{width:100%;border-collapse:collapse;font-size:12px;margin-bottom:10px}
.standings th,.standings td{padding:5px 6px;text-align:left;border-bottom:1px solid var(--border)}
.standings th{color:var(--muted);font-weight:600}
.standings tr.adv td{color:#86efac;font-weight:700}
.standings tr.out td{opacity:.5}
.group-matches{display:flex;flex-direction:column;gap:6px}
.group-matches .match-card{min-width:0}
.adv-badge{font-size:10px;background:#14532d;color:#86efac;padding:1px 6px;border-radius:4px;margin-left:4px}
@media print{
  body{background:#fff;color:#000}
  .no-print{display:none!important}
  .match-card{border:1px solid #999}
  .slot-team.winner{background:#dcfce7!important;color:#166534!important}
}
</style>
</head>
<body>
<div class="container">
  <h1>🏆 淘汰賽產生器</h1>
  <p class="subtitle">單敗 · 雙敗 · 小組賽四晉二 · 點選晉級 · 圖表顯示</p>

  <div class="panel no-print">
    <h2>1️⃣ 輸入隊伍（每行一隊）</h2>
    <textarea id="teams-input"></textarea>
    <div class="btn-row">
      <button onclick="fillDefault(8)">8 隊</button>
      <button class="secondary" onclick="fillDefault(16)">16 隊</button>
      <button class="secondary" onclick="fillDefault(32)">32 隊</button>
      <button class="secondary" onclick="fillDefault(64)">64 隊</button>
      <button class="secondary" onclick="clearTeams()">清空</button>
      <button class="secondary" onclick="shuffleInput()">打亂</button>
    </div>
    <div class="options">
      <label>賽制：
        <select id="format">
          <option value="single">單敗淘汰</option>
          <option value="double">雙敗淘汰</option>
          <option value="group" selected>小組賽（四晉二）</option>
        </select>
      </label>
      <label><input type="radio" name="mode" value="random" checked> 隨機分組/抽籤</label>
      <label><input type="radio" name="mode" value="seeded"> 種子（依輸入順序）</label>
    </div>
    <p class="note">小組賽：每組 4 隊循環賽，積分前 2 名晉級淘汰賽。隊伍數需為 4 的倍數（如 8、16、32、64）。</p>
    <div class="btn-row" style="margin-top:10px">
      <button class="success" onclick="generate()">🎲 產生對戰表</button>
    </div>
  </div>

  <div id="bracket-area" class="panel">
    <h2>2️⃣ 對戰表 · 點選勝者</h2>
    <div class="stats" id="stats"></div>
    <div class="progress-bar-wrap no-print"><div class="progress-bar" id="progress" style="width:0%"></div></div>
    <div class="hint no-print">👉 點擊隊伍名稱選勝者。小組賽完成後，前兩名會自動進入淘汰賽圖表。</div>
    <div class="btn-row no-print view-toggle">
      <button id="btn-list" class="active" onclick="setView('list')">📋 列表</button>
      <button id="btn-chart" class="secondary" onclick="setView('chart')">📊 圖表</button>
      <button class="secondary" onclick="window.print()">🖨️ 列印</button>
      <button class="secondary" onclick="copyText()">複製</button>
      <button class="danger" onclick="resetResults()">重置賽果</button>
      <button class="warning" onclick="generate()">重新抽籤</button>
    </div>
    <div id="bracket-content"></div>
  </div>
</div>

<script>
function nextPow2(n){let p=1;while(p<n)p<<=1;return p}
function seedOrder(size){
  function s(n){if(n===1)return[1];const h=s(n/2),r=[];for(const x of h){r.push(x);r.push(n+1-x)}return r}
  return s(size).map(x=>x-1);
}
function shuffle(a){const arr=[...a];for(let i=arr.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[arr[i],arr[j]]=[arr[j],arr[i]]}return arr}
function rndName(size){const m={2:'決賽',4:'準決賽',8:'8強',16:'16強',32:'32強',64:'64強',128:'128強'};return m[size]||(size+'強')}
function team(name,seed,extra){return Object.assign({name:name,seed:seed||null,isBye:false,tbd:false},extra||{})}
function tbd(){return{name:'待定',seed:null,isBye:false,tbd:true}}
function bye(){return{name:'輪空',seed:null,isBye:true,tbd:false}}
function esc(s){return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;')}

let STATE=null, currentView='list';

/* ===== 單敗 ===== */
function buildSingle(teams, mode){
  const n=teams.length, bs=nextPow2(n), byes=bs-n;
  const ordered=mode==='seeded'?[...teams]:shuffle(teams);
  const slots=new Array(bs).fill(null);
  const pos=seedOrder(bs);
  for(let i=0;i<n;i++) slots[pos[i]]=team(ordered[i],i+1);
  for(let i=0;i<bs;i++) if(!slots[i]) slots[i]=bye();
  const rounds=[]; let cur=slots.map(t=>({...t}));
  while(cur.length>1){
    const matches=[];
    for(let i=0;i<cur.length;i+=2){
      const t1=cur[i],t2=cur[i+1];
      const isBye=!!(t1.isBye||t2.isBye);
      let winner=null;
      if(t1.isBye&&!t2.isBye) winner=2;
      else if(t2.isBye&&!t1.isBye) winner=1;
      matches.push({t1:{...t1},t2:{...t2},winner,isBye});
    }
    rounds.push(matches);
    cur=matches.map(m=>{
      if(m.winner===1) return {...m.t1,isBye:false,tbd:false};
      if(m.winner===2) return {...m.t2,isBye:false,tbd:false};
      return tbd();
    });
  }
  return{type:'single',teams:n,bracketSize:bs,byes,wb:rounds};
}

/* ===== 雙敗 ===== */
function buildDouble(teams, mode){
  const base=buildSingle(teams,mode);
  const wb=base.wb, wbRounds=wb.length;
  const lb=[];
  let dropFromWb=0;
  let pending=wb[0].length;
  { const mc=Math.floor(pending/2)||(pending>0?1:0);
    const matches=[];
    for(let i=0;i<mc;i++) matches.push({t1:tbd(),t2:tbd(),winner:null,isBye:false});
    lb.push({name:'敗者組 第1輪',type:'drop',matches,fromWb:0});
    dropFromWb=1;
  }
  let lbSize=lb[0].matches.length, roundNum=2;
  while((lbSize>=1||dropFromWb<wbRounds)&&roundNum<=20){
    const isDrop=(dropFromWb<wbRounds)&&(roundNum%2===0||lbSize<=1);
    if(isDrop&&dropFromWb<wbRounds){
      const matchCount=Math.max(1,lbSize);
      const matches=[];
      for(let i=0;i<matchCount;i++) matches.push({t1:tbd(),t2:tbd(),winner:null,isBye:false});
      lb.push({name:'敗者組 第'+roundNum+'輪',type:'drop',matches,fromWb:dropFromWb});
      dropFromWb++; lbSize=matchCount;
    } else {
      if(lbSize<=1&&dropFromWb>=wbRounds) break;
      const matchCount=Math.max(1,Math.floor(lbSize/2)||1);
      const matches=[];
      for(let i=0;i<matchCount;i++) matches.push({t1:tbd(),t2:tbd(),winner:null,isBye:false});
      lb.push({name:'敗者組 第'+roundNum+'輪',type:'consol',matches});
      lbSize=matchCount;
    }
    roundNum++;
  }
  if(!lb.length) lb.push({name:'敗者組決賽',type:'consol',matches:[{t1:tbd(),t2:tbd(),winner:null,isBye:false}]});
  return{type:'double',teams:base.teams,bracketSize:base.bracketSize,byes:base.byes,wb,lb,grand:{t1:tbd(),t2:tbd(),winner:null,isBye:false}};
}

/* ===== 小組賽四晉二 ===== */
function buildGroup(teams, mode){
  let list=mode==='seeded'?[...teams]:shuffle(teams);
  // 補齊到 4 的倍數
  while(list.length%4!==0) list.push('待補 '+ (list.length+1));
  const n=list.length;
  const groupCount=n/4;
  const groups=[];
  for(let g=0;g<groupCount;g++){
    const members=[];
    for(let i=0;i<4;i++){
      const name=list[g*4+i];
      members.push(team(name, g*4+i+1, {pts:0,w:0,l:0,gid:g,idx:i}));
    }
    // 循環賽：6 場
    const pairs=[[0,1],[2,3],[0,2],[1,3],[0,3],[1,2]];
    const matches=pairs.map(([a,b])=>({
      t1:{...members[a]}, t2:{...members[b]},
      winner:null, isBye:false, a, b
    }));
    groups.push({id:g, name:'小組 ' + String.fromCharCode(65+g), members, matches});
  }
  // 淘汰賽 bracket：每組前2 → 共 groupCount*2 隊
  const koSlots=groupCount*2;
  const ko=buildSingle(
    Array.from({length:koSlots},(_,i)=>'待定'),
    'seeded'
  );
  // 清空自動 bye winner（因為全是待定）
  for(const round of ko.wb){
    for(const m of round){ m.winner=null; m.isBye=false; m.t1=tbd(); m.t2=tbd(); }
  }
  return{
    type:'group',
    teams:teams.length,
    bracketSize:ko.bracketSize,
    byes:0,
    groups,
    groupCount,
    wb:ko.wb, // 淘汰賽用 wb 結構
    advanceReady:false
  };
}

/* ===== 積分 / 晉級 ===== */
function calcGroupStandings(g){
  // reset
  g.members.forEach(m=>{m.pts=0;m.w=0;m.l=0});
  for(const m of g.matches){
    if(!m.winner) continue;
    const wIdx=m.winner===1?m.a:m.b;
    const lIdx=m.winner===1?m.b:m.a;
    g.members[wIdx].pts+=3;
    g.members[wIdx].w+=1;
    g.members[lIdx].l+=1;
  }
  // sort: pts desc, then seed asc
  const ranked=[...g.members].sort((a,b)=>{
    if(b.pts!==a.pts) return b.pts-a.pts;
    return (a.seed||99)-(b.seed||99);
  });
  return ranked;
}

function getGroupAdvancers(){
  if(!STATE||STATE.type!=='group') return [];
  const adv=[];
  for(const g of STATE.groups){
    const allDone=g.matches.every(m=>m.winner!==null);
    if(!allDone){ adv.push(null,null); continue; }
    const ranked=calcGroupStandings(g);
    adv.push({...ranked[0],tbd:false,isBye:false}, {...ranked[1],tbd:false,isBye:false});
  }
  return adv;
}

function fillKnockoutFromGroups(){
  if(STATE.type!=='group') return;
  const adv=getGroupAdvancers();
  // 配對：傳統交叉 A1 vs B2, B1 vs A2, C1 vs D2...
  // 簡化：依序放入標準種子位置
  const slots=adv.map(t=>t?{...t,tbd:false,isBye:false}:tbd());
  const n=slots.length;
  const bs=STATE.wb[0].length*2;
  // 放入第一輪
  const pos=seedOrder(bs);
  const first=STATE.wb[0];
  // 重建第一輪對戰：用 slots 填
  const ordered=new Array(bs).fill(null);
  for(let i=0;i<n&&i<bs;i++) ordered[pos[i]]=slots[i];
  for(let i=0;i<bs;i++) if(!ordered[i]) ordered[i]=bye();
  for(let i=0;i<first.length;i++){
    const t1=ordered[i*2], t2=ordered[i*2+1];
    first[i].t1={...t1};
    first[i].t2={...t2};
    first[i].isBye=!!(t1.isBye||t2.isBye);
    // 不自動標勝（小組晉級後由用戶點）
    if(first[i].isBye){
      if(t1.isBye&&!t2.isBye&&!t2.tbd) first[i].winner=2;
      else if(t2.isBye&&!t1.isBye&&!t1.tbd) first[i].winner=1;
      else first[i].winner=null;
    } else {
      // 若之前有 winner 但隊伍變了就清
      if(first[i].winner===1&&(t1.tbd||t1.isBye)) first[i].winner=null;
      if(first[i].winner===2&&(t2.tbd||t2.isBye)) first[i].winner=null;
    }
  }
  // propagate 後續輪
  propagateSingle();
}

/* ===== Propagate ===== */
function getLoser(m){if(!m.winner)return null;return m.winner===1?m.t2:m.t1}
function getWinner(m){if(!m.winner)return null;return m.winner===1?m.t1:m.t2}

function propagateSingle(){
  const rounds=STATE.wb;
  for(let r=0;r<rounds.length-1;r++){
    const cur=rounds[r], next=rounds[r+1];
    for(let i=0;i<cur.length;i++){
      const m=cur[i], ni=Math.floor(i/2), side=(i%2===0)?'t1':'t2', nm=next[ni];
      if(m.winner===1) nm[side]={...m.t1,isBye:false,tbd:false};
      else if(m.winner===2) nm[side]={...m.t2,isBye:false,tbd:false};
      else nm[side]=tbd();
    }
    for(const nm of next){
      if(nm.winner===1&&(nm.t1.tbd||nm.t1.isBye)) nm.winner=null;
      if(nm.winner===2&&(nm.t2.tbd||nm.t2.isBye)) nm.winner=null;
      if(!nm.winner){
        if(nm.t1.isBye&&!nm.t2.isBye&&!nm.t2.tbd) nm.winner=2;
        else if(nm.t2.isBye&&!nm.t1.isBye&&!nm.t1.tbd) nm.winner=1;
      }
    }
  }
}

function propagateDouble(){
  propagateSingle();
  const wb=STATE.wb, lb=STATE.lb;
  const wbLosers=wb.map(round=>round.map(m=>{
    if(m.isBye||!m.winner) return null;
    const loser=getLoser(m);
    if(!loser||loser.isBye||loser.tbd) return null;
    return{...loser,isBye:false,tbd:false};
  }));
  for(let li=0;li<lb.length;li++){
    const round=lb[li];
    const prevWinners=li>0?lb[li-1].matches.map(m=>{
      const w=getWinner(m); return w&&!w.tbd?{...w,isBye:false,tbd:false}:null;
    }):[];
    if(round.type==='drop'){
      const fromWb=round.fromWb;
      const losers=(fromWb!=null&&wbLosers[fromWb])?wbLosers[fromWb].filter(Boolean):[];
      if(li===0){
        for(let i=0;i<round.matches.length;i++){
          const m=round.matches[i];
          const a=losers[i*2]||null, b=losers[i*2+1]||null;
          m.t1=a?{...a}:tbd();
          m.t2=b?{...b}:(a?bye():tbd());
          if(m.winner===1&&(m.t1.tbd||m.t1.isBye)) m.winner=null;
          if(m.winner===2&&(m.t2.tbd||m.t2.isBye)) m.winner=null;
        }
      } else {
        let li2=0;
        for(let i=0;i<round.matches.length;i++){
          const m=round.matches[i];
          m.t1=prevWinners[i]?{...prevWinners[i]}:tbd();
          m.t2=li2<losers.length?{...losers[li2++]}:tbd();
          if(m.winner===1&&(m.t1.tbd||m.t1.isBye)) m.winner=null;
          if(m.winner===2&&(m.t2.tbd||m.t2.isBye)) m.winner=null;
        }
      }
    } else {
      for(let i=0;i<round.matches.length;i++){
        const m=round.matches[i];
        const a=prevWinners[i*2]||null, b=prevWinners[i*2+1]||null;
        m.t1=a?{...a}:tbd();
        m.t2=b?{...b}:(a?bye():tbd());
        if(m.winner===1&&(m.t1.tbd||m.t1.isBye)) m.winner=null;
        if(m.winner===2&&(m.t2.tbd||m.t2.isBye)) m.winner=null;
      }
    }
  }
  const wbChamp=getWinner(wb[wb.length-1][0]);
  const lbLast=lb[lb.length-1];
  const lbChamp=lbLast&&lbLast.matches[0]?getWinner(lbLast.matches[0]):null;
  STATE.grand.t1=wbChamp&&!wbChamp.tbd?{...wbChamp,tbd:false}:team('勝者組冠軍',null,{tbd:true});
  STATE.grand.t2=lbChamp&&!lbChamp.tbd?{...lbChamp,tbd:false}:team('敗者組冠軍',null,{tbd:true});
  if(STATE.grand.winner===1&&STATE.grand.t1.tbd) STATE.grand.winner=null;
  if(STATE.grand.winner===2&&STATE.grand.t2.tbd) STATE.grand.winner=null;
}

function propagate(){
  if(!STATE) return;
  if(STATE.type==='single') propagateSingle();
  else if(STATE.type==='double') propagateDouble();
  else if(STATE.type==='group'){
    fillKnockoutFromGroups();
  }
}

/* ===== Clicks ===== */
function setWBWinner(r,mi,side){
  const m=STATE.wb[r][mi];
  if(m.isBye&&!((side===1&&!m.t1.isBye&&!m.t1.tbd)||(side===2&&!m.t2.isBye&&!m.t2.tbd))) return;
  if(side===1&&(m.t1.tbd||m.t1.isBye)) return;
  if(side===2&&(m.t2.tbd||m.t2.isBye)) return;
  m.winner=m.winner===side?null:side;
  propagate(); render();
}
function setLBWinner(r,mi,side){
  const m=STATE.lb[r].matches[mi];
  if(side===1){ if(m.t1.tbd||m.t1.isBye) return; if(m.t2.tbd&&!m.t2.isBye) return; }
  else { if(m.t2.tbd||m.t2.isBye) return; if(m.t1.tbd&&!m.t1.isBye) return; }
  m.winner=m.winner===side?null:side;
  propagate(); render();
}
function setGFWinner(side){
  const m=STATE.grand;
  if(m.t1.tbd||m.t2.tbd) return;
  m.winner=m.winner===side?null:side;
  render();
}
function setGroupWinner(gi,mi,side){
  const m=STATE.groups[gi].matches[mi];
  if(m.t1.tbd||m.t2.tbd) return;
  m.winner=m.winner===side?null:side;
  // 更新 members 名稱顯示（已有）
  propagate(); render();
}

function getChampion(){
  if(!STATE) return null;
  if(STATE.type==='double') return getWinner(STATE.grand);
  const last=STATE.wb[STATE.wb.length-1][0];
  return getWinner(last);
}

function countDecided(){
  let decided=0,total=0;
  const cnt=ms=>{for(const m of ms){if(m.isBye)continue;total++;if(m.winner)decided++}};
  if(STATE.type==='group'){
    for(const g of STATE.groups) cnt(g.matches);
    for(const r of STATE.wb) cnt(r);
  } else {
    for(const r of STATE.wb) cnt(r);
    if(STATE.type==='double'){
      for(const r of STATE.lb) cnt(r.matches);
      if(!STATE.grand.t1.tbd&&!STATE.grand.t2.tbd){total++;if(STATE.grand.winner)decided++}
    }
  }
  return{decided,total};
}

/* ===== Render helpers ===== */
function renderSlot(t,winner,side,onclickStr){
  let cls='slot-team';
  if(t.isBye) cls+=' bye-team disabled';
  else if(t.tbd) cls+=' tbd disabled';
  else if(winner===side) cls+=' winner';
  else if(winner&&winner!==side) cls+=' loser';
  if(!onclickStr) cls+=' disabled';
  const seed=t.seed?'<span class="seed-tag">#'+t.seed+'</span>':'';
  const icon=winner===side?'<span class="win-icon">✓</span>':'';
  const oc=onclickStr?'onclick="'+onclickStr+'"':'';
  return '<div class="'+cls+'" '+oc+'>'+seed+'<span>'+esc(t.name)+'</span>'+icon+'</div>';
}
function renderMatchCard(m,on1,on2){
  const decided=m.winner!==null;
  const cls='match-card'+(decided?' decided':'')+(m.isBye?' bye-match':'');
  return '<div class="'+cls+'">'+renderSlot(m.t1,m.winner,1,on1)+renderSlot(m.t2,m.winner,2,on2)+'</div>';
}

function renderGroupsBlock(chartMode){
  let html='<div class="section-div grp">🔵 小組賽（每組 4 隊，積分前 2 名晉級）</div>';
  if(chartMode){
    html+='<div class="groups-grid">';
  } else {
    html+='<div class="groups-grid">';
  }
  STATE.groups.forEach((g,gi)=>{
    const ranked=calcGroupStandings(g);
    const allDone=g.matches.every(m=>m.winner!==null);
    html+='<div class="group-card">';
    html+='<h3>'+esc(g.name)+(allDone?' <span class="adv-badge">已結束</span>':'')+'</h3>';
    // 積分表
    html+='<table class="standings"><thead><tr><th>#</th><th>隊伍</th><th>勝</th><th>負</th><th>分</th></tr></thead><tbody>';
    ranked.forEach((m,idx)=>{
      const cls=allDone?(idx<2?'adv':'out'):'';
      html+='<tr class="'+cls+'"><td>'+(idx+1)+'</td><td>'+esc(m.name)+(allDone&&idx<2?' ↑':'')+'</td><td>'+m.w+'</td><td>'+m.l+'</td><td>'+m.pts+'</td></tr>';
    });
    html+='</tbody></table>';
    // 比賽
    html+='<div class="group-matches">';
    g.matches.forEach((m,mi)=>{
      const c1='setGroupWinner('+gi+','+mi+',1)';
      const c2='setGroupWinner('+gi+','+mi+',2)';
      html+=renderMatchCard(m,c1,c2);
    });
    html+='</div></div>';
  });
  html+='</div>';
  return html;
}

function renderKOBlock(title, chartMode){
  let html='<div class="section-div w">'+title+'</div>';
  if(chartMode){
    html+='<div class="bracket-scroll"><div class="bracket-chart">';
    const baseH=70;
    STATE.wb.forEach((matches,r)=>{
      const size=matches.length*2;
      const isFinal=r===STATE.wb.length-1;
      const blockH=baseH*Math.pow(2,r);
      const padTop=(blockH-baseH)/2;
      html+='<div class="round-col">';
      html+='<div class="round-col-title'+(isFinal?' final':'')+'">'+rndName(size)+'</div>';
      matches.forEach((m,mi)=>{
        html+='<div class="match-slot" style="margin-top:'+(mi===0?padTop:blockH-baseH)+'px">';
        const c1=(!m.t1.tbd&&!m.t1.isBye)?'setWBWinner('+r+','+mi+',1)':'';
        const c2=(!m.t2.tbd&&!m.t2.isBye)?'setWBWinner('+r+','+mi+',2)':'';
        html+=renderMatchCard(m,c1,c2);
        html+='</div>';
      });
      html+='</div>';
    });
    html+='</div></div>';
  } else {
    STATE.wb.forEach((matches,r)=>{
      const size=matches.length*2;
      const real=matches.filter(m=>!m.isBye).length;
      html+='<div class="round-title">'+rndName(size)+'（'+real+' 場）</div>';
      html+='<div class="matches">';
      matches.forEach((m,mi)=>{
        const c1=(!m.t1.tbd&&!m.t1.isBye)?'setWBWinner('+r+','+mi+',1)':'';
        const c2=(!m.t2.tbd&&!m.t2.isBye)?'setWBWinner('+r+','+mi+',2)':'';
        html+=renderMatchCard(m,c1,c2);
      });
      html+='</div>';
    });
  }
  return html;
}

function renderList(){
  let html='';
  if(STATE.type==='group'){
    html+=renderGroupsBlock(false);
    html+=renderKOBlock('🟢 淘汰賽（小組前兩名晉級）', false);
  } else {
    const isD=STATE.type==='double';
    html+='<div class="section-div w">🟢 '+(isD?'勝者組（輸一次掉入敗者組）':'單敗淘汰賽')+'</div>';
    if(!isD) html+='<p class="ko-flow">由左至右（或由上至下）逐輪點選勝者，直至決賽冠軍</p>';
    STATE.wb.forEach((matches,r)=>{
      const size=matches.length*2;
      const real=matches.filter(m=>!m.isBye).length;
      const decided=matches.filter(m=>!m.isBye&&m.winner).length;
      html+='<div class="round-title">'+(isD?'勝者組 · ':'')+rndName(size)+'（'+decided+'/'+real+' 場已決）</div>';
      html+='<div class="matches">';
      matches.forEach((m,mi)=>{
        const c1=(!m.isBye&&!m.t1.tbd&&!m.t1.isBye)?'setWBWinner('+r+','+mi+',1)':'';
        const c2=(!m.isBye&&!m.t2.tbd&&!m.t2.isBye)?'setWBWinner('+r+','+mi+',2)':'';
        html+=renderMatchCard(m,c1,c2);
      });
      html+='</div>';
    });
    if(STATE.type==='double'){
      html+='<div class="section-div l">🔴 敗者組（再輸一次出局）</div>';
      STATE.lb.forEach((round,ri)=>{
        html+='<div class="round-title lr">'+round.name+'</div>';
        html+='<div class="matches">';
        round.matches.forEach((m,mi)=>{
          const c1=(!m.t1.tbd&&!m.t1.isBye&&(!m.t2.tbd||m.t2.isBye))?'setLBWinner('+ri+','+mi+',1)':'';
          const c2=(!m.t2.tbd&&!m.t2.isBye&&(!m.t1.tbd||m.t1.isBye))?'setLBWinner('+ri+','+mi+',2)':'';
          html+=renderMatchCard(m,c1,c2);
        });
        html+='</div>';
      });
      html+='<div class="section-div g">🟡 總決賽</div>';
      html+='<div class="matches">';
      const g=STATE.grand;
      html+=renderMatchCard(g,!g.t1.tbd?'setGFWinner(1)':'',!g.t2.tbd?'setGFWinner(2)':'');
      html+='</div>';
    }
  }
  const champ=getChampion();
  html+='<div class="champ-panel">';
  if(champ&&!champ.tbd) html+='<h3>🏆 冠軍誕生！</h3><div class="champ-name">'+esc(champ.name)+'</div>';
  else html+='<h3>🏆 冠軍</h3><p style="color:#94a3b8;font-size:14px">完成所有比賽後顯示</p>';
  html+='</div>';
  return html;
}

function renderChart(){
  let html='';
  if(STATE.type==='group'){
    html+=renderGroupsBlock(true);
    html+=renderKOBlock('🟢 淘汰賽圖表（小組前兩名）', true);
  } else {
    const isDouble = STATE.type==='double';
    html+='<div class="bracket-legend">';
    html+='<span><i class="legend-dot w"></i> 綠框 / 綠底 = 已決出勝者</span>';
    html+='<span><i class="legend-dot u"></i> 點擊隊伍名稱選擇勝者</span>';
    if(isDouble){
      html+='<span><i class="legend-dot w"></i> 勝者組：輸一次掉入敗者組</span>';
      html+='<span><i class="legend-dot l"></i> 敗者組：再輸出局</span>';
      html+='<span><i class="legend-dot g"></i> 總決賽</span>';
    }
    html+='</div>';
    html+='<div class="section-div w">🟢 '+(isDouble?'勝者組（Winners Bracket）':'單敗淘汰圖表')+'</div>';
    if(isDouble) html+='<p class="ko-flow">流向：左 → 右 ｜ 勝者向右晉級 ｜ 敗者掉入下方敗者組</p>';
    else html+='<p class="ko-flow">流向：左 → 右 ｜ 每輪點選勝者，自動晉級下一輪直至冠軍</p>';
    html+='<div class="bracket-scroll"><div class="bracket-chart">';
    const baseH=70;
    const totalR=STATE.wb.length;
    STATE.wb.forEach((matches,r)=>{
      const size=matches.length*2;
      const isFinal=r===totalR-1;
      const blockH=baseH*Math.pow(2,r);
      const padTop=(blockH-baseH)/2;
      const decided=matches.filter(m=>!m.isBye&&m.winner).length;
      const real=matches.filter(m=>!m.isBye).length;
      const label=(isFinal?'🏆 ':'')+rndName(size)+' · '+decided+'/'+real;
      html+='<div class="round-col"><div class="round-col-title'+(isFinal?' final':'')+'">'+label+'</div>';
      matches.forEach((m,mi)=>{
        html+='<div class="match-slot" style="margin-top:'+(mi===0?padTop:blockH-baseH)+'px">';
        const c1=(!m.isBye&&!m.t1.tbd&&!m.t1.isBye)?'setWBWinner('+r+','+mi+',1)':'';
        const c2=(!m.isBye&&!m.t2.tbd&&!m.t2.isBye)?'setWBWinner('+r+','+mi+',2)':'';
        html+=renderMatchCard(m,c1,c2);
        html+='</div>';
      });
      html+='</div>';
    });
    html+='</div></div>';
    if(STATE.type==='double'){
      html+='<div class="section-div l">🔴 敗者組（Losers Bracket）</div>';
      html+='<p class="ko-flow">流向：左 → 右 ｜ 接收勝者組敗者 ｜ 點選晉級直至敗者組冠軍</p>';
      html+='<div class="bracket-scroll"><div class="bracket-chart">';
      const lb=STATE.lb, lbBaseH=70, lbFirst=Math.max(1,lb[0]?lb[0].matches.length:1);
      lb.forEach((round,ri)=>{
        const matches=round.matches;
        const scale=Math.max(1,Math.pow(2,Math.min(ri,4))*(lbFirst/Math.max(1,matches.length)));
        const blockH=lbBaseH*scale;
        const padTop=Math.max(0,(blockH-lbBaseH)/2);
        const isLast=ri===lb.length-1;
        const dec=matches.filter(m=>!m.isBye&&m.winner).length;
        const realM=matches.filter(m=>!m.isBye).length;
        html+='<div class="round-col"><div class="round-col-title lr-title'+(isLast?' final':'')+'">'+(isLast?'🏆 ':'')+esc(round.name)+' · '+dec+'/'+realM+'</div>';
        matches.forEach((m,mi)=>{
          const top=mi===0?padTop:Math.max(8,blockH-lbBaseH);
          html+='<div class="match-slot" style="margin-top:'+top+'px">';
          const c1=(!m.t1.tbd&&!m.t1.isBye&&(!m.t2.tbd||m.t2.isBye))?'setLBWinner('+ri+','+mi+',1)':'';
          const c2=(!m.t2.tbd&&!m.t2.isBye&&(!m.t1.tbd||m.t1.isBye))?'setLBWinner('+ri+','+mi+',2)':'';
          html+=renderMatchCard(m,c1,c2);
          html+='</div>';
        });
        html+='</div>';
      });
      html+='</div></div>';
      html+='<div class="section-div g">🟡 總決賽</div>';
      html+='<div class="bracket-scroll"><div class="bracket-chart"><div class="round-col">';
      html+='<div class="round-col-title final">總決賽</div><div class="match-slot">';
      const g=STATE.grand;
      html+=renderMatchCard(g,!g.t1.tbd?'setGFWinner(1)':'',!g.t2.tbd?'setGFWinner(2)':'');
      html+='</div></div></div></div>';
    }
  }
  const champ=getChampion();
  html+='<div class="champ-panel">';
  if(champ&&!champ.tbd) html+='<h3>🏆 冠軍誕生！</h3><div class="champ-name">'+esc(champ.name)+'</div>';
  else html+='<h3>🏆 冠軍</h3><p style="color:#94a3b8">完成所有比賽後顯示</p>';
  html+='</div>';
  return html;
}

function setView(v){
  currentView=v;
  document.getElementById('btn-list').classList.toggle('active',v==='list');
  document.getElementById('btn-chart').classList.toggle('active',v==='chart');
  document.getElementById('btn-list').classList.toggle('secondary',v!=='list');
  document.getElementById('btn-chart').classList.toggle('secondary',v!=='chart');
  render();
}

function render(){
  if(!STATE) return;
  const ct=countDecided();
  const pct=ct.total?Math.round(ct.decided/ct.total*100):0;
  document.getElementById('progress').style.width=pct+'%';
  const champ=getChampion();
  const typeLabel=STATE.type==='single'?'單敗':STATE.type==='double'?'雙敗':'小組四晉二';
  let sh='<span>賽制：'+typeLabel+'</span><span>隊伍：'+STATE.teams+'</span>';
  if(STATE.type==='group') sh+='<span>小組：'+STATE.groupCount+'</span>';
  else sh+='<span>輪空：'+STATE.byes+'</span>';
  sh+='<span>已決出：'+ct.decided+' / '+ct.total+'（'+pct+'%）</span>';
  if(champ&&!champ.tbd) sh+='<span style="color:#86efac">🏆 '+esc(champ.name)+'</span>';
  document.getElementById('stats').innerHTML=sh;
  document.getElementById('bracket-content').innerHTML=currentView==='chart'?renderChart():renderList();
}

function resetResults(){
  if(!STATE) return;
  if(STATE.type==='group'){
    for(const g of STATE.groups) for(const m of g.matches) m.winner=null;
    for(const r of STATE.wb) for(const m of r){ m.winner=null; m.t1=tbd(); m.t2=tbd(); m.isBye=false; }
  } else {
    for(const round of STATE.wb) for(const m of round){
      if(m.isBye){ if(m.t1.isBye&&!m.t2.isBye)m.winner=2; else if(m.t2.isBye&&!m.t1.isBye)m.winner=1; }
      else m.winner=null;
    }
    if(STATE.lb) for(const r of STATE.lb) for(const m of r.matches) m.winner=null;
    if(STATE.grand) STATE.grand.winner=null;
  }
  propagate(); render();
}

function getTeams(){return document.getElementById('teams-input').value.split('\n').map(s=>s.trim()).filter(Boolean)}
function fillDefault(c){const n=[];for(let i=1;i<=c;i++)n.push('隊伍 '+i);document.getElementById('teams-input').value=n.join('\n')}
function clearTeams(){document.getElementById('teams-input').value=''}
function shuffleInput(){document.getElementById('teams-input').value=shuffle(getTeams()).join('\n')}

function generate(){
  const teams=getTeams();
  if(teams.length<2){alert('請至少輸入 2 支隊伍');return}
  if(teams.length>128){alert('最多 128 隊');return}
  const format=document.getElementById('format').value;
  const mode=document.querySelector('input[name="mode"]:checked').value;
  if(format==='group' && teams.length<4){alert('小組賽至少需要 4 支隊伍');return}
  if(format==='single') STATE=buildSingle(teams,mode);
  else if(format==='double') STATE=buildDouble(teams,mode);
  else STATE=buildGroup(teams,mode);
  propagate();
  document.getElementById('bracket-area').style.display='block';
  currentView=format==='group'?'chart':'list';
  setView(currentView);
  window.scrollTo({top:document.getElementById('bracket-area').offsetTop-12,behavior:'smooth'});
}

function copyText(){
  if(!STATE) return;
  const typeLabel=STATE.type==='single'?'單敗':STATE.type==='double'?'雙敗':'小組四晉二';
  let t='【'+typeLabel+'】隊伍'+STATE.teams+'\n\n';
  if(STATE.type==='group'){
    STATE.groups.forEach(g=>{
      t+='=== '+g.name+' ===\n';
      const ranked=calcGroupStandings(g);
      ranked.forEach((m,i)=>{t+='  '+(i+1)+'. '+m.name+'  '+m.pts+'分 ('+m.w+'勝'+m.l+'負)\n'});
      t+='\n';
    });
    t+='【淘汰賽】\n';
  }
  STATE.wb.forEach(matches=>{
    t+='--- '+rndName(matches.length*2)+' ---\n';
    matches.forEach((m,i)=>{
      let w=m.winner===1?' ←勝':m.winner===2?' 勝→':'';
      t+='  '+(i+1)+'. '+m.t1.name+' vs '+m.t2.name+w+'\n';
    });
  });
  const champ=getChampion();
  if(champ&&!champ.tbd) t+='\n🏆 冠軍：'+champ.name+'\n';
  navigator.clipboard.writeText(t).then(()=>alert('已複製！'));
}

fillDefault(16);
</script>
</body>
</html>
