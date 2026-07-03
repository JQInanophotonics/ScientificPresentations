# 00 — Structuring a talk

Before any artboard gets opened, decide what the talk is actually going to say. This page is about that — the other pages ([01](01-Artboards.md)–[04](04-UsingBeamer.md)) are about building and placing the figures once you know what story they need to tell.

## Interest the audience — that is the actual job

A presentation, like a paper, has to be appealing — more so, since there's no abstract for someone to skim before deciding whether to keep listening. Settle these two things before drafting a single slide, and keep re-checking your draft against them:

- **Tell them why they're here.** Not "why does this topic matter to the field" in the abstract — why should *this* audience, in *this* room, want to give you their attention for the next several minutes. A hook that only justifies the topic in general still risks leaving the room polite but unmoved.
- **Respect how much they can actually absorb.** A talk is consumed **once, linearly, at a pace you control** — no rereading, no skipping ahead to the figure they care about, unlike a paper. A paper can carry every control experiment and every derivation; a 12-minute talk has room for two or three ideas. If a slide needs two minutes to explain, it's actually two slides, or it doesn't belong in this talk. Keep only the results and points that matter to the story you're telling — everything else is a paper, a poster, or a backup slide (see below).

## A shape you can borrow: hook → tension → idea → evidence → payoff

This isn't handed down from anywhere authoritative — it's the pattern that falls out of reading the reference talk ([Example — CLEOus](Example-CLEOus.md)) section by section. It happens to resemble "And, But, Therefore" (ABT), a setup/complication/resolution structure taught in some science-communication training — worth knowing that name exists if you want to read further, but the evidence for it here is simply that it's what a real, well-received talk from this lab actually does, not a claim that it's the only shape or the right one for every talk:

1. **Hook** — ground the audience in why the general topic matters, before your specific contribution shows up at all. CLEOus opens not with the authors' own result, but with optical frequency combs in general and their three textbook applications (frequency synthesis, low-noise microwave generation, clockwork) — context the whole room already cares about. This is the mechanism for "tell them why they're here": start from something the audience already values, then narrow to your work.
2. **Tension** — state, plainly, what the field has been stuck on. Not "our result is nice," but "here is a real bottleneck, and it has resisted solutions." CLEOus: shrinking a comb onto a chip lowers per-tooth power, which makes carrier-envelope-offset (CEO) detection — needed for every application from step 1 — very hard. The slide literally ends on `\bad{CEO detection VERY hard with on-chip OFC}`.
3. **The idea** — introduce your approach as the response to that specific tension, not as a free-standing achievement. CLEOus pivots explicitly: *"Rethinking microcomb beyond shrinking... CEO encoded rather than post-generation detected"* — the idea is framed as attacking the exact bottleneck just stated, not as a new topic.
4. **Evidence, including the false start.** CLEOus doesn't hide that the first χ⁽³⁾ PDCS demonstration wasn't good enough — a slide is spent on it, ending with `\badbox{Not readily useful: (1) does not span an octave (2) pumps are not comb teeth}` — before the next slide shows the fix (adding the self-alignment condition). Showing the dead end first makes the eventual result land harder, and it's honest about how the work actually went.
5. **Payoff, bookended back to the hook.** The second-to-last slide, "Metrological use," revisits the *exact same three applications* from the opening slide — synthesis, microwave generation, clockwork — and shows the new architecture satisfying each. Opening and closing on the same list is what makes the talk feel like it resolved something, instead of just stopping.
6. **Conclusion** — restate the bottleneck, restate the architecture, then perspectives (what's next, who's talking about it tomorrow). Short; the audience already has the story, this is the recap they'll remember.

## The `\good`/`\warn`/`\bad` macros are how you mark this on-slide

[04 — Using Beamer](04-UsingBeamer.md) documents `\good{...}`, `\warn{...}`, `\bad{...}` and their box forms as a color/semantic system — but they exist for exactly this narrative purpose. Use them to make the tension/resolution structure visible in real time, not just in your speech:

- `\bad{...}` — mark the bottleneck, the dead end, the thing that doesn't work (yet). CLEOus: `\bad{Does not solve our metrology problem (yet)}`.
- `\good{...}` — mark the result, the thing that now works. CLEOus: `\good{Self-aligned PDCS across entire wafer}`.
- `\warn{...}`/`\warnbox{...}` — mark a caveat, a condition, an open question — something the audience should hold onto without it being a full stop.

An audience that's skimming a slide (which is most of an audience, most of the time) can read the color-coded phrases alone and follow the argument's shape even before you finish the sentence.

## What doesn't fit the story goes to backup

CLEOus has five appendix frames after the bibliography — simulations, coupling-space details, a second self-alignment demonstration, ring-radius engineering, the full experimental setup (see [Example — CLEOus, "Backup slides"](Example-CLEOus.md#backup-slides)) — real, relevant material that would have slowed the main narrative down or answered a question nobody in the room ended up asking. None of it needed the animation/reveal treatment from [03 — Animations](03-Animations.md); it's there to be pulled up if someone asks, not to be walked through.

If you're cutting a result from the main talk and hesitating because "but it's important" — it probably *is* important, which is exactly why it belongs in backup rather than cut entirely. The main narrative only has room for what the story needs to move forward.

## A short checklist

- Does your opening give the audience a reason to want to listen, or does it just state the topic?
- Can you state the tension/bottleneck in one sentence, before you've mentioned your own result?
- Does your idea slide explicitly answer that sentence, or does it just start a new topic?
- If you have a result that didn't pan out on the way there, does the talk show it (briefly) rather than hide it?
- Does the ending return to something from the opening — a question, an application, a number — so the talk resolves instead of just stopping?
- For each slide: is this one idea, or is it secretly two? If two, split it or cut one.
- Everything that's correct but not load-bearing for the story: backup slide, not main narrative.

Next: [01 — Artboards](01-Artboards.md)
