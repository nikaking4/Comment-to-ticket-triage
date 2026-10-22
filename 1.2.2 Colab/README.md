# 1.2.2 Colab prototype (Gemini API)

`comment_to_ticket_triage.ipynb` – working prototype of the triage agent in Google Colab.

- Reads the labelled examples from `1.2.1 End-to-End/data/examples.json`
- Step 1: Gemini decides TICKET / NO_TICKET
- Step 2: Gemini returns ticket JSON, which is validated and normalised
- Evaluates the results against the expected y and plots decisions, categories and priorities

**Run:** open in Colab (File → Open notebook → GitHub), add the `GEMINI_API_KEY` secret, Runtime → Run all.
