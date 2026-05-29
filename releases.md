---
layout: default
title: Releases · ETHS Rocketry
permalink: /releases.html
nav: releases
body_class: page-releases
---

<header class="page-header">
    <h1>Releases</h1>
    <p>Public changelog and resources. View release notes or embedded media below.</p>
  </header>

  <script type="text/plain" id="releases-data">
## Lil' Willy's First Flight
- date: 2026-05-01
- mode: embed
- url: arc26-media/ARC Launch 1 Release.mp4
---
Watch Lil' Willy's first flight.

## Website Beta Version
- date: 2026-04-28
- mode: tab
- summary: Example in-page release tab with markdown body content.
---
This website was created by Prady using various AI Agents (Gemini, Claude, ChatGPT Codex, and Grok). 

## ARC Registration Packet
- date: 2026-03-16
- mode: link
- summary: Example direct link release item.
- url: https://www.rocketcontest.org/
---
Optional notes can live here for team reference, even for link entries.
</script>

  <main class="release-wrap">
    <section class="release-list">
      <h2>Release Feed</h2>
      <div class="release-buttons" id="release-buttons">Loading releases…</div>
    </section>

    <section class="release-viewer">
      <h2>Viewer</h2>
      <div class="release-tabbar" id="release-tabbar"></div>
      <div class="release-panel" id="release-panel">
        <p class="empty-state">Select a release to view.</p>
      </div>
    </section>
  </main>

<script>
function escapeHtml(str) {
      return str.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;').replace(/'/g, '&#39;');
    }

    function renderMarkdown(md) {
      const lines = md.split('\n');
      const out = [];
      let inList = false;
      for (const line of lines) {
        if (line.startsWith('- ')) {
          if (!inList) { out.push('<ul>'); inList = true; }
          out.push(`<li>${escapeHtml(line.slice(2))}</li>`);
          continue;
        }
        if (line.trim() === '') {
          if (inList) { out.push('</ul>'); inList = false; }
          continue;
        }
        if (inList) { out.push('</ul>'); inList = false; }
        out.push(`<p>${escapeHtml(line)}</p>`);
      }
      if (inList) out.push('</ul>');
      return out.join('');
    }

    function parseReleases(md) {
      const sections = md.split(/\n##\s+/).map((chunk, i) => i === 0 ? chunk.replace(/^##\s+/, '') : chunk);
      const releases = [];
      sections.forEach((section) => {
        const trimmed = section.trim();
        if (!trimmed) return;
        const lines = trimmed.split('\n');
        const title = lines.shift().trim();
        const meta = {};
        const bodyLines = [];
        let inBody = false;
        lines.forEach((line) => {
          if (!inBody && line.startsWith('- ')) {
            const sep = line.indexOf(':');
            if (sep > -1) {
              meta[line.slice(2, sep).trim().toLowerCase()] = line.slice(sep + 1).trim();
              return;
            }
          }
          if (line.trim() === '---') { inBody = true; return; }
          if (inBody) bodyLines.push(line);
        });
        releases.push({
          title,
          date: meta.date || 'undated',
          summary: meta.summary || '',
          mode: (meta.mode || 'tab').toLowerCase(),
          url: meta.url || '',
          body: bodyLines.join('\n').trim()
        });
      });
      return releases;
    }

    const openTabs = [];
    let activeTabIndex = -1;

    function renderTabPanel() {
      const tabbar = document.getElementById('release-tabbar');
      const panel = document.getElementById('release-panel');
      if (openTabs.length === 0) {
        tabbar.innerHTML = '';
        panel.innerHTML = '<p class="empty-state">Select a release from the feed.</p>';
        return;
      }
      tabbar.innerHTML = openTabs.map((tab, idx) => `
        <button class="release-tab ${idx === activeTabIndex ? 'active' : ''}" data-tab-index="${idx}">${escapeHtml(tab.title)}</button>
      `).join('');

      const release = openTabs[activeTabIndex];
      let mediaContent = '';

      if (release.mode === 'embed' && release.url) {
        const url = release.url.toLowerCase();
        const isPdf = url.endsWith('.pdf');
        const isVideo = url.endsWith('.mp4') || url.endsWith('.webm') || url.endsWith('.mov');
        const isImage = url.endsWith('.jpg') || url.endsWith('.jpeg') || url.endsWith('.png') || url.endsWith('.gif') || url.endsWith('.webp');

        if (isVideo) {
          mediaContent = `<div class="embed-container"><video controls><source src="${release.url}" type="video/mp4">Your browser does not support the video tag.</video></div>`;
        } else if (isPdf) {
          mediaContent = `<div class="embed-container"><iframe src="${release.url}"></iframe></div>`;
        } else if (isImage) {
          mediaContent = `<img src="${release.url}" class="embed-img" alt="${escapeHtml(release.title)}">`;
        } else {
          mediaContent = `<div class="embed-container"><iframe src="${release.url}"></iframe></div>`;
        }
      }

      panel.innerHTML = `
        <h3>${escapeHtml(release.title)}</h3>
        <p class="release-date">${escapeHtml(release.date)}</p>
        ${mediaContent}
        <div class="release-body" style="margin-top: 20px;">${renderMarkdown(release.body || '')}</div>
      `;

      tabbar.querySelectorAll('[data-tab-index]').forEach(btn => {
        btn.onclick = () => { activeTabIndex = Number(btn.dataset.tabIndex); renderTabPanel(); };
      });
    }

    async function loadReleases() {
      const buttonWrap = document.getElementById('release-buttons');
      try {
        const raw = document.getElementById('releases-data')?.textContent || '';
        const releases = parseReleases(raw);
        buttonWrap.innerHTML = '';
        releases.forEach((release) => {
          const btn = document.createElement('button');
          btn.className = 'release-button';
          btn.innerHTML = `<div class="release-title">${escapeHtml(release.title)}</div><div class="release-meta">${escapeHtml(release.date)}</div>`;
          btn.onclick = () => {
            if (release.mode === 'link') { window.open(release.url, '_blank'); return; }
            const idx = openTabs.findIndex(t => t.title === release.title);
            if (idx === -1) { openTabs.push(release); activeTabIndex = openTabs.length - 1; }
            else { activeTabIndex = idx; }
            renderTabPanel();
          };
          buttonWrap.appendChild(btn);
        });
      } catch (err) { buttonWrap.innerHTML = '<p class="empty-state">Error loading releases.</p>'; }
    }
    loadReleases();
</script>
