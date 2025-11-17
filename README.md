# index.html-<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Simple Compound Interest Calculator</title>
  <style>
    body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, Arial; max-width: 720px; margin: 2rem auto; padding: 1rem; line-height:1.5; }
    .card { border: 1px solid #ddd; padding: 1rem; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); }
    label { display:block; margin-top: 0.75rem; }
    input[type="number"] { width: 100%; padding: 0.5rem; margin-top: 0.25rem; box-sizing: border-box; }
    button { margin-top: 1rem; padding: 0.6rem 1rem; }
    .result { margin-top: 1rem; font-weight:600; }
    small { color:#666; }
  </style>
</head>
<body>
  <h1>Compound Interest Calculator</h1>
  <div class="card">
    <p>Calculate final amount (A) and total interest for compound interest:</p>
    <form id="calcForm" onsubmit="return false;">
      <label>
        Principal (P)
        <input id="principal" type="number" step="0.01" min="0" value="1000" required>
      </label>
      <label>
        Annual interest rate (%) (r)
        <input id="rate" type="number" step="0.01" min="0" value="5" required>
      </label>
      <label>
        Times compounded per year (n)
        <input id="n" type="number" step="1" min="1" value="12" required>
        <small>e.g., 12 for monthly, 4 for quarterly, 1 for yearly</small>
      </label>
      <label>
        Number of years (t)
        <input id="years" type="number" step="0.1" min="0" value="10" required>
      </label>

      <button id="calcBtn">Calculate</button>
      <button id="resetBtn" type="button">Reset</button>
    </form>

    <div id="output" class="result" aria-live="polite"></div>
  </div>

  <script>
    function formatCurrency(x) {
      try {
        return new Intl.NumberFormat(undefined, {style:'currency', currency:'USD', maximumFractionDigits:2}).format(x);
      } catch (e) {
        return '$' + Number(x).toFixed(2);
      }
    }

    function calculate() {
      const P = parseFloat(document.getElementById('principal').value) || 0;
      const rPercent = parseFloat(document.getElementById('rate').value) || 0;
      const n = parseFloat(document.getElementById('n').value) || 1;
      const t = parseFloat(document.getElementById('years').value) || 0;

      const r = rPercent / 100;
      const A = P * Math.pow(1 + r / n, n * t);
      const interest = A - P;

      const out = document.getElementById('output');
      out.innerHTML = `
        Final amount (A): <strong>${formatCurrency(A)}</strong><br>
        Total interest earned: <strong>${formatCurrency(interest)}</strong><br>
        <small>Using P = ${formatCurrency(P)}, r = ${rPercent}%, n = ${n}, t = ${t} years</small>
      `;
    }

    document.getElementById('calcBtn').addEventListener('click', calculate);
    document.getElementById('resetBtn').addEventListener('click', () => {
      document.getElementById('calcForm').reset();
      document.getElementById('output').textContent = '';
    });

    document.getElementById('calcForm').addEventListener('submit', (e) => { e.preventDefault(); calculate(); });
  </script>
</body>
</html>
