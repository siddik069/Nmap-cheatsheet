<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NMAP Reference Guide</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #050a08;
    --panel: #080f0b;
    --border: #0d2b1a;
    --green: #00ff6a;
    --green-dim: #00c44f;
    --green-faint: #0a2e18;
    --cyan: #00e5ff;
    --cyan-dim: #00b8cc;
    --amber: #ffb300;
    --red: #ff4444;
    --text: #c8e6c9;
    --text-dim: #4a7a58;
    --mono: 'Share Tech Mono', monospace;
    --head: 'Orbitron', sans-serif;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--mono);
    font-size: 13px;
    line-height: 1.6;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Scanline overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,255,106,0.015) 2px,
      rgba(0,255,106,0.015) 4px
    );
    pointer-events: none;
    z-index: 9999;
  }

  /* Noise texture */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 9998;
    opacity: 0.4;
  }

  /* HEADER */
  header {
    padding: 40px 40px 30px;
    border-bottom: 1px solid var(--border);
    position: relative;
    overflow: hidden;
    background: linear-gradient(180deg, #000a04 0%, transparent 100%);
  }

  header::before {
    content: 'NMAP';
    position: absolute;
    right: -20px;
    top: -20px;
    font-family: var(--head);
    font-size: 160px;
    font-weight: 900;
    color: rgba(0,255,106,0.03);
    line-height: 1;
    user-select: none;
    letter-spacing: -5px;
  }

  .header-tag {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--green);
    letter-spacing: 4px;
    text-transform: uppercase;
    margin-bottom: 8px;
    opacity: 0.7;
  }

  header h1 {
    font-family: var(--head);
    font-size: 32px;
    font-weight: 900;
    color: var(--green);
    letter-spacing: 6px;
    text-shadow: 0 0 30px rgba(0,255,106,0.5), 0 0 60px rgba(0,255,106,0.2);
    animation: pulse-glow 3s ease-in-out infinite;
  }

  @keyframes pulse-glow {
    0%, 100% { text-shadow: 0 0 30px rgba(0,255,106,0.5), 0 0 60px rgba(0,255,106,0.2); }
    50% { text-shadow: 0 0 40px rgba(0,255,106,0.8), 0 0 80px rgba(0,255,106,0.4); }
  }

  .header-sub {
    font-size: 11px;
    color: var(--text-dim);
    letter-spacing: 2px;
    margin-top: 6px;
  }

  .header-meta {
    display: flex;
    gap: 24px;
    margin-top: 20px;
    flex-wrap: wrap;
  }

  .meta-pill {
    font-size: 10px;
    letter-spacing: 2px;
    padding: 4px 12px;
    border: 1px solid var(--border);
    color: var(--text-dim);
  }

  .meta-pill span { color: var(--green); }

  /* MAIN LAYOUT */
  main {
    max-width: 1100px;
    margin: 0 auto;
    padding: 40px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
  }

  /* SECTION CARD */
  .section {
    border: 1px solid var(--border);
    background: var(--panel);
    position: relative;
    animation: fadeIn 0.5s ease both;
  }

  .section.full-width { grid-column: 1 / -1; }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .section:nth-child(1) { animation-delay: 0.1s; }
  .section:nth-child(2) { animation-delay: 0.2s; }
  .section:nth-child(3) { animation-delay: 0.3s; }
  .section:nth-child(4) { animation-delay: 0.4s; }

  /* Corner decorations */
  .section::before, .section::after {
    content: '';
    position: absolute;
    width: 10px;
    height: 10px;
    border-color: var(--green);
    border-style: solid;
  }
  .section::before { top: -1px; left: -1px; border-width: 2px 0 0 2px; }
  .section::after  { bottom: -1px; right: -1px; border-width: 0 2px 2px 0; }

  /* Section Header */
  .section-head {
    padding: 12px 20px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    gap: 12px;
    background: linear-gradient(90deg, rgba(0,255,106,0.05) 0%, transparent 60%);
  }

  .section-num {
    font-family: var(--head);
    font-size: 9px;
    font-weight: 700;
    color: var(--bg);
    background: var(--green);
    padding: 2px 6px;
    letter-spacing: 1px;
  }

  .section-title {
    font-family: var(--head);
    font-size: 11px;
    font-weight: 700;
    color: var(--green);
    letter-spacing: 3px;
    text-transform: uppercase;
  }

  /* SUBSECTION */
  .subsec {
    padding: 16px 20px;
    border-bottom: 1px solid rgba(13,43,26,0.6);
  }

  .subsec:last-child { border-bottom: none; }

  .subsec-num {
    font-size: 10px;
    color: var(--text-dim);
    letter-spacing: 1px;
    margin-bottom: 6px;
  }

  .subsec-num span { color: var(--cyan); }

  .subsec-title {
    font-size: 12px;
    color: var(--cyan);
    letter-spacing: 1px;
    margin-bottom: 10px;
    font-weight: bold;
  }

  /* Command block */
  .cmd-block {
    background: #020704;
    border: 1px solid var(--border);
    border-left: 3px solid var(--green);
    padding: 10px 14px;
    margin: 8px 0;
    position: relative;
    overflow: hidden;
  }

  .cmd-block::before {
    content: '$';
    color: var(--green-dim);
    margin-right: 8px;
    user-select: none;
  }

  .cmd {
    color: var(--green);
    font-size: 12px;
    word-break: break-all;
    display: inline;
  }

  .cmd .flag { color: var(--cyan); }
  .cmd .target { color: var(--amber); }
  .cmd .path { color: var(--text); opacity: 0.7; }
  .cmd .comment {
    display: block;
    color: var(--text-dim);
    font-size: 10px;
    margin-top: 4px;
    padding-left: 14px;
  }

  /* Info text */
  .info {
    font-size: 11px;
    color: var(--text-dim);
    line-height: 1.7;
    margin-bottom: 8px;
  }

  /* Flag table */
  .flag-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 8px;
  }

  .flag-table tr {
    border-bottom: 1px solid rgba(13,43,26,0.5);
    transition: background 0.15s;
  }

  .flag-table tr:hover { background: rgba(0,255,106,0.03); }
  .flag-table tr:last-child { border-bottom: none; }

  .flag-table td {
    padding: 7px 10px;
    vertical-align: top;
  }

  .flag-table td:first-child {
    color: var(--cyan);
    white-space: nowrap;
    width: 120px;
    font-size: 11px;
  }

  .flag-table td:last-child {
    color: var(--text-dim);
    font-size: 11px;
    line-height: 1.5;
  }

  /* Badge */
  .badge {
    display: inline-block;
    font-size: 9px;
    padding: 1px 6px;
    letter-spacing: 1px;
    margin-left: 6px;
    vertical-align: middle;
  }

  .badge.warn {
    background: rgba(255,179,0,0.1);
    border: 1px solid rgba(255,179,0,0.3);
    color: var(--amber);
  }

  .badge.info-b {
    background: rgba(0,229,255,0.08);
    border: 1px solid rgba(0,229,255,0.2);
    color: var(--cyan);
  }

  /* Divider */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, var(--green) 0%, transparent 100%);
    margin: 8px 0;
    opacity: 0.2;
  }

  /* Rate badge for timing */
  .timing-row {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    padding: 8px 0;
    border-bottom: 1px solid rgba(13,43,26,0.4);
  }
  .timing-row:last-child { border-bottom: none; }

  .t-label {
    min-width: 110px;
    color: var(--cyan);
    font-size: 11px;
  }

  .t-val {
    color: var(--text-dim);
    font-size: 11px;
    line-height: 1.5;
  }

  .t-val .highlight { color: var(--green); }

  /* Footer */
  footer {
    text-align: center;
    padding: 24px 40px;
    border-top: 1px solid var(--border);
    color: var(--text-dim);
    font-size: 10px;
    letter-spacing: 2px;
  }

  footer span { color: var(--green); }

  @media (max-width: 700px) {
    main { grid-template-columns: 1fr; padding: 20px; }
    .section.full-width { grid-column: 1; }
    header { padding: 24px 20px; }
    header h1 { font-size: 22px; }
  }
</style>
</head>
<body>

<header>
  <div class="header-tag">// Network Mapper — Reference Manual</div>
  <h1>NMAP CHEATSHEET</h1>
  <div class="header-sub">OS DETECTION · SERVICE DETECTION · TIMING · OUTPUT</div>
  <div class="header-meta">
    <div class="meta-pill">CATEGORY: <span>RECONNAISSANCE</span></div>
    <div class="meta-pill">PLATFORM: <span>LINUX / WIN / MAC</span></div>
    <div class="meta-pill">TYPE: <span>ACTIVE SCANNING</span></div>
  </div>
</header>

<main>

  <!-- ═══════════════════════════════════════════ -->
  <!-- SECTION 1: OS & SERVICE DETECTION          -->
  <!-- ═══════════════════════════════════════════ -->
  <div class="section full-width">
    <div class="section-head">
      <div class="section-num">01</div>
      <div class="section-title">OS &amp; Service Detection</div>
    </div>

    <!-- 1.1 Version Detection Overview -->
    <div class="subsec">
      <div class="subsec-num"><span>1.1</span> — Overview</div>
      <div class="subsec-title">Version Detection Overview</div>
      <p class="info">
        Nmap's version detection engine probes open ports to determine the software name, version number, and other attributes of the service. Combined with OS detection, it builds a detailed picture of a target host's attack surface.
      </p>
      <table class="flag-table">
        <tr>
          <td>-sV</td>
          <td>Probe open ports to determine service/version info</td>
        </tr>
        <tr>
          <td>--version-intensity</td>
          <td>Set probe intensity from 0 (light) to 9 (all probes) — default is 7</td>
        </tr>
        <tr>
          <td>--version-light</td>
          <td>Shorthand for <span style="color:var(--cyan)">--version-intensity 2</span> (faster, less accurate)</td>
        </tr>
        <tr>
          <td>--version-all</td>
          <td>Shorthand for <span style="color:var(--cyan)">--version-intensity 9</span> (every probe tried)</td>
        </tr>
        <tr>
          <td>--version-trace</td>
          <td>Print detailed version scan activity for debugging</td>
        </tr>
      </table>
    </div>

    <div style="display:grid;grid-template-columns:1fr 1fr;gap:0;border-top:1px solid var(--border)">

      <!-- 1.2 OS Detection -->
      <div class="subsec" style="border-right:1px solid var(--border)">
        <div class="subsec-num"><span>1.2</span> — OS Fingerprinting</div>
        <div class="subsec-title">OS Detection</div>
        <p class="info">Nmap sends a series of TCP/UDP/ICMP probes and compares responses against its OS fingerprint database.</p>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-O</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Enable OS detection</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-O --osscan-limit</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Only try OS detect if ≥1 open &amp; ≥1 closed TCP port</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-O --osscan-guess</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Aggressively guess OS when unsure</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-O --fuzzy</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Synonym for --osscan-guess</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-A</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Aggressive: OS + version + scripts + traceroute</span>
        </div>
      </div>

      <!-- 1.3 Service Version Detection -->
      <div class="subsec">
        <div class="subsec-num"><span>1.3</span> — Service Probing</div>
        <div class="subsec-title">Service Version Detection</div>
        <p class="info">Queries open ports with protocol-specific payloads to extract banner and version strings.</p>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-sV</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Standard service version detection</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-sV --version-intensity 5</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Custom probe depth (0–9)</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-sV --version-light</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Faster, fewer probes (intensity 2)</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-sV --version-all</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Try every probe (intensity 9)</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-sV -sC</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Version + default scripts for rich detail</span>
        </div>
      </div>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════ -->
  <!-- SECTION 2: TIMING OPTIONS                  -->
  <!-- ═══════════════════════════════════════════ -->
  <div class="section full-width">
    <div class="section-head">
      <div class="section-num">02</div>
      <div class="section-title">Timing Options</div>
    </div>

    <!-- 2.1 Overview -->
    <div class="subsec">
      <div class="subsec-num"><span>2.1</span> — Timing Overview</div>
      <div class="subsec-title">Timing Options Overview</div>
      <p class="info">
        Nmap provides six timing templates (T0–T5) and granular parameters to balance scan speed against stealth and accuracy. Faster scans risk triggering IDS/IPS systems; slower scans reduce noise.
      </p>
      <table class="flag-table">
        <tr><td>-T0 (paranoid)</td><td>Extremely slow. Serialised probes, 5-min delay. IDS evasion.</td></tr>
        <tr><td>-T1 (sneaky)</td><td>Very slow. 15-sec delay between probes.</td></tr>
        <tr><td>-T2 (polite)</td><td>Slow. 400ms delay. Minimises bandwidth usage.</td></tr>
        <tr><td>-T3 (normal)</td><td>Default. Balances speed and reliability.</td></tr>
        <tr><td>-T4 (aggressive)</td><td>Fast. Assumes reliable, fast network. Common choice.</td></tr>
        <tr><td>-T5 (insane)</td><td>Extremely fast. May miss results on slow networks.</td></tr>
      </table>
    </div>

    <!-- 2.2–2.6 grid -->
    <div style="display:grid;grid-template-columns:1fr 1fr 1fr;border-top:1px solid var(--border)">

      <!-- 2.2 Timing Parameters -->
      <div class="subsec" style="border-right:1px solid var(--border)">
        <div class="subsec-num"><span>2.2</span> — Core Params</div>
        <div class="subsec-title">Timing Parameters</div>
        <div class="timing-row">
          <div class="t-label">--host-timeout</div>
          <div class="t-val">Max time per host. e.g. <span class="highlight">--host-timeout 30m</span></div>
        </div>
        <div class="timing-row">
          <div class="t-label">--max-retries</div>
          <div class="t-val">Cap probe retransmissions. e.g. <span class="highlight">--max-retries 2</span></div>
        </div>
        <div class="timing-row">
          <div class="t-label">--initial-rtt-timeout</div>
          <div class="t-val">Starting probe RTT. e.g. <span class="highlight">100ms</span></div>
        </div>
        <div class="timing-row">
          <div class="t-label">--max-rtt-timeout</div>
          <div class="t-val">Upper RTT limit. e.g. <span class="highlight">1000ms</span></div>
        </div>
        <div class="cmd-block" style="margin-top:10px">
          <span class="cmd">nmap <span class="flag">--max-retries 2 --host-timeout 30m</span> <span class="target">&lt;target&gt;</span></span>
        </div>
      </div>

      <!-- 2.3 Min Scan Delay -->
      <div class="subsec" style="border-right:1px solid var(--border)">
        <div class="subsec-num"><span>2.3</span> — Rate Control</div>
        <div class="subsec-title">Minimum Scan Delay</div>
        <p class="info">Sets a minimum wait time between probes sent to a host. Useful for avoiding overwhelmed or rate-limiting targets.</p>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">--scan-delay 1s</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Wait ≥1 second between probes</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">--scan-delay 500ms</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># 500ms minimum between probes</span>
        </div>
        <p class="info" style="margin-top:8px">Time units: <span style="color:var(--cyan)">ms</span> · <span style="color:var(--cyan)">s</span> · <span style="color:var(--cyan)">m</span> · <span style="color:var(--cyan)">h</span></p>
      </div>

      <!-- 2.4 Min Packet Rate -->
      <div class="subsec">
        <div class="subsec-num"><span>2.4</span> — Throughput</div>
        <div class="subsec-title">Minimum Packet Rate</div>
        <p class="info">Forces Nmap to send at least N packets per second, overriding adaptive timing. Useful when you need guaranteed throughput.</p>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">--min-rate 100</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Send ≥100 packets/sec</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">--min-rate 500</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Aggressive floor rate</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">--min-parallelism 10</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Minimum concurrent probes</span>
        </div>
      </div>
    </div>

    <div style="display:grid;grid-template-columns:1fr 1fr;border-top:1px solid var(--border)">

      <!-- 2.5 Max Scan Delay -->
      <div class="subsec" style="border-right:1px solid var(--border)">
        <div class="subsec-num"><span>2.5</span> — Delay Cap</div>
        <div class="subsec-title">Maximum Scan Delay</div>
        <p class="info">Caps the maximum inter-probe delay. Prevents Nmap's adaptive engine from slowing down too much on congested networks.</p>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">--max-scan-delay 10ms</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Never wait more than 10ms between probes</span>
        </div>
        <div class="cmd-block">
          <span class="cmd">nmap <span class="flag">-T4 --max-scan-delay 5ms</span> <span class="target">&lt;target&gt;</span></span>
          <span class="cmd comment"># Combine with -T4 for tight control</span>
        </div>
      </div>

      <!-- 2.6 Max Packet Rate -->
      <div class="subsec">
        <div clas
