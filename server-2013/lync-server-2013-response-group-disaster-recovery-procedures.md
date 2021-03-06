﻿---
title: 'Lync Server 2013: Procedure per il ripristino di emergenza dei Response Group'
TOCTitle: Procedure per il ripristino di emergenza dei Response Group
ms:assetid: b49577b7-0ca3-4f20-b614-f3a2a0046b58
ms:mtpsurl: https://technet.microsoft.com/it-it/library/JJ205186(v=OCS.15)
ms:contentKeyID: 49301725
ms.date: 08/24/2015
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Procedure per il ripristino di emergenza dei Response Group in Lync Server 2013

 

_**Ultima modifica dell'argomento:** 2012-11-01_

Durante la fase di failover del ripristino di emergenza, i Response Group risiedono in più pool, ovvero nel pool principale (che non è disponibile) e nel pool di backup. I Response Group in entrambi i pool hanno lo stesso nome e lo stesso proprietario (il pool principale), ma hanno elementi padre diversi. Nel corso di questo periodo di tempo, i cmdlet di Response Group funzionano in modo leggermente diverso. Prestare attenzione a utilizzare i parametri esattamente come specificato nella procedura seguente. Per informazioni dettagliate sul funzionamento dei cmdlet durante la fase di failover, vedere l'articolo del blog NextHop sul ripristino dei Response Group durante il ripristino di emergenza in Lync Server 2013 all'indirizzo [http://go.microsoft.com/fwlink/p/?LinkId=263957](http://go.microsoft.com/fwlink/p/?linkid=263957). Questo articolo del blog si applica anche alla versione definitiva di Lync Server 2013.

Utilizzare i passaggi della procedura riportata di seguito per effettuare la preparazione ed eseguire il ripristino di emergenza per Lync Server, Response Group Service.

## Per eseguire il failover e il failback di Response Group

1.  Avviare Lync Server Management Shell: fare clic sul pulsante **Start**, scegliere **Tutti i programmi**, **Microsoft Lync Server 2013** e quindi **Lync Server Management Shell**.

2.  Eseguire regolarmente i backup. Nella riga di comando digitare quanto segue:
    
        Export-CsRgsConfiguration -Source "service:ApplicationServer:<primary pool FQDN>" -FileName "<backup path and file name>"
    
    Ad esempio:
    
        Export-CsRgsConfiguration -Source "service:ApplicationServer:primary.contoso.com" -FileName "C:\RgsExportPrimary.zip"

3.  Durante un'interruzione del servizio, dopo il failover al pool di backup, importare i Response Group nel pool di backup. Nella riga di comando digitare quanto segue:
    
        Import-CsRgsConfiguration -Destination "service:ApplicationServer:<backup pool FQDN>" -FileName "<backup path and file name>"
    
    Se si desidera sostituire le impostazioni a livello di applicazione nel pool di backup con le impostazioni del pool principale, includere il parametro –ReplaceExistingSettings. Ad esempio:
    
        Import-CsRgsConfiguration -Destination "service:ApplicationServer:backup.contoso.com" -FileName "C:\RgsExportPrimary.zip" -ReplaceExistingSettings
    
    > [!Caution]  
    > Se non si sostituiscono le impostazioni nel pool di backup e il pool principale non può essere ripristinato, le impostazioni del pool principale andranno perse. Per informazioni dettagliate, vedere <a href="lync-server-2013-planning-for-response-group-disaster-recovery.md">Pianificazione per il ripristino di emergenza dei Response Group in Lync Server 2013</a>.

4.  Verificare che l'importazione sia stata eseguita correttamente visualizzando i Response Group importati. Tali Response Group sono ancora di proprietà del pool principale. Eseguire le operazioni seguenti:
    
      - Visualizzare tutti i flussi di lavoro nel pool di backup che sono di proprietà del pool principale e verificare che siano inclusi tutti i flussi di lavoro del pool principale. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsWorkflow -Identity "service:ApplicationServer:<backup pool FQDN>" -Owner "service:ApplicationServer"<primary pool FQDN>
        
        Ad esempio:
        
            Get-CsRgsWorkflow -Identity "service:ApplicationServer:backup.contoso.com" -Owner "service:ApplicationServer:primary.contoso.com"
    
      - Visualizzare tutte le code nel pool di backup che sono di proprietà del pool principale e verificare che siano incluse tutte le code del pool principale. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsQueue -Identity "service:ApplicationServer:<backup pool FQDN>" -Owner "service:ApplicationServer"<primary pool FQDN>
        
        Ad esempio:
        
            Get-CsRgsQueue -Identity "service:ApplicationServer:backup.contoso.com" -Owner "service:ApplicationServer"primary.contoso.com"
    
      - Visualizzare tutti i gruppi di agenti nel pool di backup che sono di proprietà del pool principale e verificare che siano inclusi tutti i gruppi di agenti del pool principale. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsAgentGroup -Identity "service:ApplicationServer:<backup pool FQDN>" -Owner "service:ApplicationServer"<primary pool FQDN>
        
        Ad esempio:
        
            Get-CsRgsAgentGroup -Identity "service:ApplicationServer:backup.contoso.com" -Owner "service:ApplicationServer"primary.contoso.com"
    
      - Visualizzare tutti gli orari di ufficio nel pool di backup che sono di proprietà del pool principale e verificare che siano inclusi tutti gli orari di ufficio del pool principale. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsHoursOfBusiness -Identity "service:ApplicationServer:<backup pool FQDN>" -Owner "service:ApplicationServer"<primary pool FQDN>
        
        Ad esempio:
        
            Get-CsRgsHoursOfBusiness -Identity "service:ApplicationServer:backup.contoso.com" -Owner "service:ApplicationServer"primary.contoso.com"
    
      - Visualizzare tutti i set di festività nel pool di backup che sono di proprietà del pool principale e verificare che siano inclusi tutti i set di festività del pool principale. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsHolidaySet -Identity "service:ApplicationServer:<backup pool FQDN>" -Owner "service:ApplicationServer"<primary pool FQDN>
        
        Ad esempio:
        
            Get-CsRgsHolidaySet -Identity "service:ApplicationServer:backup.contoso.com" -Owner "service:ApplicationServer"primary.contoso.com"
    
    In alternativa, è possibile visualizzare tutti i Response Group presenti nel pool di backup, inclusi quelli di proprietà del pool principale e quelli di proprietà del pool di backup, utilizzando il parametro –ShowAll anziché il parametro –Owner. Ad esempio:
    
        Get-CsRgsWorkflow -Identity "service:ApplicationServer:<backup pool FQDN>" -ShowAll
    
    > [!important]  
    > È necessario utilizzare il parametro –ShowAll oppure il parametro –Owner, altrimenti i Response Group importati nel pool di backup non verranno elencati nei risultati restituiti dai cmdlet.

5.  Verificare che l'importazione sia stata eseguita correttamente effettuando una chiamata a un Response Group importato e controllando che la chiamata venga gestita correttamente.

6.  Richiedere agli agenti membri dei gruppi di agenti formali di connettersi ai rispettivi gruppi nel pool di backup.

7.  Gestire e modificare come di consueto i Response Group importati.
    
    > [!important]  
    > Mentre i Response Group si trovano nel pool di backup, è necessario utilizzare Lync Server Management Shell per gestirli. Non è possibile utilizzare il Pannello di controllo di Lync Server per gestire i Response Group importati nel pool di backup.

8.  Dopo il ripristino del pool principale e il completamento del failback, esportare i Response Group del pool principale che erano stati importati nel pool di backup. Nella riga di comando digitare quanto segue:
    
        Export-CsRgsConfiguration -Source ApplicationServer:<backup pool FQDN> -Owner ApplicationServer:<primary pool FQDN> -FileName "<backup path and file name>"

9.  Reimportare i Response Group nel pool principale. Nella riga di comando digitare quanto segue:
    
        Import-CsRgsConfiguration -Destination "service:ApplicationServer:<primary pool FQDN>" -OverwriteOwner -FileName "<exported path and file name>"
    
    Ad esempio:
    
        Import-CsRgsConfiguration -Destination "service:ApplicationServer:primary.contoso.com" -OverwriteOwner -FileName "C:\RgsExportPrimaryUpdated.zip"
    

    > [!NOTE]
    > Se si rigenera un pool durante il ripristino, con lo stesso o con un altro nome di dominio completo (FQDN), è necessario utilizzare il parametro –OverwriteOwner. Come regola generale, è sempre possibile utilizzare il parametro –OverwriteOwner quando si reimportano i Response Group nel pool principale.

    
    Se è stato distribuito un nuovo pool (con lo stesso o con un altro FQDN) per sostituire il pool principale e si desidera utilizzare le impostazioni a livello di applicazione del pool di backup per il nuovo pool, includere il parametro –ReplaceExistingSettings. Nella riga di comando digitare quanto segue:
    
        Import-CsRgsConfiguration -Destination "service:ApplicationServer:<new primary pool FQDN>" -OverwriteOwner -FileName "<exported path and file name>" -ReplaceExistingSettings
    
    Ad esempio:
    
        Import-CsRgsConfiguration -Destination "service:ApplicationServer:newprimary.contoso.com" -OverwriteOwner -FileName "C:\RgsExportPrimaryUpdated.zip" -ReplaceExistingSettings
    
    > [!important]  
    > Se non si desidera sostituire le impostazioni a livello di applicazione e il file audio predefinito della musica di attesa per il nuovo pool con le impostazioni del pool di backup, il nuovo pool utilizzerà le impostazioni predefinite a livello di applicazione.

10. Verificare che la reimportazione nel pool principale sia stata eseguita correttamente visualizzando la configurazione di Response Group importata. Eseguire le operazioni seguenti:
    
      - Visualizzare tutti i flussi di lavoro nel pool principale e verificare che siano inclusi tutti i flussi di lavoro importati. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsWorkflow -Identity "service:ApplicationServer:<primary pool FQDN>" -ShowAll
        
        Ad esempio:
        
            Get-CsRgsWorkflow -Identity "service:ApplicationServer: primary.contoso.com" -ShowAll
    
      - Visualizzare tutte le code nel pool principale e verificare che siano incluse tutte le code importate. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsQueue -Identity "service:ApplicationServer:<primary pool FQDN>" -ShowAll
        
        Ad esempio:
        
            Get-CsRgsQueue -Identity "service:ApplicationServer:primary.contoso.com" -ShowAll
    
      - Visualizzare tutti i gruppi di agenti nel pool principale e verificare che siano inclusi tutti i gruppi di agenti importati. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsAgentGroup -Identity "service:ApplicationServer: <primary pool FQDN>" -ShowAll
        
        Ad esempio:
        
            Get-CsRgsAgentGroup -Identity "service:ApplicationServer:primary.contoso.com" -ShowAll
    
      - Visualizzare tutti i gli orari di ufficio nel pool principale e verificare che siano inclusi tutti gli orari di ufficio importati. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsHoursOfBusiness -Identity "service:ApplicationServer:<primary pool FQDN>" -ShowAll
        
        Ad esempio:
        
            Get-CsRgsHoursOfBusiness -Identity "service:ApplicationServer:primary.contoso.com" -ShowAll
    
      - Visualizzare tutti i set di festività nel pool principale e verificare che siano inclusi tutti i set di festività importati. Nella riga di comando digitare quanto segue:
        
            Get-CsRgsHolidaySet -Identity "service:ApplicationServer:<primary pool FQDN>" -ShowAll
        
        Ad esempio:
        
            Get-CsRgsHolidaySet -Identity "service:ApplicationServer:primary.contoso.com" -ShowAll

11. Verificare che l'importazione sia stata eseguita correttamente effettuando una chiamata a un Response Group importato e controllando che la chiamata venga gestita correttamente.

12. Facoltativamente, rimuovere dal pool di backup i Response Group di proprietà del pool principale. Nella riga di comando digitare quanto segue:
    
        Export-CsRgsConfiguration -Source "service:ApplicationServer:<backup pool FQDN>" -Owner "service:ApplicationServer:<primary pool FQDN>" -FileName "<backup path and file name>" -RemoveExportedConfiguration
    
    Ad esempio:
    
        Export-CsRgsConfiguration -Source "service:ApplicationServer:backup.contoso.com" -Owner "service:ApplicationServer:primary.contoso.com" -FileName "C:\RgsExportPrimaryUpdated.zip" -RemoveExportedConfiguration
    

    > [!NOTE]
    > Questo passaggio consente di creare un nuovo file con la configurazione esportata e quindi di rimuoverlo dal pool di backup.


