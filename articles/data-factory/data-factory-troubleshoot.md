---
title: "aaaTroubleshoot Azure Data Factory problémák"
description: "Ismerje meg, hogyan tootroubleshoot állít ki az Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: spelluru
ms.openlocfilehash: cf65bcf3e1c3f061d3ac1dbf32e99cc7b014f9dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-data-factory-issues"></a>Data Factory-hibák elhárítása
Ez a cikk biztosítja a hibaelhárítási tippekért problémák Azure Data Factory használatával. Ez a cikk nem szerepel minden hello lehetséges problémákat hello szolgáltatás használatakor, de bizonyos problémák és általános hibaelhárítási tippek magában foglalja.   

## <a name="troubleshooting-tips"></a>Hibaelhárítási tippek
### <a name="error-hello-subscription-is-not-registered-toouse-namespace-microsoftdatafactory"></a>Hiba: hello előfizetés nincs regisztrált toouse névtér "Microsoft.DataFactory"
Ha ez a hibaüzenet jelenik meg, hello Azure Data Factory erőforrás-szolgáltató nincs regisztrálva a számítógépen. A következő hello:

1. Indítsa el az Azure PowerShellt.
2. Jelentkezzen be tooyour Azure-fiók hello a következő parancs használatával.

    ```powershell
    Login-AzureRmAccount
    ```
3. Futtassa a következő parancs tooregister hello Azure Data Factory szolgáltató hello.

    ```powershell        
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Probléma: Nem engedélyezett hiba a Data Factory parancsmag futtatásakor
Az Azure PowerShell hello hello jobb Azure-fiók vagy előfizetés valószínűleg nem használ. A következő parancsmagok tooselect hello jobb hello Azure PowerShell-lel rendelkező Azure fiókja és -előfizetése toouse hello használata.

1. Login-AzureRmAccount - használata hello megfelelő felhasználói Azonosítót és jelszót
2. Get-AzureRmSubscription – összes hello hello fiókhoz előfizetések nézet.
3. SELECT-AzureRmSubscription &lt;előfizetés neve&gt; -válasszon hello megfelelő előfizetést. Használjon hello azonos toocreate egy adat-előállító hello Azure-portált használja.

### <a name="problem-fail-toolaunch-data-management-gateway-express-setup-from-azure-portal"></a>Hiba: Sikertelen toolaunch adatok Management Gateway Express telepítése Azure-portálon
hello Expressz telepítés az adatkezelési átjáró hello Internet Explorer vagy a Microsoft ClickOnce kompatibilis böngésző szükséges. Ha hello Express telepítő toostart sikertelen, hello következő módokon:

* Az Internet Explorer vagy a Microsoft ClickOnce kompatibilis böngésző használata.

    Ha a Chrome használ, lépjen a toohello [Chrome webes tároló](https://chrome.google.com/webstore/), keressen a "ClickOnce" kulcsszóval, válasszon egyet a hello ClickOnce kiterjesztések, és telepítse.

    Hello azonos Firefox (install-bővítmény). Hello eszköztáron (három vízszintes vonal hello jobb felső sarokban) menü megnyitása gombra, kattintson a bővítmények, keressen a "ClickOnce" kulcsszóval egyikével hello ClickOnce kiválasztása, és telepítse.
* Használjon hello **manuális telepítés** hivatkozás jelenik meg a hello hello portálon azonos panelen. Ez a megközelítés toodownload telepítőfájlt használ, és manuális futtatása. Hello telepítés befejezését követően hello adatok adatkezelési átjáró konfigurálása párbeszédpanel jelenik meg. Másolás hello **kulcs** hello Letöltés gombra, és használja azt a hello configuration manager toomanually hello átjáró regisztrálása hello szolgáltatásban.  

### <a name="problem-fail-tooconnect-tooon-premises-sql-server"></a>Hiba: Sikertelen tooconnect tooon helyszíni SQL Server
Indítsa el **az adatkezelési átjáró konfigurációkezelőjének** az átjárót működtető gépen hello és hello használata **hibaelhárítás** tootest hello kapcsolat tooSQL Server hello átjáró gépről fülre. Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Probléma: Ha valaha is várakozó állapotban vannak bemeneti szeletek
lehet, hogy hello szeletek a **Várakozás** állapot toovarious okok miatt. Hello gyakori okai egyik adott hello **külső** tulajdonság értéke nem túl**igaz**. Bármely adatkészlet hatókörében létrehozott külső hello Azure Data Factory, fel kell tüntetni **külső** tulajdonság. Ez a tulajdonság jelzi, hogy hello adatok-e a külső és a nem mentett minden belül hello adat-előállító adatcsatornák által. hello adatszeletek fel van tüntetve **készen** hello adatok elérhetővé válik a hello megfelelő áruházban.

Tekintse meg a következő példa hello használati hello hello **külső** tulajdonság. Opcionálisan megadhat **externalData*** külső tootrue beállításakor.

Lásd: [adatkészletek](data-factory-create-datasets.md) cikk részletesebb ezt a tulajdonságot.

```json
{
  "name": "CustomerTable",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "MyLinkedService",
    "typeProperties": {
      "folderPath": "MyContainer/MySubFolder/",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      }
    }
  }
}
```

tooresolve hello hiba, vegye fel a hello **külső** tulajdonság és az opcionális hello **externalData** toohello JSON-definícióból hello bemeneti tábla szakaszt, és hozza létre újra a hello tábla.

### <a name="problem-hybrid-copy-operation-fails"></a>Probléma: Hibrid másolás nem lehetséges
Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) vonatkozó lépéseket tootroubleshoot problémákat másolása onnan egy a helyszíni adatok tárolására használatával hello az adatkezelési átjáró.

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Probléma: Igény szerinti HDInsight kiépítése sikertelen
HDInsightOnDemand típusú társított szolgáltatás használatakor a linkedServiceName mutat, tooan Azure Blob Storage toospecify kell. Data Factory szolgáltatásnak használ a tárolási toostore naplók és egyéb szükséges fájlokat a igény szerinti HDInsight-fürthöz.  Egyes esetekben az igény szerinti HDInsight-fürtök kiépítése meghiúsul, és a következő hiba hello:

```
Failed toocreate cluster. Exception: Unable toocomplete hello cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

Ez a hiba általában azt jelzi, hogy hello linkedServiceName megadott hello tárfiók hello helye nem a hello azonos adatközpont-helyet, ahol hello HDInsight kiépítése történik. Példa: Ha a data factory USA nyugati régiója és az Azure storage hello az USA keleti régiója, hello igény létesítési meghiúsul az USA nyugati régiója.

Az additionalLinkedServiceNames egy másik JSON-tulajdonság, amelyben további tárfiókok adhatók meg az igény szerinti HDInsight szolgáltatáshoz. Ezen további kapcsolt tárfiókot kell hello ugyanazon a helyen hello HDInsight-fürtöt, vagy nem sikerül a hello ugyanezt a hibaüzenetet.

### <a name="problem-custom-net-activity-fails"></a>Probléma: Egyéni .NET tevékenység sikertelen
Lásd: [folyamat hibakeresése egyéni tevékenységeket](data-factory-use-custom-activities.md#troubleshoot-failures) a részletes lépéseket.

## <a name="use-azure-portal-tootroubleshoot"></a>Az Azure portál tootroubleshoot használata
### <a name="using-portal-blades"></a>Portál paneleken használatával
Lásd: [figyelő folyamat](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) lépéseket.

### <a name="using-monitor-and-manage-app"></a>A Megfigyelés és kezelés alkalmazás használata
Lásd: [figyelése és kezelése a data factory folyamatok figyelése és kezelése App](data-factory-monitor-manage-app.md) részleteket.

## <a name="use-azure-powershell-tootroubleshoot"></a>Azure PowerShell tootroubleshoot használata
### <a name="use-azure-powershell-tootroubleshoot-an-error"></a>Használja az Azure PowerShell tootroubleshoot hiba
Lásd: [képernyő adat-előállító folyamatok, az Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) részleteiről.

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
