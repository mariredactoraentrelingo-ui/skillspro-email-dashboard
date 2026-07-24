<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard Agendamientos · SkillsPRO</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.2.0/dist/chartjs-plugin-datalabels.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&family=DM+Sans:wght@400;500;700&display=swap');
:root{
  --petroleo:#0c465b;
  --teal:#10728d;
  --cyan:#0fb5c9;
  --coral:#ff6b5b;
  --amber:#ffb020;
  --violet:#7c5cff;
  --lime:#3ecf8e;
  --white:#ffffff;
  --gray-text:#3f545e;
  --border:#e2ebef;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:'DM Sans',sans-serif;background:#eef3f6;color:#132c37;font-size:14px;min-height:100vh;-webkit-print-color-adjust:exact;print-color-adjust:exact;}

.header{background:linear-gradient(120deg,#0c465b 0%,#10728d 55%,#0fb5c9 100%);padding:20px 28px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:10px;}
.header-left h1{font-family:'Plus Jakarta Sans',sans-serif;font-size:19px;font-weight:800;color:#ffffff;letter-spacing:-0.3px;}
.header-left p{font-size:12px;color:rgba(255,255,255,0.75);margin-top:3px;}
.header-right{display:flex;align-items:center;gap:10px;}
.badge-cache{background:var(--amber);color:#5c3d00;font-size:11px;font-weight:700;padding:5px 12px;border-radius:20px;}
.btn-update{background:#ffffff;color:var(--petroleo);border:none;padding:8px 18px;border-radius:8px;font-family:'DM Sans',sans-serif;font-size:12px;font-weight:700;cursor:pointer;transition:transform .15s,box-shadow .15s;box-shadow:0 2px 8px rgba(0,0,0,.15);}
.btn-update:hover{transform:translateY(-1px);box-shadow:0 4px 12px rgba(0,0,0,.2);}

.tab-bar{background:#0a3b4d;padding:11px 28px;display:flex;align-items:center;gap:8px;flex-wrap:wrap;}
.tab-bar label{font-size:11px;color:rgba(255,255,255,0.5);font-weight:700;letter-spacing:.6px;text-transform:uppercase;margin-right:4px;}
.chip{padding:6px 16px;border-radius:20px;border:none;background:rgba(255,255,255,0.12);color:rgba(255,255,255,0.75);font-size:12px;font-weight:700;cursor:pointer;font-family:'DM Sans',sans-serif;transition:all .15s;}
.chip:hover{background:rgba(255,255,255,0.22);}
.chip.active{background:var(--cyan);color:#04323c;}

.page{max-width:1220px;margin:0 auto;padding:22px 24px 44px;}
.section-label{font-size:11px;font-weight:700;color:#6f8b98;letter-spacing:.9px;text-transform:uppercase;margin:28px 0 12px;display:flex;align-items:center;gap:10px;}
.section-label:first-of-type{margin-top:14px;}
.section-label::after{content:'';flex:1;height:1px;background:linear-gradient(90deg,#d3e2e8,transparent);}

.kpi-grid{display:grid;grid-template-columns:repeat(5,1fr);gap:12px;}
@media(max-width:980px){.kpi-grid{grid-template-columns:repeat(2,1fr);}}
.kcard{background:var(--white);border-radius:14px;padding:18px;border:1px solid var(--border);position:relative;overflow:hidden;transition:transform .15s,box-shadow .15s;}
.kcard:hover{transform:translateY(-2px);box-shadow:0 8px 20px rgba(12,70,91,.10);}
.kcard::before{content:'';position:absolute;top:0;left:0;right:0;height:4px;}
.kcard.k1::before{background:linear-gradient(90deg,#0c465b,#10728d);}
.kcard.k2::before{background:linear-gradient(90deg,#0fb5c9,#4fe0d5);}
.kcard.k3::before{background:linear-gradient(90deg,#ffb020,#ffd166);}
.kcard.k4::before{background:linear-gradient(90deg,#3ecf8e,#8de3b8);}
.kcard.k5::before{background:linear-gradient(90deg,#7c5cff,#b39dff);}
.klabel{font-size:10px;font-weight:700;color:#84a0ac;text-transform:uppercase;letter-spacing:.5px;margin-bottom:8px;}
.kval{font-family:'Plus Jakarta Sans',sans-serif;font-size:30px;font-weight:800;line-height:1;}
.k1 .kval{color:#0c465b;} .k2 .kval{color:#0a94a6;} .k3 .kval{color:#c47f00;} .k4 .kval{color:#22a46a;} .k5 .kval{color:#5f42d6;}
.ksub{font-size:11px;color:#93a9b2;margin-top:7px;}

.charts-row{display:grid;grid-template-columns:1.25fr 1fr;gap:14px;}
@media(max-width:980px){.charts-row{grid-template-columns:1fr;}}
.charts-row-2{display:grid;grid-template-columns:1fr 1fr;gap:14px;}
@media(max-width:980px){.charts-row-2{grid-template-columns:1fr;}}
.ccard{background:var(--white);border-radius:14px;padding:20px;border:1px solid var(--border);}
.ctitle{font-family:'Plus Jakarta Sans',sans-serif;font-size:14px;font-weight:800;color:var(--petroleo);margin-bottom:3px;}
.csub{font-size:11px;color:#93a9b2;margin-bottom:16px;}
.cwrap{position:relative;}

.table-card{background:var(--white);border-radius:14px;border:1px solid var(--border);overflow:hidden;}
.table-scroll{overflow-x:auto;}
table{width:100%;border-collapse:collapse;font-size:12px;}
thead th{background:var(--petroleo);color:var(--white);padding:12px 12px;text-align:left;font-family:'Plus Jakarta Sans',sans-serif;font-weight:700;font-size:11px;white-space:nowrap;}
thead th.hi{background:linear-gradient(135deg,#0fb5c9,#0a94a6);color:#ffffff;}
thead th.hi::after{content:' ★';font-size:10px;}
tbody tr{transition:background .12s;}
tbody tr:nth-child(even){background:#f9fcfd;}
tbody tr:hover{background:#e9f7fa;}
tbody td{padding:11px 12px;text-align:left;color:var(--gray-text);border-bottom:1px solid #eef3f5;white-space:nowrap;vertical-align:middle;}
tbody td.wrap{white-space:normal;line-height:1.45;max-width:200px;}
tbody td.hi-cell{background:#e6f8fb;border-left:3px solid var(--cyan);border-right:3px solid var(--cyan);}
tbody tr:hover td.hi-cell{background:#d3f2f7;}
tbody td.muted{color:#a9bcc4;}

.campaign-tag{display:inline-block;background:linear-gradient(135deg,#0fb5c9,#0a94a6);color:#ffffff;font-size:11px;font-weight:700;padding:4px 12px;border-radius:20px;white-space:nowrap;}
.campaign-tag.oraculo{background:linear-gradient(135deg,#7c5cff,#5f42d6);}

.pill{display:inline-block;font-size:10px;font-weight:700;padding:4px 11px;border-radius:20px;white-space:nowrap;}
.p-invitado{background:#dcecff;color:#1b5fa8;}
.p-programada{background:#fff0cd;color:#a06a00;}
.p-reprogramada{background:#ffe0d9;color:#c14a35;}
.p-ejecutada{background:#d6f5e6;color:#17784f;}
.p-closed{background:#ede8ff;color:#5f42d6;}
.p-converted{background:linear-gradient(135deg,#3ecf8e,#22a46a);color:#ffffff;}
.p-message{background:#e3f1f5;color:#357788;}
.p-none{background:#f1f4f6;color:#a3b2b9;}
.p-presu{background:linear-gradient(135deg,#22a46a,#17784f);color:#ffffff;}
.p-podria{background:#dff5e7;color:#2e7d55;}

.note-card{background:var(--white);border-radius:14px;border:1px solid var(--border);border-left:5px solid var(--coral);padding:24px 26px;}
.note-title{font-family:'Plus Jakarta Sans',sans-serif;font-size:16px;font-weight:800;color:var(--petroleo);margin-bottom:7px;}
.note-intro{font-size:13px;color:#4c6470;line-height:1.75;margin-bottom:18px;}
.note-sub{font-family:'Plus Jakarta Sans',sans-serif;font-size:12px;font-weight:800;color:var(--petroleo);margin:20px 0 10px;text-transform:uppercase;letter-spacing:.5px;}
.note-card ul{padding-left:20px;}
.note-card li{font-size:13px;color:#4c6470;line-height:1.75;margin-bottom:8px;}
.note-card li::marker{color:var(--coral);}
.note-card li strong{color:var(--petroleo);}
.note-card ul.sol li::marker{color:var(--lime);}
.metric-tag{display:inline-block;background:linear-gradient(135deg,#e6f8fb,#f2fbfc);border:1px solid #b9e6ee;color:#076b7c;font-size:11px;font-weight:700;padding:4px 11px;border-radius:20px;margin:0 4px 6px 0;}

.legend-inline{display:flex;flex-wrap:wrap;gap:16px;margin-top:14px;}
.legend-item{display:flex;align-items:center;gap:7px;font-size:11px;color:#657f8a;font-weight:500;}
.legend-dot{width:11px;height:11px;border-radius:4px;}

.footer-note{font-size:11px;color:#a3b6bd;text-align:center;margin-top:32px;}

@media print{body{background:#ffffff;}.btn-update{display:none;}.table-scroll{overflow:visible;}}
@media(max-width:600px){.kpi-grid{grid-template-columns:1fr;}.page{padding:16px 14px 30px;}}
</style>
</head>
<body>

<div class="header">
  <div class="header-left">
    <h1>Dashboard Agendamientos · SkillsPRO</h1>
    <p>Seguimiento de consultorías</p>
  </div>
</div>

</body>
</html>
