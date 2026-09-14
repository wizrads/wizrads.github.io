---
layout: single
title: "What is the best medical physics program? I asked eight LLMs"
excerpt: "Eight large language models, 320 answers, and a ranking of US graduate medical physics programs that nobody had published before. Including the two schools in the top ten that run no accredited graduate program at all."
tags:
  - medical physics
  - LLMs
  - data
# The body is a stylesheet plus 260 lines of chart code; feed readers would get
# all of it and none of the charts. Send them the excerpt and a link instead.
feed:
  excerpt_only: true
google_fonts: "https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,700;9..144,900&family=IBM+Plex+Mono:wght@400;600&display=swap"
---


<div class="llm-post">
<style>
/* Scoped to .llm-post so nothing here escapes into the rest of the site and the
   theme's own rules cannot reach in. Originally the standalone page's :root and
   bare element selectors. Light only, on purpose - see the post's front matter. */
.llm-post{
  color-scheme: light only;
  --paper:#ffffff; --panel:#f2f3f3; --ink:#1e2225; --ink-2:#494e52; --ink-3:#8a9095;
  --rule:#bdc1c4; --grid:#e4e6e7;
  --yes:#007bb6;   /* site blue */
  --no:#ef8f22;    /* site orange, one step darker so bars clear 3:1 on white */
  --seq-0:#eef5f9; --seq-1:#c2e0ef; --seq-2:#93c9e3; --seq-3:#52adc8; --seq-4:#007bb6; --seq-5:#00537a;
  --shadow:0 1px 2px rgba(30,34,37,.07);

  margin:0; background:var(--paper); color:var(--ink-2);
  font-family:-apple-system,".SFNSText-Regular","San Francisco",Roboto,"Segoe UI","Helvetica Neue","Lucida Grande",Arial,sans-serif;
  font-size:17px; line-height:1.65; padding-inline:20px; padding-block:0;
  -webkit-font-smoothing:antialiased;
}
.llm-post,.llm-post *{box-sizing:border-box}

/* Undoing what the theme paints on these elements. Everything below then styles
   the post the way the standalone page did. Kept first so later rules win. */
.llm-post figure{display:block}
.llm-post figcaption{font-family:inherit;margin-bottom:0}
.llm-post table{border:0}
.llm-post thead{background-color:transparent;border-bottom:0}
.llm-post th,.llm-post td{border-right:0}
.llm-post h2{padding-bottom:0;border-bottom:0}

.llm-post .wrap{max-width:1080px;margin:0 auto}
.llm-post .col{max-width:66ch;margin-inline:auto}
.llm-post h2,.llm-post .num,.llm-post .figtitle,.llm-post .pullquote .said{font-family:"Fraunces",Georgia,serif;color:var(--ink)}
.llm-post h2{font-size:clamp(1.4rem,3.2vw,2rem);line-height:1.18;margin:0 0 .4em;font-weight:700;text-wrap:balance}
.llm-post .lede{font-size:1.2rem;color:var(--ink-2);margin:0 0 1.2em;line-height:1.5}
.llm-post .coi{font-size:.92rem;color:var(--ink-2);border-left:4px solid var(--no);
  padding:.55em 0 .55em 1em;margin:0 0 1.6em}
.llm-post .coi strong{color:var(--ink)}
.llm-post p{margin:0 0 1.1em}
.llm-post a{color:var(--yes)}
.llm-post strong{font-weight:600;color:var(--ink)}
.llm-post .kicker{border-top:2px solid var(--ink);padding-top:.7em;margin-top:4.2em}
.llm-post section{margin-bottom:3em}
/* The standalone page opened with a big h1; the theme renders the title instead,
   so this only has to clear the theme's own heading. */
.llm-post header{padding-block:0 1.4rem}
.llm-post .prompt{font-family:"IBM Plex Mono",monospace;font-size:.88rem;border-left:3px solid var(--yes);
  padding:.2em 0 .2em 1em;color:var(--ink-2);margin:1.4em 0}

.llm-post .hero-stat{display:flex;gap:clamp(1rem,3vw,2.2rem);align-items:center;flex-wrap:wrap;
  border-block:2px solid var(--ink);padding-block:1.3rem;margin-block:2.2em}
.llm-post .hero-stat .num{font-size:clamp(3.4rem,12vw,6.4rem);line-height:.84;font-weight:900;
  font-variation-settings:"opsz" 144;color:var(--yes);letter-spacing:-.03em;font-variant-numeric:tabular-nums}
.llm-post .hero-stat .say{font-size:1.02rem;color:var(--ink-2);max-width:36ch;flex:1 1 16rem}

.llm-post figure{margin:0 0 1.2em;background:var(--panel);border:1px solid var(--rule);border-radius:8px;
  padding:clamp(1rem,2.6vw,1.6rem);box-shadow:var(--shadow)}
.llm-post .figwide{max-width:1080px;margin-inline:auto}
.llm-post figcaption{font-size:.85rem;color:var(--ink-3);margin-top:.9em;line-height:1.55}
.llm-post .figtitle{font-weight:700;font-size:1.05rem;margin:0 0 .15em}
.llm-post .figsub{font-size:.84rem;color:var(--ink-3);margin:0 0 1em}
.llm-post svg{display:block;width:100%;height:auto;overflow:visible}
.llm-post .legend{display:flex;gap:1.1rem;flex-wrap:wrap;align-items:center;margin:0 0 .9em;
  font-family:"IBM Plex Mono",monospace;font-size:.73rem;color:var(--ink-2)}
.llm-post .legend i{width:11px;height:11px;border-radius:3px;display:inline-block;margin-right:.4em;vertical-align:-1px}
.llm-post .axlab{font-family:"IBM Plex Mono",monospace;font-size:11px;fill:var(--ink-3)}
.llm-post .marklab{font-family:"IBM Plex Mono",monospace;font-size:12px;fill:var(--ink);font-variant-numeric:tabular-nums}
.llm-post .namelab{font-family:-apple-system,"Helvetica Neue",Arial,sans-serif;font-size:12.5px;fill:var(--ink)}
.llm-post .gridline{stroke:var(--grid);stroke-width:1}
.llm-post .axis{stroke:var(--rule);stroke-width:1}
.llm-post .bar{transition:opacity .12s}
.llm-post .hitrow:hover .bar,.llm-post .hitrow:focus-visible .bar{opacity:.75}
.llm-post .hitrow{cursor:default}
.llm-post .hitrow:focus-visible{outline:2px solid var(--yes);outline-offset:2px}

.llm-post #tip{position:fixed;pointer-events:none;z-index:50;opacity:0;transition:opacity .1s;
  background:var(--ink);color:var(--paper);font-family:"IBM Plex Mono",monospace;font-size:12px;
  padding:.5em .65em;border-radius:5px;max-width:15rem;line-height:1.45}
.llm-post #tip b{font-weight:600}

.llm-post details{margin-top:.9em;font-size:.85rem}
.llm-post summary{cursor:pointer;color:var(--ink-3);font-family:"IBM Plex Mono",monospace;font-size:.72rem;
  letter-spacing:.08em;text-transform:uppercase}
.llm-post summary:focus-visible{outline:2px solid var(--yes);outline-offset:2px}
.llm-post .tbl{width:100%;border-collapse:collapse;margin-top:.7em;font-family:"IBM Plex Mono",monospace;font-size:.77rem}
.llm-post .tbl th,.llm-post .tbl td{text-align:left;padding:.32em .5em;border-bottom:1px solid var(--rule);font-variant-numeric:tabular-nums}
.llm-post .tbl th{color:var(--ink-3);font-weight:600}
.llm-post .scroll{overflow-x:auto}
.llm-post .foot{color:var(--ink-3);font-size:.84rem;border-top:1px solid var(--rule);padding-top:1.3em;margin-top:1em}
.llm-post .pullquote{margin:1.6em 0;padding:1em 0 1em 1.4em;border-left:4px solid var(--no);
  font-size:1.16rem;line-height:1.5}
.llm-post .pullquote cite{display:block;margin-top:.7em;font-style:normal;font-size:.78rem;
  font-family:"IBM Plex Mono",monospace;color:var(--ink-3);letter-spacing:.04em}
.llm-post .tally{display:flex;gap:4px;flex-wrap:wrap;margin:1.3em 0 .4em}
.llm-post .tally span{width:34px;height:42px;border-radius:4px;display:grid;place-items:center;
  font-family:"IBM Plex Mono",monospace;font-size:.7rem;font-weight:600}
.llm-post .tally .ok{background:var(--seq-1);color:var(--ink)}
.llm-post .tally .no{background:var(--no);color:#fff}
.llm-post .tally-cap{font-family:"IBM Plex Mono",monospace;font-size:.71rem;color:var(--ink-3);margin:0 0 1.4em}
.llm-post .lineup{width:100%;border-collapse:collapse;margin:1.4em 0 .6em;font-size:.88rem}
.llm-post .lineup th{text-align:left;font-family:"IBM Plex Mono",monospace;font-size:.67rem;letter-spacing:.1em;
  text-transform:uppercase;color:var(--ink-3);font-weight:600;padding:0 .6em .5em 0;border-bottom:2px solid var(--ink)}
.llm-post .lineup td{padding:.5em .6em .5em 0;border-bottom:1px solid var(--rule);vertical-align:baseline;color:var(--ink-2)}
.llm-post .lineup .mk{font-weight:600;color:var(--ink)}
.llm-post .lineup .idc{font-family:"IBM Plex Mono",monospace;font-size:.71rem;color:var(--ink-3);word-break:break-all}
.llm-post .promptbox{background:var(--panel);border:1px solid var(--rule);border-radius:8px;
  padding:1.1rem 1.2rem;margin:1.5em 0}
.llm-post .promptline{font-family:"IBM Plex Mono",monospace;font-size:clamp(.82rem,2.4vw,1rem);
  line-height:1.75;color:var(--ink);margin:0}
.llm-post .swap{display:inline-block;position:relative;color:var(--no);font-weight:600;
  border-bottom:2px solid var(--no);padding:0 .12em;min-width:5.6em;text-align:center;
  transition:opacity .22s ease, transform .22s ease}
.llm-post .swap.out{opacity:0;transform:translateY(-.32em)}
.llm-post .degreebtns{display:flex;gap:.4rem;flex-wrap:wrap;margin-top:1rem}
.llm-post .degreebtns button{font-family:"IBM Plex Mono",monospace;font-size:.74rem;letter-spacing:.06em;
  padding:.42em .8em;border-radius:5px;border:1px solid var(--rule);background:var(--paper);
  color:var(--ink-2);cursor:pointer}
.llm-post .degreebtns button[aria-pressed="true"]{background:var(--no);border-color:var(--no);color:#fff;font-weight:600}
.llm-post .degreebtns button:focus-visible{outline:2px solid var(--yes);outline-offset:2px}
.llm-post .hashline{font-family:"IBM Plex Mono",monospace;font-size:.68rem;color:var(--ink-3);margin:.85em 0 0}
.llm-post .reworded{margin:1.4em 0 0;padding-top:1.1em;border-top:1px solid var(--rule)}
.llm-post .reworded p{font-family:"IBM Plex Mono",monospace;font-size:.82rem;line-height:1.6;
  color:var(--ink-2);margin:0 0 .7em}
.llm-post .reworded .tag{color:var(--ink-3);font-size:.68rem;letter-spacing:.1em;text-transform:uppercase;
  display:block;margin-bottom:.15em}
@media (prefers-reduced-motion:reduce){.llm-post .swap{transition:none}}
@media (prefers-reduced-motion:reduce){.llm-post *{transition:none!important;animation:none!important}}
</style>

<div class="wrap">
<header class="col">
  <p class="lede">Everyone in this field gets asked which program is best, and everyone has an
  opinion. Nobody has a ranking. US News ranks law schools, business schools, hospitals and
  engineering departments, and skips graduate medical physics entirely. So I got curious and asked
  the machines.</p>
  <p class="coi"><strong>Conflict of interest, stated up front:</strong> I am a PhD student at
  Wisconsin, which finished first. Weigh everything below accordingly.</p>
</header>

<div class="col">
<p>Rankings get made by people who pick the criteria, weight them, and publish the result.
Medical physics never got that treatment. It is a small field, under a hundred accredited
graduate programs in the United States, and no major outlet bothers to rank them. Applicants compare them by
asking around, which is how I ended up with opinions of my own.</p>

<p>Increasingly, applicants ask a chatbot instead. I wanted to know what it would tell them. So I
gave eight large language models, one from each major company, the same question in a fresh
conversation with no memory of the last one:</p>

<div class="promptbox">
  <p class="promptline">Rank the top 15 <span class="swap" id="swapword">graduate</span> programs
  in medical physics in the United States.</p>
  <p class="promptline" style="color:var(--ink-3);font-size:.82rem;margin-top:.9em">Give a numbered
  list from 1 to 15 containing only the institution name. Do not provide explanations, commentary,
  caveats, or additional text.</p>
  <div class="degreebtns" role="group" aria-label="Which degree word was used">
    <button type="button" id="w-graduate" aria-pressed="true">graduate</button>
    <button type="button" id="w-masters" aria-pressed="false">master&rsquo;s</button>
    <button type="button" id="w-doctoral" aria-pressed="false">doctoral</button>
  </div>

  <div class="reworded">
    <p><span class="tag">Reworded A</span>Which 15 graduate programs in medical physics are the best
    in the United States?</p>
    <p><span class="tag">Reworded B</span>List the 15 leading U.S. graduate programs in medical
    physics in rank order.</p>
  </div>
</div>

<p>Ten times each. Eighty answers. Then two reworded versions and a pair asking specifically about
master&rsquo;s and doctoral programs, for 320 answers in total. Every call went out one at a time.
I logged each one, hashed the response, and checked which company&rsquo;s servers actually
answered.</p>

<p>Here is the field. One model per company, pinned to an exact version and serving provider so
the same weights answered every time:</p>

<div class="scroll"><table class="lineup" id="lineup"><thead><tr>
<th>Model</th><th>Company</th><th>Exact version</th></tr></thead><tbody></tbody></table></div>

<div class="scroll"><table class="lineup" id="settings"><thead><tr>
<th>Model</th><th>Endpoint</th><th>Temperature</th><th>Max tokens</th><th>Reasoning</th>
</tr></thead><tbody></tbody></table></div>

<p>Two of the eight will not accept a temperature setting at all, and four cannot switch their
reasoning off, so those run at the lowest effort their provider allows. Holding every model to
identical settings is impossible, so the table records what each one actually accepted.</p>

<p>What comes back is a ranking. A confident one, with clear favourites and a sensible-looking
tail. The only question worth asking is what it is made of.</p>
</div>

<div class="col">
<div class="hero-stat">
  <div class="num">0</div>
  <div class="say">published rankings of US graduate medical physics programs existed before this
  one. The models produced a top 15 anyway, and largely agreed with each other.</div>
</div>
</div>

<section class="figwide">
  <figure>
    <p class="figtitle">The consensus top 15</p>
    <p class="figsub">Pooled across 8 models, 10 answers each. Every model gets one vote.</p>
    <div class="legend">
      <span><i style="background:var(--yes)"></i>CAMPEP-accredited</span>
      <span><i style="background:var(--no)"></i>No accredited graduate program</span>
      <span style="color:var(--ink-3)">whisker = 95% interval</span>
    </div>
    <div id="c-board"></div>
    <figcaption>Scoring is a normalised Borda count. First place earns 10 points, tenth earns 1,
    eleventh onward earns nothing. A score of 1.00 would mean every model put it first every time.
    The fraction beside each bar counts how many of the 80 answers named the school at all.</figcaption>
    <details><summary>Show the numbers</summary><div class="scroll" id="t-board"></div></details>
  </figure>
</section>

<div class="col">
<h2 class="kicker">The list holds up better than it should</h2>
<p>Thirteen of the fifteen hold CAMPEP accreditation, which is the field&rsquo;s actual quality
bar and the closest thing to an answer key. Wisconsin, MD Anderson and Duke lead, and any medical
physicist would put those three near the top of their own list. For a ranking assembled by next-token prediction, that is a respectable showing.</p>

<p>Two do not belong. Michigan lands fifth and Harvard tenth, and neither appears on the CAMPEP
graduate program list. Michigan showed up in 58 of 80 answers, so the models have settled on it.</p>

<p>Across every answer the models named 59 distinct schools, and 35 of those hold no accredited
graduate program in the field. Most appear once or twice. The models know the top of the field well and
improvise below it, which is roughly what a well-read undergraduate would do.</p>
</div>

<section class="figwide">
  <figure>
    <p class="figtitle">Master&rsquo;s or doctoral? The models shrug</p>
    <p class="figsub">All fifteen for each phrasing. Rank sits at the left of each row.</p>
    <div class="legend">
      <span><i style="background:var(--yes)"></i>CAMPEP-accredited</span>
      <span><i style="background:var(--no)"></i>No accredited graduate program</span>
    </div>
    <div id="c-three"></div>
    <figcaption>CAMPEP accredits master&rsquo;s and doctoral programs separately and they are
    genuinely different degrees, so I asked separately. The same three schools hold the top of all
    three lists. Below that the order reshuffles without settling into anything resembling a
    master&rsquo;s versus doctoral distinction. Columbia appears only for master&rsquo;s. Stanford
    and WashU appear only for doctoral. Vanderbilt falls from sixth to twelfth. The lists shuffle.
    They do not discriminate.</figcaption>
    <details><summary>Show the numbers</summary><div class="scroll" id="t-three"></div></details>
  </figure>
</section>

<div class="col">
<h2 class="kicker">So where is it getting this?</h2>
<p>Across the 79 graduate answers the models named 35 schools that hold no CAMPEP-accredited
graduate program. That is a narrower claim than it sounds. CAMPEP accredits graduate programs,
residencies and certificate programs on separate lists, and several of these schools run an
accredited residency or certificate program. What none of them runs is an accredited graduate
program, which is what the prompt asked for.</p>
<p>Look at the list and the pattern is obvious. These are famous universities. The models appear to
be reaching for general institutional reputation and letting it stand in for a field they have
little specific information about, which is roughly what a well-read undergraduate would do when
put on the spot.</p>
</div>

<section class="figwide">
  <figure>
    <p class="figtitle">Schools with no accredited GRADUATE program in medical physics</p>
    <p class="figsub">Times named across the 79 graduate answers. Darker segment counts top-ten finishes.</p>
    <div id="c-notelig"></div>
    <figcaption>Every one is a real, well-regarded university, and several run an accredited
    residency or certificate program in medical physics. What none of them runs is an accredited
    GRADUATE program, which is what the prompt asked for. Michigan accounts for a quarter of every
    mention here; the rest thin out quickly, which is what improvisation looks like.</figcaption>
    <details><summary>Show the numbers</summary><div class="scroll" id="t-notelig"></div></details>
  </figure>
</section>

<section class="figwide">
  <figure>
    <p class="figtitle">Rephrasing the question barely moves the needle</p>
    <p class="figsub">Each answer hands out ten top-ten places. The share of them that went to a
    school running no accredited graduate program.</p>
    <div id="c-share"></div>
    <figcaption>Read the top row like this: the graduate question drew 79 usable answers, so the
    models handed out 790 top-ten places in total, and 140 of them &mdash; about one in six &mdash;
    went to a school with no accredited graduate program in medical physics. Five ways of asking,
    five answers between 15 and 19 percent. Naming the degree explicitly buys about two and a half
    points. Michigan alone accounts for 41 percent of those places in the graduate question; the
    rest spread thinly across 17 other schools.</figcaption>
    <details><summary>Show the numbers</summary><div class="scroll" id="t-share"></div></details>
  </figure>
</section>

<div class="col">
<h2 class="kicker">Do they agree with each other?</h2>
<p>At the top, strongly. Wisconsin, MD Anderson and Duke clear nine of ten from almost every
model. Past Chicago the grid breaks apart, and several schools survive on one model&rsquo;s
enthusiasm. Gemini alone carries Johns Hopkins. DeepSeek alone carries UC Berkeley/UCSF.</p>
</div>

<section class="figwide">
  <figure>
    <p class="figtitle">Who picked whom</p>
    <p class="figsub">How many of each model&rsquo;s 10 answers put the school in its top ten.</p>
    <div class="legend">
      <span style="color:var(--ink-3)">0 of 10</span>
      <span><i style="background:var(--seq-1)"></i><i style="background:var(--seq-2)"></i><i style="background:var(--seq-3)"></i><i style="background:var(--seq-4)"></i><i style="background:var(--seq-5)"></i></span>
      <span style="color:var(--ink-3)">10 of 10</span>
      <span style="margin-left:auto"><i style="background:var(--no)"></i>no accredited graduate program</span>
    </div>
    <div id="c-heat"></div>
    <figcaption>Orange marks a school with no accredited graduate program in medical physics. The top four
    rows are near-unanimous. Everything below row five depends heavily on which model you asked.</figcaption>
    <details><summary>Show the numbers</summary><div class="scroll" id="t-heat"></div></details>
  </figure>
</section>

<div class="col">
<h2 class="kicker">One model refused to play</h2>
<p>Grok answered the graduate question ten times. Nine times it produced a tidy list of fifteen
schools. On the ninth attempt, same prompt and same settings, it stopped and said this:</p>

<div class="pullquote">
  <span class="said">&ldquo;There is no official or universally accepted ranking of graduate
  programs in medical physics. I cannot provide a fabricated list.&rdquo;</span>
  <cite>Grok 4.6 (xAI), graduate question, answer 9 of 10</cite>
</div>

<div class="tally" role="img" aria-label="Nine of ten answers listed fifteen schools; answer nine refused">
  <span class="ok">15</span><span class="ok">15</span><span class="ok">15</span><span class="ok">15</span><span class="ok">15</span><span class="ok">15</span><span class="ok">15</span><span class="ok">15</span><span class="no">no</span><span class="ok">15</span>
</div>
<p class="tally-cap">Grok&rsquo;s ten answers to the graduate question, in order.</p>

<p>It declined twice more on master&rsquo;s and twice again on doctoral. Claude did something
similar once on a reworded question, explaining at length that no such ranking exists in a form it
could reproduce.</p>

<p>Grok was right, which is the awkward part. There is no official ranking. Every list on this
page is fabricated in exactly the sense it meant. It just happened to be outvoted nine to one by
its own other answers.</p>
</div>

<div class="col">
<h2 class="kicker">So what is this worth?</h2>
<p>About as much as any ranking, which is the honest answer. Every ranking you have read was
somebody choosing criteria and weights. This one is eight language models averaging whatever they
absorbed about American universities, with no criteria at all. The exact prompt, the exact model
versions, the exact date and the raw counts are all here, so you can see how the sausage got made.
Most rankings do not offer that.</p>

<p>The reason it matters is that this is plausibly how a lot of applicants will shortlist programs
from here on. Not by reading accreditation lists. By asking a chatbot, once, and taking the answer.
Anyone who does that in medical physics gets Michigan fifth and Harvard tenth.</p>

<p>Which would sting more if program rank were the thing that mattered. It mostly is not &mdash;
and &ldquo;graduate&rdquo; is doing a lot of quiet work in that sentence, because it covers two
degrees that get decided on completely different grounds.</p>

<p>For a PhD, the department barely registers next to the person supervising it: what they
research, how they mentor, whether they have funding that will still be there in year four, whether
you can stand working with them for five years. Two students in the same accredited department can
have completely different educations depending on whose lab they join. No ranking captures that,
and no language model can tell you who your advisor should be.</p>

<p>A master&rsquo;s is a different question, and a harder one to put to a chatbot. It is a short
professional degree pointed at a residency, so the things that decide whether it was worth it are
mostly ones a ranking never sees. What does it cost, and is any of it funded or offset by a
stipend? How much clinical time do you actually get, in whose clinic, and on whose machines? And
above all, where did recent cohorts end up &mdash; because in this field the residency match is the
bottleneck, not the degree. Most accredited programs publish outcome data somewhere on their own
site: completion rates, and where their graduates went. Go read it. A program whose graduates
consistently match into residencies is telling you something a top-15 list cannot, and a program
that is vague about it is telling you something too.</p>

<p>So ask the chatbot if you like. Then go read the faculty pages, email a few of them, talk to
their current students, and look up the placement numbers. That is the part that decides your
degree.</p>

<p>And yes, Wisconsin came first, and yes, I go here. I would have told you it belonged near the
top before I ran any of this. That is exactly the problem with letting anyone, or anything, hand
you a ranking.</p>

<p class="foot">320 responses collected 14 September 2026 through OpenRouter, one call at a time,
with the model and serving provider verified on every call. Total cost, $1.28. Accreditation
checked against the CAMPEP GRADUATE program list as published on the collection date, 62 programs.
CAMPEP accredits residency and certificate programs on separate lists, which were not consulted, so
&ldquo;no accredited graduate program&rdquo; never means a school has no accredited program of any
kind.
School names were reconciled by hand, because automated matching proposed merging the University of
Iowa with the University of Miami.</p>
</div>
</div>

<div id="tip" role="status" aria-live="polite"></div>

<script id="data" type="application/json">{"graduate":[{"short":"Wisconsin","borda":0.914,"lo":0.881,"hi":0.936,"sel":79,"den":80,"campep":true,"rank":1},{"short":"UT MD Anderson","borda":0.795,"lo":0.755,"hi":0.831,"sel":79,"den":80,"campep":true,"rank":2},{"short":"Duke","borda":0.756,"lo":0.73,"hi":0.776,"sel":79,"den":80,"campep":true,"rank":3},{"short":"Chicago","borda":0.444,"lo":0.414,"hi":0.47,"sel":71,"den":80,"campep":true,"rank":4},{"short":"Michigan","borda":0.403,"lo":0.374,"hi":0.43,"sel":58,"den":80,"campep":false,"rank":5},{"short":"Penn","borda":0.393,"lo":0.365,"hi":0.419,"sel":66,"den":80,"campep":true,"rank":6},{"short":"UCLA","borda":0.236,"lo":0.206,"hi":0.268,"sel":47,"den":80,"campep":true,"rank":7},{"short":"Florida","borda":0.194,"lo":0.174,"hi":0.215,"sel":43,"den":80,"campep":true,"rank":8},{"short":"Vanderbilt","borda":0.175,"lo":0.151,"hi":0.201,"sel":42,"den":80,"campep":true,"rank":9},{"short":"Harvard","borda":0.16,"lo":0.14,"hi":0.181,"sel":18,"den":80,"campep":false,"rank":10},{"short":"WashU","borda":0.145,"lo":0.121,"hi":0.169,"sel":41,"den":80,"campep":true,"rank":11},{"short":"Stanford","borda":0.139,"lo":0.116,"hi":0.161,"sel":19,"den":80,"campep":true,"rank":12},{"short":"Johns Hopkins","borda":0.114,"lo":0.094,"hi":0.135,"sel":29,"den":80,"campep":true,"rank":13},{"short":"UC Berkeley/UCSF","borda":0.092,"lo":0.072,"hi":0.116,"sel":13,"den":80,"campep":true,"rank":14},{"short":"Columbia","borda":0.087,"lo":0.072,"hi":0.101,"sel":26,"den":80,"campep":true,"rank":15}],"masters":[{"short":"Wisconsin","borda":0.865,"lo":0.826,"hi":0.899,"sel":78,"den":80,"campep":true,"rank":1},{"short":"Duke","borda":0.81,"lo":0.775,"hi":0.841,"sel":78,"den":80,"campep":true,"rank":2},{"short":"UT MD Anderson","borda":0.72,"lo":0.672,"hi":0.766,"sel":76,"den":80,"campep":true,"rank":3},{"short":"Penn","borda":0.441,"lo":0.412,"hi":0.47,"sel":66,"den":80,"campep":true,"rank":4},{"short":"Chicago","borda":0.366,"lo":0.318,"hi":0.415,"sel":62,"den":80,"campep":true,"rank":5},{"short":"Vanderbilt","borda":0.344,"lo":0.31,"hi":0.38,"sel":56,"den":80,"campep":true,"rank":6},{"short":"Michigan","borda":0.331,"lo":0.302,"hi":0.359,"sel":45,"den":80,"campep":false,"rank":7},{"short":"Florida","borda":0.255,"lo":0.229,"hi":0.281,"sel":44,"den":80,"campep":true,"rank":8},{"short":"Columbia","borda":0.237,"lo":0.215,"hi":0.259,"sel":43,"den":80,"campep":true,"rank":9},{"short":"UCLA","borda":0.181,"lo":0.146,"hi":0.22,"sel":42,"den":80,"campep":true,"rank":10},{"short":"Harvard","borda":0.11,"lo":0.091,"hi":0.122,"sel":10,"den":80,"campep":false,"rank":11},{"short":"Johns Hopkins","borda":0.09,"lo":0.071,"hi":0.11,"sel":26,"den":80,"campep":true,"rank":12},{"short":"Stanford","borda":0.08,"lo":0.059,"hi":0.098,"sel":10,"den":80,"campep":true,"rank":13},{"short":"WashU","borda":0.07,"lo":0.051,"hi":0.089,"sel":19,"den":80,"campep":true,"rank":14},{"short":"Georgia Tech","borda":0.064,"lo":0.046,"hi":0.083,"sel":23,"den":80,"campep":true,"rank":15}],"doctoral":[{"short":"Wisconsin","borda":0.892,"lo":0.856,"hi":0.921,"sel":78,"den":80,"campep":true,"rank":1},{"short":"Duke","borda":0.721,"lo":0.694,"hi":0.744,"sel":78,"den":80,"campep":true,"rank":2},{"short":"UT MD Anderson","borda":0.719,"lo":0.684,"hi":0.749,"sel":78,"den":80,"campep":true,"rank":3},{"short":"Michigan","borda":0.44,"lo":0.405,"hi":0.474,"sel":60,"den":80,"campep":false,"rank":4},{"short":"Chicago","borda":0.43,"lo":0.396,"hi":0.461,"sel":63,"den":80,"campep":true,"rank":5},{"short":"Penn","borda":0.34,"lo":0.315,"hi":0.362,"sel":52,"den":80,"campep":true,"rank":6},{"short":"UCLA","borda":0.261,"lo":0.239,"hi":0.284,"sel":47,"den":80,"campep":true,"rank":7},{"short":"Stanford","borda":0.199,"lo":0.17,"hi":0.229,"sel":35,"den":80,"campep":true,"rank":8},{"short":"Florida","borda":0.189,"lo":0.172,"hi":0.204,"sel":43,"den":80,"campep":true,"rank":9},{"short":"WashU","borda":0.185,"lo":0.154,"hi":0.216,"sel":41,"den":80,"campep":true,"rank":10},{"short":"Harvard","borda":0.154,"lo":0.138,"hi":0.172,"sel":18,"den":80,"campep":false,"rank":11},{"short":"Vanderbilt","borda":0.136,"lo":0.113,"hi":0.159,"sel":38,"den":80,"campep":true,"rank":12},{"short":"MIT","borda":0.11,"lo":0.105,"hi":0.113,"sel":10,"den":80,"campep":false,"rank":13},{"short":"UC Berkeley/UCSF","borda":0.098,"lo":0.075,"hi":0.121,"sel":13,"den":80,"campep":true,"rank":14},{"short":"Johns Hopkins","borda":0.092,"lo":0.081,"hi":0.105,"sel":24,"den":80,"campep":true,"rank":15}],"models":[{"key":"model_01","name":"GPT-6 Astra","maker":"OpenAI","id":"openai/gpt-6-astra"},{"key":"model_02","name":"Claude Opus 5","maker":"Anthropic","id":"anthropic/claude-opus-5"},{"key":"model_03","name":"Gemini 3.8 Flash","maker":"Google","id":"google/gemini-3.8-flash"},{"key":"model_04","name":"Grok 4.6","maker":"xAI","id":"x-ai/grok-4.6"},{"key":"model_05","name":"DeepSeek V4.1","maker":"DeepSeek","id":"deepseek/deepseek-v4.1-flash"},{"key":"model_06","name":"Llama 4 Maverick","maker":"Meta","id":"meta-llama/llama-4-maverick"},{"key":"model_07","name":"Qwen3.8 Max","maker":"Qwen","id":"qwen/qwen3.8-max-0902"},{"key":"model_08","name":"Mistral Medium 3.5","maker":"Mistral","id":"mistralai/mistral-medium-3-5"}],"share":[{"q":"Graduate","slots":790,"c2":140,"pct":17.7},{"q":"Master's","slots":780,"c2":118,"pct":15.1},{"q":"Doctoral","slots":780,"c2":142,"pct":18.2},{"q":"Reworded A","slots":400,"c2":76,"pct":19.0},{"q":"Reworded B","slots":350,"c2":64,"pct":18.3}],"heat":{"short":["Wisconsin","UT MD Anderson","Duke","Chicago","Michigan","Penn","UCLA","Florida","Vanderbilt","Harvard","WashU","Stanford","Johns Hopkins","UC Berkeley/UCSF","Columbia"],"models":["GPT-6 Astra","Claude Opus 5","Gemini 3.8 Flash","Grok 4.6","DeepSeek V4.1","Llama 4 Maverick","Qwen3.8 Max","Mistral Medium 3.5"],"cells":[[10,10,10,9,10,10,10,10],[10,10,10,9,10,10,10,10],[10,10,10,9,10,10,10,10],[10,10,10,9,9,7,7,9],[0,9,1,8,10,10,10,10],[10,10,10,9,8,10,8,1],[10,5,9,8,0,10,0,6],[7,0,4,2,9,10,10,1],[10,10,10,5,2,0,5,0],[0,0,4,4,0,0,0,10],[1,10,2,5,5,10,8,0],[0,0,0,6,0,0,3,10],[0,4,10,7,0,0,0,8],[0,0,0,0,10,0,0,3],[9,6,10,0,0,0,1,0]],"axis":["GPT-6","Opus 5","Gemini 3.8","Grok 4.6","DeepSeek V4.1","Llama 4","Qwen3.8","Mistral 3.5"]},"notelig":[{"short":"Michigan","named":63,"top10":58},{"short":"U. Washington","named":23,"top10":7},{"short":"Harvard","named":18,"top10":18},{"short":"UNC Chapel Hill","named":17,"top10":10},{"short":"UC San Diego","named":14,"top10":10},{"short":"Mayo Clinic","named":13,"top10":9},{"short":"Iowa","named":13,"top10":2},{"short":"Colorado Denver","named":10,"top10":0},{"short":"Memorial Sloan Kettering","named":8,"top10":7},{"short":"Thomas Jefferson","named":8,"top10":0},{"short":"Emory","named":8,"top10":2},{"short":"Rutgers","named":8,"top10":0},{"short":"Ohio State","named":8,"top10":2},{"short":"MIT","named":6,"top10":6}],"settings":[{"name":"GPT-6 Astra","maker":"OpenAI","endpoint":"openai","temp":"not accepted","maxtok":2048,"reason":"minimal (cannot disable)"},{"name":"Claude Opus 5","maker":"Anthropic","endpoint":"anthropic","temp":"not accepted","maxtok":2048,"reason":"disabled"},{"name":"Gemini 3.8 Flash","maker":"Google","endpoint":"google-ai-studio","temp":"0.7","maxtok":2048,"reason":"minimal (cannot disable)"},{"name":"Grok 4.6","maker":"xAI","endpoint":"xai/zdr","temp":"0.7","maxtok":2048,"reason":"minimal (cannot disable)"},{"name":"DeepSeek V4.1","maker":"DeepSeek","endpoint":"together","temp":"0.7","maxtok":2048,"reason":"disabled"},{"name":"Llama 4 Maverick","maker":"Meta","endpoint":"digitalocean","temp":"0.7","maxtok":2048,"reason":"not supported"},{"name":"Qwen3.8 Max","maker":"Qwen","endpoint":"alibaba","temp":"0.7","maxtok":2048,"reason":"minimal (cannot disable)"},{"name":"Mistral Medium 3.5","maker":"Mistral","endpoint":"mistral/zdr","temp":"0.7","maxtok":2048,"reason":"disabled"}],"prompts":{"graduate":{"text":"Rank the top 15 graduate programs in medical physics in the United States.\n\nGive a numbered list from 1 to 15 containing only the institution name. Do not provide explanations, commentary, caveats, or additional text.\n","sha":"7fd91866c90f"},"master's":{"text":"Rank the top 15 master's programs in medical physics in the United States.\n\nGive a numbered list from 1 to 15 containing only the institution name. Do not provide explanations, commentary, caveats, or additional text.\n","sha":"59822c750892"},"doctoral":{"text":"Rank the top 15 doctoral programs in medical physics in the United States.\n\nGive a numbered list from 1 to 15 containing only the institution name. Do not provide explanations, commentary, caveats, or additional text.\n","sha":"2432ad8bc2f9"},"reworded A":{"text":"Which 15 graduate programs in medical physics are the best in the United States?\n\nRespond with a numbered list from 1 to 15 containing only institution names. Include no explanation or other text.\n","sha":"c6ac9222e66d"},"reworded B":{"text":"List the 15 leading U.S. graduate programs in medical physics in rank order.\n\nReturn only a numbered list of institution names from 1 to 15, with no explanation or other text.\n","sha":"a0fa3a0d81c0"}}}</script>
<script>
(function(){
  var D = JSON.parse(document.getElementById('data').textContent);
  var tip = document.getElementById('tip');
  function show(e, html){ tip.innerHTML = html; tip.style.opacity = 1;
    var x = e.clientX + 14, y = e.clientY + 14;
    if (x + 240 > innerWidth) x = e.clientX - 234;
    if (y + 90 > innerHeight) y = e.clientY - 84;
    tip.style.left = x + 'px'; tip.style.top = y + 'px'; }
  function hide(){ tip.style.opacity = 0; }
  function el(n,a,kids){ var e=document.createElementNS('http://www.w3.org/2000/svg',n);
    for(var k in a) e.setAttribute(k,a[k]);
    (kids||[]).forEach(function(c){e.appendChild(c)}); return e; }
  function txt(s){ return document.createTextNode(s); }

  /* ---------- 0. the lineup ---------- */
  (function(){
    var tb=document.querySelector('#lineup tbody');
    tb.innerHTML = D.models.map(function(m){
      return '<tr><td class="mk">'+m.name+'</td><td>'+m.maker+'</td><td class="idc">'+m.id+'</td></tr>';
    }).join('');
  })();

  /* ---------- 0b. settings table ---------- */
  (function(){
    document.querySelector('#settings tbody').innerHTML = D.settings.map(function(s){
      return '<tr><td class="mk">'+s.name+'</td><td class="idc">'+s.endpoint+'</td><td>'+
        s.temp+'</td><td>'+s.maxtok+'</td><td>'+s.reason+'</td></tr>';
    }).join('');
  })();

  /* ---------- 0c. the one-word swap ---------- */
  (function(){
    var word=document.getElementById('swapword');
    var steps=[{id:'w-graduate',w:'graduate',k:'graduate',n:'80 answers'},
               {id:'w-masters', w:'master\u2019s',k:"master's",n:'80 answers'},
               {id:'w-doctoral',w:'doctoral',k:'doctoral',n:'80 answers'}];
    var i=0, timer=null;
    var reduce = matchMedia('(prefers-reduced-motion: reduce)').matches;
    function paint(n){
      i=n; var s=steps[n];
      steps.forEach(function(t,j){ document.getElementById(t.id).setAttribute('aria-pressed', j===n?'true':'false'); });
      function set(){ word.textContent=s.w; }
      if(reduce){ set(); return; }
      word.classList.add('out');
      setTimeout(function(){ set(); word.classList.remove('out'); }, 220);
    }
    steps.forEach(function(s,j){
      document.getElementById(s.id).addEventListener('click',function(){ stop(); paint(j); });
    });
    function stop(){ if(timer){ clearInterval(timer); timer=null; } }
    /* Cycles on its own so the single-word difference is obvious at a glance,
       and stops the moment a reader takes control. */
    if(!reduce) timer=setInterval(function(){ paint((i+1)%steps.length); }, 2600);
    paint(0);
  })();

  /* ---------- 1. leaderboard ---------- */
  (function(){
    var d = D.graduate, n = d.length;
    var W=820, padL=148, padR=104, padT=26, rowH=30, H=padT + n*rowH + 34;
    var x0=padL, x1=W-padR, max=1.0;
    var sx=function(v){ return x0 + (v/max)*(x1-x0); };
    var svg = el('svg',{viewBox:'0 0 '+W+' '+H, role:'img',
      'aria-label':'Pooled score for all fifteen ranked programs; Wisconsin leads at 0.91'});
    [0,.25,.5,.75,1].forEach(function(t){
      svg.appendChild(el('line',{x1:sx(t),x2:sx(t),y1:padT-8,y2:padT+n*rowH,class:'gridline'}));
      svg.appendChild((function(){var e=el('text',{x:sx(t),y:padT+n*rowH+18,class:'axlab','text-anchor':'middle'});e.appendChild(txt(t.toFixed(2)));return e;})());
    });
    d.forEach(function(p,i){
      var y = padT + i*rowH, bh = 17, by = y + (rowH-bh)/2 - 2;
      var col = p.campep ? 'var(--yes)' : 'var(--no)';
      var g = el('g',{class:'hitrow', tabindex:'0', role:'listitem',
        'aria-label': p.short+', score '+p.borda.toFixed(3)+', named in '+p.sel+' of '+p.den+' answers'});
      var nm = el('text',{x:x0-12, y:by+13, class:'namelab','text-anchor':'end'});
      nm.setAttribute('fill', p.campep ? 'var(--ink)' : 'var(--no)');
      nm.appendChild(txt(p.short)); g.appendChild(nm);
      g.appendChild(el('rect',{x:x0, y:by, width:Math.max(2,sx(p.borda)-x0), height:bh,
        rx:4, fill:col, class:'bar'}));
      /* 95% interval */
      var a=sx(p.lo), b=sx(p.hi), cy=by+bh/2;
      g.appendChild(el('line',{x1:a,x2:b,y1:cy,y2:cy,stroke:'var(--ink)','stroke-width':1.5}));
      [a,b].forEach(function(v){ g.appendChild(el('line',{x1:v,x2:v,y1:cy-4,y2:cy+4,stroke:'var(--ink)','stroke-width':1.5})); });
      var lv = el('text',{x:b+9, y:cy+4, class:'marklab'});
      lv.appendChild(txt(p.borda.toFixed(2)+'  ('+p.sel+'/'+p.den+')')); g.appendChild(lv);
      g.addEventListener('mousemove',function(e){ show(e,
        '<b>'+p.short+'</b><br>score '+p.borda.toFixed(3)+'<br>95% CI '+p.lo.toFixed(2)+'&ndash;'+p.hi.toFixed(2)+
        '<br>named in '+p.sel+' of '+p.den+' answers<br>'+(p.campep?'CAMPEP-accredited':'NOT accredited')); });
      g.addEventListener('mouseleave',hide);
      svg.appendChild(g);
    });
    document.getElementById('c-board').appendChild(svg);
    var rows = d.map(function(p){ return '<tr><td>'+p.rank+'</td><td>'+p.short+'</td><td>'+p.borda.toFixed(3)+
      '</td><td>'+p.lo.toFixed(2)+'&ndash;'+p.hi.toFixed(2)+'</td><td>'+p.sel+'/'+p.den+'</td><td>'+
      (p.campep?'yes':'no')+'</td></tr>'; }).join('');
    document.getElementById('t-board').innerHTML =
      '<table class="tbl"><thead><tr><th>#</th><th>Program</th><th>Score</th><th>95% CI</th><th>Named</th><th>Accredited</th></tr></thead><tbody>'+rows+'</tbody></table>';
  })();

  /* ---------- 2. share by phrasing ---------- */
  (function(){
    var d = D.share, n=d.length;
    var W=820, padL=132, padR=92, padT=22, rowH=42, H=padT+n*rowH+34;
    var x0=padL, x1=W-padR, max=25;
    var sx=function(v){ return x0 + (v/max)*(x1-x0); };
    var svg = el('svg',{viewBox:'0 0 '+W+' '+H, role:'img',
      'aria-label':'Share of top-ten places going to schools with no accredited graduate program, between 15 and 19 percent for all five phrasings'});
    [0,5,10,15,20,25].forEach(function(t){
      svg.appendChild(el('line',{x1:sx(t),x2:sx(t),y1:padT-6,y2:padT+n*rowH-8,class:'gridline'}));
      var e=el('text',{x:sx(t),y:padT+n*rowH+10,class:'axlab','text-anchor':'middle'});
      e.appendChild(txt(t+'%')); svg.appendChild(e);
    });
    d.forEach(function(p,i){
      var cy = padT + i*rowH + 10;
      var g = el('g',{class:'hitrow', tabindex:'0',
        'aria-label':p.q+', '+p.pct+' percent, '+p.c2+' of '+p.slots+' top-ten places'});
      var nm=el('text',{x:x0-12,y:cy+4,class:'namelab','text-anchor':'end',fill:'var(--ink)'});
      nm.appendChild(txt(p.q)); g.appendChild(nm);
      g.appendChild(el('line',{x1:x0,x2:sx(p.pct),y1:cy,y2:cy,stroke:'var(--grid)','stroke-width':2}));
      g.appendChild(el('circle',{cx:sx(p.pct),cy:cy,r:7,fill:'var(--no)',stroke:'var(--panel)','stroke-width':2}));
      var lv=el('text',{x:sx(p.pct)+15,y:cy+4,class:'marklab'});
      lv.appendChild(txt(p.pct.toFixed(1)+'%')); g.appendChild(lv);
      var sub=el('text',{x:x0-12,y:cy+19,class:'axlab','text-anchor':'end'});
      sub.appendChild(txt(p.c2+' of '+p.slots+' places')); g.appendChild(sub);
      g.addEventListener('mousemove',function(e){ show(e,'<b>'+p.q+'</b><br>'+p.pct.toFixed(1)+
        '% of top-ten places<br>'+p.c2+' of '+p.slots+' went to a school<br>with no accredited graduate program'); });
      g.addEventListener('mouseleave',hide);
      svg.appendChild(g);
    });
    document.getElementById('c-share').appendChild(svg);
    document.getElementById('t-share').innerHTML='<table class="tbl"><thead><tr><th>Phrasing</th><th>No accredited grad program</th><th>Top-ten places</th><th>Share</th></tr></thead><tbody>'+
      d.map(function(p){return '<tr><td>'+p.q+'</td><td>'+p.c2+'</td><td>'+p.slots+'</td><td>'+p.pct.toFixed(1)+'%</td></tr>'}).join('')+'</tbody></table>';
  })();


  /* ---------- 3b. schools with no accredited graduate program ---------- */
  (function(){
    var d=D.notelig, n=d.length;
    var W=860, padL=212, padR=96, padT=22, rowH=30, H=padT+n*rowH+34;
    var x0=padL, x1=W-padR, max=Math.max.apply(null,d.map(function(p){return p.named}));
    var sx=function(v){ return x0+(v/max)*(x1-x0); };
    var svg=el('svg',{viewBox:'0 0 '+W+' '+H, role:'img',
      'aria-label':'Schools named that hold no accredited graduate program in medical physics; Michigan leads at 63 mentions'});
    [0,20,40,60].forEach(function(t){
      if(t>max) return;
      svg.appendChild(el('line',{x1:sx(t),x2:sx(t),y1:padT-8,y2:padT+n*rowH,class:'gridline'}));
      var e=el('text',{x:sx(t),y:padT+n*rowH+18,class:'axlab','text-anchor':'middle'});
      e.appendChild(txt(t)); svg.appendChild(e);
    });
    d.forEach(function(p,i){
      var y=padT+i*rowH, bh=17, by=y+(rowH-bh)/2-2;
      var g=el('g',{class:'hitrow',tabindex:'0',
        'aria-label':p.short+', named '+p.named+' times, '+p.top10+' of them in the top ten'});
      var nm=el('text',{x:x0-12,y:by+13,class:'namelab','text-anchor':'end',fill:'var(--ink)'});
      nm.appendChild(txt(p.short)); g.appendChild(nm);
      g.appendChild(el('rect',{x:x0,y:by,width:Math.max(2,sx(p.named)-x0),height:bh,rx:4,
        fill:'var(--no)',opacity:.42,class:'bar'}));
      if(p.top10>0) g.appendChild(el('rect',{x:x0,y:by,width:Math.max(2,sx(p.top10)-x0),height:bh,rx:4,
        fill:'var(--no)',class:'bar'}));
      var lv=el('text',{x:sx(p.named)+9,y:by+13,class:'marklab'});
      lv.appendChild(txt(p.named+'  ('+p.top10+' top-10)')); g.appendChild(lv);
      g.addEventListener('mousemove',function(e){ show(e,'<b>'+p.short+'</b><br>named '+p.named+
        ' times in 79 answers<br>'+p.top10+' of those reached the top ten<br>no accredited graduate program'); });
      g.addEventListener('mouseleave',hide);
      svg.appendChild(g);
    });
    document.getElementById('c-notelig').appendChild(svg);
    document.getElementById('t-notelig').innerHTML='<table class="tbl"><thead><tr><th>School</th><th>Times named</th><th>Top-ten finishes</th></tr></thead><tbody>'+
      d.map(function(p){return '<tr><td>'+p.short+'</td><td>'+p.named+'</td><td>'+p.top10+'</td></tr>'}).join('')+'</tbody></table>';
  })();

  /* ---------- 4. heatmap ---------- */
  (function(){
    var H0 = D.heat, progs=H0.short, models=H0.models, cells=H0.cells;
    var campep = {}; D.graduate.forEach(function(p){ campep[p.short]=p.campep; });
    var cw=76, ch=28, padL=150, padT=76, W=padL+models.length*cw+40, H=padT+progs.length*ch+22;
    var steps=['var(--seq-0)','var(--seq-1)','var(--seq-2)','var(--seq-3)','var(--seq-4)','var(--seq-5)'];
    function step(v){ if(v===0) return steps[0]; if(v<=2) return steps[1]; if(v<=4) return steps[2];
      if(v<=6) return steps[3]; if(v<=8) return steps[4]; return steps[5]; }
    var svg=el('svg',{viewBox:'0 0 '+W+' '+H, role:'img',
      'aria-label':'Grid of how often each model named each program in its top ten'});
    models.forEach(function(m,j){
      var cx=padL+j*cw+cw/2;
      var e=el('text',{x:cx, y:padT-10, class:'axlab','text-anchor':'start',
        transform:'rotate(-42 '+cx+' '+(padT-10)+')'});
      /* Short forms on the axis; the lineup table and the tooltips carry the full names. */
      e.appendChild(txt((H0.axis && H0.axis[j]) || m)); svg.appendChild(e);
    });
    progs.forEach(function(p,i){
      var y=padT+i*ch;
      var nm=el('text',{x:padL-12,y:y+ch/2+4,class:'namelab','text-anchor':'end',fill:'var(--ink)'});
      nm.appendChild(txt(p)); svg.appendChild(nm);
      if (campep[p]===false){
        /* A filled swatch, because orange TEXT at 12.5px on a light panel washes
           out and the grid would otherwise carry no orange at all. */
        svg.appendChild(el('rect',{x:6,y:y+ch/2-5,width:10,height:10,rx:2,fill:'var(--no)'}));
      }
      cells[i].forEach(function(v,j){
        var g=el('g',{class:'hitrow',tabindex:'0','aria-label':p+', '+models[j]+', '+v+' of 10'});
        g.appendChild(el('rect',{x:padL+j*cw+1,y:y+1,width:cw-3,height:ch-3,rx:4,fill:step(v)}));
        var t=el('text',{x:padL+j*cw+cw/2,y:y+ch/2+4,class:'marklab','text-anchor':'middle',
          fill: v>=9 ? 'var(--panel)' : 'var(--ink)'});
        t.appendChild(txt(v)); g.appendChild(t);
        g.addEventListener('mousemove',function(e){ show(e,'<b>'+p+'</b><br>'+models[j]+
          '<br>named in '+v+' of its 10 answers'); });
        g.addEventListener('mouseleave',hide);
        svg.appendChild(g);
      });
    });
    document.getElementById('c-heat').appendChild(svg);
    document.getElementById('t-heat').innerHTML='<table class="tbl"><thead><tr><th>Program</th>'+
      models.map(function(m){return '<th>'+m+'</th>'}).join('')+'</tr></thead><tbody>'+
      progs.map(function(p,i){return '<tr><td>'+p+'</td>'+cells[i].map(function(v){return '<td>'+v+'</td>'}).join('')+'</tr>'}).join('')+
      '</tbody></table>';
  })();

  /* ---------- 5. three phrasings side by side ---------- */
  (function(){
    var cols=[{k:'graduate',t:'Graduate'},{k:'masters',t:"Master's"},{k:'doctoral',t:'Doctoral'}];
    var N=15, cw=250, gap=26, padT=52, rowH=30, padL=4;
    var W=padL+cols.length*cw+(cols.length-1)*gap, H=padT+N*rowH+18;
    /* barL is the name gutter: the longest label, UC Berkeley/UCSF, measures
       107.4 units from x=19, so 126 left it touching the bar. barR shrinks by
       the same amount so the bars keep their length. */
    var barL=136, barR=38, max=1.0;
    var svg=el('svg',{viewBox:'0 0 '+W+' '+H, role:'img',
      'aria-label':'Top eight schools for each of three phrasings; the same three lead all three lists'});
    cols.forEach(function(c,ci){
      var ox=padL+ci*(cw+gap);
      var h=el('text',{x:ox,y:20,class:'namelab',fill:'var(--ink)','font-weight':'700'});
      h.appendChild(txt(c.t)); svg.appendChild(h);
      svg.appendChild(el('line',{x1:ox,x2:ox+cw-10,y1:30,y2:30,class:'axis'}));
      var x0=ox+barL, x1=ox+cw-barR;
      var sx=function(v){ return x0+(v/max)*(x1-x0); };
      D[c.k].slice(0,N).forEach(function(p,i){
        var y=padT+i*rowH, bh=16, by=y+(rowH-bh)/2-3;
        var col=p.campep?'var(--yes)':'var(--no)';
        var g=el('g',{class:'hitrow',tabindex:'0',
          'aria-label':c.t+' rank '+p.rank+', '+p.short+', score '+p.borda.toFixed(3)});
        var rk=el('text',{x:ox+13,y:by+12,class:'axlab','text-anchor':'end'});
        rk.appendChild(txt(p.rank)); g.appendChild(rk);
        var nm=el('text',{x:ox+19,y:by+12,class:'namelab'});
        nm.setAttribute('fill', p.campep?'var(--ink)':'var(--no)');
        nm.appendChild(txt(p.short.length>16?p.short.slice(0,15)+'\u2026':p.short)); g.appendChild(nm);
        g.appendChild(el('rect',{x:x0,y:by,width:Math.max(2,sx(p.borda)-x0),height:bh,rx:4,fill:col,class:'bar'}));
        var lv=el('text',{x:sx(p.borda)+7,y:by+12,class:'marklab'});
        lv.appendChild(txt(p.borda.toFixed(2))); g.appendChild(lv);
        g.addEventListener('mousemove',function(e){ show(e,'<b>'+p.short+'</b><br>'+c.t+
          ' question<br>rank #'+p.rank+'<br>score '+p.borda.toFixed(3)+'<br>named in '+p.sel+' of '+p.den+
          '<br>'+(p.campep?'CAMPEP-accredited':'NOT accredited')); });
        g.addEventListener('mouseleave',hide);
        svg.appendChild(g);
      });
    });
    document.getElementById('c-three').appendChild(svg);
    var rows='';
    for(var i=0;i<N;i++){
      rows+='<tr><td>'+(i+1)+'</td>'+cols.map(function(c){var p=D[c.k][i];
        return '<td>'+p.short+'</td><td>'+p.borda.toFixed(3)+'</td>';}).join('')+'</tr>';
    }
    document.getElementById('t-three').innerHTML='<table class="tbl"><thead><tr><th>#</th>'+
      cols.map(function(c){return '<th>'+c.t+'</th><th>score</th>'}).join('')+'</tr></thead><tbody>'+rows+'</tbody></table>';
  })();
})();
</script>
</div>
