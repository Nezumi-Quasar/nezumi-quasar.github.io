---
layout: post
title: DataContractSerializer Vs XmlSerializer
published: true
author: emanueleg
comments: true
date: 2010-07-06 01:07:04
tags:
    - .Net Framework
    - Serialization
categories:
    - uncategorized
permalink: /2010/07/06/datacontractserializer-vs-xmlserializer
---
Serializzatore&hellip;quale scegliere e perch&egrave;? 

Diciamo, innanzitutto, che dal framework 3.0 abbiamo la possibilit&agrave; di scegliere come serializzare un grafo di oggetti (che bello ora abbiamo l&rsquo;imbarazzo della scelta T_T), ma in base a cosa scelgo? beh&hellip; ho fatto un p&ograve; di test ed ho evidenziato quelli che, dal mio punto di vista, potrebbero essere pregi e difetti dell&rsquo;uno e dell&rsquo;altro.Iniziamo rispettando i pi&ugrave; anziani:

XmlSerializer:

  * Serializza tutte le property , tranne quelle che gli viene detto di ignorare (ricordate che lavora solo sulle property pubbliche) 
  * Controllo completo sull&#8217;xml generato (posso scegliere quale property rendere un attribute e cosa un element)

DataContractSerializer:

  * Serializza solo quello che viene decorato come da serializzare (anche datamember privati) 
  * pu&ograve; serializzare classi decorate con l&#8217;attributo \[Serializable\] (lo vedo un pregio se voglio usare questo serializzatore senza dover ridecorare tutte le mie classi,ma guardando l&rsquo;xml generato&hellip;sembra quasi un difetto) 
  * ho letto su internet che rende la serializzazione pi&ugrave; veloce del 10% rispetto ad XmlSerializer ma sui test fatti da me siamo su un ordine leggermente diverso 
      * su 1 elemento 
          * DataContractSerializer 34ms&nbsp; 
          * XmlSerializer 204ms
      * su 900000 elementi (senza oggetti innestati) 
          * DataContractSerializer 1863ms&nbsp; 
          * XmlSerializer 3750ms
      * su 900000 elementi (di cui 9000 con altri oggetti contenuti che contengono a loro volta altri 3000 oggetti) 
          * DataContractSerializer 2458ms&nbsp; 
          * XmlSerializer 3691ms 
  * Crea solo element e non attribute (su questo voglio indagare meglio, ma finora non ho avuto modo di fargli creare attribute), l&rsquo;unico controllo che ho sui membri decorati con [DataMember] sono &ldquo;IsRequired, Name, Order, DefaultValue&rdquo;

&nbsp;

considerando il divario prestazionale che si &egrave; creato tra le due serializzazioni, direi che se devo scegliere come serializzare, indipendentemente dal modo (nodi ed attributi), beh&hellip;non ho molti dubbi su cosa scegliere (anche se non vi nascondo che voglio fare ancora qualche test, magari con un grafo pi&ugrave; complesso)

viceversa se voglio che il mio xml abbia una struttura ben precisa (magari che rispecchi uno Schema XSD contenente anche degli attributi) beh devo adattare il mio sviluppo e le mie classi all&rsquo;utilizzo con XmlSerializer perch&egrave; &egrave; lui la scelta migliore

Se non mi preoccupano ne prestazioni ne schema, sceglierei (personalmente) DataContractSerializer perch&egrave; le decorazioni necessarie rendono pi&ugrave; chiaro, per chi guarda le nostre classi, cosa intendiamo esporre 

&nbsp;

sperando come sempre di essere stato utile a qualcuno attendo eventuali vostri rimproveri o suggerimenti

E.