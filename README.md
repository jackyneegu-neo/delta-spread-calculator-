
    <div class="section">
      <h2>Global</h2>
      <div class="grid">
        <div class="field">
          <label for="Total_trades">Total number of trades (pairs)</label>
          <input id="Total_trades" type="number" placeholder="e.g. 10">
        </div>
      </div>
    </div>

    <!-- Long inputs -->
    <div class="section">
      <h2>Long Leg</h2>
      <div class="grid">
        <div class="field">
          <label for="L_entry">Long entry price (optional)</label>
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
          <label for="L_SL_count">Long stop loss hits (count)</label>
          <input id="L_SL_count" type="number" placeholder="e.g. 3">
        </div>
        <div class="field">
          <label for="L_TP_count">Long target hits (auto)</label>
          <input id="L_TP_count" type="number" placeholder="auto" readonly>
        </div>
      </div>
    </div>

    <!-- Short inputs -->
    <div class="section">
      <h2>Short Leg</h2>
      <div class="grid">
        <div class="field">
          <label for="S_entry">Short entry price (optional)</label>
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
          <label for="S_SL_count">Short stop loss hits (auto)</label>
          <input id="S_SL_count" type="number" placeholder="auto" readonly>
        </div>
        <div class="field">
          <label for="S_TP_count">Short target hits (auto)</label>
          <input id="S_TP_count" type="number" placeholder="auto" readonly>
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
        <span class="label">Long leg P&amp;L</span>
        <span class="value" id="L_total"></span>
      </div>
      <div class="output-row">
        <span class="label">Short leg P&amp;L</span>
        <span class="value" id="S_total"></span>
      </div>
      <div class="output-row">
        <span class="label">Net combined P&amp;L</span>
        <span class="value" id="Net_total"></span>
      </div>
    </div>
