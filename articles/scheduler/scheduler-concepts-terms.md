---
title: "aaaScheduler fogalmakat, feltételek és entitások |} Microsoft Docs"
description: "Az Azure Scheduler alapfogalmai, entitáshierarchiája és terminológiája, beleértve a feladatokat és a feladatgyűjteményeket.  Egy ütemezett feladat átfogó példáját mutatja be."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 3ef16fab-d18a-48ba-8e56-3f3e0a1bcb92
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 73e7de7bfd2937e401aeab05e0e10fa292cf37b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>A Scheduler alapfogalmai, entitáshierarchiája és terminológiája
## <a name="scheduler-entity-hierarchy"></a>A Scheduler entitáshierarchiája
hello következő táblázatban hello fő erőforrások elérhetővé tett vagy hello Scheduler API által használt.

| Erőforrás | Leírás |
| --- | --- |
| **Feladatgyűjtemény** |A feladatgyűjtemény több feladatot tartalmaz, és tárolja a beállításokat, kvóták és szabályozások hello gyűjteményben feladatok által megosztott. A feladatgyűjteményeket az előfizetés tulajdonosa hozza létre, ahol a feladatokat használat vagy alkalmazáshatárok alapján csoportosítja. Korlátozott tooone régióban. Lehetővé teszi a kvóták hello kényszerítési tooconstrain hello használata az összes feladat a gyűjteményben. hello kvóták MaxJobs és maximális ismétlődésére tartalmazza. |
| **Feladat** |Egyedi, ismétlődő műveletet megadó feladat, egyszerű vagy összetett végrehajtási stratégiákkal. A műveletek HTTP-, tárolásisor-, Service Bus üzenetsor- vagy Service Bus témakörkéréseket tartalmazhatnak. |
| **Feladatelőzmények** |A feladatelőzmény egy feladat végrehajtásának részletes adatait jelenti. Megállapítható belőle a feladat végrehajtásának sikeressége vagy meghiúsulása, illetve bármely részletes válaszadat. |

## <a name="scheduler-entity-management"></a>A Scheduler entitáskezelése
Magas szinten hello Feladatütemező és hello service management API teszi közzé a következő hello erőforrásokon végrehajtott műveletek hello:

| Képesség | Leírás és URI-cím |
| --- | --- |
| **A feladatgyűjtemény kezelése** |PUT BESZERZÉSÉHEZ, és törölje a létrehozása és módosítása feladatgyűjteményei és a benne foglalt hello feladatok támogatása. A feladatgyűjtemény feladatok tárolója, és leképezi tooquotas és közös beállításokat. Példák a később részletezendő kvótákra: feladatok maximális száma és legkisebb ismétlődési időköz. <p>PUT és DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p> |
| **Feladatkezelés** |GET, PUT, POST, PATCH és DELETE kérések támogatása a feladatok létrehozásához és módosításához. Minden feladat kell tartozniuk tooa feladatgyűjteményben már létező, így nem implicit létrehozása. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p> |
| **Feladatelőzmények kezelése** |GET parancs támogatása a 60 napos feladat-végrehajtási előzménytörténet lekéréséhez, ide értve a végrehajtás során eltelt időt és annak eredményeit is. Az állapot szerinti szűrés érdekében támogatja a lekérdezési karakterláncok paramétereit. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p> |

## <a name="job-types"></a>Feladattípusok
Számos feladattípus létezik: HTTP-feladatok (beleértve az SSL-t támogató HTTPS-feladatokat), tárolásisor-feladatok, Service Bus üzenetsor-feladatok és Service Bus témakör-feladatok. A HTTP-feladatok remekül használhatók, ha egy meglévő számítási feladat vagy szolgáltatás egy végponttal rendelkezik. Várólista feladatok toopost üzenetek toostorage tárüzenetsort, is használ, így ezek a feladatok tárüzenetsort használó munkaterhelések ideális. Ehhez hasonlóan a Service Bus feladatok olyan számítási feladatok esetében alkalmazhatók előnyösen, amelyek Service Bus-üzenetsorokat és -témaköröket használnak.

## <a name="hello-job-entity-in-detail"></a>hello "feladat" entitás részletesen
Alapszinten egy ütemezett feladat számos részből áll:

* hello művelet tooperform hello feladat időzítő akkor következik be, amikor  
* (Választható) hello idő toorun hello feladat  
* (Választható) Mikor és milyen gyakran toorepeat hello feladat  
* (Választható) Egy művelet toofire Ha hello elsődleges művelet sikertelen  

Belsőleg egy ütemezett feladatot is rendszer által biztosított adatok, például a következő ütemezett végrehajtási idő hello tartalmazza.

a következő kód hello példát egy átfogó egy ütemezett feladatot. Részletes információkat az ezt követő szakaszokban talál.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often toofire (default too1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default toorecur infinitely)
            "endTime": "2012-11-04",                     // optional (default toorecur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Hello minta ütemezett feladat fent látható, akkor olyan feladatdefinícióban több részből rendelkezik:

* Kezdési idő („startTime”)  
* Művelet („action”), amely hibaműveletet („errorAction”) is tartalmaz
* Ismétlődés („recurrence”)  
* Folyamatállapot („state”)  
* Feladatállapot („status”)  
* Újrapróbálkozási házirend („retryPolicy”)  

Vizsgáljuk meg részletesebben ezeket:

## <a name="starttime"></a>startTime
"startTime" Hello hello kezdési ideje, és lehetővé teszi, hogy a hívó toospecify hello eltolás hello keresztülhaladnak a hálózaton lévő időzóna [ISO-8601 formátumban](http://en.wikipedia.org/wiki/ISO_8601).

## <a name="action-and-erroraction"></a>action és errorAction
hello "action" hello művelet meghívása a mindegyik előfordulásakor, és egy szolgáltatás hívása típusú ismerteti. hello művelete mi végrehajtja a megadott ütemezés hello. A Scheduler támogatja a HTTP-, a tárolásisor-, a Service Bus üzenetsor- és a Service Bus témakörműveleteket.

a fenti példában hello hello művelet egy HTTP-műveletet. Az alábbiakban egy példát láthat egy tárolásisor-műveletre:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Az alábbiakban egy példát láthat egy a Service Bus témakör-műveletre:

  "action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",  
      "namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }

Az alábbiakban egy példát láthat egy Service Bus üzenetsor-műveletre:

  "action": { "serviceBusQueueMessage": { "queueName": "q1",  
      "namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": {  
        "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",  
      "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }

hello "errorAction" hello hibakezelő, ha hello elsődleges művelet nem sikerült meghívni hello művelet. A változó toocall hibakezelés végpont használhatja, vagy a felhasználó értesítést küldeni. Ez használható egy másodlagos végponti hello esetben eléréséhez adott hello elsődleges nem érhető el (például az hello esetben egy olyan vészhelyzet esetén hello végpont helyen), vagy egy hibakezelési végpont értesítésére használható. Hello elsődleges művelet, mint például a hello hiba művelet lehet más műveletek alapján egyszerű vagy összetett logikát. Hogyan toocreate egy SAS-jogkivonatot, tekintse meg a túl toolearn[létrehozhat és használhat egy közös hozzáférésű Jogosultságkód](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>recurrence
Az ismétlődés több részből áll:

* Gyakoriság: percenként, óránként, naponta, hetente, havonta, évente  
* Időköz: Hello Ismétlődési gyakoriság megadott hello időközét  
* Előírt ütemezés: Adja meg a perc, óra, létrehozását, hónap és hello ismételt monthdays  
* Darabszám: Az előfordulások darabszáma  
* A Befejezés időpontja: nincsenek feladatok hajtja végre, miután hello megadott befejezési időpontja  

Ismétlődő feladatról akkor beszélünk, ha ismétlődő objektummal rendelkezik a JSON-definíciójában. Ha száma és az EndTime megadása meg van adva, akkor következik be előbb hello befejezési szabály figyelembe véve.

## <a name="state"></a>state
hello hello feladat állapota négy értékek egyikét: engedélyezve, befejeződött-e vagy hibát jelzett. HELYEZHET el vagy javítás feladatokat, mint tooupdate őket toohello engedélyezett vagy letiltott állapotba. Ha egy feladat már befejeződött vagy meghibásodott, vagyis a végső állapot nem frissíthető, (bár a hello feladat továbbra is TÖRÖLHETŐK). Hello-state tulajdonsága például a következőképpen történik:

        "state": "disabled", // enabled, disabled, completed, or faulted
A befejeződött vagy meghiúsult feladatok 60 nap után törlődnek.

## <a name="status"></a>status
Miután egy ütemezési feladat elindult, információkat visszaadott kapcsolatos hello hello feladat jelenlegi állapota. Ez az objektum nincs hello felhasználó állítható be – hello rendszer szerint van állítva. Azonban szerepel hello feladatobjektum (nem pedig egy külön csatolt erőforrás), hogy egy egyszerűen szerezhet hello feladat állapotának.

A feladatállapot tartalmaz hello idő hello előző végrehajtás (ha van ilyen), hello hello következő ütemezett végrehajtáskor (a folyamatban lévő feladatok), és hello végrehajtási száma hello feladat.

## <a name="retrypolicy"></a>retryPolicy
A Feladatütemező feladat sikertelen lesz, esetén lehetséges toospecify egy újrapróbálkozási házirend toodetermine, hogyan hello műveletet a rendszer ismét megkísérli. Hello határozzák **retryType** objektum – túl van beállítva**nincs** Ha nincs újrapróbálkozási házirend, ahogy fent látható. Állítsa be úgy túl**rögzített** újrapróbálkozási házirendje esetén.

tooset újrapróbálkozási házirendje, két további beállítás adható meg: újrapróbálkozási időközt (**retryInterval**) és az újrapróbálkozások számát hello (**retryCount**).

hello újrapróbálkozási időköz hello megadott **retryInterval** objektum, a hello időköz újrapróbálkozások között. Ennek alapértelmezett értéke 30 másodperc; minimum 15 másodperc, maximum 18 hónap állítható be. Az Ingyenes feladatok gyűjteményében szereplő feladatok minimális konfigurálható értéke 1 óra.  Hello ISO 8601 formátumban van definiálva. Hasonló módon van megadva az újrapróbálkozások számát hello hello értékének hello **retryCount** objektum; hello szám, ahányszor egy újrapróbálkozás. Az alapértelmezett érték 4, és maximális értéke pedig 20\. Mindkét **retryInterval** és **retryCount** megadása nem kötelező. Az alapértelmezett értékükön akkor kapnak, ha **retryType** értéke túl**rögzített** és nincs kifejezetten megadva.

## <a name="see-also"></a>Lásd még:
 [A Scheduler ismertetése](scheduler-intro.md)

 [Az ütemező hello Azure-portálon az első lépéseiben](scheduler-get-started-portal.md)

 [Csomagok és számlázás az Azure Schedulerben](scheduler-plans-billing.md)

 [Hogyan ütemezi a toobuild összetett és speciális ismétlődési és Azure-ütemező](scheduler-advanced-complexity.md)

 [Az Azure Scheduler REST API-jának leírása](https://msdn.microsoft.com/library/mt629143)

 [Az Azure Scheduler PowerShell-parancsmagjainak leírása](scheduler-powershell-reference.md)

 [Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure Scheduler – korlátozások, alapértékek és hibakódok](scheduler-limits-defaults-errors.md)

 [Kimenő hitelesítés az Azure Schedulerben](scheduler-outbound-authentication.md)

