# AI content pipeline — case study

## What this is
An end-to-end AI content workflow for a small craft brand: research, generation, editing, GEO, publishing, repurposing, and inbound lead handling. Built to demonstrate the AI Content Editor workflow (research → draft → edit → publish → repurpose → orchestrate).

Live site: https://martamani.github.io/pottery-ai-content-pipeline/
Repo: https://github.com/martamani/pottery-ai-content-pipeline

## 1. Site and content
Built a static site (GitHub Pages) from real source material: homepage, product pages, process page. Source text written by the maker (Marta).

**Screenshot:** homepage and pieces page.

## 2. GEO layer
FAQ page generated via AirOps (Quill agent), grounded in the live site content. Quill flagged missing content instead of inventing an FAQ when the site was still offline — a real example of the tool refusing to hallucinate.
Added FAQPage schema (JSON-LD) to the FAQ page for AI/answer-engine citability.

**Screenshot:** AirOps chat showing the "no content to draw from" flag. Screenshot of the FAQ page live, with schema visible in page source.

## 3. Editorial guidelines
Wrote a brand voice styleguide, grounded in the source text's actual tone (plain, first-person, undersells rather than oversells; inspired by Richard Sennett's *The Craftsman*). Uploaded into AirOps's Brand Kit so future generations reference it.

**Screenshot:** editorial-guidelines.md, and the Brand Kit "Voice & Tone" field showing it pasted in.

## 4. Content repurposing
Same site content repurposed into a LinkedIn post and Instagram caption via AirOps.

**Screenshot:** AirOps output (LinkedIn + Instagram text).

## 5. Editing pass
Manually edited the AI-generated LinkedIn post against the styleguide: cut redundant adverbs, fixed a tense mismatch, corrected a mismatched word pair, adjusted capitalization for tone. Full before/after table in `repurposed-content.md`.

**Screenshot:** the before/after table.

## 6. Styleguide validation
Tested whether the Brand Kit styleguide actually influences new output: asked AirOps to generate care instructions using the stored voice guidelines. Output matched the intended tone (plain, no spec-sheet language).

**Screenshot:** care-instructions.md and the AirOps generation.

## 7. Contact form automation — design decision: working within platform limits
Built a Tally contact form, wired to Make.com with conditional routing: messages over ~100 words route to a summarization/classification step (lead vs. not lead) before reaching a weekly email digest; shorter messages skip straight to the digest.

The summarization/classification step was built as a published AirOps Workflow, triggered via AirOps's documented webhook API. The scenario was fully wired and tested — routing logic, JSON payload structure, webhook authentication all functioned correctly. Execution was blocked by an AirOps platform restriction: external API calls require a paid tier, confirmed by the API's own error response (not a configuration error).

**Decision:** rather than pay for a tier upgrade to unlock one feature for a demo project, redesigned the flow to use AirOps's native Grid feature (pulling rows from Google Sheets) instead of an external API call — same triage logic, no paid tier required.

**Screenshots:**
- Make.com scenario canvas (Tally → Router → long/short branches → HTTP module)
- The AirOps webhook test succeeding inside AirOps's own test panel (proves the workflow logic itself works)
- The Make.com error message: `"source API is not available on the free trial or free tier"`
- Diagram: attempted architecture vs. free alternative

## Next steps (not yet built)
- Google Sheets step in Make.com (save raw submissions)
- AirOps Grid connected to that sheet, running the triage workflow per row
- Weekly digest email reading the enriched sheet
