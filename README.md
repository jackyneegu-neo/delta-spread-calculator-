<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Long–Short P&L Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #0f172a;
      color: #e5e7eb;
      margin: 0;
      padding: 20px;
    }
    .container {
      max-width: 900px;
      margin: 0 auto;
      background: #111827;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 20px rgba(0,0,0,0.4);
    }
    h1 {
      font-size: 22px;
      margin-bottom: 10px;
      color: #facc15;
    }
    h2 {
      font-size: 18px;
      margin-top: 20px;
      margin-bottom: 10px;
      color: #a5b4fc;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 12px 20px;
    }
    .field {
      display: flex;
      flex-direction: column;
      font-size: 14px;
    }
    label {
      margin-bottom: 4px;
      color: #9ca3af;
    }
    input[type="number"] {
      padding: 6px 8px;
      border-radius: 4px;
      border: 1px solid #374151;
      background: #020617;
      color: #e5e7eb;
    }
    input[type="number"]::placeholder {
      color: #4b5563;
    }
    .section {
      margin-top: 20px;
      padding-top: 10px;
      border-top: 1px solid #1f2937;
    }
    .button-row {
      margin-top: 20px;
      display: flex;
      gap: 10px;
    }
    button {
      padding: 8px 16px;
      border-radius: 4px;
      border: none;
      cursor: pointer;
      font-size: 14px;
      font-weight: 600;
    }
    #calcBtn {
      background: #22c55e;
      color: #022c22;
    }
    #resetBtn {
      background: #4b5563;
      color: #e5e7eb;
    }
    .output-box {
      margin-top: 20px;
      padding: 12px;
      border-radius: 6px;
      background: #020617;
      border: 1px solid #1f2937;
      font-size: 14px;
      line-height: 1.4;
    }
    .output-row {
      display: flex;
      justify-content: space-between;
      margin-bottom: 4px;
    }
    .label {
      color: #9ca3af;
    }
    .value {
      font-weight: 600;
      color: #f9fafb;
    }
    .net-positive {
      color: #4ade80;
    }
    .net-negative {
      color: #f97373;
    }
    @media (max-width: 700px) {
      .grid {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Long–Short Strategy Calculator</h1>

    <!-- Long inputs -->
    <div class="section">
      <h2>Long Leg</h2>
      <div class="grid">
        <div class="field">
          <label for="L_entry">Long entry price</label>
          <input id="L_entry" type="number" placeholder="e.g. 275000">
        </div>
        <div class="field">
          <label for="L_SL_pts">Long SL (points)</label>
          <input id="L_SL_pts" type="number" placeholder="e.g. 4000">
        </div>
        <div class="field">
          <label for="L_TP_pts">Long TP (points)</label>
          <input id="L_TP_pts" type="number" placeholder="e.g. 6000">
        </div>
        <div class="field">
          <label for="L_TP_count">Long target hit count</label>
          <input id="L_TP_count" type="number" placeholder="e.g. 10">
        </div>
        <div class="field">
          <label for="L_SL_count">Long stop hit count</label>
          <input id="L_SL_count" type="number" placeholder="e.g. 5">
        </div>
      </div>
    </div>

    <!-- Short inputs -->
    <div class="section">
      <h2>Short Leg</h2>
      <div class="grid">
        <div class="field">
          <label for="S_entry">Short entry price</label>
          <input id="S_entry" type="number" placeholder="e.g. 276000">
        </div>
        <div class="field">
          <label for="S_SL_pts">Short SL (points)</label>
          <input id="S_SL_pts" type="number" placeholder="e.g. 4000">
        </div>
        <div class="field">
          <label for="S_TP_pts">Short TP (points)</label>
          <input id="S_TP_pts" type="number" placeholder="e.g. 6000">
        </div>
        <div class="field">
          <label for="S_TP_count">Short target hit count</label>
          <input id="S_TP_count" type="number" placeholder="e.g. 10">
        </div>
        <div class="field">
          <label for="S_SL_count">Short stop hit count</label>
          <input id="S_SL_count" type="number" placeholder="e.g. 5">
        </div>
      </div>
    </div>

    <!-- Gap inputs -->
    <div class="section">
      <h2>Gap (optional)</h2>
      <div class="grid">
        <div class="field">
          <label for="GapUp_pts">Avg gap up (pts)</label>
          <input id="GapUp_pts" type="number" value="0">
        </div>
        <div class="field">
          <label for="GapUp_count">Gap up days</label>
          <input id="GapUp_count" type="number" value="0">
        </div>
        <div class="field">
          <label for="GapDn_pts">Avg gap down (pts)</label>
          <input id="GapDn_pts" type="number" value="0">
        </div>
        <div class="field">
          <label for="GapDn_count">Gap down days</label>
          <input id="GapDn_count" type="number" value="0">
        </div>
      </div>
    </div>

    <!-- Buttons -->
    <div class="button-row">
      <button id="calcBtn">Calculate</button>
      <button id="resetBtn" type="button">Reset</button>
    </div>

    <!-- Outputs -->
    <div class="output-box" id="outputBox" style="display:none;">
      <div class="output-row">
        <span class="label">Long leg P&amp;L (incl. gaps)</span>
        <span class="value" id="L_total"></span>
      </div>
      <div class="output-row">
        <span class="label">Short leg P&amp;L (incl. gaps)</span>
        <span class="value" id="S_total"></span>
      </div>
      <div class="output-row">
        <span class="label">Net combined P&amp;L</span>
        <span class="value" id="Net_total"></span>
      </div>
    </div>
  </div>

  <script>
    function num(id) {
      const v = document.getElementById(id).value;
      return v === '' ? 0 : parseFloat(v);
    }

    function formatPts(x) {
      return x.toLocaleString('en-IN', { maximumFractionDigits: 2 });
    }

    document.getElementById('calcBtn').addEventListener('click', function () {
      const L_TP_pts   = num('L_TP_pts');
      const L_SL_pts   = num('L_SL_pts');
      const L_TP_count = num('L_TP_count');
      const L_SL_count = num('L_SL_count');

      const S_TP_pts   = num('S_TP_pts');
      const S_SL_pts   = num('S_SL_pts');
      const S_TP_count = num('S_TP_count');
      const S_SL_count = num('S_SL_count');

      const GapUp_pts   = num('GapUp_pts');
      const GapUp_count = num('GapUp_count');
      const GapDn_pts   = num('GapDn_pts');
      const GapDn_count = num('GapDn_count');

      const L_PnL = L_TP_count * L_TP_pts - L_SL_count * L_SL_pts;
      const S_PnL = S_TP_count * S_TP_pts - S_SL_count * S_SL_pts;

      const L_gap_PnL = GapUp_count * GapUp_pts - GapDn_count * GapDn_pts;
      const S_gap_PnL = GapDn_count * GapDn_pts - GapUp_count * GapUp_pts;

      const L_total = L_PnL + L_gap_PnL;
      const S_total = S_PnL + S_gap_PnL;
      const Net_total = L_total + S_total;

      const LEl = document.getElementById('L_total');
      const SEl = document.getElementById('S_total');
      const NEl = document.getElementById('Net_total');

      LEl.textContent = formatPts(L_total) + ' pts';
      SEl.textContent = formatPts(S_total) + ' pts';
      NEl.textContent = formatPts(Net_total) + ' pts';

      NEl.classList.remove('net-positive', 'net-negative');
      if (Net_total > 0) {
        NEl.classList.add('net-positive');
      } else if (Net_total < 0) {
        NEl.classList.add('net-negative');
      }

      document.getElementById('outputBox').style.display = 'block';
    });

    document.getElementById('resetBtn').addEventListener('click', function () {
      document.querySelectorAll('input[type="number"]').forEach(inp => {
        if (inp.id === 'GapUp_pts' || inp.id === 'GapUp_count' ||
            inp.id === 'GapDn_pts' || inp.id === 'GapDn_count') {
          inp.value = 0;
        } else {
          inp.value = '';
        }
      });
      document.getElementById('outputBox').style.display = 'none';
    });
  </script>
</body>
</html>
