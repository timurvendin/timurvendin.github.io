<!doctype html>
<html lang="de">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>⚓🐳P2P Steuer-Generator für Krypto (DE version)</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-900 text-gray-100 min-h-screen p-6">
  <div class="max-w-7xl mx-auto space-y-6">
    <header class="flex justify-between items-center">
      <img src="https://upload.wikimedia.org/wikipedia/commons/4/49/Coat_of_Arms_of_Odesa.svg"
     alt="Wappen Odessa"
     class="w-12 h-auto inline-block align-middle" />
      <h1 class="text-xl font-bold text-cyan-400">⚓🐳P2P-Steuer-Generator — Krypto (DE)</h1>
      <span class="text-sm text-gray-400">Lokal im Browser. Bitte mit Steuerberater abstimmen.</span>
    </header>

    <div class="grid md:grid-cols-3 gap-6">
      <div class="md:col-span-2 space-y-6">
        <div class="bg-gray-800 rounded-xl shadow p-4 space-y-4">
          <div class="flex flex-col md:flex-row gap-4">
            <div class="flex-1">
              <label class="block text-sm text-gray-300 mb-1">Beginn des Steuerjahres</label>
              <input id="jahrStart" type="date" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100" />
            </div>
            <div class="w-full md:w-64">
              <label class="block text-sm text-gray-300 mb-1">Bewertungsmethode</label>
              <select id="method" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100">
                <option value="FIFO">FIFO (Standard)</option>
                <option value="LIFO">LIFO</option>
                <option value="AVG">Durchschnitt</option>
              </select>
            </div>
          </div>

          <div class="flex flex-wrap items-center gap-2">
            <button id="addBtn" class="bg-cyan-500 hover:bg-cyan-600 text-white px-3 py-2 rounded-md">Neue Transaktion</button>
            <button id="impCsv" class="border border-gray-600 text-gray-300 hover:bg-gray-700 px-3 py-2 rounded-md">CSV importieren</button>
            <input id="fileIn" type="file" accept="text/csv" class="hidden" />
            <span class="text-xs text-gray-400 ml-auto">Felder: Datum, Typ, Asset, Menge, Preis(EUR), Gebühr(EUR), Wallet/TxID, Klassifikation</span>
          </div>

          <div class="overflow-x-auto">
            <table class="w-full text-sm text-left mt-2">
              <thead class="bg-gray-700 text-gray-300">
                <tr>
                  <th class="px-3 py-2">Datum</th>
                  <th class="px-3 py-2">Typ</th>
                  <th class="px-3 py-2">Asset</th>
                  <th class="px-3 py-2">Menge</th>
                  <th class="px-3 py-2">Preis (EUR)</th>
                  <th class="px-3 py-2">Gebühr (EUR)</th>
                  <th class="px-3 py-2">Wallet / TxID</th>
                  <th class="px-3 py-2"></th>
                </tr>
              </thead>
              <tbody id="tbody" class="divide-y divide-gray-700"></tbody>
            </table>
          </div>

          <div class="flex flex-wrap gap-2 mt-4">
            <button id="exportCsv" class="bg-cyan-500 hover:bg-cyan-600 text-white px-3 py-2 rounded-md">Export CSV (Anlage SO)</button>
            <button id="exportPdf" class="bg-cyan-500 hover:bg-cyan-600 text-white px-3 py-2 rounded-md">Export PDF</button>
            <button id="saveJson" class="border border-gray-600 text-gray-300 hover:bg-gray-700 px-3 py-2 rounded-md">Export JSON</button>
          </div>

          <p class="text-xs text-gray-400 mt-2">Hinweis: Dieses Tool implementiert mehrere Heuristiken (FIFO/LIFO/Durchschnitt). Es bietet Hilfsfunktionen zur Erstellung einer Übersicht für die <strong>Anlage SO</strong> (private Veräußerungsgeschäfte). Rechtliche Aussagekraft nur mit Steuerberater.</p>
        </div>
      </div>

      <div class="space-y-6">
        <div class="bg-gray-800 rounded-xl shadow p-4 space-y-2">
          <h2 class="font-semibold">Zusammenfassung</h2>
          <div>
            <span class="text-sm text-gray-400">Realisierte Gewinne (EUR)</span>
            <div id="real" class="text-lg font-mono">0.00</div>
          </div>
          <div>
            <span class="text-sm text-gray-400">Unrealisierte Gewinne (EUR)</span>
            <div id="unreal" class="text-lg font-mono">0.00</div>
          </div>
          <div>
            <span class="text-sm text-gray-400">Anzahl Transaktionen</span>
            <div id="count" class="text-lg font-mono">0</div>
          </div>
          <p class="text-xs text-gray-500 pt-2 border-t border-gray-700">Aufbewahrungsempfehlung: Belege 8–10 Jahre aufbewahren (§147 AO / 2025 Anpassungen).</p>
        </div>

        <div class="bg-gray-800 rounded-xl shadow p-4">
          <h2 class="font-semibold text-sm mb-2">Deutsche Aspekte (Kurz)</h2>
          <ul class="list-disc pl-5 text-xs text-gray-400 space-y-1">
            <li>Spekulationsfrist: <strong>1 Jahr</strong> (bei Privatpersonen; Gewinne nach 1 Jahr meist steuerfrei), Ausnahmen bei Staking/Lending.</li>
            <li>Gewinne in Anlage SO eintragen (private Veräußerungsgeschäfte).</li>
            <li>BMF-Schreiben (06.03.2025) enthält detaillierte Dokumentationspflichten zu Kryptowerte.</li>
            <li>Aufbewahrungsfristen: i.d.R. 8–10 Jahre (§147 AO und Änderungen 2025).</li>
          </ul>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <div id="modal" class="hidden fixed inset-0 bg-black/60 flex items-center justify-center p-4">
      <div class="bg-gray-800 rounded-xl shadow p-6 w-full max-w-2xl space-y-4">
        <h3 class="text-lg font-semibold">Transaktion hinzufügen / bearbeiten</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block text-sm mb-1">Datum / Zeit</label>
            <input id="m_date" type="datetime-local" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100" />
          </div>
          <div>
            <label class="block text-sm mb-1">Typ</label>
            <select id="m_type" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100">
              <option>BUY</option>
              <option>SELL</option>
              <option>TRANSFER_IN</option>
              <option>TRANSFER_OUT</option>
              <option>INCOME</option>
            </select>
          </div>
          <div>
            <label class="block text-sm mb-1">Asset (Symbol)</label>
            <input id="m_asset" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100" />
          </div>
          <div>
            <label class="block text-sm mb-1">Menge</label>
            <input id="m_amount" type="number" step="any" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100" />
          </div>
          <div>
            <label class="block text-sm mb-1">Preis pro Einheit (EUR)</label>
            <input id="m_price" type="number" step="any" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100" />
          </div>
          <div>
            <label class="block text-sm mb-1">Gebühr (EUR)</label>
            <input id="m_fee" type="number" step="any" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100" />
          </div>
          <div class="md:col-span-2">
            <label class="block text-sm mb-1">Wallet-Adresse / TxID / Gegenpartei</label>
            <input id="m_tx" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100" />
          </div>
          <div class="md:col-span-2">
            <label class="block text-sm mb-1">Klassifikation (z.B. privat / beruflich / staking)</label>
            <input id="m_class" class="w-full rounded-md border border-gray-600 bg-gray-700 p-2 text-gray-100" />
          </div>
        </div>
        <div class="flex justify-end gap-2">
          <button id="save" class="bg-cyan-500 hover:bg-cyan-600 text-white px-4 py-2 rounded-md">Speichern</button>
          <button id="cancel" class="border border-gray-600 text-gray-300 hover:bg-gray-700 px-4 py-2 rounded-md">Abbrechen</button>
        </div>
      </div>
    </div>
  </div>

  <script>
    // Vollständige JS-Logik (angepasst für Tailwind-Modal-Handling, 'klass' statt 'class')
    const txs = [];
    let edit = -1;

    const $ = id => document.getElementById(id);

    function render(){
      const tbody = $('tbody'); tbody.innerHTML = '';
      txs.forEach((t,i)=>{
        const tr = document.createElement('tr');
        tr.className = 'hover:bg-gray-700';
        tr.innerHTML = `<td class="px-3 py-2">${new Date(t.date).toLocaleString()}</td>
                        <td class="px-3 py-2">${t.type}</td>
                        <td class="px-3 py-2">${t.asset}</td>
                        <td class="px-3 py-2">${t.amount}</td>
                        <td class="px-3 py-2">${t.price||''}</td>
                        <td class="px-3 py-2">${t.fee||''}</td>
                        <td class="px-3 py-2 text-gray-400">${t.tx||''}</td>
                        <td class="px-3 py-2 text-right"><button data-i="${i}" class="edit-btn text-sm text-cyan-300 hover:underline">Bearbeiten</button></td>`;
        tbody.appendChild(tr);
      });
      $('count').textContent = txs.length;
      computeSummary();
      document.querySelectorAll('button[data-i]').forEach(b=>b.addEventListener('click',e=>openEdit(Number(e.target.dataset.i))));
    }

    function openNew(){
      edit = -1;
      $('m_date').value = new Date().toISOString().slice(0,16);
      $('m_type').value = 'BUY';
      $('m_asset').value = 'BTC';
      $('m_amount').value = '';
      $('m_price').value = '';
      $('m_fee').value = '';
      $('m_tx').value = '';
      $('m_class').value = '';
      $('modal').classList.remove('hidden');
    }

    function openEdit(i){
      edit = i;
      const t = txs[i];
      $('m_date').value = new Date(t.date).toISOString().slice(0,16);
      $('m_type').value = t.type;
      $('m_asset').value = t.asset;
      $('m_amount').value = t.amount;
      $('m_price').value = t.price || '';
      $('m_fee').value = t.fee || '';
      $('m_tx').value = t.tx || '';
      $('m_class').value = t.klass || '';
      $('modal').classList.remove('hidden');
    }

    function saveTx(){
      const t = {
        date: new Date($('m_date').value).toISOString(),
        type: $('m_type').value,
        asset: $('m_asset').value.trim().toUpperCase(),
        amount: parseFloat($('m_amount').value),
        price: $('m_price').value ? parseFloat($('m_price').value) : null,
        fee: $('m_fee').value ? parseFloat($('m_fee').value) : 0,
        tx: $('m_tx').value,
        klass: $('m_class').value
      };
      if(!t.asset || isNaN(t.amount)) { alert('Bitte Asset und Menge angeben'); return; }
      if(edit >= 0) txs[edit] = t; else txs.push(t);
      $('modal').classList.add('hidden');
      render();
    }

    function computeSummary(){
      const method = $('method').value;
      const byAsset = {};
      let realized = 0, unrealized = 0;
      const sorted = txs.slice().sort((a,b)=>new Date(a.date)-new Date(b.date));

      sorted.forEach(t=>{
        const a = t.asset;
        if(!byAsset[a]) byAsset[a] = [];
        if(['BUY','TRANSFER_IN','INCOME'].includes(t.type)){
          byAsset[a].push({qty: t.amount, cost: t.price || 0, fee: t.fee || 0, date: t.date});
        } else if(['SELL','TRANSFER_OUT'].includes(t.type)){
          let rem = t.amount;
          if(method === 'AVG'){
            const lots = byAsset[a];
            const totalQty = lots.reduce((s,l)=>s+l.qty,0);
            const totalCost = lots.reduce((s,l)=>s + l.qty * l.cost + l.fee,0);
            const avg = totalQty ? totalCost/totalQty : 0;
            realized += rem * ((t.price||0) - avg) - (t.fee||0);
            byAsset[a].forEach(l=> l.qty = 0);
            byAsset[a] = byAsset[a].filter(l=>l.qty>0);
          } else {
            if(method === 'LIFO') byAsset[a].reverse();
            while(rem > 0 && byAsset[a].length){
              const lot = byAsset[a][0];
              const take = Math.min(rem, lot.qty);
              const costBasis = take * lot.cost + (lot.fee * (take/lot.qty || 0));
              const proceeds = take * (t.price || 0) - (t.fee * (take / t.amount || 0));
              realized += (proceeds - costBasis);
              lot.qty -= take; rem -= take;
              if(lot.qty <= 0) byAsset[a].shift();
            }
            if(method === 'LIFO') byAsset[a].reverse();
          }
        }
      });

      Object.keys(byAsset).forEach(asset=>{
        byAsset[asset].forEach(l=>{
          const prices = txs.filter(x=>x.asset===asset && x.price).map(x=>({d:new Date(x.date), p:x.price})).sort((a,b)=>b.d-a.d);
          const last = prices.length ? prices[0].p : l.cost;
          unrealized += l.qty * (last - l.cost);
        });
      });

      $('real').textContent = realized.toFixed(2);
      $('unreal').textContent = unrealized.toFixed(2);
    }

    function exportCsv(){
      if(!txs.length){ alert('Keine Transaktionen.'); return; }
      const header = ['datum_iso','typ','asset','menge','preis_eur','gebuehr_eur','txid_wallet','klassifikation','in_steuerjahr'];
      const ys = $('jahrStart').value ? new Date($('jahrStart').value + 'T00:00:00') : null;
      const rows = [header];
      txs.forEach(t=>{
        const inYear = ys ? (new Date(t.date) >= ys && new Date(t.date) < new Date(ys.getFullYear()+1+'-01-01')) : '';
        rows.push([t.date,t.type,t.asset,t.amount,t.price||'',t.fee||'',t.tx||'',t.klass||'',inYear]);
      });
      const csv = rows.map(r=>r.map(c=>`"${String(c).replace(/"/g,'""')}"`).join(',')).join('\n');
      const blob = new Blob([csv],{type:'text/csv;charset=utf-8;'});
      const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href = url; a.download = `steuer_p2p_${(new Date()).toISOString().slice(0,10)}.csv`; a.click(); URL.revokeObjectURL(url);
    }

    async function exportPdf(){
      if(!txs.length){ alert('Keine Transaktionen.'); return; }
      try {
        await loadScript('https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js');
        await loadScript('https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js');
      } catch(e){
        alert('Fehler beim Laden der PDF-Bibliotheken (Internetverbindung benötigt).');
        return;
      }
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({unit:'pt',format:'a4'});
      doc.setFontSize(12); doc.text('P2P Steuerreport (DE)',40,40); doc.text('Erstellt: '+new Date().toLocaleString(),40,56);
      const cols = ['Datum','Typ','Asset','Menge','Preis(EUR)','Gebühr(EUR)','Tx/Wallet','Klassifikation'];
      const rows = txs.map(t=>[new Date(t.date).toLocaleString(),t.type,t.asset,t.amount,t.price||'',t.fee||'',t.tx||'',t.klass||'']);
      // @ts-ignore
      doc.autoTable({head:[cols],body:rows,startY:80,styles:{fontSize:9}});
      doc.save(`steuer_p2p_${(new Date()).toISOString().slice(0,10)}.pdf`);
    }

    function loadScript(src){
      return new Promise((res,rej)=>{
        const s = document.createElement('script'); s.src = src; s.onload = res; s.onerror = rej; document.head.appendChild(s);
      });
    }

    function importCsv(file){
      const r = new FileReader();
      r.onload = e=>{
        const text = e.target.result;
        const lines = text.split(/\r?\n/).filter(l=>l.trim());
        if(!lines.length) return;
        const header = lines.shift().split(',').map(h=>h.replace(/(^"|"$)/g,'').trim().toLowerCase());
        lines.forEach(l=>{
          const cols = l.split(',').map(c=>c.replace(/(^"|"$)/g,'').trim());
          const obj = {};
          header.forEach((h,idx)=> obj[h]=cols[idx]);
          txs.push({
            date: obj.datum_iso || new Date().toISOString(),
            type: (obj.typ || 'BUY').toUpperCase(),
            asset: (obj.asset || 'BTC').toUpperCase(),
            amount: parseFloat(obj.menge || 0),
            price: obj.preis_eur ? parseFloat(obj.preis_eur) : (obj.preis ? parseFloat(obj.preis) : null),
            fee: obj.gebuehr_eur ? parseFloat(obj.gebuehr_eur) : (obj.fee ? parseFloat(obj.fee) : 0),
            tx: obj.txid_wallet || '',
            klass: obj.klassifikation || obj.klass || ''
          });
        });
        render();
      };
      r.readAsText(file);
    }

    // Events
    $('addBtn').addEventListener('click', openNew);
    $('cancel').addEventListener('click', ()=> $('modal').classList.add('hidden'));
    $('save').addEventListener('click', saveTx);
    $('exportCsv').addEventListener('click', exportCsv);
    $('exportPdf').addEventListener('click', exportPdf);
    $('saveJson').addEventListener('click', ()=>{ const blob=new Blob([JSON.stringify(txs,null,2)],{type:'application/json'}); const url=URL.createObjectURL(blob); const a=document.createElement('a'); a.href=url; a.download='p2p_txs.json'; a.click(); URL.revokeObjectURL(url); });
    $('impCsv').addEventListener('click', ()=> $('fileIn').click());
    $('fileIn').addEventListener('change', e=>{ if(e.target.files.length) importCsv(e.target.files[0]); e.target.value=''; });

    // Initial render
    render();
  </script>
</body>
</html>
