[Məktəb tapşırıqlar paneli.html](https://github.com/user-attachments/files/35481459/M.kt.b.tapsiriqlar.paneli.html)
<!DOCTYPE html>
<html lang="az">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Məktəb tapşırıqlar paneli</title>
<style>
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px);
--bg:#f6f7f9;--card:#fff;--tx:#1b2430;--mu:#6a7686;--bd:#dde2e9;--ac:#1f5fbf;--ok:#1f8a4c;--wa:#b7791f;--er:#c0392b}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){color-scheme:dark;--bg:#14181e;--card:#1c222b;--tx:#e6eaf0;--mu:#93a0b2;--bd:#2d3643;--ac:#6ea3f0;--ok:#4cc582;--wa:#e0aa4a;--er:#ef7a6c}}
:root[data-theme="dark"]{color-scheme:dark;--bg:#14181e;--card:#1c222b;--tx:#e6eaf0;--mu:#93a0b2;--bd:#2d3643;--ac:#6ea3f0;--ok:#4cc582;--wa:#e0aa4a;--er:#ef7a6c}
*{box-sizing:border-box}html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;background:var(--bg);color:var(--tx);font:15px/1.45 system-ui,-apple-system,"Segoe UI",Roboto,sans-serif}
.w{max-width:1000px;margin:0 auto;padding:16px}
h1{font-size:20px;margin:0 0 12px}
.row{display:flex;flex-wrap:wrap;gap:8px}
.stat{flex:1 1 110px;background:var(--card);border:1px solid var(--bd);border-radius:10px;padding:10px 12px}
.stat b{display:block;font-size:22px}.stat span{color:var(--mu);font-size:12px}
.bar{margin:14px 0;display:flex;flex-wrap:wrap;gap:8px;align-items:center}
select,input,textarea,button{font:inherit;color:var(--tx);background:var(--card);border:1px solid var(--bd);border-radius:8px;padding:8px 10px}
button{cursor:pointer}button.p{background:var(--ac);border-color:var(--ac);color:#fff}
.roles{display:grid;grid-template-columns:repeat(auto-fill,minmax(210px,1fr));gap:8px;margin-bottom:14px}
.rc{background:var(--card);border:1px solid var(--bd);border-radius:10px;padding:8px 10px;font-size:13px;cursor:pointer}
.rc.on{border-color:var(--ac)}.rc small{color:var(--mu);display:block}
.pg{height:5px;background:var(--bd);border-radius:3px;margin-top:5px}.pg i{display:block;height:5px;background:var(--ok);border-radius:3px}
.t{background:var(--card);border:1px solid var(--bd);border-radius:10px;padding:12px;margin-bottom:8px;border-left:4px solid var(--mu)}
.t.done{border-left-color:var(--ok)}.t.late{border-left-color:var(--er)}.t.prog{border-left-color:var(--wa)}.t.fail{border-left-color:var(--er)}
.t h3{margin:0 0 4px;font-size:15px}.m{color:var(--mu);font-size:12.5px}
.tag{display:inline-block;padding:1px 8px;border-radius:99px;border:1px solid var(--bd);font-size:12px;margin-right:4px}
.ex{margin-top:8px;padding:8px;border:1px dashed var(--bd);border-radius:8px;font-size:13.5px}
.act{margin-top:8px;display:flex;gap:6px;flex-wrap:wrap}.act button{padding:4px 10px;font-size:13px}
dialog{border:1px solid var(--bd);border-radius:12px;background:var(--card);color:var(--tx);width:min(480px,94vw);padding:16px}
dialog::backdrop{background:#0008}
label{display:block;font-size:13px;color:var(--mu);margin:10px 0 3px}
dialog input,dialog select,dialog textarea{width:100%}
.ft{display:flex;gap:8px;justify-content:flex-end;margin-top:14px}
</style>
</head>
<body>
<div class="w">
<h1>Məktəb idarəetməsi · Tapşırıqlar paneli</h1>
<div class="row" id="stats"></div>
<div class="bar">
<button class="p" onclick="openNew()">+ Yeni tapşırıq</button>
<select id="fs" onchange="render()"><option value="">Bütün statuslar</option><option value="gozleyir">Gözləyir</option><option value="icrada">İcrada</option><option value="icra">İcra olunub</option><option value="icraolunmayib">İcra olunmayıb</option><option value="gecikib">Gecikib</option></select>
<button onclick="exportCsv()">CSV yüklə</button>
<span class="m" id="fr"></span>
</div>
<div class="roles" id="roles"></div>
<div id="list"></div>
</div>

<dialog id="dn"><form method="dialog" onsubmit="return false">
<b>Tapşırıq</b>
<label>Məsul şəxs (vəzifə)</label><select id="nr"></select>
<label>Tapşırıq / tədbir</label><input id="nt">
<label>Əlavə qeyd</label><textarea id="nd" rows="2"></textarea>
<label>Son icra tarixi</label><input type="date" id="nv">
<label>Prioritet</label><select id="np"><option>Adi</option><option>Vacib</option><option>Təcili</option></select>
<div class="ft"><button onclick="dn.close()">Ləğv et</button><button class="p" onclick="saveNew()">Yadda saxla</button></div>
</form></dialog>

<dialog id="de"><form method="dialog" onsubmit="return false">
<b id="et"></b>
<label>İcra statusu</label><select id="es"><option value="icrada">İcrada</option><option value="icra">İcra olunub</option><option value="icraolunmayib">İcra olunmayıb</option></select>
<label>İcra tarixi</label><input type="date" id="ed">
<label>İcranın əsası</label><select id="eb"><option>Təsərrüfat fəaliyyəti əsasında</option><option>İllik iş planı əsasında</option><option>Direktorun əmri / göstərişi əsasında</option><option>Təhsil şöbəsinin tapşırığı əsasında</option><option>Digər</option></select>
<label>Səbəb / icranın təsviri</label><textarea id="ex" rows="3" placeholder="Nə edildi? İcra olunmayıbsa, səbəbi göstərin."></textarea>
<div class="ft"><button onclick="de.close()">Ləğv et</button><button class="p" onclick="saveExec()">Yadda saxla</button></div>
</form></dialog>

<script>
var ROLES=["UBTR","Təlim-tərbiyə işləri üzrə direktor","Təşkilatçı","Təsərrüfat müdiri","Psixoloq","Kitabxana müdiri","Əməliyyat rəhbəri"];
var ST={gozleyir:"Gözləyir",icrada:"İcrada",icra:"İcra olunub",icraolunmayib:"İcra olunmayıb"};
var K="mektebTapshiriqlar",tasks=[],fr="",cur=null;
try{tasks=JSON.parse(localStorage.getItem(K)||"[]")}catch(e){tasks=[]}
function save(){try{localStorage.setItem(K,JSON.stringify(tasks))}catch(e){}}
function $(i){return document.getElementById(i)}
function esc(s){return String(s==null?"":s).replace(/[&<>"]/g,function(c){return{"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]})}
function today(){return new Date().toISOString().slice(0,10)}
function late(t){return (t.s==="gozleyir"||t.s==="icrada")&&t.due&&t.due<today()}
function eff(t){return late(t)?"gecikib":t.s}
function fmt(d){if(!d)return"—";var p=d.split("-");return p[2]+"."+p[1]+"."+p[0]}
$("nr").innerHTML=ROLES.map(function(r){return"<option>"+esc(r)+"</option>"}).join("");
function openNew(){$("nt").value="";$("nd").value="";$("nv").value="";$("np").value="Adi";if(fr)$("nr").value=fr;$("dn").showModal()}
function saveNew(){var t=$("nt").value.trim();if(!t){$("nt").focus();return}
tasks.unshift({id:Date.now(),role:$("nr").value,title:t,desc:$("nd").value.trim(),due:$("nv").value,pr:$("np").value,s:"gozleyir",created:today()});
save();$("dn").close();render()}
function openExec(id){cur=tasks.filter(function(t){return t.id===id})[0];if(!cur)return;
$("et").textContent=cur.title;$("es").value=cur.s==="gozleyir"?"icra":cur.s;$("ed").value=cur.doneDate||today();
$("eb").value=cur.basis||"Təsərrüfat fəaliyyəti əsasında";$("ex").value=cur.reason||"";$("de").showModal()}
function saveExec(){if(!cur)return;
if($("es").value==="icraolunmayib"&&!$("ex").value.trim()){$("ex").focus();return}
cur.s=$("es").value;cur.doneDate=$("ed").value;cur.basis=$("eb").value;cur.reason=$("ex").value.trim();
save();$("de").close();render()}
function del(id){if(confirm("Bu tapşırıq silinsin?")){tasks=tasks.filter(function(t){return t.id!==id});save();render()}}
function setRole(r){fr=fr===r?"":r;render()}
function render(){var fs=$("fs").value;
var c={all:tasks.length,icra:0,icrada:0,gozleyir:0,gecikib:0,icraolunmayib:0};
tasks.forEach(function(t){var e=eff(t);if(c[e]!==undefined)c[e]++});
$("stats").innerHTML=[["Cəmi",c.all],["İcra olunub",c.icra],["İcrada",c.icrada],["Gözləyir",c.gozleyir],["Gecikib",c.gecikib],["İcra olunmayıb",c.icraolunmayib]].map(function(x){return'<div class="stat"><b>'+x[1]+'</b><span>'+x[0]+'</span></div>'}).join("");
$("roles").innerHTML=ROLES.map(function(r){var a=tasks.filter(function(t){return t.role===r}),d=a.filter(function(t){return t.s==="icra"}).length,p=a.length?Math.round(d*100/a.length):0;
return'<div class="rc'+(fr===r?" on":"")+'" onclick="setRole(ROLES['+ROLES.indexOf(r)+'])"><b>'+esc(r)+'</b><small>'+d+" / "+a.length+' icra olunub</small><div class="pg"><i style="width:'+p+'%"></i></div></div>'}).join("");
$("fr").textContent=fr?"Filtr: "+fr+" (ləğv etmək üçün kartı yenidən seçin)":"";
var L=tasks.filter(function(t){return(!fr||t.role===fr)&&(!fs||eff(t)===fs)});
$("list").innerHTML=L.length?L.map(function(t){var e=eff(t),cl=e==="icra"?"done":e==="gecikib"?"late":e==="icrada"?"prog":e==="icraolunmayib"?"fail":"";
var ex=(t.reason||t.basis&&t.s!=="gozleyir")?'<div class="ex"><b>İcranın əsası:</b> '+esc(t.basis||"—")+(t.doneDate?' · <b>Tarix:</b> '+fmt(t.doneDate):"")+(t.reason?'<br><b>Səbəb / təsvir:</b> '+esc(t.reason):"")+'</div>':"";
return'<div class="t '+cl+'"><h3>'+esc(t.title)+'</h3><div class="m"><span class="tag">'+esc(t.role)+'</span><span class="tag">'+(e==="gecikib"?"Gecikib":ST[t.s])+'</span><span class="tag">'+esc(t.pr)+'</span>Son tarix: '+fmt(t.due)+'</div>'+(t.desc?'<div class="m" style="margin-top:4px">'+esc(t.desc)+'</div>':"")+ex+
'<div class="act"><button onclick="openExec('+t.id+')">İcranı qeyd et</button><button onclick="del('+t.id+')">Sil</button></div></div>'}).join(""):'<p class="m">Tapşırıq yoxdur. «Yeni tapşırıq» düyməsi ilə əlavə edin.</p>'}
function exportCsv(){var h=["Vəzifə","Tapşırıq","Qeyd","Son tarix","Prioritet","Status","İcra tarixi","İcranın əsası","Səbəb/təsvir"];
var r=tasks.map(function(t){return[t.role,t.title,t.desc,t.due,t.pr,ST[t.s],t.doneDate,t.basis,t.reason].map(function(v){return'"'+String(v||"").replace(/"/g,'""')+'"'}).join(",")});
var b=new Blob(["\ufeff"+[h.join(",")].concat(r).join("\n")],{type:"text/csv"});var a=document.createElement("a");a.href=URL.createObjectURL(b);a.download="tapshiriqlar.csv";a.click()}
render();
</script>
</body>
</html>
