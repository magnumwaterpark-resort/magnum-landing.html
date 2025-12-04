<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Magnum Waterpark — Follow & Subscribe</title>

  <!-- Open Graph for nicer previews when this page is shared -->
  <meta property="og:title" content="Magnum Waterpark — Follow & Subscribe" />
  <meta property="og:description" content="Follow Magnum Waterpark on Instagram and subscribe on YouTube." />
  <meta property="og:type" content="website" />

  <style>
    :root{--bg:#0f172a;--card:#0b1220;--accent:#06b6d4;--glass:rgba(255,255,255,0.04)}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{display:grid;place-items:center;background:linear-gradient(180deg,#071029 0%, #042033 100%);color:#e6eef6;padding:24px}
    .card{width:100%;max-width:520px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:16px;padding:28px;box-shadow:0 8px 30px rgba(2,6,23,0.6)}
    header{display:flex;gap:12px;align-items:center}
    header img{width:64px;height:64px;border-radius:10px;object-fit:cover}
    h1{font-size:20px;margin:0}
    p.lead{margin:6px 0 18px;color:rgba(230,238,246,0.85)}

    .links{display:grid;grid-template-columns:1fr 1fr;gap:12px}
    .btn{display:inline-flex;align-items:center;justify-content:center;padding:12px 14px;border-radius:10px;border:0;font-weight:600;cursor:pointer}
    .yt{background:linear-gradient(90deg,#ff4d4d,#ff2a2a);color:#fff}
    .ig{background:linear-gradient(90deg,#f58529,#c13584);color:#fff}

    .qr-area{display:flex;gap:18px;align-items:center;margin-top:18px}
    .qr{background:var(--glass);padding:12px;border-radius:10px}
    .qr canvas, .qr img{display:block}

    .actions{display:flex;gap:10px;margin-top:14px}
    .small{padding:8px 10px;border-radius:8px;background:transparent;border:1px solid rgba(255,255,255,0.06);color:inherit}
    footer{margin-top:18px;font-size:13px;color:rgba(230,238,246,0.65)}

    @media (max-width:520px){.links{grid-template-columns:1fr;}}
  </style>
</head>
<body>
  <div class="card">
    <header>
      <img src="https://raw.githubusercontent.com/magnum-waterpark/logo-placeholder/main/logo.png" alt="Magnum logo" onerror="this.style.display='none'" />
      <div>
        <h1>मॅग्नम वॉटरपार्क — Follow & Subscribe</h1>
        <p class="lead">Open this page and ask people to scan the QR code below — it links to this landing page with big buttons to follow on Instagram and subscribe on YouTube.</p>
      </div>
    </header>

    <div class="links">
      <a id="ytBtn" class="btn yt" href="#" target="_blank" rel="noopener noreferrer">Subscribe on YouTube</a>
      <a id="igBtn" class="btn ig" href="#" target="_blank" rel="noopener noreferrer">Follow on Instagram</a>
    </div>

    <div class="qr-area">
      <div class="qr" id="qrcode"></div>
      <div>
        <div style="font-weight:700;margin-bottom:6px">Scan to open this page</div>
        <div style="font-size:13px;color:rgba(230,238,246,0.8);max-width:230px">When you host this file on a web server (or GitHub Pages / Netlify) the QR code will point to the hosted URL automatically. You can also download the QR as a PNG for printing.</div>
        <div class="actions">
          <button id="downloadBtn" class="small">Download QR</button>
          <button id="copyBtn" class="small">Copy landing URL</button>
        </div>
      </div>
    </div>

    <footer>
      Pro tip: when you upload this file to a public URL (for example https://yourdomain.com/magnum-landing.html) share that URL or print the QR. Links automatically open in the official apps when scanned from phones.
    </footer>
  </div>

  <!-- QRCode library (small, no build step) -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <script>
    // Replace these with the final links provided
    const YT_LINK = 'https://youtube.com/@magnumwaterparkresort?si=d6J8SvXM9NiG7qJ9';
    const IG_LINK = 'https://www.instagram.com/magnumwaterpark/?hl=en';

    // Set up buttons to open links directly (so the page works even if not hosted)
    const ytBtn = document.getElementById('ytBtn');
    const igBtn = document.getElementById('igBtn');
    ytBtn.href = YT_LINK;
    igBtn.href = IG_LINK;

    // Landing URL to encode in the QR: prefer actual hosted URL (window.location.href)
    // If file is opened locally (file://) we fallback to a compact data URL which opens this page's content.
    function getLandingUrl(){
      const loc = window.location.href;
      if(loc.startsWith('http') || loc.startsWith('https')) return loc;
      // fallback: create a tiny redirect using a data URL that opens the page with the two links
      // This works for quick testing, but printing a QR that points to a hosted http(s) URL is recommended.
      const redirectHtml = `<!doctype html><meta charset=\"utf-8\"><meta name=\"viewport\" content=\"width=device-width,initial-scale=1\"><title>Magnum</title><p><a href=\"${YT_LINK}\">Subscribe on YouTube</a></p><p><a href=\"${IG_LINK}\">Follow on Instagram</a></p>`;
      return 'data:text/html,' + encodeURIComponent(redirectHtml);
    }

    const landingUrl = getLandingUrl();

    // Generate QR
    const qrContainer = document.getElementById('qrcode');
    const qr = new QRCode(qrContainer, {
      text: landingUrl,
      width: 260,
      height: 260,
      correctLevel: QRCode.CorrectLevel.H
n    });

    // download button: convert QR canvas or img to PNG
    document.getElementById('downloadBtn').addEventListener('click', () => {
      // QRCode.js may render as <img> or <canvas>
      const img = qrContainer.querySelector('img');
      if(img){
        const url = img.src;
        const a = document.createElement('a');
        a.href = url; a.download = 'magnum-landing-qr.png'; a.click();
        return;
      }
      const canvas = qrContainer.querySelector('canvas');
      if(canvas){
        const url = canvas.toDataURL('image/png');
        const a = document.createElement('a');
        a.href = url; a.download = 'magnum-landing-qr.png'; a.click();
      }
    });

    // copy landing URL
    document.getElementById('copyBtn').addEventListener('click', async () => {
      try{ await navigator.clipboard.writeText(landingUrl); alert('Landing URL copied to clipboard'); }
      catch(e){ prompt('Copy this URL manually:', landingUrl); }
    });

    // If you want the QR to always encode a specific public URL instead of current location,
    // replace `landingUrl` variable above with the hosted URL string, e.g. 'https://yourdomain.com/magnum-landing.html'
  </script>
</body>
</html>
