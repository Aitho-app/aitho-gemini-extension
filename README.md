# Aitho for Gemini CLI

Rehearse and deliver presentations with your own slides. This extension connects Gemini CLI to [Aitho](https://aitho.app), a presentation rehearsal and delivery app, and tells Gemini when and how to use it.

## What it does

- Turns a PDF or PowerPoint deck into an Aitho talk, drafts a speaker script with you slide by slide, and sends you to rehearse it out loud. In Aitho the script scrolls with your voice and the slides advance as you reach each slide's words.
- Builds the questions your audience is likely to ask from your own slides and runs a mock Q&A.
- Writes or tightens a script for spoken delivery and a time limit, then saves it to your talk.

## How to use it

Install it with `gemini extensions install https://github.com/Aitho-app/aitho-gemini-extension`, then sign in to your Aitho account when Gemini CLI asks you to authenticate the `aitho` server (or run `/mcp auth aitho`). Then just say what's coming up, for example "I have a 10-minute pitch on Thursday, help me rehearse it" or "make me speaker notes for this deck".

The free plan lets you create and script one talk through Gemini CLI. More talks through Gemini CLI need Presenter Pro; the Aitho app's own editor is free.

## Data and privacy

The extension contains no code that runs on your machine. It uses one remote connector, `https://present.aitho.app/mcp`, which signs you in to your own Aitho account with OAuth. When you ask Gemini to use Aitho, Gemini CLI sends the deck file, the speaker script and any Q&A documents you choose to Aitho, and Aitho stores them in your account. The `ask` tool may run a web lookup when your own material doesn't cover a question. Aitho acts only on your own talks. Privacy policy: https://present.aitho.app/legal#privacy

## Support

https://aitho.app/support
