# مواصفة قالب جدول القوائم المنسدلة (dropdown-table spec)

> القالب المعتمد رسمياً للأسئلة من نوع "اختر التصنيف من قائمة منسدلة لكل كلمة في جملة"
> المرجع المستخرج من: ملف الاختبار التشخيصي — السؤال الثالث ("أستخرجُ الفعلَ والفاعلَ والمفعولَ بهِ")
> آخر تحديث: 2026-05-28

---

## ١. متى يُستخدم هذا القالب؟

يُستخدم هذا القالب حين يحتوي السؤال على عدة جمل، تُقسَّم كلٌّ منها إلى كلمات مفردة، ويُطلب من الطالب تصنيف كل كلمة عبر قائمة منسدلة مخصصة لها. يُعالج القالب كل خانة باستقلالية (عداد محاولات خاص ومنفصل)، ثم يُعالج كل جملة باستقلالية (ينتهي التصفح بزر "الجملة التالية" الذي لا يُفعَّل إلا بعد اكتمال جميع الخانات).

---

## ٢. البنية البصرية (Visual Structure)

```
.dropdown-tables-wrap
└── .dropdown-sentence-card  [data-sent-idx="N"]  ← واحدة تظهر في وقت واحد
    ├── .dropdown-sentence-header
    │   ├── .dropdown-sentence-num          ← "الجملة ١ من ٣"  (اختياري)
    │   └── .dropdown-sentence-text         ← نص الجملة الكاملة
    ├── table.dropdown-table
    │   ├── thead > tr > th × 2             ← "الكلمةُ" | "اختَرْ نوعَها"
    │   └── tbody
    │       └── tr.dropdown-row  [data-sent-idx data-word-idx data-correct]
    │           ├── td.dropdown-word-cell   ← الكلمة
    │           └── td.dropdown-select-cell
    │               └── .custom-dropdown  [data-sent-idx data-word-idx]
    │                   ├── button.custom-dropdown-trigger
    │                   │   ├── span.custom-dropdown-value   ← "اختَرْ"
    │                   │   └── i.custom-dropdown-arrow      ← fa-chevron-down
    │                   └── ul.custom-dropdown-menu
    │                       └── li.custom-dropdown-option × N  [data-value]
    ├── .q-feedback.dropdown-sentence-fb   ← [id="fb-qX-sY"]
    └── .dropdown-sentence-nav
        ├── button.btn-sentence-prev        ← السابق (hidden if sIdx===0)
        └── button.btn-sentence-next        ← التالي (disabled حتى اكتمال الجملة)
```

---

## ٣. متغيرات التصميم المطلوبة (Required CSS Variables)

```css
:root {
  --color-primary:          #844816;
  --color-primary-light:    #F3EDE8;
  --color-primary-shadow:   #5e3210;
  --color-secondary:        #EDC66F;
  --color-secondary-light:  #FFF4E5;
  --color-secondary-shadow: #c9a548;
  --color-sand:             #C9B5A0;
  --color-ink:              #3a2410;
  --color-error:            #B7472D;
  --color-success:          #6b8b1f;
  --font-family:            'Scheherazade', serif;
  --font-weight-bold:       700;
  --font-weight-extrabold:  700;
  --line-height-relaxed:    1.8;
}
```

---

## ٤. كود CSS كامل (Complete CSS — جاهز للنسخ)

```css
/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Container + Cards
═══════════════════════════════════════════════════════════════ */
.dropdown-tables-wrap {
  margin: 14px 16px 0;
}
.dropdown-sentence-card {
  background: #fff;
  border: 2px solid var(--color-primary);
  border-radius: 16px;
  padding: 18px;
  box-shadow: 0 4px 16px rgba(132, 72, 22, 0.10);
  animation: fadeIn 0.4s ease;
}
.dropdown-sentence-header {
  text-align: center;
  margin-bottom: 16px;
}
.dropdown-sentence-num {
  font-size: 15px;
  font-weight: var(--font-weight-bold);
  color: var(--color-primary);
  background: var(--color-secondary-light);
  border: 1.5px solid var(--color-secondary);
  border-radius: 100px;
  padding: 5px 16px;
  display: inline-block;
  margin-bottom: 10px;
}
.dropdown-sentence-text {
  font-size: 24px;
  font-weight: var(--font-weight-extrabold);
  color: var(--color-ink);
  line-height: var(--line-height-relaxed);
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Table
═══════════════════════════════════════════════════════════════ */
.dropdown-table {
  width: 100%;
  border-collapse: collapse;
  font-family: var(--font-family);
  margin-bottom: 16px;
}
.dropdown-table thead th {
  background: var(--color-primary);
  color: #fff;
  font-size: 17px;
  font-weight: var(--font-weight-extrabold);
  padding: 12px 8px;
  border: 1.5px solid var(--color-primary-shadow);
  text-align: center;
}
.dropdown-row {
  transition: background 0.3s ease;
}
.dropdown-row td {
  padding: 10px;
  border: 1.5px solid var(--color-sand);
  text-align: center;
  vertical-align: middle;
}
.dropdown-word-cell {
  background: var(--color-secondary-light);
  font-size: 19px;
  font-weight: var(--font-weight-extrabold);
  color: var(--color-primary);
  width: 45%;
}
.dropdown-select-cell {
  background: #fff;
  padding: 12px !important;
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Custom Dropdown
═══════════════════════════════════════════════════════════════ */
.custom-dropdown {
  position: relative;
  display: inline-block;
  width: 100%;
  max-width: 220px;
  font-family: var(--font-family);
}
.custom-dropdown-trigger {
  width: 100%;
  padding: 10px 14px;
  font-family: var(--font-family);
  font-size: 17px;
  font-weight: var(--font-weight-bold);
  color: var(--color-ink);
  background: #fff;
  border: 2px solid var(--color-primary-light);
  border-radius: 10px;
  cursor: pointer;
  outline: none;
  transition: all 0.2s ease;
  direction: rtl;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 8px;
  -webkit-tap-highlight-color: transparent;
}
.custom-dropdown-trigger:hover {
  border-color: var(--color-secondary);
  background: #fffaf3;
}
.custom-dropdown.is-open .custom-dropdown-trigger {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(237, 198, 111, 0.30);
}
.custom-dropdown-value {
  flex: 1;
  text-align: center;
  line-height: 1.4;
}
.custom-dropdown-arrow {
  color: var(--color-primary);
  font-size: 14px;
  transition: transform 0.25s ease;
  flex-shrink: 0;
}
.custom-dropdown.is-open .custom-dropdown-arrow {
  transform: rotate(180deg);
}
.custom-dropdown-menu {
  position: absolute;
  top: calc(100% + 4px);
  bottom: auto;
  left: 0;
  right: 0;
  background: #fff;
  border: 2px solid var(--color-primary);
  border-radius: 12px;
  padding: 6px;
  list-style: none;
  margin: 0;
  box-shadow: 0 8px 24px rgba(132, 72, 22, 0.20);
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-8px);
  transition: opacity 0.2s ease, transform 0.2s ease, visibility 0.2s;
  max-height: min(280px, 50vh);
  overflow-y: auto;
  /* Smooth scrolling on iOS */
  -webkit-overflow-scrolling: touch;
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Drop-up variant
═══════════════════════════════════════════════════════════════ */
/* Drop-up variant: menu opens above the trigger when there's not enough space below */
.custom-dropdown.drop-up .custom-dropdown-menu {
  top: auto;
  bottom: calc(100% + 4px);
  transform: translateY(8px);
}
.custom-dropdown.is-open.drop-up .custom-dropdown-menu {
  transform: translateY(0);
}
.custom-dropdown.is-open .custom-dropdown-menu {
  opacity: 1;
  visibility: visible;
}
/* Open transform — overridden by drop-up rule above */
.custom-dropdown.is-open:not(.drop-up) .custom-dropdown-menu {
  transform: translateY(0);
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Scrollbar
═══════════════════════════════════════════════════════════════ */
/* Firefox: themed thin scrollbar (deep brown thumb on transparent track) */
.custom-dropdown-menu {
  scrollbar-width: thin;
  scrollbar-color: var(--color-primary) transparent;
}
/* WebKit (Chrome, Safari, iOS) */
.custom-dropdown-menu::-webkit-scrollbar {
  width: 5px;
}
.custom-dropdown-menu::-webkit-scrollbar-track {
  background: transparent;
  margin: 6px 2px;
}
.custom-dropdown-menu::-webkit-scrollbar-thumb {
  background: var(--color-primary);
  border-radius: 999px;
  transition: background 0.2s ease;
}
.custom-dropdown-menu::-webkit-scrollbar-thumb:hover {
  background: var(--color-primary-shadow);
}
.custom-dropdown-menu::-webkit-scrollbar-thumb:active {
  background: var(--color-primary-shadow);
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Options
═══════════════════════════════════════════════════════════════ */
.custom-dropdown-option {
  padding: 10px 14px;
  font-family: var(--font-family);
  font-size: 17px;
  font-weight: var(--font-weight-bold);
  color: var(--color-ink);
  border-radius: 8px;
  cursor: pointer;
  text-align: center;
  transition: background 0.15s ease, color 0.15s ease;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}
.custom-dropdown-option + .custom-dropdown-option {
  margin-top: 2px;
  border-top: 1px solid var(--color-primary-light);
  border-radius: 0;
}
.custom-dropdown-option:first-child {
  border-radius: 8px 8px 0 0;
}
.custom-dropdown-option:last-child {
  border-radius: 0 0 8px 8px;
}
.custom-dropdown-option:only-child {
  border-radius: 8px;
}
.custom-dropdown-option:hover {
  background: var(--color-secondary-light);
  color: var(--color-primary);
}
.custom-dropdown-option.is-selected {
  background: var(--color-primary);
  color: #fff;
}
.custom-dropdown.is-disabled .custom-dropdown-trigger {
  cursor: default;
  pointer-events: none;
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Row States (cascade into trigger + arrow)
═══════════════════════════════════════════════════════════════ */
.dropdown-row.correct-row {
  background: #f3f7e3;
}
.dropdown-row.correct-row .custom-dropdown-trigger {
  border-color: var(--color-success);
  border-width: 3px;
  background: #f3f7e3;
  color: var(--color-success);
  font-weight: var(--font-weight-extrabold);
}
.dropdown-row.correct-row .custom-dropdown-arrow {
  color: var(--color-success);
}
.dropdown-row.wrong-row {
  background: #fbeeea;
}
.dropdown-row.wrong-row .custom-dropdown-trigger {
  border-color: var(--color-error);
  border-width: 3px;
  background: #fbeeea;
  color: var(--color-error);
  animation: dropdownShake 0.5s ease;
}
.dropdown-row.wrong-row .custom-dropdown-arrow {
  color: var(--color-error);
}
.dropdown-row.revealed-row {
  background: #f3f7e3;
}
.dropdown-row.revealed-row .custom-dropdown-trigger {
  border-color: var(--color-success);
  border-width: 3px;
  background: #f3f7e3;
  color: var(--color-success);
  font-weight: var(--font-weight-extrabold);
  animation: pulse-correct 0.6s ease;
}
.dropdown-row.revealed-row .custom-dropdown-arrow {
  color: var(--color-success);
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Animations
═══════════════════════════════════════════════════════════════ */
@keyframes dropdownShake {
  0%, 100% { transform: translateX(0); }
  20% { transform: translateX(-6px); }
  40% { transform: translateX(6px); }
  60% { transform: translateX(-4px); }
  80% { transform: translateX(4px); }
}
@keyframes pulse-correct {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.02); }
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Feedback + Nav
═══════════════════════════════════════════════════════════════ */
.dropdown-sentence-fb {
  margin: 12px 0;
  min-height: 1px;
}
.dropdown-sentence-fb:empty {
  display: none;
}
.dropdown-sentence-nav {
  display: flex;
  justify-content: space-between;
  gap: 12px;
  flex-wrap: wrap;
}
.dropdown-sentence-nav .btn-action {
  min-width: 0;
  width: 50px;
  height: 50px;
  padding: 0;
  font-size: 20px;
  border-radius: 50%;
}
.dropdown-sentence-nav .btn-action i {
  font-size: 18px;
}
.dropdown-sentence-nav > div:empty {
  flex: 1;
}
.btn-sentence-next:not(:disabled) {
  animation: nextBtnPulse 2s ease-in-out infinite;
}
@keyframes nextBtnPulse {
  0%, 100% { box-shadow: 0 5px 0 var(--color-primary-shadow), 0 0 0 0 rgba(132, 72, 22, 0.35); }
  50%      { box-shadow: 0 5px 0 var(--color-primary-shadow), 0 0 0 12px rgba(132, 72, 22, 0); }
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Responsive 700px
═══════════════════════════════════════════════════════════════ */
@media (max-width: 700px) {
  .dropdown-sentence-card { padding: 14px; }
  .dropdown-sentence-text { font-size: 21px; }
  .dropdown-table thead th { font-size: 16px; padding: 10px 6px; }
  .dropdown-word-cell { font-size: 17px; }
  .custom-dropdown { max-width: 200px; }
  .custom-dropdown-trigger { font-size: 16px; padding: 9px 12px; }
  .custom-dropdown-option { font-size: 16px; padding: 9px 12px; }
  .custom-dropdown-menu { max-height: min(240px, 45vh); }
  .dropdown-sentence-nav .btn-action { width: 46px; height: 46px; font-size: 18px; }
  .dropdown-sentence-nav .btn-action i { font-size: 16px; }
}

/* ═══════════════════════════════════════════════════════════════
   DROPDOWN TABLES — Responsive 480px
═══════════════════════════════════════════════════════════════ */
@media (max-width: 480px) {
  .dropdown-tables-wrap { margin-left: 8px; margin-right: 8px; }
  .dropdown-sentence-card { padding: 12px; }
  .dropdown-sentence-text { font-size: 19px; }
  .dropdown-sentence-num { font-size: 13px; padding: 4px 12px; }
  .dropdown-table thead th { font-size: 14px; padding: 9px 4px; }
  .dropdown-word-cell { font-size: 16px; padding: 9px 6px; }
  .dropdown-select-cell { padding: 8px !important; }
  .custom-dropdown { max-width: 100%; }
  .custom-dropdown-trigger { font-size: 15px; padding: 9px 10px; }
  .custom-dropdown-option { font-size: 15px; padding: 9px 10px; }
  .custom-dropdown-menu { max-height: min(200px, 40vh); }
  .dropdown-sentence-nav { gap: 8px; }
  .dropdown-sentence-nav .btn-action { width: 44px; height: 44px; font-size: 16px; flex: 0 0 auto; padding: 0; gap: 0; }
  .dropdown-sentence-nav .btn-action i { font-size: 15px; }
}
```

---

## ٥. كود JavaScript كامل (Complete JS — جاهز للنسخ)

```javascript
/* ══════════════════════════════════════════════════════════════
   CONSTANTS
══════════════════════════════════════════════════════════════ */
const MAX_ATTEMPTS = 3;

/* ══════════════════════════════════════════════════════════════
   LABELS — label-to-display-value mapping
   Keys must match the `correct` values in SLIDE_DATA sentences.words[]
   These are the SAME object — Q3_LABELS used in review, Q3_OPTION_LABELS used in runtime.
══════════════════════════════════════════════════════════════ */
const Q3_LABELS = {
  fel:    'فعل',
  fael:   'فاعل',
  mafool: 'مفعول به',
  none:   'لا شيء مما ذُكر'
};

const Q3_OPTION_LABELS = {
  fel:    'فعل',
  fael:   'فاعل',
  mafool: 'مفعول به',
  none:   'لا شيء مما ذُكر'
};

/* ══════════════════════════════════════════════════════════════
   STATE INIT — dropdown_tables branch inside initSlideState()
══════════════════════════════════════════════════════════════ */
// Called once at startup for every slide.
// For dropdown_tables slides, produces this state shape:
//   {
//     type: 'dropdown_tables',
//     currentSentence: 0,
//     sentencesCompleted: [],          // index → true when sentence is fully done
//     cells: {}                        // key "sIdx-wIdx" → { selected, attempts, status }
//   }
function initSlideState() {
  for (let i = 1; i <= TOTAL_SLIDES; i++) {
    const data = SLIDE_DATA[i];
    if (!data) continue;
    if (data.type === 'mcq') {
      slideState[i] = { type: 'mcq', answers: {}, attempts: {}, status: {} };
    } else if (data.type === 'click') {
      slideState[i] = { type: 'click', attempts: 0, completed: false, correctSelected: new Set(), selected: new Set(), revealed: false };
    } else if (data.type === 'dropdown_tables') {
      slideState[i] = { type: 'dropdown_tables', currentSentence: 0, sentencesCompleted: [], cells: {} };
    }
  }
}

/* ══════════════════════════════════════════════════════════════
   BUILD SLIDE HTML — dropdown_tables branch inside buildAllSlides()
══════════════════════════════════════════════════════════════ */
// This is the HTML template produced by the dropdown_tables branch.
// Called once; output is injected into #slidesContainer.
// Replace `i` with the slide index and `data` with SLIDE_DATA[i].
function buildDropdownTablesHTML(i, data) {
  let html = `<div class="dropdown-tables-wrap" id="dropdownTablesWrap-${i}">`;

  data.sentences.forEach((sent, sIdx) => {
    html += `
      <div class="dropdown-sentence-card" data-sent-idx="${sIdx}" ${sIdx === 0 ? '' : 'style="display:none;"'}>
        <div class="dropdown-sentence-header">
          <div class="dropdown-sentence-text">${sent.text}</div>
        </div>
        <table class="dropdown-table">
          <thead>
            <tr>
              <th>الكلمةُ</th>
              <th>اختَرْ نوعَها</th>
            </tr>
          </thead>
          <tbody>
    `;
    sent.words.forEach((w, wIdx) => {
      html += `
        <tr class="dropdown-row" data-sent-idx="${sIdx}" data-word-idx="${wIdx}" data-correct="${w.correct}">
          <td class="dropdown-word-cell">${w.word}</td>
          <td class="dropdown-select-cell">
            <div class="custom-dropdown" data-sent-idx="${sIdx}" data-word-idx="${wIdx}">
              <button type="button" class="custom-dropdown-trigger" aria-haspopup="listbox" aria-expanded="false">
                <span class="custom-dropdown-value">اختَرْ</span>
                <i class="fas fa-chevron-down custom-dropdown-arrow"></i>
              </button>
              <ul class="custom-dropdown-menu" role="listbox">
                <li class="custom-dropdown-option" data-value="fel" role="option">فعل</li>
                <li class="custom-dropdown-option" data-value="fael" role="option">فاعل</li>
                <li class="custom-dropdown-option" data-value="mafool" role="option">مفعول به</li>
                <li class="custom-dropdown-option" data-value="none" role="option">لا شيء مما ذُكر</li>
              </ul>
            </div>
          </td>
        </tr>
      `;
    });
    html += `
          </tbody>
        </table>
        <div class="q-feedback dropdown-sentence-fb" id="fb-q${i}-s${sIdx}"></div>
        <div class="dropdown-sentence-nav">
          ${sIdx > 0
            ? `<button class="btn-action btn-nav btn-sentence-prev" onclick="goToSentence(${i}, ${sIdx - 1})" aria-label="الجملة السابقة"><i class="fas fa-arrow-right"></i></button>`
            : '<div></div>'}
          ${sIdx < data.sentences.length - 1
            ? `<button class="btn-action btn-nav btn-sentence-next" data-sent-idx="${sIdx}" onclick="goToSentence(${i}, ${sIdx + 1})" disabled aria-label="الجملة التالية"><i class="fas fa-arrow-left"></i></button>`
            : '<div></div>'}
        </div>
      </div>
    `;
  });

  html += `</div>`;
  return html;
}

/* ══════════════════════════════════════════════════════════════
   ATTACH DROPDOWN HANDLERS — click-open + outside-click-close
══════════════════════════════════════════════════════════════ */

// Register ONCE per page: close any open dropdown when clicking outside it
function initDropdownGlobalClose() {
  document.addEventListener('click', (e) => {
    document.querySelectorAll('.custom-dropdown.is-open').forEach(dd => {
      if (!dd.contains(e.target)) {
        dd.classList.remove('is-open');
        dd.querySelector('.custom-dropdown-trigger').setAttribute('aria-expanded', 'false');
      }
    });
  });
}

// Wire up open/close and option-selection events for every dropdown in a slide
function renderDropdownTables(slideIdx) {
  const data = SLIDE_DATA[slideIdx];
  if (!data || data.type !== 'dropdown_tables') return;
  const wrap = document.getElementById(`dropdownTablesWrap-${slideIdx}`);
  if (!wrap) return;
  const state = slideState[slideIdx];

  wrap.querySelectorAll('.custom-dropdown').forEach(dd => {
    const sIdx = parseInt(dd.dataset.sentIdx);
    const wIdx = parseInt(dd.dataset.wordIdx);
    const key = sIdx + '-' + wIdx;
    const trigger = dd.querySelector('.custom-dropdown-trigger');
    const valueSpan = dd.querySelector('.custom-dropdown-value');

    // initialise per-cell state
    if (!state.cells[key]) {
      state.cells[key] = { selected: null, attempts: 0, status: null };
    }

    // open / close trigger
    trigger.addEventListener('click', (e) => {
      e.stopPropagation();
      if (dd.classList.contains('is-disabled')) return;
      // close other open dropdowns in same slide
      wrap.querySelectorAll('.custom-dropdown.is-open').forEach(other => {
        if (other !== dd) {
          other.classList.remove('is-open');
          other.querySelector('.custom-dropdown-trigger').setAttribute('aria-expanded', 'false');
        }
      });
      const wasOpen = dd.classList.contains('is-open');
      // determine drop-up vs drop-down before opening
      if (!dd.classList.contains('is-open')) {
        checkDropdownDirection(dd);
      }
      dd.classList.toggle('is-open');
      trigger.setAttribute('aria-expanded', !wasOpen);
    });

    // option selection
    dd.querySelectorAll('.custom-dropdown-option').forEach(opt => {
      opt.addEventListener('click', (e) => {
        e.stopPropagation();
        const selectedValue = opt.dataset.value;
        const labelText = Q3_OPTION_LABELS[selectedValue];

        // close immediately
        dd.classList.remove('is-open');
        trigger.setAttribute('aria-expanded', 'false');

        handleDropdownSelection(slideIdx, dd, sIdx, wIdx, selectedValue, labelText, valueSpan);
      });
    });
  });
}

/* ══════════════════════════════════════════════════════════════
   CHECK DROPDOWN DIRECTION (drop-up detector)
══════════════════════════════════════════════════════════════ */
// Temporarily measures the menu height, compares space above/below viewport,
// and adds/removes .drop-up class accordingly.
function checkDropdownDirection(dd) {
  if (!dd) return;
  const trigger = dd.querySelector('.custom-dropdown-trigger');
  const menu = dd.querySelector('.custom-dropdown-menu');
  if (!trigger || !menu) return;

  const wasOpen = dd.classList.contains('is-open');
  const prevVis = menu.style.visibility;
  const prevDisp = menu.style.display;
  if (!wasOpen) {
    menu.style.visibility = 'hidden';
    menu.style.display = 'block';
  }
  const triggerRect = trigger.getBoundingClientRect();
  const menuHeight = menu.scrollHeight;
  const spaceBelow = window.innerHeight - triggerRect.bottom;
  const spaceAbove = triggerRect.top;

  if (!wasOpen) {
    menu.style.visibility = prevVis;
    menu.style.display = prevDisp;
  }

  // Add 8px safety margin; require 80% of menu height to fit
  const needed = h => h * 0.8 + 8;
  if (spaceBelow < needed(menuHeight) && spaceAbove > spaceBelow) {
    dd.classList.add('drop-up');
  } else {
    dd.classList.remove('drop-up');
  }
}

/* ══════════════════════════════════════════════════════════════
   HANDLE DROPDOWN SELECTION — core feedback / state logic
══════════════════════════════════════════════════════════════ */
function handleDropdownSelection(slideIdx, dd, sIdx, wIdx, selectedValue, labelText, valueSpan) {
  const key = sIdx + '-' + wIdx;
  const state = slideState[slideIdx].cells[key];
  // ignore if already locked (correct or revealed)
  if (state.status === 'correct' || state.status === 'revealed') return;
  if (state.attempts >= MAX_ATTEMPTS) return;

  state.attempts++;
  const row = dd.closest('.dropdown-row');
  const correctValue = row.dataset.correct;
  const isCorrect = selectedValue === correctValue;

  const fb = document.getElementById(`fb-q${slideIdx}-s${sIdx}`);
  // clear any pending feedback timeout from a previous interaction in this sentence
  if (fb && fb._fbTimeoutId) {
    clearTimeout(fb._fbTimeoutId);
    fb._fbTimeoutId = null;
  }

  // show selected label in trigger
  valueSpan.textContent = labelText;

  // clear previous row state
  row.classList.remove('correct-row', 'wrong-row', 'revealed-row');

  if (isCorrect) {
    // ✅ CORRECT
    state.selected = selectedValue;
    state.status = 'correct';
    row.classList.add('correct-row');
    dd.classList.add('is-disabled');
    // short "أحسنتَ!" toast — auto-clears unless sentence-complete overwrites it
    if (fb) {
      fb.className = 'q-feedback dropdown-sentence-fb show-correct';
      fb.innerHTML = '<i class="fas fa-check-circle"></i> أحسنتَ!';
      fb._fbTimeoutId = setTimeout(() => {
        if (fb.classList.contains('show-correct') && !fb.dataset.persistent) {
          fb.className = 'q-feedback dropdown-sentence-fb';
          fb.innerHTML = '';
        }
        fb._fbTimeoutId = null;
      }, 1500);
    }
    checkSentenceComplete(slideIdx, sIdx);

  } else {
    // ❌ WRONG — force animation restart via reflow
    row.classList.remove('wrong-row');
    void row.offsetWidth;
    row.classList.add('wrong-row');

    if (state.attempts >= MAX_ATTEMPTS) {
      // 3 wrong attempts → reveal correct answer
      state.selected = correctValue;
      state.status = 'revealed';
      // "reveal" toast (green styling)
      if (fb) {
        fb.className = 'q-feedback dropdown-sentence-fb show-correct';
        fb.innerHTML = '<i class="fas fa-check-circle"></i> أحسنتَ على المحاولةِ! تعرَّفْ على الإجابةِ الصّحيحةِ';
        fb._fbTimeoutId = setTimeout(() => {
          if (fb.classList.contains('show-correct') && !fb.dataset.persistent) {
            fb.className = 'q-feedback dropdown-sentence-fb';
            fb.innerHTML = '';
          }
          fb._fbTimeoutId = null;
        }, 2500);
      }
      // after 3000ms: switch row to revealed-row, show correct answer, lock
      setTimeout(() => {
        row.classList.remove('wrong-row');
        row.classList.add('revealed-row');
        valueSpan.textContent = Q3_OPTION_LABELS[correctValue];
        dd.classList.add('is-disabled');
        checkSentenceComplete(slideIdx, sIdx);
      }, 3000);

    } else {
      // fewer than 3 attempts: show "try again" toast, reset row after 1500ms
      if (fb) {
        fb.className = 'q-feedback dropdown-sentence-fb show-wrong';
        fb.innerHTML = '<i class="fas fa-times-circle"></i> حاولْ مرّةً أخرى';
      }
      setTimeout(() => {
        row.classList.remove('wrong-row');
        valueSpan.textContent = 'اختَرْ';
        if (fb && fb.classList.contains('show-wrong')) {
          fb.className = 'q-feedback dropdown-sentence-fb';
          fb.innerHTML = '';
        }
      }, 1500);
    }
  }
}

/* ══════════════════════════════════════════════════════════════
   CHECK SENTENCE COMPLETE
══════════════════════════════════════════════════════════════ */
function checkSentenceComplete(slideIdx, sIdx) {
  const data = SLIDE_DATA[slideIdx];
  const state = slideState[slideIdx];
  const sent = data.sentences[sIdx];
  const allDone = sent.words.every((w, wIdx) => {
    const key = sIdx + '-' + wIdx;
    const cell = state.cells[key];
    return cell && (cell.status === 'correct' || cell.status === 'revealed');
  });
  if (allDone) {
    state.sentencesCompleted[sIdx] = true;
    // enable the "next sentence" button for this sentence card
    const wrap = document.getElementById(`dropdownTablesWrap-${slideIdx}`);
    const card = wrap ? wrap.querySelector(`.dropdown-sentence-card[data-sent-idx="${sIdx}"]`) : null;
    if (card) {
      const nextBtn = card.querySelector('.btn-sentence-next');
      if (nextBtn) nextBtn.disabled = false;
    }
    // persistent completion toast — overrides any transient toast
    const fb = document.getElementById(`fb-q${slideIdx}-s${sIdx}`);
    if (fb) {
      if (fb._fbTimeoutId) { clearTimeout(fb._fbTimeoutId); fb._fbTimeoutId = null; }
      fb.className = 'q-feedback dropdown-sentence-fb show-correct';
      fb.innerHTML = '<i class="fas fa-check-circle"></i> ممتاز! أكملتَ الجملةَ';
      fb.dataset.persistent = 'true';
    }
    updateNavButtons();
    updateProgressBar();
  }
}

/* ══════════════════════════════════════════════════════════════
   GO TO SENTENCE (sentence-level pagination)
══════════════════════════════════════════════════════════════ */
function goToSentence(slideIdx, targetIdx) {
  const data = SLIDE_DATA[slideIdx];
  if (targetIdx < 0 || targetIdx >= data.sentences.length) return;
  const wrap = document.getElementById(`dropdownTablesWrap-${slideIdx}`);
  if (!wrap) return;
  wrap.querySelectorAll('.dropdown-sentence-card').forEach(card => {
    const idx = parseInt(card.dataset.sentIdx);
    card.style.display = idx === targetIdx ? '' : 'none';
  });
  slideState[slideIdx].currentSentence = targetIdx;
  wrap.scrollIntoView({ behavior: 'smooth', block: 'start' });
}

/* ══════════════════════════════════════════════════════════════
   IS SLIDE ANSWERED — dropdown_tables branch
══════════════════════════════════════════════════════════════ */
// Called by isSlideAnswered(idx) — returns true only when all sentences are complete.
// if (data.type === 'dropdown_tables') {
//   return data.sentences.every((s, sIdx) => state.sentencesCompleted[sIdx]);
// }
```

---

## ٦. بنية HTML المُنتَجة (Generated HTML Structure)

HTML كامل لبطاقة جملة واحدة مع العناصر `${…}` الموضحة لكل متغير:

```html
<!-- بطاقة الجملة — sIdx = 0 (أولى، تظهر؛ بقية الجمل display:none) -->
<div class="dropdown-sentence-card"
     data-sent-idx="0">              <!-- ← رقم الجملة (0-based) -->

  <!-- رأس البطاقة: نص الجملة الكامل -->
  <div class="dropdown-sentence-header">
    <!-- .dropdown-sentence-num اختياري — يُضاف لو أردنا "الجملة ١ من ٣" -->
    <div class="dropdown-sentence-text">انتصرَ الأسرى في المعركةِ.</div>
                                      <!-- ← sent.text -->
  </div>

  <!-- جدول الكلمات -->
  <table class="dropdown-table">
    <thead>
      <tr>
        <th>الكلمةُ</th>
        <th>اختَرْ نوعَها</th>
      </tr>
    </thead>
    <tbody>

      <!-- صف واحد لكل كلمة -->
      <tr class="dropdown-row"
          data-sent-idx="0"            <!-- ← sIdx -->
          data-word-idx="0"            <!-- ← wIdx -->
          data-correct="fel">          <!-- ← w.correct (مفتاح من LABELS) -->

        <td class="dropdown-word-cell">انتصرَ</td>   <!-- ← w.word -->

        <td class="dropdown-select-cell">
          <div class="custom-dropdown"
               data-sent-idx="0"       <!-- ← sIdx -->
               data-word-idx="0">      <!-- ← wIdx -->

            <button type="button"
                    class="custom-dropdown-trigger"
                    aria-haspopup="listbox"
                    aria-expanded="false">
              <span class="custom-dropdown-value">اختَرْ</span>
              <i class="fas fa-chevron-down custom-dropdown-arrow"></i>
            </button>

            <ul class="custom-dropdown-menu" role="listbox">
              <!-- الخيارات — data-value يجب أن يطابق مفاتيح LABELS -->
              <li class="custom-dropdown-option" data-value="fel"    role="option">فعل</li>
              <li class="custom-dropdown-option" data-value="fael"   role="option">فاعل</li>
              <li class="custom-dropdown-option" data-value="mafool" role="option">مفعول به</li>
              <li class="custom-dropdown-option" data-value="none"   role="option">لا شيء مما ذُكر</li>
            </ul>

          </div>
        </td>
      </tr>

      <!-- ... باقي صفوف الكلمات ... -->

    </tbody>
  </table>

  <!-- منطقة الفيدباك — id مرتبط بالسلايد والجملة -->
  <div class="q-feedback dropdown-sentence-fb" id="fb-q3-s0"></div>
                                                   <!-- ← fb-q{slideIdx}-s{sIdx} -->

  <!-- شريط التنقل بين الجمل -->
  <div class="dropdown-sentence-nav">
    <!-- زر السابق: مخفي في الجملة الأولى (sIdx===0 → <div></div>) -->
    <div></div>

    <!-- زر التالي: disabled حتى اكتمال جميع الخانات؛ مخفي في الجملة الأخيرة -->
    <button class="btn-action btn-nav btn-sentence-next"
            data-sent-idx="0"          <!-- ← sIdx -->
            onclick="goToSentence(3, 1)"  <!-- ← goToSentence(slideIdx, sIdx+1) -->
            disabled
            aria-label="الجملة التالية">
      <i class="fas fa-arrow-left"></i>
    </button>
  </div>

</div>
```

---

## ٧. بنية البيانات (Data Structure)

### إدخال SLIDE_DATA

```javascript
const SLIDE_DATA = {
  // ...
  3: {
    title: "السّؤالُ الثّالثُ",
    type: "dropdown_tables",                     // ← ثابت، لا يتغير
    instr: "<strong>السّؤالُ الثّالثُ:</strong> أستخرجُ الفعلَ والفاعلَ والمفعولَ بهِ (إن وجد) من الجملِ الفعليّةِ الآتيةِ.",
    sentences: [
      {
        text: "انتصرَ الأسرى في المعركةِ.",       // ← النص الكامل للجملة (يظهر فوق الجدول)
        words: [
          { word: "انتصرَ",   correct: "fel"   },  // ← word: الكلمة، correct: مفتاح من LABELS
          { word: "الأسرى",   correct: "fael"  },
          { word: "في",       correct: "none"  },
          { word: "المعركةِ", correct: "none"  }
        ]
      },
      {
        text: "استعادَ الشّعبُ حقوقَهُ.",
        words: [
          { word: "استعادَ", correct: "fel"    },
          { word: "الشّعبُ", correct: "fael"   },
          { word: "حقوقَهُ", correct: "mafool" }
        ]
      },
      // ... المزيد من الجمل
    ]
  }
};
```

### خريطة LABELS

```javascript
// Q3_LABELS / Q3_OPTION_LABELS — يجب أن تغطي كل القيم المستخدمة في correct
// المفاتيح اعتباطية/قابلة للتخصيص — القيم هي النصوص التي تظهر في الواجهة
const Q3_LABELS = {
  fel:    'فعل',
  fael:   'فاعل',
  mafool: 'مفعول به',
  none:   'لا شيء مما ذُكر'
  // أضف مفاتيح جديدة حسب طبيعة السؤال
  // مثال لسؤال آخر: mubtada: 'مبتدأ', khabar: 'خبر'
};
```

**ملاحظة:** المفاتيح في `correct` (مثل `"fel"`, `"fael"`) تستخدم داخلياً فقط للمطابقة؛ أسماء الكلاسات ونصوص القائمة تُقرأ من `Q3_LABELS`. يمكنك استخدام أي مفاتيح إنجليزية منطقية.

---

## ٨. السلوك التفصيلي (Behavior)

### عند اختيار صح
- صف الجدول يصير أخضر فاتح (`.correct-row`)
- الـ dropdown trigger يصير أخضر (border + text)
- السهم يصير أخضر (`.custom-dropdown-arrow`)
- الـ dropdown يصير `.is-disabled` (لا يفتح مجدداً)
- فيدباك تحت الجدول: `<i class="fas fa-check-circle"></i> أحسنتَ!` بكلاس `show-correct` لمدة 1500ms ثم يُمسح تلقائياً
- إذا كانت هذه آخر خانة في الجملة → تُطلَق `checkSentenceComplete()`

### عند اختيار خطأ (قبل 3 محاولات)
- الصف يصير أحمر (`.wrong-row`) + `dropdownShake` animation (reflow يُعيد الأنيميشن في كل محاولة)
- فيدباك: `<i class="fas fa-times-circle"></i> حاولْ مرّةً أخرى` بكلاس `show-wrong`
- بعد 1500ms: الصف يرجع عادي + الـ dropdown value يرجع "اختَرْ" + الفيدباك يُمسح

### بعد 3 محاولات خاطئة
- الفيدباك الفوري (يظهر مع آخر محاولة): `أحسنتَ على المحاولةِ! تعرَّفْ على الإجابةِ الصّحيحةِ` بكلاس `show-correct` لمدة 2500ms
- بعد **3000ms** (تأخير أطول): الصف ينتقل من `.wrong-row` إلى `.revealed-row` + pulse animation
- الـ dropdown يعرض الإجابة الصحيحة + `.is-disabled`
- تُطلَق `checkSentenceComplete()`

### عند إكمال كل dropdowns في الجملة
- `checkSentenceComplete()` تُلغي أي timeout فيدباك معلّق
- الفيدباك يصبح **دائماً** (persistent): `<i class="fas fa-check-circle"></i> ممتاز! أكملتَ الجملةَ`
- `fb.dataset.persistent = 'true'` يمنع timeouts لاحقة من مسحه
- زر "الجملة التالية" يصير مفعّلاً (`disabled` يُزال)
- تُستدعى `updateNavButtons()` و `updateProgressBar()`

### Drop-up Logic
- قبل فتح dropdown، تُستدعى `checkDropdownDirection(dd)`:
  - تُخفي القائمة مؤقتاً وتقيس `scrollHeight` لمعرفة الارتفاع الكامل
  - تقارن `spaceBelow` vs `spaceAbove` مع هامش أمان 8px وعتبة 80% من ارتفاع القائمة
  - إذا كانت المساحة تحت غير كافية والمساحة فوق أكبر → تُضيف `.drop-up` → القائمة تفتح للأعلى

---

## ٩. التوقيتات (Timings)

| الحالة | المدة |
|---|---|
| Correct toast (`أحسنتَ!`) | 1500ms ثم auto-clear |
| Wrong toast + row reset (`حاولْ مرّةً أخرى`) | 1500ms |
| Revealed toast (`أحسنتَ على المحاولةِ!`) | 2500ms ثم auto-clear |
| Reveal row delay (بعد 3 محاولات خاطئة) | 3000ms |
| Sentence completion toast | دائم (persistent) — لا يُمسح |

---

## ١٠. الأيقونات (Icons)

| الحالة | Font Awesome class |
|---|---|
| Correct / Completion / Revealed | `fa-check-circle` |
| Wrong | `fa-times-circle` |
| Navigation (next) | `fa-arrow-left` |
| Navigation (prev) | `fa-arrow-right` |
| Dropdown arrow | `fa-chevron-down` |

---

## ١١. ممنوعات (Forbidden)

- **عرض عداد المحاولات للطالب** — ممنوع منعاً باتاً؛ العداد داخلي فقط
- **`fa-info-circle`** — استخدم `fa-circle-info` للمعلومات فقط (وليس في هذا القالب)
- **`show-revealed` className** — غير موجودة في القالب؛ نستخدم `show-correct` لكل حالات النجاح (صح + مكشوف + اكتمال)
- **إضافة حالات لون جديدة** خارج `correct-row` / `wrong-row` / `revealed-row`
- **تعديل `overflow: hidden`** على `.task-app` — إزالته مقصودة لمنع قطع القوائم المنسدلة؛ لا تعد إضافته

---

## ١٢. القابل للتعديل والثابت (Tweakable vs Fixed)

**SAFE — يمكن تغييره:**
- عدد الجمل (طول `sentences[]`)
- عدد الكلمات لكل جملة (طول `words[]`)
- `LABELS` mapping — مفاتيح وقيم مخصصة لكل سؤال
- `max-width` للـ dropdown (القيمة الافتراضية 220px)
- نص الجملة (`sent.text`) وكلمات الجدول (`w.word`)
- عنوان السؤال (`title`) ونص التعليمات (`instr`)

**FIXED — لا تلمسه:**
- الألوان (جميع قيم الـ CSS variables)
- الـ animations (`dropdownShake`, `pulse-correct`, `nextBtnPulse`)
- `MAX_ATTEMPTS` — دائماً 3
- نصوص الفيدباك (أحسنتَ! / حاولْ مرّةً أخرى / أحسنتَ على المحاولةِ! / ممتاز! أكملتَ الجملةَ)
- التوقيتات (1500ms / 2500ms / 3000ms)
- Row state classes (`correct-row` / `wrong-row` / `revealed-row`)

---

## ١٣. القائمة المرجعية لإنشاء سؤال جديد (Checklist)

- [ ] أضف entry لـ `SLIDE_DATA` بـ `type: "dropdown_tables"`
- [ ] أضف: `title`, `instr`, `sentences[]`
- [ ] كل sentence: `text` (نص الجملة), `words[]`
- [ ] كل word: `word` (الكلمة للعرض), `correct` (مفتاح من LABELS)
- [ ] تأكد أن `Q3_LABELS` / `Q3_OPTION_LABELS` يغطيان كل قيم `correct` المستخدمة
- [ ] استورد CSS من القسم ٤ بدون تعديل (أو تأكد من وجوده في ملف الـ CSS المشترك)
- [ ] استورد JS من القسم ٥ بدون تعديل
- [ ] زِد `TOTAL_SLIDES` ليشمل السؤال الجديد
- [ ] داخل `initSlideState()`: تأكد من وجود فرع `dropdown_tables`
- [ ] داخل init loop: `renderDropdownTables(i)` يُستدعى للسلايد الجديد
- [ ] تأكد أن `.task-app` ليس عليه `overflow: hidden`
- [ ] اختبر: drop-up (ضع الجدول قرب أسفل الشاشة), scrollbar للقوائم الطويلة
- [ ] اختبر: feedback timing (1500ms / 3000ms), sentence-complete gating
- [ ] اختبر: التنقل بين الجمل (prev/next)، وأن next يبقى disabled حتى الاكتمال

---

**نهاية المواصفة**
