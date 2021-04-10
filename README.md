# NdR-FloorplanUI

#### Screenshots

<p align="center">
<img src="/www/ndr_floorplan/Screenshot/IMG_0595.GIF" width="600" />
<img src="/www/ndr_floorplan/Screenshot/IMG_0596.GIF" width="600" />
</p>

### Premessa

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

><details><summary>Esempi</summary>
>1. Picture Elements con un Badge come elemento:
>
> ```yaml
>type: picture-elements
>image: 'https://demo.home-assistant.io/stub_config/floorplan.png'
>elements:
>  - type: state-badge
>    entity: binary_sensor.stato_asciugatrice
>    style:
>      top: 32%
>      left: 40%
> ```
>
><img src="/www/ndr_floorplan/Screenshot/pic_el1.jpg" width="300" />   
>
>Notare come l'elemento si sovrappone all'immagine principale, ovvero la planimetria.
>2. Picture Elements con due elementi:
>
> ```yaml
>type: picture-elements
>image: 'https://demo.home-assistant.io/stub_config/floorplan.png'
>elements:
>  - type: state-badge
>    entity: binary_sensor.stato_asciugatrice
>    style:
>      top: 32%
>      left: 40%
>  - type: image
>    image: /local/ndr_floorplan/blurredFloorplan.png
>    style:
>      top: 32%
>      left: 40%
> ```
><img src="/www/ndr_floorplan/Screenshot/pic_el2.jpg" width="300" />   
>
> In questo caso, il secondo elemento (image) sovrasta il primo.
></details>

- Gli elementi possono essere di svariati generi. In questa repo troverete prevalentemente Immagini e Button Cards.

Detto questo, una cosa importante da sapere è che le immagini che andremo a creare per il nostro floorplan avranno dimensioni specifiche per il dispositivo che userete per visualizzare questa dashboard. Per questa ragione, questo metodo non ha alcuna possibilità di essere scalabile per diverse risoluzioni.
>Le immagini che troverete in questa repo sono state creaate per una risoluzione di 2388×1668 pixel (iPad Pro 11")

## Creazione delle immagini
Per la creazione delle immagini necessarie, sono necessari due software:
1. SweetHome3d (o simili);
2. Photoshop (o simili, es. Gimp);

### SweetHome3D
La creazione della propria planimetria è un processo più o meno lungo. Il tempo che impiegherete dipenderà soprattutto dal livello di dettaglio che desiderate raggiungere nella rappresentazione di casa vostra.

Sul sito ufficiale, oltre al download gratuito del software, troverete anche una lista di Modelli e Texture (anch'essi gratuiti) che vi serviranno per "decorare" la vostra planimetria. Ce ne sono anche tanti altri in rete, sia gratuiti che a pagamento, basta cercare.

Le istruzioni specifiche per l'utilizzo di questo software non saranno trattate su questa repo, per cui vi consiglio di fare una semplice ricerca sul web.

Una volta completata la propria planimetria con il livello di dettaglio desiderato, si potrà passare alla creazione delle immagini.
>Attenzione: oltre alla planimetria, è necessario inserire i punti luce che si vorranno mostrare nella nostra Dashboard. I punti luce possono essere dei modelli, nel caso in cui vogliate mostrare anche l'oggetto (lampada, faretto ecc..), oppure delle fonti di luce "invisibili". Anche in questo caso, a voi la scelta.  

><details><summary>Tipi di luce</summary>
><img src="/www/ndr_floorplan/Screenshot/swt-luci.jpg" width="300" /> 
> </details>

La planimetria completata sarà simile alla seguente:
<p align="center">
<img src="/www/ndr_floorplan/Screenshot/swt-plan.jpg" width="500" /> 
</p>

Ora, il punto fondamentale: la creazione delle immagini che useremo nel nostro Floorplan.
Per fare questo, dovrete "posizionarvi" in un punto il più centrale possibile all'interno della vostra planimetria.
Per farlo, utilizzate il menu **"Vista 3D --> Vista Virtuale"**, o semplicemente **Ctrl+Shift+D** (Windows) / **Shift+Cmd+D** (Mac OS).
<p align="center">
<img src="/www/ndr_floorplan/Screenshot/swt-vst.jpg" width="500" /> 
</p>

Una volta entrati nella modalità Vista Virtuale, apparirà un indicatore a forma di "uomo" che potrete posizionare a vostro piacimento.
Importante: una volta decisi tutti i parametri *"X, Y, Altezza Occhi, Angoli..."* segnateveli. Dovranno essere sempre identici per ogni immagine che creerete, anche in futuro per eventuali aggiornamenti.
<p align="center">
<img src="/www/ndr_floorplan/Screenshot/swt-sgn.jpg" width="500" /> 
</p>

Settata la posizione, non resta che generare le immagini. Il totale delle immagini che vi serviranno equivale ad 1 + No. di Luci che avete (+1 opzionale).
>Esempio:
>Nel caso in cui aveste 10 punti luce da gestire, avrete 11 o 12 foto in totale:
> - La prima immagine, sarà la vostra planimetria di notte, con tutte le luci spente;
> - L'ultima, quella opzionale, sarà la vosra planimetria di giorno, sempre a luci spente;
> - Le restanti immagini saranno le vostre luci accese (una foto per luce accesa, di notte).

Le immagini si possono generare tramite il Menu *"Crea Foto"* (Icona con la Macchina Fotografica nera), andando ad inserire le impostazioni di risoluzione desidetare. 
Molto importante sarà la selezione dell'orario in cui "simulare" la nostra foto, perchè questo andrà ad incidere direttamente sulla quantità di luce presente.
Il mio consiglio è di generare la foto notturna impostando l'orario tra le 20:00 e le 21:00 circa, altrimenti la rappresentazione sarà completamente buia.
Per la stessa ragione, per la foto di giorno consiglio di impostare l'orario alle 05:00 circa.
>La scelta dell'orario è soggettiva e cambierà significativamente i colori delle immagini che andrete a creare. Vi consiglio di fare qualche prova e verificare che la resa sia di vostro gradimento, soprattutto per quelle notturne che, se troppo scure, renderanno indistinguibili le stanze della vostra planimetria.
<p align="center">
<img src="/www/ndr_floorplan/Screenshot/swt-img.png" width="500" />   
<img src="/www/ndr_floorplan/Screenshot/swt-stg.jpg" width="300" /> 
</p>

**Importante**: scegliete accuratamente la risoluzione delle vostre immagini, la quale non solo dovrà essere pari o inferiore a quella del vostro Tablet/Display, ma dovrà tenere anche conto di eventuali elementi aggiuntivi che vorrete inserire nella vostra Dashboard. Ad esempio, se prevedete di inserire una sidebar larga 300px, allora la risoluzione delle vostre immagini dovrà essere inferiore di minimo 350px circa.

Per accendere/spegnere le singole luci, basta attivare o meno il flag che troverete nell'elenco degli elementi inseriti nella vostra planimetria (barra in basso a sinistra).

<details><summary>Screenshots:</summary>
<p align="center">
<img src="/www/ndr_floorplan/Screenshot/swt-lucioff.png" width="800" /> 
<img src="/www/ndr_floorplan/Screenshot/swt-lucion.png" width="800" /> 
</p>
</details>

### Elaborazione Immagini (Photoshop, Gimp, ecc...)

Ora che le immagini sono pronte, è arrivato il momento di elaborarle per far si che possano essere sistemate nella nostra Picture Elements.

Quello che dovrete fare, è aggiungerle tutte in un unico progetto, creando un layer per ogni immagine aggiunta; In questo modo le immagini si sovrapporranno perfettamente e voi non dovrete faticare per allinearle tutte:

<p align="center">
<img src="/www/ndr_floorplan/Screenshot/ps-all.png" width="800" />   
</p>

>Attenzione: prima ancora di importare le vostre immagini sul vostro software di editing, assicuratevi di creare un progetto con la stessa risoluzione del vostro Tablet.
><details><summary>Esempio:</summary>
><img src="/www/ndr_floorplan/Screenshot/ps-stg.png" width="300" /> 
></details>

Ogni layer dovrà essere "ritagliato" come segue (su Photoshop lo strumento sarà il Lazzo Poligonale):
- Le due immagini "totali" giorno e notte dovranno essere tagliate nel perimetro. 
>In sostanza dovrete eliminare il "fondo" grigio lasciato da SweetHome3D
><details><summary>Esempio:</summary>
><img src="/www/ndr_floorplan/Screenshot/casa-notte.png" width="600" /> 
></details>

- Ogni immagine appartenente ad una luce accesa, dovrà contenere **solo** la stanza appartenente a quella luce.

>Quindi, se per esempio prendiamo una luce in Salotto, dovrà rimanere soltanto un'immagine del salotto con quella luce accesa
><details><summary>Esempio:</summary>
><img src="/www/ndr_floorplan/Screenshot/sala_faretti.png" width="600" /> 
></details>


- BONUS: delle due immagini *totali*, ritagliare soltanto l'interno della casa (nel caso in cui abbiate disegnato anche balconi/terrazze/giardini).

>Queste due immagini aggiuntive possono servire nel caso in cui si decida di inserire una gif in overlay durante le giornate di pioggia. Vedremo questa "aggiunta" nei paragrafi successivi
><details><summary>Esempio:</summary>
>Casa di giorno con balcone escluso:
> 
><img src="/www/ndr_floorplan/Screenshot/interno-giorno.png" width="600" />   
>  
>Casa di notte con balcone escluso:
><img src="/www/ndr_floorplan/Screenshot/interno-notte.png" width="600" /> 
></details>  
 
A questo punto, se non prevedete di aggiungere ulteriori elementi (es. Sidebar), non dovrete fare altro che esportare ogni layer come singola immagine .png, rinominandole come preferite (tenendo conto del fatto che, nel vostro file .yaml, dovrete inserire i percorsi di ognuna di esse).

#### Sidebar

Piccolo paragrafo a proposito della Sidebar, in quanto anch'essa non è altro che un'immagine che andremo a sovrapporre come *elemento* alla nostra Picture Elements.
In modo da essere sicuri che tutte le nostre immagini siano ben posizionate all'interno della nostra dashboard, aggiungete l'immagine della Sidebar (quella presente in questa repo, o una qualsiasi a vostra scelta) al vostro progetto.
>Nota: è molto probabile che dovrete ridimensionare l'immagine in questione. Anche in questo caso sta a voi decidere quale risoluzione dare alla Sidebar, tenendo conto del fatto che dovrete inserire dei bottoni su di essa.

Ora, selezionate tutti i layer relativi alla vostra planimetria e, con lo strumento *Sposta* traslateli tutti in modo che non si sovrappongano alla sidebar appena aggiunta. Anche questa operazione fa si che non dobbiate riallinearli tutti in seguito.

<p align="center">
<img src="/www/ndr_floorplan/Screenshot/ps-cmp.png" width="800" />   
</p>

# Costruzione della Dashboard

Finalmente tutti gli elementi necessari per la creazione della nostra Dashboard sono pronti, quindi non ci resta che "assemblare" il tutto in un unico file.
> *Nota: In realtà, tutto quello che troverete qui di seguito è assolutamente replicabile anche nella modalità di modifica della Dashboard integrata sul Frontend di Home Assistant. Tuttavia, il mio cionsiglio è di farlo "manualmente", ovvero in modalità yaml, in modo da poter utilizzare strumenti come Visual Studio Code. In questo modo risulta molto più semplice la gestione di un file che supererà abbondantemente le 5000 righe*

## Configuration.yaml
Per prima cosa, dovremo predisporre il nostro *configuration.yaml* per poter utilizzare **anche** le Dashboard in modalità yaml.

```yaml
lovelace:
  # MODE
  mode: storage
  # ------------------------------------------------------
  # Dashoards
  dashboards:
    #Floorplan UI
    ndr-floorplan: <-- Nome a piacimento
      mode: yaml
      title: NdR Floorplan UI <-- Titolo a piacimento
      icon: mdi:tablet <-- Icona a piacimento
      show_in_sidebar: true
      filename: ndr_floorplan.yaml <-- Nome a piacimento
    # ----------------------------------------------------
```
> *Nota: molto importante la parte **mode: storage**. Questo significa che potrete utilizzare sia Dashboard create da Frontend che in modalità yaml*

## Tema

La scelta del tema, in questo caso, non è particolarmente vincolante. 
Potete crearne uno vostro, copiare quello presente in questa o in altre mie repo (es. [NdR-TabletUI](https://github.com/NdR91/NdR-TabletUI)), o semplicemente scaricare uno dei tanti presenti su HACS. Starà a voi cercare l'abbinamento corretto con i colori che userete.