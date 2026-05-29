---
layout: default
title: Info Base · ETHS Rocketry
permalink: /info-base.html
nav: info-base
body_class: page-info-base
---

<script type="text/plain" id="info-base-data">
// Supported link types: DOC, PDF, CSV, TXT, PPTX, VIDEO, IMAGE
// IMAGE aliases also supported in info-base: IMG, JPG, JPEG, PNG, GIF, WEBP, SVG

heading(getting started)
link(Software Kit, A list of useful apps and instructions on installation, DOC, https://docs.google.com/document/d/1lk6KhbNYfQ0HLD55EobMOPDYFdndjwz_wkb6v5dFQxQ/edit?tab=t.0)

heading(using the website)
link(Using the Rocketry Website, A guide to using and maintaining this website, DOC, https://docs.google.com/document/d/12gRNPaZGAlTICXUV52kFTW7cqM-3rsZHIYmJUNRoMXM/edit?tab=t.0#heading=h.yp0w4xn10ih7)
link(Markdown Syntax, A guide to using markdown, DOC, https://www.markdownguide.org/basic-syntax/)

heading(quick-learn)
link(Electronics Notes, Quick guide to arduino-style electronics, DOC, https://docs.google.com/document/d/1epr-BBOIx0zVUhN5aM855oR_4kyLSvjc7tyNIKxZ7uQ/edit?tab=t.0)
link(OpenRocket User Guide, A comprehensive guide to using OpenRocket, DOC, https://openrocket.readthedocs.io/en/latest/)
//
heading(fabrication)
link(3D Printing Tolerance Sheet, Dimensional and fit guidance for printed parts, DOC, https://docs.google.com/document/d/1o35jaJcufN3r2FYCSSN3ju66eLskUaP5I9Abqzxr5Nw/edit)
link(Laser Cutting, Cut template file and guide to using it, DOC, https://docs.google.com/document/d/1fdLBoLlA14ZmZz34XSjDOz9xr1uwUYBFo4vrAvqtSec/edit)
//
heading(physics)
link(Aerodynamics Notes [WIP], Comprehensive set of aerodynamics equations and use guides, DOC, https://docs.google.com/document/d/1tGBDdoWZ5yXNuwJ4qt3pcJ0UZ5BCknf8UAwRt1Fy1B4/edit?tab=t.pnmtgqjl5sa9)
link(OpenRocket Technical Documentation, How the physics and code behind OpenRocket works, PDF, https://drive.google.com/file/d/1LAQwhqnrm9TVrqVO8VWRuQ25nordtF0B/view)
//
heading(guidance, navigation, and control (GNC))
link(Helpful airbrake control videos, A set of useful videos on active airbraked control, VIDEO, https://docs.google.com/document/d/1SS-ee4huyNWXu5YKEJBs0b0ZoFZePtNta1MYV-bVyBA/edit?tab=t.0)
//
heading(datasets)
link(Motor Thrust and Weight Data, Datasets of various motor thrust profiles, CSV, https://docs.google.com/spreadsheets/d/1_fO9yHK-BEU7YAX_vI6N74XljMth9HyZWJK6BY4gGcc/edit?gid=1343307092#gid=1343307092)
//
heading(pre-flight simulation and testing)
link(Ground Truth: Physical Testing, Apogee Components newsletter on simulation and physical ground testing of aerostructures, PDF, https://www.apogeerockets.com/education/downloads/Newsletter677.pdf?_kx=uSVweLHx6NxMcbZrmiYCufdlaHVnP7_0ByKhwfypaaz26CQcSfInkqTYdV_q4tDH.V8TZeH)
</script>

<div id="login-gate">
    <div class="login-card">
      <h1>Member access</h1>
      <p>Sign in to access the info base.</p>
      <div class="field"><label>Username</label><input type="text" id="username" placeholder="username" /></div>
      <div class="field"><label>Password</label><input type="password" id="password" placeholder="••••••••" /></div>
      <div class="error-msg" id="error-msg">Incorrect username or password.</div>
      <button class="login-btn" onclick="tryLogin()">Sign in →</button>
    </div>
  </div>

  <div id="member-content">
    <div class="page-header">
      <div>
        <div class="page-eyebrow">Members only</div>
        <h1>Info base</h1>
        <div class="search-container">
          <input type="text" id="info-search" placeholder="Search entries..." onkeyup="filterEntries()">
        </div>
      </div>
      <button class="logout-btn" onclick="logout()">Sign out</button>
    </div>

    <div id="dynamic-content"></div>
  </div>

<script>
const SESSION_KEY = 'etrb_auth';
    const CORRECT_USER = 'member';
    const CORRECT_PASS = 'rktpass1';

    // --- NAV STATE ---
    function setMemberNavState(isLoggedIn) {
      const icon = document.getElementById('member-lock-icon');
      if (icon) icon.classList.toggle('unlocked', isLoggedIn);
      document.querySelectorAll('.member-only').forEach(el => {
        el.style.display = isLoggedIn ? 'inline-flex' : 'none';
      });
    }

    // --- SEARCH FILTER ---
    function filterEntries() {
      const query = document.getElementById('info-search').value.toLowerCase();
      const entries = document.querySelectorAll('.doc-list li');
      const sections = document.querySelectorAll('.section');

      entries.forEach(entry => {
        const title = entry.querySelector('.doc-title').textContent.toLowerCase();
        const desc = entry.querySelector('.doc-desc').textContent.toLowerCase();
        
        if (title.includes(query) || desc.includes(query)) {
          entry.style.display = "";
        } else {
          entry.style.display = "none";
        }
      });

      sections.forEach(section => {
        const visibleItems = section.querySelectorAll('li:not([style*="display: none"])');
        section.style.display = visibleItems.length === 0 && query !== "" ? "none" : "";
      });
    }

    // --- PARSER ENGINE ---
    async function loadInfoBase() {
      const container = document.getElementById('dynamic-content');
      try {
        const text = document.getElementById('info-base-data')?.textContent || '';
        const lines = text.split('\n');
        
        let html = '';
        let listOpen = false;

        lines.forEach(line => {
          line = line.trim();
          if (!line || line.startsWith('//')) return;

          const headingMatch = line.match(/^heading\((.*)\)$/);
          const linkMatch = line.match(/^link\((.*),\s*(.*),\s*(.*),\s*(.*)\)$/);

          if (headingMatch) {
            if (listOpen) { html += '</ul></section>'; }
            html += `<section class="section"><h2>${headingMatch[1]}</h2><ul class="doc-list">`;
            listOpen = true;
          } 
          else if (linkMatch) {
            const [_, title, desc, type, url] = linkMatch;
            const normalizedType = type.trim().toUpperCase();
            const iconByType = {
              DOC: `<svg class="doc-icon" viewBox="0 0 20 20" fill="none"><rect x="4" y="3" width="12" height="14" rx="2" stroke="currentColor" stroke-width="1.5"/><path d="M7 7H13M7 10H13M7 13H11" stroke="currentColor" stroke-width="1.3" stroke-linecap="round"/></svg>`,
              PDF: `<svg class="doc-icon" viewBox="0 0 20 20" fill="none"><rect x="4" y="3" width="12" height="14" rx="2" stroke="currentColor" stroke-width="1.5"/><text x="10" y="12" text-anchor="middle" font-size="4.2" font-family="Inter, sans-serif" font-weight="700" fill="currentColor">PDF</text></svg>`,
              CSV: `<svg class="doc-icon" viewBox="0 0 20 20" fill="none"><rect x="4" y="3" width="12" height="14" rx="2" stroke="currentColor" stroke-width="1.5"/><path d="M7 8H13M7 10.5H13M7 13H13M9.2 7.2V14.8M11.8 7.2V14.8" stroke="currentColor" stroke-width="1.1" stroke-linecap="round"/></svg>`,
              TXT: `<svg class="doc-icon" viewBox="0 0 20 20" fill="none"><rect x="4" y="3" width="12" height="14" rx="2" stroke="currentColor" stroke-width="1.5"/><path d="M7 8H13M7 10.5H13M7 13H12" stroke="currentColor" stroke-width="1.2" stroke-linecap="round"/></svg>`,
              PPTX: `<svg class="doc-icon" viewBox="0 0 20 20" fill="none"><rect x="4" y="3" width="12" height="14" rx="2" stroke="currentColor" stroke-width="1.5"/><path d="M7.2 12V8H8.6C9.3 8 9.8 8.4 9.8 9.1C9.8 9.8 9.3 10.2 8.6 10.2H7.2M10.7 12L12 10L13.3 12M11.2 11.2H12.8" stroke="currentColor" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/></svg>`,
              VIDEO: `<svg class="doc-icon" viewBox="0 0 20 20" fill="none"><rect x="4" y="5" width="9.5" height="10" rx="2" stroke="currentColor" stroke-width="1.5"/><path d="M8 8L10.8 10L8 12V8Z" fill="currentColor"/><path d="M13.5 8.3L16 7V13L13.5 11.7" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"/></svg>`,
              IMAGE: `<svg class="doc-icon" viewBox="0 0 20 20" fill="none"><rect x="3.8" y="4" width="12.4" height="12" rx="2" stroke="currentColor" stroke-width="1.5"/><circle cx="8" cy="8" r="1.2" fill="currentColor"/><path d="M5.5 14L9.2 10.3C9.6 9.9 10.2 9.9 10.6 10.3L12 11.7L13.1 10.6C13.5 10.2 14.1 10.2 14.5 10.6L16 12.1" stroke="currentColor" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/></svg>`
            };
            const imageTypes = ['IMG', 'JPG', 'JPEG', 'PNG', 'GIF', 'WEBP', 'SVG'];
            const presentationTypes = ['SLIDES', 'PPT'];
            const effectiveType = imageTypes.includes(normalizedType)
              ? 'IMAGE'
              : (presentationTypes.includes(normalizedType) ? 'PPTX' : normalizedType);
            const iconSVG = iconByType[effectiveType] || iconByType.DOC;
            const iconTypeClass = `type-${effectiveType.toLowerCase()}`;

            html += `
              <li>
                <a class="doc-link" href="${url.trim()}" target="_blank" rel="noopener noreferrer">
                  <div>
                    <div class="doc-title">${title.trim()}</div>
                    <div class="doc-desc">${desc.trim()}</div>
                  </div>
                  <span class="doc-icon-wrap ${iconTypeClass}">${iconSVG}</span>
                </a>
              </li>`;
          }
        });

        if (listOpen) html += '</ul></section>';
        container.innerHTML = html;

      } catch (err) {
        container.innerHTML = `<div class="section"><p style="color:var(--red); font-family:var(--mono); font-size:13px;">[ERROR] Failed to load info base data from this page.</p></div>`;
      }
    }

    // --- AUTH LOGIC ---
    function tryLogin() {
      const user = document.getElementById('username').value.trim();
      const pass = document.getElementById('password').value;
      const err  = document.getElementById('error-msg');
      if (user === CORRECT_USER && pass === CORRECT_PASS) {
        sessionStorage.setItem(SESSION_KEY, '1');
        err.classList.remove('visible');
        showContent();
      } else {
        err.classList.add('visible');
        document.getElementById('password').value = '';
      }
    }

    function logout() {
      sessionStorage.removeItem(SESSION_KEY);
      location.reload();
    }

    function showContent() {
      document.getElementById('login-gate').style.display = 'none';
      document.getElementById('member-content').style.display = 'block';
      setMemberNavState(true);
      loadInfoBase();
    }

    if (sessionStorage.getItem(SESSION_KEY) === '1') showContent();
    else setMemberNavState(false);

    document.addEventListener('keydown', e => {
      if (e.key === 'Enter' && document.getElementById('login-gate').style.display !== 'none') tryLogin();
    });
</script>
