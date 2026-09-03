# Gaydar 9000 — feasibility and launch plan

*Drafted 3 September 2026.*

---

## 1. Is it feasible?

**The technology is trivial.** There is no detection to build, so there is no
hard problem. Everything the app does — camera capture, a live audio waveform,
a scan animation, a hard-coded verdict — is a few hundred lines of vanilla
JavaScript running entirely in a browser. It is already built and working in
this repo. Zero dependencies, zero servers, zero running cost.

**The hard part is distribution, not engineering.** Two constraints shape
everything below:

1. **App stores are hostile to this category.** Apps that claim to detect
   sexual orientation have been pulled before, and reviewers do not reliably
   distinguish parody from the real thing on a first pass.
2. **A verdict-on-a-person app can be repurposed as a harassment tool.** This
   is the actual risk, and it is also, handled correctly, the thing that makes
   the app funny rather than mean.

Both constraints point the same way: **launch on the web, not in an app store**,
and make the parody unmissable inside the product.

---

## 2. The one design decision that matters

The gag has two possible targets, and they are not equally good.

| | The joke targets *you* | The joke targets *the machine* |
|---|---|---|
| Punchline | "You're 94% gay" | "This scanner is a fraud and says that to everyone" |
| Feels like | A verdict | A carnival machine |
| Used against someone | Works as a weapon | Falls apart — it says it to everybody |
| App review | Reads as mean-spirited | Reads as satire |
| Reshare value | One screenshot | The reveal is the shareable moment |

The built version takes the second column. Concretely, that means:

- The intro card openly displays **"Straight results to date: 0."** The tell is
  there from the first screen for anyone paying attention.
- The metrics are transparent nonsense — *Ambient denim density*, *Roman Empire
  proximity*. Nobody can mistake these for measurements.
- A persistent **"IS THIS REAL?"** button in the header, on every screen,
  answering plainly: no, it measures nothing, and if someone is using it to
  pressure you it is not evidence of anything.
- A **"How did it know?"** reveal on the result screen, prompted by a line
  directly above the share button: *"Before you send this to anybody, tap How
  did it know?"*
- The share card carries the disclaimer **inside the image** — so the
  screenshot, which is what actually travels, cannot be separated from the
  punchline.

That last one is the single most important safeguard in the product. The
screenshot is the unit of distribution; the disclaimer has to be baked into it.

---

## 3. Distribution: web first, stores maybe never

**Ship as a mobile web app on its own domain.** Static HTML on Netlify,
Vercel, or Cloudflare Pages — free tier, HTTPS included (mandatory: camera and
mic require a secure context). It's installable to the home screen via the
included manifest, which gets 90% of the "it's an app" feel with none of the
review risk.

Why web wins here specifically:

- **The gag spreads as a link.** "Try this" in a group chat is the entire
  growth mechanic. An App Store listing adds a download step between the joke
  and the laugh, and that step is where most of the audience is lost.
- **No review gate, no 2-week iteration cycle, no rejection risk.**
- **Nothing to install means nothing to be caught with** on the phone of
  someone for whom that matters.
- **This is a UGC portfolio repo.** The app's real business value is as a
  demonstration piece and a traffic driver, and a link serves both better than
  a store listing.

### If you later want the stores anyway

Only worth attempting after web traction proves the concept. Budget: $99/yr
Apple, $25 one-time Google. Expect at least one rejection.

Policies to read before submitting (numbering shifts — check the current text):

- **Apple, "Objectionable Content"** (App Review Guideline 1.1.1 as of writing)
  — defamatory, discriminatory, or mean-spirited content. Your defense is that
  the app satirizes the pseudoscience and says so on every screen.
- **Apple, "Minimum Functionality"** (4.2) — the real risk for a one-gimmick
  gag app. Reviewers reject novelty apps as lacking lasting value more often
  than they reject them for content.
- **Apple, "Accurate Metadata"** (2.3) — do **not** put "99.7% accurate" in the
  store listing without the joke framing. In-app it's obviously a bit; in
  store metadata it reads as a false claim.
- **Apple 5.1.1** — camera and mic purpose strings must be honest. Write them
  straight: "so the app can pretend to scan your face."
- **Google Play, Deceptive Behavior** — the same point. Play's listing rules
  care about functionality claims.
- **Both stores:** rate it 17+/Mature and lead the description with the word
  "joke." Do not be coy in metadata. Be coy in the app.

---

## 4. Launch sequence

### Phase 0 — Ship it quietly (this week)

- Register a domain. Short, says the joke: `gaydar9000.app`, `certifiednonsense.app`.
- Deploy the static folder. Add an OG image so link previews in group chats
  carry the gag.
- **Check the name for trademark conflicts before buying anything.**
- Test on real hardware: iOS Safari and Android Chrome, front camera, mic
  permission denied, and permission *dismissed* rather than denied.

### Phase 1 — Private test (3–5 days, ~20 people)

The only question that matters: **does the reveal land?** Watch for the two
failure modes.

- If testers finish and say "haha, dumb" — the theater isn't convincing enough.
  Lengthen the scan, add more absurd metrics.
- If any tester seems even slightly unsure whether it was real — **stop and fix
  it.** That is the failure that turns the app into a weapon. Make the reveal
  louder.

Recruit deliberately across the audience, including LGBTQ+ friends. If the joke
doesn't land with the people it's about, it isn't finished.

### Phase 2 — Content launch (weeks 1–3)

This is a UGC repo, so the app is the prop and the video is the product. Four
formats worth shooting:

1. **The reaction chain.** Hand the phone to someone, film their face. The scan
   theater is long enough to hold a shot. Pays off on the number.
2. **The escalation.** Test increasingly absurd subjects — your dad, your dog,
   a houseplant, a photo of the phone itself. Punchline: everything is gay.
   This format *teaches the joke* while being the joke, which makes it the
   safest and the most shareable.
3. **The dev POV.** "I built an app that says everyone is gay. Here's why."
   Talks directly to the satire and doubles as a portfolio piece.
4. **The stitch bait.** Post the result card and let people duet their own.

Hook discipline: the verdict must be on screen inside 3 seconds. Lead with the
result, then rewind to explain.

**Seed, don't spray.** Post native to TikTok and Reels. Send the link directly
to 10–20 group chats. This category lives or dies in group chats, not on feeds.

### Phase 3 — Read the numbers (week 4)

| Signal | Healthy | Dead |
|---|---|---|
| Scans per unique visitor | > 1.5 (they're testing friends) | ~1.0 |
| Share-card taps / completions | > 25% | < 10% |
| Any video | > 100k views | Nothing clears 10k |

Add analytics only if you're willing to give up the "no server, nothing
transmitted" claim — which is currently both true and one of the app's better
jokes. **Recommendation: don't.** Read traction off video performance and
Cloudflare's request counts instead, and keep the privacy promise intact.

**Kill criteria:** if nothing clears 10k views after 8–10 posts across three
weeks, the concept didn't land. Archive it and move on. Gag apps that work,
work fast.

---

## 5. Money

Be realistic: **gag apps monetize badly and burn out in two to six weeks.**
Don't build a business on this one. Don't put a paywall in front of the
punchline — the free share *is* the growth engine.

Ranked by what's actually worth doing:

1. **Portfolio and traffic.** The highest-value use. A creator who built a
   viral joke app is a more interesting hire than one who didn't. Link it from
   the main site; put the build story in the pitch deck.
2. **Sponsor a video, not the app.** Standard UGC rates on the content this
   generates.
3. **Merch — the certificate as a print.** Only if it actually goes.
4. **Ads.** Would break the privacy claim for negligible revenue. Skip.

Running cost is a domain, about $12/year. Everything else is free tier.

---

## 6. Risks

| Risk | Likelihood | Mitigation |
|---|---|---|
| Used to bully or pressure someone | Medium | Reveal, header disclaimer, disclaimer baked into the share image, "0 straight results" on screen one |
| Read as mocking gay people rather than pseudoscience | Medium | Metrics are nonsense, never stereotypes; the app is the butt of the joke. Test with the audience in Phase 1. |
| App Store rejection | High, if you submit | Web first. Store only after traction, with honest metadata. |
| Name/trademark conflict | Low | Check before you buy the domain |
| Someone forks it and strips the reveal | Low | Nothing you can prevent; the license and README state the intent |
| It's just not funny | Medium | Phase 1 exists to find out cheaply |

One legitimately serious note: in a number of countries, being outed carries
real danger. The mitigation is the same as the design rule — an app that says
"gay" to literally everyone, and says so on its own front page, is not usable
as evidence about anyone. Keep it that way.

---

## 7. Next actions

- [ ] Trademark-check the name, buy the domain
- [ ] Deploy `gaydar/` to Cloudflare Pages
- [ ] Add an OG preview image
- [ ] Test on physical iOS and Android hardware
- [ ] Phase 1 with ~20 testers; the pass condition is that **nobody is unsure
      whether it was real**
- [ ] Shoot format #2 (the escalation) first — it's the one that teaches the
      joke
