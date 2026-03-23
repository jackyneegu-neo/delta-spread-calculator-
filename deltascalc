<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Long–Short P&L Calculator</title>
  <link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Syne:wght@500;600;700;800&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:        #f0f2f5;
      --surface:   #ffffff;
      --surface2:  #f7f8fa;
      --border:    #e0e3ea;
      --border2:   #c8cdd8;
      --text:      #1a1e2e;
      --muted:     #6b7488;
      --long:      #0f7b4e;
      --long-bg:   #edfaf3;
      --long-bd:   #b6e9d2;
      --short:     #c0392b;
      --short-bg:  #fff2f1;
      --short-bd:  #f5c0bc;
      --accent:    #3b55e6;
      --accent-bg: #eff1fd;
      --gold:      #b07d20;
      --gold-bg:   #fdf6e3;
      --gold-bd:   #e8d39a;
      --green:     #16a05c;
      --red:       #d63b2f;
      --mono:      'DM Mono', monospace;
      --sans:      'DM Sans', sans-serif;
      --display:   'Syne', sans-serif;
    }

    body {
      font-family: var(--sans);
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      padding: 28px 16px 60px;
    }

    .page-header {
      max-width: 940px;
      margin: 0 auto 24px;
      display: flex;
      align-items: baseline;
      gap: 12px;
    }
    .page-header h1 {
      font-family: var(--display);
      font-size: 26px;
      font-weight: 800;
      color: var(--text);
      letter-spacing: -0.5px;
    }
    .page-header .badge {
      font-family: var(--mono);
      font-size: 11px;
      font-weight: 500;
      background: var(--accent-bg);
      color: var(--accent);
      border: 1px solid #c5ccf7;
      border-radius: 4px;
      padding: 2px 8px;
      letter-spacing: 0.5px;
    }

    .layout {
      max-width: 940px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
    }

    /* Cards */
    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 20px 22px;
      box-shadow: 0 1px 4px rgba(0,0,0,0.06);
    }
    .card.full { grid-column: 1 / -1; }
    .card.long-card  { border-top: 3px solid var(--long); }
    .card.short-card { border-top: 3px solid var(--short); }
    .card.global-card { border-top: 3px solid var(--accent); }
    .card.result-card { border-top: 3px solid var(--gold); }
    .card.formula-card { border-top: 3px solid #8b5cf6; background: #fafafe; }

    .card-title {
      font-family: var(--display);
      font-size: 13px;
      font-weight: 700;
      letter-spacing: 1px;
      text-transform: uppercase;
      margin-bottom: 16px;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .card-title .dot {
      width: 8px; height: 8px;
      border-radius: 50%;
      flex-shrink: 0;
    }
    .dot-long  { background: var(--long); }
    .dot-short { background: var(--short); }
    .dot-global { background: var(--accent); }
    .dot-gold   { background: var(--gold); }
    .dot-purple { background: #8b5cf6; }

    /* Field grid */
    .field-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px 16px;
    }
    .field-grid.cols3 { grid-template-columns: 1fr 1fr 1fr; }

    .field { display: flex; flex-direction: column; }
    .field label {
      font-size: 11.5px;
      font-weight: 600;
      color: var(--muted);
      margin-bottom: 5px;
      letter-spacing: 0.3px;
      text-transform: uppercase;
    }
    .field input {
      font-family: var(--mono);
      font-size: 13.5px;
      padding: 8px 10px;
      border: 1.5px solid var(--border);
      border-radius: 7px;
      background: var(--surface2);
      color: var(--text);
      transition: border-color 0.15s, box-shadow 0.15s;
      outline: none;
    }
    .field input:focus {
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(59,85,230,0.1);
    }
    .field input[readonly] {
      background: #f0f4ff;
      border-color: #d0d8f8;
      color: var(--accent);
      cursor: default;
    }
    .field input.long-readonly {
      background: var(--long-bg);
      border-color: var(--long-bd);
      color: var(--long);
    }
    .field input.short-readonly {
      background: var(--short-bg);
      border-color: var(--short-bd);
      color: var(--short);
    }
    .field input::placeholder { color: #bec3cf; }

    /* Buttons */
    .btn-row {
      grid-column: 1 / -1;
      display: flex;
      gap: 10px;
      margin-top: 4px;
    }
    .btn {
      font-family: var(--display);
      font-weight: 700;
      font-size: 13px;
      letter-spacing: 0.5px;
      padding: 10px 22px;
      border-radius: 8px;
      border: none;
      cursor: pointer;
      transition: transform 0.1s, box-shadow 0.1s;
    }
    .btn:active { transform: scale(0.97); }
    .btn-calc {
      background: var(--accent);
      color: #fff;
      box-shadow: 0 2px 8px rgba(59,85,230,0.3);
    }
    .btn-calc:hover { box-shadow: 0 4px 14px rgba(59,85,230,0.4); }
    .btn-reset {
      background: var(--surface2);
      color: var(--muted);
      border: 1.5px solid var(--border2);
    }
    .btn-reset:hover { background: var(--border); }

    /* Results */
    .result-grid {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      gap: 12px;
      margin-bottom: 16px;
    }
    .result-tile {
      background: var(--surface2);
      border: 1.5px solid var(--border);
      border-radius: 10px;
      padding: 14px 16px;
      display: flex;
      flex-direction: column;
      gap: 6px;
    }
    .result-tile .rt-label {
      font-size: 11px;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      color: var(--muted);
    }
    .result-tile .rt-pts {
      font-family: var(--mono);
      font-size: 17px;
      font-weight: 500;
      color: var(--text);
    }
    .result-tile .rt-val {
      font-family: var(--mono);
      font-size: 12px;
      color: var(--muted);
    }
    .result-tile.pos .rt-pts { color: var(--green); }
    .result-tile.neg .rt-pts { color: var(--red); }
    .result-tile.long-tile  { border-color: var(--long-bd); background: var(--long-bg); }
    .result-tile.short-tile { border-color: var(--short-bd); background: var(--short-bg); }
    .result-tile.net-tile   { border-color: var(--gold-bd); background: var(--gold-bg); }
    .result-tile.net-tile .rt-label { color: var(--gold); }

    .net-banner {
      background: linear-gradient(135deg, #1a1e2e 0%, #2a3080 100%);
      border-radius: 10px;
      padding: 18px 22px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
    }
    .net-banner .nb-label {
      font-family: var(--display);
      font-size: 12px;
      font-weight: 700;
      letter-spacing: 1px;
      text-transform: uppercase;
      color: #8a9cc8;
    }
    .net-banner .nb-val {
      font-family: var(--mono);
      font-size: 28px;
      font-weight: 500;
      color: #fff;
    }
    .net-banner .nb-val.pos { color: #4ade80; }
    .net-banner .nb-val.neg { color: #f87171; }

    /* Spread row */
    .spread-row {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 8px 12px;
      background: #f4f5ff;
      border: 1px solid #dde1f8;
      border-radius: 8px;
      margin-bottom: 12px;
      font-size: 12.5px;
    }
    .spread-row .sr-label { color: var(--muted); font-weight: 600; }
    .spread-row .sr-val { font-family: var(--mono); font-weight: 500; color: var(--accent); }

    /* Formula section */
    .formula-block {
      background: #f8f7ff;
      border: 1px solid #e2dff8;
      border-radius: 8px;
      padding: 14px 16px;
      margin-bottom: 12px;
    }
    .formula-block h3 {
      font-family: var(--display);
      font-size: 12px;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.8px;
      color: #7c6ff7;
      margin-bottom: 10px;
    }
    .formula-line {
      font-family: var(--mono);
      font-size: 12.5px;
      line-height: 1.9;
      color: var(--text);
    }
    .formula-line span { color: #5a54c4; font-weight: 500; }
    .formula-line em { color: var(--muted); font-style: normal; font-size: 11.5px; }

    .calc-breakdown {
      display: none;
      margin-top: 14px;
    }
    .calc-breakdown.visible { display: block; }
    .cb-title {
      font-family: var(--display);
      font-size: 11px;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.8px;
      color: var(--muted);
      margin-bottom: 8px;
    }
    .cb-table {
      width: 100%;
      border-collapse: collapse;
      font-size: 12.5px;
    }
    .cb-table th {
      text-align: left;
      padding: 6px 10px;
      background: var(--surface2);
      border: 1px solid var(--border);
      font-weight: 600;
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      color: var(--muted);
    }
    .cb-table td {
      padding: 7px 10px;
      border: 1px solid var(--border);
      font-family: var(--mono);
      font-size: 12px;
    }
    .cb-table tr:nth-child(even) td { background: var(--surface2); }
    .cb-table td.pos { color: var(--green); font-weight: 500; }
    .cb-table td.neg { color: var(--red); font-weight: 500; }
    .cb-table .total-row td {
      font-weight: 700;
      background: var(--gold-bg);
      border-color: var(--gold-bd);
    }

    /* Dividers and util */
    .divider {
      border: none;
      border-top: 1px solid var(--border);
      margin: 14px 0;
    }

    /* Hidden until calculated */
    .results-section { display: none; }
    .results-section.visible { display: block; }

    /* Formula toggle */
    .toggle-formula {
      font-family: var(--sans);
      font-size: 12px;
      font-weight: 600;
      color: #7c6ff7;
      background: none;
      border: 1px solid #d4d0f8;
      border-radius: 6px;
      padding: 4px 12px;
      cursor: pointer;
      margin-left: auto;
      display: flex;
      align-items: center;
      gap: 4px;
    }

    /* Responsive */
    @media (max-width: 680px) {
      .layout { grid-template-columns: 1fr; }
      .card.full { grid-column: 1; }
      .result-grid { grid-template-columns: 1fr; }
      .field-grid { grid-template-columns: 1fr; }
      .field-grid.cols3 { grid-template-columns: 1fr 1fr; }
    }
  </style>
</head>
<body>

  <div class="page-header">
    <h1>Long–Short P&L Calculator</h1>
    <span class="badge">STRATEGY TOOL</span>
  </div>

  <div class="layout">

    <!-- Global Card -->
    <div class="card global-card full">
      <div class="card-title"><span class="dot dot-global"></span> Global Parameters</div>
      <div class="field-grid cols3">
        <div class="field">
          <label>Total Trades (per leg)</label>
          <input id="Total_trades" type="number" placeholder="e.g. 10">
        </div>
        <div class="field">
          <label>Lot Size (multiplier)</label>
          <input id="Lot_size" type="number" placeholder="e.g. 1 or 25">
        </div>
        <div class="field">
          <label>Price Spread (auto: Short − Long)</label>
          <input id="Price_spread" type="number" placeholder="auto" readonly>
        </div>
      </div>
    </div>

    <!-- Long Card -->
    <div class="card long-card">
      <div class="card-title"><span class="dot dot-long"></span> Long Leg</div>
      <div class="field-grid">
        <div class="field">
          <label>Entry Price</label>
          <input id="L_entry" type="number" placeholder="e.g. 275000">
        </div>
        <div class="field">
          <label>SL (Points)</label>
          <input id="L_SL_pts" type="number" placeholder="e.g. 4000">
        </div>
        <div class="field">
          <label>TP (Points)</label>
          <input id="L_TP_pts" type="number" placeholder="e.g. 6000">
        </div>
        <div class="field">
          <label>Stop Loss Hits</label>
          <input id="L_SL_count" type="number" placeholder="e.g. 5">
        </div>
        <div class="field">
          <label>TP Hits (auto)</label>
          <input id="L_TP_count" type="number" placeholder="auto" readonly class="long-readonly">
        </div>
        <div class="field">
          <label>SL Exit Price (auto)</label>
          <input id="L_SL_exit" type="number" placeholder="auto" readonly class="long-readonly">
        </div>
        <div class="field">
          <label>TP Exit Price (auto)</label>
          <input id="L_TP_exit" type="number" placeholder="auto" readonly class="long-readonly">
        </div>
      </div>
    </div>

    <!-- Short Card -->
    <div class="card short-card">
      <div class="card-title"><span class="dot dot-short"></span> Short Leg</div>
      <div class="field-grid">
        <div class="field">
          <label>Entry Price</label>
          <input id="S_entry" type="number" placeholder="e.g. 276000">
        </div>
        <div class="field">
          <label>SL (Points)</label>
          <input id="S_SL_pts" type="number" placeholder="e.g. 4000">
        </div>
        <div class="field">
          <label>TP (Points)</label>
          <input id="S_TP_pts" type="number" placeholder="e.g. 6000">
        </div>
        <div class="field">
          <label>Stop Loss Hits</label>
          <input id="S_SL_count" type="number" placeholder="e.g. 5">
        </div>
        <div class="field">
          <label>TP Hits (auto)</label>
          <input id="S_TP_count" type="number" placeholder="auto" readonly class="short-readonly">
        </div>
        <div class="field">
          <label>SL Exit Price (auto)</label>
          <input id="S_SL_exit" type="number" placeholder="auto" readonly class="short-readonly">
        </div>
        <div class="field">
          <label>TP Exit Price (auto)</label>
          <input id="S_TP_exit" type="number" placeholder="auto" readonly class="short-readonly">
        </div>
      </div>
    </div>

    <!-- Buttons -->
    <div class="btn-row" style="grid-column: 1 / -1; margin-top: 0;">
      <button class="btn btn-calc" id="calcBtn">▶ Calculate</button>
      <button class="btn btn-reset" id="resetBtn">↺ Reset</button>
    </div>

    <!-- Results -->
    <div class="card result-card full results-section" id="resultsCard">
      <div style="display:flex; align-items:center; margin-bottom:16px;">
        <div class="card-title" style="margin-bottom:0;"><span class="dot dot-gold"></span> Results</div>
        <button class="toggle-formula" id="toggleFormula">⊕ Show Full Breakdown</button>
      </div>

      <!-- Spread info row -->
      <div class="spread-row" id="spreadRow">
        <span class="sr-label">Entry Spread (Short − Long):</span>
        <span class="sr-val" id="spreadDisplay">—</span>
        <span class="sr-label" style="margin-left:12px;">Spread × Trades:</span>
        <span class="sr-val" id="spreadTotalDisplay">—</span>
      </div>

      <!-- 3 result tiles -->
      <div class="result-grid">
        <div class="result-tile long-tile" id="tile_long">
          <div class="rt-label">Long Leg P&L</div>
          <div class="rt-pts" id="L_pts">—</div>
          <div class="rt-val" id="L_val">—</div>
        </div>
        <div class="result-tile short-tile" id="tile_short">
          <div class="rt-label">Short Leg P&L</div>
          <div class="rt-pts" id="S_pts">—</div>
          <div class="rt-val" id="S_val">—</div>
        </div>
        <div class="result-tile net-tile">
          <div class="rt-label">Spread Adjustment</div>
          <div class="rt-pts" id="Adj_pts">—</div>
          <div class="rt-val" id="Adj_val">—</div>
        </div>
      </div>

      <!-- Net banner -->
      <div class="net-banner">
        <div>
          <div class="nb-label">Net Combined P&L (after spread adj. × lot size)</div>
          <div style="font-size:11px;color:#6a7fa0;margin-top:3px;">= (Long + Short + Spread Adj.) × Lot Size</div>
        </div>
        <div class="nb-val" id="Net_val">—</div>
      </div>

      <!-- Full breakdown (hidden by default) -->
      <div class="calc-breakdown" id="calcBreakdown">
        <hr class="divider">

        <!-- Formula Reference -->
        <div class="formula-block">
          <h3>Formula Reference</h3>
          <div class="formula-line">
            <span>Long P&L (pts)</span>  = (TP_hits × TP_pts) − (SL_hits × SL_pts)<br>
            <em>                 = (TP_count × TP_pts) − (SL_count × SL_pts)</em>
          </div>
          <hr class="divider" style="margin:8px 0;">
          <div class="formula-line">
            <span>Short P&L (pts)</span> = (TP_hits × TP_pts) − (SL_hits × SL_pts)<br>
            <em>                  = same formula, short leg values</em>
          </div>
          <hr class="divider" style="margin:8px 0;">
          <div class="formula-line">
            <span>Spread Adj. (pts)</span> = (Short_entry − Long_entry) × Total_trades<br>
            <em>                   = captures cumulative entry-price differential</em>
          </div>
          <hr class="divider" style="margin:8px 0;">
          <div class="formula-line">
            <span>Net P&L (pts)</span>    = Long P&L + Short P&L + Spread Adj.<br>
            <span>Net P&L (value)</span>  = Net P&L (pts) × Lot_size
          </div>
        </div>

        <!-- Step-by-step breakdown table -->
        <div class="cb-title">Step-by-Step Calculation</div>
        <table class="cb-table" id="breakdownTable">
          <thead>
            <tr>
              <th>Step</th>
              <th>Description</th>
              <th>Long</th>
              <th>Short</th>
            </tr>
          </thead>
          <tbody id="breakdownBody">
          </tbody>
        </table>
      </div>
    </div>

  </div><!-- /layout -->

  <script>
    const $ = id => document.getElementById(id);

    function num(id) {
      const v = $(id).value;
      return v === '' ? 0 : parseFloat(v);
    }
    function setVal(id, v) {
      $(id).value = v;
    }
    function fmtPts(x) {
      return (x >= 0 ? '+' : '') + x.toLocaleString('en-IN', { maximumFractionDigits: 2 }) + ' pts';
    }
    function fmtVal(x, lot) {
      if (!lot || lot === 0) return '(no lot size)';
      const v = x * lot;
      return (v >= 0 ? '+' : '') + v.toLocaleString('en-IN', { maximumFractionDigits: 2 }) + ' value';
    }
    function fmtNum(x) {
      return x.toLocaleString('en-IN', { maximumFractionDigits: 2 });
    }
    function colorClass(x) {
      return x > 0 ? 'pos' : x < 0 ? 'neg' : '';
    }

    function recalcAuto() {
      const total   = num('Total_trades');
      const L_sl    = num('L_SL_count');
      const S_sl    = num('S_SL_count');
      const L_entry = num('L_entry');
      const S_entry = num('S_entry');
      const L_sl_pts = num('L_SL_pts');
      const L_tp_pts = num('L_TP_pts');
      const S_sl_pts = num('S_SL_pts');
      const S_tp_pts = num('S_TP_pts');

      const L_tp = Math.max(total - L_sl, 0);
      const S_tp = Math.max(total - S_sl, 0);
      setVal('L_TP_count', L_tp);
      setVal('S_TP_count', S_tp);

      // Exit prices
      if (L_entry) {
        setVal('L_SL_exit', L_entry - L_sl_pts);
        setVal('L_TP_exit', L_entry + L_tp_pts);
      } else {
        setVal('L_SL_exit', '');
        setVal('L_TP_exit', '');
      }
      if (S_entry) {
        setVal('S_SL_exit', S_entry + S_sl_pts);
        setVal('S_TP_exit', S_entry - S_tp_pts);
      } else {
        setVal('S_SL_exit', '');
        setVal('S_TP_exit', '');
      }

      // Spread
      if (L_entry && S_entry) {
        setVal('Price_spread', S_entry - L_entry);
      } else {
        setVal('Price_spread', '');
      }
    }

    ['Total_trades','L_SL_count','S_SL_count','L_entry','S_entry',
     'L_SL_pts','L_TP_pts','S_SL_pts','S_TP_pts'].forEach(id => {
      $(id).addEventListener('input', recalcAuto);
    });

    $('calcBtn').addEventListener('click', function () {
      const total      = num('Total_trades');
      const lot        = num('Lot_size');
      const L_TP_pts   = num('L_TP_pts');
      const L_SL_pts   = num('L_SL_pts');
      const L_TP_count = num('L_TP_count');
      const L_SL_count = num('L_SL_count');
      const S_TP_pts   = num('S_TP_pts');
      const S_SL_pts   = num('S_SL_pts');
      const S_TP_count = num('S_TP_count');
      const S_SL_count = num('S_SL_count');
      const L_entry    = num('L_entry');
      const S_entry    = num('S_entry');

      // Core P&L
      const L_gain = L_TP_count * L_TP_pts;
      const L_loss = L_SL_count * L_SL_pts;
      const L_PnL  = L_gain - L_loss;

      const S_gain = S_TP_count * S_TP_pts;
      const S_loss = S_SL_count * S_SL_pts;
      const S_PnL  = S_gain - S_loss;

      // Spread adjustment = (Short entry - Long entry) × total trades
      const spread     = (L_entry && S_entry) ? S_entry - L_entry : 0;
      const spreadAdj  = spread * total;   // cumulative across all trade pairs
      const netPts     = L_PnL + S_PnL + spreadAdj;
      const netVal     = netPts * (lot || 1);

      // Update tiles
      const Lel = $('L_pts'); Lel.textContent = fmtPts(L_PnL);
      Lel.className = 'rt-pts ' + colorClass(L_PnL);
      $('L_val').textContent = fmtVal(L_PnL, lot);

      const Sel = $('S_pts'); Sel.textContent = fmtPts(S_PnL);
      Sel.className = 'rt-pts ' + colorClass(S_PnL);
      $('S_val').textContent = fmtVal(S_PnL, lot);

      const Ael = $('Adj_pts'); Ael.textContent = fmtPts(spreadAdj);
      Ael.className = 'rt-pts ' + colorClass(spreadAdj);
      $('Adj_val').textContent = fmtVal(spreadAdj, lot);

      const Nel = $('Net_val'); Nel.textContent = fmtNum(netVal) + ' value';
      Nel.className = 'nb-val ' + colorClass(netVal);

      // Spread info row
      $('spreadDisplay').textContent = spread ? fmtNum(spread) + ' pts' : 'N/A (no entry prices)';
      $('spreadTotalDisplay').textContent = spread ? fmtNum(spreadAdj) + ' pts (' + total + ' trades)' : 'N/A';

      // Build breakdown table
      const rows = [
        ['1', 'Entry Price', fmtNum(L_entry) || '—', fmtNum(S_entry) || '—', '', ''],
        ['2', 'SL Exit Price', fmtNum(L_entry - num('L_SL_pts')) || '—', fmtNum(S_entry + num('S_SL_pts')) || '—', '', ''],
        ['3', 'TP Exit Price', fmtNum(L_entry + num('L_TP_pts')) || '—', fmtNum(S_entry - num('S_TP_pts')) || '—', '', ''],
        ['4', 'Total Trades', fmtNum(total), fmtNum(total), '', ''],
        ['5', 'SL Hits × SL Points', fmtNum(L_SL_count)+' × '+fmtNum(L_SL_pts), fmtNum(S_SL_count)+' × '+fmtNum(S_SL_pts), '', ''],
        ['6', 'TP Hits × TP Points', fmtNum(L_TP_count)+' × '+fmtNum(L_TP_pts), fmtNum(S_TP_count)+' × '+fmtNum(S_TP_pts), '', ''],
        ['7', 'Gross Gain (pts)', '+'+fmtNum(L_gain), '+'+fmtNum(S_gain), 'pos', 'pos'],
        ['8', 'Gross Loss (pts)', '−'+fmtNum(L_loss), '−'+fmtNum(S_loss), 'neg', 'neg'],
        ['9', 'Leg P&L (pts)', fmtPts(L_PnL), fmtPts(S_PnL), colorClass(L_PnL), colorClass(S_PnL)],
        ['10','Leg P&L × Lot Size', fmtNum(L_PnL*lot), fmtNum(S_PnL*lot), colorClass(L_PnL), colorClass(S_PnL)],
      ];

      const tbody = $('breakdownBody');
      tbody.innerHTML = '';
      rows.forEach(r => {
        const tr = document.createElement('tr');
        tr.innerHTML = `<td>${r[0]}</td><td>${r[1]}</td>
          <td class="${r[4]}">${r[2]}</td>
          <td class="${r[5]}">${r[3]}</td>`;
        tbody.appendChild(tr);
      });

      // Spread adj row
      const trSpread = document.createElement('tr');
      trSpread.innerHTML = `<td>11</td><td>Spread Adj. (Short−Long) × Trades</td>
        <td colspan="2" class="${colorClass(spreadAdj)} total-row">${fmtNum(spreadAdj)} pts → × ${fmtNum(lot)} lot = ${fmtNum(spreadAdj * lot)}</td>`;
      tbody.appendChild(trSpread);

      // Net row
      const trNet = document.createElement('tr');
      trNet.className = 'total-row';
      trNet.innerHTML = `<td>12</td><td><strong>NET COMBINED P&L</strong></td>
        <td colspan="2" class="${colorClass(netVal)}"><strong>${fmtNum(netPts)} pts × ${fmtNum(lot)} lot = ${fmtNum(netVal)}</strong></td>`;
      tbody.appendChild(trNet);

      $('resultsCard').style.display = 'block';
      $('resultsCard').classList.add('visible');
    });

    // Toggle breakdown
    let breakdownVisible = false;
    $('toggleFormula').addEventListener('click', function () {
      breakdownVisible = !breakdownVisible;
      $('calcBreakdown').classList.toggle('visible', breakdownVisible);
      this.textContent = breakdownVisible ? '⊖ Hide Breakdown' : '⊕ Show Full Breakdown';
    });

    $('resetBtn').addEventListener('click', function () {
      document.querySelectorAll('input[type="number"]').forEach(inp => {
        if (!inp.readOnly) inp.value = '';
        else inp.value = '';
      });
      $('resultsCard').style.display = 'none';
      $('resultsCard').classList.remove('visible');
      breakdownVisible = false;
      $('calcBreakdown').classList.remove('visible');
      $('toggleFormula').textContent = '⊕ Show Full Breakdown';
    });
  </script>
</body>
</html>
