# 练习题模式 DataviewJS 模板

把下面这段代码块直接粘进 Obsidian 笔记即可运行。

```dataviewjs
(() => {
  const questions = [
    {
      type: "单选题",
      title: "如果要做“文章阅读量计数器”，最适合的数据结构是？",
      hint: "重点看哪种结构最适合原子自增。",
      choices: ["Hash", "String", "List", "Set"],
      correctIndex: 1,
      explanation: "计数器最典型的做法是 `String + INCR`。它简单、直接，而且 Redis 对数字字符串的自增支持很成熟。"
    },
    {
      type: "单选题",
      title: "如果用户资料经常只改 `nickname` 或 `avatar` 这样的单个字段，优先考虑哪种结构？",
      hint: "关注“按字段修改”的成本。",
      choices: ["String", "Hash", "List", "Set"],
      correctIndex: 1,
      explanation: "`Hash` 更适合按字段更新。你不需要先把整个对象读出来再整体写回，而是可以直接修改目标字段。"
    },
    {
      type: "判断题",
      title: "`Set` 很适合做共同好友这类能力，因为它天然支持集合运算。",
      hint: "想想交集这个动作。",
      choices: ["对", "错"],
      correctIndex: 0,
      explanation: "共同好友本质上就是交集问题。`Set` 的 `SINTER` 这类操作天然匹配这个需求。"
    },
    {
      type: "单选题",
      title: "如果要求“元素不能重复，同时还要按权重排序”，优先选哪个结构？",
      hint: "既要唯一性，也要排序能力。",
      choices: ["Set", "List", "Hash", "ZSet"],
      correctIndex: 3,
      explanation: "`ZSet` 同时满足成员唯一和按 `score` 排序两个要求，是这类场景的标准选择。"
    },
    {
      type: "单选题",
      title: "为什么 `List` 比 `Set` 更适合做最新评论流？",
      hint: "关注顺序和重复元素。",
      choices: [
        "因为 `List` 自动去重",
        "因为 `List` 保留顺序且允许重复",
        "因为 `List` 自带分数排序",
        "因为 `List` 天生适合字段更新"
      ],
      correctIndex: 1,
      explanation: "评论流首先关心的是顺序，新评论前置和最近 N 条读取都依赖有序序列；`List` 保留顺序，也允许重复元素。"
    }
  ];

  const state = {
    currentIndex: 0,
    selectedIndex: null,
    answered: false,
    completed: false,
    correctCount: 0,
    attempted: 0,
    streak: 0
  };

  const root = dv.el("div", "");
  root.className = "il-quiz-board";
  root.innerHTML = `
    <style>
      .il-quiz-board {
        --il-bg: color-mix(in srgb, var(--background-primary) 78%, var(--background-secondary));
        --il-panel: color-mix(in srgb, var(--background-primary) 90%, transparent);
        --il-panel-strong: color-mix(in srgb, var(--background-primary) 96%, transparent);
        --il-text: var(--text-normal);
        --il-muted: var(--text-muted);
        --il-line: color-mix(in srgb, var(--background-modifier-border) 72%, var(--text-muted));
        --il-accent: var(--interactive-accent);
        --il-accent-soft: color-mix(in srgb, var(--interactive-accent) 16%, transparent);
        --il-success: #1f8f55;
        --il-success-soft: rgba(31, 143, 85, 0.12);
        --il-danger: #c34b45;
        --il-danger-soft: rgba(195, 75, 69, 0.12);
        --il-shadow: 0 18px 38px rgba(0, 0, 0, 0.10);
        color: var(--il-text);
        margin: 1rem 0;
      }

      .il-quiz-board * {
        box-sizing: border-box;
      }

      .il-quiz-layout {
        display: grid;
        grid-template-columns: 260px minmax(0, 1fr);
        gap: 18px;
        align-items: stretch;
      }

      .il-quiz-panel {
        background: var(--il-panel);
        border: 1px solid var(--il-line);
        border-radius: 24px;
        box-shadow: var(--il-shadow);
        height: 100%;
      }

      .il-quiz-sidebar {
        padding: 22px;
        position: sticky;
        top: 16px;
        display: flex;
        flex-direction: column;
        align-self: stretch;
      }

      .il-quiz-main {
        padding: 22px;
        display: flex;
        flex-direction: column;
        align-self: stretch;
      }

      .il-pill {
        display: inline-flex;
        align-items: center;
        gap: 8px;
        padding: 8px 12px;
        border-radius: 999px;
        background: var(--il-accent-soft);
        color: var(--il-accent);
        font-size: 12px;
        font-weight: 700;
        letter-spacing: 0.04em;
      }

      .il-title {
        margin: 16px 0 10px;
        font-size: 28px;
        line-height: 1.2;
        font-weight: 800;
      }

      .il-copy,
      .il-meta,
      .il-hint,
      .il-explanation,
      .il-legend {
        color: var(--il-muted);
      }

      .il-copy {
        margin: 0 0 18px;
        line-height: 1.7;
        font-size: 14px;
      }

      .il-stats {
        display: grid;
        gap: 12px;
      }

      .il-stat {
        padding: 14px 16px;
        border-radius: 18px;
        border: 1px solid var(--il-line);
        background: var(--il-panel-strong);
      }

      .il-stat-label {
        font-size: 12px;
        color: var(--il-muted);
        margin-bottom: 6px;
      }

      .il-stat-value {
        font-size: 24px;
        font-weight: 800;
      }

      .il-legend {
        margin-top: 18px;
        padding-top: 18px;
        border-top: 1px dashed var(--il-line);
        font-size: 13px;
        line-height: 1.8;
      }

      .il-toolbar,
      .il-footer {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 12px;
        align-items: center;
      }

      .il-progress-wrap {
        margin: 18px 0 22px;
      }

      .il-progress-meta {
        display: flex;
        justify-content: space-between;
        gap: 12px;
        font-size: 13px;
        color: var(--il-muted);
        margin-bottom: 10px;
      }

      .il-progress {
        height: 10px;
        width: 100%;
        border-radius: 999px;
        background: color-mix(in srgb, var(--il-line) 40%, transparent);
        overflow: hidden;
      }

      .il-progress-bar {
        height: 100%;
        width: 0;
        border-radius: inherit;
        background: linear-gradient(90deg, var(--il-accent), color-mix(in srgb, var(--il-accent) 72%, white));
        transition: width 220ms ease;
      }

      .il-card {
        border: 1px solid var(--il-line);
        border-radius: 24px;
        background: var(--il-panel-strong);
        padding: 22px;
      }

      .il-question-type {
        font-size: 12px;
        font-weight: 700;
        letter-spacing: 0.04em;
        text-transform: uppercase;
        color: var(--il-accent);
        margin-bottom: 10px;
      }

      .il-question-title {
        margin: 0 0 14px;
        font-size: 26px;
        line-height: 1.45;
      }

      .il-hint {
        font-size: 14px;
        line-height: 1.7;
        margin-bottom: 20px;
      }

      .il-choices {
        display: grid;
        gap: 12px;
      }

      .il-choice {
        width: 100%;
        text-align: left;
        padding: 16px 18px;
        border-radius: 18px;
        border: 1px solid var(--il-line);
        background: transparent;
        color: inherit;
        cursor: pointer;
        font-size: 16px;
        line-height: 1.6;
        transition: transform 120ms ease, border-color 120ms ease, background 120ms ease;
      }

      .il-choice:hover:not(:disabled) {
        transform: translateY(-1px);
        border-color: var(--il-accent);
        background: var(--il-accent-soft);
      }

      .il-choice.selected {
        border-color: var(--il-accent);
        background: var(--il-accent-soft);
      }

      .il-choice.correct {
        border-color: var(--il-success);
        background: var(--il-success-soft);
      }

      .il-choice.incorrect {
        border-color: var(--il-danger);
        background: var(--il-danger-soft);
      }

      .il-feedback {
        margin-top: 18px;
        border-radius: 20px;
        padding: 16px 18px;
        border: 1px solid transparent;
        display: none;
      }

      .il-feedback.visible {
        display: block;
      }

      .il-feedback.correct {
        background: var(--il-success-soft);
        border-color: color-mix(in srgb, var(--il-success) 35%, transparent);
      }

      .il-feedback.incorrect {
        background: var(--il-danger-soft);
        border-color: color-mix(in srgb, var(--il-danger) 28%, transparent);
      }

      .il-feedback-title {
        font-weight: 800;
        margin-bottom: 8px;
      }

      .il-explanation {
        font-size: 15px;
        line-height: 1.8;
      }

      .il-footer {
        margin-top: 18px;
      }

      .il-button-group {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
      }

      .il-btn {
        border: 1px solid var(--il-line);
        border-radius: 14px;
        padding: 12px 16px;
        background: var(--il-panel-strong);
        color: inherit;
        cursor: pointer;
        font-size: 14px;
        font-weight: 700;
      }

      .il-btn-primary {
        border-color: transparent;
        background: var(--il-accent);
        color: var(--text-on-accent, #fff);
      }

      .il-btn:disabled {
        opacity: 0.45;
        cursor: not-allowed;
      }

      .il-toggle {
        display: inline-flex;
        align-items: center;
        gap: 10px;
        font-size: 14px;
        color: var(--il-muted);
      }

      @media (max-width: 900px) {
        .il-quiz-layout {
          grid-template-columns: 1fr;
        }

        .il-quiz-sidebar {
          position: static;
        }
      }
    </style>

    <div class="il-quiz-layout">
      <aside class="il-quiz-panel il-quiz-sidebar">
        <span class="il-pill">Practice Mode Template</span>
        <div class="il-title">练习题模式样板</div>
        <div class="il-copy">这个模板用于 Obsidian 内的自包含练习题场景：题卡作答、即时反馈、错题解析、手动决定是否进入下一题，以及完成态停留。</div>

        <div class="il-stats">
          <div class="il-stat">
            <div class="il-stat-label">当前进度</div>
            <div class="il-stat-value" data-role="stat-progress">1 / 5</div>
          </div>
          <div class="il-stat">
            <div class="il-stat-label">答对题数</div>
            <div class="il-stat-value" data-role="stat-correct">0</div>
          </div>
          <div class="il-stat">
            <div class="il-stat-label">连续正确</div>
            <div class="il-stat-value" data-role="stat-streak">0</div>
          </div>
        </div>

        <div class="il-legend">
          默认行为：
          <br>1. 选项点下后立即判定。
          <br>2. 答错时在卡片下方显示解析。
          <br>3. 默认停留当前题，等待你点击“下一题”或“重做本题”。
          <br>4. 最后一题完成后停留在完成态。
        </div>
      </aside>

      <main class="il-quiz-panel il-quiz-main">
        <div class="il-toolbar">
          <div>
            <div class="il-pill">DataviewJS Quiz Board</div>
            <div class="il-meta" style="margin-top: 10px;">这是 Obsidian 交互练习题的默认模板方向，不再退化成静态题单 + 底部答案，默认也不自动切题。</div>
          </div>
          <label class="il-toggle">
            <input type="checkbox" data-role="auto-next">
            答对后自动下一题
          </label>
        </div>

        <div class="il-progress-wrap">
          <div class="il-progress-meta">
            <span data-role="progress-label">第 1 题 / 共 5 题</span>
            <span data-role="progress-rate">正确率 0%</span>
          </div>
          <div class="il-progress">
            <div class="il-progress-bar" data-role="progress-bar"></div>
          </div>
        </div>

        <section class="il-card">
          <div class="il-question-type" data-role="question-type">单选题</div>
          <h2 class="il-question-title" data-role="question-title"></h2>
          <div class="il-hint" data-role="question-hint"></div>
          <div class="il-choices" data-role="choices"></div>

          <div class="il-feedback" data-role="feedback">
            <div class="il-feedback-title" data-role="feedback-title"></div>
            <div class="il-explanation" data-role="feedback-body"></div>
          </div>

          <div class="il-footer">
            <div class="il-button-group">
              <button class="il-btn" type="button" data-role="retry-btn" disabled>重做本题</button>
              <button class="il-btn" type="button" data-role="show-answer-btn">显示正确答案</button>
            </div>
            <div class="il-button-group">
              <button class="il-btn" type="button" data-role="reset-btn">重置练习</button>
              <button class="il-btn il-btn-primary" type="button" data-role="next-btn" disabled>下一题</button>
            </div>
          </div>
        </section>
      </main>
    </div>
  `;

  const $ = (selector) => root.querySelector(selector);
  const els = {
    questionType: $('[data-role="question-type"]'),
    questionTitle: $('[data-role="question-title"]'),
    questionHint: $('[data-role="question-hint"]'),
    choices: $('[data-role="choices"]'),
    feedback: $('[data-role="feedback"]'),
    feedbackTitle: $('[data-role="feedback-title"]'),
    feedbackBody: $('[data-role="feedback-body"]'),
    progressLabel: $('[data-role="progress-label"]'),
    progressRate: $('[data-role="progress-rate"]'),
    progressBar: $('[data-role="progress-bar"]'),
    statProgress: $('[data-role="stat-progress"]'),
    statCorrect: $('[data-role="stat-correct"]'),
    statStreak: $('[data-role="stat-streak"]'),
    nextBtn: $('[data-role="next-btn"]'),
    retryBtn: $('[data-role="retry-btn"]'),
    resetBtn: $('[data-role="reset-btn"]'),
    showAnswerBtn: $('[data-role="show-answer-btn"]'),
    autoNext: $('[data-role="auto-next"]')
  };

  function currentQuestion() {
    return questions[state.currentIndex];
  }

  function updateStats() {
    const total = questions.length;
    const rate = state.attempted === 0 ? 0 : Math.round((state.correctCount / state.attempted) * 100);
    const progress = state.completed ? total : state.currentIndex + 1;

    els.progressLabel.textContent = state.completed ? `已完成 ${total} / ${total} 题` : `第 ${progress} 题 / 共 ${total} 题`;
    els.progressRate.textContent = `正确率 ${rate}%`;
    els.progressBar.style.width = `${(progress / total) * 100}%`;
    els.statProgress.textContent = state.completed ? "完成" : `${progress} / ${total}`;
    els.statCorrect.textContent = String(state.correctCount);
    els.statStreak.textContent = String(state.streak);
  }

  function renderQuestion() {
    const q = currentQuestion();
    state.completed = false;
    state.selectedIndex = null;
    state.answered = false;

    els.questionType.textContent = q.type;
    els.questionTitle.textContent = q.title;
    els.questionHint.textContent = q.hint;
    els.choices.innerHTML = "";
    els.feedback.className = "il-feedback";
    els.feedbackTitle.textContent = "";
    els.feedbackBody.textContent = "";
    els.nextBtn.disabled = true;
    els.nextBtn.textContent = "下一题";
    els.retryBtn.disabled = true;
    els.showAnswerBtn.disabled = false;

    q.choices.forEach((choice, index) => {
      const button = document.createElement("button");
      button.className = "il-choice";
      button.type = "button";
      button.textContent = `${String.fromCharCode(65 + index)}. ${choice}`;
      button.addEventListener("click", () => submitAnswer(index));
      els.choices.appendChild(button);
    });

    updateStats();
  }

  function renderCompletion() {
    const total = questions.length;
    const rate = state.attempted === 0 ? 0 : Math.round((state.correctCount / state.attempted) * 100);
    state.completed = true;
    state.selectedIndex = null;
    state.answered = true;

    els.questionType.textContent = "练习完成";
    els.questionTitle.textContent = "已完成所有题目";
    els.questionHint.textContent = `本轮共完成 ${total} 题，答对 ${state.correctCount} 题，正确率 ${rate}%。可以点击“重置练习”再做一轮。`;
    els.choices.innerHTML = "";
    els.feedback.className = "il-feedback visible correct";
    els.feedbackTitle.textContent = "完成状态";
    els.feedbackBody.textContent = "这里停留在完成页，不再自动回到第一题。这个状态适合 Obsidian 练习笔记的收尾。";
    els.nextBtn.disabled = true;
    els.nextBtn.textContent = "已完成";
    els.retryBtn.disabled = true;
    els.showAnswerBtn.disabled = true;

    updateStats();
  }

  function markChoices(correctIndex, selectedIndex) {
    [...els.choices.children].forEach((button, index) => {
      button.disabled = true;
      button.classList.remove("selected", "correct", "incorrect");

      if (index === selectedIndex) {
        button.classList.add("selected");
      }
      if (index === correctIndex) {
        button.classList.add("correct");
      } else if (index === selectedIndex && selectedIndex !== correctIndex) {
        button.classList.add("incorrect");
      }
    });
  }

  function submitAnswer(index) {
    if (state.answered || state.completed) {
      return;
    }

    const q = currentQuestion();
    state.selectedIndex = index;
    state.answered = true;
    state.attempted += 1;

    const isCorrect = index === q.correctIndex;
    if (isCorrect) {
      state.correctCount += 1;
      state.streak += 1;
    } else {
      state.streak = 0;
    }

    markChoices(q.correctIndex, index);

    els.feedback.className = `il-feedback visible ${isCorrect ? "correct" : "incorrect"}`;
    els.feedbackTitle.textContent = isCorrect ? "回答正确" : "回答不正确";
    els.feedbackBody.textContent = q.explanation;
    els.nextBtn.disabled = false;
    els.retryBtn.disabled = false;
    updateStats();

    if (isCorrect && els.autoNext.checked) {
      window.setTimeout(() => {
        if (state.answered && !state.completed) {
          nextQuestion();
        }
      }, 700);
    }
  }

  function revealAnswer() {
    if (state.answered || state.completed) {
      return;
    }

    const q = currentQuestion();
    state.answered = true;
    state.streak = 0;
    markChoices(q.correctIndex, q.correctIndex);
    els.feedback.className = "il-feedback visible correct";
    els.feedbackTitle.textContent = "已显示参考答案";
    els.feedbackBody.textContent = q.explanation;
    els.nextBtn.disabled = false;
    els.retryBtn.disabled = false;
    updateStats();
  }

  function retryQuestion() {
    if (state.completed) {
      return;
    }
    renderQuestion();
  }

  function nextQuestion() {
    if (state.currentIndex >= questions.length - 1) {
      renderCompletion();
      return;
    }

    state.currentIndex += 1;
    renderQuestion();
  }

  function resetAll() {
    state.currentIndex = 0;
    state.selectedIndex = null;
    state.answered = false;
    state.completed = false;
    state.correctCount = 0;
    state.attempted = 0;
    state.streak = 0;
    renderQuestion();
  }

  els.nextBtn.addEventListener("click", nextQuestion);
  els.retryBtn.addEventListener("click", retryQuestion);
  els.resetBtn.addEventListener("click", resetAll);
  els.showAnswerBtn.addEventListener("click", revealAnswer);

  renderQuestion();
})();
```
