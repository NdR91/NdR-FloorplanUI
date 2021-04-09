# NdR-FloorplanUI
## Readme: work in progress

*Screenshots:*

<img src="/www/ndr_floorplan/Screenshot/IMG_0595.GIF" width="600" /> <img src="/www/ndr_floorplan/Screenshot/IMG_0596.GIF" width="600" />

## Premessa

Data l'alta richiesta di istruzioni su come creare un'interfaccia Floorplan su HomeAssistant, ho deciso di condividere qui la mia esperienza.
Questa repo non vuole essere propriamente una guida "steb by step", ma cercherò comunque di farla il più completa possibile. 

Questa volta scriverò tutto in Italiano, per il semplice fatto che di Guide/Repo in inglese sull'argomento ce ne sono già parecchie.

Ultima, doverosa premessa: non so per quanto tempo manterrò aggiornata questa Repo. Qualsiasi consiglio, correztione o suggerimento saranno sempre ben accetti: aprite una Issue in modo da tenere tutto sempre ben tracciato.

# Floorplan e Picture Elements

## Approccio

Nella creazione di questa dashboard il primo, fondamentale, concetto da tenere a mente è il funzionamento della card "[Picture Elements](https://www.home-assistant.io/lovelace/picture-elements/)".

Questa card, in realtà, ha un meccanismo molto semplice: 
- Avrà **sempre** un'immagine di sfondo e degli elementi posti **sopra** di essa;
- Ogni elemento creerà come una sorta di layer aggiuntivo;
- Ogni elemento/layer sarà inserito nello stesso ordine secondo il quale è stato inserito nel vostro file .yaml. Questo significa che due elementi immagine messi uno sopra l'altro (e nella stessa posizione, ma questo lo vedremo dopo) si sovrasteranno. Se la seconda immagine (elemento) inserita nel file .yaml è più grande della precedente, essa la coprirà completamente;
- Gli elementi possono essere di svariati generi. In questa repo troverete prevalentemente Immagini e Button Cards.

>Screenshot

Detto questo, una cosa importante da sapere è che le immagini che andremo a creare per il nostro floorplan avranno dimensioni specifiche per il dispositivo che userete per visualizzare questa dashboard. Per questa ragione, questo metodo non ha alcuna possibilità di essere scalabile per diverse risoluzioni.
>Le immagini che troverete in questa repo sono state creaate per una risoluzione di 2388×1668 pixel (iPad Pro 11")

## Tema

La scelta del tema, in questo caso, non è particolarmente vincolante. 
Potete crearne uno vostro, copiare quello presente in questa o in altre mie repo (es. [NdR-TabletUI](https://github.com/NdR91/NdR-TabletUI)), o semplicemente scaricare uno dei tanti presenti su HACS. Starà a voi cercare l'abbinamento corretto con i colori che userete.

## Creazione delle immagini necessarie
Per la creazione delle immagini necessarie, sono necessari due software:
1. SweetHome3d (o simili);
2. Photoshop (o simili, es. Gimp);

### SweetHome3D
La creazione della propria planimetria è un processo più o meno lungo. Il tempo che impiegherete dipenderà soprattutto dal livello di dettaglio che desiderate raggiungere nella rappresentazione di casa vostra.

Sul sito ufficiale, oltre al download gratuito del software, troverete anche una lista di Modelli e Texture (anch'essi gratuiti) che vi serviranno per "decorare" la vostra planimetria. Ce ne sono anche tanti altri in rete, sia gratuiti che a pagamento, basta cercare.

Le istruzioni specifiche per l'utilizzo di questo software non saranno trattate su questa repo, per cui vi consiglio di fare una semplice ricerca sul web.

Una volta completata la propria planimetria con il livello di dettaglio desiderato, si potrà passare alla creazione delle immagini.
>Attenzione: oltre alla planimetria, è necessario inserire i punti luce che si vorranno mostrare nella nostra Dashboard. I punti luce possono essere dei modelli, nel caso in cui vogliate mostrare anche l'oggetto (lampada, faretto ecc..), oppure delle fonti di luce "invisibili". Anche in questo caso, a voi la scelta.

>Screenshot

La planimetria completata sarà simile alla seguente:

>Screenshot

Ora, il punto fondamentale: la creazione delle immagini che useremo nel nostro Floorplan.
Per fare questo, dovrete "posizionarvi" in un punto il più centrale possibile all'interno della vostra planimetria.
Per farlo, utilizzate il menu "Vista 3D --> Vista Virtuale", o semplicemente Ctrl+Shift+D.

>Screenshot

Una volta entrati nella modalità Vista Virtuale, apparirà un indicatore a forma di "uomo" che potrete posizionare a vostro piacimento.
Importante: una volta decisi tutti i parametri *"X, Y, Altezza Occhi, Angoli..."* segnateveli. Dovranno essere sempre identici per ogni immagine che creerete, anche in futuro per eventuali aggiornamenti.

>Screenshot

# Dashboard, Sidebar e Menu