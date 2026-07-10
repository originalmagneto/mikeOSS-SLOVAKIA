# Strategický report: Implementácia open-source AI pre slovenskú advokáciu (MikeOSS Slovakia)

### 1. Manažérske zhrnutie (Executive Summary)
V roku 2026 prestal byť LegalTech doménou experimentov a stal sa integrálnou súčasťou infraštruktúry moderných kancelárií. Investície do sektora prekročili hranicu 1 miliardy USD už počas druhého dňa konferencie Legalweek 2026, čo signalizuje definitívny nástup „agentickej éry“ (Agent Era). Trh sa posunul od asistenčných chatbotov k autonómnym systémom schopným vykonávať komplexné úlohy end-to-end.

Pre MikeOSS Slovakia je v tomto prostredí kritická **dátová suverenita**. Strategický cieľ je vybudovať ekosystém, ktorý kombinuje vysoký výkon generatívnej AI s lokálnou kontrolou nad dátami. Úspešná implementácia pre slovenských advokátov stojí na troch pilieroch:
1.  **Bezpečnosť a suverenita:** Prioritizácia self-hosting riešení a lokálnej indexácie (Local-first).
2.  **Agentická automatizácia:** Prechod k multi-agentovým workflow (Reasoning-Action-Evaluation) s využitím Model Context Protocol (MCP).
3.  **Regulačná zhoda:** Striktná aplikácia Metodického usmernenia SAK z roku 2025 a požiadaviek EU AI Act.

### 2. Prehľad free a open-source AI riešení (2025 – 2026)

| Názov projektu | Hlavná funkcia | Licencia | EU/Self-host | Stav údržby (2025-2026) |
| :--- | :--- | :--- | :--- | :--- |
| **MikeOSS** | Dokumentový asistent (Chat s PDF) | Open Source | Áno (S3/R2) | Aktívny (mikeoss.com) |
| **Stella** | Open-source právny workspace | Open Source | Áno (TypeScript) | Aktívny |
| **MasKIT** | Anonymizácia/Pseudonymizácia | Open Source | Áno (REST API/Local) | Aktívny (verzia 0.70) |
| **SaulLM** | Špecializovaný právny LLM | MIT | Áno | Aktívny (štandard 54B/141B) |
| **Lawma** | Klasifikácia právnych úloh | Open Source | Áno | Aktívny (Llama 3.2 base) |
| **EmuBERT** | Model pre regionálnu legislatívu | Open Source | Áno | Aktívny (RoBERTa-based) |
| **Sound Suite** | Local-first indexácia a search | Polyform NC | Áno (Offline) | Aktívny |
| **adeu** | Agentické redigovanie DOCX | Open Source | Áno | Aktívny (MCP Server) |
| **pasal** | RAG pre legislatívu | Open Source | Áno | Aktívny (Funkčný vzor pre SR) |

### 3. Anonymizácia a pseudonymizácia dokumentov: MasKIT a Stella
Nástroj **MasKIT** predstavuje technologický základ pre bezpečnú prácu s citlivými právnymi textami v slovenskom a českom prostredí.

*   **Architektúra:** MasKIT využíva externé služby **UDPipe** na hĺbkovú syntaktickú analýzu postavenú na rámci „Universal Dependencies“ a **NameTag** na rozpoznávanie pomenovaných entít (NER). Táto kombinácia umožňuje systému chápať gramatické vzťahy a presne identifikovať subjekty.
*   **Režimy spracovania:**
    *   **Anonymizácia:** Nahradenie údaja abstraktnou triedou (napr. *M-ZENA-MENO-1*), čo zachováva logickú štruktúru dokumentu.
    *   **Pseudonymizácia:** Nahradenie údaja náhodným menom rovnakého typu pri zachovaní morfologického pádu a rodu.
*   **Efektivita a známe limity:** MasKIT vykazuje **Recall 0.8** a **Precision 0.64**. Z pohľadu AI architekta je nutné upozorniť na aktuálnu verziu, ktorá **neimplementuje automatickú detekciu akademických titulov, osobných ID čísiel (rodné čísla)**, IBAN kódov a dátových schránok. Tieto prvky vyžadujú manuálnu kontrolu alebo dodatočné regulárne pravidlá.
*   **Integrácia:** V rámci projektu MikeOSS Slovakia sa MasKIT integruje ako middleware do workspace **Stella**, čím vytvára bezpečný „sanitizačný filter“ pred odoslaním dát do LLM.

### 4. Architektúra MCP serverov pre právnu prax
**Model Context Protocol (MCP)** je kľúčovým štandardom roku 2026, ktorý slúži ako most medzi AI a právnymi dátami bez nutnosti ich fyzického presúvania.

*   **Integrácia Claude:** Moderné implementácie využívajú **Claude Agent SDK** a **Claude CoWork**, ktoré umožňujú modelu priamo volať lokálne nástroje cez MCP.
*   **Zero Document Downloads:** Podobne ako systémy DeepJudge alebo NetDocuments, aj MikeOSS Slovakia presadzuje princíp, kde dáta zostávajú v pôvodnom úložisku. AI cez MCP pristupuje len k indexu a potrebným fragmentom.
*   **Navrhované MCP servery pre MikeOSS Slovakia:**
    *   **Slov-Lex MCP:** Dynamické sťahovanie a sémantické vyhľadávanie v zákonoch SR.
    *   **ORSR/RPVS MCP:** Verifikácia subjektov a vlastníckych štruktúr v reálnom čase.
    *   **Judikatúra SR MCP:** Prístup k databáze súdnych rozhodnutí s automatickým overovaním citácií (citation verification).

### 5. Agentické zručnosti (Skills) a koncept Glass Box
AI agenti v roku 2026 využívajú „agentický loop“ (Reasoning -> Action -> Evaluation), ktorý zrkadlí kognitívny proces advokáta:

1.  **READ:** Ingestácia neštruktúrovaných informácií (skeny, e-maily).
2.  **THINK:** Aplikácia doménových znalostí a interných štandardov kancelárie.
3.  **WRITE:** Produkcia štruktúrovaného výstupu (redline zmluvy, právne memo).
4.  **VERIFY (Glass Box):** Kritický krok kontroly. MikeOSS Slovakia implementuje „glass box“ prístup (vzor Mary Technology), kde je každý krok agenta, každá citácia a každá redakcia auditovateľná a spätne dohľadateľná. Tým sa eliminuje „black-box“ efekt generatívnej AI.

**Nástroje a kategórie (2026):**
*   **ACM (AI Contract Management):** Systémy ako **Spellbook** (funkcia ACM) umožňujú napájanie zmlúv end-to-end od draftu po podpis.
*   **Harvey Vault:** Hromadná analýza dokumentov pre due diligence.
*   **Clio Manage AI:** Automatická extrakcia lehôt zo súdnych podaní.
*   **Supio:** Tvorba lekárskych chronológií pri náhrade škody na zdraví.

### 6. Hybridná infraštruktúra a LLM Orchestration
MikeOSS Slovakia stavia na **Model Agnostic Architecture**, ktorá umožňuje dynamicky prepínať medzi modelmi podľa citlivosti úlohy.

*   **LLM Gateway (LiteLLM / OpenRouter):** Centrálny bod pre riadenie API požiadaviek s využitím princípu **BYOK (Bring Your Own Key)**.
*   **Lokálny hosting (Ollama / SaulLM):** Pre maximálnu dôvernosť spracovania vysoko citlivých dát v lokálnej sieti.
*   **Model MARC (TCDI/Altorney):** Vzor pre nasadzovanie AI za firewallom klienta, kde AI spracováva dáta lokálne pred akýmkoľvek cloudovým prenosom.
*   **Ekonomika:** Prechod na **Consumption-Based Pricing** (vzor Legora Agent Pro) umožňuje kanceláriám presne alokovať náklady na AI na konkrétne klientske spisy.

### 7. Zhoda s predpismi a dátová suverenita (SAK 2025)
Podľa **Metodického usmernenia SAK (2025)** nesie advokát plnú zodpovednosť za výstupy AI. Klúčovým etickým pilierom je prevencia **„Automation Bias“** – tendencie nekriticky dôverovať automatizovaným systémom na úkor vlastného odborného úsudku.

**Trojstupňový rozhodovací test SAK pred vložením dát:**
*   **Fáza 1 (Režim spracovateľa):** Analýza technického režimu. Ak existuje zmluvná garancia proti trénovaniu modelu (napr. profesionálne API), advokát pracuje v bezpečnej zóne.
*   **Fáza 2 (Anonymizácia):** Použitie nástrojov ako MasKIT. Ak je anonymizácia úspešná, mlčanlivosť nie je dotknutá a súhlas klienta nie je potrebný.
*   **Fáza 3 (Doložka o zbavení mlčanlivosti):** Ak nie je možné dáta anonymizovať a používajú sa rizikové verejné nástroje, advokát musí získať výslovný súhlas klienta ako **poslednú možnosť (last resort)**.

### 8. Strategické odporúčania pre MikeOSS Slovakia
Pre úspešnú realizáciu odporúčame tímu nasledujúce kroky:

1.  **Prioritizácia Local-first indexácie:** Postaviť vyhľadávanie na architektúre **Sound Suite** (licencia Polyform NC), kde indexácia prebieha lokálne a offline.
2.  **Vývoj Slov-Lex MCP wrapperu:** Vytvoriť vlastný konektor pre slovenskú legislatívu inšpirovaný architektúrou **Vaquill AI MCP**, kombinujúci sémantické vyhľadávanie s verifikáciou citácií.
3.  **Implementácia Audit-ready logov:** Každý agentický loop musí generovať transparentný záznam o uvažovaní (reasoning trace), aby bola splnená požiadavka transparentnosti podľa EU AI Act.
4.  **Kontinuálne vzdelávanie:** Vynucovať verifikačný krok (VERIFY) ako súčasť firemnej metodiky, čím sa predchádza disciplinárnej zodpovednosti za AI halucinácie.

Tento report definuje cestu k technologicky vyspelej, ale eticky a právne bezpečnej slovenskej advokácii 21. storočia.