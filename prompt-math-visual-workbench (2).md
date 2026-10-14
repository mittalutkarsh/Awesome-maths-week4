# Reusable prompt: research-first, visual, step-by-step math workbench

**Persona.** Three modes, run in order:
- **Strategist** to research how the topic is best taught, before anything is built.
- **Creator** to draft the pages, concept stories and worked answers.
- **Assistant** to verify, audit and test everything at the end.

**Tier.** Comprehensive, all six blocks. A child uses this unsupervised and a parent teaches from it. A
wrong number, a leaked answer or a confusing picture does real damage.

**Attach before sending:**
1. The problem set (PDF).
2. The lecture notes or slides.
3. The class transcript, if you have one. It holds the teacher's own phrasing and worked reasoning.
4. Any extra challenge material whose questions or explanations you want reused.

Fill every `[bracketed placeholder]`. The ones that matter most are grade/age, the concept list and the topic slug.

---

## The prompt

**Role.** You are a math curriculum engineer and visual explainer who builds self-marking practice
software for competition-track elementary students. You care equally about three things:
mathematical correctness, not doing the child's thinking for him, and explanations a parent can teach
from even when the topic is hard for the parent too.

**Task.** Work in two phases and stop between them.

*Phase 1: research and plan. No HTML yet.*

1. Do agentic web research on how this topic is best taught to a `[grade]` student. Look at:
   - textbooks (AoPS, Beast Academy);
   - math-education research (children's strategies, common misconceptions);
   - teacher resources (NCTM, NRICH).

   Summarize the teaching principles you will use, with citations. Then say how each one shows up in
   the pages.
2. **Find the one general method** that solves every problem in the set the same way. A child can't
   explain a different trick for each problem, and neither can a parent teaching him. He can explain one
   idea applied many times.
   - Look for it in the lecture first: the teacher's own observation that generalizes. For counting it
     was "7!/(3!4!) looks like 7 choose 3": every problem is a word with repeated letters.
   - Map **every** problem and lecture example to the method in a table: what the method's pieces are
     for that problem, and the answer they give. Check every row by brute force.
   - If a problem doesn't fit, adjust the method until it does, or flag that problem as an exception and
     say why. Don't quietly switch tricks between problems.
   - The method's 3–5 steps become the **plan**, shown as a flowchart. For counting it was the tag-word
     method:

     > Tag every object with a letter that says where it goes → Count the words the tags spell:
     > n! ÷ (each letter's repeats)! (two separate words multiply; separate cases add) → Look-alike groups
     > (same size, no names)? Write the group word, like A A A B B, and divide by its repeats! → Check a
     > tiny case you can list.

   Ground the method in your research, and say why it beats problem-by-problem tricks for this topic.
3. List the concepts. **Each concept is one way of applying the method.** For counting: choosing as
   T/L tags, tags for several groups, look-alike groups, paths as words, tagging what decides a shape,
   two words or two cases. Give each a one-sentence "idea" and a **signature visual** drawn from the
   method's own pictures.
4. Show the step decomposition and the plan-guided worked answer, frame by frame, for three
   representative problems: one easy, one where the obvious method fails, one challenge.
5. Show the Python verification output for every answer.
6. List anything in the source that is wrong, ambiguous or missing.

Then stop and wait for my go-ahead.

*Phase 2: build.* Build four self-contained HTML pages that work together as a site served from a
GitHub Pages repo root:

1. `index.html`: links to the other pages, the plan story, and how grading works.
2. `[slug]-notes.html`: reading page, no grading.
3. `[slug]-lesson.html`: workbench with one guided problem per concept, Concept tab open by default.
4. `[slug]-practice.html`: workbench with every problem from the set, plus a challenge section from
   the extra material.

**Context.**
- **Student:** `[grade]` grade, `[age]` years old, in `[program, e.g. AwesomeMath Academy Level 1]`.
  His arithmetic is reliable. He is learning to write out his method, not just answers.
- **Parent:** `[the parent]` sits with him, explains on screen with the pen, and finds some topics hard
  too. The "Ask him" prompts are written for the parent.
- **Sources:**
  - `[problem set file]`
  - `[lecture notes file]`
  - `[transcript, if attached]`: mine it for the teacher's explanations and teaching points.
  - `[extra challenge file, if attached]`: take the problems that fit this topic and reuse the good
    explanations, especially `[e.g. the pick-grid explanation]`. Say which ones you left out and why.
- **Topic scope:** `[list the concepts]`.
- **Every problem** in the set appears. Do not drop, merge or simplify any of them. If a problem sits
  above grade, keep it, split it into steps, and flag it at the end.

**Requirements.**

### 1. The visual story component (used everywhere)

A step-through explainer:
- Each frame has **one picture**, **1–3 short sentences**, and an optional **"Ask him:"** box. The box
  holds a "why" question for the parent to ask; it never states the answer.
- Controls: Back / Next step, progress dots, a "Show all steps" toggle, and ←/→ keys when focused.
- When printing, all frames show.
- One frame per idea. Never put two new ideas in one picture.

### 2. Concept stories (lesson Concept tab and notes page)

For each concept, in this order:
1. **The governing equation**, in a display block.
2. **The one-sentence idea.**
3. **A story of 4–9 frames** that follows the research:
   - **Concrete first.** Show the actual objects: sandwiches, beads, friends in chairs.
   - **Wrong answer first.** Show the tempting wrong answer crossed out, and say what it really counts
     (3 + 2 = 5 counts the foods, not the lunches).
   - **Organized listing**, holding one thing fixed (the odometer method). It should make the equal
     groups visible, and that is *why* we multiply.
   - **The picture**: a tree, grid or slots. Show both an equal tree and an unequal one, so he sees
     when multiplying fails.
   - **The numbers.**
   - **Generalize.** End with a pattern table that has one row left as "?" for him to fill in, then the
     general rule.

   For overcounting, paint the identical copies (L₁, L₂), list everything, then wash off the paint and
   show the copies collapsing into bundles. With two repeat groups, show why the copy counts multiply,
   not add.
4. **Examples in a concept story must use different numbers from that concept's guided problem.** The
   Concept tab is visible by default, so it must not solve the problem next to it.
5. **Every concept story uses the method's own pictures** (for counting: the tagged row, the tag word with
   its letter counts, the group word), so he sees one method in different settings, not nine techniques.

### 3. The plan story

The plan flowchart on the left, with the current step highlighted. On the right, **one running
example** carried through every step of the method, with its own picture. For counting: the tagged
row, the word count, the tempting count crossed out, the look-alike check, the tiny case listed.

### 4. Plan-guided worked answers (every practice and lesson problem)

- One frame per plan step (a step may take two or three frames), applied to **that** problem, with
  **the same kind of picture every time**, so every worked answer visibly follows the method. For
  counting:
  1. **Tag every object:** the objects in a row, each with its tag letter underneath.
  2. **Count the words:** the tag word with its letter counts, the formula and the result. Show the
     tempting wrong count crossed out, saying what it really counts. For two separate words, show both
     and the ×; for cases, the +.
  3. **Look-alike groups:** a clear **Yes / No**. Yes shows the group word (a letter over each group,
     with its size underneath) and the division. No says why nothing can trade places.
  4. **A tiny case he can list by hand**, with the list shown.
- Every step is answered, even when the answer is "no" ("Look-alike groups? No: the kids have names").
  Never silently drop a step.
- Layout depends on the page:
  - **Wide pages:** the full flowchart sits beside the picture.
  - **Narrow panes** (the workbench middle pane): the plan becomes a strip of step numbers across the
    top, with only the current step spelled out, so the picture gets the full width.
- Show only the plan's step numbers. Don't add a separate "Step k of n" frame counter that can
  disagree with them.
- Worked answers appear **only** after every step is closed (in the verified panel) or after "Show
  worked answer" is confirmed.

### 5. Visual primitives

Build these programmatically from data in Python. Don't hand-draw each problem's pictures.
- **The method's own primitives come first.** For counting: the tagged row (object chips with tag tiles
  underneath), the tag word with its letter counts, and the group word with the size under each
  letter. Generate every worked answer from a short per-problem spec through one story generator, so
  all of them share the same pictures. The primitives below support these:
- **Slots**, each with a label, option chips and a count, joined by × or ÷ and ending in a result box.
- **Option chips**, with three states: banned (red strike), used up (grey strike), picked (highlight).
- **Trees**: equal ones, and unequal ones with the branch counts labelled.
- **Letter tiles**, with painted copies shown by colour and subscript.
- **Bundles** that collapse painted copies into one real word.
- **The pick grid**, in progressive stages:
  - empty;
  - filled;
  - diagonal crossed out;
  - twins marked, with the lower triangle greyed and a highlighted twin pair.

  It also needs a "plain" mode for when there are no twins, and pointing at a box should light its twin.
- **Dots and lines** for handshakes and diagonals.
- **Tables** with a highlighted row.
- **Others as needed**: chairs, a checkerboard, a licence plate with a letter block, books with a block.

### 6. The notes page

Long-form, with a sticky table of contents. **Every section opens with its governing equation.**
Sections, in order:
- **The plan**, as a story.
- **Every concept**: the story, then "From class" (the lecture's own worked examples), then extra worked
  numbers, then chips deep-linking to the problems that use it (e.g. `practice.html#q8`).
- **The method section** `[e.g. "The tag word"]`:
  - its build-up on a fresh example, one frame per question the method asks;
  - then the method-mapping table from Phase 1 (every problem's pieces, no answers), showing that one
    method covers the whole set.
- **Tricky bits**: 10–15 named traps, each from a specific problem, each giving the wrong answer and why
  it's tempting.
- **Worked problems**: 3–5 plan stories.
- **A cheat sheet**: situation → move.
- **A glossary**, precise on the awkward words (empty product, ordered pair, set, "not necessarily
  distinct").
- **Write-up habits**, from the transcript if attached.

### 7. The two workbench pages

**Layout.** Three panes, one screen, no page scrolling on desktop.
- **Top bar:** a problem dropdown grouped by the set's sections, prev/next arrows, a finished counter,
  a per-problem timer, and links to the sibling pages.
- **Left rail:** problems by section, each with a status dot (untouched / in progress / closed).
- **Middle pane, tabbed:** Problem · Concept (lesson page only, the default there) · Rules (practice
  only: a one-screen reminder card that includes the plan line) · My steps.
- **Right pane:** step pills, the step question, a line-numbered work editor, an answer field, and a
  dark feedback console that logs every attempt.

**Steps.** Each problem has 1–6 steps, one quantity per step. Each step carries:
- the question and its type (`num`, `expr` for factorial/power expressions, `nums`, `choice`, `factor`
  or `text`);
- three escalating nudges and one hint;
- the revealed answer with its check;
- a one-line success note.

**Grading loop.**
- He writes his method, enters the answer, and presses **Check step**.
- Each wrong answer gets a nudge, escalating each time. After the third miss he gets one final attempt.
  If he misses that too, the step's answer is handed over and he moves to the next step, which he still
  does himself.
- **Hint for this step** opens a dialog, only when pressed. It doesn't use a try, but it's recorded.
- **Submit question** prints how each step went and unlocks the verified answer and the worked story,
  once every step is closed.
- **Show worked answer** needs an in-page two-click confirmation when any step is untried.

**Answer checking.** Accept any reasonable form: `2^3*5*11`, `2³·5·11`, `11!/(4!4!2!)`, `3×2⁹`,
commas in numbers, lists like "2, 5" or "2 and 5", letters or option words for multiple choice, and
y/n. Name specific errors wherever possible:
- too big, too small, very close;
- "that's step k's answer";
- "the product is right but some pieces are still composite";
- "right number of values but one is off";
- per-step named wrong answers, such as "81 lets yellow appear any number of times".

**Hints and nudges speak the method.** They use its vocabulary and point back to the plan step he
missed ("Plan step 3: are any groups look-alikes?"). Avoid "Plan step k" wording in a step whose answer
is k; the leakage audit will rightly flag it.

**Tone.** Plain, warm, matter-of-fact. Never gush, never scold. Nudges point at the next question to
ask, not at the child.

### 8. The teaching pen (all pages)

**Tools and controls.**
- Four ink colours, three nib widths, a translucent marker, a straight line, a box and an eraser.
- Shift snaps a pen or marker stroke to a straight line.
- Strokes are smoothed through the midpoints of samples, and their width tapers with speed.
- Undo, redo, clear (which is itself undoable) and PNG export. Say plainly that export captures the
  ink only, not the page.
- A stylus-only toggle that ignores touch input, for palm rejection.
- `P` toggles the pen and `Escape` turns it off. Both are ignored while typing in an input.

**Anchoring.**
- Reading pages: ink is stored in page coordinates and stays on the content, and a finger scrolls.
- Workbench pages: ink is viewport-anchored, the pen layer swallows scrolling, and ink is kept per
  problem through `window.INK.get/set`.

**Performance.**
- Use two viewport-sized canvases: one for committed strokes and one for the live stroke.
- Draw only the new points each frame, and never redraw old strokes while drawing.
- Never size a canvas to the document.
- Cap the device pixel ratio at 2, and batch to one repaint per animation frame.
- Report how many old strokes are redrawn per frame with 40 strokes / 2,000 points on the page (it
  should be 0), and the total canvas megapixels.

### 9. Technical

- One self-contained HTML file per page. No frameworks, and no CDN except fonts.
- The pages must survive a sandboxed `about:srcdoc` frame:
  - no `localStorage`/`sessionStorage`;
  - no `alert`/`confirm`/`prompt`;
  - `history.replaceState` and `location.hash` wrapped in try/catch and skipped when the protocol is `about:`.
- Hash deep links (`#q8`) work wherever they're allowed. Relative links only.
- Keyboard: Enter checks from the answer box, Ctrl+Enter from the editor, Escape closes the dialog.
  Visible focus rings and ARIA labels.
- On a phone, the panes stack and the rail becomes a toggle. Pen tools are hidden when printing.
- Generate everything from `build.py`:
  - problem data, concept stories, plan-guided worked answers and visual primitives in separate
    Python modules;
  - checker, workbench, pen, grid and story JS shared across pages.
- **Design:** a math-notebook look. Commit to one palette and two typefaces. Avoid cream-and-terracotta,
  all-caps eyebrow labels, and identical rounded cards with soft grey shadows.

**Boundaries.**

### Nothing hint-like is visible before the hint button is pressed

- Step questions ask for a quantity and never hand over the method. If a step can't be asked without
  the method, put the method in the hint. If a step breakdown only exists to walk him through the
  method, collapse it into one step.
- The work-area placeholder is one generic line. Answer placeholders show only the format and never
  equal that step's answer.
- Nudges never state the answer, in any form. Hints give method and setup but stop short of the value.
- The default-visible Concept tab must not solve the guided problem next to it.
- Worked stories stay hidden until the problem is finished or the worked answer is requested.

### Audit leakage in code, then report

- **What to search:** every step question, nudge, hint and placeholder, plus any text visible on the same
  screen by default (the Concept tab, the Rules card).
- **What to look for:** that step's own answer, as a numeral, in comma form, as a number word, or as any
  expression that evaluates to it (`10 − 1`, `3 × 2`, `15!/3!`).
- **What to report:** the hit count. Fix the real leaks, and list each false positive with its reason
  rather than silently rewriting it.

### Verification

- Compute every answer in Python, by brute force **and** by argument, and confirm the two agree. Don't
  trust the source's answer key or remembered competition answers.
- **Every number that appears in a picture or explanation** gets the same treatment:
  - concept-story examples;
  - the plan example;
  - every row of the method-mapping table;
  - every "tiny case" list;
  - grid box counts, which must match the answers;
  - example words, which must use exactly the problem's letters.
- Every revealed answer includes its check.

**Reasoning.**

- **Phase 1** ends with the research summary, the general method with its problem-by-problem mapping
  table, the plan, the concept and visual map, the three sample decompositions and stories, and the
  Python output. Then stop.
- **After building, run scripted passes in a headless browser**, and report the counts for each:
  1. Every step answered correctly. Confirm the verified answer and the worked story appear, and that
     the stepper advances.
  2. One step missed four times: nudge → final attempt → handover.
  3. A hint opened mid-attempt.
  4. Reveal and navigation, including deep links, Enter and Ctrl+Enter.
  5. The pen: each tool, palm rejection, undo/redo/clear, export, per-problem ink, and both anchoring
     modes.
- **Repeat the passes** with `confirm` and `history.replaceState` throwing, and again inside a real
  sandboxed srcdoc iframe.
- **Visual review.** Render every frame of every story into contact-sheet images and look at them
  yourself. Fix anything that is:
  - wrapped onto the wrong line or cut off in a narrow pane;
  - an overlapping tree node;
  - an ambiguous label (two leaves both "P"; team boxes labelled "12" that read as twelve; two objects
    sharing an initial, like Dishes and Dog both shown as "D");
  - a label repeated in one picture ("pair" written twice), or letter counts listed in a confusing order;
  - a picture that doesn't use the method's own visuals when it could.
- **Report:**
  - the pen performance numbers;
  - every element id the JS touches, confirmed present on each page;
  - no duplicate ids;
  - every internal link and anchor resolving.
- **Close with:**
  - the problems above grade level;
  - anything wrong or ambiguous in the source;
  - any answer you couldn't verify;
  - which extra-material questions you included and which you left out, and why.

---

## Notes on using this

- **Send Phase 1, check it, then say "go".** Most of the value is in the research, the plan and the
  three sample stories. Fixing the approach there is cheap; fixing 40 worked stories later is not.
- **The failure modes this prompt guards against**, in order of how much time they cost last time:
  1. **A different trick for every problem instead of one general method.** The parent couldn't explain
     why Problem 9 divides by 12 or how Problem 11 gets 240, because each worked answer reasoned its
     own way. The fix was to find one method (the tag word), check it on every problem, and rebuild
     every story, hint and nudge around it. Do that in Phase 1, not after the build.
  2. Explanations written as paragraphs instead of picture-by-picture steps.
  3. A default-visible concept tab solving its own guided problem.
  4. Hints leaking through question wording and placeholders.
  5. Illustrative numbers that were never verified.
  6. Narrow panes cutting off grids.
  7. Unguarded browser APIs blanking the page in a sandbox.
  8. A pen that redraws everything every frame.
- **Phase 2 starts only on an explicit go-ahead.** If the reply to Phase 1 is more material rather than
  "go", fold it into the plan and stop again, but say plainly that the HTML hasn't been built yet.
- **For a topic other than counting**, keep the structure and let Phase 1 find the general method and
  its signature visuals. Fractions might use one bar model for every problem ("What is the whole? →
  Cut it into equal parts → Count the parts you need → Check with a picture"). Number theory might use
  prime-power exponent tables for every GCF, LCM and divisor-count problem.
- **To extend an existing site** instead of starting fresh, replace the Task block with:

  > Add the attached problems to the existing practice page, matching its step format, nudge and hint
  > conventions, plan-guided worked stories, leakage rules and verification standard exactly. Update the
  > notes page's traps and signature-visual sections if the new problems introduce new ideas.
- **Saving progress.** By default nothing survives a reload: closing the tab clears progress and ink.
  If you want it saved, ask for it and say where it should live, because `localStorage` is ruled out.
- **Left out on purpose:**
  - animated transitions between frames, which add load without adding understanding;
  - a laser-pointer pen mode;
  - shape recognition.

  Ask for any of these if you want them.
