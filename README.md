<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>kibsim v4.4.0</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
    body { font-family:-apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif; padding:10px; max-width: 600px; margin: auto; background: #f0f2f5; color: #333; line-height: 1.5; }
    h2 { text-align: center; margin-bottom: 15px; color: #333; letter-spacing: 1px; }
    
    /* UI Components */
    .card { background: #fff; padding:15px; margin:10px 0; border-radius: 10px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); border: 1px solid #e1e4e8; position: relative; }
    input, select, button { padding: 10px; border-radius: 6px; border: 1px solid #ccc; font-size: 14px; box-sizing: border-box; outline: none; width: 100%; }
    input[type="radio"] { width: auto !important; margin: 0; cursor: pointer; }
    input:focus, select:focus { border-color: #007bff; }
    button { cursor: pointer; background: #f8f9fa; font-weight: 600; transition: 0.2s; width: auto; }
    button:active { transform: translateY(1px); }
    
    /* Section Headers */
    h3 { padding: 12px 15px; margin-top: 15px; background: #2c3e50; color: #fff; border-radius: 8px; cursor: pointer; display: flex; justify-content: space-between; align-items: center; font-size: 15px; user-select: none; }
    h3::after { content: '▼'; font-size: 0.8em; transition: transform 0.2s; }
    h3.closed::after { content: '◀'; transform: none; }
    .section-content { display: block; animation: fade 0.3s; }
    .section-content.hidden { display: none; }
    @keyframes fade { from{opacity:0} to{opacity:1} }
    
    /* Grid & Tabs */
    .num-grid { display: grid; grid-template-columns: repeat(6, 1fr); gap: 6px; padding: 10px; background: #fff; border: 1px solid #ddd; border-radius: 0 0 8px 8px; }
    .num-btn { padding: 12px 0; text-align: center; border: 1px solid #eee; background: #fff; border-radius: 6px; font-weight: bold; cursor: pointer; user-select: none; }
    .num-btn.selected { background: #007bff !important; color: #fff; border-color: #0056b3; }
    .num-btn.jiku { background: #ffc107 !important; color: #000; border-color: #d39e00; }
    
    .form-tabs { display: flex; gap: 2px; margin-top: 10px; }
    .tab { flex: 1; padding: 10px; text-align: center; background: #e9ecef; cursor: pointer; border-radius: 8px 8px 0 0; font-size: 12px; font-weight: bold; border: 1px solid #ddd; border-bottom: none; color: #666; }
    .tab.active { background: #fff; color: #007bff; border-top: 3px solid #007bff; z-index: 2; }
    
    /* Mode Select */
    .mode-container { display: flex; gap: 5px; margin-bottom: 10px; flex-wrap: wrap; }
    .mode-label { flex: 1; display: flex; align-items: center; justify-content: center; gap: 4px; background: #fff; border: 1px solid #ddd; padding: 8px 4px; border-radius: 6px; font-size: 12px; cursor: pointer; white-space: nowrap; }
    .mode-label:has(input:checked) { border-color: #007bff; background: #f0f7ff; color: #007bff; font-weight: bold; }
    .flex { display: flex; justify-content: space-between; align-items: center; gap: 5px; }
    
    /* Filter & List */
    .filter-box { background: #e9ecef; padding: 10px; border-radius: 8px; margin-bottom: 10px; font-size: 0.9em; display: none; }
    .filter-box.show { display: block; }
    .tag { font-size: 10px; padding: 2px 6px; border-radius: 4px; background: #e9ecef; border: 1px solid #ccc; margin-right: 5px; }
    .picks-display { font-size: 11px; background: #f8f9fa; padding: 5px; border-radius: 4px; border: 1px dashed #ccc; margin-top: 5px; word-break: break-all; color: #555; }
    
    /* Status Colors */
    .status-wait { border-left: 5px solid #007bff; }
    .status-win { border-left: 5px solid #28a745; background: #f3fff5; }
    .status-lose { border-left: 5px solid #dc3545; background: #fff5f5; }
    .status-dep { border-left: 5px solid #6610f2; background: #f8f0ff; }
    
    /* Result Input */
    .res-row { display: flex; align-items: center; gap: 5px; margin-bottom: 5px; }
    .res-lbl { width: 30px; font-weight: bold; font-size: 12px; flex-shrink: 0; }
    .res-inp { width: 45px; text-align: center; }
    /* Total Profit */
    .total-profit-box { text-align: center; padding: 10px; background: #343a40; color: #fff; border-radius: 8px; margin-bottom: 10px; }
    .tp-val { font-size: 1.4em; font-weight: bold; }
    .plus { color: #28a745; } .minus { color: #ff6b6b; }
    
    /* Pre-Calculation Box */
    .calc-box { background: #e3f2fd; padding: 10px; border-radius: 6px; margin: 10px 0; text-align: center; font-weight: bold; color: #0d47a1; border: 1px solid #bbdefb; }
    
    /* Autocomplete */
    .autocomplete { position: relative; display: inline-block; width: 100%; }
    .autocomplete-items { position: absolute; border: 1px solid #d4d4d4; border-bottom: none; border-top: none; z-index: 99; top: 100%; left: 0; right: 0; background: #fff; border-radius: 0 0 6px 6px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); max-height: 200px; overflow-y: auto; }
    .autocomplete-items div { padding: 10px; cursor: pointer; background-color: #fff; border-bottom: 1px solid #d4d4d4; font-size: 13px; }
    .autocomplete-items div:hover { background-color: #e9e9e9; }
    .autocomplete-active { background-color: #007bff !important; color: #ffffff; }
</style>
</head>
<body>
<h2>kibsim</h2>
<div class="card">
    <div class="flex"><span>現在残高:</span><strong style="font-size: 1.4em; color:#28a745;"><span id="balance">0</span> 円</strong></div>
    <hr style="border:0; border-top:1px solid #eee; margin:10px 0;">
    <div class="flex">
        <input type="number" id="depAmt" placeholder="入金額" style="flex:2;">
        <button onclick="deposit()" style="background:#6610f2; color:white; flex:1;">入金</button>
    </div>
    <div class="flex" style="margin-top:10px;">
        <button onclick="exportData()" style="font-size:11px; flex:1;">データ保存</button>
        <label style="background:#6c757d; color:white; padding:8px; border-radius:6px; font-size:11px; cursor:pointer; flex:1; text-align:center; margin:0 5px;">
            データ読込 <input type="file" style="display:none;" onchange="importData(event)">
        </label>
        <button onclick="resetAll()" style="font-size:11px; color:#dc3545; flex:1;">全削除</button>
    </div>
</div>

<h3 onclick="toggle('secBuy')">馬券購入</h3>
<div id="secBuy" class="section-content">
    <div class="card" style="background:#f8f9fa;">
        <div style="display:flex; gap:5px; margin-bottom:5px;">
            <input id="inDate" type="date" style="flex:1.5; min-width: 0;">
            <div style="flex:1; min-width: 0;">
                <select id="inPlaceSel" onchange="checkPlaceInput()">
                    <option value="札幌">札幌</option><option value="函館">函館</option>
                    <option value="福島">福島</option><option value="新潟">新潟</option>
                    <option value="中山">中山</option><option value="東京">東京</option>
                    <option value="中京">中京</option><option value="京都">京都</option>
                    <option value="阪神">阪神</option><option value="小倉">小倉</option>
                    <option value="その他">その他</option>
                </select>
                <input id="inPlaceMan" type="text" placeholder="場所名" style="display:none; margin-top:2px;">
            </div>
        </div>
        <div style="display:flex; gap:5px; margin-bottom:5px;">
            <input id="inR" type="number" placeholder="R" style="flex:0.5; min-width: 0;">
            <input id="inGrade" type="text" placeholder="G (G3)" style="flex:0.7; min-width: 0;">
            <input id="inName" type="text" placeholder="レース名" style="flex:1.8; min-width: 0;">
        </div>
        
        <select id="type" onchange="checkTypeLock(); setMode();" style="width:100%; font-size:16px; margin-bottom:10px;">
            <option>単勝</option><option>複勝</option>
            <option>枠連</option><option>枠単</option>
            <option>馬連</option><option>馬単</option>
            <option>ワイド</option>
            <option>三連複</option><option>三連単</option>
        </select>
        <div id="modeSelect" class="mode-container">
            <label class="mode-label"><input type="radio" name="mode" value="normal" checked onclick="setMode()">通常</label>
            <label class="mode-label"><input type="radio" name="mode" value="box" onclick="setMode()">BOX</label>
            <label class="mode-label"><input type="radio" name="mode" value="nagashi" onclick="setMode()">流し</label>
            <label class="mode-label"><input type="radio" name="mode" value="formation" onclick="setMode()">フォーメ</label>
        </div>
        <div id="tabs" class="form-tabs" style="display:none;">
            <div id="t1" class="tab active" onclick="swTab(1)">1</div>
            <div id="t2" class="tab" onclick="swTab(2)">2</div>
            <div id="t3" class="tab" onclick="swTab(3)">3</div>
        </div>
        
        <div id="grid" class="num-grid"></div>
        
        <div class="flex" style="margin-top:10px;">
            <input type="number" id="unitPrice" placeholder="1点あたり" style="flex:1;" oninput="updateEst()">
            <span style="padding:0 5px;">円</span>
        </div>

        <div id="estBox" class="calc-box">
            0点 <span style="color:#aaa;">x</span> 0円 = 0円
        </div>

        <button onclick="buy()" style="width:100%; height:45px; background:#28a745; color:white; font-size:16px; border:none;">購入確定</button>
    </div>
</div>

<h3 onclick="toggle('secRes')" class="closed">結果入力 (レース一括)</h3>
<div id="secRes" class="section-content hidden">
    <div class="card">
        <div class="autocomplete" style="margin-bottom:10px;">
            <input id="resRace" type="text" placeholder="対象レース名 (候補から選択)" style="width:100%;">
        </div>
        <div class="res-row"><div class="res-lbl">1着</div> 馬<input type="number" id="r1" class="res-inp"> 枠<input type="number" id="w1" class="res-inp"></div>
        <div class="res-row"><div class="res-lbl">2着</div> 馬<input type="number" id="r2" class="res-inp"> 枠<input type="number" id="w2" class="res-inp"></div>
        <div class="res-row"><div class="res-lbl">3着</div> 馬<input type="number" id="r3" class="res-inp"> 枠<input type="number" id="w3" class="res-inp"></div>
        <button onclick="judge()" style="width:100%; background:#343a40; color:white; margin-top:10px;">判定開始</button>
    </div>
</div>

<h3 onclick="toggle('secWait')">未確定 <span id="cntWait" style="font-size:0.8em; background:#dc3545; color:white; padding:2px 6px; border-radius:10px;">0</span></h3>
<div id="secWait" class="section-content">
    <button onclick="toggleFilter('filtWait')" style="width:100%; font-size:12px; margin-bottom:5px;">🔍 検索・ソート</button>
    <div id="filtWait" class="filter-box">
        <select id="fTypeWait" style="width:100%; margin-bottom:5px;" onchange="render()">
            <option value="all">全券種</option>
            <option>単勝</option><option>複勝</option>
            <option>枠連</option><option>枠単</option>
            <option>馬連</option><option>馬単</option>
            <option>ワイド</option>
            <option>三連複</option><option>三連単</option>
        </select>
        <div class="autocomplete" style="width:100%;">
            <input id="sWait" placeholder="レース名検索" oninput="render()">
        </div>
        <select id="sortWait" style="width:100%; margin-top:5px;" onchange="render()">
            <option value="time">日付が新しい順</option>
            <option value="timeAsc">日付が古い順</option>
            <option value="amtDesc">賭け金が高い順</option>
            <option value="amtAsc">賭け金が低い順</option>
        </select>
    </div>
    <div id="listWait"></div>
</div>

<h3 onclick="toggle('secDone')" class="closed">確定済み</h3>
<div id="secDone" class="section-content hidden">
    <button onclick="toggleFilter('filtDone')" style="width:100%; font-size:12px; margin-bottom:5px;">🔍 検索・ソート</button>
    <div id="filtDone" class="filter-box">
        <select id="fTypeDone" style="width:100%; margin-bottom:5px;" onchange="render()">
            <option value="all">全券種</option>
            <option>単勝</option><option>複勝</option>
            <option>枠連</option><option>枠単</option>
            <option>馬連</option><option>馬単</option>
            <option>ワイド</option>
            <option>三連複</option><option>三連単</option>
        </select>
        <div class="autocomplete" style="width:100%;">
            <input id="sDone" placeholder="レース名検索" oninput="render()">
        </div>
        <select id="sortDone" style="width:100%; margin-top:5px;" onchange="render()">
            <option value="time">日付が新しい順</option>
            <option value="timeAsc">日付が古い順</option>
            <option value="amtDesc">賭け金が高い順</option>
            <option value="amtAsc">賭け金が低い順</option>
            <option value="payDesc">払戻額が高い順</option>
            <option value="oddsDesc">回収率が良い順</option>
        </select>
    </div>
    <div id="listDone"></div>
</div>

<h3 onclick="toggle('secTx')" class="closed">取引履歴・収支</h3>
<div id="secTx" class="section-content hidden">
    <div class="total-profit-box">
        <div style="font-size:12px;">全期間トータル収支</div>
        <div id="totalProfit" class="tp-val">0円</div>
    </div>
    <button onclick="toggleFilter('filtTx')" style="width:100%; font-size:12px; margin-bottom:5px;">🔍 絞り込み・ソート</button>
    <div id="filtTx" class="filter-box">
        <div class="flex" style="margin-bottom:5px;">
            <button onclick="txFilterMode='all';render()" style="flex:1; font-size:11px;">全て</button>
            <button onclick="txFilterMode='dep';render()" style="flex:1; font-size:11px;">入金</button>
            <button onclick="txFilterMode='race';render()" style="flex:1; font-size:11px;">レース</button>
        </div>
        <div class="autocomplete" style="width:100%;">
            <input id="sTx" placeholder="レース名検索" oninput="render()">
        </div>
        <select id="sortTx" style="width:100%; margin-top:5px;" onchange="render()">
            <option value="time">日付が新しい順</option>
            <option value="timeAsc">日付が古い順</option>
            <option value="profDesc">収支が良い順</option>
            <option value="profAsc">収支が悪い順</option>
        </select>
    </div>
    <div id="listTx"></div>
</div>

<script>
let tickets = JSON.parse(localStorage.getItem("kb_tickets") || "[]");
let deposits = JSON.parse(localStorage.getItem("kb_deposits") || "[]");
let s1=[], s2=[], s3=[], cTab=1, txFilterMode='all';

const APP_NAME = "kibsim";

const $ = (id) => document.getElementById(id);
const norm = (s) => (s||"").replace(/[！-～]/g, m=>String.fromCharCode(m.charCodeAt(0)-0xFEE0)).replace(/\s+/g,"").toUpperCase();
const save = () => { localStorage.setItem("kb_tickets", JSON.stringify(tickets)); localStorage.setItem("kb_deposits", JSON.stringify(deposits)); };
const sum = (arr, k) => arr.reduce((a,b)=>a+b[k],0);

// Helper for combinations/permutations
const comb = (a,k) => { let r=[]; const f=(s,c)=>{if(c.length===k)r.push([...c]);else for(let i=s;i<a.length;i++){c.push(a[i]);f(i+1,c);c.pop()}};f(0,[]);return r; };
const perm = (a,k) => { let r=[]; const f=(c)=>{if(c.length===k)r.push([...c]);else for(let i=0;i<a.length;i++){if(!c.includes(a[i])){c.push(a[i]);f(c);c.pop()}}};f([]);return r; };

function toggle(id) { const e=$(id); e.classList.toggle("hidden"); document.querySelector(`h3[onclick="toggle('${id}')"]`).classList.toggle("closed"); }
function toggleFilter(id) { $(id).classList.toggle("show"); }

function checkTypeLock() {
    const t = $("type").value;
    const radios = document.getElementsByName("mode");
    if(t==="単勝" || t==="複勝") {
        radios.forEach(r => {
            if(r.value!=="normal") r.disabled = true;
            else r.checked = true;
        });
    } else {
        radios.forEach(r => r.disabled = false);
    }
    updateEst();
}

function setMode() {
    const mode = document.querySelector('input[name="mode"]:checked').value;
    const type = $("type").value;
    const tabs = $("tabs");
    const t1=$("t1"), t2=$("t2"), t3=$("t3");
    s1=[]; s2=[]; s3=[];
    let max = 1; let show = false;
    
    if (mode === "normal") {
        if(["単勝","複勝"].includes(type)) { show=false; }
        else { show=true; max = type.includes("三連") ? 3 : 2; t1.innerText="1着"; t2.innerText="2着"; t3.innerText="3着"; }
    } else if (mode === "box") { show=false; }
    else if (mode === "nagashi") { show=true; max=2; t1.innerText="軸"; t2.innerText="相手"; }
    else if (mode === "formation") { show=true; max = type.includes("三連") ? 3 : 2; t1.innerText="1頭目"; t2.innerText="2頭目"; t3.innerText="3頭目"; }
    
    tabs.style.display = show ? "flex" : "none";
    t3.style.display = max===3 ? "block" : "none";
    if(cTab > max) swTab(1);
    
    renderGrid();
    updateEst();
}

function swTab(n) { cTab=n; [1,2,3].forEach(i=>$("t"+i).classList.toggle("active", i===n)); renderGrid(); }

function renderGrid() {
    const g = $("grid"); g.innerHTML="";
    const type = $("type").value;
    const mode = document.querySelector('input[name="mode"]:checked').value;
    const maxN = type.includes("枠") ? 8 : 18;
    const tgt = (cTab===1)?s1 : (cTab===2)?s2 : s3;
    
    for(let i=1; i<=maxN; i++) {
        const b = document.createElement("div"); b.className="num-btn"; b.innerText=i;
        if(tgt.includes(i)) { b.classList.add("selected"); if(mode==="nagashi" && cTab===1) b.classList.add("jiku"); }
        b.onclick = () => {
            let arr = (cTab===1)?s1 : (cTab===2)?s2 : s3;
            if(mode==="normal") { if(arr.includes(i)) arr=[]; else arr=[i]; } 
            else { if(arr.includes(i)) arr=arr.filter(x=>x!==i); else arr.push(i); }
            if(cTab===1) s1=arr; else if(cTab===2) s2=arr; else s3=arr;
            renderGrid();
            updateEst();
        };
        g.appendChild(b);
    }
}

// Logic to calculate picks without buying
function getPicks(m_s1, m_s2, m_s3, m_type, m_mode) {
    let picks = [];
    if(m_mode==="normal") {
        if(["単勝","複勝"].includes(m_type)) m_s1.forEach(x=>picks.push([x]));
        else if(m_type.includes("三連")) { if(m_s1.length&&m_s2.length&&m_s3.length) picks.push([m_s1[0],m_s2[0],m_s3[0]]); }
        else { if(m_s1.length&&m_s2.length) picks.push([m_s1[0],m_s2[0]]); }
    } else if (m_mode==="box") {
        if(!m_s1.length) return [];
        const k = m_type.includes("三連") ? 3 : 2;
        if(["枠単","馬単","三連単"].includes(m_type)) picks = perm(m_s1, k); else picks = comb(m_s1, k);
    } else if (m_mode==="nagashi") {
        const k = m_type.includes("三連") ? 3 : 2; 
        if(m_s1.length && m_s2.length) comb(m_s2, k-m_s1.length).forEach(c => picks.push([...m_s1, ...c]));
    } else if (m_mode==="formation") {
        if(m_type.includes("三連")) { for(let a of m_s1) for(let b of m_s2) for(let c of m_s3) if(new Set([a,b,c]).size===3) picks.push([a,b,c]); }
        else { for(let a of m_s1) for(let b of m_s2) if(a!==b) picks.push([a,b]); }
    }
    return picks;
}

function updateEst() {
    const type=$("type").value;
    const mode=document.querySelector('input[name="mode"]:checked').value;
    const picks = getPicks(s1, s2, s3, type, mode);
    const cnt = picks.length;
    const u = Number($("unitPrice").value) || 0;
    $("estBox").innerHTML = `<span style="font-size:1.2em;">${cnt}</span>点 <span style="color:#aaa;">x</span> ${u}円 = <span style="font-size:1.2em; color:#d32f2f;">${(cnt*u).toLocaleString()}</span>円`;
}

window.onload = function() {
    const d = new Date();
    const iso = d.getFullYear() + '-' + String(d.getMonth()+1).padStart(2,'0') + '-' + String(d.getDate()).padStart(2,'0');
    if($("inDate")) $("inDate").value = iso;
    setupAutocomplete($("resRace"), () => getUniqueRaces('waiting'));
    setupAutocomplete($("sWait"), () => getUniqueRaces('waiting'));
    setupAutocomplete($("sDone"), () => getUniqueRaces('done'));
    setupAutocomplete($("sTx"), () => getUniqueRaces('all'));
    render();
    updateEst();
}

function checkPlaceInput() {
    const v = $("inPlaceSel").value;
    $("inPlaceMan").style.display = (v === "その他") ? "block" : "none";
}

function buy() {
    const dv=$("inDate").value; if(!dv) return alert("日付を選択してください");
    const dStr = dv.replace(/-/g,"");
    let p = $("inPlaceSel").value;
    if(p === "その他") { p = $("inPlaceMan").value; if(!p) return alert("レース場名を入力してください"); }
    const r=$("inR").value, g=$("inGrade").value, n=$("inName").value;
    if(!r) return alert("Rを入力してください");
    const race = `${dStr}${p}R${r}${g}${n}`;
    
    const amt=Number($("unitPrice").value), type=$("type").value, mode=document.querySelector('input[name="mode"]:checked').value;
    if(amt<=0) return alert("金額を入力してください");
    
    let picks = getPicks(s1, s2, s3, type, mode);
    if(!picks.length) return alert("買い目なし");
    
    const cost = picks.length * amt;
    if(bal() < cost) return alert("残高不足");
    
    if(["枠連","馬連","ワイド","三連複"].includes(type)) picks = picks.map(p=>p.sort((a,b)=>a-b));
    const rawGroups = [[...s1], [...s2], [...s3]];
    
    tickets.push({ id:Date.now(), time:Date.now(), race, type, mode, picks, groups:rawGroups, unit:amt, cost, pay:0, stat:"waiting" });
    alert(`購入完了: ${cost}円`);
    render();
}

function judge() {
    const race = norm($("resRace").value);
    const r=[$("r1").value,$("r2").value,$("r3").value].map(Number);
    const w=[$("w1").value,$("w2").value,$("w3").value].map(Number);
    if(!race || !r[0]) return alert("入力不足");
    
    const tgts = tickets.filter(t=>t.stat==="waiting" && norm(t.race)===race);
    if(!tgts.length) return alert("対象馬券なし");
    
    tgts.forEach(t => {
        const res = t.type.includes("枠") ? w : r; 
        const resStr = res.slice(0, t.type.includes("三連")?3:2).join('-');
        
        let hCnt = 0;
        t.picks.forEach(p => {
            let win=false;
            if(t.type==="単勝") win=(p[0]==res[0]);
            else if(t.type==="複勝") win=res.slice(0,3).includes(p[0]);
            else if(t.type==="ワイド") win=(p.filter(x=>res.slice(0,3).includes(x)).length===2);
            else if(["馬連","枠連"].includes(t.type)) win=(p.join()==[res[0],res[1]].sort((a,b)=>a-b).join());
            else if(["馬単","枠単"].includes(t.type)) win=(p[0]==res[0] && p[1]==res[1]);
            else if(t.type==="三連複") win=(p.join()==[res[0],res[1],res[2]].sort((a,b)=>a-b).join());
            else if(t.type==="三連単") win=(p[0]==res[0] && p[1]==res[1] && p[2]==res[2]);
            if(win) hCnt++;
        });
        
        if(hCnt>0) {
            const msg = `【的中】${t.race}\n券種: ${t.type} (${t.mode})\n確定結果: ${resStr}\n\n100円あたりの配当を入力:`;
            const odds = prompt(msg, "0");
            if(odds && Number(odds)>0) { 
                t.pay = Math.floor((Number(odds)/100) * t.unit * hCnt); 
                t.stat = "win"; 
            }
        } else { t.stat = "lose"; }
    });
    render();
}

function deposit() { const v=Number($("depAmt").value); if(v>0) { deposits.push({id:Date.now(), time:Date.now(), val:v}); $("depAmt").value=""; render(); } }
function bal() { return sum(deposits,"val") + tickets.reduce((a,b)=>a+(b.pay-b.cost),0); }
function resetAll() { if(confirm("全削除？")){ tickets=[]; deposits=[]; render(); } }
function getUniqueRaces(filterType) {
    let arr = (filterType==='waiting') ? tickets.filter(t=>t.stat==='waiting') : (filterType==='done' ? tickets.filter(t=>t.stat!=='waiting') : tickets);
    return [...new Set(arr.map(t => t.race))];
}

function setupAutocomplete(inp, arrFunc) {
    let currentFocus;
    inp.addEventListener("input", function(e) {
        let val = this.value; closeAllLists(); if (!val) return false; currentFocus = -1;
        let a = document.createElement("DIV"); a.setAttribute("class", "autocomplete-items"); this.parentNode.appendChild(a);
        let arr = arrFunc();
        for (let i = 0; i < arr.length; i++) {
            if (arr[i].toUpperCase().indexOf(val.toUpperCase()) > -1) {
                let b = document.createElement("DIV"); b.innerHTML = arr[i];
                b.addEventListener("click", function(e) { inp.value = arr[i]; closeAllLists(); if(inp.id.startsWith("s") || inp.id==="resRace") render(); });
                a.appendChild(b);
            }
        }
    });
    inp.addEventListener("keydown", function(e) {
        let x = this.parentNode.querySelector(".autocomplete-items"); if (x) x = x.getElementsByTagName("div");
        if (e.keyCode == 40) { currentFocus++; addActive(x); } else if (e.keyCode == 38) { currentFocus--; addActive(x); }
        else if (e.keyCode == 13) { e.preventDefault(); if (currentFocus > -1 && x) x[currentFocus].click(); }
    });
    function addActive(x) { if (!x) return false; removeActive(x); if (currentFocus >= x.length) currentFocus = 0; if (currentFocus < 0) currentFocus = (x.length - 1); x[currentFocus].classList.add("autocomplete-active"); }
    function removeActive(x) { for (let i = 0; i < x.length; i++) x[i].classList.remove("autocomplete-active"); }
    function closeAllLists(elmnt) { let x = document.getElementsByClassName("autocomplete-items"); for (let i = 0; i < x.length; i++) if (elmnt != x[i] && elmnt != inp) x[i].parentNode.removeChild(x[i]); }
    document.addEventListener("click", function (e) { closeAllLists(e.target); });
}

function render() {
    save(); $("balance").innerText = bal().toLocaleString(); $("cntWait").innerText = tickets.filter(t=>t.stat==="waiting").length;
    const totalP = tickets.reduce((a,b)=>a+(b.pay-b.cost), 0); const totalDiv = $("totalProfit");
    totalDiv.innerText = (totalP>=0 ? "+":"") + totalP.toLocaleString() + "円"; totalDiv.className = "tp-val " + (totalP>=0 ? "plus":"minus");
    
    const sw = norm($("sWait").value), soW = $("sortWait").value, fW = $("fTypeWait").value;
    const lW = $("listWait"); lW.innerHTML="";
    tickets.filter(t=>t.stat==="waiting" && norm(t.race).includes(sw) && (fW==="all"||t.type===fW))
           .sort((a,b)=> (soW==="time"?b.time-a.time:(soW==="timeAsc"?a.time-b.time:(soW==="amtDesc"?b.cost-a.cost:a.cost-b.cost))))
           .forEach(t => lW.innerHTML += card(t, true));
    
    const sd = norm($("sDone").value), soD = $("sortDone").value, fD = $("fTypeDone").value;
    const lD = $("listDone"); lD.innerHTML="";
    tickets.filter(t=>t.stat!=="waiting" && norm(t.race).includes(sd) && (fD==="all"||t.type===fD))
           .sort((a,b)=> (soD==="time"?b.time-a.time:(soD==="timeAsc"?a.time-b.time:(soD==="payDesc"?b.pay-a.pay:(b.pay/b.cost)-(a.pay/a.cost)))))
           .forEach(t => lD.innerHTML += card(t, false));
    
    const st = norm($("sTx").value), soT = $("sortTx").value;
    const lT = $("listTx"); lT.innerHTML=""; let logs = [];
    if(txFilterMode!=="race") deposits.forEach(d => logs.push({ type:"dep", time:d.time, profit:d.val, html:`<div class="card status-dep"><div class="flex"><b>入金</b><span class="plus">+${d.val.toLocaleString()}</span></div><small>${new Date(d.time).toLocaleString()}</small></div>` }));
    if(txFilterMode!=="dep") {
        const rMap={}; tickets.filter(t=>t.stat!=="waiting").forEach(t=>{ if(!rMap[t.race]) rMap[t.race]={buy:0, pay:0, time:t.time}; rMap[t.race].buy+=t.cost; rMap[t.race].pay+=t.pay; if(t.time>rMap[t.race].time) rMap[t.race].time=t.time; });
        Object.keys(rMap).forEach(r=>{ 
            if(norm(r).includes(st)) { 
                const d=rMap[r], prof=d.pay-d.buy; 
                logs.push({ 
                    type:"race", 
                    time:d.time, 
                    profit:prof, 
                    html:`<div class="card status-${prof>=0?'win':'lose'}">
                            <div class="flex">
                                <b>${r}</b>
                                <span class="${prof>=0?'plus':'minus'}">${prof>=0?'+':''}${prof.toLocaleString()}円</span>
                            </div>
                            <div style="font-size:12px; margin-top:4px; color:#555;">
                                投資: ${d.buy.toLocaleString()}円 / 払戻: ${d.pay.toLocaleString()}円
                            </div>
                            <div style="font-size:11px; color:#888; margin-top:2px;">
                                (収支: ${prof.toLocaleString()}円)
                            </div>
                            <small style="display:block; margin-top:5px; border-top:1px solid #eee; padding-top:3px;">${new Date(d.time).toLocaleString()}</small>
                          </div>` 
                }); 
            } 
        });
    }
    logs.sort((a,b)=>(soT==="time"?b.time-a.time:(soT==="timeAsc"?a.time-b.time:b.profit-a.profit))).forEach(l => lT.innerHTML += l.html);
}

function card(t, del) {
    let picksStr = (t.mode === "normal" || !t.groups) ? t.picks.map(p => `[${p.join('-')}]`).join(', ') : `/${t.groups.filter(g=>g.length).map(g=>g.join('.')).join(' - ')}/`;
    return `<div class="card status-${t.stat}">
        ${del ? `<button onclick="tickets=tickets.filter(x=>x.id!==${t.id});render()" style="position:absolute;top:5px;right:5px;border:none;background:none;color:#999;font-size:18px;">×</button>` : ''}
        <div class="flex"><b>${t.race}</b> <span class="tag">${t.type}</span></div>
        <div style="font-size:13px; margin:5px 0;">${t.cost.toLocaleString()}円 ${t.stat!=="waiting"?`/ 払戻: <b>${t.pay.toLocaleString()}</b>`:''}</div>
        <div class="picks-display">${t.mode}: ${picksStr}</div>
        <div style="font-size:11px; color:#888; margin-top:5px;">${new Date(t.time).toLocaleString()}</div>
    </div>`;
}

// 変更箇所：日付と時間を入れたファイル名でダウンロード
function exportData(){
    const d = new Date();
    // YYYYMMDD_HHMM形式を作成
    const str = d.getFullYear() + 
                String(d.getMonth() + 1).padStart(2, '0') + 
                String(d.getDate()).padStart(2, '0') + "_" +
                String(d.getHours()).padStart(2, '0') + 
                String(d.getMinutes()).padStart(2, '0');
    
    const filename = `kibsim_${str}.json`;
    
    const blob = new Blob([JSON.stringify({tickets,deposits})],{type:'application/json'});
    const a=document.createElement('a'); 
    a.href=URL.createObjectURL(blob); 
    a.download=filename; 
    a.click(); 
}

function importData(e){ const r=new FileReader(); r.onload=v=>{try{const d=JSON.parse(v.target.result);tickets=d.tickets;deposits=d.deposits;render();alert("完了");}catch{alert("エラー");}}; if(e.target.files[0])r.readAsText(e.target.files[0]); }

checkTypeLock(); setMode(); render();
</script>
</body>
</html>
