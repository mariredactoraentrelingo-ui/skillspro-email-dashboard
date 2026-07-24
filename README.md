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

<div class="header">
  <div class="header-left">
    <h1>Dashboard Agendamientos · SkillsPRO</h1>
    <p>Seguimiento de consultorías desde newsletter · Clientify + Airtable</p>
  </div>
  <div class="header-right">
    <span class="badge-cache">Datos manuales</span>
    <span id="updatedTs" style="font-size:11px;color:rgba(255,255,255,.7)"></span>
    <button class="btn-update" onclick="location.reload(true)">Actualizar</button>
  </div>
</div>
<div class="tab-bar">
  <label>Mes:</label>
  <button class="chip active" id="chip-julio">Julio 2026</button>
</div>
<div class="page">
  <div class="section-label">Resumen del mes</div>
  <div class="kpi-grid">
    <div class="kcard k1"><div class="klabel">Agendamientos totales</div><div class="kval">8</div><div class="ksub">Registros en Airtable</div></div>
    <div class="kcard k2"><div class="klabel">Consultorías ejecutadas</div><div class="kval">3</div><div class="ksub">37.5% del total</div></div>
    <div class="kcard k3"><div class="klabel">Programadas pendientes</div><div class="kval">3</div><div class="ksub">Agenda hasta 31 jul</div></div>
    <div class="kcard k4"><div class="klabel">Ventas cerradas</div><div class="kval">1</div><div class="ksub">Meet Your Experts</div></div>
    <div class="kcard k5"><div class="klabel">Tasa de conversión</div><div class="kval">12.5%</div><div class="ksub">1 venta / 8 agendamientos</div></div>
  </div>
  <div class="section-label">Estado de la agenda y conversión</div>
  <div class="charts-row">
    <div class="ccard">
      <div class="ctitle">Agenda newsletter en este mes</div>
      <div class="csub">Distribución por estado de consultoría</div>
      <div class="cwrap" style="height:275px"><canvas id="cEstado"></canvas></div>
    </div>
    <div class="ccard">
      <div class="ctitle">Embudo de conversión</div>
      <div class="csub">De agendamiento a venta cerrada</div>
      <div class="cwrap" style="height:275px"><canvas id="cVentas"></canvas></div>
      <div class="legend-inline">
        <div class="legend-item"><span class="legend-dot" style="background:#0c465b"></span> 8 agendados</div>
        <div class="legend-item"><span class="legend-dot" style="background:#0fb5c9"></span> 3 ejecutadas</div>
        <div class="legend-item"><span class="legend-dot" style="background:#3ecf8e"></span> 1 venta</div>
      </div>
    </div>
  </div>
  <div class="section-label">Origen y perfil de los agendamientos</div>
  <div class="charts-row-2">
    <div class="ccard">
      <div class="ctitle">Agendamientos por utm_campaign</div>
      <div class="csub">Qué campaña trajo cada consultoría</div>
      <div class="cwrap" style="height:250px"><canvas id="cCampaign"></canvas></div>
    </div>
    <div class="ccard">
      <div class="ctitle">Capacidad de inversión declarada</div>
      <div class="csub">Situación actual reportada en el formulario</div>
      <div class="cwrap" style="height:250px"><canvas id="cSituacion"></canvas></div>
    </div>
  </div>
  <div class="section-label">Detalle de agendamientos · utm_campaign destacada</div>
  <div class="table-card">
    <div class="table-scroll">
      <table>
        <thead>
          <tr>
            <th>Correo electrónico</th>
            <th>WhatsApp</th>
            <th>Nombre completo</th>
            <th>Fecha creación</th>
            <th>Fecha consultoría</th>
            <th>2ª fecha consultoría</th>
            <th>Estado consultoría</th>
            <th>Status sales</th>
            <th>Área / Campo</th>
            <th>Profesión</th>
            <th>Experiencia laboral</th>
            <th>Situación actual</th>
            <th>utm_source</th>
            <th>utm_medium</th>
            <th class="hi">utm_campaign</th>
            <th>utm_term</th>
            <th>utm_content</th>
          </tr>
        </thead>
        <tbody id="tbodyAgenda"></tbody>
      </table>
    </div>
  </div>
  <div class="section-label">Problemáticas a resolver</div>
  <div class="note-card">
    <div class="note-title">Limitaciones actuales de medición</div>
    <div class="note-intro">
      El dashboard de Entrelingo corre sobre WordPress con FunnelKit, lo que permite leer el comportamiento completo del correo y atribuir ingresos campaña por campaña. SkillsPRO todavía no tiene esa infraestructura, y Clientify no entrega esas métricas. Por eso este panel se limita a lo que sí podemos registrar de forma confiable: agendamientos, estado de consultoría, perfil del contacto y cierre de venta.
    </div>
    <ul>
      <li><strong>No tenemos aún página de WordPress</strong> que nos ayudaría a tener métricas exactas de open rate, CTR y demás datos de comportamiento del correo. Clientify no las entrega.</li>
      <li><strong>El revenue no se conecta con la campaña.</strong> Sabemos que hubo una venta, pero no podemos decir con datos cuánto revenue aportó cada campaña ni cuál fue el ticket real.</li>
      <li><strong>La campaña "pagan_australia" concentra 3 de 8 agendamientos</strong> pero no podemos saber si rindió más porque el contenido funcionó mejor o simplemente porque se envió a más contactos. Sin el total de enviados por campaña, el dato no es comparable.</li>
      <li><strong>utm_term y utm_content llegan sin poblar.</strong> Los ocho registros muestran el placeholder literal, así que perdemos la capacidad de distinguir qué enlace o qué versión del correo generó el clic.</li>
      <li><strong>Carga manual de datos.</strong> El panel se alimenta desde Airtable a mano, así que hay riesgo de desfase entre lo que muestra el dashboard y lo que ocurre en la agenda real.</li>
      <li><strong>Sin histórico comparable.</strong> Julio es el primer mes con registro estructurado, así que todavía no hay base para comparar rendimiento mes contra mes.</li>
    </ul>
    <div class="note-sub">Métricas que sí tenemos en el dashboard de Entrelingo y aquí no</div>
    <div>
      <span class="metric-tag">Open rate</span>
      <span class="metric-tag">Click rate</span>
      <span class="metric-tag">CTOR</span>
      <span class="metric-tag">Total de enviados por campaña</span>
      <span class="metric-tag">Órdenes atribuidas al correo</span>
      <span class="metric-tag">Revenue por campaña</span>
      <span class="metric-tag">Revenue por cada 1.000 enviados</span>
      <span class="metric-tag">Ticket promedio</span>
      <span class="metric-tag">Uso y revenue de cupones</span>
      <span class="metric-tag">Recuperación de carrito</span>
      <span class="metric-tag">Tasa de recuperación</span>
      <span class="metric-tag">Evolución open rate vs click rate</span>
      <span class="metric-tag">Benchmark contra promedio de industria</span>
    </div>
    <div class="note-sub">Qué desbloquea cada solución</div>
    <ul class="sol">
      <li><strong>Montar el sitio en WordPress con FunnelKit:</strong> habilita open rate, click rate, CTOR y atribución de revenue por campaña, igual que en Entrelingo.</li>
      <li><strong>Poblar utm_term y utm_content en cada envío:</strong> permite saber qué enlace y qué versión del correo generan el agendamiento, no solo qué campaña.</li>
      <li><strong>Registrar el monto de cada venta en Airtable:</strong> convierte la tasa de conversión en revenue real por origen.</li>
    </ul>
  </div>
  <p class="footer-note">Dashboard SkillsPRO · Entrelingo Group · Datos de Airtable y Clientify · Actualización manual mensual</p>
</div>
<script>
const AGENDA=[
  {correo:'javicaceresarias@gmail.com',wa:'+61434995632',nombre:'JAVIERA ALEJANDRA CACERES ARIAS',creacion:'22/7/2026',consultoria:'–',consultoria2:'–',estado:'Invitado',sales:'Message',area:'Otros',profesion:'Antropología',experiencia:'Menos de 1 año',situacion:'Tengo el presupuesto',situacionTipo:'presu',utm_source:'newsletter',utm_medium:'email',utm_campaign:'oraculo',utm_term:'[utm_term]',utm_content:'[utm_content]'},
  {correo:'ik7528148@gmail.com',wa:'+923491983002',nombre:'IMRAN KHAN',creacion:'3/7/2026',consultoria:'31/7/2026 08:00 AEST',consultoria2:'–',estado:'Programada',sales:'–',area:'Construcción, Diseño y Drafting',profesion:'Electrician',experiencia:'Más de 5 años',situacion:'Podría invertir pero necesito claridad',situacionTipo:'podria',utm_source:'clientify',utm_medium:'email',utm_campaign:'tema_mes_año',utm_term:'[utm_term]',utm_content:'[utm_content]'},
  {correo:'ajiboyemayor01@gmail.com',wa:'+2348120300834',nombre:'AJIBOYE MAYOWA',creacion:'9/7/2026',consultoria:'31/7/2026 09:00 AEST',consultoria2:'–',estado:'Programada',sales:'–',area:'Ciencias, agricultura y medio ambiente',profesion:'Fruit Picking',experiencia:'De 1 a 3 años',situacion:'Tengo el presupuesto',situacionTipo:'presu',utm_source:'clientify',utm_medium:'email',utm_campaign:'pagan_australia',utm_term:'[utm_term]',utm_content:'[utm_content]'},
  {correo:'allison.su.valenzuela@gmail.com',wa:'+61425343706',nombre:'ALLISON SU',creacion:'22/7/2026',consultoria:'29/7/2026 09:20 AEST',consultoria2:'–',estado:'Programada',sales:'–',area:'Negocios y administración',profesion:'Gestión y alta dirección',experiencia:'De 3 a 5 años',situacion:'Tengo el presupuesto',situacionTipo:'presu',utm_source:'clientify',utm_medium:'email',utm_campaign:'cada_excusa_tiene_respuesta',utm_term:'[utm_term]',utm_content:'[utm_content]'},
  {correo:'jhonatan94096@hotmail.com',wa:'+32466353259',nombre:'JHONATAN SILVA',creacion:'10/7/2026',consultoria:'15/7/2026 09:00 AEST',consultoria2:'16/7/2026 GMT',estado:'Reprogramada',sales:'Closed',area:'Oficios varios (generales)',profesion:'Mecánico',experiencia:'De 1 a 3 años',situacion:'Tengo el presupuesto',situacionTipo:'presu',utm_source:'clientify',utm_medium:'email',utm_campaign:'despues_graduacion',utm_term:'[utm_term]',utm_content:'[utm_content]'},
  {correo:'mlourdesynigo@gmail.com',wa:'+5493815360979',nombre:'LOURDES YNIGO',creacion:'2/7/2026',consultoria:'4/7/2026 13:00 AEST',consultoria2:'–',estado:'Ejecutada',sales:'Closed',area:'Construcción, Diseño y Drafting',profesion:'Graduada de arquitectura',experiencia:'Más de 5 años',situacion:'Podría invertir pero necesito claridad',situacionTipo:'podria',utm_source:'clientify',utm_medium:'email',utm_campaign:'tarifa_visas',utm_term:'[utm_term]',utm_content:'[utm_content]'},
  {correo:'ismaelvaldes@…',wa:'+4…',nombre:'SILVANA / ISMAEL VALDÉS',creacion:'9/7/2026',consultoria:'18/7/2026 13:00 AEST',consultoria2:'22/7/2026 GMT',estado:'Ejecutada',sales:'Converted',area:'Ingeniería',profesion:'Soldador industrial',experiencia:'Más de 5 años',situacion:'Tengo el presupuesto',situacionTipo:'presu',utm_source:'clientify',utm_medium:'email',utm_campaign:'pagan_australia',utm_term:'[utm_term]',utm_content:'[utm_content]'},
  {correo:'ingenierali…@…',wa:'+6…',nombre:'LIVIA',creacion:'9/7/2026',consultoria:'17/7/2026 10:00 AEST',consultoria2:'–',estado:'Ejecutada',sales:'Converted',area:'Educación, artes y cocina',profesion:'Baker',experiencia:'De 3 a 5 años',situacion:'Podría invertir pero necesito claridad',situacionTipo:'podria',utm_source:'clientify',utm_medium:'email',utm_campaign:'pagan_australia',utm_term:'[utm_term]',utm_content:'[utm_content]'}
];
function pillEstado(e){
  const map={'Invitado':'p-invitado','Programada':'p-programada','Reprogramada':'p-reprogramada','Ejecutada':'p-ejecutada'};
  return '<span class="pill '+(map[e]||'p-none')+'">'+e+'</span>';
}
function pillSales(s){
  if(!s||s==='–')return '<span class="pill p-none">–</span>';
  if(s==='Converted')return '<span class="pill p-converted">Converted</span>';
  if(s==='Closed')return '<span class="pill p-closed">Closed</span>';
  return '<span class="pill p-message">'+s+'</span>';
}
function pillSituacion(r){
  return '<span class="pill '+(r.situacionTipo==='presu'?'p-presu':'p-podria')+'">'+r.situacion+'</span>';
}
document.getElementById('tbodyAgenda').innerHTML=AGENDA.map(r=>
  '<tr>'+
  '<td class="wrap">'+r.correo+'</td>'+
  '<td>'+r.wa+'</td>'+
  '<td class="wrap">'+r.nombre+'</td>'+
  '<td>'+r.creacion+'</td>'+
  '<td class="wrap">'+r.consultoria+'</td>'+
  '<td class="wrap">'+r.consultoria2+'</td>'+
  '<td>'+pillEstado(r.estado)+'</td>'+
  '<td>'+pillSales(r.sales)+'</td>'+
  '<td class="wrap">'+r.area+'</td>'+
  '<td class="wrap">'+r.profesion+'</td>'+
  '<td>'+r.experiencia+'</td>'+
  '<td class="wrap">'+pillSituacion(r)+'</td>'+
  '<td>'+r.utm_source+'</td>'+
  '<td>'+r.utm_medium+'</td>'+
  '<td class="hi-cell"><span class="campaign-tag'+(r.utm_campaign==='oraculo'?' oraculo':'')+'">'+r.utm_campaign+'</span></td>'+
  '<td class="muted">'+r.utm_term+'</td>'+
  '<td class="muted">'+r.utm_content+'</td>'+
  '</tr>'
).join('');
Chart.register(ChartDataLabels);
new Chart(document.getElementById('cEstado'),{
  type:'bar',
  data:{labels:['Invitado','Programada','Reprogramada','Ejecutada'],
    datasets:[{data:[1,3,1,3],backgroundColor:['#7c5cff','#ffb020','#ff6b5b','#0fb5c9'],borderRadius:8}]},
  options:{responsive:true,maintainAspectRatio:false,
    plugins:{legend:{display:false},datalabels:{anchor:'end',align:'end',color:'#0c465b',font:{size:13,weight:'bold'}}},
    scales:{x:{ticks:{font:{size:11,weight:'bold'},color:'#5d7883'},grid:{display:false}},
      y:{beginAtZero:true,ticks:{stepSize:1,font:{size:11},color:'#8ea6b0'},grid:{color:'#eef3f5'},afterDataLimits(s){s.max=s.max+1;}}}}
});
new Chart(document.getElementById('cVentas'),{
  type:'bar',
  data:{labels:['Agendados','Ejecutadas','Ventas'],
    datasets:[{data:[8,3,1],backgroundColor:['#0c465b','#0fb5c9','#3ecf8e'],borderRadius:8}]},
  options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,
    plugins:{legend:{display:false},datalabels:{anchor:'end',align:'end',color:'#0c465b',font:{size:12,weight:'bold'},
      formatter:(v,ctx)=>ctx.dataIndex===0?v:v+'  ('+(v/8*100).toFixed(1)+'%)'}},
    scales:{x:{beginAtZero:true,ticks:{stepSize:2,font:{size:11},color:'#8ea6b0'},grid:{color:'#eef3f5'},afterDataLimits(s){s.max=s.max+2;}},
      y:{ticks:{font:{size:11,weight:'bold'},color:'#5d7883'},grid:{display:false}}}}
});
const campCount={};
AGENDA.forEach(r=>{campCount[r.utm_campaign]=(campCount[r.utm_campaign]||0)+1;});
const campEntries=Object.entries(campCount).sort((a,b)=>b[1]-a[1]);
new Chart(document.getElementById('cCampaign'),{
  type:'bar',
  data:{labels:campEntries.map(e=>e[0]),
    datasets:[{data:campEntries.map(e=>e[1]),backgroundColor:['#0fb5c9','#7c5cff','#ffb020','#ff6b5b','#3ecf8e','#10728d'],borderRadius:8}]},
  options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,
    plugins:{legend:{display:false},datalabels:{anchor:'end',align:'end',color:'#0c465b',font:{size:12,weight:'bold'}}},
    scales:{x:{beginAtZero:true,ticks:{stepSize:1,font:{size:11},color:'#8ea6b0'},grid:{color:'#eef3f5'},afterDataLimits(s){s.max=s.max+1;}},
      y:{ticks:{font:{size:10,weight:'bold'},color:'#5d7883'},grid:{display:false}}}}
});
new Chart(document.getElementById('cSituacion'),{
  type:'doughnut',
  data:{labels:['Tengo el presupuesto','Podría invertir pero necesito claridad'],
    datasets:[{data:[5,3],backgroundColor:['#22a46a','#8de3b8'],borderWidth:3,borderColor:'#ffffff'}]},
  options:{responsive:true,maintainAspectRatio:false,cutout:'58%',
    plugins:{legend:{position:'bottom',labels:{font:{size:11},boxWidth:12,padding:14,color:'#5d7883'}},
      datalabels:{color:'#ffffff',font:{size:14,weight:'bold'},formatter:(v)=>v}}}
});
const now=new Date();
const months=['ene','feb','mar','abr','may','jun','jul','ago','sep','oct','nov','dic'];
document.getElementById('updatedTs').textContent=now.getDate()+' '+months[now.getMonth()]+' '+now.getHours()+':'+String(now.getMinutes()).padStart(2,'0');
</script>
