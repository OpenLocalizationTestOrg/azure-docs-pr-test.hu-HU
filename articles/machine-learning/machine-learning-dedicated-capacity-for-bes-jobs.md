---
title: "a Machine Learning kötegelt végrehajtási szolgáltatás feladatok kapacitás aaaDedicated |} Microsoft Docs"
description: "A gépi tanulási feladatok Azure Batch-szolgáltatások áttekintése."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: bba7970bb31c50e5b0b9d5f4ff4f0d2dacf942e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a>A gépi tanulási feladatok az Azure Batch szolgáltatás

Gépi tanulás Batch-készlet feldolgozása ügyfél által felügyelt méretezési biztosít hello Azure Machine Learning kötegelt végrehajtási szolgáltatás. A machine Learning szolgáltatáshoz klasszikus kötegfeldolgozási egy több-bérlős környezet, mely korlátok hello száma egyidejűleg futó feladatainak is elküldhetik, és feladatok várólistára első-first out alapon történik. Ez a bizonytalanság azt jelenti, hogy Ön nem pontosan előre jelezni mikor fog futni a feladat.

Kötegfeldolgozási készlet lehetővé teszi toocreate készletek, amelyre elküldheti a kötegelt feladatok. Hello készlet mérete hello szabályozhatja, és toowhich készlet hello feladat küldése. A BES feladat fut, a saját területet biztosít feldolgozása előre jelezhető feldolgozás és hello képességét toocreate-erőforráskészleteket, amelyekben felel meg az elküldött toohello feldolgozási terhelés.

## <a name="how-toouse-batch-pool-processing"></a>Hogyan toouse Batch-készlet feldolgozása

A Batch-készlet feldolgozási konfigurációs már nem érhető el hello Azure-portálon keresztül. Batch-készlet toouse feldolgozása, meg kell:

-   Meghívja a CSS toocreate készlet Batch-fiók, és szerezze be a készlet szolgáltatás URL-címe és engedélykulcs
-   Hozzon létre egy új erőforrás-kezelő alapú webszolgáltatás és a számlázási csomag

toocreate a fiókjához, hívja a Microsoft ügyfélszolgálata és a támogatási szolgálathoz (CSS), és adja meg, az előfizetés-azonosító. CSS kapacitással rendelkező átjáróeszközt, toodetermine hello megfelelő helyzetnek fog működni. CSS ezután konfigurálja a fiók hello készleteket hozhat létre és virtuális gépek (VM) elhelyezheti mindegyik készlet maximális száma hello maximális számát. A fiók van konfigurálva, ha rendelkezésre állnak a készlet szolgáltatás URL-címe és engedélykulcs.

Miután a fiókja létrejött, az hello készlet szolgáltatás URL-címe és a hitelesítési kulcs tooperform készlet a felügyeleti műveleteihez a Batch-készlet.

![Kötegelt készlet szolgáltatás architektúrája.](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

Hello készlet létrehozása művelet hello készlet szolgáltatás URL-CÍMÉT, hogy a megadott CSS tooyou meghívásával készletek hoz létre. Készletet hoz létre, amikor adja meg, virtuális gépek és hello URL-CÍMÉT egy új Resource Manager hello swagger.json hello száma Machine Learning webszolgáltatáshoz. Ez a webszolgáltatás tooestablish hello számlázási társítás valósul meg. hello Batch-készlet szolgáltatás hello swagger.json tooassociate hello készletet használ egy számlázási csomagot. Futtathatja a BES webszolgáltatás, mindkét alapú új erőforrás-kezelő és a klasszikus, hello készletében.

Bármely új Resource Manager-alapú webszolgáltatás, de vegye figyelembe, hogy hello számlázási hello feladatok elleni hello szolgáltatáshoz tartozó számlázási csomag van-e szó. Érdemes lehet egy webszolgáltatás és az új számlázási megtervezése toocreate kifejezetten a futó feladatok Batch-készlet.

Webszolgáltatások létrehozásával kapcsolatos további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).

Miután létrehozta a készletben, elküldését hello BES hello webszolgáltatás feladat használ hello kötegelt kérelmek URL-CÍMÉT. Kiválaszthatja a toosubmit azt tooa készletet vagy tooclassic kötegfeldolgozási. egy feladat tooBatch készlet feldolgozása toosubmit, hozzáadása a következő paraméter toohello feladat elküldése kérelemtörzset hello:

"AzureBatchPoolId": "&lt;azonosító tárolókészlet&gt;"

Ha nem adja hozzá hello paramétert, hello feladat hello klasszikus kötegelt folyamat környezetében futtatható. Hello készlet elegendő erőforrás áll rendelkezésre, ha a hello feladat azonnal futásának indításakor. Hello készlet nem rendelkezik szabad erőforrást, ha a feladat várólistára van állítva, amíg egy erőforrás áll rendelkezésre.

Ha talál meg, hogy rendszeresen eléri a készletek hello kapacitását, és nagyobb kapacitást van szüksége, CSS hívja, és a kvóták egy reprezentatív tooincrease dolgozni.

Példa egy kérelem:

https://ussouthcentral.Services.azureml.NET/Subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/Jobs?API-Version=2.0

```json
{

    "Input":{
    
        "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpoint
        =https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://zhguim
        l.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;;",
        
        "BaseLocation":null,
        
        "RelativeLocation":"testint/AdultCensusIncomeBinaryClassificationDataset.csv",
        
        "SasBlobToken":null
    
    },
    
    "GlobalParameters":{ },
    
    "Outputs":{
    
        "output1":{
        
            "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpo
            int=https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://sampleaccount.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;",
            "BaseLocation":null,
            "RelativeLocation":"testintoutput/testint\_results.csv",
            
            "SasBlobToken":null
        
        }
    
    },
    
    "AzureBatchPoolId":"8dfc151b0d3e446497b845f3b29ef53b"

}
```

## <a name="considerations-when-using-batch-pool-processing"></a>Batch-készlet feldolgozási használatának szempontjai

Batch-készlet feldolgozásakor – a számlázható szolgáltatás pedig, amely szükséges az erőforrás-kezelő szerint számlázási csomag tooassociate. Csak számlázása hello számú hello készlet fut; számítási óra függetlenül attól, feladatok futtatása során idő készlethez hello száma. Készletet hoz létre, ha díját is felszámítjuk hello számítási órán át az egyes virtuális gépek hello készletben hello készlet nem törlik, még akkor is, ha nincsenek kötegelt feladatok futnak hello készletben. Számlázási hello virtuális gépek akkor kezdődik, amikor a kiépítés után, és leáll, amikor törölve lettek. Bármelyik hello csomagok hello használható [Machine Learning díjszabás lap](https://azure.microsoft.com/pricing/details/machine-learning/).

Számlázási. példa:

Ha a Batch-készlet létrehozása 2 virtuális gépekkel és 24 óra múlva törli azt a számlázási csomag terhelt számítási 48 óra; függetlenül attól, hogy hány feladat futtatná adott időszakban.

Ha a Batch-készlet létrehozása 4 virtuális gépekkel és 12 óra elteltével törölje, a számlázási csomag is számítási terhelést 48 óra.

Azt javasoljuk, hogy hello feladat állapota toodetermine lekérdezésére feladat befejeződése után. Ha a feladatok futása befejeződött, hello készlet toozero hívás hello átméretezési készlet művelet tooset hello virtuális gépek száma. Ha rövid készlet erőforrások és toocreate egy új készletet, például egy másik számlázási csomag elleni toobill kell törölheti hello készlet helyette ha összes feladat futása befejeződött.


| **Használja a kötegelt mikor feldolgozása**    | **Használja a klasszikus kötegfeldolgozási végezhető, ha**  |
|---|---|
|Feladatok nagy számú toorun van szüksége<br>Vagy<br/>A feladatok által futtatott azonnal tooknow van szüksége<br/>Vagy<br/>Garantált átviteli van szüksége. Például meg kell toorun egy adott időszakon feladatok száma, és szeretné, hogy tooscale a számítási erőforrások toomeet ki az igényeinek.    | Néhány feladatot futtat<br/>És<br/> A felesleges hello feladatok toorun azonnal |
