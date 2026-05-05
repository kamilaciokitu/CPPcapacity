<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CPP Multi-Country Feedback Report — ITU-D</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg-primary: #ffffff;
      --bg-secondary: #f5f4f0;
      --bg-tertiary: #eeede9;
      --bg-danger: #fcebeb;
      --bg-success: #e1f5ee;
      --bg-info: #e6f1fb;
      --text-primary: #1a1a18;
      --text-secondary: #5f5e5a;
      --text-danger: #a32d2d;
      --text-success: #085041;
      --text-info: #0c447c;
      --border: rgba(0,0,0,0.12);
      --border-med: rgba(0,0,0,0.2);
      --border-danger: rgba(163,45,45,0.3);
      --border-success: rgba(8,80,65,0.25);
      --radius-md: 8px;
      --radius-lg: 12px;
      --font: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
    }

    @media (prefers-color-scheme: dark) {
      :root {
        --bg-primary: #1c1c1a;
        --bg-secondary: #242422;
        --bg-tertiary: #2c2c2a;
        --bg-danger: #2a1515;
        --bg-success: #0a1f1a;
        --bg-info: #0a1e30;
        --text-primary: #e8e6df;
        --text-secondary: #a09e96;
        --text-danger: #f09595;
        --text-success: #5dcaa5;
        --text-info: #85b7eb;
        --border: rgba(255,255,255,0.1);
        --border-med: rgba(255,255,255,0.18);
        --border-danger: rgba(240,149,149,0.25);
        --border-success: rgba(93,202,165,0.2);
      }
    }

    body {
      font-family: var(--font);
      background: var(--bg-tertiary);
      color: var(--text-primary);
      min-height: 100vh;
      display: flex;
      align-items: flex-start;
      justify-content: center;
      padding: 24px 16px;
    }

    .card {
      background: var(--bg-primary);
      border: 0.5px solid var(--border);
      border-radius: var(--radius-lg);
      width: 100%;
      max-width: 820px;
      overflow: hidden;
    }

    .hdr {
      padding: 20px 26px 14px;
      border-bottom: 0.5px solid var(--border);
      display: flex;
      align-items: flex-start;
      justify-content: space-between;
      gap: 12px;
      flex-wrap: wrap;
    }
    .hdr h1 { font-size: 16px; font-weight: 500; color: var(--text-primary); margin-bottom: 3px; }
    .hdr p { font-size: 12px; color: var(--text-secondary); }
    .hdr-right { font-size: 10px; color: var(--text-secondary); text-align: right; line-height: 1.6; }

    .bdg { display: inline-flex; gap: 6px; margin-top: 7px; flex-wrap: wrap; }
    .b { font-size: 11px; font-weight: 500; padding: 3px 9px; border-radius: 20px; }
    .b-itu { background: #002868; color: #fff; }

    .nav {
      display: flex;
      gap: 0;
      border-bottom: 0.5px solid var(--border);
      padding: 0 26px;
      overflow-x: auto;
      scrollbar-width: none;
    }
    .nav::-webkit-scrollbar { display: none; }
    .nb {
      font-size: 12px;
      padding: 8px 12px;
      cursor: pointer;
      background: none;
      border: none;
      border-bottom: 2px solid transparent;
      color: var(--text-secondary);
      white-space: nowrap;
      font-family: var(--font);
    }
    .nb.on { color: var(--text-primary); border-bottom-color: #002868; font-weight: 500; }
    .nb:hover { color: var(--text-primary); }

    .sec { display: none; padding: 18px 26px; }
    .sec.on { display: block; }

    .st { font-size: 13px; font-weight: 500; color: var(--text-primary); margin: 16px 0 10px; }
    .st:first-child { margin-top: 0; }

    .narrative {
      padding: 14px 16px;
      margin-bottom: 18px;
      border-left: 3px solid #002868;
      border-radius: 0 var(--radius-lg) var(--radius-lg) 0;
      background: var(--bg-secondary);
    }
    .narrative p { font-size: 12px; color: var(--text-secondary); line-height: 1.8; }

    .sat-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 10px; margin-bottom: 18px; }
    .sat-card { background: var(--bg-secondary); border-radius: var(--radius-md); padding: 13px 15px; text-align: center; }
    .sat-flag { font-size: 20px; margin-bottom: 6px; }
    .sat-score { font-size: 24px; font-weight: 500; margin-bottom: 2px; color: #1D9E75; }
    .sat-score.blue { color: #002868; }
    .sat-label { font-size: 11px; color: var(--text-secondary); margin-bottom: 8px; }
    .sat-bar { height: 6px; border-radius: 3px; background: var(--border); overflow: hidden; margin-bottom: 6px; }
    .sat-fill { height: 100%; border-radius: 3px; }
    .sat-detail { font-size: 10px; color: var(--text-secondary); line-height: 1.5; }

    .mets { display: grid; grid-template-columns: repeat(auto-fit, minmax(110px, 1fr)); gap: 8px; margin-bottom: 18px; }
    .met { background: var(--bg-secondary); border-radius: var(--radius-md); padding: 11px 13px; }
    .ml { font-size: 11px; color: var(--text-secondary); margin-bottom: 4px; }
    .mv { font-size: 19px; font-weight: 500; color: var(--text-primary); }
    .ms { font-size: 11px; color: var(--text-secondary); margin-top: 2px; }

    .br { display: flex; align-items: center; gap: 9px; margin-bottom: 7px; }
    .bl { min-width: 165px; color: var(--text-secondary); text-align: right; font-size: 11px; flex-shrink: 0; }
    .bt { flex: 1; height: 6px; background: var(--border); border-radius: 4px; overflow: hidden; }
    .bf { height: 100%; border-radius: 4px; }
    .bp { min-width: 70px; font-size: 11px; color: var(--text-primary); font-weight: 500; }

    .div { border: none; border-top: 0.5px solid var(--border); margin: 14px 0; }

    .qi {
      border-left: 3px solid var(--border-med);
      padding: 7px 12px;
      font-size: 11px;
      color: var(--text-secondary);
      font-style: italic;
      line-height: 1.6;
      margin-bottom: 8px;
      border-radius: 0 4px 4px 0;
      background: var(--bg-secondary);
    }

    .sb { background: var(--bg-secondary); border-radius: var(--radius-md); padding: 11px 13px; }
    .sb.accent-blue { border-left: 3px solid #002868; border-radius: 0 var(--radius-md) var(--radius-md) 0; }
    .sb.accent-green { border-left: 3px solid #1D9E75; border-radius: 0 var(--radius-md) var(--radius-md) 0; }
    .sq { font-size: 11px; color: var(--text-secondary); margin-bottom: 6px; font-weight: 500; }
    .sb-title { font-size: 12px; font-weight: 500; color: var(--text-primary); margin-bottom: 4px; }
    .sb-body { font-size: 11px; color: var(--text-secondary); line-height: 1.6; }

    .ic {
      border: 0.5px solid var(--border);
      border-radius: var(--radius-lg);
      padding: 11px 13px;
      margin-bottom: 7px;
      display: flex;
      gap: 11px;
      align-items: flex-start;
    }
    .sv { width: 4px; border-radius: 2px; flex-shrink: 0; align-self: stretch; min-height: 32px; }
    .sv-red { background: #D85A30; }
    .sv-amber { background: #BA7517; }
    .sv-purple { background: #534AB7; }
    .it { font-size: 12px; font-weight: 500; color: var(--text-primary); margin-bottom: 2px; }
    .id { font-size: 11px; color: var(--text-secondary); line-height: 1.5; }
    .im { display: flex; gap: 5px; margin-top: 5px; flex-wrap: wrap; }
    .tg { font-size: 10px; padding: 2px 7px; border-radius: 20px; background: var(--bg-secondary); color: var(--text-secondary); border: 0.5px solid var(--border); }
    .tg-mz { background: #E1F5EE; color: #085041; border-color: #9FE1CB; }
    .tg-jo { background: #E6F1FB; color: #0C447C; border-color: #B5D4F4; }
    .tg-ge { background: #FAEEDA; color: #412402; border-color: #FAC775; }
    .tg-all { background: #EEEDFE; color: #3C3489; border-color: #AFA9EC; }

    .bug { background: var(--bg-danger); border: 0.5px solid var(--border-danger); border-radius: var(--radius-md); padding: 10px 13px; margin-bottom: 8px; }
    .bug-title { font-size: 12px; font-weight: 500; color: var(--text-danger); margin-bottom: 3px; }
    .bug-desc { font-size: 11px; color: var(--text-secondary); line-height: 1.5; }

    .ctry-hdr { display: flex; align-items: center; gap: 8px; margin-bottom: 10px; }
    .ctry-name { font-size: 13px; font-weight: 500; color: var(--text-primary); }
    .ctry-sub { font-size: 11px; color: var(--text-secondary); }

    .sg { display: grid; grid-template-columns: 1fr 1fr; gap: 11px; }

    .cmp-row { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 9px; }
    .cmp-cell { background: var(--bg-secondary); border-radius: var(--radius-md); padding: 10px 12px; }
    .cmp-label { font-size: 10px; color: var(--text-secondary); margin-bottom: 4px; font-weight: 500; }
    .cmp-val { font-size: 11px; color: var(--text-primary); line-height: 1.5; }

    .rc { border-left: 3px solid; padding: 10px 13px; margin-bottom: 8px; border-radius: 0 var(--radius-md) var(--radius-md) 0; background: var(--bg-secondary); }
    .rc.p1 { border-color: #D85A30; }
    .rc.p2 { border-color: #BA7517; }
    .rc.p3 { border-color: #1D9E75; }
    .rl { font-size: 10px; font-weight: 500; margin-bottom: 2px; }
    .rc.p1 .rl { color: #D85A30; }
    .rc.p2 .rl { color: #BA7517; }
    .rc.p3 .rl { color: #1D9E75; }
    .rt { font-size: 12px; font-weight: 500; color: var(--text-primary); margin-bottom: 2px; }
    .rd { font-size: 11px; color: var(--text-secondary); line-height: 1.5; }

    .positives { font-size: 11px; color: var(--text-secondary); line-height: 1.9; }

    svg text { font-family: var(--font); }

    @media (max-width: 520px) {
      .sg, .cmp-row { grid-template-columns: 1fr; }
      .bl { min-width: 110px; }
      .hdr { padding: 16px 16px 12px; }
      .sec { padding: 14px 16px; }
      .nav { padding: 0 16px; }
    }
  </style>
</head>
<body>
<div class="card">

  <div class="hdr">
    <div>
      <h1>Connectivity Planning Platform — Multi-Country Feedback Report</h1>
      <p>Country A (March 2025) · Country B (March 2026) · Country C (April 2026)</p>
      <div class="bdg">
        <span class="b b-itu">ITU-D</span>
        <span class="b" style="background:#006B3C;color:#fff">Country A</span>
        <span class="b" style="background:#DC143C;color:#fff">Country B</span>
        <span class="b" style="background:#007A3D;color:#fff">Country C</span>
      </div>
    </div>
    <div class="hdr-right">Confidential<br>ITU-D Connectivity Planning Platform</div>
  </div>

  <div class="nav">
    <button class="nb on" onclick="go('ov',this)">Overview</button>
    <button class="nb" onclick="go('mz',this)">Country A</button>
    <button class="nb" onclick="go('jo',this)">Country C</button>
    <button class="nb" onclick="go('ge',this)">Country B</button>
    <button class="nb" onclick="go('cmp',this)">Comparison</button>
    <button class="nb" onclick="go('mp',this)">Map</button>
    <button class="nb" onclick="go('rc',this)">Recommendations</button>
  </div>

  <!-- OVERVIEW -->
  <div id="tab-ov" class="sec on">
    <div class="narrative">
      <p>Across three countries and three distinct engagement formats, the Connectivity Planning Platform has generated strong interest and genuine enthusiasm from government agencies, telecom regulators, and technical specialists. Participants consistently recognized the platform's strategic value for data-driven connectivity planning, and several proactively requested deeper collaboration, including joining the CPP Ambassador Programme. These engagements confirm that CPP is addressing a real and pressing need across ITU-D member states, and that the foundation for a growing community of practice is already in place. The feedback gathered reflects the commitment of all participants to make this platform work within their national contexts, and is offered in a spirit of constructive partnership.</p>
    </div>

    <div class="st">Participant satisfaction</div>
    <div class="sat-grid">
      <div class="sat-card">
        <div class="sat-flag">A</div>
        <div class="sat-score">High</div>
        <div class="sat-label">Country A, ~8 participants</div>
        <div class="sat-bar"><div class="sat-fill" style="width:80%;background:#1D9E75"></div></div>
        <div class="sat-detail">Platform value recognized by all. Majority answered Yes or Mostly yes on ease of navigation. Practical relevance confirmed.</div>
      </div>
      <div class="sat-card">
        <div class="sat-flag">B</div>
        <div class="sat-score">Strong</div>
        <div class="sat-label">Country B, 4 participants</div>
        <div class="sat-bar"><div class="sat-fill" style="width:88%;background:#1D9E75"></div></div>
        <div class="sat-detail">Advanced user independently tested platform before session. Accepted Ambassador role enthusiastically. Clear project pipeline identified.</div>
      </div>
      <div class="sat-card">
        <div class="sat-flag">C</div>
        <div class="sat-score">Positive</div>
        <div class="sat-label">Country C, 4 participants</div>
        <div class="sat-bar"><div class="sat-fill" style="width:75%;background:#1D9E75"></div></div>
        <div class="sat-detail">Formal appreciation letter submitted. two national institutions engaged jointly. Ambassador programme discussed. Follow-up session requested.</div>
      </div>
      <div class="sat-card">
        <div class="sat-flag">📊</div>
        <div class="sat-score blue">~80%</div>
        <div class="sat-label">Overall satisfaction rate</div>
        <div class="sat-bar"><div class="sat-fill" style="width:80%;background:#002868"></div></div>
        <div class="sat-detail">Across all sessions. All countries confirmed platform relevance. No country expressed intent to disengage.</div>
      </div>
    </div>

    <div class="mets">
      <div class="met"><div class="ml">Countries engaged</div><div class="mv">3</div><div class="ms">A, B, C</div></div>
      <div class="met"><div class="ml">Participants</div><div class="mv">~16</div><div class="ms">All sessions</div></div>
      <div class="met"><div class="ml">Issues identified</div><div class="mv">26</div><div class="ms">UX, strategic, bugs</div></div>
      <div class="met"><div class="ml">Critical (P1)</div><div class="mv">7</div><div class="ms">Fix immediately</div></div>
      <div class="met"><div class="ml">Confirmed bugs</div><div class="mv">3</div><div class="ms">Technical failures</div></div>
      <div class="met"><div class="ml">Ambassadors</div><div class="mv">2</div><div class="ms">Country B confirmed, Country C pending</div></div>
    </div>

    <div class="st">Cross-country themes</div>
    <div class="br"><span class="bl">Regulator vs investor scope</span><div class="bt"><div class="bf" style="width:100%;background:#D85A30"></div></div><span class="bp">All 3, Critical</span></div>
    <div class="br"><span class="bl">Calculation failures, no error msg</span><div class="bt"><div class="bf" style="width:95%;background:#D85A30"></div></div><span class="bp">Countries A and B</span></div>
    <div class="br"><span class="bl">Data confidentiality trust</span><div class="bt"><div class="bf" style="width:90%;background:#D85A30"></div></div><span class="bp">All 3</span></div>
    <div class="br"><span class="bl">Projects not editable or resumable</span><div class="bt"><div class="bf" style="width:88%;background:#D85A30"></div></div><span class="bp">Countries A and B</span></div>
    <div class="br"><span class="bl">Dataset status inconsistent</span><div class="bt"><div class="bf" style="width:82%;background:#BA7517"></div></div><span class="bp">Country B, confirmed bug</span></div>
    <div class="br"><span class="bl">Workflow steps unclear</span><div class="bt"><div class="bf" style="width:78%;background:#BA7517"></div></div><span class="bp">Countries A and C</span></div>
    <div class="br"><span class="bl">Scenario flexibility</span><div class="bt"><div class="bf" style="width:72%;background:#BA7517"></div></div><span class="bp">Countries B and C</span></div>
    <div class="br"><span class="bl">Map usability</span><div class="bt"><div class="bf" style="width:65%;background:#BA7517"></div></div><span class="bp">Countries A and B</span></div>
    <div class="br"><span class="bl">Multi-institution data</span><div class="bt"><div class="bf" style="width:62%;background:#BA7517"></div></div><span class="bp">Countries B and C</span></div>

    <hr class="div">
    <div class="st">Key quotes across all sessions</div>
    <div class="qi">A "Why are the datasets in the same table as economic parameters? I didn't see them!" — Country A participant</div>
    <div class="qi">C "These parameters seem more for the investment ministry, not for us as a regulator." — Country C national regulatory commission (a national regulatory commission representative)</div>
    <div class="qi">B "I got the status failed 73, nothing more. As a user I need to know WHY it failed." — Country B national communications commission (the specialist)</div>
    <div class="qi">B "It will be great to put points directly on the map without uploading a file." — Country B national communications commission (the specialist)</div>

    <hr class="div">
    <div class="st">Strategic opportunity</div>
    <div class="sb accent-blue">
      <div class="sb-title">A growing and diverse community of practice</div>
      <div class="sb-body">These three engagements represent a rich cross-section of ITU-D member states: a non-technical ministry workshop (Country A), a regulatory onboarding (Country C), and an advanced technical session (Country B). Each country brings a different context, data maturity, and use case. Addressing the shared feedback on regulator-facing scenarios, data governance, and platform stability will unlock the platform's potential across all three and set a strong precedent for future country onboardings.</div>
    </div>
  </div>

  <!-- COUNTRY A -->
  <div id="tab-mz" class="sec">
    <div class="ctry-hdr">
      <span style="font-size:20px">A</span>
      <div><div class="ctry-name">Country A — In-Person Workshop</div><div class="ctry-sub">~8 participants · Ministry level · In-person · March 2025</div></div>
    </div>
    <div class="sb accent-green" style="margin-bottom:14px">
      <div class="sq">Overall sentiment</div>
      <div class="sb-body">Participants engaged actively throughout the workshop and consistently affirmed the practical value of the platform for connectivity planning. The majority responded positively across all survey dimensions including navigation, clarity, and output usefulness. The feedback below reflects participants' investment in making the platform work well for their context.</div>
    </div>
    <div class="st">Critical issues</div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Upload field does not reset after validation</div><div class="id">Filename persists after upload. Users block themselves trying to upload a second dataset.</div><div class="im"><span class="tg">Critical</span><span class="tg-mz">Country A</span></div></div></div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Revenue outputs wildly inflated — hide immediately</div><div class="id">Small regional projects show multi-billion revenue. Calculation bug must be hidden until fixed.</div><div class="im"><span class="tg">Critical</span><span class="tg-mz">Country A</span></div></div></div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Steps 2 and 3 skipped silently — no validation gate</div><div class="id">Technology and dataset steps are missed. Projects are submitted incomplete with no warning and cannot be edited afterwards.</div><div class="im"><span class="tg">Critical</span><span class="tg-mz">Country A</span></div></div></div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Project inputs invisible after calculation</div><div class="id">Only accessible by downloading a file. Non-technical users need inputs visible on-screen.</div><div class="im"><span class="tg">Critical</span><span class="tg-mz">Country A</span></div></div></div>
    <div class="st">UX and map issues</div>
    <div class="ic"><div class="sv sv-amber"></div><div><div class="it">Step labels appear clickable but are not</div><div class="id">Users expect to navigate back and forward via step indicators.</div><div class="im"><span class="tg">High</span><span class="tg-mz">Country A</span></div></div></div>
    <div class="ic"><div class="sv sv-amber"></div><div><div class="it">Dataset list and upload on separate pages</div><div class="id">Should be one page: list above, upload below.</div><div class="im"><span class="tg">High</span><span class="tg-mz">Country A</span></div></div></div>
    <div class="ic"><div class="sv sv-amber"></div><div><div class="it">No basemap, no zoom-to-extent, unclear legend</div><div class="id">No OSM background. Users lose orientation. Dark equals high density is not explained. ADM1/ADM2 boundaries missing.</div><div class="im"><span class="tg">High</span><span class="tg-mz">Country A</span></div></div></div>
    <div class="st">Feature requests</div>
    <div class="positives">
      <div>Auto-generated executive PDF report for decision-makers</div>
      <div>In-platform screenshot tool</div>
      <div>Delete capability for datasets and projects</div>
      <div>Modify inputs and re-run projects</div>
    </div>
  </div>

  <!-- COUNTRY C -->
  <div id="tab-jo" class="sec">
    <div class="ctry-hdr">
      <span style="font-size:20px">C</span>
      <div><div class="ctry-name">Country C — Regulatory and Digital Economy Agencies</div><div class="ctry-sub">4 participants · Remote onboarding · April 2026</div></div>
    </div>
    <div class="sb accent-green" style="margin-bottom:14px">
      <div class="sq">Overall sentiment</div>
      <div class="sb-body">Country C's team, spanning both the national regulatory commission and digital economy ministry, engaged thoughtfully and submitted a formal appreciation letter following the session. Their feedback reflects a high level of institutional seriousness and a genuine desire to integrate CPP into national connectivity planning processes. A follow-up data session was proactively requested, and the team expressed openness to representing the Arab region as CPP ambassadors.</div>
    </div>
    <div class="st">Strategic concerns (formal letter and session)</div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Platform is investor/operator-oriented, not regulator-oriented</div><div class="id">Both predefined scenarios (NPV max and TCO min) reflect investor logic. The regulatory commission's mandate is coverage regulation and interconnection, not investment optimization.</div><div class="im"><span class="tg">Critical</span><span class="tg-jo">Country C</span><span class="tg-all">Strategic</span></div></div></div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Need for regulator-specific scenarios — collaborative design requested</div><div class="id">Country C formally requests co-development of models reflecting regulatory needs, plus consultative sessions with regulators from other countries.</div><div class="im"><span class="tg">Critical</span><span class="tg-jo">Country C</span></div></div></div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Data confidentiality needs formal documentation</div><div class="id">Verbal reassurance was given in session but formal policy documentation is required for institutional adoption.</div><div class="im"><span class="tg">Critical</span><span class="tg-jo">Country C</span></div></div></div>
    <div class="st">Usability and data observations</div>
    <div class="ic"><div class="sv sv-amber"></div><div><div class="it">Only 2 predefined scenarios — insufficient for regulatory work</div><div class="id">Country C asked explicitly for custom scenarios or coverage-gap-focused options.</div><div class="im"><span class="tg">High</span><span class="tg-jo">Country C</span></div></div></div>
    <div class="ic"><div class="sv sv-amber"></div><div><div class="it">Data fragmented across national regulatory commission, operators, and ministries</div><div class="id">Connectivity data sits in multiple institutions. The platform assumes one data owner, which does not match Country C's reality.</div><div class="im"><span class="tg">High</span><span class="tg-jo">Country C</span></div></div></div>
    <div class="ic"><div class="sv sv-amber"></div><div><div class="it">Example datasets not downloadable before project creation</div><div class="id">Confirmed bug. a national regulatory commission representative could not access example data before creating a project.</div><div class="im"><span class="tg">High</span><span class="tg-jo">Country C</span><span class="tg-mz">Also Country A</span></div></div></div>
    <div class="st">Positive signals</div>
    <div class="positives">
      <div>Two national institutions attending together — strong cross-ministry engagement</div>
      <div>Open to ambassador programme (digital economy ministry recommended as umbrella body)</div>
      <div>Country C has a national open data platform — potential CPP integration point</div>
      <div>Proactively mapped data gaps across institutions</div>
    </div>
  </div>

  <!-- COUNTRY B -->
  <div id="tab-ge" class="sec">
    <div class="ctry-hdr">
      <span style="font-size:20px">B</span>
      <div><div class="ctry-name">Country B — National Communications Commission</div><div class="ctry-sub">4 participants · Remote session · March 2026</div></div>
    </div>
    <div class="sb accent-green" style="margin-bottom:14px">
      <div class="sq">Overall sentiment</div>
      <div class="sb-body">Country B's team arrived as the most technically engaged participants across all sessions. a national communications commission specialist independently accessed the platform before the meeting, prepared real fiber node data, and tested project creation, demonstrating a high level of initiative and commitment. He accepted the CPP Ambassador invitation enthusiastically, and the team has a clear and realistic project pipeline with international development bank funding discussions already on the horizon.</div>
    </div>
    <div class="st">Confirmed technical bugs</div>
    <div class="bug">
      <div class="bug-title">Bug 1 — Calculation engine fails with no meaningful error</div>
      <div class="bug-desc">Projects fail with only "status: failed 73" and no explanation. the specialist tried two projects with real real fiber node data, with and without duplicates. Both failed on Edge and Chrome. Error code 73 is backend-only and never surfaced to users.</div>
    </div>
    <div class="bug">
      <div class="bug-title">Bug 2 — Dataset status shows "unknown" after successful validation</div>
      <div class="bug-desc">After validation shows "successful," status flips to "unknown" in All Datasets view. The dataset cannot be opened or inspected. This is likely the root cause of calculation failures, as the engine cannot read the dataset. Reproduced live during the session.</div>
    </div>
    <div class="bug">
      <div class="bug-title">Bug 3 — No input datasets shown on failed project view</div>
      <div class="bug-desc">"No input data sets available" is shown inside a failed project even when datasets were uploaded and validated. The dataset-to-project link is broken when calculation fails.</div>
    </div>
    <div class="st">Feature requests from advanced user</div>
    <div class="ic"><div class="sv sv-purple"></div><div><div class="it">Direct point placement on the map, no file upload needed</div><div class="id">"Put a point directly on the map and calculate from it." Particularly valuable for smaller or less technical users who cannot prepare structured CSV or Excel files.</div><div class="im"><span class="tg">Feature request</span><span class="tg-ge">Country B</span></div></div></div>
    <div class="ic"><div class="sv sv-purple"></div><div><div class="it">Save draft and resume incomplete projects</div><div class="id">Closing a project mid-creation loses all progress. Must restart from scratch, which is especially difficult when working with hundreds of nodes.</div><div class="im"><span class="tg">Feature request</span><span class="tg-ge">Country B</span><span class="tg-mz">Also Country A</span></div></div></div>
    <div class="ic"><div class="sv sv-purple"></div><div><div class="it">Edit projects in "not yet calculated" state</div><div class="id">Once started but not submitted, projects enter a locked editing mode. Users cannot add or remove information.</div><div class="im"><span class="tg">Feature request</span><span class="tg-ge">Country B</span><span class="tg-mz">Also Country A</span></div></div></div>
    <div class="st">Context and use case</div>
    <div class="sg">
      <div class="sb"><div class="sq">Connectivity maturity</div><div class="sb-body">Country B is highly connected with near-complete fiber and mobile coverage including 5G. CPP will be used for new infrastructure projects in rural and underserved areas.</div></div>
      <div class="sb"><div class="sq">Primary use case</div><div class="sb-body">White zone elimination with a 2-3 year timeline and future budget optimization for optical cable deployment. an international development bank identified as a potential funder.</div></div>
    </div>
    <div class="st">Positive signals</div>
    <div class="positives">
      <div>the specialist enthusiastically accepted CPP Ambassador Programme invitation</div>
      <div>Most technically self-sufficient user, will test extensively and provide bug reports</div>
      <div>National data portal in progress, expected complete by end of 2026</div>
      <div>Clear funding pathway identified with an international development bank — output report directly useful</div>
    </div>
  </div>

  <!-- COMPARISON -->
  <div id="tab-cmp" class="sec">
    <div class="st">Three-country comparison</div>
    <div class="cmp-row">
      <div class="cmp-cell"><div class="cmp-label">A Session type</div><div class="cmp-val">In-person workshop, Country A, March 2025</div></div>
      <div class="cmp-cell"><div class="cmp-label">C Session type</div><div class="cmp-val">Remote onboarding, Country C, April 2026</div></div>
      <div class="cmp-cell"><div class="cmp-label">B Session type</div><div class="cmp-val">Remote onboarding, Country B, March 2026</div></div>
    </div>
    <div class="cmp-row">
      <div class="cmp-cell"><div class="cmp-label">A User profile</div><div class="cmp-val">Non-technical ministry practitioners</div></div>
      <div class="cmp-cell"><div class="cmp-label">C User profile</div><div class="cmp-val">National telecom regulator and digital economy ministry</div></div>
      <div class="cmp-cell"><div class="cmp-label">B User profile</div><div class="cmp-val">Advanced technical specialist (national communications commission)</div></div>
    </div>
    <div class="cmp-row">
      <div class="cmp-cell"><div class="cmp-label">A Connectivity status</div><div class="cmp-val">Major connectivity gaps, rural priority</div></div>
      <div class="cmp-cell"><div class="cmp-label">C Connectivity status</div><div class="cmp-val">Moderate, some rural gaps</div></div>
      <div class="cmp-cell"><div class="cmp-label">B Connectivity status</div><div class="cmp-val">High, new projects and white zones only</div></div>
    </div>
    <div class="cmp-row">
      <div class="cmp-cell"><div class="cmp-label">A Top blocker</div><div class="cmp-val">UX friction, upload reset, inflated revenue, no edit</div></div>
      <div class="cmp-cell"><div class="cmp-label">C Top blocker</div><div class="cmp-val">Regulatory scenarios absent, data governance clarity needed</div></div>
      <div class="cmp-cell"><div class="cmp-label">B Top blocker</div><div class="cmp-val">Calculation engine crash, dataset status bug, no draft save</div></div>
    </div>
    <div class="cmp-row">
      <div class="cmp-cell"><div class="cmp-label">A Data readiness</div><div class="cmp-val">Partial, data held by ministry</div></div>
      <div class="cmp-cell"><div class="cmp-label">C Data readiness</div><div class="cmp-val">Fragmented across regulator, operators, and ministry</div></div>
      <div class="cmp-cell"><div class="cmp-label">B Data readiness</div><div class="cmp-val">Portal in progress, ready end-2026</div></div>
    </div>
    <div class="cmp-row">
      <div class="cmp-cell"><div class="cmp-label">A Ambassador</div><div class="cmp-val">Not discussed</div></div>
      <div class="cmp-cell"><div class="cmp-label">C Ambassador</div><div class="cmp-val">Pending, digital economy ministry as umbrella recommended</div></div>
      <div class="cmp-cell"><div class="cmp-label">B Ambassador</div><div class="cmp-val">the specialist accepted enthusiastically</div></div>
    </div>
    <hr class="div">
    <div class="st">Issues shared across all 3 countries</div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Platform scenarios are oriented towards investors — regulators need their own framing</div><div class="id">All three sessions independently surfaced this observation. Developing regulatory-focused scenarios would significantly broaden the platform's relevance across member states.</div><div class="im"><span class="tg-all">All 3 countries</span><span class="tg">Strategic</span></div></div></div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Data confidentiality requires formal documentation</div><div class="id">Country C raised it formally. Country B was reassured during the session. Country A flagged it in the workshop. Published policy would support institutional adoption across all contexts.</div><div class="im"><span class="tg-all">All 3 countries</span></div></div></div>
    <div class="ic"><div class="sv sv-red"></div><div><div class="it">Projects cannot be edited or resumed after interruption</div><div class="id">Country A must restart if a step is missed. Country B loses progress when closing mid-creation. Both confirmed independently as significant friction points.</div><div class="im"><span class="tg-mz">Country A</span><span class="tg-ge">Country B</span></div></div></div>
    <div class="ic"><div class="sv sv-amber"></div><div><div class="it">Example datasets not accessible before project creation</div><div class="id">Confirmed independently in Countries B and C. Making these available upfront would support self-directed onboarding.</div><div class="im"><span class="tg-jo">Country C</span><span class="tg-ge">Country B</span></div></div></div>
    <div class="ic"><div class="sv sv-amber"></div><div><div class="it">Data fragmented across multiple national institutions</div><div class="id">Both Country B and Country C hold connectivity data across multiple agencies. A multi-institution data mapping guide would help countries prepare for platform use.</div><div class="im"><span class="tg-jo">Country C</span><span class="tg-ge">Country B</span></div></div></div>
  </div>

  <!-- MAP -->
  <div id="tab-mp" class="sec">
    <div class="st">Session locations</div>
    <div style="border:0.5px solid var(--border);border-radius:var(--radius-lg);overflow:hidden;margin-bottom:14px">
      <svg width="100%" viewBox="0 0 640 380" role="img" xmlns="http://www.w3.org/2000/svg">
        <title>World map showing Country A, Country C and Country B as CPP partner countries</title>
        <desc>Simplified world map with three countries highlighted and their capital cities marked</desc>
        <rect width="640" height="380" fill="#f0efe9"/>
        <path d="M215,58 L275,50 L336,60 L376,86 L406,112 L430,150 L440,194 L444,238 L442,282 L432,320 L412,346 L382,358 L352,351 L328,338 L310,318 L296,296 L286,270 L278,244 L270,216 L260,188 L250,160 L238,132 L226,106 L220,80 L216,64 Z" fill="#cccbc2" stroke="#b4b2a8" stroke-width="0.7"/>
        <path d="M293,33 L338,23 L368,26 L388,33 L396,48 L386,58 L370,66 L352,70 L334,66 L314,56 L296,48 Z" fill="#cccbc2" stroke="#b4b2a8" stroke-width="0.7"/>
        <path d="M398,53 L438,48 L466,56 L476,76 L470,100 L453,110 L430,106 L412,90 L400,70 Z" fill="#cccbc2" stroke="#b4b2a8" stroke-width="0.7"/>
        <path d="M422,42 L452,36 L474,40 L482,52 L476,62 L456,66 L436,62 L424,52 Z" fill="#cccbc2" stroke="#b4b2a8" stroke-width="0.7"/>
        <path d="M458,28 L558,20 L608,38 L618,78 L598,108 L568,118 L538,113 L508,98 L486,88 L470,100 L453,110 L430,106 L418,83 L438,48 L458,28 Z" fill="#cccbc2" stroke="#b4b2a8" stroke-width="0.7"/>
        <path d="M58,53 L108,40 L138,48 L146,78 L136,113 L118,143 L103,168 L93,198 L86,228 L80,258 L86,283 L76,308 L60,323 L48,308 L40,278 L43,243 L50,208 L46,168 L40,138 L33,103 L36,70 L53,56 Z" fill="#cccbc2" stroke="#b4b2a8" stroke-width="0.7"/>
        <path d="M352,268 L364,256 L374,244 L378,230 L377,216 L372,204 L368,216 L364,232 L358,248 L354,262 Z" fill="#006B3C" opacity="0.55" stroke="#005030" stroke-width="0.8"/>
        <path d="M428,74 L442,68 L452,72 L456,84 L450,96 L438,100 L426,94 L422,82 Z" fill="#007A3D" opacity="0.6" stroke="#005A30" stroke-width="0.8"/>
        <path d="M426,44 L450,38 L470,42 L478,52 L472,60 L450,64 L430,60 L424,50 Z" fill="#DC143C" opacity="0.45" stroke="#B00020" stroke-width="0.8"/>
        <circle cx="360" cy="285" r="8" fill="#006B3C" opacity="0.2"/><circle cx="360" cy="285" r="5" fill="#006B3C" opacity="0.5"/><circle cx="360" cy="285" r="3" fill="#006B3C"/>
        <circle cx="440" cy="84" r="8" fill="#007A3D" opacity="0.2"/><circle cx="440" cy="84" r="5" fill="#007A3D" opacity="0.5"/><circle cx="440" cy="84" r="3" fill="#007A3D"/>
        <circle cx="450" cy="50" r="8" fill="#DC143C" opacity="0.2"/><circle cx="450" cy="50" r="5" fill="#DC143C" opacity="0.5"/><circle cx="450" cy="50" r="3" fill="#DC143C"/>
        <rect x="368" y="274" width="76" height="20" rx="3" fill="white" stroke="#005030" stroke-width="0.5"/>
        <text x="406" y="288" text-anchor="middle" font-size="11" font-weight="500" fill="#006B3C">Country A A</text>
        <rect x="450" y="72" width="66" height="20" rx="3" fill="white" stroke="#005A30" stroke-width="0.5"/>
        <text x="483" y="86" text-anchor="middle" font-size="11" font-weight="500" fill="#007A3D">Country C C</text>
        <rect x="460" y="38" width="66" height="20" rx="3" fill="white" stroke="#B00020" stroke-width="0.5"/>
        <text x="493" y="52" text-anchor="middle" font-size="11" font-weight="500" fill="#DC143C">Country B B</text>
        <rect x="10" y="10" width="205" height="40" rx="6" fill="white" stroke="#ccc" stroke-width="0.5"/>
        <text x="20" y="26" font-size="12" font-weight="500" fill="#1a1a18">CPP Engagements — ITU-D</text>
        <text x="20" y="42" font-size="10" fill="#5f5e5a">A · B · C · 2025-2026</text>
        <rect x="10" y="338" width="290" height="30" rx="5" fill="white" stroke="#ccc" stroke-width="0.5"/>
        <circle cx="24" cy="354" r="5" fill="#006B3C"/><text x="33" y="358" font-size="10" fill="#5f5e5a">Country A (workshop)</text>
        <circle cx="148" cy="354" r="5" fill="#007A3D"/><text x="157" y="358" font-size="10" fill="#5f5e5a">Country C (remote)</text>
        <circle cx="225" cy="354" r="5" fill="#DC143C"/><text x="234" y="358" font-size="10" fill="#5f5e5a">Country B (remote)</text>
      </svg>
    </div>
    <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:11px">
      <div class="sb"><div class="sq">Country A</div><div class="sb-body">Major connectivity gaps. Rural priority. Non-technical users. Strong platform value recognized. Focus: simplified UX and workflow clarity.</div></div>
      <div class="sb"><div class="sq">Country C</div><div class="sb-body">Established regulator. National open data platform available. Moderate connectivity. Focus: regulatory scenarios and formal data governance.</div></div>
      <div class="sb"><div class="sq">Country B</div><div class="sb-body">Highly connected. Advanced technical users. White zone projects. National data portal in progress. Focus: stable calculation engine and draft-save capability.</div></div>
    </div>
  </div>

  <!-- RECOMMENDATIONS -->
  <div id="tab-rc" class="sec">
    <div class="st">Priority 1 — Fix immediately</div>
    <div class="rc p1"><div class="rl">P1 · Bug · B</div><div class="rt">Fix calculation engine and surface meaningful error messages</div><div class="rd">Country B's the specialist reproduced this reliably. "Status: failed 73" provides no actionable information. Investigating the dataset-to-project linkage failure and validating against real-world node data would resolve this.</div></div>
    <div class="rc p1"><div class="rl">P1 · Bug · B</div><div class="rt">Fix dataset status showing "unknown" after successful validation</div><div class="rd">Reproduced live during the session. Dataset validates successfully but shows "unknown" in the list and cannot be opened, breaking the calculation pipeline.</div></div>
    <div class="rc p1"><div class="rl">P1 · UX · A</div><div class="rt">Reset upload field after validation and address inflated revenue display</div><div class="rd">Upload field does not clear after save. Revenue calculation shows inflated figures for small regional projects — the field should be hidden until the underlying issue is resolved.</div></div>
    <div class="rc p1"><div class="rl">P1 · Onboarding · C B</div><div class="rt">Make example datasets downloadable before project creation</div><div class="rd">Confirmed in both Country B and Country C sessions. Making these available upfront would allow users to explore the platform independently from the first login.</div></div>
    <div class="rc p1"><div class="rl">P1 · Workflow · A B</div><div class="rt">Enable draft save, resume, and project editing</div><div class="rd">Country A participants cannot modify projects post-submission. Country B's the specialist loses all progress when closing mid-creation. Both scenarios are particularly acute for large datasets with many nodes.</div></div>
    <div class="rc p1"><div class="rl">P1 · Strategy · All 3</div><div class="rt">Develop regulator-specific scenarios and publish a data confidentiality policy</div><div class="rd">All three countries represent regulatory and government bodies whose needs go beyond the two existing investor-facing scenarios. A published data governance policy would remove a key barrier to institutional adoption.</div></div>
    <div class="st">Priority 2 — Short-term improvements</div>
    <div class="rc p2"><div class="rl">P2 · Map · A B</div><div class="rt">Add direct point placement on map, OSM basemap, and zoom-to-extent</div><div class="rd">Country B explicitly requested the ability to draw nodes directly on the map. Country A needs an OSM background and a zoom-to-layer button. Both improvements reduce dependence on file preparation skills.</div></div>
    <div class="rc p2"><div class="rl">P2 · Transparency · A</div><div class="rt">Show project inputs as an in-app dashboard</div><div class="rd">All inputs visible on-screen after calculation. Minimum: read-only panel. Ideal: editable with option to re-run.</div></div>
    <div class="rc p2"><div class="rl">P2 · Engagement · C</div><div class="rt">Host a cross-country regulator working group to co-design scenarios</div><div class="rd">Country C formally requested consultative sessions with regulators from other countries. Countries A and B would be natural participants. ITU-D is well-placed to convene this group.</div></div>
    <div class="rc p2"><div class="rl">P2 · Data · C B</div><div class="rt">Create a multi-institution data mapping guide</div><div class="rd">Both Country B and Country C hold connectivity data across multiple agencies. A practical guide identifying which fields come from which type of institution would significantly accelerate country readiness.</div></div>
    <div class="st">Priority 3 — Roadmap</div>
    <div class="rc p3"><div class="rl">P3 · Reporting</div><div class="rt">Auto-generate an executive PDF for funders and decision-makers</div><div class="rd">Country B needs this for international development bank proposals. Country A needs it for government presentations. A one-click export with map, key metrics, and cost table would extend the platform's reach to non-technical audiences.</div></div>
    <div class="rc p3"><div class="rl">P3 · Ambassador programme</div><div class="rt">Formalize: Country B (the specialist/national communications commission) confirmed, Country C (digital economy ministry) pending</div><div class="rd">Country B brings strong technical depth. Country C's digital economy ministry is well-positioned as a regional institutional lead. Both would strengthen the CPP community of practice.</div></div>
    <div class="rc p3"><div class="rl">P3 · Platform</div><div class="rt">Delete, edit, and re-run across all project types</div><div class="rd">Expected as standard functionality across all three countries and all skill levels.</div></div>
  </div>

</div>

<script>
  function go(id, btn) {
    document.querySelectorAll('.sec').forEach(s => s.classList.remove('on'));
    document.querySelectorAll('.nb').forEach(b => b.classList.remove('on'));
    document.getElementById('tab-' + id).classList.add('on');
    btn.classList.add('on');
  }
</script>
</body>
</html>
