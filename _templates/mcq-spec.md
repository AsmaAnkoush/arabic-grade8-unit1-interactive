# قالب نشاط الاختيار من متعدد (MCQ)

> هذا الملف يوثّق نمط نشاط MCQ الموحّد في كل دروس مشروع "وفوق كل ذي علم عليم".
> المرجع المعتمد: الاختبار التشخيصي.html — السؤال الأول ("أختارُ الإجابةَ الصّحيحةَ")
> آخر تحديث: 2026-05-29

---

## ١. متى يُستخدم

يُستخدم نشاط MCQ حين يحتوي السؤال على سؤال واحد أو أسئلة متعددة، لكل منها مجموعة خيارات ثابتة وإجابة صحيحة واحدة فقط. يُناسب هذا القالب:

- الأسئلة المفاهيمية: "ما تعريف...؟" أو "أيٌّ مما يأتي..."
- الأسئلة التي تختبر الحفظ أو الاستيعاب بخيارين أو أكثر
- حين يُراد تحقق فوري عند الاختيار (لا زر "تحقق" منفصل)
- حين يكفي 3 محاولات قبل الكشف عن الإجابة الصحيحة

**السمات المميِّزة لهذا القالب:**
- كل سؤال في بطاقة مستقلة `.q-card` مع زر سماعة + رقم + نص
- الأسئلة تظهر تباعاً (progressive reveal): السؤال التالي لا يظهر إلا بعد الإجابة على الحالي
- تحقق فوري عند اختيار الخيار (بدون زر إضافي)
- ثلاث محاولات لكل سؤال، بعدها تُكشف الإجابة الصحيحة بعد 3000ms
- زر "التالي" لا يُفعَّل إلا بعد الإجابة على كل الأسئلة في السلايد

---

## ٢. البنية الكاملة (HTML)

البنية الناتجة عن `renderMCQQuestions(slideIdx)` — مثال لسؤالين:

```html
<!-- الحاوية الرئيسية للأسئلة — تُولَّد داخل .slide -->
<div class="questions-container" id="qContainer-1">

  <!-- ═══ السؤال الأول (يظهر دائماً عند البداية) ═══ -->
  <div class="q-card" data-q-idx="0">

    <!-- رأس البطاقة: زر السماعة + رقم السؤال + نص السؤال -->
    <div class="q-card-header">
      <button class="audio-btn"
              onclick="toggleSimpleAudio(this)"
              aria-label="استمع للسؤال">
        <i class="fas fa-volume-high"></i>
      </button>
      <p class="q-card-title">
        <span class="q-num-inline">١.</span>
        ............... تغيُّرُ العلامةِ الموجودةِ في آخرِ الكلمةِ.
      </p>
    </div>

    <!-- منطقة الخيارات: كل خيار label يحتوي radio + نص -->
    <div class="mcq-options">

      <label class="mcq-option-label">
        <input type="radio" name="q-1-0" value="0">
        <span class="mcq-option-text">أ. الإعرابُ</span>
      </label>

      <label class="mcq-option-label">
        <input type="radio" name="q-1-0" value="1">
        <span class="mcq-option-text">ب. البناءُ</span>
      </label>

    </div>

    <!-- منطقة الفيدباك — id: fb-q{slideIdx}-{qIdx} -->
    <div class="q-feedback" id="fb-q1-0"></div>

  </div>

  <!-- ═══ السؤال الثاني (مخفي حتى الإجابة على الأول) ═══ -->
  <div class="q-card hidden-reveal" data-q-idx="1">
    <div class="q-card-header">
      <button class="audio-btn"
              onclick="toggleSimpleAudio(this)"
              aria-label="استمع للسؤال">
        <i class="fas fa-volume-high"></i>
      </button>
      <p class="q-card-title">
        <span class="q-num-inline">٢.</span>
        ............... لزومُ آخرِ الكلمةِ حركةً واحدةً لا تتغيّرُ.
      </p>
    </div>
    <div class="mcq-options">
      <label class="mcq-option-label">
        <input type="radio" name="q-1-1" value="0">
        <span class="mcq-option-text">أ. الإعرابُ</span>
      </label>
      <label class="mcq-option-label">
        <input type="radio" name="q-1-1" value="1">
        <span class="mcq-option-text">ب. البناءُ</span>
      </label>
    </div>
    <div class="q-feedback" id="fb-q1-1"></div>
  </div>

</div>
```

### ملاحظات البنية:
- `name="q-{slideIdx}-{qIdx}"` — يجمع radio buttons الخاصة بكل سؤال فريد
- `value="{i}"` — الفهرس العددي للخيار (0-based، يُطابق `correct` في SLIDE_DATA)
- `id="fb-q{slideIdx}-{qIdx}"` — فريد لكل سؤال، يُستخدم من JS للفيدباك
- `.hidden-reveal` — تُزال عند الإجابة على السؤال السابق (progressive reveal)

---

## ٣. CSS كامل

> انسخ هذه الأقسام كاملةً في `<style>` داخل ملف الدرس. لا تغيّر أي قيمة.

### ٣.١ زر السماعة (.audio-btn) + متغيرات الجذر

```css
/* ══ متغيرات الجذر الخاصة بالسماعة ══ */
:root {
  --audio-btn-size:      36px;   /* حجم زر السماعة */
  --audio-btn-radius:    9px;    /* نصف قطر حواف زر السماعة */
  --audio-btn-icon-size: 16px;   /* حجم أيقونة السماعة */
}

/* ══════════════════════════════════════════════════════════════
   AUDIO BUTTON
══════════════════════════════════════════════════════════════ */
.audio-btn {
  background: var(--color-primary);
  height: var(--audio-btn-size);
  width: var(--audio-btn-size);
  min-width: var(--audio-btn-size);
  border: none;
  border-radius: var(--audio-btn-radius);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  font-size: var(--audio-btn-icon-size);
  flex-shrink: 0;
  box-shadow: 0 3px 0 var(--color-primary-shadow);
  cursor: pointer;
  touch-action: manipulation;
  transition: background 0.2s ease, box-shadow 0.2s ease, transform 0.15s ease;
}
.audio-btn:hover  { transform: scale(1.05); background: var(--color-primary-hover); }
.audio-btn:active { transform: scale(0.97); box-shadow: 0 2px 0 var(--color-primary-shadow); }
.audio-btn.state-playing {
  background: var(--color-success);
  box-shadow: 0 3px 0 #4d6618;
  animation: audioPulse 1.5s ease-in-out infinite;
}
@keyframes audioPulse {
  0%, 100% { transform: scale(1); }
  50%      { transform: scale(1.05); }
}

/* أجهزة اللمس: تعطيل hover + حد أدنى للمساحة اللمسية */
@media (hover: none) {
  .audio-btn:hover { transform: none; background: var(--color-primary); }
  .audio-btn { min-width: 44px; min-height: 44px; }
}
```

### ٣.٢ بطاقة السؤال (.q-card + .q-card-header + .q-card-title)

```css
/* ══════════════════════════════════════════════════════════════
   QUESTION CARD — base + correct/wrong states
══════════════════════════════════════════════════════════════ */
.q-card {
  background: #fffdf8;
  border: 2px solid var(--color-primary);
  border-radius: 20px;
  padding: 16px 18px;
  margin: 14px 16px 0;
  box-shadow: 0 4px 16px rgba(132, 72, 22, 0.10);
  transition: border-color 0.3s ease, background 0.3s ease;
}
.q-card.q-card-correct {
  border-color: var(--color-success);
  background: #f3f7e3;
  border-width: 3px;
}
.q-card.q-card-wrong {
  border-color: var(--color-error);
  background: #fbeeea;
  border-width: 3px;
  animation: qCardShake 0.5s;
}
@keyframes qCardShake {
  0%, 100% { transform: translateX(0); }
  20% { transform: translateX(-6px); }
  40% { transform: translateX(6px); }
  60% { transform: translateX(-4px); }
  80% { transform: translateX(4px); }
}

/* ══ رأس البطاقة ══ */
.q-card-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 12px;
}
.q-num-inline {
  color: var(--color-primary-shadow);
  font-weight: var(--font-weight-extrabold);
  font-size: 1.2em;
  margin-left: 0;
}
.q-card-title {
  font-size: var(--font-size-instructions); /* 24px */
  font-weight: var(--font-weight-bold);
  color: var(--color-ink);
  line-height: var(--line-height-relaxed);
  margin: 0;
  flex: 1;
}

/* ══ Progressive reveal (ظهور تدريجي للأسئلة) ══ */
.q-card.hidden-reveal {
  display: none;
}
.q-card.revealing {
  animation: qCardReveal 0.55s cubic-bezier(0.34, 1.56, 0.64, 1);
}
@keyframes qCardReveal {
  0%   { opacity: 0; transform: translateY(24px) scale(0.95); }
  60%  { opacity: 1; }
  100% { opacity: 1; transform: translateY(0) scale(1); }
}
```

### ٣.٣ خيارات MCQ (.mcq-options + .mcq-option-label + الحالات)

```css
/* ══════════════════════════════════════════════════════════════
   MCQ OPTIONS — base + hover + radio + state classes
══════════════════════════════════════════════════════════════ */
.mcq-options {
  display: grid;
  gap: 10px;
}

/* ══ القاعدة الأساسية للخيار ══ */
.mcq-option-label {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 14px;
  border: 2px solid var(--color-primary-light);
  border-radius: 14px;
  background: #fffaf3;
  cursor: pointer;
  transition: all 0.2s ease;
  font-weight: var(--font-weight-bold);
  font-family: var(--font-family);
}
.mcq-option-label:hover {
  background: var(--color-primary-light);
  border-color: var(--color-secondary);
}
.mcq-option-label input[type="radio"] {
  accent-color: var(--color-primary);
  transform: scale(1.15);
  cursor: pointer;
  flex-shrink: 0;
}
.mcq-option-text {
  font-size: 20px;
  color: var(--color-ink);
  line-height: var(--line-height-normal); /* 1.7 */
}

/* حالة محدد قبل التحقق (اختياري — غير مُفعَّل في التحقق الفوري) */
.mcq-option-label.checked-selection {
  border-color: var(--color-primary);
  border-width: 3px;
  background: var(--color-secondary-light);
  box-shadow: 0 0 0 3px rgba(237, 198, 111, 0.25);
}

/* ══ حالة الإجابة الصحيحة ══ */
.mcq-option-label.correct-option {
  border-color: var(--color-success) !important;
  border-width: 3px !important;
  background: #f3f7e3 !important;
  animation: optionPulseCorrect 0.6s ease;
}
.mcq-option-label.correct-option .mcq-option-text {
  color: var(--color-success);
  font-weight: var(--font-weight-extrabold);
}

/* ══ حالة الإجابة الخاطئة ══ */
.mcq-option-label.wrong-option {
  border-color: var(--color-error) !important;
  border-width: 3px !important;
  background: #fbeeea !important;
}
.mcq-option-label.wrong-option .mcq-option-text {
  color: var(--color-error);
  font-weight: var(--font-weight-extrabold);
}

/* ══ حالة الكشف بعد 3 محاولات ══ */
.mcq-option-label.revealed {
  border-color: var(--color-success) !important;
  border-width: 3px !important;
  background: #f3f7e3 !important;
  animation: optionPulseCorrect 0.6s ease;
}
.mcq-option-label.revealed .mcq-option-text {
  color: var(--color-success);
  font-weight: var(--font-weight-extrabold);
}

/* ══ حالة القفل ══ */
.mcq-option-label.disabled-option {
  pointer-events: none;
  cursor: default;
}

/* ══ Animation: نبضة الصح/الكشف ══ */
@keyframes optionPulseCorrect {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.02); }
}

/* أجهزة اللمس: تعطيل hover */
@media (hover: none) {
  .mcq-option-label:hover {
    background: #fffaf3;
    border-color: var(--color-primary-light);
  }
}
```

### ٣.٤ صندوق الفيدباك (.q-feedback + show-correct + show-wrong)

```css
/* ══════════════════════════════════════════════════════════════
   FEEDBACK BOX
══════════════════════════════════════════════════════════════ */
.q-feedback {
  background: #fff;
  border: 2px solid var(--color-light-border);
  border-radius: 14px;
  padding: 12px 16px;
  margin-top: 12px;
  font-size: 17px;
  font-weight: var(--font-weight-semibold);
  color: var(--color-ink);
  text-align: center;
  line-height: var(--line-height-normal);
  display: none;
  font-family: var(--font-family);
  animation: fadeInFb 0.3s ease;
}
.q-feedback i {
  margin-left: 6px;
  font-size: 1em;
  line-height: 1;
  vertical-align: middle;
  position: relative;
  top: -1px;
}
.q-feedback.show-correct i,
.q-feedback.show-wrong i {
  top: 0;
  margin-left: 0;
}
.q-feedback.show-correct {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  border-color: var(--color-success);
  background: #f3f7e3;
  color: var(--color-success);
  border-width: 3px;
}
.q-feedback.show-wrong {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  border-color: var(--color-error);
  background: #fbeeea;
  color: var(--color-error);
  border-width: 3px;
}
@keyframes fadeInFb {
  from { opacity: 0; transform: translateY(6px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

### ٣.٥ التجاوب (Responsive @700px + @480px) — بما في ذلك محاذاة السماعة لأول سطر

```css
/* ══════════════════════════════════════════════════════════════
   MCQ RESPONSIVE — 700px
   السماعة تنتقل من center إلى flex-start لمحاذاة أول سطر
══════════════════════════════════════════════════════════════ */
@media (max-width: 700px) {
  .q-card { padding: 16px; }
  .q-card-title { font-size: 19px; }
  .mcq-option-text { font-size: 18px; }
  .q-card-header {
    align-items: flex-start;
  }
  .q-card-header .audio-btn {
    margin-top: 2px; /* يحاذي السماعة بصرياً مع أول سطر العنوان */
  }
}

/* ══════════════════════════════════════════════════════════════
   MCQ RESPONSIVE — 480px
══════════════════════════════════════════════════════════════ */
@media (max-width: 480px) {
  .q-card { padding: 14px; }
  .q-card-title { font-size: 17px; }
  .mcq-option-text { font-size: 16px; }
  .mcq-option-label { padding: 10px 12px; gap: 10px; }
  /* حجم السماعة يزداد للوصول اللمسي (touch target) */
  .audio-btn { width: 40px; height: 40px; min-width: 40px; font-size: 18px; }
  .q-card-header {
    align-items: flex-start;
  }
  .q-card-header .audio-btn {
    margin-top: 1px;
  }
}

/* ══════════════════════════════════════════════════════════════
   MCQ RESPONSIVE — 360px
══════════════════════════════════════════════════════════════ */
@media (max-width: 360px) {
  .q-card-title { font-size: 16px; }
  .mcq-option-text { font-size: 15px; }
}
```

---

## ٤. JavaScript كامل

```javascript
/* ══════════════════════════════════════════════════════════════
   CONSTANTS
══════════════════════════════════════════════════════════════ */
const MAX_ATTEMPTS = 3;

/* ══════════════════════════════════════════════════════════════
   SLIDE DATA — هيكل بيانات MCQ
══════════════════════════════════════════════════════════════ */
const SLIDE_DATA = {
  1: {
    title: "السّؤالُ الأوّلُ",
    type: "mcq",                    // ← ثابت، لا يتغير
    instr: "<strong>السّؤالُ الأوّلُ:</strong> أختارُ الإجابةَ الصّحيحةَ فيما يأتي.",
    questions: [
      {
        q: "............... تغيُّرُ العلامةِ الموجودةِ في آخرِ الكلمةِ.",
        options: ["أ. الإعرابُ", "ب. البناءُ"],
        correct: 0   // ← فهرس الخيار الصحيح (0-based)
      },
      {
        q: "............... لزومُ آخرِ الكلمةِ حركةً واحدةً لا تتغيّرُ.",
        options: ["أ. الإعرابُ", "ب. البناءُ"],
        correct: 1
      }
    ]
  }
};

/* ══════════════════════════════════════════════════════════════
   STATE INIT — MCQ branch داخل initSlideState()
   شكل الحالة لكل سلايد MCQ:
   { type, answers:{qIdx→selIdx}, attempts:{qIdx→count}, status:{qIdx→{type,attempts}} }
══════════════════════════════════════════════════════════════ */
function initSlideState() {
  for (let i = 1; i <= TOTAL_SLIDES; i++) {
    const data = SLIDE_DATA[i];
    if (!data) continue;
    if (data.type === 'mcq') {
      slideState[i] = { type: 'mcq', answers: {}, attempts: {}, status: {} };
    }
    // ... بقية الأنواع (click, dropdown_tables)
  }
}

/* ══════════════════════════════════════════════════════════════
   BUILD SLIDE HTML — MCQ branch داخل buildAllSlides()
   يُنشئ الحاوية فقط؛ renderMCQQuestions() يملؤها لاحقاً
══════════════════════════════════════════════════════════════ */
// داخل buildAllSlides():
// if (data.type === 'mcq') {
//   html += `<div class="questions-container" id="qContainer-${i}"></div>`;
// }

/* ══════════════════════════════════════════════════════════════
   PROGRESSIVE REVEAL — إظهار تدريجي للأسئلة
══════════════════════════════════════════════════════════════ */

// إخفاء كل الأسئلة عدا الأول — يُستدعى بعد renderMCQQuestions()
function hideAllMCQExceptFirst(slideIdx) {
  const cards = document.querySelectorAll(`#qContainer-${slideIdx} .q-card`);
  cards.forEach((card, idx) => {
    if (idx === 0) {
      card.classList.remove('hidden-reveal');
      card.classList.add('revealing');
    } else {
      card.classList.add('hidden-reveal');
    }
  });
}

// إظهار السؤال التالي بعد الإجابة على السؤال الحالي
function revealNextMCQ(slideIdx, currentQIdx) {
  const nextCard = document.querySelector(
    `#qContainer-${slideIdx} .q-card[data-q-idx="${currentQIdx + 1}"]`
  );
  if (nextCard) {
    nextCard.classList.remove('hidden-reveal');
    // إعادة تشغيل الأنيميشن عبر إجبار reflow
    nextCard.classList.remove('revealing');
    void nextCard.offsetWidth;
    nextCard.classList.add('revealing');
    setTimeout(() => {
      nextCard.scrollIntoView({ behavior: 'smooth', block: 'center' });
    }, 200);
  }
}

/* ══════════════════════════════════════════════════════════════
   RENDER MCQ QUESTIONS — HTML + Event Listeners
══════════════════════════════════════════════════════════════ */
function renderMCQQuestions(slideIdx) {
  const data = SLIDE_DATA[slideIdx];
  if (!data || data.type !== 'mcq') return;
  const container = document.getElementById(`qContainer-${slideIdx}`);
  if (!container) return;
  const state = slideState[slideIdx];

  let html = '';
  data.questions.forEach((qData, qIdx) => {
    let optionsHtml = '';
    qData.options.forEach((opt, i) => {
      optionsHtml += `
        <label class="mcq-option-label">
          <input type="radio" name="q-${slideIdx}-${qIdx}" value="${i}">
          <span class="mcq-option-text">${opt}</span>
        </label>
      `;
    });

    html += `
      <div class="q-card" data-q-idx="${qIdx}">
        <div class="q-card-header">
          <button class="audio-btn" onclick="toggleSimpleAudio(this)" aria-label="استمع للسؤال">
            <i class="fas fa-volume-high"></i>
          </button>
          <p class="q-card-title">
            <span class="q-num-inline">${toArabic(qIdx + 1)}.</span> ${qData.q}
          </p>
        </div>
        <div class="mcq-options">
          ${optionsHtml}
        </div>
        <div class="q-feedback" id="fb-q${slideIdx}-${qIdx}"></div>
      </div>
    `;
  });

  container.innerHTML = html;

  // ربط المستمعين — تحقق فوري عند الاختيار
  data.questions.forEach((qData, qIdx) => {
    if (state.attempts[qIdx] === undefined) state.attempts[qIdx] = 0;

    container.querySelectorAll(`input[name="q-${slideIdx}-${qIdx}"]`).forEach(input => {
      input.addEventListener('change', () => {
        handleMCQAnswer(slideIdx, qIdx, input, qData);
      });
    });
  });
}

/* ══════════════════════════════════════════════════════════════
   HANDLE MCQ ANSWER — منطق الإجابة الكامل مع 3 محاولات
══════════════════════════════════════════════════════════════ */
function handleMCQAnswer(slideIdx, qIdx, input, qData) {
  const state = slideState[slideIdx];

  // تجاهل إن كان السؤال مقفلاً أو تجاوز الحد الأقصى للمحاولات
  if (state.answers[qIdx] !== undefined) return;
  if (state.attempts[qIdx] >= MAX_ATTEMPTS) return;

  state.attempts[qIdx]++;
  const selectedIdx = parseInt(input.value);
  const correctIdx  = qData.correct;
  const isCorrect   = selectedIdx === correctIdx;

  const card   = input.closest('.q-card');
  const labels  = card.querySelectorAll('.mcq-option-label');
  const inputs  = card.querySelectorAll('input[type="radio"]');
  const fb      = document.getElementById(`fb-q${slideIdx}-${qIdx}`);

  // امسح حالات سابقة
  labels.forEach(l => l.classList.remove('correct-option', 'wrong-option', 'revealed'));

  if (isCorrect) {
    // ═══════════════════════════════════════════════════════
    // ✅ إجابة صحيحة
    // ═══════════════════════════════════════════════════════
    state.answers[qIdx] = selectedIdx;
    state.status[qIdx]  = { type: 'correct', attempts: state.attempts[qIdx] };

    labels.forEach((label, i) => {
      if (i === correctIdx) label.classList.add('correct-option');
      inputs[i].disabled = true;
      label.classList.add('disabled-option');
    });
    card.classList.add('q-card-correct');

    fb.className = 'q-feedback show-correct';
    fb.innerHTML = '<i class="fas fa-check-circle"></i> أحسنتَ! إجابةٌ صحيحةٌ';

    // الفيدباك يُمسح بعد 2500ms؛ التظليل الأخضر على الخيار والكرت يبقى
    setTimeout(() => {
      fb.className = 'q-feedback';
      fb.innerHTML = '';
    }, 2500);

    revealNextMCQ(slideIdx, qIdx);
    updateNavButtons();
    updateProgressBar();

  } else {
    // ═══════════════════════════════════════════════════════
    // ❌ إجابة خاطئة
    // ═══════════════════════════════════════════════════════
    labels.forEach((label, i) => {
      if (i === selectedIdx) label.classList.add('wrong-option');
    });

    // إعادة تشغيل qCardShake في كل محاولة عبر إجبار reflow
    card.classList.remove('q-card-wrong');
    void card.offsetWidth;
    card.classList.add('q-card-wrong');

    if (state.attempts[qIdx] >= MAX_ATTEMPTS) {
      // ═══════════════════════════════════════════════════
      // 3 محاولات → إظهار رسالة الكشف لـ3000ms ثم الكشف
      // ═══════════════════════════════════════════════════
      fb.className = 'q-feedback show-correct';
      fb.innerHTML = '<i class="fas fa-check-circle"></i> أحسنتَ على المحاولةِ! تعرَّفْ على الإجابةِ الصّحيحةِ';

      // بعد 3000ms: كشف الإجابة الصحيحة + قفل الخيارات
      setTimeout(() => {
        state.answers[qIdx] = correctIdx;
        state.status[qIdx]  = { type: 'revealed', attempts: MAX_ATTEMPTS };

        labels.forEach((label, i) => {
          label.classList.remove('wrong-option');
          if (i === correctIdx) label.classList.add('revealed');
          inputs[i].disabled = true;
          label.classList.add('disabled-option');
        });
        card.classList.remove('q-card-wrong');
        card.classList.add('q-card-correct');

        fb.className = 'q-feedback';
        fb.innerHTML = '';

        revealNextMCQ(slideIdx, qIdx);
        updateNavButtons();
        updateProgressBar();
      }, 3000);

    } else {
      // ═══════════════════════════════════════════════════
      // أقل من 3 محاولات: فيدباك أحمر + إعادة ضبط بعد 1500ms
      // ═══════════════════════════════════════════════════
      fb.className = 'q-feedback show-wrong';
      fb.innerHTML = '<i class="fas fa-times-circle"></i> إجابةٌ غيرُ صحيحةٍ. حاولْ مرّةً أخرى!';

      setTimeout(() => {
        labels.forEach(l => l.classList.remove('wrong-option'));
        inputs.forEach(inp => { inp.checked = false; });
        card.classList.remove('q-card-wrong');
        fb.className = 'q-feedback';
        fb.innerHTML = '';
      }, 1500);
    }
  }
}

/* ══════════════════════════════════════════════════════════════
   IS SLIDE ANSWERED — MCQ branch داخل isSlideAnswered(idx)
   يُعيد true فقط حين أُجيب عن كل الأسئلة (صحيح أو مكشوف)
══════════════════════════════════════════════════════════════ */
// if (data.type === 'mcq') {
//   return data.questions.every((q, qIdx) => state.answers[qIdx] !== undefined);
// }

/* ══════════════════════════════════════════════════════════════
   TOGGLE AUDIO — دالة مشتركة
══════════════════════════════════════════════════════════════ */
function toggleSimpleAudio(btn) {
  btn.classList.toggle('state-playing');
}

/* ══════════════════════════════════════════════════════════════
   INIT — ترتيب الاستدعاءات داخل init loop
══════════════════════════════════════════════════════════════ */
// for (let i = 1; i <= TOTAL_SLIDES; i++) {
//   const data = SLIDE_DATA[i];
//   if (!data) continue;
//   if (data.type === 'mcq') {
//     renderMCQQuestions(i);
//     hideAllMCQExceptFirst(i);  // ← مهم: بعد render مباشرة
//   }
// }
```

---

## ٥. متغيرات التصميم وأحجام الخطوط

> جميع القيم أدناه مستخرجة مباشرةً من الاختبار التشخيصي.html — لا تغيّر أياً منها.

### ٥.١ متغيرات الجذر (CSS Variables)

| المتغير | القيمة | الغرض |
|---|---|---|
| `--audio-btn-size` | `36px` | حجم زر السماعة (عرض وارتفاع) |
| `--audio-btn-radius` | `9px` | نصف قطر حواف زر السماعة |
| `--audio-btn-icon-size` | `16px` | حجم أيقونة السماعة |
| `--color-primary` | `#844816` | البني الأساسي — حدود الكرت + accent الراديو |
| `--color-primary-light` | `#F3EDE8` | حدود الخيار الافتراضية |
| `--color-primary-shadow` | `#5e3210` | ظل زر السماعة + لون رقم السؤال |
| `--color-primary-hover` | `#9d5a22` | hover على زر السماعة |
| `--color-secondary` | `#EDC66F` | الذهبي الفاتح — حدود hover على الخيار |
| `--color-secondary-light` | `#FFF4E5` | خلفية checked-selection |
| `--color-success` | `#6b8b1f` | أخضر الصح + الكشف |
| `--color-error` | `#B7472D` | أحمر الخطأ |
| `--color-ink` | `#3a2410` | لون نص السؤال والخيارات |
| `--color-light-border` | `#C9B5A0` | حدود صندوق الفيدباك الافتراضي |
| `--font-size-instructions` | `24px` | حجم خط عنوان السؤال (`.q-card-title`) |
| `--line-height-relaxed` | `1.8` | ارتفاع سطر عنوان السؤال |
| `--line-height-normal` | `1.7` | ارتفاع سطر نص الخيار |
| `--font-weight-bold` | `700` | الوزن الأساسي |
| `--font-weight-extrabold` | `700` | وزن الحالات (صح/خطأ/كشف) |

### ٥.٢ أحجام الخطوط — تفصيلية (مهم جداً)

| العنصر | اللابتوب (>700px) | ≤700px | ≤480px | ≤360px |
|---|---|---|---|---|
| `.q-card-title` (عنوان السؤال) | `24px` (من `--font-size-instructions`) | **19px** | **17px** | **16px** |
| `.q-num-inline` (رقم السؤال "١.") | `1.2em` ≈ 28.8px | `1.2em` ≈ 22.8px | `1.2em` ≈ 20.4px | `1.2em` ≈ 19.2px |
| `.mcq-option-text` (نص الخيار) | **20px** | **18px** | **16px** | **15px** |
| `.audio-btn` icon | `16px` (من `--audio-btn-icon-size`) | `16px` | **18px** | `18px` |
| `.q-feedback` | `17px` | `17px` | `17px` | `17px` |

> **ملاحظة**: حجم السماعة يزداد عند ≤480px (16px → 18px) بسبب تحسين الوصول اللمسي.

### ٥.٣ أوزان الخط (font-weight)

| العنصر | الوزن |
|---|---|
| `.q-card-title` (عنوان السؤال) | `var(--font-weight-bold)` = 700 |
| `.q-num-inline` (رقم السؤال) | `var(--font-weight-extrabold)` = 700 |
| `.mcq-option-label` (الافتراضي) | `var(--font-weight-bold)` = 700 |
| `.mcq-option-text` (الافتراضي) | (يرث من label) = 700 |
| `.correct-option .mcq-option-text` | `var(--font-weight-extrabold)` = 700 |
| `.wrong-option .mcq-option-text` | `var(--font-weight-extrabold)` = 700 |
| `.revealed .mcq-option-text` | `var(--font-weight-extrabold)` = 700 |
| `.q-feedback` | `var(--font-weight-semibold)` = 600 |

### ٥.٤ ارتفاع السطر (line-height)

| العنصر | القيمة |
|---|---|
| `.q-card-title` (عنوان السؤال) | `var(--line-height-relaxed)` = 1.8 |
| `.mcq-option-text` (نص الخيار) | `var(--line-height-normal)` = 1.7 |
| `.q-feedback` | `var(--line-height-normal)` = 1.7 |

### ٥.٥ مقاسات أخرى (padding، gap، radius)

| العنصر | القيمة |
|---|---|
| `.mcq-options` gap | `10px` |
| `.mcq-option-label` padding | `12px 14px` (≤480px: `10px 12px`) |
| `.mcq-option-label` gap | `12px` (≤480px: `10px`) |
| `.mcq-option-label` border | `2px solid var(--color-primary-light)` |
| `.mcq-option-label` border-radius | `14px` |
| `.mcq-option-label` background (افتراضي) | `#fffaf3` |
| `.mcq-option-label.checked-selection` border-width | `3px` |
| `.mcq-option-label.correct-option` border-width | `3px !important` |
| `.mcq-option-label.wrong-option` border-width | `3px !important` |
| `.mcq-option-label.revealed` border-width | `3px !important` |
| `.q-card` padding | `16px 18px` (≤480px: `14px`) |
| `.q-card` margin | `14px 16px 0` |
| `.q-card` border-radius | `20px` |
| `.q-card-header` gap | `12px` |
| `.q-card-header` margin-bottom | `12px` |
| `.q-card-header .audio-btn` margin-top (≤700px) | `2px` |
| `.q-card-header .audio-btn` margin-top (≤480px) | `1px` |
| `.audio-btn` size | `36px × 36px` (≤480px: `40px × 40px`) |
| `.audio-btn` border-radius | `var(--audio-btn-radius)` = `9px` |
| `.q-feedback` padding | `12px 16px` |
| `.q-feedback` margin-top | `12px` |
| `.q-feedback` border-radius | `14px` |
| `.q-feedback` font-size | `17px` |

### ٥.٦ ألوان الـ states

| الحالة | الحدود | الخلفية | لون النص |
|---|---|---|---|
| افتراضي (base) | `2px solid var(--color-primary-light)` | `#fffaf3` | `var(--color-ink)` = #3a2410 |
| `:hover` | `var(--color-secondary)` | `var(--color-primary-light)` | (يتغير) |
| `checked-selection` | `3px solid var(--color-primary)` + box-shadow | `var(--color-secondary-light)` | (يتغير) |
| `correct-option` | `3px solid var(--color-success)` | `#f3f7e3` | `var(--color-success)` = #6b8b1f |
| `wrong-option` | `3px solid var(--color-error)` | `#fbeeea` | `var(--color-error)` = #B7472D |
| `revealed` | `3px solid var(--color-success)` | `#f3f7e3` | `var(--color-success)` = #6b8b1f |
| `q-card-correct` | `3px solid var(--color-success)` | `#f3f7e3` | — |
| `q-card-wrong` | `3px solid var(--color-error)` | `#fbeeea` | — |
| `q-feedback show-correct` | `3px solid var(--color-success)` | `#f3f7e3` | `var(--color-success)` |
| `q-feedback show-wrong` | `3px solid var(--color-error)` | `#fbeeea` | `var(--color-error)` |

### ٥.٧ الأنيميشن

| الاسم | يُطبَّق على | المدة | المنحنى |
|---|---|---|---|
| `optionPulseCorrect` | `.correct-option`, `.revealed` | **0.6s** | ease |
| `audioPulse` | `.audio-btn.state-playing` | **1.5s** | ease-in-out infinite |
| `qCardShake` | `.q-card.q-card-wrong` | **0.5s** | (keyframes) |
| `qCardReveal` | `.q-card.revealing` | **0.55s** | cubic-bezier(0.34, 1.56, 0.64, 1) |
| `fadeInFb` | `.q-feedback` (كل مرة يظهر) | **0.3s** | ease |

---

## ٦. البنية البصرية

```
.slide
└── .questions-container [id="qContainer-{N}"]
    ├── .q-card [data-q-idx="0"]          ← يظهر دائماً
    │   ├── .q-card-header
    │   │   ├── button.audio-btn          ← 36×36px (أو 40px على موبايل)
    │   │   └── p.q-card-title
    │   │       ├── span.q-num-inline     ← "١." بالأرقام العربية
    │   │       └── نص السؤال (flex: 1)
    │   ├── .mcq-options (display:grid, gap:10px)
    │   │   ├── label.mcq-option-label    ← الحالات: base/checked-selection/correct-option/wrong-option/revealed/disabled-option
    │   │   │   ├── input[type="radio"]
    │   │   │   └── span.mcq-option-text
    │   │   └── label.mcq-option-label × N
    │   └── .q-feedback [id="fb-q{N}-0"]  ← display:none → show-correct/show-wrong
    │
    ├── .q-card.hidden-reveal [data-q-idx="1"]  ← display:none حتى الإجابة على [0]
    └── .q-card.hidden-reveal [data-q-idx="2"]  ← display:none حتى الإجابة على [1]
```

### الطبقات البصرية:
1. **الكرت** (`.q-card`): الحاوية الرئيسية — تتلون خضراء (صح/كشف) أو حمراء (خطأ)
2. **الرأس** (`.q-card-header`): flex أفقي — سماعة + رقم + نص
3. **الخيارات** (`.mcq-options`): شبكة عمودية — كل خيار label مع radio
4. **الفيدباك** (`.q-feedback`): أسفل الخيارات — يظهر/يختفي مؤقتاً

### تفاصيل المحاذاة (اللابتوب vs الموبايل):
- **اللابتوب (>700px)**: `.q-card-header { align-items: center; }` — السماعة في منتصف الرأس
- **≤700px**: `align-items: flex-start` + `.audio-btn { margin-top: 2px; }` — محاذاة أول سطر
- **≤480px**: `align-items: flex-start` + `.audio-btn { margin-top: 1px; }` — نفس المبدأ أدق

---

## ٧. السلوك التفصيلي

### ٧.١ عند اختيار الإجابة الصحيحة
1. الخيار المختار يأخذ `.correct-option`: حدود خضراء 3px + خلفية `#f3f7e3` + `optionPulseCorrect 0.6s`
2. نص الخيار يصير أخضر + `font-weight: extrabold`
3. جميع الخيارات: `input.disabled = true` + `.disabled-option` (pointer-events: none)
4. الكرت يأخذ `.q-card-correct`: حدود خضراء 3px + خلفية `#f3f7e3`
5. الفيدباك: `show-correct` + `أحسنتَ! إجابةٌ صحيحةٌ` — يُمسح بعد **2500ms** (التظليل يبقى)
6. `revealNextMCQ(slideIdx, qIdx)` — السؤال التالي يظهر بأنيميشن `qCardReveal`
7. `updateNavButtons()` + `updateProgressBar()`

### ٧.٢ عند اختيار خطأ (قبل 3 محاولات)
1. الخيار المختار يأخذ `.wrong-option`: حدود حمراء 3px + خلفية `#fbeeea`
2. نص الخيار يصير أحمر + `font-weight: extrabold`
3. الكرت يأخذ `.q-card-wrong` + `qCardShake 0.5s`
   - **مهم**: يُزال الكلاس أولاً → `void card.offsetWidth` (reflow) → يُعاد — لإعادة الأنيميشن في كل محاولة
4. الفيدباك: `show-wrong` + `إجابةٌ غيرُ صحيحةٍ. حاولْ مرّةً أخرى!`
5. بعد **1500ms**:
   - `.wrong-option` تُزال من الخيار
   - `inp.checked = false` — إلغاء تحديد الراديو
   - `.q-card-wrong` تُزال من الكرت
   - الفيدباك يُمسح

### ٧.٣ بعد 3 محاولات خاطئة (الكشف)
1. الفيدباك الفوري: `show-correct` + `أحسنتَ على المحاولةِ! تعرَّفْ على الإجابةِ الصّحيحةِ`
2. **انتظار 3000ms** (أثناء ظهور الرسالة)
3. بعد 3000ms — كشف الإجابة:
   - `.wrong-option` تُزال من الخيار الخاطئ
   - `.revealed` تُضاف على الخيار الصحيح: أخضر + `optionPulseCorrect 0.6s`
   - جميع الخيارات: `input.disabled = true` + `.disabled-option`
   - الكرت ينتقل: `.q-card-wrong` تُزال → `.q-card-correct` تُضاف
   - الفيدباك يُمسح
4. `revealNextMCQ(slideIdx, qIdx)` — السؤال التالي يظهر
5. `updateNavButtons()` + `updateProgressBar()`

### ٧.٤ السماعة (audio-btn)
- النقر الأول: تُضاف `.state-playing` (أخضر + `audioPulse 1.5s infinite`)
- النقر الثاني: تُزال `.state-playing` (رجوع للبني الطبيعي)
- عند الانتقال لسلايد آخر: `instrAudioBtn.classList.remove('state-playing')` — تُعاد حالة السماعة

### ٧.٥ الظهور التدريجي للأسئلة (Progressive Reveal)
- عند init: `hideAllMCQExceptFirst(i)` تخفي الأسئلة من الثاني فصاعداً
- بعد الإجابة على السؤال N: `revealNextMCQ(slideIdx, N)` تظهر السؤال N+1
- الظهور بأنيميشن `qCardReveal 0.55s cubic-bezier(0.34, 1.56, 0.64, 1)` (bounce)
- بعد 200ms: `scrollIntoView` للسؤال الجديد

---

## ٨. الفيدباك + النصوص

### النصوص الموحّدة (لا تغيير في أي ملف)

| الحالة | النص الكامل |
|---|---|
| ✅ صحيح | `أحسنتَ! إجابةٌ صحيحةٌ` |
| ❌ خطأ | `إجابةٌ غيرُ صحيحةٍ. حاولْ مرّةً أخرى!` |
| 🔓 كشف (toast قبل 3000ms) | `أحسنتَ على المحاولةِ! تعرَّفْ على الإجابةِ الصّحيحةِ` |

### ملاحظات التشكيل الحرجة:
- `أحسنتَ` — فتحة على التاء المفتوحة (تنوين)
- `إجابةٌ صحيحةٌ` — تنوين ضم على التاء المربوطة — **بدون نقطة في النهاية**
- `تعرَّفْ` — فتحة + شدة على الراء، سكون على الفاء — `تعرَّفْ` وليس `تعرّفْ`
- `الإجابةِ الصّحيحةِ` — كسرة على التاء المربوطة، شدة على الصاد

### الأيقونات المسموحة:

| الحالة | Font Awesome class |
|---|---|
| صحيح | `fa-check-circle` |
| كشف | `fa-check-circle` |
| خطأ | `fa-times-circle` |

### بنية HTML الفيدباك (كاملة جاهزة للنسخ):

```html
<!-- ✅ صحيح -->
<div class="q-feedback show-correct">
  <i class="fas fa-check-circle"></i> أحسنتَ! إجابةٌ صحيحةٌ
</div>

<!-- ❌ خطأ -->
<div class="q-feedback show-wrong">
  <i class="fas fa-times-circle"></i> إجابةٌ غيرُ صحيحةٍ. حاولْ مرّةً أخرى!
</div>

<!-- 🔓 كشف -->
<div class="q-feedback show-correct">
  <i class="fas fa-check-circle"></i> أحسنتَ على المحاولةِ! تعرَّفْ على الإجابةِ الصّحيحةِ
</div>
```

---

## ٩. التوقيتات

| الحالة | المدة |
|---|---|
| Correct toast (`أحسنتَ! إجابةٌ صحيحةٌ`) | **2500ms** ثم auto-clear |
| Wrong feedback + row reset (`إجابةٌ غيرُ صحيحةٍ`) | **1500ms** |
| Revealed toast (`أحسنتَ على المحاولةِ! تعرَّفْ...`) | **3000ms** ثم كشف الإجابة |
| Reveal delay (تأخير ظهور `.revealed` + قفل الخيارات) | **3000ms** (نفس مهلة الـ toast) |
| Animation `optionPulseCorrect` (صح/كشف) | **0.6s** ease |
| Animation `qCardReveal` (ظهور سؤال جديد) | **0.55s** cubic-bezier(0.34, 1.56, 0.64, 1) |
| Animation `audioPulse` (state-playing) | **1.5s** ease-in-out infinite |
| Animation `qCardShake` (كرت خطأ) | **0.5s** |
| ScrollIntoView delay (بعد reveal السؤال التالي) | **200ms** |

---

## ١٠. الأيقونات

| الغرض | Font Awesome class | ملاحظة |
|---|---|---|
| فيدباك صحيح | `fa-check-circle` | |
| فيدباك كشف | `fa-check-circle` | نفس أيقونة الصح |
| فيدباك خطأ | `fa-times-circle` | |
| زر السماعة | `fa-volume-high` | داخل `.audio-btn` |
| `fa-info-circle` | **ممنوع** | لا تستخدمه في MCQ نهائياً |

---

## ١١. ممنوعات

- **نقطة في نهاية رسالة الصح**: `إجابةٌ صحيحةٌ.` ← ممنوع؛ الصحيح: `إجابةٌ صحيحةٌ` (بدون نقطة)
- **تشكيل خاطئ في رسالة الكشف**: `تعرّفْ` (شدة فقط بدون فتحة) ← ممنوع؛ الصحيح: `تعرَّفْ`
- **رسالة الكشف المختصرة**: `أحسنتَ على المحاولةِ!` (بدون `تعرَّفْ على الإجابةِ الصّحيحةِ`) ← ممنوع
- **كشف فوري بدون 3000ms**: جعل `.revealed` يظهر مباشرة بدون setTimeout — ممنوع
- **عرض عداد المحاولات للطالب**: ("تَبقّتْ X محاولاتٍ") — ممنوع منعاً باتاً
- **`fa-info-circle`** — ممنوع في كل فيدباك MCQ
- **تغيير المدد**: `1500ms / 2500ms / 3000ms` ثابتة لا تتغير
- **إضافة زر "تحقق" منفصل** — هذا النمط يعتمد تحققاً فورياً عند الاختيار
- **تغيير `MAX_ATTEMPTS`** عن 3 — ثابت في جميع الأنشطة
- **تغيير ألوان الحالات**: الأخضر/الأحمر الموحّد ثابت، لا إضافة ألوان جديدة
- **تغيير حجم السماعة** عن `36px/9px/16px` (عدا الـ 480px override الموثّق في القسم ٣.٥)

---

## ١٢. القابل للتعديل

**SAFE — يمكن تغييره:**
- نص كل سؤال (`qData.q`)
- نصوص الخيارات (`qData.options[]`)
- الإجابة الصحيحة (`qData.correct` — فهرس 0-based)
- عدد الأسئلة (طول `questions[]`)
- عدد الخيارات لكل سؤال (طول `options[]`)
- `title` و`instr` (عنوان السؤال وتعليماته)
- `qData.customFeedback` (نص مخصص لرسالة الصح — نادراً، وبدون نقطة في النهاية)

**FIXED — لا تلمسه:**
- الألوان (جميع قيم الـ CSS variables)
- الأنيميشن (`optionPulseCorrect`, `qCardShake`, `qCardReveal`, `audioPulse`)
- `MAX_ATTEMPTS` — دائماً 3
- نصوص الفيدباك (إلا عبر `customFeedback` المقيّد)
- التوقيتات (`1500ms / 2500ms / 3000ms`)
- بنية CSS الكلاسات:
  - خيار: `correct-option` / `wrong-option` / `revealed` / `disabled-option`
  - كرت: `q-card-correct` / `q-card-wrong`
- منطق `isSlideAnswered` (شرط إكمال كل الأسئلة قبل "التالي")

---

## ١٣. القائمة المرجعية لإنشاء MCQ جديد

- [ ] أضف entry لـ `SLIDE_DATA` بـ `type: "mcq"`, `title`, `instr`, `questions[]`
- [ ] كل سؤال: `q` (نص السؤال)، `options[]` (الخيارات)، `correct` (فهرس 0-based)
- [ ] انسخ CSS من القسم ٣ كاملاً إن لم يكن موجوداً في الملف
- [ ] انسخ JS من القسم ٤ (الدوال: `renderMCQQuestions`, `handleMCQAnswer`, `hideAllMCQExceptFirst`, `revealNextMCQ`)
- [ ] داخل `initSlideState()`: تأكد من وجود فرع `mcq`
- [ ] داخل init loop: `renderMCQQuestions(i)` ثم `hideAllMCQExceptFirst(i)` (بهذا الترتيب)
- [ ] في HTML بنية buildAllSlides: أضف `<div class="questions-container" id="qContainer-{N}"></div>`
- [ ] ربط السماعة: `onclick="toggleSimpleAudio(this)"` على كل `.audio-btn`
- [ ] **اختبر على لابتوب وموبايل (≤700px)**: تأكد أن السماعة تحاذي أول سطر العنوان الطويل
- [ ] **اختبر السيناريو الأول** ✅: اختيار صحيح → toast أخضر 2500ms → يُمسح → التظليل الأخضر يبقى → السؤال التالي يظهر
- [ ] **اختبر السيناريو الثاني** ❌: اختيار خاطئ → toast أحمر 1500ms → إعادة ضبط (الراديو + الكرت + الفيدباك)
- [ ] **اختبر السيناريو الثالث** 🔓: 3 محاولات خاطئة → toast أخضر 3000ms → **بعد 3 ثوانٍ** يظهر الخيار الصحيح → السؤال التالي
- [ ] تحقق من النصوص: لا نقطة في `صحيحةٌ`، تشكيل `تعرَّفْ` (فتحة+شدة) صحيح
- [ ] تحقق أن العداد لا يظهر للطالب (فقط عداد داخلي)
- [ ] تحقق أن زر "التالي" مقفل حتى الإجابة على **كل** الأسئلة في السلايد

---

## ١٤. السماعة + الموبايل (تفاصيل دقيقة)

### المشكلة
عندما يكون عنوان السؤال طويلاً وينتقل لسطرين على الموبايل، `align-items: center` يضع السماعة في منتصف الكتلة بدلاً من محاذاة أول سطر، فتبدو عائمة بين السطرين.

### الحل (CSS الكامل)

```css
/* اللابتوب (>700px) — القاعدة الافتراضية */
.q-card-header {
  display: flex;
  align-items: center;   /* السماعة في منتصف الرأس (العنوان سطر واحد عادةً) */
  gap: 12px;
  margin-bottom: 12px;
}

/* ≤700px — السماعة تتحاذى مع أول سطر العنوان الطويل */
@media (max-width: 700px) {
  .q-card-header {
    align-items: flex-start;
  }
  .q-card-header .audio-btn {
    margin-top: 2px; /* هامش لتوازن بصري مع خط الأساس للنص */
  }
}

/* ≤480px — نفس المبدأ، margin أدق (الخط أصغر قليلاً) */
@media (max-width: 480px) {
  .q-card-header {
    align-items: flex-start;
  }
  .q-card-header .audio-btn {
    margin-top: 1px;
  }
}
```

### التحقق من الصحة
1. ضع عنواناً طويلاً (>40 حرفاً) في سؤال MCQ
2. ضيّق النافذة إلى ≤700px
3. يجب أن تبقى السماعة متحاذية مع أول كلمة من السطر الأول (وليس في المنتصف)
4. إذا تحوّل العنوان لسطرين، السماعة تبقى أعلاه

### القيم الثابتة لزر السماعة

| السمة | القيمة >480px | القيمة ≤480px |
|---|---|---|
| `width` / `height` | `36px` (من `--audio-btn-size`) | `40px` (override مباشر) |
| `min-width` | `36px` | `40px` |
| `font-size` (أيقونة) | `16px` (من `--audio-btn-icon-size`) | `18px` |
| `border-radius` | `9px` (من `--audio-btn-radius`) | `9px` (لا يتغير) |
| `box-shadow` | `0 3px 0 var(--color-primary-shadow)` | لا يتغير |
| `flex-shrink` | `0` | `0` (لا يتغير) |

> **تنبيه**: المتغيرات `--audio-btn-size`, `--audio-btn-radius`, `--audio-btn-icon-size` ثابتة في جذر CSS. لا تغيّرها. القيم 40px و 18px على ≤480px هي override مباشر مقصود للوصول اللمسي فقط.

---

**نهاية المواصفة**
