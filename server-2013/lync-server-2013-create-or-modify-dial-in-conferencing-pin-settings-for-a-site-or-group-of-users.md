﻿---
title: 'Lync Server 2013: Creare o modificare le impostazioni del PIN di conferenza telefonica con accesso esterno per un sito o un gruppo di utenti'
TOCTitle: Creare o modificare le impostazioni del PIN di conferenza telefonica con accesso esterno per un sito o un gruppo di utenti
ms:assetid: c29bab5c-2b93-48e0-ae0b-29564daaff9a
ms:mtpsurl: https://technet.microsoft.com/it-it/library/Gg412959(v=OCS.15)
ms:contentKeyID: 49301900
ms.date: 08/24/2015
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Creare o modificare le impostazioni del PIN di conferenza telefonica con accesso esterno in Lync Server 2013 per un sito o un gruppo di utenti

 

_**Ultima modifica dell'argomento:** 2012-10-18_

Eseguire la procedura seguente per creare o modificare un criterio PIN per conferenze telefoniche con accesso esterno a livello di utente o di sito. Per informazioni dettagliate su come modificare il criterio PIN globale, vedere [Modifica delle impostazioni predefinite dei PIN per le conferenze telefoniche con accesso esterno in Lync Server 2013](lync-server-2013-modify-the-default-dial-in-conferencing-pin-settings.md).

## Per creare criteri per il PIN a livello di utente o sito

1.  Da un account utente membro del gruppo RTCUniversalServerAdmins (o con diritti utente equivalenti) oppure assegnato al ruolo CsServerAdministrator o CsAdministrator, accedere a un computer nella rete in cui è stato distribuito Lync Server 2013.

2.  Aprire una finestra del browser e quindi immettere l'URL di amministrazione per aprire il Pannello di controllo di Lync Server. Per informazioni dettagliate sui diversi metodi disponibili per avviare il Pannello di controllo di Lync Server, vedere [Aprire gli strumenti di amministrazione di Lync Server](lync-server-2013-open-lync-server-administrative-tools.md).

3.  Sulla barra di spostamento sinistra fare clic su **Servizio di conferenza** e quindi su **Criteri PIN** .

4.  Nella pagina **Criteri PIN** fare clic su **Nuovo** e quindi eseguire una delle operazioni seguenti:
    
      - Per creare criteri a livello di utente fare clic su **Criteri utente** . In **Nuovi criteri PIN** digitare un nome descrittivo dei criteri in **Nome** .
    
      - Per creare criteri a livello di sito, fare clic su **Criteri sito** . Nel campo di ricerca **Seleziona un sito** digitare il nome del sito per cui si desidera creare i criteri, per intero o in parte. Fare clic sul sito desiderato nell'elenco dei siti e quindi fare clic su **OK** .

5.  Nel campo **Descrizione** digitare una descrizione per i criteri per il PIN.

6.  In **Lunghezza minima PIN** digitare o selezionare la lunghezza minima che si desidera consentire per il PIN. La lunghezza minima predefinita è di cinque cifre.

7.  Per poter specificare il numero massimo di tentativi di accesso prima che un utente venga bloccato, selezionare la casella di controllo **Specifica numero massimo di tentativi di accesso** . Se non si seleziona questa opzione, il numero massimo di tentativi consentiti viene determinato automaticamente in base alla lunghezza del PIN. Per impostazione predefinita, il numero massimo di tentativi viene determinato automaticamente.

8.  Se si seleziona la casella di controllo **Specifica numero massimo di tentativi di accesso** , in **Numero massimo di tentativi di accesso** digitare o selezionare il numero massimo di tentativi di accesso che si desidera consentire.

9.  Per impostare una scadenza per i PIN, selezionare la casella di controllo **Abilita scadenza PIN** . Se non si seleziona questa opzione, i PIN non scadranno mai, come da impostazione predefinita.

10. Se si seleziona la casella di controllo **Abilita scadenza PIN** , in **Scadenza PIN dopo (giorni)** digitare o selezionare il numero di giorni trascorsi i quali scadranno i PIN.

11. In **Conteggio cronologia PIN** digitare il numero di PIN che deve creare un utente prima che possa riutilizzare un PIN. Per impostazione predefinita, gli utenti possono riutilizzare i loro PIN.

12. Per consentire schemi comuni di cifre nei PIN, ad esempio numeri progressivi o gruppi ripetuti di numeri, selezionare la casella di controllo **Consenti formati comuni** . Se non si seleziona questa opzione, saranno consentiti solo schemi complessi di cifre, come da impostazione predefinita.
    
    > [!important]  
    > È consigliabile evitare di consentire i formati comuni.

13. Fare clic su **Commit** .

## Per modificare criteri per il PIN a livello di utente o sito

1.  Da un account utente membro del gruppo RTCUniversalServerAdmins (o con diritti utente equivalenti) oppure assegnato al ruolo CsServerAdministrator o CsAdministrator, accedere a un computer nella rete in cui è stato distribuito Lync Server 2013.

2.  Aprire una finestra del browser e quindi immettere l'URL di amministrazione per aprire il Pannello di controllo di Lync Server. Per informazioni dettagliate sui diversi metodi disponibili per avviare il Pannello di controllo di Lync Server, vedere [Aprire gli strumenti di amministrazione di Lync Server](lync-server-2013-open-lync-server-administrative-tools.md).

3.  Sulla barra di spostamento sinistra fare clic su **Servizio di conferenza** e quindi su **Criteri PIN** .

4.  Nella pagina **Criteri PIN** fare clic sui criteri per il PIN che si desidera modificare, fare clic su **Modifica** e quindi su **Mostra dettagli** .

5.  In **Modifica criteri PIN** modificare le impostazioni dei criteri nel modo desiderato, con l'eccezione del nome che non è modificabile.

6.  Fare clic su **Commit** .

