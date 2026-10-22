# Ticket Triage System

Informacinė sistema, kuri naudoja generatyvųjį dirbtinį intelektą vartotojų komentarams paversti support tiketais.
Komentarai ateina iš įvairių kanalų: programėlių atsiliepimų, web formų, pokalbių valdiklių ir el. pašto.
DI agentas nusprendžia, ar komentarui reikia tiketo, ir jei reikia, sugeneruoja **pavadinimą**,
**kategoriją** (bug / feature / billing / account / other), **prioritetą** (low / medium / high) ir trumpą **santrauką**.

## Kaip veikia

1. **Triage** – modelis atsako `TICKET` arba `NO_TICKET`.
2. **Tiketo generavimas** – jei `TICKET`, modelis grąžina struktūrizuotą JSON, kuris patikrinamas ir normalizuojamas.
3. **Įvertinimas** – rezultatai palyginami su laukiamomis reikšmėmis ir pavaizduojami grafikais.

## Užduotys

| Užduotis | Aplankas | Turinys |
|---|---|---|
| 1.1 | [`1.1 UML`](1.1%20UML) | Projektavimo promptas, DI atsakymas, PlantUML diagramos ir jų Mermaid versijos |
| 1.2.1 | [`1.2.1 End-to-End`](1.2.1%20End-to-End) | `data/` su (X, y) poromis, `prompts/` su triage promptais, testavimo promptu ir agento atsakymu |
| 1.2.2 | [`1.2.2 Colab`](1.2.2%20Colab) | Google Colab užrašų knygelė, naudojanti Gemini API |

## Paleidimas (1.2.2)

1. Atidarykite `1.2.2 Colab/comment_to_ticket_triage.ipynb` Google Colab aplinkoje (File → Open notebook → GitHub).
2. Colab Secrets skiltyje pridėkite `GEMINI_API_KEY` (raktą galima gauti https://aistudio.google.com/apikey).
3. Paleiskite visas ląsteles: Runtime → Run all.

## Autoriai

- Neidas Šibačivoas
- Dominyk Čečenikov
