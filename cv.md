<style>
.cv-toolbar { display: flex; justify-content: center; gap: 12px; flex-wrap: wrap; max-width: 900px; margin: 20px auto 0; }
.cv-btn { padding: 8px 16px; border: 1px solid #ccc; border-radius: 6px; text-decoration: none; font-size: 0.95em; }
.cv-btn:hover { background: rgba(0, 0, 0, 0.05); }
.cv-container { width: 100%; max-width: 850px; margin: 16px auto; }
.cv-fallback { text-align: center; }
.cv-page { position: relative; margin: 0 auto; }
.cv-page + .cv-page { border-top: 1px solid #e5e5e5; margin-top: 24px; padding-top: 24px; }
.cv-page canvas { display: block; width: 100%; height: auto; }
.cv-link { position: absolute; display: block; border-radius: 3px; }
.cv-link:hover, .cv-link:focus { background: rgba(255, 215, 0, 0.25); outline: 1px solid rgba(255, 170, 0, 0.7); }
</style>
<div class="cv-toolbar">
  <a class="cv-btn" href="{{ '/assets/files/Ellen_Kolesnikova_resume.pdf' | relative_url }}" target="_blank" rel="noopener">Open PDF in new tab</a>
  <a class="cv-btn" href="{{ '/assets/files/Ellen_Kolesnikova_resume.pdf' | relative_url }}" download>Download PDF</a>
</div>
<div id="cv" class="cv-container" data-pdf="{{ '/assets/files/Ellen_Kolesnikova_resume.pdf' | relative_url }}">
  <p class="cv-fallback">Loading resume… If it doesn't appear, <a href="{{ '/assets/files/Ellen_Kolesnikova_resume.pdf' | relative_url }}" target="_blank" rel="noopener">open the PDF here</a>.</p>
</div>
<script type="module">
import * as pdfjsLib from 'https://cdn.jsdelivr.net/npm/pdfjs-dist@4.10.38/build/pdf.min.mjs';

pdfjsLib.GlobalWorkerOptions.workerSrc =
  'https://cdn.jsdelivr.net/npm/pdfjs-dist@4.10.38/build/pdf.worker.min.mjs';

const container = document.getElementById('cv');
const PDF_URL = container.dataset.pdf;

let pdf = null;
let lastWidth = 0;
let renderToken = 0;

// Only allow normal web and email links from the PDF.
function safeHref(annot) {
  const url = annot.url || annot.unsafeUrl;
  if (!url) return null;
  return /^(https?:|mailto:)/i.test(url) ? url : null;
}

function pct(value, total) {
  return (value / total) * 100 + '%';
}

async function renderPage(page, width, dpr) {
  const base = page.getViewport({ scale: 1 });
  const viewport = page.getViewport({ scale: width / base.width });

  const wrap = document.createElement('div');
  wrap.className = 'cv-page';

  const canvas = document.createElement('canvas');
  canvas.width = Math.floor(viewport.width * dpr);
  canvas.height = Math.floor(viewport.height * dpr);
  wrap.appendChild(canvas);

  await page.render({
    canvasContext: canvas.getContext('2d'),
    viewport,
    transform: dpr !== 1 ? [dpr, 0, 0, dpr, 0, 0] : null,
  }).promise;

  // Positions are percentages, so links stay aligned when the page resizes.
  for (const annot of await page.getAnnotations()) {
    if (annot.subtype !== 'Link') continue;
    const href = safeHref(annot);
    if (!href) continue;

    const [x1, y1, x2, y2] = viewport.convertToViewportRectangle(annot.rect);
    const link = document.createElement('a');
    link.className = 'cv-link';
    link.href = href;
    link.title = href.replace(/^mailto:/i, '');
    link.setAttribute('aria-label', link.title);
    if (!/^mailto:/i.test(href)) {
      link.target = '_blank';
      link.rel = 'noopener';
    }
    link.style.left = pct(Math.min(x1, x2), viewport.width);
    link.style.top = pct(Math.min(y1, y2), viewport.height);
    link.style.width = pct(Math.abs(x2 - x1), viewport.width);
    link.style.height = pct(Math.abs(y2 - y1), viewport.height);
    wrap.appendChild(link);
  }
  return wrap;
}

async function render() {
  const width = container.clientWidth;
  if (!pdf || !width || width === lastWidth) return;
  lastWidth = width;
  const token = ++renderToken;
  const dpr = window.devicePixelRatio || 1;

  const pages = [];
  for (let n = 1; n <= pdf.numPages; n++) {
    pages.push(await renderPage(await pdf.getPage(n), width, dpr));
  }
  // Ignore renders superseded by a newer resize.
  if (token === renderToken) container.replaceChildren(...pages);
}

let resizeTimer;
window.addEventListener('resize', () => {
  clearTimeout(resizeTimer);
  resizeTimer = setTimeout(render, 250);
});

try {
  pdf = await pdfjsLib.getDocument(PDF_URL).promise;
  await render();
} catch (err) {
  // Leave the fallback link in place.
  console.error('[resume-viewer] PDF.js failed:', err);
}
</script>
