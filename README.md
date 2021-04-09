# NdR-FloorplanUI
## Readme: work in progress

*Screenshots:*

<img src="/www/ndr_floorplan/Screenshot/IMG_0595.GIF" width="600" /> <img src="/www/ndr_floorplan/Screenshot/IMG_0596.GIF" width="600" />

## Premessa

Data l'alta richiesta di istruzioni su come creare un'interfaccia Floorplan su HomeAssistant, ho deciso di condividere qui la mia esperienza.
Questa repo non vuole essere propriamente una guida "steb by step", ma cercherò comunque di farla il più completa possibile.

Questa volta scriverò tutto in Italiano, per il semplice fatto che di Guide/Repo in inglese sull'argomento ce ne sono già parecchie.

Ultima, doverosa premessa: non so per quanto tempo manterrò aggiornata questa Repo. Qualsiasi consiglio, correztione o suggerimento saranno sempre ben accetti: aprite una Issue in modo da tenere tutto sempre ben tracciato.

# Getting Started

## Approccio

Nella creazione di questa dashboard, il primo, fondamentale, concetto da tenere a mente è il funzionamento della card "[Picture Elements](https://www.home-assistant.io/lovelace/picture-elements/)".

Questa card, in realtà, ha un meccanismo molto semplice: 
- Avrà **sempre** un'immagine di sfondo e degli elementi posti **sopra** di essa;
- Ogni elemento creerà come una sorta di layer aggiuntivo;
- Ogni elemento/layer sarà inserito nello stesso ordine secondo il quale è stato inserito nel vostro file .yaml. Questo significa che due elementi immagine messi uno sopra l'altro (e nella stessa posizione, ma questo lo vedremo dopo) si sovrasteranno. Se la seconda immagine (elemento) inserita nel file .yaml è più grande della precedente, essa la coprirà completamente;
- Gli elementi possono essere di svariati generi. In questa repo troverete prevalentemente Immagini e Button Cards.

## Tema

La scelta del tema, in questo caso, non è particolarmente vincolante. 
Potete crearne uno vostro, copiare quello presente in questa o in altre mie repo (es. [NdR-TabletUI](https://github.com/NdR91/NdR-TabletUI)), o semplicemente scaricare uno dei tanti presenti su HACS. Starà a voi cercare l'abbinamento corretto con i colori che userete.

##