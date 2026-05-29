---
layout: default
title: Member Tools · ETHS Rocketry
permalink: /member-tools.html
nav: member-tools
body_class: page-member-tools
---

<div id="login-gate">
    <div class="login-card">
      <div class="login-icon">
        <svg viewBox="0 0 22 22" fill="none">
          <rect x="3" y="10" width="16" height="11" rx="3" stroke="#4f9cf9" stroke-width="1.5"/>
          <path d="M7 10V7a4 4 0 0 1 8 0v3" stroke="#4f9cf9" stroke-width="1.5" stroke-linecap="round"/>
          <circle cx="11" cy="15.5" r="1.5" fill="#4f9cf9"/>
        </svg>
      </div>
      <h1>Member access</h1>
      <p>This area is for ETHS Rocketry members only.</p>
      <div class="field">
        <label>Username</label>
        <input type="text" id="username" placeholder="username" autocomplete="username" />
      </div>
      <div class="field">
        <label>Password</label>
        <input type="password" id="password" placeholder="••••••••" autocomplete="current-password" />
      </div>
      <div class="error-msg" id="error-msg">Incorrect username or password.</div>
      <button class="login-btn" onclick="tryLogin()">Sign in →</button>
    </div>
  </div>

  <div id="member-content">

    <div class="page-header">
      <div class="page-header-left">
        <div class="page-eyebrow">Members only</div>
        <h1>Member tools</h1>
      </div>
      <button class="logout-btn" onclick="logout()">Sign out</button>
    </div>

    <section class="section">
      <div class="section-tag">Resources</div>
      <h2>Tools &amp; links</h2>
      <div class="tools-grid">

        <a class="tool-card" href="https://sites.google.com/eths202.org/e-town-rocket-bureau/member-tools/library-search" target="_blank">
          <div class="tool-icon">
            <svg viewBox="0 0 20 20" fill="none">
              <path d="M4 4h5v12H4zM11 4h5v12h-5z" stroke="#4f9cf9" stroke-width="1.4" stroke-linejoin="round"/>
            </svg>
          </div>
          <div>
            <div class="tool-name">Library search <span class="wip-badge">WIP</span></div>
            <div class="tool-desc">Search the club's reference library for technical resources and documents.</div>
          </div>
        </a>

        <a class="tool-card" href="#" target="_blank">
          <div class="tool-icon">
            <svg viewBox="0 0 20 20" fill="none">
              <path d="M10 2L10 14M10 2L7 6M10 2L13 6" stroke="#4f9cf9" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M4 17h12" stroke="#4f9cf9" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
          </div>
          <div>
            <div class="tool-name">Flight logs <span class="wip-badge">WIP</span></div>
            <div class="tool-desc">Past launch data, altimeter readouts, and post-flight analysis.</div>
          </div>
        </a>

        <a class="tool-card" href="#" target="_blank">
          <div class="tool-icon">
            <svg viewBox="0 0 20 20" fill="none">
              <rect x="3" y="3" width="14" height="14" rx="3" stroke="#4f9cf9" stroke-width="1.4"/>
              <path d="M7 10h6M7 7h6M7 13h3" stroke="#4f9cf9" stroke-width="1.3" stroke-linecap="round"/>
            </svg>
          </div>
          <div>
            <div class="tool-name">Meeting notes <span class="wip-badge">WIP</span></div>
            <div class="tool-desc">Agendas, decisions, and action items from club meetings.</div>
          </div>
        </a>

        <a class="tool-card" href="#" target="_blank">
          <div class="tool-icon">
            <svg viewBox="0 0 20 20" fill="none">
              <circle cx="10" cy="10" r="7" stroke="#4f9cf9" stroke-width="1.4"/>
              <path d="M10 6v4.5L12.5 13" stroke="#4f9cf9" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <div>
            <div class="tool-name">Schedule <span class="wip-badge">WIP</span></div>
            <div class="tool-desc">Upcoming meetings, launch dates, and competition deadlines.</div>
          </div>
        </a>

        <a class="tool-card" href="info-base.html">
          <div class="tool-icon">
            <svg viewBox="0 0 20 20" fill="none">
              <path d="M5 4h10a1 1 0 0 1 1 1v10a1 1 0 0 1-1 1H5a1 1 0 0 1-1-1V5a1 1 0 0 1 1-1z" stroke="#4f9cf9" stroke-width="1.4"/>
              <path d="M7 8h6M7 11h6M7 14h4" stroke="#4f9cf9" stroke-width="1.3" stroke-linecap="round"/>
            </svg>
          </div>
          <div>
            <div class="tool-name">Info base</div>
            <div class="tool-desc">Quick-learn and fabrication resources in one shared page.</div>
          </div>
        </a>

        <a class="tool-card" href="join-us.html">
          <div class="tool-icon">
            <svg viewBox="0 0 20 20" fill="none">
              <circle cx="10" cy="10" r="7" stroke="#4f9cf9" stroke-width="1.4"/>
              <path d="M10 7v6M7 10h6" stroke="#4f9cf9" stroke-width="1.4" stroke-linecap="round"/>
            </svg>
          </div>
          <div>
            <div class="tool-name">Join us</div>
            <div class="tool-desc">Unlocked placeholder page for future recruitment content.</div>
          </div>
        </a>

      </div>
    </section>

    <footer>
      <div class="footer-left">ETHS Rocketry · Evanston Township High School</div>
      <div class="footer-right">2025 – 2026</div>
    </footer>

  </div>

<script>
const SESSION_KEY = 'etrb_auth';
    const CORRECT_USER = 'member';
    const CORRECT_PASS = 'rktpass1';

    function setMemberNavState(isLoggedIn) {
      const icon = document.getElementById('member-lock-icon');
      if (icon) icon.classList.toggle('unlocked', isLoggedIn);
      document.querySelectorAll('.member-only').forEach(el => {
        el.style.display = isLoggedIn ? 'inline-flex' : 'none';
      });
    }

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
        document.getElementById('password').focus();
      }
    }

    function logout() {
      sessionStorage.removeItem(SESSION_KEY);
      document.getElementById('member-content').style.display = 'none';
      document.getElementById('login-gate').style.display = 'flex';
      document.getElementById('username').value = '';
      document.getElementById('password').value = '';
      setMemberNavState(false);
    }

    function showContent() {
      document.getElementById('login-gate').style.display = 'none';
      document.getElementById('member-content').style.display = 'block';
      setMemberNavState(true);
    }

    if (sessionStorage.getItem(SESSION_KEY) === '1') showContent();
    else setMemberNavState(false);

    document.addEventListener('keydown', e => {
      if (e.key === 'Enter' && document.getElementById('login-gate').style.display !== 'none') tryLogin();
    });
</script>
