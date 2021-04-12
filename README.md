# NdR-FloorplanUI

#### Screenshots

<p align="center">
<img src="/www/ndr_floorplan/Screenshot/IMG_0595.GIF" width="600" />
<img src="/www/ndr_floorplan/Screenshot/IMG_0596.GIF" width="600" />
</p>

#### Se ti piace il mio lavoro...

<p align="center">
<a href="https://www.buymeacoffee.com/Ndr91" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>
</p>

### Premessa

Data l'alta richiesta di istruzioni su come creare un'interfaccia Floorplan su HomeAssistant, ho deciso di condividere qui la mia esperienza.
Questa repo non vuole essere propriamente una guida "steb by step", ma cercherò comunque di farla il più completa possibile. 

Questa volta scriverò tutto in Italiano, per il semplice fatto che di Guide/Repo in inglese sull'argomento ce ne sono già parecchie.

Ultima, doverosa premessa: non so per quanto tempo manterrò aggiornata questa Repo. Qualsiasi consiglio, correztione o suggerimento saranno sempre ben accetti: aprite una Issue in modo da tenere tutto sempre ben tracciato.

# Indice

- [Floorplan e Picture Elements](#floorplan-e-picture-elements)
  - [Approccio](#approccio)
  - [Creazione delle Immagini](#creazione-delle-immagini)  
    - [SweetHome3D](#sweethome3d)
    - [Elaborazione Immagini](#elaborazione-immagini-photoshop-gimp-ecc)
- [Costruzione della Dashboard](#costruzione-della-dashboard)
  - [Posizione dei file immagine](#posizione-dei-file-immagine)
  - [Configuration.yaml](#configurationyaml)
  - [Tema](#tema)
  - [Custom Cards](#custom-cards)
  - [File dashboard.yaml](#file-dashboardyaml)
    - [Picture Elements](#picture-elements)

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

Ora potrete esportare ogni layer come descritto nel paragrafo precedente.

# Costruzione della Dashboard

Finalmente tutti gli elementi necessari per la creazione della nostra Dashboard sono pronti, quindi non ci resta che "assemblare" il tutto in un unico file.
> *Nota: In realtà, tutto quello che troverete qui di seguito è assolutamente replicabile anche nella modalità di modifica della Dashboard integrata sul Frontend di Home Assistant. Tuttavia, il mio cionsiglio è di farlo "manualmente", ovvero in modalità yaml, in modo da poter utilizzare strumenti come Visual Studio Code. In questo modo risulta molto più semplice la gestione di un file che supererà abbondantemente le 5000 righe*

## Posizione dei file immagine

Le immagini create devono ovviamente essere presenti all'interno della vostra cartella */config* di Home Assistant.
Per semplicità, vi consiglio di creare una nuova cartella all'interno di */www*, in modo da raggruppare tutti i file necessari in un unico luogo.

>Perchè all'interno di */www*? Semplicemente, i file all'interno di questa cartella sono accessibili anche dal file che andrete a creare. In caso contrario, vi invito a consultare la documentazione ufficiale, che descrive come utilizzare [allowlist_external_dirs](https://www.home-assistant.io/docs/configuration/basic/).

Negli esempi che troverete in questa repo, i file relativi alla Dashboard sono **sempre** all'interno di */config/www/ndr_floorplan/*.

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
    ndr-floorplan: #<-- Nome a piacimento
      mode: yaml
      title: NdR Floorplan UI #<-- Titolo a piacimento
      icon: mdi:tablet #<-- Icona a piacimento
      show_in_sidebar: true
      filename: ndr_floorplan.yaml #<-- Nome a piacimento
    # ----------------------------------------------------
```
> *Nota: molto importante la parte **mode: storage**. Questo significa che potrete utilizzare sia Dashboard create da Frontend che in modalità yaml*

## Tema

La scelta del tema, in questo caso, non è particolarmente vincolante. 
Potete crearne uno vostro, copiare quello presente in questa o in altre mie repo (es. [NdR-TabletUI](https://github.com/NdR91/NdR-TabletUI)), o semplicemente scaricare uno dei tanti presenti su HACS. Starà a voi cercare l'abbinamento corretto con i colori che userete.

## Custom Cards
In questa Repo, le custom Cards utilizzate sono le seguenti:

- [X] [Config Template Card](https://github.com/iantrich/config-template-card)
- [x] [Layout Card](https://github.com/thomasloven/lovelace-layout-card)
- [x] [Card Mod](https://github.com/thomasloven/lovelace-card-mod)
- [x] [Button Card](https://github.com/custom-cards/button-card)
- [x] [Browser Mod](https://github.com/thomasloven/hass-browser_mod)
- [ ] [Mini Graph Card](https://github.com/kalkih/mini-graph-card)
- [ ] [Flexible Horseshoe Card](https://github.com/AmoebeLabs/flex-horseshoe-card)
- [ ] [Mini Climate Card](https://github.com/artem-sedykh/mini-climate-card)
- [ ] [Gap Card](https://github.com/thomasloven/lovelace-gap-card)
- [ ] [Simple Weather Card](https://github.com/kalkih/simple-weather-card)

<details><summary> Legenda</summary>

- [X] = Importante 
- [ ] = Non Importante

</details>

Vi invito a leggere la documentazione di quelle che intendete ad utilizzare.

## File Dashboard.yaml

Bene, ora finalmente andremo a creare il file yaml della nostra Dashboard. 
Il file dovrà avere lo stesso nome inserito in *"filename:"* nel nostro [configuration.yaml](#configuration.yaml).  
Per prima cosa, bisogna inserire le prima due righe:
```yaml
title: TabletUI #<-- Titolo a piacimento
views:
```
> Ovviamente il titolo è a vostra scelta.  

Dopo *"views"* andremo ad inserire tutte le nostre *VISTE*, che saranno tendenzialmente una per ogni *"Bottone"* che vorremo inserire nella nostra Dashboard.

La vista contenente il nostro Floorplan, sarà impostata in modo simile al seguente:

```yaml
- title: Home #<-- Nome a piacimento
  icon: 'mdi:floor-plan' #<-- Icona a piacimento
  panel: true
  path: home #<-- Il Path sarà utile per generare link navigabili all'interno dell'interfaccia
  badges: []    
  cards:
```

### Picture Elements

Questa è la card più importante in assoluto, ovvero quella che vi permetterà di visualizzare la vostra planimetria e di renderla interattiva.

I principi fonamentali di questa card sono stati discussi nella sezione [Approccio](#approccio).

Per mettere in pratica quanto descritto, andremo ad utilizzare le immagini create nei paragrafi precedenti:

```yaml
type: picture-elements       
image: /local/ndr_floorplan/floorplan/casa-notte.png #<-- percorso della vostra immagine della Planimetria di notte
style: |
  ha-card:first-child {
    background: rgba(42, 46, 48, 1)
  }
elements:
```

Questo primo "blocco" non fa altro che inserire l'immagine di sfondo, quella della Planimetria *"di notte"* e con tutte le luci spente.

Adesso, ovviamente, andremo ad aggiungere gli *Elementi*, ovvero tutte le nostre luci.

```yaml
- type: image
  action: none
  entity: light.faretti
  hold_action:
    action: none
  image: /local/ndr_floorplan/floorplan/sala_faretti.png
  style:
    filter: >-
      ${ "hue-rotate(" + (states['light.faretti'].attributes.hs_color
      ? states['light.faretti'].attributes.hs_color[0] : 0) + "deg)"}
    left: 50%
    mix-blend-mode: lighten
    opacity: "${states['light.faretti'].state === 'on' ? (states['light.faretti'].attributes.brightness / 255) : '0'}"
    top: 50%
    width: 100%
  tap_action:
    action: none

- type: image
  action: none
  entity: light.led_cucina
  hold_action:
    action: none
  image: /local/ndr_floorplan/floorplan/sala_led-cucina.png
  style:
    filter: >-
      ${ "hue-rotate(" + (states['light.led_cucina'].attributes.hs_color
      ? states['light.led_cucina'].attributes.hs_color[0] : 0) + "deg)"}
    left: 50%
    mix-blend-mode: lighten
    opacity: "${states['light.led_cucina'].state === 'on' ? (states['light.led_cucina'].attributes.brightness / 255) : '0'}"
    top: 50%
    width: 100%
  tap_action:
    action: none
  
  ...continua con altre luci
```
**Importante**:  
Fate attenzione alle seguenti istruzioni:
- **filter**: utile per aggiungere un filtro alla vostra luce, che cambierà colore in base a quello utilizzato (utile per lampadine/strisce led RGB);
- **opacity**: utile per sfumare la quantità di luce in base alla percentuale di luminosità impostata (utile per qualsiasi lampadina/striscia led dimmerabile);

###### BONUS 1: Planimetria "di giorno"

Volendo, si può aggiungere un elemento corrispondente alla vostra planimetria scattata "di giorno".

```yaml
- type: image
  action: none
  entity: sun.sun
  hold_action:
    action: none
  state_image:
    above_horizon: /local/ndr_floorplan/floorplan/casa-giorno.png
    below_horizon: /local/ndr_floorplan/transparent.png
  style:
    height: 100%
    left: 50%
    mix-blend-mode: lighten
    opacity: '${ states[''sensor.sunlight_opacity''].state }'
    top: 50%
    width: 100%
  tap_action:
    action: none 
```
L'opzione **state_image** renderà trasparente l'immagine durante le ore notture (e quindi mostrando la principale), mentre la mostrerà durante le ore di luce (notare l'entità *sun.sun* ed i suoi stati *above_horizon* e *below_horizon*).

###### BONUS 2: Gif animate per il Meteo

Nella sezione [Elaborazione Immagini](#elaborazione-immagini-photoshop-gimp-ecc) avevo menzionato un BONUS riguardante due immagini (giorno + notte) con eventuali balconi/terrazzie/giardini esclusi.
Bene, lo scopo di queste due ulteriori immagini è quello di inserire nel nostro Floorplan, una o più gif raffiguranti il meteo che agiscano ovviamente solo all'esterno dell'abitazione.

Per raggiungere lo scopo, utilizzeremo questo codice:

```yaml
- type: image
  action: none
  entity: weather.home
  hold_action:
    action: none
  state_image:
    rainy: /local/ndr_floorplan/floorplan/weather/rainstorm.gif
    pouring: /local/ndr_floorplan/floorplan/weather/rain.gif
    lightning-rainy: /local/ndr_floorplan/floorplan/weather/rain2.gif
    snowy: /local/ndr_floorplan/transparent.png
    snowy-rainy: /local/ndr_floorplan/transparent.png
    sunny: /local/ndr_floorplan/transparent.png
    clear-night: /local/ndr_floorplan/transparent.png
    fog: /local/ndr_floorplan/transparent.png
    hail: /local/ndr_floorplan/transparent.png
    lightning: /local/ndr_floorplan/transparent.png
    cloudy: /local/ndr_floorplan/transparent.png
    partlycloudy: /local/ndr_floorplan/transparent.png
    windy: /local/ndr_floorplan/transparent.png
    windy-variant: /local/ndr_floorplan/transparent.png
    exceptional: /local/ndr_floorplan/transparent.png
  style:
    left: 60%
    mix-blend-mode: color-dodge
    top: 50%
    width: 110% 
  tap_action:
    action: none
```

> Il metodo sopra descritto è stato utilizzato con l'intento di utilizzare più di una gif. Se si volesse utilizzarne solo una, si potrebbe utilizzare *state_filter*. In questo modo si eviterebbe di menzionare tutti gli stati possibili dell'entità meteo. Più info sulla [doc ufficiale](https://www.home-assistant.io/lovelace/picture-elements/).

#### Icone

Arrivati a questo punto, avrete finalmente il vostro Floorplan con tutte le immagini ed eventuali gif inserite. Cosa manca? Le icone "cliccabili" per poter controllare le luci di casa vostra.

Anche queste saranno degli elementi della Picture Elements:

```yaml
- type: state-icon
  entity: light.faretti
  icon: 'mdi:string-lights'
  style:
    '--iron-icon-height': 1.2vw
    '--iron-icon-width': 1.2vw
    '--paper-item-icon-active-color': '#ffff66'
    '--paper-item-icon-color': darkgrey
    align-items: center
    background-color: '#FFFFFF'
    border-radius: 100%
    box-shadow: '0px 0px 28px 0px rgba(0,0,0,0.39)'
    display: flex
    justify-content: center
    left: 61%
    margin-left: '-1.5vw'
    margin-top: '-1.5vw'
    top: 71%
    transform: scale(1.2)
    width: 2.5vw
    height: 2.5vw
  tap_action:
    action: toggle
```
Fatto questo, il vostro Floorplan sarà funzionante.

#### Sidebar

Se durante la creazione delle vostre immagini avete previsto uno spazio per la Sidebar, è arrivato il momento di inserirla.

Anche questa, ovviamente, altro non è che un *Elemento* della Picture Elements. Quindi, il suo inserimento sarà del tutto simile agli elementi precedenti:

```yaml
- action: none
  hold_action:
    action: none
  image: /local/ndr_floorplan/sidebar/sidebar_xl.png
  style:
    height: 100%
    left: 11.73828125%
    top: 50%
    width: 23.4765625%
  tap_action:
    action: none
  type: image
```
>Nota 1: il consiglio è di perfezionare la vostra sidebar nella prima vista, in modo da copiarla/incollarla tale e quale anche nelle viste successive (con eventuali piccole correzioni);  
>Nota 2: tutte le card che trovate nella sidebar in questa repo, sono a loro volta degli *elementi* della Picture Elements.