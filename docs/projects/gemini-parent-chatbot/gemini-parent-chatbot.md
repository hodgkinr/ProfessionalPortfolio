---
layout: page
title: Github Chatbot
nav_exclude: true
---


# Local Gemini Chatbot for Parents

## Project Summary

The Local Gemini Chatbot for Parents project provides a locally deployable AI chatbot designed to support AI literacy, exploration, and experimentation in a privacy-conscious environment. Built using Python, Flask, and Google’s Gemini API, the system runs entirely on a local machine or controlled development environment, allowing users to interact with a modern AI model without relying on opaque, cloud-hosted consumer interfaces.

This project supports the BOBPE mission by extending bot-based personalized education beyond formal classrooms and into family, community, and public learning contexts.

---

## Educational Problem Addressed

Parents and members of the public are increasingly aware that AI tools are shaping education, work, and daily life, yet many lack opportunities to explore these systems in a transparent and low-risk setting. Commercial AI platforms often obscure how data is stored, how responses are generated, and what controls users actually have.

This project addresses the need for:
- Trustworthy, hands-on AI literacy experiences
- Clear boundaries around privacy and data ownership
- Opportunities to explore AI capabilities without institutional barriers

---

## How AI Is Used

The chatbot uses Google’s Gemini model to:
- Respond to user questions in real time
- Stream responses incrementally for a conversational experience
- Support Markdown-formatted outputs, including code blocks
- Enable reflective exploration of AI behavior and limitations

The system is intentionally general-purpose, allowing users to ask questions, test prompts, and explore how AI responds across domains.

---

## Privacy and Local Deployment

A central design goal of this project is transparency and control:

- The application runs locally using Flask
- Chat transcripts are stored in a local SQLite database
- Users can view, delete, and export their own chat history
- No external logging or analytics are required beyond the API call itself
- The system can be run in a local Python environment or a controlled Codespace

This makes the project well-suited for demonstrations, workshops, and home use where privacy and trust are paramount.

---

## Example Use Cases

- Parents exploring how AI tools respond to homework-related questions
- Families discussing appropriate and inappropriate AI use together
- Community workshops on AI literacy and responsible use
- Demonstrations of prompt design and response variability
- Conversations about data privacy, limitations, and guardrails

---

## Deployment Status

**Status:** Active demonstration tool  
The system is used as part of AI literacy workshops and community discussions, and serves as a reference implementation for locally controlled AI exploration.

---

## Artifacts and Links
### Example UI Walkthrough Video

<iframe src="https://drive.google.com/file/d/17TpmM2Ik7M4Bz_SNP4mkohsUsKqEzrTF/view?usp=sharing"
        width="700"
        height="394"
        allow="autoplay"
        style="max-width: 100%;">
</iframe>

- [GitHub repository](https://github.com/hodgkinr/basic-google-chat)

---

## Alignment with BOBPE Mission

This project extends the BOBPE vision of bot-based personalized education into public and family-facing contexts. By prioritizing transparency, local control, and hands-on exploration, it supports AI literacy and trust-building while demonstrating how modern AI systems can be engaged responsibly outside of formal instructional settings.
