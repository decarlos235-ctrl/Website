# Website
# Create the project files (paste all of this into the Gitpod terminal)
cat > index.html <<'HTML'
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>DoItAllHalls — Agreement & Signature (Single-file)</title>
  <meta name="description" content="Agreement and signature capture for DoItAllHalls — single HTML to upload to static hosting." />
  <style>
    :root{
      --bg:#f6f8fb;--card:#fff;--accent:#2463ff;--muted:#6b7280;--radius:10px;
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;color:#111;background:var(--bg)}
    .container{max-width:980px;margin:28px auto;padding:20px}
    h1{margin:0 0 10px;font-size:1.5rem}
    p.lead{margin:0 0 18px;color:var(--muted)}
    .field-row{margin-bottom:12px;display:flex;flex-direction:column}
    label{font-size:0.9rem;margin-bottom:6px;color:var(--muted)}
    input[type="text"],input[type="email"]{padding:10px;border:1px solid #e6e9ef;border-radius:8px;font-size:1rem}
    .signature-card{background:var(--card);padding:16px;border-radius:var(--radius);box-shadow:0 6px 18px rgba(17,24,39,0.06);margin:16px 0}
    .instructions{margin:0 0 12px;color:var(--muted);font-size:0.95rem}
    .toolbar{display:flex;gap:8px;align-items:center;margin-bottom:10px;flex-wrap:wrap}
    .toolbar label{display:flex;align-items:center;gap:6px;color:var(--muted);font-size:0.95rem}
    .canvas-wrap{width:100%;height:260px;border:1px dashed #d6dbe8;border-radius:8px;overflow:hidden;background:#fff;display:flex;align-items:center;justify-content:center}
    canvas#signature-pad{width:100%;height:100%;display:block;touch-action:none;background:transparent;cursor:crosshair}
    button{padding:8px 12px;border-radius:8px;border:1px solid #dbe5ff;background:#fff;color:#111;cursor:pointer}
    button.primary{background:var(--accent);color:#fff;border-color:var(--accent);padding:10px 14px}
    button.pay{background:#0b8; color:#fff; border-color:#0b8}
    .deposit-row{display:flex;align-items:center;justify-content:space-between;margin-top:12px;gap:10px}
    .checkbox{display:flex;gap:8px;align-items:center;color:var(--muted)}
    .form-actions{margin-top:14px;display:flex;gap:8px;flex-wrap:wrap}
    .note{margin-top:18px;color:var(--muted);font-size:0.9rem}
    .small{font-size:0.9rem;color:var(--muted)}
    .meta-area{margin-top:12px;padding:12px;background:#fbfdff;border-radius:8px;border:1px solid #eef5ff;color:var(--muted)}
    details{margin-top:14px;background:#fff;padding:12px;border-radius:8px;border:1px solid #eef5ff}
    summary{font-weight:600;cursor:pointer}
    pre{white-space:pre-wrap;background:#0f1720;color:#ecfeff;padding:12px;border-radius:6px;overflow:auto}
    code{background:#eef6ff;padding:2px 6px;border-radius:4px}
    @media (max-width:520px){ .canvas-wrap{height:180px} .toolbar{gap:6px} }
  </style>
</head>
<body>
  <main class="container">
    <h1>DoItAllHalls — Agreement & Signature</h1>
    <p class="lead">Single-file site ready to paste into Namecheap static hosting (public_html/index.html). It is pre-configured to use api.doitallhalls.com for the backend and doitallhalls.com/pay-deposit for payments. Below the app you'll find embedded deployment instructions and downloadable example server files.</p>

    <form id="agreementForm" novalidate>
      <div class="field-row">
        <label for="name">Full name</label>
        <input id="name" name="name" required placeholder="Your full name" />
      </div>

      <div class="field-row">
        <label for="email">Email</label>
        <input id="email" name="email" type="email" required placeholder="you@doitallhalls.com" />
      </div>

      <section class="signature-card">
        <p class="instructions">Sign inside the box. Tools: color/width, undo, clear, preview, download, save locally, or submit to the API.</p>

        <div class="toolbar">
          <label>Color:
            <input type="color" id="penColor" value="#000000" />
          </label>
          <label>Width:
            <input type="range" id="penWidth" min="1" max="8" value="2" />
          </label>
          <button type="button" id="undoBtn" title="Undo last action">Undo</button>
          <button type="button" id="clearBtn">Clear</button>
          <button type="button" id="downloadBtn">Download PNG</button>
          <button type="button" id="previewBtn">Preview</button>
        </div>

        <div class="canvas-wrap">
          <canvas id="signature-pad" aria-label="Signature pad"></canvas>
        </div>

        <div class="deposit-row">
          <label class="checkbox">
            <input id="depositConfirm" type="checkbox" />
            I understand a deposit is required to secure scheduling.
          </label>
          <div>
            <button type="button" id="payDepositBtn" class="pay">Pay Deposit</button>
          </div>
        </div>
      </section>

      <input type="hidden" id="signatureData" name="signatureData" />

      <div class="form-actions">
        <button id="submitBtn" type="button" class="primary">Submit Agreement</button>
        <button id="saveLocalBtn" type="button">Save Locally (PNG + metadata)</button>
        <button id="downloadServerFilesBtn" type="button">Download Server Files</button>
      </div>
    </form>

    <div class="meta-area">
      <div class="small">Client expects the API at: <code id="endpointDisplay"></code></div>
      <div class="small" style="margin-top:6px;">If you want everything on one domain, you can also host both frontend and API on the same host and use nginx to proxy /api to the Node/Python process — instructions are below.</div>
    </div>

    <details open>
      <summary>Deployment & DNS instructions (Namecheap + recommended hosting)</summary>

      <h4>1) Static site (front-end) — Namecheap shared/static hosting</h4>
      <ol>
        <li>Login to Namecheap → Domain List → Manage for doitallhalls.com → Hosting (cPanel) → File Manager → public_html.</li>
        <li>Upload this file as index.html into public_html (this file).</li>
        <li>Alternatively use FTP/SFTP: supply Namecheap FTP credentials and upload index.html to public_html.</li>
      </ol>

      <h4>2) Backend (recommended): managed service (Render / Railway) on subdomain api.doitallhalls.com</h4>
      <p>Why: managed apps provide automatic HTTPS, auto-redeploy, and are easiest for small apps. Steps (Render example):</p>
      <ol>
        <li>Create a new Web Service on Render; point it at a repo containing the Node server (server/node/app.js) or paste code in a new service.</li>
        <li>Set the start command: <code>node app.js</code>. Ensure app listens on port from <code>process.env.PORT</code>.</li>
        <li>In Render dashboard -> Custom Domains add <code>api.doitallhalls.com</code>. Render will show a CNAME target.</li>
        <li>On Namecheap → Advanced DNS add a CNAME: Host = <code>api</code>, Value = Render-provided target. (If using Railway/others, follow their custom domain instructions.)</li>
        <li>Render will provision TLS automatically. Client will now POST to <code>https://api.doitallhalls.com/api/submit-agreement</code>.</li>
      </ol>

      <h4>3) Backend alternative: VPS (DigitalOcean) + nginx + Let's Encrypt</h4>
      <p>High-level steps (DigitalOcean droplet Ubuntu):</p>
      <pre>
# on server (Ubuntu)
sudo apt update && sudo apt install -y nginx certbot python3-certbot-nginx
# clone server
git clone &lt;your-repo&gt; /var/www/signature-server
cd /var/www/signature-server/server/node
npm install
# run with pm2
sudo npm install -g pm2
pm2 start app.js --name signature-server
pm2 save

# nginx config (example)
sudo tee /etc/nginx/sites-available/doitallhalls <<'NGINX'
server {
  listen 80;
  server_name doitallhalls.com www.doitallhalls.com;

  root /var/www/html; # or where your static index.html is
  index index.html;

  location / {
    try_files $uri $uri/ =404;
  }

  location /api/ {
    proxy_pass http://127.0.0.1:3000/;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}
NGINX

sudo ln -s /etc/nginx/sites-available/doitallhalls /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx

# obtain TLS cert
sudo certbot --nginx -d doitallhalls.com -d api.doitallhalls.com
      </pre>

      <h4>4) DNS records on Namecheap</h4>
      <ul>
        <li>For frontend on Namecheap hosting: Namecheap will already point the domain to cPanel. If using external static host (Netlify, Vercel), follow their domain instructions.</li>
        <li>For API on Render: add CNAME host <code>api</code> → Render target. For VPS: create <code>A</code> record <code>api</code> → droplet IP. Also set A record for root (@) → droplet IP if serving front & API together.</li>
        <li>Set TTL as default. Wait up to 10-60 minutes for DNS propagation.</li>
      </ul>

      <h4>5) Payment (Pay Deposit) — options</h4>
      <ol>
        <li>Simple: point PAY button to a hosted payment page (Stripe Checkout session URL, PayPal link, etc.). We set a placeholder to <code>https://doitallhalls.com/pay-deposit</code> — replace with your provider's checkout URL.</li>
        <li>Stripe Checkout (recommended): create Checkout Session server-side, return session URL to the client, redirect user. If you want, I can add Stripe server code (server-side) to create sessions securely.</li>
      </ol>

      <h4>6) Testing & CORS</h4>
      <p>If the frontend is on doitallhalls.com and the API on api.doitallhalls.com, the server must allow CORS. Example Node/Flask examples provided enable CORS for testing. After deployment, test with curl or the site (open browser, sign & Submit).</p>

      <h4>7) Security notes</h4>
      <ul>
        <li>Authenticate or rate-limit the API if public.</li>
        <li>Sanitize inputs and filenames (examples include simple sanitization).</li>
        <li>Store images in durable storage (S3) for production, and use SSL.</li>
      </ul>
    </details>

    <details>
      <summary>Included example server files (downloadable)</summary>
      <p>Click "Download Server Files" below to download Node and Python example server files and a README. Use them as a starting point.</p>
      <p>Node example listens on <code>/api/submit-agreement</code> and saves PNG + metadata to a signatures folder. Flask example does the same.</p>
    </details>

  </main>

  <script>
/* =========================
   Configuration for doitallhalls.com
   ========================= */
// API endpoint for the deployed backend (change if you host differently)
const ENDPOINT = 'https://api.doitallhalls.com/api/submit-agreement';
// Payment page or checkout link (replace with your Stripe/PayPal checkout URL)
const PAYMENT_LINK = 'https://doitallhalls.com/pay-deposit';

document.getElementById('endpointDisplay').textContent = ENDPOINT;

/* =========================
   Signature pad client logic
   ========================= */
const canvas = document.getElementById('signature-pad');
const ctx = canvas.getContext('2d');

const penColorInput = document.getElementById('penColor');
const penWidthInput = document.getElementById('penWidth');
const clearBtn = document.getElementById('clearBtn');
const downloadBtn = document.getElementById('downloadBtn');
const previewBtn = document.getElementById('previewBtn');
const undoBtn = document.getElementById('undoBtn');
const submitBtn = document.getElementById('submitBtn');
const payDepositBtn = document.getElementById('payDepositBtn');
const depositConfirm = document.getElementById('depositConfirm');
const signatureDataInput = document.getElementById('signatureData');
const saveLocalBtn = document.getElementById('saveLocalBtn');
const downloadServerFilesBtn = document.getElementById('downloadServerFilesBtn');

let drawing = false;
let last = { x: 0, y: 0 };
let strokes = [];

function resizeCanvas() {
  const ratio = Math.max(window.devicePixelRatio || 1, 1);
  const temp = document.createElement('canvas');
  temp.width = canvas.width || 1;
  temp.height = canvas.height || 1;
  temp.getContext('2d').drawImage(canvas, 0, 0);

  const cssWidth = canvas.clientWidth || canvas.offsetWidth || 600;
  const cssHeight = canvas.clientHeight || canvas.offsetHeight || 240;

  canvas.width = Math.floor(cssWidth * ratio);
  canvas.height = Math.floor(cssHeight * ratio);
  canvas.style.width = cssWidth + 'px';
  canvas.style.height = cssHeight + 'px';

  ctx.setTransform(1, 0, 0, 1, 0, 0);
  ctx.scale(ratio, ratio);

  if (temp.width && temp.height) ctx.drawImage(temp, 0, 0, temp.width / ratio, temp.height / ratio);
}
window.addEventListener('resize', resizeCanvas);
resizeCanvas();

function saveSnapshot() {
  try {
    strokes.push(canvas.toDataURL('image/png'));
    if (strokes.length > 40) strokes.shift();
  } catch (e) { console.warn('snapshot failed', e); }
}

function undo() {
  if (!strokes.length) { clearSignature(false); return; }
  const lastData = strokes.pop();
  const img = new Image();
  img.onload = () => {
    ctx.setTransform(1,0,0,1,0,0);
    ctx.clearRect(0,0,canvas.width,canvas.height);
    resizeCanvas();
    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
    ctx.setTransform(1,0,0,1,0,0);
    resizeCanvas();
  };
  img.src = lastData;
}
undoBtn.addEventListener('click', undo);

function pointerDown(e) {
  drawing = true;
  saveSnapshot();
  canvas.setPointerCapture && canvas.setPointerCapture(e.pointerId);
  const rect = canvas.getBoundingClientRect();
  last.x = e.clientX - rect.left;
  last.y = e.clientY - rect.top;
}

function pointerUp(e) {
  if (drawing) {
    drawing = false;
    try { canvas.releasePointerCapture && canvas.releasePointerCapture(e.pointerId); } catch {}
    ctx.beginPath();
  }
}

function pointerMove(e) {
  if (!drawing) return;
  const rect = canvas.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;

  ctx.lineWidth = Number(penWidthInput.value) || 2;
  ctx.lineCap = 'round';
  ctx.strokeStyle = penColorInput.value || '#000';

  ctx.beginPath();
  ctx.moveTo(last.x, last.y);
  ctx.lineTo(x, y);
  ctx.stroke();

  last.x = x;
  last.y = y;
}

canvas.addEventListener('pointerdown', pointerDown);
canvas.addEventListener('pointerup', pointerUp);
canvas.addEventListener('pointercancel', pointerUp);
canvas.addEventListener('pointermove', pointerMove);

function clearSignature(saveSnapshotBeforeClear = true) {
  if (saveSnapshotBeforeClear) saveSnapshot();
  ctx.setTransform(1, 0, 0, 1, 0, 0);
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  resizeCanvas();
}
clearBtn.addEventListener('click', () => clearSignature(true));

function downloadSignature(filename = 'signature.png') {
  const exportCanvas = document.createElement('canvas');
  exportCanvas.width = canvas.width;
  exportCanvas.height = canvas.height;
  const exportCtx = exportCanvas.getContext('2d');
  exportCtx.fillStyle = "#fff";
  exportCtx.fillRect(0, 0, exportCanvas.width, exportCanvas.height);
  exportCtx.drawImage(canvas, 0, 0, exportCanvas.width, exportCanvas.height);
  const dataURL = exportCanvas.toDataURL('image/png');
  const a = document.createElement('a');
  a.href = dataURL;
  a.download = filename;
  document.body.appendChild(a);
  a.click();
  a.remove();
}
downloadBtn.addEventListener('click', () => downloadSignature('signature.png'));

function previewSignature() {
  const dataURL = canvas.toDataURL('image/png');
  const win = window.open();
  if (!win) { alert('Popup blocked — please allow popups to preview the signature.'); return; }
  const img = new Image();
  img.src = dataURL;
  img.style.maxWidth = '100%';
  img.onload = () => {
    win.document.body.style.margin = '0';
    win.document.body.style.display = 'flex';
    win.document.body.style.justifyContent = 'center';
    win.document.body.style.alignItems = 'center';
    win.document.body.appendChild(img);
  };
}
previewBtn.addEventListener('click', previewSignature);

payDepositBtn.addEventListener('click', () => {
  window.open(PAYMENT_LINK, '_blank');
});

function hasInk() {
  try {
    const checkCanvas = document.createElement('canvas');
    checkCanvas.width = 16;
    checkCanvas.height = 16;
    const checkCtx = checkCanvas.getContext('2d');
    checkCtx.drawImage(canvas, 0, 0, 16, 16);
    const pixels = checkCtx.getImageData(0, 0, 16, 16).data;
    for (let i = 0; i < pixels.length; i += 4) {
      if (pixels[i+3] > 10 && !(pixels[i] > 250 && pixels[i+1] > 250 && pixels[i+2] > 250)) return true;
    }
  } catch (e) { console.warn('ink check failed', e); }
  return false;
}

async function submitAgreement() {
  const name = document.getElementById('name').value.trim();
  const email = document.getElementById('email').value.trim();
  const depositChecked = depositConfirm.checked;

  if (!name) { alert('Please enter your full name.'); return; }
  if (!email) { alert('Please enter your email.'); return; }
  if (!depositChecked) { alert('Please confirm the deposit requirement before submitting.'); return; }
  if (!hasInk()) { alert('Please provide a signature before submitting.'); return; }

  const dataURL = canvas.toDataURL('image/png');
  signatureDataInput.value = dataURL;

  submitBtn.disabled = true;
  submitBtn.textContent = 'Submitting...';

  try {
    const resp = await fetch(ENDPOINT, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        name,
        email,
        signatureData: dataURL,
        depositConfirmed: depositChecked,
        userAgent: navigator.userAgent
      })
    });

    if (!resp.ok) {
      const text = await resp.text();
      throw new Error(text || `Server error: ${resp.status}`);
    }

    const result = await resp.json();
    alert(result.message || 'Agreement submitted successfully.');

    if (result.savedPath) {
      if (confirm('Open saved signature file on server (if accessible)?')) {
        window.open(result.savedPath, '_blank');
      }
    }
  } catch (err) {
    console.error(err);
    alert('Submission failed: ' + (err.message || err) +
      '\n\nIf you do not have a server running, use "Save Locally (PNG + metadata)" or "Download Server Files" to run a local example server.');
  } finally {
    submitBtn.disabled = false;
    submitBtn.textContent = 'Submit Agreement';
  }
}
submitBtn.addEventListener('click', submitAgreement);

// Save locally (PNG + JSON)
function saveLocally() {
  const name = document.getElementById('name').value.trim();
  const email = document.getElementById('email').value.trim();
  if (!name || !email) { alert('Please fill name and email before saving locally.'); return; }
  if (!hasInk()) { alert('Please provide a signature before saving locally.'); return; }

  const exportCanvas = document.createElement('canvas');
  exportCanvas.width = canvas.width;
  exportCanvas.height = canvas.height;
  const exportCtx = exportCanvas.getContext('2d');
  exportCtx.fillStyle = "#fff";
  exportCtx.fillRect(0, 0, exportCanvas.width, exportCanvas.height);
  exportCtx.drawImage(canvas, 0, 0, exportCanvas.width, exportCanvas.height);
  const dataURL = exportCanvas.toDataURL('image/png');

  const now = new Date().toISOString().replace(/[:.]/g, '-');
  const safeName = (name || 'user').replace(/[^a-z0-9-_]/gi, '_').slice(0, 40);
  const pngFilename = `${now}-${safeName}.png`;
  const jsonFilename = `${now}-${safeName}.json`;

  // download PNG
  const a = document.createElement('a');
  a.href = dataURL;
  a.download = pngFilename;
  document.body.appendChild(a);
  a.click();
  a.remove();

  // download metadata
  const meta = { name, email, depositConfirmed: !!depositConfirm.checked, userAgent: navigator.userAgent, savedAt: new Date().toISOString(), file: pngFilename };
  const blob = new Blob([JSON.stringify(meta, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const b = document.createElement('a');
  b.href = url;
  b.download = jsonFilename;
  document.body.appendChild(b);
  b.click();
  b.remove();
  URL.revokeObjectURL(url);
}
saveLocalBtn.addEventListener('click', saveLocally);

/* =========================
   Server example files (downloadable)
   =========================
   These are starter files: Node (Express) and Python (Flask), plus nginx example and Stripe notes.
*/
const SERVER_FILES = {
  "server_node_package.json": `{
  "name": "signature-agreement-server",
  "version": "1.0.0",
  "description": "Example server for receiving signature DataURLs and saving PNGs",
  "main": "app.js",
  "scripts": { "start": "node app.js" },
  "dependencies": { "cors": "^2.8.5", "express": "^4.18.2", "mkdirp": "^1.0.4" }
}
`,
  "server_node_app.js": `// Node/Express server (save as server/node/app.js)
const express = require('express');
const fs = require('fs');
const path = require('path');
const mkdirp = require('mkdirp');
const cors = require('cors');

const app = express();
app.use(cors());
app.use(express.json({ limit: '15mb' }));

const PORT = process.env.PORT || 3000;
const STORAGE_DIR = path.join(__dirname, 'signatures');
mkdirp.sync(STORAGE_DIR);

function saveDataUrl(dataUrl, filename) {
  const matches = dataUrl.match(/^data:(.+);base64,(.+)$/);
  if (!matches) throw new Error('Invalid data URL');
  const base64Data = matches[2];
  const buffer = Buffer.from(base64Data, 'base64');
  fs.writeFileSync(filename, buffer);
  return { size: buffer.length };
}

app.post('/api/submit-agreement', (req, res) => {
  try {
    const { name, email, signatureData, depositConfirmed, userAgent } = req.body || {};
    if (!name || !email || !signatureData) return res.status(400).json({ message: 'Missing name, email, or signatureData' });

    const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
    const safeName = (name || 'unknown').replace(/[^a-z0-9-_]/gi, '_').slice(0, 40);
    const pngFilename = \`\${timestamp}-\${safeName}.png\`;
    const jsonFilename = \`\${timestamp}-\${safeName}.json\`;

    const pngPath = path.join(STORAGE_DIR, pngFilename);
    const jsonPath = path.join(STORAGE_DIR, jsonFilename);

    saveDataUrl(signatureData, pngPath);

    const meta = { name, email, depositConfirmed: !!depositConfirmed, userAgent: userAgent || '', savedAt: new Date().toISOString(), file: pngFilename };
    fs.writeFileSync(jsonPath, JSON.stringify(meta, null, 2), 'utf8');

    res.json({ message: 'Saved signature', savedPath: \`/signatures/\${pngFilename}\`, meta });
  } catch (err) {
    console.error('Error:', err);
    res.status(500).json({ message: 'Server error', error: String(err) });
  }
});

app.use('/signatures', express.static(STORAGE_DIR));
app.listen(PORT, () => console.log(\`Server listening on http://localhost:\${PORT}\`));
`,
  "server_python_requirements.txt": `Flask>=2.2
flask-cors>=3.0
`,
  "server_python_app.py": `# Flask server (save as server/python/app.py)
import re, base64, json
from datetime import datetime
from pathlib import Path
from flask import Flask, request, jsonify, send_from_directory
from flask_cors import CORS

app = Flask(__name__)
CORS(app)

STORAGE_DIR = Path(__file__).resolve().parent / "signatures"
STORAGE_DIR.mkdir(parents=True, exist_ok=True)

def save_data_url(data_url, out_path):
    import re, base64
    m = re.match(r"^data:(.+?);base64,(.+)$", data_url)
    if not m: raise ValueError("Invalid data URL")
    data = base64.b64decode(m.group(2))
    out_path.write_bytes(data)
    return len(data)

@app.route('/api/submit-agreement', methods=['POST'])
def submit_agreement():
    payload = request.get_json(force=True)
    name = payload.get('name'); email = payload.get('email'); signature_data = payload.get('signatureData')
    if not name or not email or not signature_data: return jsonify({'message': 'Missing name, email, or signatureData'}), 400
    timestamp = datetime.utcnow().isoformat().replace(':', '-').replace('.', '-')
    safe = re.sub(r'[^a-zA-Z0-9_-]', '_', name)[:40]
    png_filename = f"{timestamp}-{safe}.png"
    png_path = STORAGE_DIR / png_filename
    save_data_url(signature_data, png_path)
    meta = {'name': name, 'email': email, 'savedAt': datetime.utcnow().isoformat(), 'file': png_filename}
    (STORAGE_DIR / f"{timestamp}-{safe}.json").write_text(json.dumps(meta, indent=2))
    return jsonify({'message': 'Saved signature', 'savedPath': f'/signatures/{png_filename}', 'meta': meta})

@app.route('/signatures/<path:filename>')
def serve_signature(filename):
    return send_from_directory(str(STORAGE_DIR), filename)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
`,
  "nginx_example.conf": `# nginx snippet (use on your VPS; proxy /api to your Node app listening on 127.0.0.1:3000)
server {
  listen 80;
  server_name doitallhalls.com www.doitallhalls.com api.doitallhalls.com;

  root /var/www/html; # static site location for front-end index.html
  index index.html;

  location / {
    try_files $uri $uri/ =404;
  }

  location /api/ {
    proxy_pass http://127.0.0.1:3000/;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_http_version 1.1;
    proxy_set_header Connection "";
  }
}
`,
  "stripe_quick_notes.txt": `Stripe Checkout quick notes:
1) Create a Stripe account and get your secret key.
2) Server: create an endpoint that calls stripe.checkout.sessions.create(...) with amount/currency and success/cancel URLs.
3) Return session.url to the client and redirect the user to it.
4) Securely verify any webhooks on your server to confirm payment and mark depositConfirmed for the user's record.
I can produce a full server Stripe example on request (Node/Express).`
};

function downloadStringAsFile(filename, content, mime='text/plain') {
  const blob = new Blob([content], { type: mime });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = filename;
  document.body.appendChild(a);
  a.click();
  a.remove();
  setTimeout(() => URL.revokeObjectURL(url), 1500);
}

downloadServerFilesBtn.addEventListener('click', () => {
  // download the example files (flat file names - move into folders after download)
  downloadStringAsFile('server_node_package.json', SERVER_FILES.server_node_package, 'application/json');
  downloadStringAsFile('server_node_app.js', SERVER_FILES.server_node_app, 'application/javascript');
  downloadStringAsFile('server_python_requirements.txt', SERVER_FILES.server_python_requirements, 'text/plain');
  downloadStringAsFile('server_python_app.py', SERVER_FILES.server_python_app, 'text/x-python');
  downloadStringAsFile('nginx_example.conf', SERVER_FILES.nginx_example, 'text/plain');
  downloadStringAsFile('stripe_quick_notes.txt', SERVER_FILES.stripe_quick_notes, 'text/plain');
  alert('Example server files downloaded. Move them into server/node and server/python folders and follow the README-like instructions provided in the Deployment section.');
});

/* Avoid errors in some browsers if keys missing (legacy fallback) */
if (!SERVER_FILES.server_node_package) {
  SERVER_FILES.server_node_package = SERVER_FILES["server_node_package.json"];
  SERVER_FILES.server_node_app = SERVER_FILES["server_node_app.js"];
  SERVER_FILES.server_python_requirements = SERVER_FILES["server_python_requirements.txt"];
  SERVER_FILES.server_python_app = SERVER_FILES["server_python_app.py"];
  SERVER_FILES.nginx_example = SERVER_FILES["nginx_example.conf"];
  SERVER_FILES.stripe_quick_notes = SERVER_FILES["stripe_quick_notes.txt"];
}
  </script>
</body>
</html>
HTML

cat > package.json <<'JSON'
{
  "name": "doitallhalls-pdf-generator",
  "version": "1.0.0",
  "description": "Generate a PDF from the single-file DoItAllHalls HTML using Puppeteer",
  "main": "generate-pdf.js",
  "scripts": {
    "generate-pdf": "node generate-pdf.js index.html doitallhalls.pdf"
  },
  "dependencies": {
    "puppeteer": "^20.8.0"
  },
  "author": "",
  "license": "MIT"
}
JSON

cat > generate-pdf.js <<'JS'
const fs = require('fs');
const path = require('path');
const puppeteer = require('puppeteer');

(async () => {
  const htmlPath = process.argv[2] || 'index.html';
  const pdfPath = process.argv[3] || 'doitallhalls.pdf';

  if (!fs.existsSync(htmlPath)) {
    console.error(`HTML file not found: ${htmlPath}`);
    process.exit(1);
  }

  const html = fs.readFileSync(htmlPath, 'utf8');

  const browser = await puppeteer.launch({
    args: ['--no-sandbox', '--disable-setuid-sandbox']
  });

  try {
    const page = await browser.newPage();
    await page.setViewport({ width: 1200, height: 800 });
    await page.setContent(html, { waitUntil: 'networkidle0' });
    await page.waitForTimeout(300);
    await page.pdf({
      path: pdfPath,
      format: 'A4',
      printBackground: true,
      margin: { top: '12mm', bottom: '12mm', left: '10mm', right: '10mm' }
    });
    console.log(`Saved PDF: ${path.resolve(pdfPath)}`);
  } catch (err) {
    console.error('Error creating PDF:', err);
  } finally {
    await browser.close();
  }
})();
JS

cat > README.md <<'MD'
# DoItAllHalls — PDF generator package

Files included:
- index.html (single-file website)
- package.json
- generate-pdf.js

How to create the PDF (quick):
1. Save the three files into a folder.
2. Open a terminal in that folder.
3. Run:
   npm install
   npm run generate-pdf

This will produce doitallhalls.pdf in the same folder.

Notes:
- Puppeteer will download a Chromium build during npm install. The download size can be large (~100MB+).
- If you prefer using an existing Chrome/Chromium, modify `generate-pdf.js` to launch puppeteer with `executablePath` pointing to your Chrome binary.
- You can also open index.html in Chrome and print to PDF manually (Ctrl/Cmd+P → Save as PDF).
MD

echo "All files created. Run 'npm install' and then 'npm run generate-pdf' to produce doitallhalls.pdf"
