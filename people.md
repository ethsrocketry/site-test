---
layout: default
title: People · ETHS Rocketry
permalink: /people.html
nav: people
body_class: page-people
---

<header class="page-header">
    <h1>People</h1>
    <p>Current members and alumni.</p>
    <div class="search-container">
      <input type="text" id="people-search" placeholder="Search entries..." aria-label="Search people entries">
    </div>
  </header>

  <main class="people-wrap" id="people-content">Loading people data…</main>

<script type="text/plain" id="people-data">
// people_data.md format
// section(<section title>)
// person(<name>, <image path>, <bio>)
// contact(<method>, <url>)
// endperson
//
// Notes:
// - Add at least one section(current members) and optionally section(alumni).
// - contact method can be LinkedIn, GitHub, Email, Instagram, Website, etc.
// - For Email, use a mailto: link (example: mailto:name@example.com).
// - Make sure to add quotes around the bio section. (example: "Willy the Wildkit is capable of creating general artificial intelligence but chooses not to")

section(current members)
person(Riley Fortune, people-media/wip-square.svg, "Club facilitator and teacher.")
contact(Email, mailto:fortuner@eths202.org)
endperson

person(Kevin Duffy, people-media/wip-square.svg, "Club facilitator and teacher.")
contact(Email, mailto:duffyk@eths202.org)
endperson

person(Prady Manur, people-media/wip-square.svg, "E-Town Rocket Bureau Co-founder and ARC 2026 chief and systems engineer working on organization architecture, GNC, and systems integration.")
contact(LinkedIn, https://www.linkedin.com/in/prady-m/)
endperson

person(Peter Elbakian, people-media/wip-square.svg, "Lead structures engineer for Lil' Willy (2026) and Wildkit SRB MK3 (2025).")
contact(LinkedIn, https://www.linkedin.com/in/peter-elbakian-2a66453b0/)
endperson

person(Jasper Harris, people-media/wip-square.svg, "Electrical engineer for Lil' Willy (2026) working on manufacturing and design of avionics hardware.")
// contact(<method>, <url>)
endperson

person(Eli Drake, people-media/wip-square.svg, "Testing and Launch Tower engineer for Lil' Willy (2026) working on Tower 1 upgrades and flight testing.")
// contact(<method>, <url>)
endperson

person(Mark Velichko, people-media/wip-square.svg, "Launch Tower engineer for Lil' Willy (2026) working on Tower 1 upgrades.")
// contact(<method>, <url>)
endperson

person(Mark Wolf, people-media/wip-square.svg, "Launch Tower engineer for Lil' Willy (2026) working on Tower 1 upgrades.")
// contact(<method>, <url>)
endperson

person(Alex Martin, people-media/wip-square.svg, "Launch Tower engineer for Lil' Willy (2026) working on Tower 1 upgrades.")
// contact(<method>, <url>)
endperson

person(Parker Kloster, people-media/wip-square.svg, "[BIO WIP]")
// contact(<method>, <url>)
endperson

person(Liam Kipnis, people-media/wip-square.svg, "[BIO WIP]")
// contact(<method>, <url>)
endperson

person(Laney Harris-Stapleton, people-media/wip-square.svg, "[BIO WIP]")
// contact(<method>, <url>)
endperson

person(Anne Bauer, people-media/wip-square.svg, "[BIO WIP]")
// contact(<method>, <url>)
endperson

person(Neil Ghaskadvi, people-media/wip-square.svg, "[BIO WIP]")
// contact(<method>, <url>)
endperson

person(Zach Fogel, people-media/wip-square.svg, "[BIO WIP]")
// contact(<method>, <url>)
endperson

section(alumni)
person(Eli Corr, people-media/eli-corr.jpg, "Founder of E-Town Rocket Bureau and ETHS's ARC program, engineered critical systems for Wildkit SRB MK3 (2025). Currently studying aerospace engineering at Rensselaer Polytechnic Institute.")
contact(LinkedIn, https://www.linkedin.com/in/eli-corr-486847392/)
endperson

person(Tristan Schultz, people-media/tristan-schultz.jpg, "Co-founder of E-Town Rocket Bureau and ETHS's ARC program, engineered critical systems for Wildkit SRB MK3 (2025). Currently studying aerospace engineering at the University of Colorado, Boulder.")
contact(LinkedIn, https://www.linkedin.com/in/tristan-schultz-54930428a/)
endperson
</script>

<script>
function escapeHtml(text) {
      return text
        .replaceAll('&', '&amp;')
        .replaceAll('<', '&lt;')
        .replaceAll('>', '&gt;')
        .replaceAll('"', '&quot;')
        .replaceAll("'", '&#039;');
    }

    function splitArgs(raw) {
      const args = [];
      let current = '';
      let inQuote = false;
      for (const ch of raw) {
        if (ch === '"') {
          inQuote = !inQuote;
          continue;
        }
        if (ch === ',' && !inQuote) {
          args.push(current.trim());
          current = '';
        } else {
          current += ch;
        }
      }
      if (current) args.push(current.trim());
      return args;
    }

    function iconFor(method) {
      const m = method.toLowerCase();
      if (m === 'linkedin') return '<svg viewBox="0 0 24 24" fill="currentColor"><path d="M6.94 8.5H3.56V20h3.38V8.5Zm.22-3.54A1.96 1.96 0 1 0 3.24 5a1.96 1.96 0 0 0 3.92-.04ZM20.76 13.44c0-3.46-1.85-5.07-4.32-5.07a3.74 3.74 0 0 0-3.35 1.84V8.5H9.72V20h3.37v-6.05c0-1.6.3-3.15 2.28-3.15 1.95 0 1.98 1.82 1.98 3.25V20h3.41v-6.56Z"/></svg>';
      if (m === 'github') return '<svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.48 2 2 6.58 2 12.22c0 4.5 2.87 8.32 6.84 9.66.5.1.68-.22.68-.49 0-.24-.01-1.03-.01-1.86-2.78.62-3.36-1.21-3.36-1.21-.46-1.19-1.11-1.5-1.11-1.5-.9-.63.07-.62.07-.62 1 .07 1.52 1.04 1.52 1.04.89 1.56 2.32 1.11 2.88.85.09-.66.35-1.11.64-1.36-2.22-.26-4.56-1.14-4.56-5.07 0-1.12.39-2.03 1.03-2.75-.1-.26-.45-1.31.1-2.73 0 0 .84-.28 2.75 1.05A9.27 9.27 0 0 1 12 7.1a9.3 9.3 0 0 1 2.5.35c1.9-1.33 2.74-1.05 2.74-1.05.56 1.42.2 2.47.11 2.73.64.72 1.03 1.63 1.03 2.75 0 3.94-2.34 4.81-4.57 5.06.36.32.68.95.68 1.92 0 1.39-.01 2.5-.01 2.84 0 .27.18.6.69.49A10.23 10.23 0 0 0 22 12.22C22 6.58 17.52 2 12 2Z"/></svg>';
      if (m === 'email') return '<svg viewBox="0 0 24 24" fill="currentColor"><path d="M3 5h18a1 1 0 0 1 1 1v12a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1V6a1 1 0 0 1 1-1Zm9 7 8-5H4l8 5Zm0 2-8-5v8h16V9l-8 5Z"/></svg>';
      if (m === 'instagram') return '<svg viewBox="0 0 24 24" fill="currentColor"><path d="M7 2h10a5 5 0 0 1 5 5v10a5 5 0 0 1-5 5H7a5 5 0 0 1-5-5V7a5 5 0 0 1 5-5Zm0 2a3 3 0 0 0-3 3v10a3 3 0 0 0 3 3h10a3 3 0 0 0 3-3V7a3 3 0 0 0-3-3H7Zm11.5 1.5a1 1 0 1 1-1 1 1 1 0 0 1 1-1ZM12 7a5 5 0 1 1-5 5 5 5 0 0 1 5-5Zm0 2a3 3 0 1 0 3 3 3 3 0 0 0-3-3Z"/></svg>';
      return '<svg viewBox="0 0 24 24" fill="currentColor"><path d="M14 3h7v7h-2V6.41l-8.29 8.3-1.42-1.42 8.3-8.29H14V3ZM5 5h6v2H7v10h10v-4h2v6H5V5Z"/></svg>';
    }

    function parsePeople(md) {
      const lines = md.split('\n');
      const sections = [];
      let currentSection = null;
      let currentPerson = null;

      function pushPerson() {
        if (!currentSection || !currentPerson) return;
        currentSection.people.push(currentPerson);
        currentPerson = null;
      }

      for (const lineRaw of lines) {
        const line = lineRaw.trim();
        if (!line || line.startsWith('//')) continue;

        if (line.startsWith('section(') && line.endsWith(')')) {
          pushPerson();
          const title = line.slice(8, -1).trim();
          currentSection = { title, people: [] };
          sections.push(currentSection);
          continue;
        }

        if (line.startsWith('person(') && line.endsWith(')')) {
          pushPerson();
          if (!currentSection) continue;
          const args = splitArgs(line.slice(7, -1));
          currentPerson = {
            name: args[0] || 'Unnamed person',
            image: args[1] || 'people-media/wip-square.svg',
            bio: args[2] || 'Bio coming soon.',
            contacts: []
          };
          continue;
        }

        if (line.startsWith('contact(') && line.endsWith(')') && currentPerson) {
          const args = splitArgs(line.slice(8, -1));
          if (args[0] && args[1]) {
            currentPerson.contacts.push({ method: args[0], url: args[1] });
          }
          continue;
        }

        if (line === 'endperson') {
          pushPerson();
        }
      }
      pushPerson();
      return sections;
    }

    function sectionCountLabel(title, count) {
      const t = title.toLowerCase();
      if (t.includes('alumni')) return `${count} alumni`;
      if (t.includes('member')) return `${count} members`;
      return `${count} entries`;
    }

    function renderPeopleSections(sections) {
      if (!sections.length) return '<p class="no-results">No people data found.</p>';

      return sections.map((section) => {
        const peopleHtml = section.people.map((person) => {
          const contacts = person.contacts.length
            ? person.contacts.map((contact) => `<a class="contact-link" href="${escapeHtml(contact.url)}" target="_blank" rel="noopener noreferrer" aria-label="${escapeHtml(contact.method)}">${iconFor(contact.method)}</a>`).join('')
            : '<span style="font-family: var(--mono); font-size: 11px; color: var(--text-faint);">contact info coming soon</span>';
          return `<article class="person-card">
            <img class="person-photo" src="${escapeHtml(person.image)}" alt="Photo of ${escapeHtml(person.name)}" />
            <h3 class="person-name">${escapeHtml(person.name)}</h3>
            <p class="person-bio">${escapeHtml(person.bio)}</p>
            <div class="person-contacts">${contacts}</div>
          </article>`;
        }).join('');

        return `<section class="people-section">
          <h2>${escapeHtml(section.title)} <span class="section-count">${sectionCountLabel(section.title, section.totalCount)}</span></h2>
          <div class="people-grid">${peopleHtml}</div>
        </section>`;
      }).join('');
    }

    let allSections = [];

    function filterPeople() {
      const query = (document.getElementById('people-search')?.value || '').trim().toLowerCase();
      const filteredSections = allSections
        .map((section) => {
          const filteredPeople = query
            ? section.people.filter((person) => {
                const searchBlob = `${section.title} ${person.name} ${person.bio}`.toLowerCase();
                return searchBlob.includes(query);
              })
            : section.people;
          return {
            title: section.title,
            people: filteredPeople,
            totalCount: section.people.length
          };
        })
        .filter((section) => section.people.length > 0);

      document.getElementById('people-content').innerHTML = filteredSections.length
        ? renderPeopleSections(filteredSections)
        : '<p class="no-results">No matching people entries found.</p>';
    }

    allSections = parsePeople(document.getElementById('people-data')?.textContent || '');
    filterPeople();

    document.getElementById('people-search')?.addEventListener('input', filterPeople);
</script>
