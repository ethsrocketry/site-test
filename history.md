---
layout: default
title: History · ETHS Rocketry
permalink: /history.html
nav: history
body_class: page-history
---

<header class="page-header">
    <h1>History</h1>
    <p>Milestones and program timeline for ETHS Rocketry.</p>
  </header>

  <main class="history-wrap">
    <article class="history-card" id="history-content">Loading history…</article>
  </main>

<script type="text/plain" id="history-data">
# E-Town Rocket Bureau History

## August 2025 · A Fresh Start
- **Start of New ARC 2026 Program**
- Initially a 250% membership increase from last year, but later converges to 75%. 
- **Members:** Prady Manur, Peter Elbakian, Jasper Harris, Eli Drake, Mark Velichko, Mark Wolf, and Alex Martin

## March 2025 · Wildkit SRB MK3 Launches
- Wildkit SRB (Solid Rocket Booster) MK3 launches twice
- Flight #1 ends with parachute deployment failure and ballistic descent
- Flight #2 is nominal until the end, when it lands in a lake. Although the vehicle is recovered without water damage, the parachute sustains damage. As replacement chutes are not available, ETRB's ARC 2025 program ends abruptly.

## August 2024 · Foundation
- **E-Town Rocket Bureau founded**
- **Founding members:** Eli Corr, Tristan Schultz, Prady Manur, and Peter Elbakian

---

_This is a test history entry format. Add more milestones as the team grows._
</script>

<script>
function renderMarkdown(md) {
      const lines = md.split('\n');
      const out = [];
      let inList = false;

      for (const line of lines) {
        if (line.startsWith('# ')) { if (inList) { out.push('</ul>'); inList = false; } out.push(`<h1>${line.slice(2)}</h1>`); continue; }
        if (line.startsWith('## ')) { if (inList) { out.push('</ul>'); inList = false; } out.push(`<h2>${line.slice(3)}</h2>`); continue; }
        if (line.startsWith('- ')) {
          if (!inList) { out.push('<ul>'); inList = true; }
          out.push(`<li>${line.slice(2).replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')}</li>`);
          continue;
        }
        if (line.trim() === '---') { if (inList) { out.push('</ul>'); inList = false; } out.push('<hr />'); continue; }
        if (line.trim() === '') { if (inList) { out.push('</ul>'); inList = false; } continue; }
        if (inList) { out.push('</ul>'); inList = false; }
        out.push(`<p>${line.replace(/\*(.*?)\*/g, '<em>$1</em>').replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')}</p>`);
      }
      if (inList) out.push('</ul>');
      return out.join('');
    }

    const historyMarkdown = document.getElementById('history-data')?.textContent || '';
    document.getElementById('history-content').innerHTML = renderMarkdown(historyMarkdown);
</script>
