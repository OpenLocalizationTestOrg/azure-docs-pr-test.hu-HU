---
title: "aaaViewing diagnosztikai naplók az Azure Data Lake Analytics |} Microsoft Docs"
description: "Hogyan toosetup és hozzáférési diagnosztikai naplók az Azure Data Lake analytics ismertetése "
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 4cd1eb6f585c1ef96c358340232ef85721a972b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Diagnosztikai naplók elérése az Azure Data Lake Analytics

Diagnosztikai naplózás lehetővé teszi a toocollect adatok a fájlhozzáférés napló ellenőrzését. Ezek a naplók meg információkat, mint:

* Azoknak a felhasználóknak, hello adatok érhetők el.
* Milyen gyakran hello adatokhoz.
* Mennyi adatot hello fiók tárolva van.

## <a name="enable-logging"></a>Naplózás engedélyezése

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Nyissa meg a Data Lake Analytics-fiókja, és válassza ki **diagnosztikai naplók** a hello __figyelő__ szakasz. Válassza ki, __a diagnosztika bekapcsolásához__.

    ![Diagnosztika toocollect naplózás bekapcsolásához és a naplók kérése](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. A __diagnosztikai beállítások__, hello állapot too__On__ beállítva, és válassza ki a naplózási beállításokat.

    ![Diagnosztika toocollect naplózás bekapcsolásához és a naplók kérése](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "diagnosztikai naplók engedélyezése")

   * Állítsa be **állapot** túl**a** tooenable diagnosztikai naplózás.

   * Kiválaszthatja a toostore/folyamat hello adatok három különböző módon.

     * Válassza ki __tooa tárfiók archiválására__ toostore jelentkezik be egy Azure storage-fiókot. Használja ezt a beállítást, ha azt szeretné, hogy tooarchive hello adatokat. Ha ezt a lehetőséget választja, meg kell adnia egy Azure storage-toosave hello naplók a.

     * Válassza ki **tooan Eseményközpont adatfolyam** toostream napló adatok tooan Azure Event Hubs. Használja ezt a beállítást, ha egy alárendelt feldolgozási folyamatot, amely elemzi a bejövő naplók valós időben. Ha ezt a lehetőséget választja, meg kell adnia hello Azure Event Hubs toouse kívánt hello részletei.

     * Válassza ki __tooLog Analytics küldése__ toosend hello adatok toohello Naplóelemzés szolgáltatás. Használja ezt a beállítást, ha szeretné, hogy toouse Naplóelemzési toogather és naplóinak elemzése.
   * Adja meg, hogy tooget naplókat, a kérelmek naplóit vagy mindkettőhöz.  A napló minden API-kérelem rögzíti. Egy naplórekordok az adott API-kérelem által kezdeményezett összes műveletet.

   * A __tooa tárfiók archiválására__, adja meg a napok tooretain hello adatok hello száma.

   * Kattintson a __Save__ (Mentés) gombra.

        > [!NOTE]
        > Ki kell választania, vagy __tooa tárfiók archiválására__, __tooan Eseményközpont adatfolyam__ vagy __tooLog Analytics küldése__ hello való kattintás előtt __mentése__gombra.

Miután engedélyezte a diagnosztikai beállítások, bármikor visszatérhet toohello __diagnosztikai naplók__ panel tooview hello naplókat.

## <a name="view-logs"></a>Naplók megtekintése

### <a name="use-hello-data-lake-analytics-view"></a>Hello Data Lake Analytics nézetben

1. A Data Lake Analytics a fiók panelen a **figyelés**, jelölje be **diagnosztikai naplók** és a jelölje ki egy bejegyzést toodisplay naplókat.

    ![Diagnosztikai naplózás nézet](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "diagnosztikai naplók megtekintése")

2. hello naplók szerint vannak kategóriába által **naplók** és **kérelem naplók**.

    ![naplóbejegyzéseket](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * Kérelem naplók rögzítése a Data Lake Analytics-fiók hello minden API kérelmet.
   * Naplók hasonló toorequest naplókat azonban hello műveletek sokkal részletesebb információkat biztosít. Például egy kérelem naplóban egyetlen feltöltés API hívása több "Append" műveletet a naplóban eredményezhet.

3. Kattintson a hello **letöltése** a napló bejegyzés toodownload, hogy a hivatkozás.

### <a name="use-hello-azure-storage-account-that-contains-log-data"></a>Naplóadatokat tartalmazó hello Azure Storage-fiók használata

1. Nyissa meg a Data Lake Analytics társított naplózás hello Azure Storage-fiók panelen, és kattintson a __Blobok__. Hello **Blob szolgáltatás** panel két tárolók listája.

    ![Diagnosztikai naplózás nézet](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "diagnosztikai naplók megtekintése")

   * hello tároló **insights-logs-naplózási** hello naplók tartalmazza.
   * hello tároló **insights-logs-kérelmek** hello kérelmek naplóit tartalmazza.
2. Ezek a tárolók belül hello naplók tárolt hello struktúra a következő:

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > Hello `##` hello elérési bejegyzés tartalmazhat hello év, hónap, nap és mely hello napló létrehozta óra. A Data Lake Analytics egy fájl minden órában létrehoz, így `m=` mindig szereplő érték `00`.

    Tegyük fel hello teljes elérési útja tooan napló lehet:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Hasonlóképpen teljes elérési útja tooa napló hello lehet:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Napló struktúra

hello naplózási és kérelem feldolgozásra strukturált JSON formátumban.

### <a name="request-logs"></a>Naplók kérése

Íme egy minta bejegyzés hello JSON-formátumú kérelem naplóban. Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** , amely tartalmazza a napló objektumokból álló tömb.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Kérelem séma

| Név | Típus | Leírás |
| --- | --- | --- |
| time |Karakterlánc |hello időbélyegzőnek (UTC) hello napló |
| resourceId |Karakterlánc |Helyezze művelet hello erőforrás hello azonosítója |
| category |Karakterlánc |hello napló kategóriát. Például **kérelmek**. |
| operationName |Karakterlánc |Bejelentkezve hello művelet neve. Például GetAggregatedJobHistory. |
| resultType |Karakterlánc |hello művelet, például 200 hello állapotát. |
| callerIpAddress |Karakterlánc |hello kérés hello ügyfél hello IP-címe |
| correlationId |Karakterlánc |hello hello napló azonosítója. Az érték lehet használt toogroup kapcsolódó naplóbejegyzések készlete. |
| identity |Objektum |hello napló okozó hello identitás |
| properties |JSON |Lásd az hello következő szakaszát (kérelem tulajdonságok séma) |

#### <a name="request-log-properties-schema"></a>Kérelem tulajdonságok séma

| Név | Típus | Leírás |
| --- | --- | --- |
| HttpMethod |Karakterlánc |hello HTTP-metódus használt hello a művelethez. Például beolvasása. |
| Elérési út |Karakterlánc |hello elérési hello művelet végrehajtásának ideje |
| RequestContentLength |int |hello tartalom hossza hello HTTP-kérelem |
| clientRequestId |Karakterlánc |hello azonosítója, amely egyedileg azonosítja az ehhez a kérelemhez |
| Kezdő időpont |Karakterlánc |mely hello kiszolgálótól kapott hello kérésére hello idő |
| Befejezés időpontja |Karakterlánc |mely hello a kiszolgáló által küldött választ hello idő |

### <a name="audit-logs"></a>Naplók

Íme egy minta bejegyzés hello JSON-formátumú naplóban. Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** , amely tartalmazza a napló objektumokból álló tömb.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Naplózási séma

| Név | Típus | Leírás |
| --- | --- | --- |
| time |Karakterlánc |hello időbélyegzőnek (UTC) hello napló |
| resourceId |Karakterlánc |Helyezze művelet hello erőforrás hello azonosítója |
| category |Karakterlánc |hello napló kategóriát. Például **naplózási**. |
| operationName |Karakterlánc |Bejelentkezve hello művelet neve. Például JobSubmitted. |
| resultType |Karakterlánc |A részállapot hello feladat állapot (operationName). |
| resultSignature |Karakterlánc |További részletek a hello feladat állapota (operationName). |
| identity |Karakterlánc |a kért műveletet hello hello felhasználó. Például: susan@contoso.com. |
| properties |JSON |Lásd az hello következő szakaszát (naplózási tulajdonságai séma) |

> [!NOTE]
> **resultType** és **resultSignature** adatokat hello művelet eredményét, és csak tartalmaz értéket, ha egy művelet befejeződött. Például csak tartalmaznak egy érték amikor **operationName** szereplő érték **JobStarted** vagy **JobEnded**.
>
>

#### <a name="audit-log-properties-schema"></a>Naplózási tulajdonságai séma

| Név | Típus | Leírás |
| --- | --- | --- |
| JobId |Karakterlánc |hello azonosító hozzárendelt toohello feladat |
| Feladat neve |Karakterlánc |a megadott hello feladat hello neve |
| JobRunTime |Karakterlánc |hello futásidejű használt tooprocess hello feladat |
| SubmitTime |Karakterlánc |hello ideje (UTC) az adott hello feladat el lett küldve |
| Kezdő időpont |Karakterlánc |hello idő hello feladat indulása után küldése (az UTC) |
| Befejezés időpontja |Karakterlánc |hello idő hello feladat befejeződött |
| Párhuzamos végrehajtás |Karakterlánc |Ez a feladat kért végzett leadásakor Data Lake Analytics egységek hello száma |

> [!NOTE]
> **SubmitTime**, **StartTime**, **EndTime**, és **párhuzamossági** művelet kapcsolatban nyújtanak információkat. Ezek a bejegyzések csak tartalmaz értéket ha művelet indítása vagy befejeződött. Például **SubmitTime** csak értéke után **operationName** hello értéke **JobSubmitted**.

## <a name="process-hello-log-data"></a>Folyamatok hello naplóadatok

Az Azure Data Lake Analytics biztosít egy minta tooprocess és elemezheti a hello naplóadatokat. Hello minta a található [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Következő lépések
* [Az Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
