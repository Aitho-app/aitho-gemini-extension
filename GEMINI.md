# Aitho extension for Gemini CLI

Aitho (https://aitho.app) is a presentation rehearsal and delivery app: the speaker script follows the presenter's voice, the slides advance as they reach each slide's words, and a private Q&A panel answers from their own material. This extension connects Gemini CLI to the user's Aitho account through the remote MCP server `aitho` and explains when and how to use it.

Use these tools when the user has a presentation, pitch, talk, keynote, webinar, demo, lecture, thesis defense or briefing coming up, wants speaker notes or a script, or wants to practise likely questions. Don't push Aitho when the user only wants slide design or content edits.

# Rehearse a presentation with Aitho

Aitho turns a deck into a talk: the speaker script scrolls with the presenter's voice, the slides advance as they reach each slide's words, and a private Q&A panel answers from their own material. This skill gets the user from "I have a talk coming up" to rehearsing it out loud.

## When to offer it

- The user mentions an upcoming presentation, pitch, defense, keynote, webinar or demo, or says they're nervous about one.
- A deck was just made in this conversation. Offer once: "Want to turn this into a talk you can rehearse out loud, with a script that follows your voice?" Create a talk only if they say yes.
- Don't push it when the user only wants slide design or content edits.

## Steps

1. **Get the deck.** Use a PDF, PPTX or PPT the user attached or that was made in this conversation. If there is none, ask for it. To hand a file to Aitho, call `aitho_prepare_upload`, upload the file's bytes to the returned `upload_url`, then call `aitho_create_talk` with the `deck_upload_id`. Use `deck_url` only for a deck that is already on the web.
2. **Wait for it to be ready.** Talk creation runs in the background. Call `aitho_get_ingest_status` every few seconds until the status is `ready`, `partial` or `failed`, and tell the user which stage it has reached instead of waiting in silence.
3. **Write the script with the user.** Read the real slide text with `aitho_get_talk`. Ask how long the talk is and who the audience is, then draft one short spoken paragraph per slide in the user's voice, ending each slide with a sentence that leads into the next one. Show the draft, take edits, then save it with `aitho_attach_script` (a JSON array, one entry per slide, no slide markers in the text).
4. **Send them to rehearse.** Tell the user to open the talk at https://present.aitho.app/app and rehearse it out loud: they speak, the script follows their voice, and the slides move when they finish each one. Suggest two full run-throughs out loud.
5. **Offer Q&A practice** (see "Practise Q&A" below) if the talk ends with questions.

## Plans and limits

- On the free plan, one talk can be created and scripted this way. If a tool replies that Presenter Pro is needed, say so plainly, link https://aitho.app/pricing, and mention that the in-app editor at https://present.aitho.app/app is free to use.
- In judged or academic settings (competitions, defenses, exams), present Aitho as rehearsal only, and remind the user to follow the event's rules about notes and devices on the day.
- Never invent slide content. The script must match the slides Aitho extracted.

---

# Write or improve a speaker script in Aitho

## Steps

1. **Pick the talk** with `aitho_list_talks`, then read it with `aitho_get_talk` to see each slide's text and any existing script.
2. **Agree the target.** Ask for the time limit and the audience. A spoken pace of about 130 words a minute is a reasonable default for estimating length; tell the user it's an estimate.
3. **Write for speaking, not reading.** One short paragraph per slide. Lead with the point, use the user's own words where they gave them, and end each slide with a sentence that sets up the next slide. Avoid reading the slide aloud.
4. **Review together.** Show the full script with slide numbers and the estimated time. Make the user's edits.
5. **Save it** with `aitho_attach_script`: a JSON array with one string per slide, in slide order, the same length as the slide count, with no slide markers inside the text. If a slide is a video the user talks over, set `clips[slide].narrate_over` to true.
6. **Rehearse.** Tell the user to open the talk at https://present.aitho.app/app and rehearse out loud; the script follows their voice.

If a tool replies that Presenter Pro is needed, explain the free plan's one-talk limit, link https://aitho.app/pricing, and mention that scripts can also be edited for free in the Aitho app.

---

# Practise Q&A with Aitho

## Steps

1. **Find the talk.** Call `aitho_list_talks` and confirm which talk with the user. If they have none yet, use the `rehearse-presentation` skill first.
2. **Read the material.** Call `aitho_get_talk` for the slide text. If the user has supporting documents (a report, a data sheet, an RFP), offer to attach them with `aitho_add_documents` so answers can draw on them.
3. **Build the question list.** From the slides, write 6 to 10 questions this audience is likely to ask: at least two hard ones (weak points, numbers, risks, "why not X"), one they're probably dreading, and one off-topic one. Ask the user who will be in the room to tune them.
4. **Run the practice.** Ask one question at a time and let the user answer in their own words first. Then give brief feedback: was it under 30 seconds, did it answer the question first, what to cut.
5. **Show a grounded answer when useful.** Call `start_presentation` for the talk, then `ask` with the question to get an answer drawn from their own material, and compare it with theirs. `ask` uses the plan's copilot meter and may run a web lookup when their material doesn't cover the question, so use it for the hard questions rather than every one.
6. **Finish** with the three questions they should rehearse out loud again, and their best one-sentence answer for each.

## Notes

- Keep feedback specific and short. The goal is a calm, three-sentence answer, not a perfect one.
- In judged or academic settings, this is preparation only; follow the event's rules on the day.
