# ElregelAI

## Vad är detta?
ElregelAI är en RAG-baserad AI-assistent för svenska elregelverket. Den hjälper elingenjörer och besiktningsingenjörer att ställa frågor om lagar, föreskrifter, standarder och handböcker — och får svar med källhänvisning.

## Live
- Webb: https://mikael-jpg.github.io/elregel-ai
- Hosting: GitHub Pages (main branch)

## Teknikstack
- **Frontend:** En enda `index.html` (HTML + CSS + vanilla JS)
- **Databas:** Supabase med pgvector (tabell: `regeltext`, ~4503 poster)
- **Embeddings:** OpenAI text-embedding-3-small
- **AI-svar:** Anthropic Claude (claude-sonnet-4-20250514)
- **Sökfunktion:** Supabase RPC `sok_regler` med vektorsökning

## Arkitektur
1. Användaren skriver en fråga (+ valfritt installationsår)
2. Frågan skickas till OpenAI för embedding
3. Embedding söker i Supabase via `sok_regler` — prioriterar lag > föreskrift > standard > handbok > besiktning
4. Hämtade regeltexter skickas som kontext till Claude med systemprompt
5. Claude svarar med bedömning (A1/A2/B/EJ BRIST), regelgrund och konkreta krav

## Supabase
- URL: https://rnnjnnulqdgbwghaxbdg.supabase.co
- API-nycklar laddas från Supabase Storage: `app/config.json` (bucket: "app")
- Lager i databasen: lag, foreskrift, standard, handbok, expert, qa, besiktning

## Viktiga regler för AI-svaren
- STRÄNGASTE tolkning — alltid vid nya installationer
- Hänvisa BARA till avsnitt som finns i hämtad kontext — hitta aldrig på avsnittsnummer
- Svara alltid på svenska
- Format: Bedömning → Gäller från → Regelgrund → Konkreta krav
- Källhierarki: Lag > Föreskrift > Standard > Handbok > Besiktningserfarenhet

## Användare
- Mikael (besiktningsingenjör, Total Elkontroll)
- Robin (VD, Svenska Elanläggningar)
- Daniel

## Vid ändringar
- All kod finns i `index.html`
- Push till `main` → GitHub Pages uppdateras automatiskt
- Testa alltid inloggning + en fråga efter ändring
