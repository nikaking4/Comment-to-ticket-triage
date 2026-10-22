# 1.2.1 End-to-End examples

## data/
`examples.json` and `examples.csv` contain 8 labelled examples.

- **X** = `comment` (plus `channel`)
- **y** = `should_ticket`, `title`, `category`, `priority`, `summary`

| id | X – comment | y |
|---|---|---|
| 1 | The app crashes every time I open it on my Pixel 8. Please fix this ASAP, I can't use it at all. | TICKET – bug / high – App crashes on launch (Pixel 8) |
| 2 | Love the app, keep up the great work! | NO_TICKET |
| 3 | I was charged twice for my monthly subscription. Please refund the duplicate payment. | TICKET – billing / high – Duplicate subscription charge |
| 4 | Great service, but it would be awesome to have a dark mode. | TICKET – feature / low – Add dark mode |
| 5 | I can't log in anymore. The password reset email never arrives, even after checking spam. | TICKET – account / medium – Password reset email not delivered |
| 6 | Thanks, the support team solved my problem quickly! | NO_TICKET |
| 7 | The export to PDF button does nothing when I click it. Other buttons work fine. | TICKET – bug / medium – Export to PDF button unresponsive |
| 8 | Ok I guess. Nothing special. | NO_TICKET |

## prompts/
| File | Purpose |
|---|---|
| `01_triage_prompt.md` | Step 1: TICKET / NO_TICKET decision |
| `02_ticket_generation_prompt.md` | Step 2: structured ticket JSON |
| `03_e2e_test_prompt.md` | Test prompt for a GPT/Claude agent (examples 1–3) |
| `03_e2e_test_response.md` | The agent's actual answer and a comparison with the expected y |
