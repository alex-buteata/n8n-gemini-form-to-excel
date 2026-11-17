# n8n-gemini-form-to-excel
# Clasificare automată a mesajelor din formular și salvare în Excel (n8n + Gemini)

Acest proiect este un flux de automatizare realizat în n8n care preia mesajele trimise printr-un formular web, le analizează cu Google Gemini, filtrează spam-ul și salvează cererile valide într-un fișier Excel local (`.xlsx`), păstrând tot istoricul anterior.

## Arhitectura fluxului
<img width="1382" height="603" alt="image" src="https://github.com/user-attachments/assets/e1c5761d-6739-4164-94a5-34e9751c709b" />

## Cum funcționează

1. **Preluarea datelor (`On form submission`):** colectează numele, adresa de email și mesajul completate în formular.
2. **Analiza cu AI (`Basic LLM Chain` + `Google Gemini Chat Model`):**
   - citește mesajul și îl încadrează într-una dintre categoriile: `Oferta`, `Suport` sau `Spam`;
   - extrage un rezumat scurt al solicitării;
   - returnează datele într-un format JSON fix, impus prin nodul `Structured Output Parser`.
3. **Filtrarea spam-ului (`If`):** lasă să treacă mai departe doar mesajele valide (`categorie != Spam`), oprind mesajele irelevante pe ramura `false`.
4. **Curățarea coloanelor (`Edit Fields`):** redenumește câmpurile primite de la AI în coloane simple pentru Excel (`Nume`, `Email`, `Categorie`, `Rezumat`).
5. **Salvarea în Excel fără suprascriere (`Read File` -> `Extract From XLSX` -> `Merge` -> `Convert to XLSX` -> `Write File`):**
   - citește înregistrările existente din fișierul `File.xlsx`;
   - adaugă rândul nou la finalul tabelului prin nodul `Merge` (modul `Append`), ca să nu se piardă datele vechi;
   - rescrie fișierul `.xlsx` actualizat pe disc.
<img width="1420" height="219" alt="image" src="https://github.com/user-attachments/assets/1d2ffd66-2c2a-4e25-8127-1de5c94e490f" />
