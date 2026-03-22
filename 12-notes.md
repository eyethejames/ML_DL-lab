a. Dense-modell
Det svakeste resultatet.
- training MAE går litt ned, validation MAE hopper voldsomt opp og ned
- særlig stor spike på epoch 8
tyder på at modellen lærer noe men ganske ustabilt og fanger ikke tidsstrukturen spesielt godt

Dense-modellen flater ut hele sekvensen og mister mye av den naturlige tidsinformasjonen. OK som læringsbaseline, men ikke en spesielt god tidsseriemodell.

Hvorfor hopper MAE så voldsomt i dense-modellen?
> Dense-modellen passer dårligere til strukturen i problemet fordi denne flater ut hele sekvensen, og mister derfor mye av den naturlige tidsrekkefølgen og lokale sammenhenger. Den er rett og slett mindre naturlig for tidsserier.

b. 1D Conv-model
Bedre resultat.
- training og validation MAE faller ganske jevnt
- roligere validation-kurve enn for dense-modellen
- training og validation ligger ganske nær hverandre mot slutten
tyder på at modellen lærer reelle mønstre i tidsserien og at treningen er stabil

-> Conv1D er bedre fordi denne faktisk ser på lokale mønstre over tid:
- nabotidspunkter
- korte trender
- lokale variasjoner

c. LSTM / recurrent-modell
Resultatet ser bra ut, og gir enda smoothere graf
- veldig jevn og pen nedgang i både training og validation MAE
- ingen store eller plutselige hopp
- ligger litt høyere enn Conv1D-plottet (4.2-4.3) vs (3.0)
tyder på at modellen er stabil og pen å trene, men ser ut til å være svakere enn Conv1D på validation MAE.

d. Regularisert LSTM / recurrent-modell
- trente lenge og stabilt
- validation MAE ender på rundt 4.66
så allerede nå ser det ut som at Conv1D fortsatt virker bedre

e. Stacked GRU-modell
Stacked recurrent-modeller kan fange mer komplekse tidsavhengigheter enn en enkelt LSTM, men prisen er:
- mer treningstid
- flere parametre
- økt overfitting-risiko
- større behov for regularisering
Resultat:
- Test MAE på 3.94
- nan på: loss, mae, val_loss og val_mae
=> Brukbart resultat, men svakere enn de beste modellene testet

f. Bidirectional LSTM
Denne modellen leser inputsekvensen en gang forlengs og en gang baklengs i tid, og kombinerer det den lærer fra begge retninger.
=> Helt greit, men ikke spesielt sterkt resultat.
- modellen lærer relativt stabilt
- ender på en relativt høy validation MAE (5.72)

Dette er egentlig et fint eksempel på at en enklere sekvensmodell kan slå en mer avansert modell, og viser oss at LSTM, eller en enda mer komplekse og tidkrevende modeller som stacked GRU eller regularisert recurrent-modell ikke automatisk er best, men at Conv1D er både raskere og bedre.

Det er nettopp arkitekturen, altså hvordan modellen er bygd opp som gjør at de lærer ulikt.
Dense ser bare en lang vektor og utnytter ikke tidsrekkefølgen
Conv1D ser lokale tidsmønstre og matcher sekvensdata
LSTM prosesserer sekvensen steg for steg, og forsøker å huske relevant informasjon over tid

...men det er også andre ting som påvirker resultatene:
- tilfeldig initialisering av vekter
- rekkefølgen batches kommer i
- optimizer
- learning dynamics
- antall epochs
