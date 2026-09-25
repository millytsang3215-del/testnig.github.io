<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>淘汰賽產生器</title>
<style>
:root{--bg:#0f172a;--card:#1e293b;--line:#334155;--text:#f1f5f9;--muted:#94a3b8;--blue:#3b82f6;--green:#22c55e;--red:#ef4444;--amber:#f59e0b}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:system-ui,"Noto Sans TC","Microsoft JhengHei",sans-serif;background:var(--bg);color:var(--text);padding:16px;line-height:1.4}
.wrap{max-width:1400px;margin:0 auto}
h1{text-align:center;font-size:1.6rem;margin-bottom:4px;color:#86efac}
.sub{text-align:center;color:var(--muted);font-size:13px;margin-bottom:16px}
.card{background:var(--card);border:1px solid var(--line);border-radius:12px;padding:16px;margin-bottom:14px}
.card h2{font-size:1.05rem;color:#93c5fd;margin-bottom:10px}
textarea{width:100%;min-height:120px;background:#0f172a;border:1px solid var(--line);border-radius:8px;color:var(--text);padding:10px;font-size:14px;font-family:inherit}
.row{display:flex;flex-wrap:wrap;gap:8px;margin-top:10px;align-items:center}
button{border:0;border-radius:8px;padding:10px 14px;font-size:14px;font-weight:600;cursor:pointer;background:var(--blue);color:#fff}
button:hover{filter:brightness(1.1)}
button.g{background:var(--green)}
button.s{background:#475569}
button.d{background:var(--red)}
button.a{background:var(--amber);color:#111}
label{font-size:13px;margin-right:8px}
select,input[type=radio]{accent-color:var(--blue)}
select{background:#0f172a;color:var(--text);border:1px solid var(--line);border-radius:6px;padding:6px 8px}
.note{font-size:12px;color:var(--muted);margin-top:8px}
#out{display:none}
.stats{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:10px}
.stats span{background:#0f172a;padding:5px 10px;border-radius:6px;font-size:12px;color:var(--muted)}
.hint{background:#1e3a5f;border:1px solid var(--blue);color:#bfdbfe;padding:10px;border-radius:8px;font-size:13px;margin-bottom:12px}
.err{background:#450a0a;border:1px solid var(--red);color:#fecaca;padding:10px;border-radius:8px;margin:10px 0;display:none;white-space:pre-wrap}
.sec{margin:16px 0 8px;padding:8px 12px;background:#0f172a;border-radius:8px;font-weight:700;font-size:14px;border-left:4px solid var(--green)}
.sec.l{border-left-color:var(--red)}
.sec.y{border-left-color:var(--amber)}
.sec.b{border-left-color:var(--blue)}
.rt{font-size:13px;color:#93c5fd;margin:12px 0 8px;padding-bottom:4px;border-bottom:1px solid var(--line)}
.rt.l{color:#fca5a5}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:8px}
.mc{background:#0f172a;border:1px solid var(--line);border-radius:8px;overflow:hidden}
.mc.on{border-color:var(--green)}
.tm{padding:9px 10px;font-size:13px;cursor:pointer;display:flex;gap:6px;align-items:center;border-bottom:1px solid var(--line);user-select:none}
.tm:last-child{border-bottom:0}
.tm:hover{background:#1e3a5f}
.tm.win{background:#166534;color:#bbf7d0;font-weight:700}
.tm.lose{opacity:.4;text-decoration:line-through}
.tm.muted{color:var(--muted);cursor:default}
.tm.muted:hover{background:transparent}
.seed{font-size:11px;color:var(--muted);min-width:24px}
.groups{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:12px}
.gc{background:#0f172a;border:1px solid var(--line);border-radius:10px;padding:12px}
.gc h3{font-size:14px;color:#93c5fd;margin-bottom:8px}
table{width:100%;border-collapse:collapse;font-size:12px;margin-bottom:8px}
th,td{padding:4px 6px;border-bottom:1px solid var(--line);text-align:left}
th{color:var(--muted)}
tr.up td{color:#86efac;font-weight:700}
.scroll{overflow-x:auto;padding-bottom:8px}
.chart{display:flex;min-width:max-content;align-items:flex-start}
.col{min-width:190px;padding:0 12px;border-right:1px dashed var(--line)}
.col:last-child{border-right:0}
.ct{text-align:center;font-size:12px;font-weight:700;color:#93c5fd;background:#0f172a;border:1px solid var(--line);border-radius:8px;padding:6px;margin-bottom:12px}
.ct.f{color:#fcd34d;border-color:#b45309}
.ct.l{color:#fca5a5;border-color:#7f1d1d}
.slot{margin-top:8px}
.champ{text-align:center;margin-top:18px;padding:20px;border-radius:12px;border:2px solid var(--green);background:linear-gradient(135deg,#1e3a5f,#14532d)}
.champ .n{font-size:1.5rem;font-weight:800;color:#bbf7d0;margin-top:6px}
.flow{font-size:12px;color:var(--muted);margin:4px 0 10px}
</style>
</head>
<body>
<div class="wrap">
  <h1>淘汰賽產生器</h1>
  <p class="sub">單敗 · 雙敗 · 小組賽四晉二 · 點選晉級</p>

  <div class="card">
    <h2>1. 隊伍名單</h2>
    <div class="row" style="margin-bottom:10px">
      <button type="button" class="g" id="btnUpload">📂 上傳 Excel / CSV / Google表單</button>
      <input type="file" id="fileInput" accept=".csv,.tsv,.txt,.xlsx,.xls,text/csv,application/vnd.ms-excel,application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" style="display:none">
      <span class="note" id="uploadStatus" style="margin:0"></span>
    </div>
    <div id="colPick" style="display:none;margin-bottom:10px">
      <label>選擇名稱欄位
        <select id="colSelect"></select>
      </label>
      <button type="button" class="s" id="btnApplyCol">套用此欄</button>
    </div>
    <textarea id="teams" placeholder="可手動輸入（每行一隊），或上方上傳檔案自動填入"></textarea>
    <div class="row">
      <button type="button" class="s" id="btn8">8隊</button>
      <button type="button" class="s" id="btn16">16隊</button>
      <button type="button" class="s" id="btn32">32隊</button>
      <button type="button" class="s" id="btnClear">清空</button>
      <button type="button" class="s" id="btnShuffle">打亂</button>
    </div>
    <div class="row" style="margin-top:12px">
      <label>賽制
        <select id="format">
          <option value="single">單敗淘汰</option>
          <option value="double">雙敗淘汰</option>
          <option value="group">小組賽（四晉二）</option>
        </select>
      </label>
      <label><input type="radio" name="mode" value="random" checked> 隨機</label>
      <label><input type="radio" name="mode" value="seeded"> 種子</label>
    </div>
    <p class="note">支援 .xlsx / .xls / .csv（Google 表單 → 試算表 → 下載 CSV 或 Excel）。小組賽建議 4 的倍數隊伍。</p>
    <div class="row">
      <button type="button" class="g" id="btnGo">產生對戰表</button>
    </div>
    <div class="err" id="err"></div>
  </div>

  <div class="card" id="out">
    <h2>2. 對戰表</h2>
    <div class="stats" id="stats"></div>
    <div class="hint">點擊隊伍名稱選擇勝者；再點一次可取消。</div>
    <div class="row">
      <button type="button" class="s" id="btnList">列表</button>
      <button type="button" class="s" id="btnChart">圖表</button>
      <button type="button" class="s" id="btnPrint">列印</button>
      <button type="button" class="d" id="btnReset">重置賽果</button>
    </div>
    <div id="content"></div>
  </div>
</div>

<script src="https://cdn.sheetjs.com/xlsx-0.20.3/package/dist/xlsx.full.min.js"></script>
<script>
(function () {
  "use strict";

  function $(id) { return document.getElementById(id); }
  var pendingRows = null; // 2D array after file parse
  function showErr(msg) {
    var e = $("err");
    e.style.display = msg ? "block" : "none";
    e.textContent = msg || "";
  }
  function shuffle(arr) {
    var a = arr.slice();
    for (var i = a.length - 1; i > 0; i--) {
      var j = Math.floor(Math.random() * (i + 1));
      var t = a[i]; a[i] = a[j]; a[j] = t;
    }
    return a;
  }
  function nextPow2(n) {
    var p = 1;
    while (p < n) p *= 2;
    return p;
  }
  function seedOrder(size) {
    function s(n) {
      if (n === 1) return [1];
      var h = s(n / 2), r = [];
      for (var i = 0; i < h.length; i++) {
        r.push(h[i]);
        r.push(n + 1 - h[i]);
      }
      return r;
    }
    return s(size).map(function (x) { return x - 1; });
  }
  function T(name, seed, extra) {
    var o = { name: name, seed: seed || null, isBye: false, tbd: false };
    if (extra) for (var k in extra) o[k] = extra[k];
    return o;
  }
  function TBD() { return { name: "待定", seed: null, isBye: false, tbd: true }; }
  function BYE() { return { name: "輪空", seed: null, isBye: true, tbd: false }; }
  function esc(s) {
    return String(s).replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
  }
  function roundName(size) {
    var m = { 2: "決賽", 4: "準決賽", 8: "8強", 16: "16強", 32: "32強", 64: "64強" };
    return m[size] || (size + "強");
  }
  function winnerOf(m) {
    if (!m || !m.winner) return null;
    return m.winner === 1 ? m.t1 : m.t2;
  }
  function loserOf(m) {
    if (!m || !m.winner) return null;
    return m.winner === 1 ? m.t2 : m.t1;
  }

  var STATE = null;
  var view = "list";

  function getTeams() {
    return $("teams").value.split(/\r?\n/).map(function (s) { return s.trim(); }).filter(Boolean);
  }
  function fill(n) {
    var lines = [];
    for (var i = 1; i <= n; i++) lines.push("隊伍 " + i);
    $("teams").value = lines.join("\n");
  }
  function getMode() {
    var el = document.querySelector('input[name="mode"]:checked');
    return el ? el.value : "random";
  }

  /* ---- build single ---- */
  function buildSingle(teams, mode) {
    var n = teams.length;
    var bs = nextPow2(n);
    var byes = bs - n;
    var ordered = mode === "seeded" ? teams.slice() : shuffle(teams);
    var slots = new Array(bs);
    var pos = seedOrder(bs);
    var i;
    for (i = 0; i < bs; i++) slots[i] = null;
    for (i = 0; i < n; i++) slots[pos[i]] = T(ordered[i], i + 1);
    for (i = 0; i < bs; i++) if (!slots[i]) slots[i] = BYE();

    var rounds = [];
    var cur = slots.map(function (t) { return Object.assign({}, t); });
    while (cur.length > 1) {
      var matches = [];
      for (i = 0; i < cur.length; i += 2) {
        var t1 = cur[i], t2 = cur[i + 1];
        var isBye = !!(t1.isBye || t2.isBye);
        var w = null;
        if (t1.isBye && !t2.isBye) w = 2;
        else if (t2.isBye && !t1.isBye) w = 1;
        matches.push({ t1: Object.assign({}, t1), t2: Object.assign({}, t2), winner: w, isBye: isBye });
      }
      rounds.push(matches);
      cur = matches.map(function (m) {
        if (m.winner === 1) return Object.assign({}, m.t1, { isBye: false, tbd: false });
        if (m.winner === 2) return Object.assign({}, m.t2, { isBye: false, tbd: false });
        return TBD();
      });
    }
    return { type: "single", teams: n, byes: byes, wb: rounds };
  }

  /* ---- build double ---- */
  function buildDouble(teams, mode) {
    var base = buildSingle(teams, mode);
    var wb = base.wb;
    var lb = [];
    var dropFrom = 0;
    var mc = Math.max(1, Math.floor(wb[0].length / 2) || 1);
    var matches = [], i;
    for (i = 0; i < mc; i++) matches.push({ t1: TBD(), t2: TBD(), winner: null, isBye: false });
    lb.push({ name: "敗者組 第1輪", type: "drop", matches: matches, fromWb: 0 });
    dropFrom = 1;
    var lbSize = matches.length;
    var rn = 2;
    while ((lbSize >= 1 || dropFrom < wb.length) && rn <= 16) {
      var isDrop = (dropFrom < wb.length) && (rn % 2 === 0 || lbSize <= 1);
      if (isDrop && dropFrom < wb.length) {
        var cnt = Math.max(1, lbSize);
        matches = [];
        for (i = 0; i < cnt; i++) matches.push({ t1: TBD(), t2: TBD(), winner: null, isBye: false });
        lb.push({ name: "敗者組 第" + rn + "輪", type: "drop", matches: matches, fromWb: dropFrom });
        dropFrom++;
        lbSize = cnt;
      } else {
        if (lbSize <= 1 && dropFrom >= wb.length) break;
        cnt = Math.max(1, Math.floor(lbSize / 2) || 1);
        matches = [];
        for (i = 0; i < cnt; i++) matches.push({ t1: TBD(), t2: TBD(), winner: null, isBye: false });
        lb.push({ name: "敗者組 第" + rn + "輪", type: "consol", matches: matches });
        lbSize = cnt;
      }
      rn++;
    }
    return {
      type: "double", teams: base.teams, byes: base.byes, wb: wb, lb: lb,
      grand: { t1: TBD(), t2: TBD(), winner: null, isBye: false }
    };
  }

  /* ---- build group ---- */
  function buildGroup(teams, mode) {
    var list = mode === "seeded" ? teams.slice() : shuffle(teams);
    while (list.length % 4 !== 0) list.push("待補" + (list.length + 1));
    var groupCount = list.length / 4;
    var groups = [];
    var g, i;
    for (g = 0; g < groupCount; g++) {
      var members = [];
      for (i = 0; i < 4; i++) {
        members.push(T(list[g * 4 + i], g * 4 + i + 1, { pts: 0, w: 0, l: 0, idx: i }));
      }
      var pairs = [[0, 1], [2, 3], [0, 2], [1, 3], [0, 3], [1, 2]];
      var ms = pairs.map(function (p) {
        return {
          t1: Object.assign({}, members[p[0]]),
          t2: Object.assign({}, members[p[1]]),
          winner: null, isBye: false, a: p[0], b: p[1]
        };
      });
      groups.push({ name: "小組 " + String.fromCharCode(65 + g), members: members, matches: ms });
    }
    var koTeams = [];
    for (i = 0; i < groupCount * 2; i++) koTeams.push("待定");
    var ko = buildSingle(koTeams, "seeded");
    for (g = 0; g < ko.wb.length; g++) {
      for (i = 0; i < ko.wb[g].length; i++) {
        ko.wb[g][i].winner = null;
        ko.wb[g][i].isBye = false;
        ko.wb[g][i].t1 = TBD();
        ko.wb[g][i].t2 = TBD();
      }
    }
    return { type: "group", teams: teams.length, byes: 0, groups: groups, groupCount: groupCount, wb: ko.wb };
  }

  function calcStandings(g) {
    g.members.forEach(function (m) { m.pts = 0; m.w = 0; m.l = 0; });
    g.matches.forEach(function (m) {
      if (!m.winner) return;
      var wi = m.winner === 1 ? m.a : m.b;
      var li = m.winner === 1 ? m.b : m.a;
      g.members[wi].pts += 3;
      g.members[wi].w += 1;
      g.members[li].l += 1;
    });
    return g.members.slice().sort(function (a, b) {
      if (b.pts !== a.pts) return b.pts - a.pts;
      return (a.seed || 99) - (b.seed || 99);
    });
  }

  function propagateSingle() {
    var rounds = STATE.wb;
    var r, i;
    for (r = 0; r < rounds.length - 1; r++) {
      var cur = rounds[r], next = rounds[r + 1];
      for (i = 0; i < cur.length; i++) {
        var m = cur[i];
        var ni = Math.floor(i / 2);
        var side = (i % 2 === 0) ? "t1" : "t2";
        var nm = next[ni];
        if (m.winner === 1) nm[side] = Object.assign({}, m.t1, { isBye: false, tbd: false });
        else if (m.winner === 2) nm[side] = Object.assign({}, m.t2, { isBye: false, tbd: false });
        else nm[side] = TBD();
      }
      next.forEach(function (nm) {
        if (nm.winner === 1 && (nm.t1.tbd || nm.t1.isBye)) nm.winner = null;
        if (nm.winner === 2 && (nm.t2.tbd || nm.t2.isBye)) nm.winner = null;
        if (!nm.winner) {
          if (nm.t1.isBye && !nm.t2.isBye && !nm.t2.tbd) nm.winner = 2;
          else if (nm.t2.isBye && !nm.t1.isBye && !nm.t1.tbd) nm.winner = 1;
        }
      });
    }
  }

  function propagateDouble() {
    propagateSingle();
    var wb = STATE.wb, lb = STATE.lb;
    var wbLosers = wb.map(function (round) {
      return round.map(function (m) {
        if (m.isBye || !m.winner) return null;
        var L = loserOf(m);
        if (!L || L.isBye || L.tbd) return null;
        return Object.assign({}, L, { isBye: false, tbd: false });
      });
    });
    var li, i;
    for (li = 0; li < lb.length; li++) {
      var round = lb[li];
      var prevW = [];
      if (li > 0) {
        prevW = lb[li - 1].matches.map(function (m) {
          var w = winnerOf(m);
          return w && !w.tbd ? Object.assign({}, w, { isBye: false, tbd: false }) : null;
        });
      }
      if (round.type === "drop") {
        var from = round.fromWb;
        var losers = (from != null && wbLosers[from]) ? wbLosers[from].filter(Boolean) : [];
        if (li === 0) {
          for (i = 0; i < round.matches.length; i++) {
            var m = round.matches[i];
            var a = losers[i * 2] || null;
            var b = losers[i * 2 + 1] || null;
            m.t1 = a ? Object.assign({}, a) : TBD();
            m.t2 = b ? Object.assign({}, b) : (a ? BYE() : TBD());
            if (m.winner === 1 && (m.t1.tbd || m.t1.isBye)) m.winner = null;
            if (m.winner === 2 && (m.t2.tbd || m.t2.isBye)) m.winner = null;
          }
        } else {
          var k = 0;
          for (i = 0; i < round.matches.length; i++) {
            m = round.matches[i];
            m.t1 = prevW[i] ? Object.assign({}, prevW[i]) : TBD();
            m.t2 = k < losers.length ? Object.assign({}, losers[k++]) : TBD();
            if (m.winner === 1 && (m.t1.tbd || m.t1.isBye)) m.winner = null;
            if (m.winner === 2 && (m.t2.tbd || m.t2.isBye)) m.winner = null;
          }
        }
      } else {
        for (i = 0; i < round.matches.length; i++) {
          m = round.matches[i];
          a = prevW[i * 2] || null;
          b = prevW[i * 2 + 1] || null;
          m.t1 = a ? Object.assign({}, a) : TBD();
          m.t2 = b ? Object.assign({}, b) : (a ? BYE() : TBD());
          if (m.winner === 1 && (m.t1.tbd || m.t1.isBye)) m.winner = null;
          if (m.winner === 2 && (m.t2.tbd || m.t2.isBye)) m.winner = null;
        }
      }
    }
    var wbChamp = winnerOf(wb[wb.length - 1][0]);
    var lbLast = lb[lb.length - 1];
    var lbChamp = lbLast && lbLast.matches[0] ? winnerOf(lbLast.matches[0]) : null;
    STATE.grand.t1 = wbChamp && !wbChamp.tbd ? Object.assign({}, wbChamp, { tbd: false }) : T("勝者組冠軍", null, { tbd: true });
    STATE.grand.t2 = lbChamp && !lbChamp.tbd ? Object.assign({}, lbChamp, { tbd: false }) : T("敗者組冠軍", null, { tbd: true });
    if (STATE.grand.winner === 1 && STATE.grand.t1.tbd) STATE.grand.winner = null;
    if (STATE.grand.winner === 2 && STATE.grand.t2.tbd) STATE.grand.winner = null;
  }

  function fillKOFromGroups() {
    var adv = [];
    STATE.groups.forEach(function (g) {
      var done = g.matches.every(function (m) { return m.winner !== null; });
      if (!done) { adv.push(null); adv.push(null); return; }
      var ranked = calcStandings(g);
      adv.push(Object.assign({}, ranked[0], { tbd: false, isBye: false }));
      adv.push(Object.assign({}, ranked[1], { tbd: false, isBye: false }));
    });
    var first = STATE.wb[0];
    var bs = first.length * 2;
    var pos = seedOrder(bs);
    var ordered = new Array(bs);
    var i;
    for (i = 0; i < bs; i++) ordered[i] = null;
    for (i = 0; i < adv.length && i < bs; i++) ordered[pos[i]] = adv[i] || TBD();
    for (i = 0; i < bs; i++) if (!ordered[i]) ordered[i] = BYE();
    for (i = 0; i < first.length; i++) {
      var t1 = ordered[i * 2], t2 = ordered[i * 2 + 1];
      first[i].t1 = Object.assign({}, t1);
      first[i].t2 = Object.assign({}, t2);
      first[i].isBye = !!(t1.isBye || t2.isBye);
      if (first[i].isBye) {
        if (t1.isBye && !t2.isBye && !t2.tbd) first[i].winner = 2;
        else if (t2.isBye && !t1.isBye && !t1.tbd) first[i].winner = 1;
        else first[i].winner = null;
      } else {
        if (first[i].winner === 1 && (t1.tbd || t1.isBye)) first[i].winner = null;
        if (first[i].winner === 2 && (t2.tbd || t2.isBye)) first[i].winner = null;
      }
    }
    propagateSingle();
  }

  function propagate() {
    if (!STATE) return;
    if (STATE.type === "single") propagateSingle();
    else if (STATE.type === "double") propagateDouble();
    else if (STATE.type === "group") fillKOFromGroups();
  }

  /* ---- clicks ---- */
  window.pickWB = function (r, mi, side) {
    try {
      var m = STATE.wb[r][mi];
      if (side === 1 && (m.t1.tbd || m.t1.isBye)) return;
      if (side === 2 && (m.t2.tbd || m.t2.isBye)) return;
      m.winner = m.winner === side ? null : side;
      propagate();
      render();
    } catch (e) { showErr(String(e)); }
  };
  window.pickLB = function (r, mi, side) {
    try {
      var m = STATE.lb[r].matches[mi];
      if (side === 1 && (m.t1.tbd || m.t1.isBye)) return;
      if (side === 2 && (m.t2.tbd || m.t2.isBye)) return;
      if (side === 1 && m.t2.tbd && !m.t2.isBye) return;
      if (side === 2 && m.t1.tbd && !m.t1.isBye) return;
      m.winner = m.winner === side ? null : side;
      propagate();
      render();
    } catch (e) { showErr(String(e)); }
  };
  window.pickGF = function (side) {
    try {
      var m = STATE.grand;
      if (m.t1.tbd || m.t2.tbd) return;
      m.winner = m.winner === side ? null : side;
      render();
    } catch (e) { showErr(String(e)); }
  };
  window.pickG = function (gi, mi, side) {
    try {
      var m = STATE.groups[gi].matches[mi];
      m.winner = m.winner === side ? null : side;
      propagate();
      render();
    } catch (e) { showErr(String(e)); }
  };

  function champ() {
    if (!STATE) return null;
    if (STATE.type === "double") return winnerOf(STATE.grand);
    return winnerOf(STATE.wb[STATE.wb.length - 1][0]);
  }

  function countDecided() {
    var d = 0, t = 0;
    function cnt(ms) {
      ms.forEach(function (m) {
        if (m.isBye) return;
        t++;
        if (m.winner) d++;
      });
    }
    if (STATE.type === "group") {
      STATE.groups.forEach(function (g) { cnt(g.matches); });
      STATE.wb.forEach(cnt);
    } else {
      STATE.wb.forEach(cnt);
      if (STATE.type === "double") {
        STATE.lb.forEach(function (r) { cnt(r.matches); });
        if (!STATE.grand.t1.tbd && !STATE.grand.t2.tbd) {
          t++;
          if (STATE.grand.winner) d++;
        }
      }
    }
    return { d: d, t: t };
  }

  function matchHTML(m, c1, c2) {
    function slot(tm, side, on) {
      var cls = "tm";
      if (tm.isBye || tm.tbd) cls += " muted";
      else if (m.winner === side) cls += " win";
      else if (m.winner) cls += " lose";
      var seed = tm.seed ? '<span class="seed">#' + tm.seed + "</span>" : "";
      var mark = m.winner === side ? " ✓" : "";
      var oc = on ? ' onclick="' + on + '"' : "";
      return '<div class="' + cls + '"' + oc + ">" + seed + "<span>" + esc(tm.name) + "</span>" + mark + "</div>";
    }
    var box = "mc" + (m.winner ? " on" : "");
    return '<div class="' + box + '">' + slot(m.t1, 1, c1) + slot(m.t2, 2, c2) + "</div>";
  }

  function renderGroups() {
    var html = '<div class="sec b">小組賽（四晉二）</div><div class="groups">';
    STATE.groups.forEach(function (g, gi) {
      var ranked = calcStandings(g);
      var done = g.matches.every(function (m) { return m.winner !== null; });
      html += '<div class="gc"><h3>' + esc(g.name) + (done ? " · 已結束" : "") + "</h3>";
      html += "<table><tr><th>#</th><th>隊伍</th><th>勝</th><th>負</th><th>分</th></tr>";
      ranked.forEach(function (m, idx) {
        html += '<tr class="' + (done && idx < 2 ? "up" : "") + '"><td>' + (idx + 1) + "</td><td>" + esc(m.name) + (done && idx < 2 ? " ↑" : "") + "</td><td>" + m.w + "</td><td>" + m.l + "</td><td>" + m.pts + "</td></tr>";
      });
      html += "</table>";
      g.matches.forEach(function (m, mi) {
        html += matchHTML(m, "pickG(" + gi + "," + mi + ",1)", "pickG(" + gi + "," + mi + ",2)");
      });
      html += "</div>";
    });
    html += "</div>";
    return html;
  }

  function renderWB(listMode, title) {
    var html = '<div class="sec">' + title + "</div>";
    if (!listMode) {
      html += '<p class="flow">流向：左 → 右 · 點選勝者晉級</p><div class="scroll"><div class="chart">';
      var baseH = 72;
      STATE.wb.forEach(function (matches, r) {
        var size = matches.length * 2;
        var isF = r === STATE.wb.length - 1;
        var block = baseH * Math.pow(2, r);
        var pad = (block - baseH) / 2;
        var real = matches.filter(function (m) { return !m.isBye; }).length;
        var dec = matches.filter(function (m) { return !m.isBye && m.winner; }).length;
        html += '<div class="col"><div class="ct' + (isF ? " f" : "") + '">' + (isF ? "🏆 " : "") + roundName(size) + " · " + dec + "/" + real + "</div>";
        matches.forEach(function (m, mi) {
          html += '<div class="slot" style="margin-top:' + (mi === 0 ? pad : block - baseH) + 'px">';
          var c1 = (!m.t1.tbd && !m.t1.isBye) ? "pickWB(" + r + "," + mi + ",1)" : "";
          var c2 = (!m.t2.tbd && !m.t2.isBye) ? "pickWB(" + r + "," + mi + ",2)" : "";
          html += matchHTML(m, c1, c2) + "</div>";
        });
        html += "</div>";
      });
      html += "</div></div>";
    } else {
      STATE.wb.forEach(function (matches, r) {
        var size = matches.length * 2;
        var real = matches.filter(function (m) { return !m.isBye; }).length;
        var dec = matches.filter(function (m) { return !m.isBye && m.winner; }).length;
        html += '<div class="rt">' + roundName(size) + '（' + dec + '/' + real + '）</div><div class="grid">';
        matches.forEach(function (m, mi) {
          var c1 = (!m.t1.tbd && !m.t1.isBye) ? "pickWB(" + r + "," + mi + ",1)" : "";
          var c2 = (!m.t2.tbd && !m.t2.isBye) ? "pickWB(" + r + "," + mi + ",2)" : "";
          html += matchHTML(m, c1, c2);
        });
        html += "</div>";
      });
    }
    return html;
  }

  function renderLB(listMode) {
    var html = '<div class="sec l">敗者組</div><p class="flow">勝者組敗者會自動掉入 · 點選晉級</p>';
    if (!listMode) {
      html += '<div class="scroll"><div class="chart">';
      STATE.lb.forEach(function (round, ri) {
        var matches = round.matches;
        var isF = ri === STATE.lb.length - 1;
        var dec = matches.filter(function (m) { return m.winner; }).length;
        html += '<div class="col"><div class="ct l' + (isF ? " f" : "") + '">' + esc(round.name) + " · " + dec + "/" + matches.length + "</div>";
        matches.forEach(function (m, mi) {
          html += '<div class="slot">';
          var c1 = (!m.t1.tbd && !m.t1.isBye) ? "pickLB(" + ri + "," + mi + ",1)" : "";
          var c2 = (!m.t2.tbd && !m.t2.isBye) ? "pickLB(" + ri + "," + mi + ",2)" : "";
          html += matchHTML(m, c1, c2) + "</div>";
        });
        html += "</div>";
      });
      html += "</div></div>";
    } else {
      STATE.lb.forEach(function (round, ri) {
        html += '<div class="rt l">' + esc(round.name) + '</div><div class="grid">';
        round.matches.forEach(function (m, mi) {
          var c1 = (!m.t1.tbd && !m.t1.isBye) ? "pickLB(" + ri + "," + mi + ",1)" : "";
          var c2 = (!m.t2.tbd && !m.t2.isBye) ? "pickLB(" + ri + "," + mi + ",2)" : "";
          html += matchHTML(m, c1, c2);
        });
        html += "</div>";
      });
    }
    html += '<div class="sec y">總決賽</div><div class="grid">';
    var g = STATE.grand;
    html += matchHTML(g, !g.t1.tbd ? "pickGF(1)" : "", !g.t2.tbd ? "pickGF(2)" : "");
    html += "</div>";
    return html;
  }

  function render() {
    if (!STATE) return;
    var c = countDecided();
    var ch = champ();
    var label = STATE.type === "single" ? "單敗" : STATE.type === "double" ? "雙敗" : "小組四晉二";
    var st = "<span>賽制：" + label + "</span><span>隊伍：" + STATE.teams + "</span>";
    st += "<span>已決：" + c.d + "/" + c.t + "</span>";
    if (ch && !ch.tbd) st += '<span style="color:#86efac">冠軍：' + esc(ch.name) + "</span>";
    $("stats").innerHTML = st;

    var html = "";
    var listMode = view === "list";
    if (STATE.type === "group") {
      html += renderGroups();
      html += renderWB(listMode, "淘汰賽（小組前兩名）");
    } else {
      html += renderWB(listMode, STATE.type === "double" ? "勝者組" : "單敗淘汰");
      if (STATE.type === "double") html += renderLB(listMode);
    }
    html += '<div class="champ"><h3>冠軍</h3>';
    if (ch && !ch.tbd) html += '<div class="n">' + esc(ch.name) + "</div>";
    else html += '<p style="color:#94a3b8">完成比賽後顯示</p>';
    html += "</div>";
    $("content").innerHTML = html;
  }

  function generate() {
    showErr("");
    try {
      var teams = getTeams();
      if (teams.length < 2) {
        showErr("請至少輸入 2 支隊伍（每行一隊）");
        return;
      }
      if (teams.length > 128) {
        showErr("最多 128 隊");
        return;
      }
      var format = $("format").value;
      var mode = getMode();
      if (format === "group" && teams.length < 4) {
        showErr("小組賽至少需要 4 支隊伍");
        return;
      }
      if (format === "single") STATE = buildSingle(teams, mode);
      else if (format === "double") STATE = buildDouble(teams, mode);
      else STATE = buildGroup(teams, mode);
      propagate();
      $("out").style.display = "block";
      view = format === "group" ? "chart" : "list";
      render();
      $("out").scrollIntoView({ behavior: "smooth", block: "start" });
    } catch (e) {
      showErr("產生失敗：" + e.message + "\n" + (e.stack || ""));
      console.error(e);
    }
  }

  function resetResults() {
    if (!STATE) return;
    try {
      if (STATE.type === "group") {
        STATE.groups.forEach(function (g) {
          g.matches.forEach(function (m) { m.winner = null; });
        });
        STATE.wb.forEach(function (r) {
          r.forEach(function (m) {
            m.winner = null; m.t1 = TBD(); m.t2 = TBD(); m.isBye = false;
          });
        });
      } else {
        STATE.wb.forEach(function (r) {
          r.forEach(function (m) {
            if (m.isBye) {
              if (m.t1.isBye && !m.t2.isBye) m.winner = 2;
              else if (m.t2.isBye && !m.t1.isBye) m.winner = 1;
            } else m.winner = null;
          });
        });
        if (STATE.lb) STATE.lb.forEach(function (r) {
          r.matches.forEach(function (m) { m.winner = null; });
        });
        if (STATE.grand) STATE.grand.winner = null;
      }
      propagate();
      render();
    } catch (e) { showErr(String(e)); }
  }

  function parseCSVText(text) {
    var rows = [];
    var i = 0, field = "", row = [], inQ = false;
    text = text.replace(/^\uFEFF/, "");
    while (i < text.length) {
      var c = text[i];
      if (inQ) {
        if (c === '"') {
          if (text[i + 1] === '"') { field += '"'; i++; }
          else inQ = false;
        } else field += c;
      } else {
        if (c === '"') inQ = true;
        else if (c === "," || c === "\t") { row.push(field); field = ""; }
        else if (c === "\n" || c === "\r") {
          if (c === "\r" && text[i + 1] === "\n") i++;
          row.push(field); field = "";
          if (row.some(function (x) { return String(x).trim(); })) rows.push(row);
          row = [];
        } else field += c;
      }
      i++;
    }
    if (field.length || row.length) {
      row.push(field);
      if (row.some(function (x) { return String(x).trim(); })) rows.push(row);
    }
    return rows;
  }

  function guessNameCol(rows) {
    if (!rows || !rows.length) return 0;
    var header = rows[0].map(function (h) { return String(h || "").toLowerCase(); });
    var keys = ["name", "player", "team", "姓名", "名字", "名稱", "隊員", "隊伍", "選手", "暱稱", "nickname", "participant", "回答"];
    var j, k;
    for (j = 0; j < header.length; j++) {
      for (k = 0; k < keys.length; k++) {
        if (header[j].indexOf(keys[k]) !== -1) return j;
      }
    }
    // pick column with most non-empty unique text values (skip first if looks like timestamp/email)
    var best = 0, bestScore = -1;
    for (j = 0; j < header.length; j++) {
      var set = {};
      var score = 0;
      for (k = 1; k < rows.length; k++) {
        var v = String((rows[k] && rows[k][j]) || "").trim();
        if (!v) continue;
        if (v.indexOf("@") !== -1) continue;
        if (/^\d{4}[-\/]/.test(v)) continue;
        if (!set[v]) { set[v] = 1; score++; }
      }
      if (score > bestScore) { bestScore = score; best = j; }
    }
    return best;
  }

  function applyColumn(colIdx) {
    if (!pendingRows || !pendingRows.length) return;
    var start = 0;
    // skip header row if first cell looks like header
    var first = String(pendingRows[0][colIdx] || "").trim();
    var headerLike = /name|player|team|姓名|名字|名稱|隊員|隊伍|選手|timestamp|時間戳|email/i.test(first);
    if (headerLike) start = 1;
    var names = [];
    var seen = {};
    for (var i = start; i < pendingRows.length; i++) {
      var v = String((pendingRows[i] && pendingRows[i][colIdx]) || "").trim();
      if (!v) continue;
      if (seen[v]) continue;
      seen[v] = 1;
      names.push(v);
    }
    $("teams").value = names.join("\n");
    $("uploadStatus").textContent = "已匯入 " + names.length + " 名";
    $("colPick").style.display = "none";
    showErr("");
  }

  function setupColPicker(rows) {
    pendingRows = rows;
    var maxCols = 0;
    rows.forEach(function (r) { if (r.length > maxCols) maxCols = r.length; });
    var sel = $("colSelect");
    sel.innerHTML = "";
    var header = rows[0] || [];
    for (var j = 0; j < maxCols; j++) {
      var opt = document.createElement("option");
      var label = String(header[j] || "").trim() || ("欄 " + (j + 1));
      opt.value = String(j);
      opt.textContent = label + " （第 " + (j + 1) + " 欄）";
      sel.appendChild(opt);
    }
    var guess = guessNameCol(rows);
    sel.value = String(guess);
    $("colPick").style.display = maxCols > 1 ? "block" : "none";
    applyColumn(guess);
  }

  function handleFile(file) {
    if (!file) return;
    showErr("");
    $("uploadStatus").textContent = "讀取中… " + file.name;
    var name = (file.name || "").toLowerCase();
    var isExcel = /\.xlsx?$/.test(name) || /sheet|excel/.test(file.type || "");

    if (isExcel) {
      if (typeof XLSX === "undefined") {
        showErr("無法載入 Excel 套件。請改用 CSV：Google 表單 → 試算表 → 檔案 → 下載 → CSV");
        $("uploadStatus").textContent = "";
        return;
      }
      var reader = new FileReader();
      reader.onload = function (e) {
        try {
          var data = new Uint8Array(e.target.result);
          var wb = XLSX.read(data, { type: "array" });
          var sheet = wb.Sheets[wb.SheetNames[0]];
          var rows = XLSX.utils.sheet_to_json(sheet, { header: 1, defval: "" });
          rows = rows.filter(function (r) {
            return r && r.some(function (c) { return String(c).trim(); });
          });
          if (!rows.length) { showErr("檔案沒有資料"); return; }
          setupColPicker(rows);
        } catch (err) {
          showErr("Excel 讀取失敗：" + err.message);
        }
      };
      reader.onerror = function () { showErr("檔案讀取失敗"); };
      reader.readAsArrayBuffer(file);
      return;
    }

    // CSV / TSV / TXT
    var reader2 = new FileReader();
    reader2.onload = function (e) {
      try {
        var text = String(e.target.result || "");
        var rows = parseCSVText(text);
        if (!rows.length) { showErr("檔案沒有資料"); return; }
        // if single column plain list
        if (rows.every(function (r) { return r.length <= 1; })) {
          pendingRows = rows;
          applyColumn(0);
          return;
        }
        setupColPicker(rows);
      } catch (err) {
        showErr("CSV 讀取失敗：" + err.message);
      }
    };
    reader2.onerror = function () { showErr("檔案讀取失敗"); };
    reader2.readAsText(file, "UTF-8");
  }

  // bind buttons (no inline onclick on main controls)
  function ready() {
    $("btn8").onclick = function () { fill(8); };
    $("btn16").onclick = function () { fill(16); };
    $("btn32").onclick = function () { fill(32); };
    $("btnClear").onclick = function () { $("teams").value = ""; $("uploadStatus").textContent = ""; };
    $("btnShuffle").onclick = function () {
      $("teams").value = shuffle(getTeams()).join("\n");
    };
    $("btnGo").onclick = generate;
    $("btnList").onclick = function () { view = "list"; render(); };
    $("btnChart").onclick = function () { view = "chart"; render(); };
    $("btnPrint").onclick = function () { window.print(); };
    $("btnReset").onclick = resetResults;

    $("btnUpload").onclick = function () { $("fileInput").click(); };
    $("fileInput").onchange = function () {
      var f = $("fileInput").files && $("fileInput").files[0];
      handleFile(f);
      $("fileInput").value = "";
    };
    $("btnApplyCol").onclick = function () {
      applyColumn(parseInt($("colSelect").value, 10) || 0);
    };

    fill(8);
    showErr("");
  }

  if (document.readyState === "loading") {
    document.addEventListener("DOMContentLoaded", ready);
  } else {
    ready();
  }
})();
</script>
</body>
</html>
