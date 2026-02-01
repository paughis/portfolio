---
layout: page
title: 
permalink : /olist_markdown/
---

<img src="{{ site.baseurl }}/assets/olist1_titolo.png" alt="Olist E-Commerce" style="width: 100%;">

---

## Indice dei Contenuti

1. [Obiettivi del Progetto e Business Case](#obiettivi-del-progetto-e-business-case)
2. [Architettura dei Dati](#architettura-dei-dati)
3. [Analisi Descrittiva](#analisi-descrittiva)
4. [Analisi Comparativa: Ranking](#analisi-comparativa-ranking)
5. [Analisi di Correlazione](#analisi-di-correlazione)
6. [Analisi della Complessità Logistica](#analisi-della-complessità-logistica)
7. [Analisi del Lead Time](#analisi-del-lead-time)
8. [Text Mining delle Recensioni](#text-mining-delle-recensioni)
9. [Sentiment Analysis: Sentimento Positivo](#sentiment-analysis-sentimento-positivo)
10. [Sentiment Analysis: Sentimento Negativo](#sentiment-analysis-sentimento-negativo)
11. [Conclusioni](#conclusioni)

---

## Obiettivi del Progetto e Business Case

<img src="{{ site.baseurl }}/assets/olist2.png" alt="Obiettivi del Progetto" style="width: 100%;">

### Obiettivi del Progetto

**Identificare i principali fattori dell'esperienza d'acquisto associati alla soddisfazione del cliente per Olist e proporre azioni di miglioramento adeguate.**

### Business Case

Olist è una piattaforma brasiliana che supporta i venditori nella gestione delle vendite online tramite marketplace di terze parti. Il dataset include informazioni su ordini, item, pagamenti, logistica, clienti, venditori e recensioni, definendo un modello complesso e multidimensionale.

---

## Architettura dei Dati

<img src="{{ site.baseurl }}/assets/olist3_model.png" alt="Modello dei Dati" style="width: 100%;">

### Modello Multidimensionale - Power BI

#### Fact Table: Granularità a Livello Ordini

Per garantire coerenza analitica ed evitare ambiguità dovute alle relazioni molti a molti presenti sul modello, è stata costruita una fact table a livello ordine.

La tabella aggrega informazioni provenienti da più fonti (tabelle 'payments', 'reviews', 'orders' e 'items'), consentendo analisi consistenti sulla soddisfazione del cliente.

---

## Analisi Descrittiva

### Distribuzione Temporale e Pattern delle Valutazioni

<img src="{{ site.baseurl }}/assets/olist4_panoramica_sopra.png" alt="Analisi Descrittiva" style="width: 100%;">

#### Stabilità delle Valutazioni nel Tempo

La distribuzione dei voti rimane, nel complesso, stabile nel periodo analizzato. L'unica eccezione è la valutazione più bassa (voto 1), che mostra un lieve aumento tra novembre 2017 e marzo 2018, un possibile segnale di criticità temporanee nel servizio.

#### Tempo di Consegna e Soddisfazione

<img src="{{ site.baseurl }}/assets/olist5_tempo_consegna_valutacione.png" alt="Tempo di Consegna vs Valutazione" style="float: left; margin-right: 20px; margin-bottom: 10px; max-width: 45%;">

Gli ordini si concentrano prevalentemente nella fascia standard (6-15 giorni), mentre la categoria molto lenta è marginale.

È evidente una correlazione positiva tra rapidità di consegna e valutazione media, suggerendo che il tempo di consegna potrebbe essere un driver significativo della customer satisfaction.

<div style="clear: both;"></div>

---

## Analisi Comparativa: Ranking

### Fattori Correlati alle Valutazioni

<img src="{{ site.baseurl }}/assets/olist_dash_2.png" alt="Analisi Comparativa" style="width: 100%;">

I pulsanti di filtro nella parte superiore consentono di selezionare la valutazione media e di aggiornare dinamicamente tutte le visualizzazioni, permettendo il confronto tra ordini ben valutati e quelli con valutazioni basse, al fine di esplorare possibili fattori associati all'esito dell'ordine.

#### Insight Principali (Confronto tra Voto 1 e Voto 5)

1. **Impatto del Tempo di Consegna**: tra le variabili analizzate, il tempo totale di consegna e il ritardo di consegna sono quelle che mostrano la maggiore differenza passando dalle valutazioni più basse a quelle più alte.

2. **Stabilità del Giorno della Settimana**: contrariamente all'ipotesi iniziale, la distribuzione degli ordini per giorno della settimana rimane sostanzialmente stabile tra le diverse valutazioni.

3. **Complessità Multi-Venditore**: gli ordini con un solo venditore rappresentano la maggioranza in tutte le classi di voto. Gli ordini con 4-5 venditori, sebbene molto rari, compaiono solo tra quelli con valutazione minima, suggerendo un possibile legame tra la complessità dell'ordine e l'esperienza d'acquisto (più venditori = rischio più elevato di errori o ritardi).

---

## Analisi di Correlazione

Per approfondire l'analisi delle variabili numeriche di interesse, è stata costruita una matrice di correlazione di Pearson utilizzando Python (librerie Pandas e Seaborn):

<img src="{{ site.baseurl }}/assets/olist7_corr.png" alt="Matrice di Correlazione" style="width: 100%;">


### Insight Principali

1. **Correlazione Negativa**: le aree più scure evidenziano una correlazione negativa moderata tra la valutazione media e il tempo di consegna (ritardo di consegna e tempo totale di spedizione), in linea con quanto osservato nelle dashboard precedenti.

2. **Assenza di Correlazione con la Complessità**: non emerge una correlazione significativa tra i tempi di consegna e la quantità di venditori coinvolti nell'ordine, contrariamente all'ipotesi iniziale di una maggiore complessità logistica. Tuttavia, data la bassa numerosità degli ordini multi-venditore, questi casi risultano poco rappresentativi e potrebbero richiedere un'analisi separata.

<div style="clear: both;"></div>

---

## Analisi della Complessità Logistica

### Ordini Multi-Venditori

#### Ritardo di Consegna e Numero di Venditori

<img src="{{ site.baseurl }}/assets/olist8_venditore_vs_ritardo.png" alt="Venditori vs Ritardo" style="float: left; margin-right: 20px; margin-bottom: 10px; max-width: 45%;">

All'aumentare del numero di venditori, il ritardo di consegna (definito come la differenza tra la data stimata di consegna e la data effettiva di arrivo al cliente) tende a spostarsi verso valori inferiori, sia nella media sia nell'intervallo interquartile.

Questo risultato appare controintuitivo e solleva una domanda: **Perché ordini più complessi mostrano un ritardo minore?**

La riposta si potrebbe trovare nelle **stime del tempo di consegna**:

<div style="clear: both;"></div>

#### Stima del Tempo di Consegna e Numero di Venditori

<img src="{{ site.baseurl }}/assets/olist9_stima_consegna.png" alt="Stima Consegna vs Venditori" style="width: 100%;">

L'analisi conferma che Olist applica stime sempre più conservative per gli ordini multi-venditore, gestendo efficacemente le aspettative dei clienti e riducendo i ritardi percepiti.

<div style="clear: both;"></div>

---

## Analisi del Lead Time

<img src="{{ site.baseurl }}/assets/olist10_leadtime.png" alt="Analisi Lead Time" style="width: 100%;">

### Identificazione delle Fasi Critiche

Per esplorare in quale fase del flusso di acquisto si concentrano i maggiori ritardi, sono stati analizzati quattro boxplot che confrontano la distribuzione dei giorni trascorsi in ciascuna fase del processo.

I risultati mostrano che **i tempi più lunghi si osservano nella fase tra l'acquisto e l'approvazione del pagamento**, con una media di circa 25 giorni e un intervallo interquartile compreso tra 20 e 30 giorni. Questa fase presenta anche la maggiore dispersione, indicando una forte variabilità dei tempi di approvazione.

---

## Text Mining delle Recensioni

### Tematiche d'Importanza per i Clienti

<img src="{{ site.baseurl }}/assets/olist_wc.png" alt="Word Cloud Recensioni" style="width: 100%;">

I commenti delle recensioni sono stati tradotti in inglese e analizzati in Python tramite una word cloud. Le parole più visibili fanno riferimento a prodotto, consegna e tempi, con una prevalenza di termini dal tono positivo, coerente con la distribuzione complessiva delle valutazioni.

#### Temi Chiave Identificati

<div style="overflow-x: auto;">
  <table style="width: 100%; table-layout: fixed;">
    <thead>
      <tr style="background-color: #f0f0f0;">
        <th style="width: 33.33%;">Aspettative sui Tempi di Consegna</th>
        <th style="width: 33.33%;">Conformità Prodotto-Descrizione</th>
        <th style="width: 33.33%;">Qualità del Servizio Clienti</th>
      </tr>
    </thead>
    <tbody>
      <tr style="background-color: #ffffff;">
        <td>Tempo di consegna</td>
        <td>Prodotto corretto</td>
        <td>Assistenza</td>
      </tr>
      <tr style="background-color: #ffffff;">
        <td>Consegna rapida</td>
        <td>Conforme alla descrizione</td>
        <td>Comunicazione</td>
      </tr>
      <tr style="background-color: #ffffff;">
        <td>Consegna in anticipo</td>
        <td>Come da foto</td>
        <td>Risposta</td>
      </tr>
      <tr style="background-color: #ffffff;">
        <td>Puntualità</td>
        <td>Qualità del prodotto</td>
        <td>Contatto</td>
      </tr>
      <tr style="background-color: #ffffff;">
        <td>Attesa</td>
        <td>Aspettative rispettate</td>
        <td>Rimborso</td>
      </tr>
      <tr style="background-color: #ffffff;">
        <td>Ritardo</td>
        <td></td>
        <td>Gestione del problema</td>
      </tr>
    </tbody>
  </table>
</div>

---

## Sentiment Analysis: Sentimento Positivo

<img src="{{ site.baseurl }}/assets/olist12_pos_wordcloud.png" alt="Word Cloud Sentimento Positivo" style="width: 100%;">

### Tematiche d'Importanza per i Clienti

I clienti che esprimono sentiment positivo spesso menzionano anche la facilità dell'acquisto e il servizio clienti reattivo. Le parole sui tempi di consegna emergono molto, confermando le analisi precedenti che identificano la logistica come fattore chiave di soddisfazione.

#### Temi Positivi Chiave

<div style="overflow-x: auto;">
  <table style="width: 100%; table-layout: fixed;">
    <thead>
      <tr style="background-color: #f0f0f0;">
        <th style="width: 33.33%;">Tempi di Consegna</th>
        <th style="width: 33.33%;">Condizioni del Prodotto</th>
        <th style="width: 33.33%;">Esperienza d'Acquisto</th>
      </tr>
    </thead>
    <tbody>
      <tr style="background-color: #ffffff;">
        <td>Molte recensioni apprezzano l'arrivo anticipato dei prodotti e la puntualità</td>
        <td>I clienti evidenziano la buona qualità, la bellezza e la cura nell'imballaggio</td>
        <td>Compaiono termini che indicano un alto livello di soddisfazione complessiva</td>
      </tr>
    </tbody>
  </table>
</div>

<div style="clear: both;"></div>

---

## Sentiment Analysis: Sentimento Negativo

<img src="{{ site.baseurl }}/assets/olist13_neg_wordcloud.png" alt="Word Cloud Sentimento Negativo" style="float: left; margin-right: 20px; margin-bottom: 10px; max-width: 45%;">

### Tematiche d'Importanza per i Clienti

I problemi con il prodotto ricevuto sembrano influire in modo significativo sul sentiment negativo, più del ritardo o del tempo di attesa prolungato. Alcuni commenti menzionano la comunicazione con il venditore, suggerendo la necessità di un servizio clienti più efficace.

#### Temi Negativi Chiave

<div style="overflow-x: auto;">
  <table style="width: 100%; table-layout: fixed;">
    <thead>
      <tr style="background-color: #f0f0f0;">
        <th style="width: 33.33%;">Prodotto Ricevuto</th>
        <th style="width: 33.33%;">Consegna</th>
        <th style="width: 33.33%;">Esperienza d'Acquisto</th>
      </tr>
    </thead>
    <tbody>
      <tr style="background-color: #ffffff;">
        <td>I clienti lamentano prodotti rotti, errati o di bassa qualità</td>
        <td>Problemi legati a ritardi, smarrimenti o all'imballaggio</td>
        <td>Esperienze poco soddisfacenti durante il processo di acquisto e con il servizio clienti</td>
      </tr>
    </tbody>
  </table>
</div>

<div style="clear: both;"></div>

---

## Conclusioni

### Fattori Chiave della Soddisfazione del Cliente

#### Situazione Attuale
Esperienza del cliente complessivamente positiva, correlata principalmente alla performance logistica.

#### Best Practice da Consolidare
Mantenere l'approccio conservativo nelle stime dei tempi di consegna, particolarmente efficace per ordini multi-venditori.

#### Azioni Prioritarie

**1. Tempo di Consegna**
- Ridurre la varianza dei tempi di consegna, con il focus sulle vendite da un unico venditore (segmento a maggiore volume).

**2. Qualità del Prodotto**
- Implementare controlli più rigorosi sulla qualità dei prodotti spediti.
- Migliorare i processi di verifica del prodotto prima della spedizione.

**3. Piano d'Azione**
- Investigare e risolvere il collo di bottiglia identificato nella fase di approvazione del pagamento.
- Migliorare i protocolli di comunicazione con i clienti durante i ritardi.
- Rafforzare gli standard di qualità dei venditori e il monitoraggio.

---

### Riepilogo: Il Percorso da Seguire

L'analisi rivela che, sebbene Olist mantenga una forte soddisfazione complessiva del cliente, miglioramenti strategici nell'efficienza logistica e nel controllo qualità dei prodotti porteranno a risultati ancora migliori. Le stime conservative di consegna per ordini complessi si dimostrano efficaci e dovrebbero essere mantenute, mentre il focus operativo dovrebbe spostarsi sulla razionalizzazione del processo di approvazione dei pagamenti e sulla garanzia di una qualità del prodotto costante tra tutti i venditori.

---

*Report preparato utilizzando Power BI, Python (Pandas, Seaborn) e tecniche di analytics avanzate.*
