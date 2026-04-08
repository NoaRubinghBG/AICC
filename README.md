# AICC
Code AI Act Assessment tool
<!DOCTYPE html>
<html lang="nl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Compliance Quickscan — EU AI Act</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:opsz,wght@9..40,300;9..40,400;9..40,500;9..40,600&display=swap" rel="stylesheet">
<style>
  :root {
    --ink: #2F302F;
    --paper: #DEDAC6;
    --cream: #ede8dc;
    --accent: #B9B9EB;
    --accent-dark: #8585cc;
    --accent-light: #f0eff9;
    --muted: #7a7468;
    --white: #fdfaf5;
    --green: #2d7a5f;
    --green-light: #e4f0eb;
    --red: #c8401e;
    --red-light: #f5e8e4;
    --amber: #c9841e;
    --amber-light: #f5ede0;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--paper);
    color: var(--ink);
    min-height: 100vh;
    overflow-x: hidden;
  }

  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 9999;
    opacity: 0.4;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 28px 48px;
    border-bottom: 1px solid rgba(13,13,13,0.12);
    position: sticky;
    top: 0;
    background: var(--paper);
    z-index: 100;
  }

  .logo {
    font-family: 'DM Serif Display', serif;
    font-size: 1.25rem;
    letter-spacing: -0.01em;
    color: var(--ink);
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .logo-dot {
    width: 8px; height: 8px;
    background: var(--accent-dark);
    border-radius: 50%;
    display: inline-block;
  }

  .badge {
    font-size: 0.72rem;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    border: 1px solid rgba(13,13,13,0.18);
    padding: 6px 14px;
    border-radius: 100px;
  }

  .screen { display: none; animation: fadeUp 0.45s ease; }
  .screen.active { display: block; }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* ── INTRO ── */
  #screen-intro {
    max-width: 800px;
    margin: 0 auto;
    padding: 80px 48px 60px;
    text-align: center;
  }

  .eyebrow {
    font-size: 0.72rem;
    font-weight: 600;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    color: var(--accent-dark);
    margin-bottom: 22px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
  }

  .eyebrow::before, .eyebrow::after {
    content: '';
    display: block;
    width: 40px;
    height: 1px;
    background: var(--accent-dark);
    opacity: 0.5;
  }

  h1 {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(2.4rem, 5vw, 3.8rem);
    line-height: 1.1;
    letter-spacing: -0.02em;
    color: var(--ink);
    margin-bottom: 24px;
  }

  h1 em { font-style: italic; color: var(--accent-dark); }

  .hero-sub {
    font-size: 1.05rem;
    line-height: 1.7;
    color: var(--muted);
    font-weight: 300;
    max-width: 580px;
    margin: 0 auto 48px;
  }

  .meta-row {
    display: flex;
    justify-content: center;
    gap: 32px;
    margin-bottom: 48px;
    flex-wrap: wrap;
  }

  .meta-item {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 0.88rem;
    color: var(--muted);
  }

  .btn-primary {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    background: var(--ink);
    color: var(--white);
    font-family: 'DM Sans', sans-serif;
    font-size: 0.95rem;
    font-weight: 500;
    padding: 18px 40px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.25s ease;
    letter-spacing: 0.01em;
  }

  .btn-primary:hover {
    background: var(--accent-dark);
    transform: translateY(-2px);
    box-shadow: 0 12px 30px rgba(133,133,204,0.35);
  }

  .btn-primary .arrow { transition: transform 0.25s ease; }
  .btn-primary:hover .arrow { transform: translateX(4px); }

  .coverage-row {
    margin-top: 64px;
    padding-top: 40px;
    border-top: 1px solid rgba(13,13,13,0.1);
  }

  .coverage-label {
    font-size: 0.72rem;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 16px;
  }

  .tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
  }

  .tag {
    background: var(--cream);
    border: 1px solid rgba(13,13,13,0.1);
    font-size: 0.78rem;
    font-weight: 500;
    padding: 5px 12px;
    border-radius: 3px;
    color: var(--ink);
  }

  /* ── QUESTION SCREEN ── */
  #screen-question {
    max-width: 900px;
    margin: 0 auto;
    padding: 52px 48px 80px;
  }

  .progress-wrap {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-bottom: 12px;
  }

  .progress-bar-track {
    flex: 1;
    height: 3px;
    background: rgba(13,13,13,0.12);
    border-radius: 2px;
    overflow: hidden;
  }

  .progress-bar-fill {
    height: 100%;
    background: var(--accent-dark);
    transition: width 0.5s cubic-bezier(0.4,0,0.2,1);
    border-radius: 2px;
  }

  .progress-label {
    font-size: 0.78rem;
    font-weight: 600;
    color: var(--muted);
    white-space: nowrap;
  }

  .step-breadcrumb {
    font-size: 0.72rem;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--accent-dark);
    margin-bottom: 8px;
    margin-top: 28px;
  }

  .question-text {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(1.3rem, 2.5vw, 1.9rem);
    line-height: 1.3;
    letter-spacing: -0.02em;
    color: var(--ink);
    margin-bottom: 10px;
  }

  .question-hint {
    font-size: 0.875rem;
    color: var(--muted);
    margin-bottom: 8px;
    font-weight: 300;
    line-height: 1.6;
    background: var(--cream);
    border-left: 3px solid var(--accent-dark);
    padding: 10px 14px;
    border-radius: 0 4px 4px 0;
    margin-bottom: 28px;
  }

  .options-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 36px;
  }

  .option-btn {
    background: var(--white);
    border: 1.5px solid rgba(13,13,13,0.12);
    border-radius: 6px;
    padding: 16px 18px;
    text-align: left;
    cursor: pointer;
    transition: all 0.2s ease;
    display: flex;
    align-items: flex-start;
    gap: 14px;
    font-family: 'DM Sans', sans-serif;
    width: 100%;
  }

  .option-btn:hover {
    border-color: var(--accent-dark);
    background: var(--accent-light);
    transform: translateY(-1px);
    box-shadow: 0 6px 18px rgba(13,13,13,0.07);
  }

  .option-btn.selected {
    border-color: var(--accent-dark);
    background: var(--accent-light);
    box-shadow: 0 0 0 3px rgba(133,133,204,0.15);
  }

  .option-indicator {
    width: 20px; height: 20px;
    min-width: 20px;
    border: 1.5px solid rgba(13,13,13,0.2);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 2px;
    transition: all 0.2s ease;
    flex-shrink: 0;
  }

  .option-btn.selected .option-indicator {
    border-color: var(--accent-dark);
    background: var(--accent-dark);
  }

  .option-btn.selected .option-indicator::after {
    content: '';
    width: 7px; height: 7px;
    background: white;
    border-radius: 50%;
    display: block;
  }

  .option-text {
    font-size: 0.92rem;
    font-weight: 500;
    color: var(--ink);
    line-height: 1.4;
  }

  .option-sub {
    font-size: 0.8rem;
    color: var(--muted);
    font-weight: 300;
    margin-top: 3px;
    line-height: 1.4;
  }

  /* Multi-select checkbox style */
  .option-btn.checkbox .option-indicator {
    border-radius: 4px;
  }

  .option-btn.checkbox.selected .option-indicator::after {
    content: '✓';
    font-size: 0.75rem;
    border-radius: 0;
    background: transparent;
    color: white;
    width: auto;
    height: auto;
    font-weight: 700;
  }

  /* Free text input */
  .free-text-wrap {
    margin-bottom: 36px;
  }

  .free-text-input {
    font-family: 'DM Sans', sans-serif;
    font-size: 0.95rem;
    color: var(--ink);
    background: var(--white);
    border: 1.5px solid rgba(13,13,13,0.18);
    border-radius: 6px;
    padding: 14px 18px;
    width: 100%;
    outline: none;
    transition: border-color 0.2s;
    resize: vertical;
    min-height: 80px;
  }

  .free-text-input:focus { border-color: var(--accent-dark); }

  .nav-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .btn-back {
    background: none;
    border: none;
    font-family: 'DM Sans', sans-serif;
    font-size: 0.9rem;
    color: var(--muted);
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 8px 0;
    transition: color 0.2s;
  }

  .btn-back:hover { color: var(--ink); }

  .btn-next {
    background: var(--ink);
    color: var(--white);
    font-family: 'DM Sans', sans-serif;
    font-size: 0.9rem;
    font-weight: 500;
    padding: 14px 32px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 8px;
    transition: all 0.2s ease;
    opacity: 0.35;
    pointer-events: none;
  }

  .btn-next.enabled { opacity: 1; pointer-events: all; }
  .btn-next.enabled:hover { background: var(--accent-dark); transform: translateY(-1px); }

  /* ── EMAIL GATE ── */
  #screen-email {
    max-width: 560px;
    margin: 0 auto;
    padding: 100px 48px 80px;
    text-align: center;
  }

  .email-icon {
    width: 64px; height: 64px;
    background: var(--accent-light);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 auto 32px;
    font-size: 1.6rem;
  }

  .email-gate-title {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(1.8rem, 4vw, 2.6rem);
    letter-spacing: -0.02em;
    line-height: 1.15;
    margin-bottom: 16px;
  }

  .email-gate-title em { font-style: italic; color: var(--accent-dark); }

  .email-gate-sub {
    font-size: 0.95rem;
    color: var(--muted);
    font-weight: 300;
    line-height: 1.7;
    margin-bottom: 40px;
    max-width: 420px;
    margin-left: auto;
    margin-right: auto;
  }

  .email-field-wrap {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 14px;
  }

  .email-input, .name-input, .org-input {
    font-family: 'DM Sans', sans-serif;
    font-size: 1rem;
    color: var(--ink);
    background: var(--white);
    border: 1.5px solid rgba(13,13,13,0.18);
    border-radius: 6px;
    padding: 16px 20px;
    width: 100%;
    outline: none;
    transition: border-color 0.2s;
  }

  .email-input:focus, .name-input:focus, .org-input:focus { border-color: var(--accent-dark); }
  .email-input.error { border-color: var(--red); }

  .email-error {
    font-size: 0.8rem;
    color: var(--red);
    display: none;
    text-align: left;
    margin-top: -4px;
  }

  .email-error.visible { display: block; }

  .btn-email-submit {
    width: 100%;
    background: var(--ink);
    color: var(--white);
    font-family: 'DM Sans', sans-serif;
    font-size: 1rem;
    font-weight: 500;
    padding: 18px 32px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.25s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
  }

  .btn-email-submit:hover {
    background: var(--accent-dark);
    transform: translateY(-2px);
    box-shadow: 0 12px 30px rgba(133,133,204,0.35);
  }

  .btn-email-submit:disabled {
    opacity: 0.6;
    cursor: not-allowed;
    transform: none;
    box-shadow: none;
  }

  .email-privacy {
    font-size: 0.75rem;
    color: var(--muted);
    margin-top: 16px;
    line-height: 1.6;
    font-weight: 300;
  }

  /* ── RESULTS ── */
  #screen-results {
    max-width: 980px;
    margin: 0 auto;
    padding: 60px 48px 80px;
  }

  .results-header {
    margin-bottom: 48px;
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    flex-wrap: wrap;
    gap: 24px;
    border-bottom: 1px solid rgba(13,13,13,0.1);
    padding-bottom: 32px;
  }

  .results-title {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(1.8rem, 3.5vw, 2.8rem);
    letter-spacing: -0.02em;
    line-height: 1.15;
  }

  .results-title em { font-style: italic; color: var(--accent-dark); }

  .risk-band {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    font-size: 0.78rem;
    font-weight: 600;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    padding: 5px 14px;
    border-radius: 100px;
    margin-bottom: 10px;
  }

  .risk-band.high { background: var(--red-light); color: var(--red); }
  .risk-band.medium { background: var(--amber-light); color: var(--amber); }
  .risk-band.low { background: var(--green-light); color: var(--green); }
  .risk-band.blocked { background: #2a0a0a; color: #ff8c8c; }

  .section-title {
    font-size: 0.72rem;
    font-weight: 600;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 20px;
    margin-top: 48px;
  }

  /* Summary cards */
  .summary-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 16px;
    margin-bottom: 0;
  }

  .summary-card {
    background: var(--white);
    border: 1.5px solid rgba(13,13,13,0.1);
    border-radius: 8px;
    padding: 20px 22px;
  }

  .summary-card-label {
    font-size: 0.7rem;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 8px;
  }

  .summary-card-value {
    font-family: 'DM Serif Display', serif;
    font-size: 1.1rem;
    color: var(--ink);
    line-height: 1.3;
  }

  .summary-card-value.high { color: var(--red); }
  .summary-card-value.medium { color: var(--amber); }
  .summary-card-value.low { color: var(--green); }
  .summary-card-value.blocked { color: #cc2222; font-weight: 700; }

  /* Priority cards */
  .priority-list { margin-bottom: 48px; }

  .priority-card {
    background: var(--white);
    border: 1.5px solid rgba(13,13,13,0.1);
    border-radius: 8px;
    padding: 22px 24px;
    margin-bottom: 12px;
    display: flex;
    align-items: flex-start;
    gap: 16px;
  }

  .priority-card.blocker {
    border-color: var(--red);
    background: var(--red-light);
  }

  .priority-card.warning {
    border-color: var(--amber);
    background: var(--amber-light);
  }

  .priority-icon {
    width: 40px; height: 40px;
    min-width: 40px;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.15rem;
    flex-shrink: 0;
  }

  .priority-icon.blocker { background: var(--red-light); border: 1px solid rgba(200,64,30,0.2); }
  .priority-icon.warning { background: var(--amber-light); border: 1px solid rgba(201,132,30,0.2); }
  .priority-icon.info { background: var(--green-light); border: 1px solid rgba(45,122,95,0.2); }
  .priority-icon.note { background: var(--accent-light); border: 1px solid rgba(133,133,204,0.2); }

  .priority-body { flex: 1; }

  .priority-name {
    font-weight: 600;
    font-size: 0.98rem;
    color: var(--ink);
    margin-bottom: 5px;
  }

  .priority-desc {
    font-size: 0.85rem;
    color: var(--muted);
    line-height: 1.6;
    font-weight: 300;
  }

  .priority-regs {
    font-size: 0.75rem;
    color: var(--muted);
    margin-top: 8px;
    font-weight: 500;
    font-style: italic;
  }

  .priority-level {
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    margin-top: 6px;
    display: inline-block;
    padding: 2px 8px;
    border-radius: 3px;
  }

  .priority-level.blocker { color: var(--red); background: rgba(200,64,30,0.1); }
  .priority-level.warning { color: var(--amber); background: rgba(201,132,30,0.1); }
  .priority-level.info { color: var(--green); background: rgba(45,122,95,0.1); }
  .priority-level.note { color: var(--accent-dark); background: rgba(133,133,204,0.15); }

  /* Answers table */
  .answers-table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 12px;
    font-size: 0.86rem;
  }

  .answers-table th {
    text-align: left;
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    padding: 10px 14px;
    border-bottom: 2px solid rgba(13,13,13,0.12);
    background: var(--cream);
  }

  .answers-table td {
    padding: 11px 14px;
    border-bottom: 1px solid rgba(13,13,13,0.07);
    vertical-align: top;
    line-height: 1.5;
    color: var(--ink);
  }

  .answers-table tr:hover td { background: rgba(13,13,13,0.02); }

  .step-group-header td {
    font-size: 0.72rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--accent-dark);
    background: var(--accent-light) !important;
    padding: 8px 14px;
    border-bottom: 1px solid rgba(133,133,204,0.2);
  }

  /* CTA */
  .cta-block {
    background: var(--ink);
    border-radius: 10px;
    padding: 40px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 24px;
    margin-top: 48px;
  }

  .cta-text h3 {
    font-family: 'DM Serif Display', serif;
    font-size: 1.5rem;
    color: var(--white);
    margin-bottom: 6px;
  }

  .cta-text p {
    font-size: 0.88rem;
    color: rgba(255,255,255,0.55);
    font-weight: 300;
  }

  .btn-cta {
    background: var(--accent-dark);
    color: white;
    font-family: 'DM Sans', sans-serif;
    font-size: 0.9rem;
    font-weight: 600;
    padding: 14px 28px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    white-space: nowrap;
    transition: all 0.2s;
  }

  .btn-cta:hover { opacity: 0.88; transform: translateY(-1px); }

  .btn-restart {
    background: none;
    border: 1px solid rgba(255,255,255,0.2);
    color: rgba(255,255,255,0.7);
    font-family: 'DM Sans', sans-serif;
    font-size: 0.85rem;
    padding: 14px 24px;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .btn-restart:hover { border-color: rgba(255,255,255,0.5); color: white; }

  .disclaimer {
    font-size: 0.75rem;
    color: var(--muted);
    margin-top: 28px;
    line-height: 1.6;
    font-weight: 300;
    padding-top: 20px;
    border-top: 1px solid rgba(13,13,13,0.1);
  }

  @media (max-width: 640px) {
    header { padding: 20px 24px; }
    #screen-intro, #screen-question, #screen-results, #screen-email { padding: 36px 24px 60px; }
    .results-header { flex-direction: column; align-items: flex-start; }
    .summary-grid { grid-template-columns: 1fr 1fr; }
  }

  /* Skipped / N/A badge */
  .badge-skipped {
    display: inline-block;
    font-size: 0.7rem;
    font-weight: 600;
    padding: 2px 8px;
    border-radius: 3px;
    background: var(--cream);
    color: var(--muted);
    letter-spacing: 0.04em;
  }

  /* Outcome pill */
  .outcome-pill {
    display: inline-block;
    font-size: 0.72rem;
    font-weight: 700;
    padding: 3px 10px;
    border-radius: 100px;
    letter-spacing: 0.05em;
    text-transform: uppercase;
  }

  .outcome-pill.ok { background: var(--green-light); color: var(--green); }
  .outcome-pill.warn { background: var(--amber-light); color: var(--amber); }
  .outcome-pill.block { background: var(--red-light); color: var(--red); }
  .outcome-pill.na { background: var(--cream); color: var(--muted); }

  /* Conditional notice */
  .skip-notice {
    background: var(--cream);
    border: 1px solid rgba(13,13,13,0.1);
    border-radius: 6px;
    padding: 14px 18px;
    font-size: 0.85rem;
    color: var(--muted);
    margin-bottom: 28px;
    display: flex;
    gap: 10px;
    align-items: flex-start;
  }

  .skip-notice-icon { flex-shrink: 0; }
</style>
</head>
<body>

<header>
  <div class="logo">
    <span class="logo-dot"></span>
    AI Compliance Scan
  </div>
  <div class="badge">EU AI Act · Module 1</div>
</header>

<!-- INTRO SCREEN -->
<div id="screen-intro" class="screen active">
  <div class="eyebrow">EU AI Act Compliance</div>
  <h1>Bepaal uw <em>AI Act</em><br>verplichtingen in minuten</h1>
  <p class="hero-sub">Beantwoord de vragen over uw AI-systeem. Wij bepalen stapsgewijs of de AI Act van toepassing is, welke risicoklasse uw systeem heeft en welke verplichtingen gelden voor uw organisatie.</p>

  <div class="meta-row">
    <div class="meta-item">
      <svg width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
      ~10 minuten
    </div>
    <div class="meta-item">
      <svg width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path d="M9 12l2 2 4-4"/><path d="M12 2a10 10 0 100 20A10 10 0 0012 2z"/></svg>
      53 gerichte vragen
    </div>
    <div class="meta-item">
      <svg width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0110 0v4"/></svg>
      100% vertrouwelijk
    </div>
  </div>

  <button class="btn-primary" onclick="startScan()">
    Start de quickscan
    <span class="arrow">→</span>
  </button>

  <div class="coverage-row">
    <div class="coverage-label">Structuur van de scan</div>
    <div class="tags">
      <span class="tag">Stap 1 — AI-systeemkwalificatie</span>
      <span class="tag">Stap 2 — Territoriale scope</span>
      <span class="tag">Stap 3 — Uitzonderingen</span>
      <span class="tag">Stap 4 — Verboden toepassingen</span>
      <span class="tag">Stap 5 — Risicoclassificatie</span>
      <span class="tag">Stap 6 — Rol in AI-keten</span>
      <span class="tag">Stap 7 — GPAI-regime</span>
    </div>
  </div>
</div>

<!-- QUESTION SCREEN -->
<div id="screen-question" class="screen">
  <div class="progress-wrap">
    <div class="progress-bar-track">
      <div class="progress-bar-fill" id="progress-fill" style="width:0%"></div>
    </div>
    <div class="progress-label" id="progress-label">1 / 53</div>
  </div>

  <div class="step-breadcrumb" id="q-step"></div>
  <div class="question-text" id="q-text"></div>
  <div class="question-hint" id="q-hint" style="display:none"></div>

  <div id="skip-notice" class="skip-notice" style="display:none">
    <span class="skip-notice-icon">ℹ️</span>
    <span id="skip-notice-text"></span>
  </div>

  <div id="options-wrap">
    <div class="options-list" id="options-list"></div>
  </div>
  <div id="free-text-wrap" class="free-text-wrap" style="display:none">
    <textarea class="free-text-input" id="free-text-input" placeholder="Typ uw antwoord hier..."></textarea>
  </div>

  <div class="nav-row">
    <button class="btn-back" id="btn-back" onclick="prevQuestion()">← Vorige</button>
    <button class="btn-next" id="btn-next" onclick="nextQuestion()">Volgende →</button>
  </div>
</div>

<!-- EMAIL GATE -->
<div id="screen-email" class="screen">
  <div class="email-icon">📋</div>
  <div class="email-gate-title">Uw rapport is <em>gereed</em></div>
  <p class="email-gate-sub">Vul uw gegevens in om uw persoonlijk compliance-rapport te ontvangen. Het rapport wordt direct naar uw e-mailadres verzonden.</p>

  <div class="email-field-wrap">
    <input type="text" id="user-name" class="name-input" placeholder="Uw naam" />
    <input type="text" id="user-org" class="org-input" placeholder="Organisatie / bedrijfsnaam" />
    <input type="email" id="user-email" class="email-input" placeholder="uw@emailadres.nl" oninput="clearEmailError()" />
    <div class="email-error" id="email-error">Voer een geldig e-mailadres in.</div>
    <button class="btn-email-submit" id="btn-submit-email" onclick="submitEmail()">
      Bekijk mijn compliance-rapport →
    </button>
  </div>

  <p class="email-privacy">🔒 Uw resultaten worden naar uw e-mailadres verzonden. Wij respecteren uw privacy en delen uw gegevens niet met derden.</p>
</div>

<!-- RESULTS SCREEN -->
<div id="screen-results" class="screen">
  <div class="results-header">
    <div>
      <div id="risk-band" class="risk-band high"></div>
      <div class="results-title">Uw <em>AI Act</em><br>compliance-overzicht</div>
      <div style="margin-top:10px;font-size:0.88rem;color:var(--muted);font-weight:300" id="results-user-info"></div>
    </div>
  </div>

  <p class="section-title">Kernuitkomsten</p>
  <div class="summary-grid" id="summary-grid"></div>

  <p class="section-title">Bevindingen &amp; aandachtspunten</p>
  <div class="priority-list" id="priority-list"></div>

  <p class="section-title">Uw antwoorden</p>
  <table class="answers-table" id="answers-table">
    <thead>
      <tr>
        <th>Vraag</th>
        <th>Uw antwoord</th>
      </tr>
    </thead>
    <tbody id="answers-body"></tbody>
  </table>

  <div class="cta-block">
    <div class="cta-text">
      <h3>Klaar om actie te ondernemen?</h3>
      <p>Boek een gratis 30 minuten gesprek met een AI-compliance specialist en zet de uitkomsten om in een concreet actieplan.</p>
    </div>
    <div style="display:flex;gap:12px;flex-wrap:wrap">
      <button class="btn-cta" onclick="window.open('mailto:info@bg.legal?subject=Afspraak AI Compliance','_blank')">Neem contact op →</button>
      <button class="btn-restart" onclick="restart()">Opnieuw beginnen</button>
    </div>
  </div>

  <p class="disclaimer">⚠️ Deze quickscan biedt een algemeen compliance-overzicht op basis van uw antwoorden en vormt geen juridisch advies. Regelgevingsverplichtingen hangen af van uw specifieke context, jurisdictie en systeemontwerp. Raadpleeg voor specifiek juridisch advies een gekwalificeerde jurist gespecialiseerd in EU digitale regelgeving.</p>
</div>

<script>
// ═══════════════════════════════════════════════════════════════
// QUESTION DEFINITIONS
// type: 'single' = radio, 'multi' = checkboxes, 'free' = textarea
//
// IMPORTANT: skipIf is now ONLY used to conditionally show/hide
// questions that are logically irrelevant (e.g. deployer questions
// when the user is a provider). It no longer terminates the scan.
// ALL steps are ALWAYS shown and included in the final report.
// ═══════════════════════════════════════════════════════════════

const STEPS = {
  s1:  'Stap 1 — Is er sprake van een AI-systeem?',
  s2:  'Stap 2 — Territoriale en personele scope',
  s3:  'Stap 3 — Uitzonderingen op de toepasselijkheid',
  s4:  'Stap 4 — Verboden AI-toepassingen (art. 5)',
  s5a: 'Stap 5A — Hoog-risicotest: productregelgeving (art. 6 lid 1)',
  s5b: 'Stap 5B — Hoog-risicotest: Bijlage III (art. 6 lid 2)',
  s5c: 'Stap 5C — Uitzonderingstoets hoog risico (art. 6 lid 3)',
  s5d: 'Stap 5D — Beperkt-risicocheck (art. 50)',
  s6:  'Stap 6 — Rol in de AI-keten',
  s6a: 'Stap 6A — Verplichtingen aanbieders (hoog risico)',
  s6b: 'Stap 6B — Verplichtingen gebruikers/deployers (hoog risico)',
  s7:  'Stap 7 — General Purpose AI (GPAI)'
};

const questions = [
  // ── STAP 1 ─────────────────────────────────────────────────
  {
    id: 'v1_1', step: 's1',
    text: 'Wat doet het systeem concreet?',
    hint: 'De AI Act hanteert een specifieke definitie van een "AI-systeem". Niet alle software of algoritmen vallen hieronder. Selecteer de beste omschrijving.',
    type: 'single',
    options: [
      { label: 'Vaste, door mensen geprogrammeerde regels ("als X dan Y") zonder enige vorm van leren of redeneren', sub: '', value: 'rules' },
      { label: 'Statistische methoden, machine learning, deep learning of neurale netwerken', sub: 'Om op basis van data patronen te herkennen of voorspellingen te doen', value: 'ml' },
      { label: 'Logisch redeneren, optimalisatie of zoekmethoden', sub: 'Om conclusies te trekken of beslissingen te nemen', value: 'logic' },
      { label: 'Generatie van tekst, afbeeldingen, audio, video of andere content op basis van data of prompts', sub: '', value: 'gen' },
      { label: 'Combinatie van meerdere methoden', sub: '', value: 'combo' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_2', step: 's1',
    text: 'Genereert het systeem outputs — zoals voorspellingen, aanbevelingen, beslissingen, classificaties of gegenereerde content — op basis van de verwerking van input?',
    hint: 'Systemen die enkel data opslaan of weergeven zonder eigen outputs te genereren, vallen buiten de definitie van een AI-systeem.',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee — het systeem slaat data op of toont data, maar genereert geen eigen outputs', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_3', step: 's1',
    text: 'Zijn deze outputs bedoeld om te worden gebruikt voor besluitvorming, het beïnvloeden van omgevingen of het genereren van content — ook als er een mens tussenbeide komt?',
    hint: 'De definitie omvat ook systemen waarbij een mens de uiteindelijke beslissing neemt op basis van de AI-output.',
    type: 'single',
    options: [
      { label: 'Ja — outputs worden gebruikt om besluiten te onderbouwen of te nemen (al dan niet automatisch)', sub: '', value: 'decision' },
      { label: 'Ja — outputs zijn bedoeld om content te genereren die extern wordt gebruikt', sub: '', value: 'content' },
      { label: 'Nee — outputs dienen uitsluitend als interne technische tussenstap zonder externe toepassing', sub: '', value: 'internal' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  // ── STAP 2 ─────────────────────────────────────────────────
  {
    id: 'v1_4', step: 's2',
    text: 'Waar wordt het AI-systeem op de markt gebracht of in gebruik gesteld?',
    hint: 'De AI Act heeft een brede extraterritoriale werking: ook systemen die buiten de EU worden ontwikkeld maar in de EU worden ingezet of waarvan de output in de EU wordt gebruikt, kunnen onder de AI Act vallen.',
    type: 'single',
    options: [
      { label: 'In de Europese Unie (of een deel daarvan)', sub: '', value: 'eu' },
      { label: 'Buiten de EU, maar de output wordt gebruikt door personen of organisaties in de EU', sub: '', value: 'outside_eu_effect' },
      { label: 'Uitsluitend buiten de EU — de output heeft geen effect op personen of entiteiten in de EU', sub: '', value: 'outside_no_effect' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_5', step: 's2',
    text: 'Bevindt de organisatie die het systeem aanbiedt of inzet zich in de EU, of heeft zij een vestiging in de EU?',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja — de organisatie is gevestigd in de EU', sub: '', value: 'yes' },
      { label: 'Nee — de organisatie is buiten de EU gevestigd, maar het systeem wordt in de EU ingezet of heeft effect op personen in de EU', sub: '', value: 'no_but_effect' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_6', step: 's2',
    text: 'Zijn de personen of entiteiten die worden beïnvloed door de output van het systeem gevestigd of aanwezig in de EU?',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  // ── STAP 3 ─────────────────────────────────────────────────
  {
    id: 'v1_7', step: 's3',
    text: 'Is het systeem uitsluitend bestemd voor gebruik voor militaire doeleinden, nationale veiligheid of defensie?',
    hint: 'Dergelijke toepassingen zijn expliciet uitgezonderd van de AI Act (art. 2 lid 3), ook als zij technisch gezien kwalificeren als AI-systeem.',
    type: 'single',
    options: [
      { label: 'Ja — uitsluitend militair of nationale veiligheidsdoeleinden', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Gedeeltelijk of weet ik niet', sub: '', value: 'partial' }
    ]
  },
  {
    id: 'v1_8', step: 's3',
    text: 'Is het systeem uitsluitend bestemd voor wetenschappelijk onderzoek en ontwikkeling, waarbij het systeem nog niet op de markt is gebracht en niet in gebruik is gesteld?',
    hint: 'De onderzoeksuitzondering geldt uitsluitend voor pre-commercieel onderzoek. Zodra het systeem in productie gaat, vervalt de uitzondering.',
    type: 'single',
    options: [
      { label: 'Ja — uitsluitend pre-commercieel onderzoek en ontwikkeling', sub: '', value: 'yes' },
      { label: 'Gedeeltelijk — het systeem wordt ook al toegepast buiten de pure onderzoekscontext', sub: '', value: 'partial' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_9', step: 's3',
    text: 'Is het systeem uitsluitend bestemd voor persoonlijk, niet-professioneel gebruik door particulieren?',
    hint: 'Bijv. een zelfgebouwde tool voor eigen huishoudelijk gebruik. Professioneel gebruik, ook als intern hulpmiddel, valt buiten deze uitzondering.',
    type: 'single',
    options: [
      { label: 'Ja — uitsluitend persoonlijk en niet-professioneel gebruik', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  // ── STAP 4 — Verboden toepassingen ─────────────────────────
  {
    id: 'v1_10', step: 's4',
    text: 'Maakt het systeem gebruik van subliminale technieken, manipulatieve methoden of misleidende technieken om het gedrag van personen te beïnvloeden op een wijze die hun schade kan berokkenen — buiten hun bewustzijn?',
    hint: 'De volgende vragen gaan over toepassingen die de AI Act absoluut verbiedt (art. 5). Een "ja of mogelijk" leidt tot een directe rode vlag in het rapport. Juridisch advies is in dat geval direct noodzakelijk.',
    type: 'single',
    options: [
      { label: 'Ja of mogelijk', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_11', step: 's4',
    text: 'Maakt het systeem gebruik van kwetsbaarheden van specifieke groepen (kinderen, ouderen, personen met een beperking) om hun gedrag te beïnvloeden op een wijze die hen of anderen schade kan berokkenen?',
    hint: '(Art. 5 lid 1 sub b AI Act)',
    type: 'single',
    options: [
      { label: 'Ja of mogelijk', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_12', step: 's4',
    text: 'Wordt het systeem gebruikt voor het beoordelen of classificeren van personen op basis van sociaal gedrag of persoonlijke kenmerken over een langere periode, met als gevolg nadelige behandeling — social scoring door overheidsinstanties?',
    hint: '(Art. 5 lid 1 sub c AI Act)',
    type: 'single',
    options: [
      { label: 'Ja of mogelijk', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_13', step: 's4',
    text: 'Wordt het systeem gebruikt voor biometrische real-time identificatie op afstand (bijv. gezichtsherkenning in openbare ruimten) voor rechtshandhavingsdoeleinden, buiten de wettelijke uitzonderingen?',
    hint: '(Art. 5 lid 1 sub d AI Act) — Smallere uitzonderingen bestaan voor opsporing van ernstige misdrijven, terrorisme en vermiste personen.',
    type: 'single',
    options: [
      { label: 'Ja of mogelijk', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Niet van toepassing', sub: '', value: 'na' }
    ]
  },
  {
    id: 'v1_14', step: 's4',
    text: 'Maakt het systeem gebruik van emotieherkenning op de werkplek of in onderwijsinstellingen?',
    hint: '(Art. 5 lid 1 sub f AI Act)',
    type: 'single',
    options: [
      { label: 'Ja of mogelijk', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_15', step: 's4',
    text: 'Wordt het systeem gebruikt voor biometrische categorisering van personen op basis van gevoelige kenmerken zoals ras, politieke opvattingen, religie, seksuele gerichtheid of overtuigingen?',
    hint: '(Art. 5 lid 1 sub g AI Act)',
    type: 'single',
    options: [
      { label: 'Ja of mogelijk', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_16', step: 's4',
    text: 'Wordt het systeem gebruikt voor het scrapen van gezichtsafbeeldingen van internet of CCTV-beelden voor het aanleggen of uitbreiden van gezichtsherkenningsdatabases?',
    hint: '(Art. 5 lid 1 sub e AI Act)',
    type: 'single',
    options: [
      { label: 'Ja of mogelijk', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  // ── STAP 5A — Productregelgeving ────────────────────────────
  {
    id: 'v1_17', step: 's5a',
    text: 'Maakt het AI-systeem deel uit van een product dat valt onder EU-productregelgeving en is het AI-systeem de veiligheidscomponent van dat product?',
    hint: 'Een AI-systeem dat als veiligheidscomponent in een product wordt gebruikt dat onder EU-productregelgeving valt, wordt automatisch als hoog-risico aangemerkt (art. 6 lid 1 AI Act). Meerdere antwoorden mogelijk.',
    type: 'multi',
    options: [
      { label: 'Ja — veiligheidscomponent van een medisch hulpmiddel (MDR/IVDR)', sub: '', value: 'mdr' },
      { label: 'Ja — veiligheidscomponent van een machine, speelgoed, lift, explosief of radioapparatuur', sub: '', value: 'machinery' },
      { label: 'Ja — veiligheidscomponent van een voertuig of luchtvaartcomponent', sub: '', value: 'vehicle' },
      { label: 'Nee — het systeem maakt geen deel uit van een dergelijk product', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  // ── STAP 5B — Bijlage III ───────────────────────────────────
  {
    id: 'v1_18', step: 's5b',
    text: 'Wordt het systeem ingezet op het gebied van biometrische identificatie of categorisering van personen? (Bijlage III, nr. 1)',
    hint: 'Deze vragen toetsen of het systeem in een van de hoog-risico toepassingsgebieden van Bijlage III AI Act valt.',
    type: 'single',
    options: [
      { label: 'Ja — het systeem identificeert personen op afstand aan de hand van biometrische kenmerken', sub: '', value: 'identify' },
      { label: 'Ja — het systeem categoriseert personen op basis van biometrische kenmerken', sub: '', value: 'categorize' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_19', step: 's5b',
    text: 'Wordt het systeem ingezet voor het beheer en de werking van kritieke infrastructuur? (Bijlage III, nr. 2)',
    hint: 'Bijv. energie, water, gas, verwarming, wegverkeer, of digitale infrastructuur.',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_20', step: 's5b',
    text: 'Wordt het systeem ingezet op het gebied van onderwijs of beroepsopleiding? (Bijlage III, nr. 3)',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja — het systeem bepaalt toegang tot of plaatsing in onderwijs- of beroepsopleidingsinstellingen', sub: '', value: 'access' },
      { label: 'Ja — het systeem beoordeelt prestaties, past leertrajecten aan, of monitoreert studenten tijdens examens', sub: '', value: 'assess' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_21', step: 's5b',
    text: 'Wordt het systeem ingezet op het gebied van werkgelegenheid, personeelsmanagement of toegang tot zelfstandig werk? (Bijlage III, nr. 4)',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja — werving of selectie van personen (bijv. screening van sollicitaties, sollicitatiegesprekken)', sub: '', value: 'recruitment' },
      { label: 'Ja — beslissingen over promotie, ontslag, taakverdeling of prestatiebewaking van werknemers', sub: '', value: 'performance' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_22', step: 's5b',
    text: 'Wordt het systeem ingezet voor toegang tot of genot van essentiële private of publieke diensten? (Bijlage III, nr. 5)',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja — beoordeling van kredietwaardigheid of kredietscore', sub: '', value: 'credit' },
      { label: 'Ja — risicobeoordeling en prijsstelling bij levens- of zorgverzekeringen', sub: '', value: 'insurance' },
      { label: 'Ja — evaluatie en classificatie van noodoproepen of prioritering hulpdiensten', sub: '', value: 'emergency' },
      { label: 'Ja — beoordeling van aanspraken op sociale uitkeringen of bijstand', sub: '', value: 'benefits' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_23', step: 's5b',
    text: 'Wordt het systeem ingezet door of namens rechtshandhavingsautoriteiten? (Bijlage III, nr. 6)',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja — risicobeoordeling voor individuele personen (bijv. recidiverisico)', sub: '', value: 'risk' },
      { label: 'Ja — gebruik als leugendetector of vergelijkbaar instrument', sub: '', value: 'lie' },
      { label: 'Ja — beoordeling van betrouwbaarheid van bewijs in strafrechtelijke procedures', sub: '', value: 'evidence' },
      { label: 'Ja — profiling van personen in de context van opsporing of vervolging', sub: '', value: 'profiling' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_24', step: 's5b',
    text: 'Wordt het systeem ingezet op het gebied van migratie, asiel of grensbeheer? (Bijlage III, nr. 7)',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_25', step: 's5b',
    text: 'Wordt het systeem ingezet voor rechtsbedeling of democratische processen? (Bijlage III, nr. 8)',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja — het systeem ondersteunt rechtbanken of gerechtelijke autoriteiten bij het nemen van beslissingen', sub: '', value: 'courts' },
      { label: 'Ja — het systeem wordt ingezet voor beïnvloeding van verkiezingen of democratische processen', sub: '', value: 'elections' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  // ── STAP 5C — Uitzondering op hoog risico ──────────────────
  {
    id: 'v1_26', step: 's5c',
    text: 'Is het AI-systeem bedoeld om uitsluitend voorbereidende taken uit te voeren die louter ter ondersteuning dienen, waarbij het geen directe invloed heeft op enige beslissing of uitkomst in de hoog-risico context?',
    hint: 'Dit is de zogeheten art. 6 lid 3-uitzondering. Bijv. een systeem dat documenten samenvat maar waarbij een mens altijd zelfstandig en onafhankelijk beslist. Beantwoord dit altijd — ook als u hierboven geen Bijlage III-toepassing heeft aangegeven.',
    type: 'single',
    options: [
      { label: 'Ja — het systeem is uitsluitend een voorbereidend hulpmiddel zonder directe invloed op de beslissing', sub: '', value: 'yes' },
      { label: 'Nee — de output van het systeem heeft directe invloed op de uiteindelijke beslissing of uitkomst', sub: '', value: 'no' },
      { label: 'Niet van toepassing — het systeem valt niet in een Bijlage III-toepassingsgebied', sub: '', value: 'na' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  // ── STAP 5D — Beperkt risico ────────────────────────────────
  {
    id: 'v1_27', step: 's5d',
    text: 'Interageert het systeem rechtstreeks met mensen via een geautomatiseerde interface (bijv. een chatbot of een virtuele assistent)?',
    hint: 'Voor bepaalde AI-systemen gelden transparantieverplichtingen (art. 50 AI Act), ook als ze niet hoog-risico zijn. Beantwoord dit altijd.',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_28', step: 's5d',
    text: 'Genereert het systeem synthetische of gemanipuleerde tekst, audio, video of afbeeldingen die als echt kunnen worden beschouwd ("deepfakes" of AI-gegenereerde content)?',
    hint: '(Art. 50 AI Act)',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_29', step: 's5d',
    text: 'Maakt het systeem gebruik van emotieherkenning of biometrische categorisering als functionaliteit?',
    hint: '(Art. 50 AI Act)',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  // ── STAP 6 — Rol in de AI-keten ─────────────────────────────
  {
    id: 'v1_30', step: 's6',
    text: 'Ontwikkelt uw organisatie het AI-systeem en brengt zij het op de markt of stelt zij het in gebruik — ook als dat uitsluitend intern is?',
    hint: 'De AI Act onderscheidt: de "aanbieder" (provider) die het systeem ontwikkelt en op de markt brengt, de "gebruiker" (deployer) die het systeem inzet in eigen context, de "importeur" en de "distributeur". Uw verplichtingen hangen sterk af van uw rol.',
    type: 'single',
    options: [
      { label: 'Ja — wij ontwikkelen het systeem zelf en zetten het intern in', sub: '', value: 'provider_internal' },
      { label: 'Ja — wij ontwikkelen het systeem en bieden het aan externe partijen aan', sub: '', value: 'provider_external' },
      { label: 'Nee — wij gebruiken een door een derde partij ontwikkeld systeem', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_31', step: 's6',
    text: 'Zet uw organisatie het AI-systeem in voor eigen doeleinden, in uw eigen context, onder uw eigen verantwoordelijkheid — zonder het systeem zelf te hebben ontwikkeld?',
    hint: '',
    type: 'single',
    skipIf: (a) => isProvider(a),
    options: [
      { label: 'Ja — wij zijn gebruiker/deployer van een systeem ontwikkeld door een derde partij', sub: '', value: 'yes' },
      { label: 'Nee — wij distribueren of wederverkopen het systeem aan anderen', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_32', step: 's6',
    text: 'Brengt uw organisatie een door een derde partij ontwikkeld AI-systeem op de markt in de EU, zonder substantiële wijzigingen?',
    hint: '',
    type: 'single',
    skipIf: (a) => isProvider(a) || a['v1_31'] === 'yes',
    options: [
      { label: 'Ja — wij zijn importeur (het systeem is door een niet-EU-aanbieder ontwikkeld)', sub: '', value: 'importer' },
      { label: 'Ja — wij zijn distributeur (wij brengen het systeem op de markt zonder substantiële wijzigingen)', sub: '', value: 'distributor' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_33', step: 's6',
    text: 'Wijzigt uw organisatie het AI-systeem substantieel, of ontwikkelt zij een eigen toepassing bovenop een bestaand AI-systeem van een derde partij?',
    hint: 'Bij substantiële wijziging wordt de organisatie aangemerkt als aanbieder voor de gewijzigde versie (art. 25 AI Act) en gelden de zwaardere aanbiederverplichtingen.',
    type: 'single',
    skipIf: (a) => isProvider(a),
    options: [
      { label: 'Ja — wij passen het systeem substantieel aan of bouwen een eigen product op basis van een bestaand AI-systeem', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  // ── STAP 6A — Verplichtingen aanbieders ─────────────────────
  {
    id: 'v1_34', step: 's6a',
    text: 'Is er een kwaliteitsbeheersysteem (Quality Management System) ingericht dat voldoet aan de vereisten van art. 17 AI Act?',
    hint: 'Verplicht voor aanbieders van hoog-risico AI-systemen. Beantwoord dit ook als u nog niet zeker weet of uw systeem hoog-risico is.',
    type: 'single',
    options: [
      { label: 'Ja — gedocumenteerd QMS aanwezig', sub: '', value: 'yes' },
      { label: 'Gedeeltelijk — QMS is gedeeltelijk ingericht', sub: '', value: 'partial' },
      { label: 'Nee — geen QMS aanwezig', sub: '', value: 'no' },
      { label: 'In ontwikkeling', sub: '', value: 'development' }
    ]
  },
  {
    id: 'v1_35', step: 's6a',
    text: 'Is er technische documentatie opgesteld conform de vereisten van art. 11 en Bijlage IV AI Act?',
    hint: 'Technische documentatie is een voorwaarde voor de conformiteitsbeoordeling. Zonder volledige documentatie mag het systeem niet op de markt worden gebracht.',
    type: 'single',
    options: [
      { label: 'Ja — volledige technische documentatie conform Bijlage IV', sub: '', value: 'yes' },
      { label: 'Gedeeltelijk — documentatie is onvolledig', sub: '', value: 'partial' },
      { label: 'Nee — geen technische documentatie aanwezig', sub: '', value: 'no' },
      { label: 'In ontwikkeling', sub: '', value: 'development' }
    ]
  },
  {
    id: 'v1_36', step: 's6a',
    text: 'Is er een systeem voor automatische logging van gebeurtenissen (audit trail) ingericht conform art. 12 AI Act?',
    hint: 'Logging is verplicht om toezicht en controle door bevoegde autoriteiten mogelijk te maken.',
    type: 'single',
    options: [
      { label: 'Ja — volledige logging/audit trail aanwezig', sub: '', value: 'yes' },
      { label: 'Gedeeltelijk', sub: '', value: 'partial' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_37', step: 's6a',
    text: 'Zijn de vereiste transparantie-informatie en gebruikersinstructies conform art. 13 AI Act beschikbaar gesteld aan gebruikers?',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Gedeeltelijk', sub: '', value: 'partial' },
      { label: 'Nee', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_38', step: 's6a',
    text: 'Is er een conformiteitsbeoordelingsprocedure doorlopen conform art. 43 AI Act?',
    hint: 'Hoog-risico AI-systemen mogen niet op de markt worden gebracht zonder afgeronde conformiteitsbeoordeling.',
    type: 'single',
    options: [
      { label: 'Ja — self-assessment (intern)', sub: '', value: 'self' },
      { label: 'Ja — beoordeling door een Notified Body', sub: '', value: 'notified' },
      { label: 'Nee — maar vereist', sub: '', value: 'no_required' },
      { label: 'Nee — niet van toepassing geacht', sub: '', value: 'no_na' }
    ]
  },
  {
    id: 'v1_39', step: 's6a',
    text: 'Is het systeem geregistreerd in de EU-databank voor hoog-risico AI-systemen conform art. 49 AI Act?',
    hint: 'Registratie is verplicht vóór marktintroductie. Niet-registratie: boete tot €15M of 3% van de wereldwijde jaaromzet.',
    type: 'single',
    options: [
      { label: 'Ja — systeem is geregistreerd', sub: '', value: 'yes' },
      { label: 'Nee — maar vereist', sub: '', value: 'no_required' },
      { label: 'In voorbereiding', sub: '', value: 'planned' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_40', step: 's6a',
    text: 'Is er een post-market monitoringplan ingericht conform art. 72 AI Act?',
    hint: 'Post-market monitoring is een wettelijke verplichting voor hoog-risico AI-aanbieders — niet optioneel.',
    type: 'single',
    options: [
      { label: 'Ja — monitoringplan gedocumenteerd en geïmplementeerd', sub: '', value: 'yes' },
      { label: 'Nee — geen monitoringplan aanwezig', sub: '', value: 'no' },
      { label: 'In ontwikkeling', sub: '', value: 'development' }
    ]
  },
  // ── STAP 6B — Verplichtingen deployers ──────────────────────
  {
    id: 'v1_41', step: 's6b',
    text: 'Is getoetst of het AI-systeem is aangeboden met de vereiste technische documentatie en conformiteitsverklaring van de aanbieder?',
    hint: 'Als gebruiker/deployer van een hoog-risico AI-systeem bent u verplicht te controleren of het systeem voldoet aan de AI Act-vereisten. Beantwoord dit ook als u twijfelt over uw rol.',
    type: 'single',
    options: [
      { label: 'Ja — documentatie en CE-markering zijn gecontroleerd', sub: '', value: 'yes' },
      { label: 'Gedeeltelijk', sub: '', value: 'partial' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Niet van toepassing — wij zijn zelf de aanbieder', sub: '', value: 'na' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_42', step: 's6b',
    text: 'Is er een menselijk toezichtsysteem (human oversight) ingericht conform art. 26 lid 1 AI Act?',
    hint: 'De AI Act vereist oprecht menselijk toezicht — geen rubber-stamping. Er moet een gedocumenteerde toezichtsprocedure zijn die automatiebias voorkomt.',
    type: 'single',
    options: [
      { label: 'Ja — gedocumenteerde toezichtsprocedure aanwezig', sub: '', value: 'yes' },
      { label: 'Gedeeltelijk — toezicht bestaat maar is niet gedocumenteerd of niet sluitend', sub: '', value: 'partial' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Niet van toepassing', sub: '', value: 'na' }
    ]
  },
  {
    id: 'v1_43', step: 's6b',
    text: 'Worden betrokkenen (personen op wie het systeem wordt toegepast) geïnformeerd dat zij zijn blootgesteld aan een hoog-risico AI-systeem?',
    hint: '(Art. 26 lid 7 AI Act) — Voor zover van toepassing en voor zover dit mogelijk is.',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Niet van toepassing', sub: '', value: 'na' }
    ]
  },
  {
    id: 'v1_44', step: 's6b',
    text: 'Worden de logging-gegevens van het systeem bewaard conform de instructies van de aanbieder en art. 26 lid 6 AI Act?',
    hint: '',
    type: 'single',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Niet van toepassing — wij zijn zelf de aanbieder', sub: '', value: 'na' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_45', step: 's6b',
    text: 'Is er een procedure voor het melden van ernstige incidenten en storingen aan de aanbieder of aan de bevoegde autoriteit?',
    hint: '(Art. 26 lid 5 AI Act)',
    type: 'single',
    options: [
      { label: 'Ja — incidentprocedure is gedocumenteerd', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'In ontwikkeling', sub: '', value: 'development' }
    ]
  },
  // ── STAP 7 — GPAI ──────────────────────────────────────────
  {
    id: 'v1_46', step: 's7',
    text: 'Wordt in het project gebruik gemaakt van een extern vooraf getraind AI-model dat voor meerdere uiteenlopende taken kan worden ingezet (bijv. een groot taalmodel, een foundation model of een generatief AI-model)?',
    hint: 'Een GPAI-model is een AI-model getraind op grote hoeveelheden data dat voor uiteenlopende doeleinden kan worden gebruikt — zoals grote taalmodellen (LLMs). Als u een dergelijk model gebruikt of aanbiedt, gelden specifieke regels (art. 51–56 AI Act).',
    type: 'single',
    options: [
      { label: 'Ja — een extern GPAI-model wordt direct ingezet (bijv. via een API)', sub: '', value: 'api' },
      { label: 'Ja — een extern GPAI-model wordt verfijnd (fine-tuned) of aangepast voor dit project', sub: '', value: 'finetuned' },
      { label: 'Nee — het model is volledig intern ontwikkeld voor een specifieke taak', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_47', step: 's7',
    text: 'Is de organisatie de aanbieder van het GPAI-model (d.w.z. heeft de organisatie het model zelf getraind en stelt zij het ter beschikking aan anderen)?',
    hint: '',
    type: 'single',
    skipIf: (a) => !usesGPAI(a),
    options: [
      { label: 'Ja — wij zijn de aanbieder van het GPAI-model (wij hebben het getraind)', sub: '', value: 'yes' },
      { label: 'Nee — wij gebruiken een GPAI-model van een derde partij (commerciële API of open-source)', sub: '', value: 'no' }
    ]
  },
  {
    id: 'v1_48', step: 's7',
    text: 'Is er technische documentatie opgesteld over het GPAI-model conform art. 53 en Bijlage XI/XII AI Act?',
    hint: '(Alleen voor aanbieders van GPAI-modellen)',
    type: 'single',
    skipIf: (a) => !usesGPAI(a) || a['v1_47'] !== 'yes',
    options: [
      { label: 'Ja', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'In ontwikkeling', sub: '', value: 'development' }
    ]
  },
  {
    id: 'v1_49', step: 's7',
    text: 'Zijn de auteursrechtelijke vereisten (transparantie over trainingsdata) nageleefd conform art. 53 lid 1 sub d AI Act?',
    hint: 'Er dient een samenvatting te worden gepubliceerd van de gebruikte trainingsdata. Opt-outs van rechthebbenden dienen te worden gerespecteerd.',
    type: 'single',
    skipIf: (a) => !usesGPAI(a) || a['v1_47'] !== 'yes',
    options: [
      { label: 'Ja — samenvatting van trainingsdata is gepubliceerd', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'In ontwikkeling', sub: '', value: 'development' }
    ]
  },
  {
    id: 'v1_50', step: 's7',
    text: 'Is het GPAI-model getraind met een rekenkracht van meer dan 10²⁵ FLOP, of is het door de Europese Commissie aangewezen als model met systemisch risico?',
    hint: 'Modellen boven deze drempel hebben aanvullende verplichtingen (art. 55 AI Act): adversarial testing, incidentmelding, cyberbeveiliging.',
    type: 'single',
    skipIf: (a) => !usesGPAI(a) || a['v1_47'] !== 'yes',
    options: [
      { label: 'Ja — rekenkracht overschrijdt de drempel of het model is aangewezen', sub: '', value: 'yes' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_51', step: 's7',
    text: 'Welk GPAI-model van een derde partij wordt gebruikt, en is gecontroleerd of de aanbieder van dat model de GPAI-verplichtingen naleeft?',
    hint: 'Bijv. GPT-4, Claude, Gemini, Llama, Mistral, etc.',
    type: 'free',
    skipIf: (a) => !usesGPAI(a) || a['v1_47'] === 'yes',
    freeLabel: 'Naam en versie van het gebruikte GPAI-model (bijv. "GPT-4o via OpenAI API")'
  },
  {
    id: 'v1_52', step: 's7',
    text: 'Worden de gebruiksvoorwaarden en technische beperkingen van het GPAI-model van de aanbieder nageleefd bij de inzet in dit project?',
    hint: '',
    type: 'single',
    skipIf: (a) => !usesGPAI(a) || a['v1_47'] === 'yes',
    options: [
      { label: 'Ja — de gebruiksvoorwaarden zijn gecontroleerd en worden nageleefd', sub: '', value: 'yes' },
      { label: 'Gedeeltelijk', sub: '', value: 'partial' },
      { label: 'Nee', sub: '', value: 'no' },
      { label: 'Weet ik niet', sub: '', value: 'unknown' }
    ]
  },
  {
    id: 'v1_53', step: 's7',
    text: 'Integreert de organisatie het GPAI-model in een eigen product of dienst die vervolgens aan derden wordt aangeboden?',
    hint: 'Bij "ja" kwalificeert de organisatie als downstream aanbieder en draagt zij verantwoordelijkheid voor naleving van de AI Act voor het eigen product (art. 54 AI Act).',
    type: 'single',
    skipIf: (a) => !usesGPAI(a) || a['v1_47'] === 'yes',
    options: [
      { label: 'Ja — het GPAI-model is verwerkt in een eigen product of dienst voor externe afnemers', sub: '', value: 'yes' },
      { label: 'Nee — het GPAI-model wordt uitsluitend intern ingezet', sub: '', value: 'no' }
    ]
  }
];

// ═══════════════════════════════════════════════════════════════
// HELPER LOGIC FUNCTIONS
// These are used ONLY for skipIf (minor role-based skips) and
// for computing report outcomes. They do NOT terminate the scan.
// ═══════════════════════════════════════════════════════════════

function isAISystem(a) {
  if (a['v1_1'] === 'rules') return false;
  if (a['v1_2'] === 'no') return false;
  if (a['v1_3'] === 'internal') return false;
  return true;
}

function isTerritoriallyInScope(a) {
  if (a['v1_4'] === 'eu') return true;
  if (a['v1_4'] === 'outside_eu_effect') return true;
  if (a['v1_4'] === 'unknown') return true;
  return false;
}

function hasException(a) {
  if (a['v1_7'] === 'yes') return true;
  if (a['v1_8'] === 'yes') return true;
  if (a['v1_9'] === 'yes') return true;
  return false;
}

function hasForbiddenPractice(a) {
  return ['v1_10','v1_11','v1_12','v1_13','v1_14','v1_15','v1_16'].some(k => a[k] === 'yes');
}

function isHighRiskProduct(a) {
  const vals = Array.isArray(a['v1_17']) ? a['v1_17'] : [a['v1_17']];
  return vals.some(v => ['mdr','machinery','vehicle'].includes(v));
}

function isHighRiskBijlageIII(a) {
  return ['v1_18','v1_19','v1_20','v1_21','v1_22','v1_23','v1_24','v1_25'].some(k => a[k] && a[k] !== 'no');
}

function isHighRisk(a) {
  if (isHighRiskProduct(a)) return true;
  if (isHighRiskBijlageIII(a) && a['v1_26'] !== 'yes') return true;
  return false;
}

function isProvider(a) {
  return a['v1_30'] === 'provider_internal' || a['v1_30'] === 'provider_external';
}

function isProviderOrModifier(a) {
  return isProvider(a) || a['v1_33'] === 'yes';
}

function isDeployer(a) {
  return a['v1_31'] === 'yes' && !isProvider(a);
}

function usesGPAI(a) {
  return a['v1_46'] === 'api' || a['v1_46'] === 'finetuned';
}

// ═══════════════════════════════════════════════════════════════
// STATE
// ═══════════════════════════════════════════════════════════════

let currentQ  = 0;
let answers   = {};
let history   = [];
let userInfo  = { name: '', org: '', email: '' };

// ═══════════════════════════════════════════════════════════════
// SCREEN / NAVIGATION
// ═══════════════════════════════════════════════════════════════

function startScan() {
  currentQ = 0; answers = {}; history = [];
  showScreen('screen-question');
  renderQuestion();
}

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0, 0);
}

// Returns the subset of questions that should be shown given current answers.
// skipIf is ONLY used for minor role-based conditional questions (6B/GPAI sub-questions).
// ALL main questions are always shown.
function getVisibleQuestions() {
  return questions.filter(q => !q.skipIf || !q.skipIf(answers));
}

function renderQuestion() {
  const visible = getVisibleQuestions();
  if (currentQ >= visible.length) { showEmailGate(); return; }

  const q = visible[currentQ];
  const pct = (currentQ / visible.length) * 100;

  document.getElementById('progress-fill').style.width = pct + '%';
  document.getElementById('progress-label').textContent = `${currentQ + 1} / ${visible.length}`;
  document.getElementById('q-step').textContent = STEPS[q.step] || '';
  document.getElementById('q-text').textContent = q.text;

  const hintEl = document.getElementById('q-hint');
  if (q.hint) { hintEl.textContent = q.hint; hintEl.style.display = 'block'; }
  else { hintEl.style.display = 'none'; }

  document.getElementById('skip-notice').style.display = 'none';
  document.getElementById('btn-back').style.visibility = currentQ === 0 ? 'hidden' : 'visible';

  const optionsWrap  = document.getElementById('options-wrap');
  const freeWrap     = document.getElementById('free-text-wrap');
  const optionsList  = document.getElementById('options-list');
  const freeInput    = document.getElementById('free-text-input');

  if (q.type === 'free') {
    optionsWrap.style.display = 'none';
    freeWrap.style.display = 'block';
    freeInput.value = answers[q.id] || '';
    freeInput.oninput = () => updateNextBtn();
  } else {
    optionsWrap.style.display = 'block';
    freeWrap.style.display = 'none';
    optionsList.innerHTML = '';

    const isMulti = q.type === 'multi';
    const currentVal = answers[q.id];

    q.options.forEach((opt) => {
      let isSelected = false;
      if (isMulti) {
        const sel = Array.isArray(currentVal) ? currentVal : [];
        isSelected = sel.includes(opt.value);
      } else {
        isSelected = currentVal === opt.value;
      }

      const btn = document.createElement('button');
      btn.className = 'option-btn' + (isMulti ? ' checkbox' : '') + (isSelected ? ' selected' : '');
      btn.innerHTML = `
        <div class="option-indicator"></div>
        <div>
          <div class="option-text">${opt.label}</div>
          ${opt.sub ? `<div class="option-sub">${opt.sub}</div>` : ''}
        </div>
      `;
      btn.onclick = () => selectOption(opt.value, isMulti, q);
      optionsList.appendChild(btn);
    });
  }

  updateNextBtn();
}

function selectOption(value, isMulti, q) {
  if (isMulti) {
    let selected = Array.isArray(answers[q.id]) ? [...answers[q.id]] : [];
    // 'no' and 'unknown' are exclusive
    if (value === 'no' || value === 'unknown') {
      selected = [value];
    } else {
      selected = selected.filter(v => v !== 'no' && v !== 'unknown');
      if (selected.includes(value)) {
        selected = selected.filter(v => v !== value);
      } else {
        selected.push(value);
      }
    }
    answers[q.id] = selected;
    document.querySelectorAll('.option-btn').forEach((btn, i) => {
      btn.classList.toggle('selected', selected.includes(q.options[i].value));
    });
    updateNextBtn();
  } else {
    answers[q.id] = value;
    document.querySelectorAll('.option-btn').forEach((btn, i) => {
      btn.classList.toggle('selected', q.options[i].value === value);
    });
    updateNextBtn();
    // Auto-advance for single select after short delay
    setTimeout(() => {
      if (currentQ < getVisibleQuestions().length - 1) nextQuestion();
      else showEmailGate();
    }, 280);
  }
}

function updateNextBtn() {
  const visible = getVisibleQuestions();
  const q = visible[currentQ];
  if (!q) return;
  const btn = document.getElementById('btn-next');
  let hasAnswer = false;

  if (q.type === 'free') {
    hasAnswer = (document.getElementById('free-text-input').value || '').trim().length > 0;
  } else if (q.type === 'multi') {
    const sel = answers[q.id];
    hasAnswer = Array.isArray(sel) && sel.length > 0;
  } else {
    hasAnswer = answers[q.id] !== undefined && answers[q.id] !== null;
  }

  btn.classList.toggle('enabled', hasAnswer);
  btn.textContent = currentQ === visible.length - 1 ? 'Bekijk resultaten →' : 'Volgende →';
}

function nextQuestion() {
  const visible = getVisibleQuestions();
  const q = visible[currentQ];
  if (!q) return;

  if (q.type === 'free') {
    const val = (document.getElementById('free-text-input').value || '').trim();
    if (!val) return;
    answers[q.id] = val;
  } else if (q.type === 'multi') {
    const sel = answers[q.id];
    if (!Array.isArray(sel) || sel.length === 0) return;
  } else {
    if (!answers[q.id]) return;
  }

  history.push(currentQ);

  if (currentQ < visible.length - 1) {
    currentQ++;
    renderQuestion();
  } else {
    showEmailGate();
  }
}

function prevQuestion() {
  if (history.length > 0) {
    currentQ = history.pop();
    renderQuestion();
  }
}

// ═══════════════════════════════════════════════════════════════
// EMAIL GATE
// ═══════════════════════════════════════════════════════════════

function showEmailGate() {
  showScreen('screen-email');
  document.getElementById('user-name').focus();
}

function clearEmailError() {
  document.getElementById('email-error').classList.remove('visible');
  document.getElementById('user-email').classList.remove('error');
}

function isValidEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

function submitEmail() {
  const emailInput = document.getElementById('user-email');
  const email = emailInput.value.trim();

  if (!isValidEmail(email)) {
    emailInput.classList.add('error');
    document.getElementById('email-error').classList.add('visible');
    return;
  }

  userInfo.name  = document.getElementById('user-name').value.trim()  || 'Niet opgegeven';
  userInfo.org   = document.getElementById('user-org').value.trim()   || 'Niet opgegeven';
  userInfo.email = email;

  const btn = document.getElementById('btn-submit-email');
  btn.disabled = true;
  btn.textContent = 'Verzenden…';

  const reportText = buildReportText();

  sendEmails(email, reportText)
    .then(() => {
      btn.textContent = 'Rapport verzonden ✓';
      setTimeout(() => showResults(), 700);
    })
    .catch(() => {
      // Show results even if email fails
      showResults();
    });
}

// ═══════════════════════════════════════════════════════════════
// REPORT BUILDER
// ═══════════════════════════════════════════════════════════════

function buildReportText() {
  const visible = getVisibleQuestions();
  const lines = [];

  lines.push('=== EU AI ACT COMPLIANCE QUICKSCAN — RAPPORTAGE ===');
  lines.push('');
  lines.push('Naam:         ' + userInfo.name);
  lines.push('Organisatie:  ' + userInfo.org);
  lines.push('E-mail:       ' + userInfo.email);
  lines.push('Datum:        ' + new Date().toLocaleDateString('nl-NL', { day: '2-digit', month: 'long', year: 'numeric' }));
  lines.push('');
  lines.push('─'.repeat(60));
  lines.push('KERNUITKOMSTEN');
  lines.push('─'.repeat(60));

  const outcomes = computeOutcomes();
  lines.push('Kwalificeert als AI-systeem:  ' + outcomes.isAI);
  lines.push('Territoriale scope AI Act:    ' + outcomes.territorial);
  lines.push('Uitzondering van toepassing:  ' + outcomes.exception);
  lines.push('Verboden toepassing (art. 5): ' + outcomes.forbidden);
  lines.push('Risicoklasse:                 ' + outcomes.riskClass);
  lines.push('Rol in de AI-keten:           ' + outcomes.role);
  lines.push('GPAI-model in gebruik:        ' + outcomes.gpai);
  lines.push('');
  lines.push('─'.repeat(60));
  lines.push('BEVINDINGEN & AANDACHTSPUNTEN');
  lines.push('─'.repeat(60));

  const findings = computeFindings();
  if (findings.length === 0) {
    lines.push('Geen specifieke bevindingen.');
  } else {
    findings.forEach((f, i) => {
      lines.push('');
      const levelLabel = f.level === 'blocker' ? '🔴 DIRECTE ACTIE VEREIST' : f.level === 'warning' ? '🟡 ACTIE VEREIST' : f.level === 'info' ? '🟢 TER INFORMATIE' : 'ℹ️  AANDACHTSPUNT';
      lines.push(`${i+1}. [${levelLabel}] ${f.name}`);
      lines.push(f.desc);
      if (f.regs) lines.push('   Grondslag: ' + f.regs);
    });
  }

  lines.push('');
  lines.push('─'.repeat(60));
  lines.push('ANTWOORDEN PER STAP');
  lines.push('─'.repeat(60));

  let lastStep = '';
  visible.forEach(q => {
    if (q.step !== lastStep) {
      lines.push('');
      lines.push('[ ' + (STEPS[q.step] || q.step).toUpperCase() + ' ]');
      lastStep = q.step;
    }
    const ans = answers[q.id];
    let ansStr = '— Niet beantwoord';
    if (ans !== undefined && ans !== null && ans !== '') {
      if (Array.isArray(ans)) {
        ansStr = ans.map(v => {
          const opt = q.options ? q.options.find(o => o.value === v) : null;
          return opt ? opt.label : v;
        }).join(' | ');
      } else if (q.options) {
        const opt = q.options.find(o => o.value === ans);
        ansStr = opt ? opt.label : ans;
      } else {
        ansStr = ans;
      }
    }
    lines.push('V: ' + q.text);
    lines.push('A: ' + ansStr);
  });

  lines.push('');
  lines.push('─'.repeat(60));
  lines.push('DISCLAIMER');
  lines.push('─'.repeat(60));
  lines.push('Deze quickscan biedt een algemeen compliance-overzicht op basis van de verstrekte');
  lines.push('antwoorden en vormt geen juridisch advies. Regelgevingsverplichtingen hangen af');
  lines.push('van uw specifieke context, jurisdictie en systeemontwerp. Raadpleeg voor specifiek');
  lines.push('juridisch advies een gekwalificeerde jurist gespecialiseerd in EU digitale regelgeving.');
  lines.push('');
  lines.push('bg.legal | info@bg.legal');

  return lines.join('\n');
}

async function sendEmails(userEmail, reportText) {
  const BG_EMAIL = 'info@bg.legal';
  const dateStr  = new Date().toLocaleDateString('nl-NL', { day: '2-digit', month: 'long', year: 'numeric' });

  // ── Email 1: to the user ────────────────────────────────────
  const fd1 = new FormData();
  fd1.append('_to',       userEmail);
  fd1.append('_replyto',  BG_EMAIL);
  fd1.append('_subject',  'Uw EU AI Act Compliance Rapport — bg.legal');
  fd1.append('_template', 'box');
  fd1.append('Naam',         userInfo.name);
  fd1.append('Organisatie',  userInfo.org);
  fd1.append('Datum',        dateStr);
  fd1.append('Volledig rapport', reportText);

  // ── Email 2: copy to bg.legal ───────────────────────────────
  const fd2 = new FormData();
  fd2.append('_to',       BG_EMAIL);
  fd2.append('_replyto',  userEmail);
  fd2.append('_subject',  'AI Compliance Scan — Inzending van ' + userInfo.name + ' (' + userInfo.org + ')');
  fd2.append('_template', 'box');
  fd2.append('Naam',           userInfo.name);
  fd2.append('Organisatie',    userInfo.org);
  fd2.append('E-mail gebruiker', userEmail);
  fd2.append('Datum',          dateStr);
  fd2.append('Volledig rapport', reportText);

  // Fire both; don't let one failure block the other
  const [r1, r2] = await Promise.allSettled([
    fetch('https://formsubmit.co/' + userEmail, { method: 'POST', body: fd1, headers: { Accept: 'application/json' } }),
    fetch('https://formsubmit.co/' + BG_EMAIL,  { method: 'POST', body: fd2, headers: { Accept: 'application/json' } })
  ]);

  // Resolve as long as at least one succeeded (don't throw for the bg copy failure)
  return [r1, r2];
}

// ═══════════════════════════════════════════════════════════════
// OUTCOME & FINDINGS COMPUTATION
// ═══════════════════════════════════════════════════════════════

function computeOutcomes() {
  const a = answers;
  return {
    isAI:        !isAISystem(a)            ? 'Nee — systeem kwalificeert waarschijnlijk niet als AI-systeem (art. 3 lid 1)'
                                           : 'Ja — kwalificeert als AI-systeem (art. 3 lid 1 AI Act)',
    territorial: !isTerritoriallyInScope(a) ? 'Mogelijk buiten territoriale scope AI Act (art. 2)'
                                           : 'AI Act is territoriaal van toepassing (art. 2)',
    exception:   a['v1_7']==='yes'         ? 'Ja — militair/nationale veiligheid (art. 2 lid 3)'
               : a['v1_8']==='yes'         ? 'Ja — wetenschappelijk onderzoek (art. 2 lid 6)'
               : a['v1_9']==='yes'         ? 'Ja — persoonlijk gebruik (art. 2 lid 6)'
                                           : 'Nee — geen uitzondering van toepassing',
    forbidden:   hasForbiddenPractice(a)   ? '🔴 MOGELIJK VERBODEN TOEPASSING GEÏDENTIFICEERD (art. 5 AI Act)'
                                           : 'Geen verboden toepassing geïdentificeerd',
    riskClass:   isHighRisk(a)             ? '🔴 Hoog risico (art. 6 AI Act)'
               : (a['v1_27']==='yes'||a['v1_28']==='yes'||a['v1_29']==='yes')
                                           ? '🟡 Beperkt risico — transparantieverplichtingen (art. 50)'
                                           : '🟢 Minimaal risico',
    role:        isProviderOrModifier(a)   ? 'Aanbieder (provider) — art. 16–25 AI Act'
               : isDeployer(a)             ? 'Gebruiker/deployer — art. 26 AI Act'
               : a['v1_32']==='importer'   ? 'Importeur — art. 23 AI Act'
               : a['v1_32']==='distributor'? 'Distributeur — art. 24 AI Act'
                                           : 'Niet eenduidig bepaald',
    gpai:        !usesGPAI(a)             ? 'Nee'
               : a['v1_47']==='yes'       ? 'Ja — organisatie is GPAI-aanbieder (art. 53 AI Act)'
                                          : 'Ja — gebruik van derde-partij GPAI-model'
  };
}

function computeFindings() {
  const a = answers;
  const findings = [];

  // ── 1. AI-systeemkwalificatie ──────────────────────────────
  if (!isAISystem(a)) {
    findings.push({ level: 'info', name: 'Systeem kwalificeert waarschijnlijk niet als AI-systeem (art. 3 lid 1 AI Act)', desc: 'Op basis van uw antwoorden volgt het systeem vaste geprogrammeerde regels zonder leren of redeneren, genereert het geen eigen outputs, of zijn de outputs uitsluitend interne technische tussenstappen. Klassieke regelgestuurde software en pure data-opslagsystemen vallen buiten de definitie van de AI Act. Indien de architectuur van het systeem wijzigt richting ML of generatieve AI, dient de kwalificatie opnieuw te worden getoetst.', regs: 'Art. 3 lid 1 AI Act' });
  }

  // ── 2. Territoriale scope ──────────────────────────────────
  if (!isTerritoriallyInScope(a)) {
    findings.push({ level: 'note', name: 'Systeem mogelijk buiten territoriale scope AI Act (art. 2)', desc: 'Het systeem wordt uitsluitend buiten de EU ingezet en de output heeft geen effect op personen of entiteiten in de EU. De AI Act lijkt territoriaal niet van toepassing. Indien de inzetomgeving of het gebruikerspubliek wijzigt en EU-personen worden bereikt, dient de territoriale scope opnieuw te worden beoordeeld.', regs: 'Art. 2 AI Act' });
  }

  // ── 3. Uitzonderingen ──────────────────────────────────────
  if (a['v1_7'] === 'yes') findings.push({ level: 'note', name: 'Uitzondering van toepassing: militair/nationale veiligheid (art. 2 lid 3)', desc: 'Het systeem is uitsluitend bestemd voor militaire doeleinden of nationale veiligheid en is daarmee expliciet uitgezonderd van de AI Act. Let op: indien het systeem ook voor andere doeleinden wordt ingezet, vervalt de uitzondering gedeeltelijk en dient opnieuw te worden getoetst.', regs: 'Art. 2 lid 3 AI Act' });
  if (a['v1_8'] === 'yes') findings.push({ level: 'note', name: 'Uitzondering van toepassing: pre-commercieel onderzoek (art. 2 lid 6)', desc: 'Het systeem bevindt zich uitsluitend in de pre-commerciële onderzoeks- en ontwikkelingsfase. Zodra het systeem op de markt wordt gebracht of in gebruik wordt gesteld buiten de onderzoekscontext, vervalt de uitzondering en gelden alle relevante AI Act-verplichtingen.', regs: 'Art. 2 lid 6 AI Act' });
  if (a['v1_9'] === 'yes') findings.push({ level: 'note', name: 'Uitzondering van toepassing: persoonlijk gebruik (art. 2 lid 6)', desc: 'Het systeem is uitsluitend bestemd voor persoonlijk, niet-professioneel gebruik door particulieren. Zodra het systeem professioneel of commercieel wordt ingezet, vervalt de uitzondering.', regs: 'Art. 2 lid 6 AI Act' });

  // ── 4. Verboden toepassingen ───────────────────────────────
  const forbiddenMap = {
    v1_10: { art: 'art. 5 lid 1 sub a', label: 'Subliminale of manipulatieve technieken' },
    v1_11: { art: 'art. 5 lid 1 sub b', label: 'Misbruik van kwetsbaarheden van specifieke groepen' },
    v1_12: { art: 'art. 5 lid 1 sub c', label: 'Social scoring door overheidsinstanties' },
    v1_13: { art: 'art. 5 lid 1 sub d', label: 'Real-time biometrische identificatie voor rechtshandhaving' },
    v1_14: { art: 'art. 5 lid 1 sub f', label: 'Emotieherkenning op werkplek of in onderwijs' },
    v1_15: { art: 'art. 5 lid 1 sub g', label: 'Biometrische categorisering op gevoelige kenmerken' },
    v1_16: { art: 'art. 5 lid 1 sub e', label: 'Scraping van gezichtsafbeeldingen voor databases' }
  };
  Object.entries(forbiddenMap).forEach(([key, info]) => {
    if (a[key] === 'yes') {
      findings.push({ level: 'blocker', name: `🔴 MOGELIJK VERBODEN TOEPASSING — ${info.label}`, desc: `Uw antwoord geeft aan dat het systeem mogelijk een verboden toepassing betreft (${info.art} AI Act). Verboden toepassingen mogen niet worden ontwikkeld, op de markt gebracht of ingezet — ook niet in een testomgeving. Directe juridische analyse is vereist vóór enige verdere ontwikkeling of inzet. Er zijn geen uitzonderingen, tenzij expliciet vermeld in art. 5 AI Act.`, regs: `Art. 5 AI Act — ${info.art}` });
    }
  });

  // ── 5A. Hoog risico productregelgeving ─────────────────────
  if (isHighRiskProduct(a)) {
    const vals = Array.isArray(a['v1_17']) ? a['v1_17'] : [a['v1_17']];
    const labels = { mdr: 'medisch hulpmiddel (MDR/IVDR)', machinery: 'machine/speelgoed/lift/explosief/radioapparatuur', vehicle: 'voertuig/luchtvaartcomponent' };
    const productTypes = vals.filter(v => labels[v]).map(v => labels[v]).join(', ');
    findings.push({ level: 'blocker', name: `Hoog-risico AI-systeem — veiligheidscomponent in gereguleerd product (art. 6 lid 1)`, desc: `Het systeem is een veiligheidscomponent van een product dat valt onder EU-productregelgeving (${productTypes}). Dit leidt automatisch tot hoog-risicoklasse. Alle verplichtingen van Hoofdstuk III AI Act zijn van toepassing: kwaliteitsbeheersysteem (art. 9/17), datagovernance (art. 10), technische documentatie (art. 11/Bijlage IV), logging (art. 12), transparantie (art. 13), menselijk toezicht (art. 14), nauwkeurigheid & robuustheid (art. 15), conformiteitsbeoordeling (art. 43) en EU-databankregistratie (art. 49). Voor medische hulpmiddelen gelden parallel de MDR/IVDR-verplichtingen.`, regs: `Art. 6 lid 1 AI Act; Bijlage I AI Act${vals.includes('mdr') ? '; MDR EU 2017/745; IVDR EU 2017/746' : ''}` });
  }

  // ── 5B. Hoog risico Bijlage III ────────────────────────────
  const bijlageIIIMap = {
    v1_18: { label: 'Biometrische identificatie of categorisering van personen', regs: 'Bijlage III nr. 1 AI Act' },
    v1_19: { label: 'Beheer en werking van kritieke infrastructuur', regs: 'Bijlage III nr. 2 AI Act' },
    v1_20: { label: 'Onderwijs en beroepsopleiding', regs: 'Bijlage III nr. 3 AI Act' },
    v1_21: { label: 'Werkgelegenheid, personeelsmanagement en toegang tot zelfstandig werk', regs: 'Bijlage III nr. 4 AI Act' },
    v1_22: { label: 'Toegang tot essentiële private of publieke diensten', regs: 'Bijlage III nr. 5 AI Act' },
    v1_23: { label: 'Rechtshandhaving', regs: 'Bijlage III nr. 6 AI Act' },
    v1_24: { label: 'Migratie, asiel en grensbeheer', regs: 'Bijlage III nr. 7 AI Act' },
    v1_25: { label: 'Rechtsbedeling en democratische processen', regs: 'Bijlage III nr. 8 AI Act' }
  };

  if (isHighRiskBijlageIII(a)) {
    if (a['v1_26'] === 'yes') {
      // Exception claimed
      findings.push({ level: 'warning', name: 'Art. 6 lid 3-uitzondering ingeroepen — documenteer intern en robuust', desc: 'Hoewel het systeem in een Bijlage III-toepassingsgebied valt, heeft u aangegeven dat het uitsluitend voorbereidende taken uitvoert zonder directe invloed op de beslissing. De uitzondering van art. 6 lid 3 AI Act kan van toepassing zijn. Dit dient intern schriftelijk en onderbouwd te worden vastgelegd. Indien een toezichthoudende autoriteit of rechtbank dit betwist, is de hoog-risicoclassificatie alsnog van toepassing met alle bijbehorende verplichtingen. Aanbeveling: laat de documentatie extern toetsen.', regs: 'Art. 6 lid 3 AI Act' });
    } else {
      // Full high risk for each Bijlage III category found
      Object.entries(bijlageIIIMap).forEach(([key, info]) => {
        if (a[key] && a[key] !== 'no') {
          findings.push({ level: 'blocker', name: `Hoog-risico AI-systeem — ${info.label}`, desc: `Het systeem valt in de hoog-risico categorie van ${info.regs}. Alle verplichtingen van Hoofdstuk III AI Act zijn van toepassing: kwaliteitsbeheersysteem (art. 9), datagovernance (art. 10), technische documentatie (art. 11 + Bijlage IV), automatische logging (art. 12), transparantie & instructies voor gebruikers (art. 13), menselijk toezicht (art. 14), nauwkeurigheid, robuustheid & cyberbeveiliging (art. 15), conformiteitsbeoordeling (art. 43), EU-databankregistratie (art. 49) en post-market monitoring (art. 72).`, regs: `Art. 6 lid 2 jo. ${info.regs}` });
        }
      });
    }
  }

  // ── 5D. Beperkt risico / transparantie ─────────────────────
  if (a['v1_27'] === 'yes') findings.push({ level: 'warning', name: 'Transparantieverplichting — chatbot of virtuele assistent (art. 50 lid 1)', desc: 'Het systeem interageert rechtstreeks met mensen via een geautomatiseerde interface. Gebruikers moeten actief worden geïnformeerd dat zij met een AI-systeem communiceren, tenzij dit reeds ondubbelzinnig duidelijk is uit de context. De melding dient duidelijk, tijdig en op begrijpelijke wijze te worden gedaan. Niet-naleving: boete tot €15M of 3% van de wereldwijde jaaromzet.', regs: 'Art. 50 lid 1 AI Act' });
  if (a['v1_28'] === 'yes') findings.push({ level: 'warning', name: 'Transparantieverplichting — AI-gegenereerde of synthetische content (art. 50 lid 2)', desc: 'Het systeem genereert AI-content (tekst, audio, video, afbeeldingen) die als echt kan worden beschouwd. Dergelijke content dient machineleesbaar te worden gelabeld. Uitzonderingen bestaan voor artistieke of creatieve toepassingen, mits adequate openbaarmaking plaatsvindt. Distributeurs van synthetische media hebben eveneens verplichtingen.', regs: 'Art. 50 lid 2 AI Act' });
  if (a['v1_29'] === 'yes') findings.push({ level: 'warning', name: 'Transparantieverplichting — emotieherkenning of biometrische categorisering (art. 50 lid 3)', desc: 'Personen die zijn blootgesteld aan emotieherkenning of biometrische categorisering dienen hierover te worden geïnformeerd. Combineer dit met een GDPR-analyse voor verwerking van bijzondere categorieën persoonsgegevens (art. 9 GDPR).', regs: 'Art. 50 lid 3 AI Act' });

  // ── 6A. Aanbiederverplichtingen ────────────────────────────
  if (isHighRisk(a) && isProviderOrModifier(a)) {
    const providerChecks = [
      { key: 'v1_34', bad: ['partial','no'], name: 'Kwaliteitsbeheersysteem (QMS) ontbreekt of is onvolledig (art. 17)', desc: 'Een gedocumenteerd QMS is verplicht voor aanbieders van hoog-risico AI-systemen. Het QMS moet de gehele levenscyclus bestrijken: ontwerp, data, testing, monitoring en correctieve maatregelen. Zonder QMS kan geen conformiteitsbeoordeling worden afgerond en mag het systeem niet op de markt worden gebracht.', regs: 'Art. 9, 17 AI Act' },
      { key: 'v1_35', bad: ['partial','no'], name: 'Technische documentatie ontbreekt of is onvolledig (art. 11 + Bijlage IV)', desc: 'Technische documentatie is een absolute voorwaarde voor de conformiteitsbeoordeling en marktintroductie. Bijlage IV AI Act beschrijft de minimuminhoud: systeemdescriptie, ontwerpspecificaties, trainingsdata, testresultaten, prestatiestatistieken en risicobeoordeling. Veelvoorkomende lacunes: ontbrekende testresultaten per demografische groep, onvolledige data-provenance en geen documentatie van menselijk toezicht.', regs: 'Art. 11 AI Act; Bijlage IV AI Act' },
      { key: 'v1_36', bad: ['partial','no'], name: 'Automatische logging (audit trail) ontbreekt of is onvolledig (art. 12)', desc: 'Het systeem moet automatisch relevante gebeurtenissen registreren, inclusief invoerdata, outputgeneratie, uitzonderingen en menselijke interventies. Logging is verplicht om toezicht door bevoegde autoriteiten, incidentonderzoek en post-market monitoring mogelijk te maken.', regs: 'Art. 12 AI Act' },
      { key: 'v1_37', bad: ['partial','no'], name: 'Transparantie-informatie en gebruikersinstructies ontbreken (art. 13)', desc: 'Gebruikers van hoog-risico AI-systemen moeten adequate informatie ontvangen: doeleinden, beperkingen, accuraatheid, bekende vooroordelen, menselijk toezicht en contact voor klachten. Ontbrekende instructies vormen een zelfstandige grond voor non-conformiteit.', regs: 'Art. 13 AI Act' },
      { key: 'v1_38', bad: ['no_required'], name: 'Conformiteitsbeoordelingsprocedure niet doorlopen (art. 43)', desc: 'Het systeem mag niet op de markt worden gebracht of in gebruik worden gesteld zonder afgeronde conformiteitsbeoordelingsprocedure. Voor de meeste hoog-risico systemen is self-assessment toegestaan; voor biometrische systemen en kritieke infrastructuur is een Notified Body verplicht. Na afronding dient een EU-conformiteitsverklaring te worden opgesteld en CE-markering te worden aangebracht.', regs: 'Art. 43, 47, 48, 49 AI Act' },
      { key: 'v1_39', bad: ['no_required','unknown'], name: 'Registratie in EU-databank voor hoog-risico AI-systemen ontbreekt (art. 49)', desc: 'Registratie in de EU-databank (ai.europa.eu) is verplicht vóór marktintroductie. Niet-registratie is een ernstige schending van de AI Act. Boete: tot €15M of 3% van de wereldwijde jaaromzet. Voor medische AI geldt parallel een EUDAMED-registratieplicht.', regs: 'Art. 49 AI Act; Art. 33 MDR (parallel)' },
      { key: 'v1_40', bad: ['no'], name: 'Post-market monitoringplan ontbreekt (art. 72)', desc: 'Post-market monitoring is een wettelijke verplichting voor aanbieders van hoog-risico AI-systemen — niet een best practice. Het monitoringplan moet vóór go-live gedocumenteerd zijn en dient te omvatten: KPI\'s voor systeemprestaties, driftdetectie, biasmonitoring, incidentlogboek en periodieke reviewcycli. Het systeem mag niet in productie gaan zonder dit plan.', regs: 'Art. 72 AI Act' }
    ];
    providerChecks.forEach(c => {
      if (c.bad.includes(a[c.key])) {
        findings.push({ level: 'blocker', name: c.name, desc: c.desc, regs: c.regs });
      }
    });
  }

  // ── 6B. Deployerverplichtingen ─────────────────────────────
  if (isHighRisk(a) && isDeployer(a)) {
    const deployerChecks = [
      { key: 'v1_41', bad: ['partial','no','unknown'], name: 'Documentatie en conformiteit aanbieder niet (volledig) gecontroleerd (art. 26 lid 1)', desc: 'Als deployer bent u verplicht te controleren of het systeem voorzien is van de vereiste technische documentatie en conformiteitsverklaring van de aanbieder, inclusief CE-markering. Inzetten van een niet-conform systeem maakt de deployer medeverantwoordelijk.', regs: 'Art. 26 lid 1 AI Act' },
      { key: 'v1_42', bad: ['partial','no'], name: 'Menselijk toezichtsysteem (human oversight) ontbreekt of is onvolledig (art. 26 lid 1)', desc: 'De AI Act vereist oprecht menselijk toezicht — geen nominale goedkeuring (rubber-stamping). Er moet een gedocumenteerde procedure bestaan waarbij toezichthouders de AI-output kunnen begrijpen, bevragen en overschrijven. Automatiebias — de neiging van mensen om AI-output te volgen — is een compliance-risico dat actief moet worden gemitigeerd.', regs: 'Art. 26 lid 1 AI Act; Art. 14 AI Act' },
      { key: 'v1_44', bad: ['no','unknown'], name: 'Logging-gegevens worden niet bewaard conform de vereisten (art. 26 lid 6)', desc: 'Deployers zijn verplicht de logging-gegevens van het systeem te bewaren conform de instructies van de aanbieder en gedurende de wettelijk vereiste periode. Deze logs zijn essentieel voor toezicht, incidentonderzoek en het aantonen van compliance bij controles.', regs: 'Art. 26 lid 6 AI Act' },
      { key: 'v1_45', bad: ['no'], name: 'Geen procedure voor melding ernstige incidenten (art. 26 lid 5)', desc: 'Deployers zijn verplicht ernstige incidenten en storingen die zij identificeren te melden aan de aanbieder of rechtstreeks aan de bevoegde nationale autoriteit. Een gedocumenteerde incidentprocedure is verplicht.', regs: 'Art. 26 lid 5 AI Act' }
    ];
    deployerChecks.forEach(c => {
      if (c.bad.includes(a[c.key])) {
        findings.push({ level: 'warning', name: c.name, desc: c.desc, regs: c.regs });
      }
    });
  }

  // ── 7. GPAI ────────────────────────────────────────────────
  if (usesGPAI(a)) {
    if (a['v1_47'] === 'yes') {
      // GPAI provider
      if (a['v1_48'] !== 'yes') findings.push({ level: 'blocker', name: 'Technische documentatie GPAI-model ontbreekt (art. 53 AI Act)', desc: 'Aanbieders van GPAI-modellen zijn verplicht technische documentatie op te stellen conform art. 53 en Bijlage XI (voor alle GPAI-modellen) en Bijlage XII (voor GPAI-modellen met systemisch risico). Deze documentatie dient ter beschikking te worden gesteld van de Europese Commissie op verzoek.', regs: 'Art. 53 AI Act; Bijlage XI en XII' });
      if (a['v1_49'] !== 'yes') findings.push({ level: 'blocker', name: 'Transparantie over trainingsdata (auteursrecht) ontbreekt (art. 53 lid 1 sub d)', desc: 'GPAI-aanbieders zijn verplicht een voldoende gedetailleerde samenvatting te publiceren van de voor training gebruikte content, inclusief web-gescrapete bronnen. Opt-outs van rechthebbenden (uitgevers, auteurs) dienen te worden gerespecteerd. Niet-naleving leidt naast AI Act-boetes tot aansprakelijkheid onder het auteursrecht (DSM Directive art. 4).', regs: 'Art. 53 lid 1 sub d AI Act; DSM Directive art. 4' });
      if (a['v1_50'] === 'yes') findings.push({ level: 'blocker', name: '🔴 GPAI-model met systemisch risico — aanvullende verplichtingen (art. 51–55)', desc: 'Het GPAI-model overschrijdt de drempel van 10²⁵ FLOP of is aangewezen als model met systemisch risico. Aanvullende verplichtingen zijn van toepassing: (1) adversarial testing / red-teaming vóór en na marktintroductie, (2) incidentmelding aan de Europese Commissie binnen 3 dagen na bekendwording van ernstige incidenten, (3) cyberbeveiligingsmaatregelen, (4) energieverbruiksrapportage. De Europese Commissie kan aanvullende eisen stellen.', regs: 'Art. 51, 55 AI Act' });
    } else {
      // GPAI user/integrator
      if (a['v1_52'] === 'no' || a['v1_52'] === 'partial') findings.push({ level: 'warning', name: 'Gebruiksvoorwaarden GPAI-model van derde partij niet (volledig) nageleefd', desc: 'Het gebruik van een GPAI-model buiten de gebruiksvoorwaarden van de aanbieder leidt tot contractuele aansprakelijkheid en kan bijdragen aan AI Act-schendingen in de downstream toepassing. Controleer in het bijzonder: toegestane use cases, dataverwerking, reproductierechten en auteursrechtelijke restricties.', regs: 'Art. 54 AI Act' });
      if (a['v1_53'] === 'yes') findings.push({ level: 'warning', name: 'Downstream GPAI-integratie in eigen product — aanbiederverplichtingen van toepassing (art. 54)', desc: 'Door het GPAI-model van een derde partij te integreren in een eigen product voor externe afnemers, kwalificeert de organisatie als downstream aanbieder. De organisatie draagt verantwoordelijkheid voor naleving van de AI Act voor het eigen product, inclusief de verplichtingen die gelden voor de risicoklasse van dat product. Informatieverplichtingen richting de GPAI-aanbieder zijn eveneens van toepassing (art. 54 AI Act).', regs: 'Art. 54 AI Act' });
    }
  }

  // ── Minimaal risico — geen bevindingen ─────────────────────
  if (findings.length === 0) {
    findings.push({ level: 'info', name: '✅ Geen specifieke AI Act-verplichtingen vastgesteld — minimaal risico', desc: 'Op basis van de verstrekte antwoorden valt het systeem in de categorie minimaal risico. Er gelden geen specifieke verplichtingen onder de EU AI Act. Het wordt aanbevolen: (1) vrijwillige gedragscodes toe te passen (art. 95 AI Act), (2) de classificatie periodiek te hertoetsen bij wijzigingen in het systeem of de regelgeving, (3) de GPAI- en transparantievragen te herbeoordelen indien de functionaliteit wordt uitgebreid.', regs: 'Art. 95 AI Act (vrijwillige gedragscodes)' });
  }

  return findings;
}

// ═══════════════════════════════════════════════════════════════
// RESULTS SCREEN
// ═══════════════════════════════════════════════════════════════

function showResults() {
  const a        = answers;
  const outcomes = computeOutcomes();
  const findings = computeFindings();

  // User info line
  document.getElementById('results-user-info').textContent =
    userInfo.name +
    (userInfo.org && userInfo.org !== 'Niet opgegeven' ? ' · ' + userInfo.org : '') +
    ' · ' + new Date().toLocaleDateString('nl-NL', { day: '2-digit', month: 'long', year: 'numeric' });

  // Risk band
  const rb = document.getElementById('risk-band');
  const hasBlocker  = findings.some(f => f.level === 'blocker');
  const hasWarning  = findings.some(f => f.level === 'warning');

  if (hasForbiddenPractice(a)) {
    rb.className = 'risk-band blocked';
    rb.textContent = '🔴 Mogelijk verboden toepassing — directe actie vereist';
  } else if (isHighRisk(a) || hasBlocker) {
    rb.className = 'risk-band high';
    rb.textContent = '⬤ Hoog risico — aanzienlijke verplichtingen van toepassing';
  } else if (hasWarning || a['v1_27']==='yes' || a['v1_28']==='yes' || a['v1_29']==='yes') {
    rb.className = 'risk-band medium';
    rb.textContent = '◑ Beperkt risico — transparantieverplichtingen van toepassing';
  } else if (!isAISystem(a) || !isTerritoriallyInScope(a) || hasException(a)) {
    rb.className = 'risk-band low';
    rb.textContent = '○ Buiten scope of uitzondering van toepassing';
  } else {
    rb.className = 'risk-band low';
    rb.textContent = '○ Minimaal risico — geen specifieke AI Act-verplichtingen';
  }

  // Summary cards
  const grid = document.getElementById('summary-grid');
  grid.innerHTML = '';
  [
    { label: 'AI-systeem', value: isAISystem(a) ? 'Ja' : 'Nee / onduidelijk', cls: isAISystem(a) ? '' : 'low' },
    { label: 'Risicoklasse', value: isHighRisk(a) ? 'Hoog risico' : (a['v1_27']==='yes'||a['v1_28']==='yes'||a['v1_29']==='yes') ? 'Beperkt risico' : 'Minimaal risico', cls: isHighRisk(a) ? 'high' : (a['v1_27']==='yes'||a['v1_28']==='yes'||a['v1_29']==='yes') ? 'medium' : 'low' },
    { label: 'Verboden toepassing', value: hasForbiddenPractice(a) ? 'Mogelijk aanwezig' : 'Niet geïdentificeerd', cls: hasForbiddenPractice(a) ? 'blocked' : 'low' },
    { label: 'Rol in AI-keten', value: isProviderOrModifier(a) ? 'Aanbieder' : isDeployer(a) ? 'Gebruiker/deployer' : a['v1_32']==='importer' ? 'Importeur' : a['v1_32']==='distributor' ? 'Distributeur' : 'Onduidelijk', cls: '' },
    { label: 'GPAI-model', value: !usesGPAI(a) ? 'Niet in gebruik' : a['v1_47']==='yes' ? 'Aanbieder' : 'Gebruiker van extern model', cls: '' },
    { label: 'Bevindingen', value: findings.filter(f=>f.level==='blocker').length + ' directe actiepunten · ' + findings.filter(f=>f.level==='warning').length + ' waarschuwingen', cls: hasBlocker ? 'high' : hasWarning ? 'medium' : 'low' }
  ].forEach(c => {
    const card = document.createElement('div');
    card.className = 'summary-card';
    card.innerHTML = `<div class="summary-card-label">${c.label}</div><div class="summary-card-value ${c.cls}">${c.value}</div>`;
    grid.appendChild(card);
  });

  // Findings
  const list = document.getElementById('priority-list');
  list.innerHTML = '';
  const iconMap = { blocker: '🔴', warning: '🟡', info: '🟢', note: 'ℹ️' };

  findings.forEach(f => {
    const card = document.createElement('div');
    card.className = 'priority-card' + (f.level === 'blocker' ? ' blocker' : f.level === 'warning' ? ' warning' : '');
    card.innerHTML = `
      <div class="priority-icon ${f.level}">${iconMap[f.level] || '•'}</div>
      <div class="priority-body">
        <div class="priority-name">${f.name}</div>
        <div class="priority-desc">${f.desc}</div>
        ${f.regs ? `<div class="priority-regs">${f.regs}</div>` : ''}
        <div class="priority-level ${f.level}">${
          f.level === 'blocker' ? '● Directe actie vereist'
        : f.level === 'warning' ? '◑ Actie vereist vóór go-live'
        : f.level === 'info'    ? '○ Ter informatie'
                                : 'ℹ Aandachtspunt'
        }</div>
      </div>
    `;
    list.appendChild(card);
  });

  // Answers table
  const tbody = document.getElementById('answers-body');
  tbody.innerHTML = '';
  let lastStep = '';

  getVisibleQuestions().forEach(q => {
    if (q.step !== lastStep) {
      const tr = document.createElement('tr');
      tr.className = 'step-group-header';
      tr.innerHTML = `<td colspan="2">${STEPS[q.step] || q.step}</td>`;
      tbody.appendChild(tr);
      lastStep = q.step;
    }
    const ans = answers[q.id];
    let ansStr = '<span class="badge-skipped">Niet beantwoord</span>';
    if (ans !== undefined && ans !== null && ans !== '') {
      if (Array.isArray(ans)) {
        ansStr = ans.map(v => {
          const opt = q.options ? q.options.find(o => o.value === v) : null;
          return opt ? opt.label : v;
        }).join(' · ');
      } else if (q.options) {
        const opt = q.options.find(o => o.value === ans);
        ansStr = opt ? opt.label : ans;
      } else {
        ansStr = ans;
      }
    }
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td style="font-size:0.82rem;color:var(--muted);max-width:320px;line-height:1.4">${q.text}</td>
      <td style="font-weight:500">${ansStr}</td>
    `;
    tbody.appendChild(tr);
  });

  showScreen('screen-results');
}

function restart() {
  currentQ = 0; answers = {}; history = [];
  userInfo = { name: '', org: '', email: '' };
  ['user-name','user-org','user-email'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.value = '';
  });
  clearEmailError();
  const btn = document.getElementById('btn-submit-email');
  if (btn) { btn.disabled = false; btn.textContent = 'Bekijk mijn compliance-rapport →'; }
  showScreen('screen-intro');
}
</script>
</body>
</html>
