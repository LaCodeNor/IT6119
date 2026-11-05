# Klassifisering av søvn og våken fra smartklokke-data

Maskinlæringsprosjekt som klassifiserer 30-sekunders epoker som våken eller søvn fra akselerasjon og puls fra Apple Watch, med polysomnografi (PSG) som referanse. Prosjektet er en sluttoppgave i emnet IT6119 ved NTNU.

## Datasett

PhysioNet «Motion and heart rate from a wrist-worn wearable and labeled sleep from polysomnography», versjon 1.0.0 (31 deltakere, University of Michigan):

- Akselerasjon (g), tre akser. Samplingraten varierer mellom deltakere (10,0–66,0 Hz i vårt utvalg).
- Puls (slag/min), målt med fotopletysmografi hvert få sekund.
- PSG-stadier per 30-sekunders epoke (våken, N1, N2, N3, REM).

Rådataene (ca. 2,2 GB) er **ikke** en del av repositoriet. Last dem ned fra https://doi.org/10.13026/hmhs-py35 og legg mappene `labels/`, `motion/` og `heart_rate/` ved siden av notebooken.

## Innhold

| Fil eller mappe | Innhold |
|---|---|
| `final_project_laho.ipynb` | Hele analysen: kvalitetskontroll, epoketabell, evaluering og figurer |
| `figures/` | Figurene som brukes i rapporten |
| `requirements.txt` | Pakkeversjoner |
| `.gitignore` | Utelater rådata, avledede filer og Jupyter-hjelpefiler |

## Kjøring

```
pip install -r requirements.txt
```

Åpne notebooken og kjør alle celler fra topp til bunn (Kernel → Restart & Run All). Notebooken leser datamappene fra samme mappe (`DATA_DIR = '.'`) og bruker `SEED = 42`. Pipelinen tar noen minutter fordi akselerometerfilene er store.

## Analyse

1. **Kvalitetskontroll.** Per deltaker beregnes dekning (andel skårede epoker med akselerometerdata), samplingrate, største hull og tidsstempelrekkefølge. Deltakere med dekning under 0,5 ekskluderes (én deltaker, 7749105, med dekning 0,126).
2. **Epoketabell.** De tre kildene kobles på 30-sekundersepoker. Målvariabel: våken (0) mot søvn (1). Stadium −1 fjernes (uskåret). Stadium 4 forekommer hos tre deltakere, er ikke definert i datasettbeskrivelsen og regnes som søvn.
3. **Features (23).** Akselerasjonsvarians (log-transformert), gjennomsnitt og maksimum, pulsgjennomsnitt og pulsstandardavvik per epoke, rullerende gjennomsnitt og standardavvik over 5, 11, 21 og 41 epoker innenfor hver deltaker, og timer siden PSG-start.
4. **Evaluering.** Deling på deltakernivå: 24 deltakere til utvikling og 6 deltakere som testsett. Kryssvalidering (5 folds) gruppert på deltaker, gjentatt med fem tilfeldige foldinndelinger. Modeller: majoritetsklasse, logistisk regresjon og gradient boosting.
5. **Beslutningsterskel.** Valgt i kryssvalideringen: den terskelen som gir sensitivitet 0,90 for søvn (P(søvn) = 0,463).
6. **Testsett.** Evaluert én gang med fastsatt modell, features og terskel. 95 %-intervaller med bootstrap over deltakere.

Analysen omfatter 30 deltakere og 25 828 epoker, hvorav 8,6 % er våkne.

## Resultater

Kryssvalidering på de 24 utviklingsdeltakerne (gjennomsnitt over fem foldinndelinger):

| Modell | Balansert accuracy | Sensitivitet (søvn) | Spesifisitet (våken) | Kappa |
|---|---|---|---|---|
| Majoritetsklasse | 0,500 | 1,000 | 0,000 | 0,000 |
| Logistisk regresjon, enkeltepoker | 0,671 | 0,907 | 0,434 | 0,285 |
| + kontekst 5–41 epoker | 0,766 | 0,883 | 0,649 | 0,378 |
| + kontekst 5–41 epoker og timer siden PSG-start | 0,785 | 0,885 | 0,685 | 0,402 |

Endelig modell på testsettet (6 deltakere), ved terskel P(søvn) = 0,463 (95 % bootstrap-intervall):

| Mål | Verdi |
|---|---|
| Balansert accuracy | 0,763 [0,665–0,825] |
| Sensitivitet (søvn) | 0,914 [0,892–0,941] |
| Spesifisitet (våken) | 0,612 [0,398–0,747] |
| Kappa | 0,434 [0,302–0,524] |

Spesifisiteten varierte fra 0,161 til 0,920 mellom testdeltakerne. Se rapporten for tolkning og begrensninger.

## Begrensninger

- 30 unge, friske deltakere og én natt per person.
- 8,6 % av epokene er våkne. Fem deltakere har under 30 våkne epoker.
- Samplingraten varierer uten dokumentert årsak. Stadium −1 og 4 er ikke definert i datasettbeskrivelsen.
- Sentrerte tidsvinduer bruker fremtidige epoker og passer for etterberegning, ikke sanntid.
- Prosjektet er en undervisningsøvelse. Modellen er ikke klinisk validert og er ikke et medisinsk utstyr.

## Referanser

1. Resource Citation

Walch, O. (2019). Motion and heart rate from a wrist-worn wearable and labeled sleep from polysomnography (version 1.0.0). PhysioNet. RRID:SCR_007345. https://doi.org/10.13026/hmhs-py35

2. Original Publication

Olivia Walch, Yitong Huang, Daniel Forger, Cathy Goldstein, Sleep stage prediction with raw acceleration and photoplethysmography heart rate data derived from a consumer wearable device, Sleep https://doi.org/10.1093/sleep/zsz180

3. Platform Citation

Pollard, T., Moody, B. E., Lehman, L., Gow, B., Fernandes, C., Xie, C., Johnson, A., Mark, R. G., & Heldt, T. (2026). PhysioNet as a global platform for biomedical research. Nature Health. https://doi.org/10.1038/s44360-026-00096-z. Available from: https://pmc.ncbi.nlm.nih.gov/articles/PMC13641116/

## Lisens

Kildedata: Open Data Commons Attribution License (ODC-By) v1.0.
Kode: [velg lisens, for eksempel MIT, eller skriv «alle rettigheter forbeholdt»].