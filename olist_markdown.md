---
layout: page
title: Customer Satisfaction Analysis
---

# Olist E-Commerce

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

### Obiettivi del Progetto

**Identificare i principali fattori dell'esperienza d'acquisto associati alla soddisfazione del cliente per Olist e proporre azioni di miglioramento adeguate.**

### Business Case

Olist è una piattaforma brasiliana che supporta i venditori nella gestione delle vendite online tramite marketplace di terze parti. Il dataset include informazioni su ordini, item, pagamenti, logistica, clienti, venditori e recensioni, definendo un modello complesso e multidimensionale.

---

## Architettura dei Dati

### Modello Multidimensionale - Power BI

#### Fact Table: Granularità a Livello Ordini

Per garantire coerenza analitica ed evitare ambiguità dovute alle relazioni molti a molti presenti sul modello, è stata costruita una fact table a livello ordine.

La tabella aggrega informazioni provenienti da più fonti (tabelle 'payments', 'reviews', 'orders' e 'items'), consentendo analisi consistenti sulla soddisfazione del cliente.

---

## Analisi Descrittiva

### Distribuzione Temporale e Pattern delle Valutazioni

#### Stabilità delle Valutazioni nel Tempo

La distribuzione dei voti rimane, nel complesso, stabile nel periodo analizzato. L'unica eccezione è la valutazione più bassa (voto 1), che mostra un lieve aumento tra novembre 2017 e marzo 2018, un possibile segnale di criticità temporanee nel servizio.

#### Tempo di Consegna e Soddisfazione

Gli ordini si concentrano prevalentemente nella fascia standard (6-15 giorni), mentre la categoria molto lenta è marginale.

È evidente una correlazione positiva tra rapidità di consegna e valutazione media, suggerendo che il tempo di consegna potrebbe essere un driver significativo della customer satisfaction.

---

## Analisi Comparativa: Ranking

### Fattori Correlati alle Valutazioni

I pulsanti di filtro nella parte superiore consentono di selezionare la valutazione media e di aggiornare dinamicamente tutte le visualizzazioni, permettendo il confronto tra ordini ben valutati e quelli con valutazioni basse, al fine di esplorare possibili fattori associati all'esito dell'ordine.

#### Insight Principali (Confronto tra Voto 1 e Voto 5)

1. **Impatto del Tempo di Consegna**: Tra le variabili analizzate, il tempo totale di consegna e il ritardo di consegna sono quelle che mostrano la maggiore differenza passando dalle valutazioni più basse a quelle più alte.

2. **Stabilità del Giorno della Settimana**: Contrariamente all'ipotesi iniziale, la distribuzione degli ordini per giorno della settimana rimane sostanzialmente stabile tra le diverse valutazioni.

3. **Complessità Multi-Venditore**: Gli ordini con un solo venditore rappresentano la maggioranza in tutte le classi di voto. Gli ordini con 4-5 venditori, sebbene molto rari, compaiono solo tra quelli con valutazione minima, suggerendo un possibile legame tra la complessità dell'ordine e l'esperienza d'acquisto (più venditori = rischio più elevato di errori o ritardi).

---

## Analisi di Correlazione

Per approfondire l'analisi delle variabili numeriche di interesse, è stata costruita una matrice di correlazione di Pearson utilizzando Python (librerie Pandas e Seaborn).

### Insight Principali

1. **Correlazione Negativa**: Le aree più scure evidenziano una correlazione negativa moderata tra la valutazione media e il tempo di consegna (ritardo di consegna e tempo totale di spedizione), in linea con quanto osservato nelle dashboard precedenti.

2. **Assenza di Correlazione con la Complessità**: Non emerge una correlazione significativa tra i tempi di consegna e la quantità di venditori coinvolti nell'ordine, contrariamente all'ipotesi iniziale di una maggiore complessità logistica. Tuttavia, data la bassa numerosità degli ordini multi-venditore, questi casi risultano poco rappresentativi e potrebbero richiedere un'analisi separata.

---

## Analisi della Complessità Logistica

### Ordini Multi-Venditori

#### Ritardo di Consegna e Numero di Venditori

All'aumentare del numero di venditori, il ritardo di consegna (definito come la differenza tra la data stimata di consegna e la data effettiva di arrivo al cliente) tende a spostarsi verso valori inferiori, sia nella media sia nell'intervallo interquartile.

Questo risultato appare controintuitivo e solleva una domanda: **Perché ordini più complessi mostrano un ritardo minore?**

**Risposta**: Le stime di consegna diventano più conservative con l'aumentare dei venditori.

#### Stima del Tempo di Consegna e Numero di Venditori

L'analisi conferma che Olist applica stime sempre più conservative per gli ordini multi-venditore, gestendo efficacemente le aspettative dei clienti e riducendo i ritardi percepiti.

---

## Analisi del Lead Time

### Identificazione delle Fasi Critiche

Per esplorare in quale fase del flusso di acquisto si concentrano i maggiori ritardi, sono stati analizzati quattro boxplot che confrontano la distribuzione dei giorni trascorsi in ciascuna fase del processo.

I risultati mostrano che i tempi più lunghi si osservano nella fase tra l'acquisto e l'approvazione del pagamento, con una media di circa 25 giorni e un intervallo interquartile compreso tra 20 e 30 giorni. Questa fase presenta anche la maggiore dispersione, indicando una forte variabilità dei tempi di approvazione.

### Suddivisione del Lead Time

- **Tempo tra acquisto e approvazione pagamento**: Più lungo e più variabile
- **Tempo tra approvazione pagamento e spedizione**: Moderato
- **Tempo tra spedizione e arrivo al cliente**: Standard
- **Tempo totale di consegna**: Somma di tutte le fasi

---

## Text Mining delle Recensioni

### Tematiche d'Importanza per i Clienti

I commenti delle recensioni sono stati tradotti in inglese e analizzati in Python tramite una word cloud. Le parole più visibili fanno riferimento a prodotto, consegna e tempi, con una prevalenza di termini dal tono positivo, coerente con la distribuzione complessiva delle valutazioni.

#### Temi Chiave Identificati

**Aspettative sui Tempi di Consegna**
- Tempo di consegna
- Consegna rapida
- Consegna in anticipo
- Puntualità
- Attesa
- Ritardo

**Conformità Prodotto-Descrizione**
- Prodotto corretto
- Conforme alla descrizione
- Come da foto
- Qualità del prodotto
- Aspettative rispettate

**Qualità del Servizio Clienti**
- Assistenza
- Comunicazione
- Risposta
- Contatto
- Rimborso
- Gestione del problema

---

## Sentiment Analysis: Sentimento Positivo

### Tematiche d'Importanza per i Clienti

I clienti che esprimono sentiment positivo spesso menzionano anche la facilità dell'acquisto e il servizio clienti reattivo. Le parole sui tempi di consegna emergono molto, confermando le analisi precedenti che identificano la logistica come fattore chiave di soddisfazione.

#### Temi Positivi Chiave

**Tempi di Consegna**
- Molte recensioni apprezzano l'arrivo anticipato dei prodotti e la puntualità

**Condizioni del Prodotto**
- I clienti evidenziano la buona qualità, la bellezza e la cura nell'imballaggio

**Esperienza d'Acquisto**
- Compaiono termini che indicano un alto livello di soddisfazione complessiva

---

## Sentiment Analysis: Sentimento Negativo

### Tematiche d'Importanza per i Clienti

I problemi con il prodotto ricevuto sembrano influire in modo significativo sul sentiment negativo, più del ritardo o del tempo di attesa prolungato. Alcuni commenti menzionano la comunicazione con il venditore, suggerendo la necessità di un servizio clienti più efficace.

#### Temi Negativi Chiave

**Prodotto Ricevuto**
- I clienti lamentano prodotti rotti, errati o di bassa qualità

**Consegna**
- Problemi legati a ritardi, smarrimenti o all'imballaggio

**Esperienza d'Acquisto**
- Esperienze poco soddisfacenti durante il processo di acquisto e con il servizio clienti

---

## Conclusioni

### Fattori Chiave della Soddisfazione del Cliente

#### Situazione Attuale
Esperienza del cliente complessivamente positiva, correlata principalmente alla performance logistica.

#### Best Practice da Consolidare
Mantenere l'approccio conservativo nelle stime dei tempi di consegna, particolarmente efficace per ordini multi-venditori.

#### Azioni Prioritarie

**1. Tempo di Consegna (Priorità #1)**
- Ridurre la varianza dei tempi di consegna, con il focus sulle vendite da un unico venditore (segmento a maggiore volume)

**2. Qualità del Prodotto (Priorità #2)**
- Implementare controlli più rigorosi sulla qualità dei prodotti spediti
- Migliorare i processi di verifica del prodotto prima della spedizione

**3. Piano d'Azione**
- Investigare e risolvere il collo di bottiglia identificato nella fase di approvazione del pagamento
- Migliorare i protocolli di comunicazione con i clienti durante i ritardi
- Rafforzare gli standard di qualità dei venditori e il monitoraggio

---

### Riepilogo: Il Percorso da Seguire

L'analisi rivela che, sebbene Olist mantenga una forte soddisfazione complessiva del cliente, miglioramenti strategici nell'efficienza logistica e nel controllo qualità dei prodotti porteranno a risultati ancora migliori. Le stime conservative di consegna per ordini complessi si dimostrano efficaci e dovrebbero essere mantenute, mentre il focus operativo dovrebbe spostarsi sulla razionalizzazione del processo di approvazione dei pagamenti e sulla garanzia di una qualità del prodotto costante tra tutti i venditori.

---

*Report preparato utilizzando Power BI, Python (Pandas, Seaborn) e tecniche di analytics avanzate.*
