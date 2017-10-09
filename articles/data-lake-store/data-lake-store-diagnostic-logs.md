---
title: "Azure Data Lake Store aaaViewing diagnosztikai naplókat |} Microsoft Docs"
description: "Megértéséhez hogyan toosetup és hozzáférés az Azure Data Lake Store diagnosztikai naplók "
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 11fbf7f517f97abdcaf809c1ebeeb51424ab2c1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Diagnosztikai naplók az Azure Data Lake Store elérése
További információk a hogyan tooenable diagnosztikai naplózás a Data Lake Store-fiókot, és hogy miként naplózza az tooview hello gyűjtése a fiókját.

A szervezetek is az Azure Data Lake Store fiók toocollect adatok a fájlhozzáférés napló ellenőrzését, amely bemutatja, például a lista kapcsolódó felhasználók hello adatok, milyen gyakran hello hozzá az adatokhoz, mennyi adatot tárolja hello diagnosztikai naplózás engedélyezése fiók, stb.

## <a name="prerequisites"></a>Előfeltételek
* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Store-fiók**. Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>A Data Lake Store-fiók diagnosztikai naplózás engedélyezése
1. Jelentkezzen be az új toohello [Azure Portal](https://portal.azure.com).
2. Nyissa meg a Data Lake Store-fiókot, és a Data Lake Store-fiók panelen kattintson **beállítások**, és kattintson a **diagnosztikai naplók**.
3. A hello **diagnosztikai naplók** panelen kattintson a **a diagnosztika bekapcsolásához**.

    ![Diagnosztikai naplózás engedélyezése](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "diagnosztikai naplók engedélyezése")

3. A hello **diagnosztikai** panelen ellenőrizze a következő módosításokat tooconfigure diagnosztikai naplózás hello.
   
    ![Diagnosztikai naplózás engedélyezése](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "diagnosztikai naplók engedélyezése")
   
   * Állítsa be **állapot** túl**a** tooenable diagnosztikai naplózás.
   * Kiválaszthatja a toostore/folyamat hello adatokat különböző módon.
     
        * A beállításnak a hello túl**tooa tárfiók archiválására** toostore naplózza tooan Azure Storage-fiók. Ezt a beállítást használja, ha azt szeretné, hogy tooarchive hello adatot kötegelt feldolgozásra egy későbbi időpontban. Ha ezt a beállítást meg kell adnia egy Azure Storage-fiók toosave hello naplókat.
        
        * A beállításnak a hello túl**adatfolyam tooan eseményközpont** toostream napló adatok tooan Azure Event Hubs. Valószínűleg fogja használni ezt a beállítást, ha egy alárendelt feldolgozási tooanalyze bejövő naplók valós időben a következő feldolgozási sorban. Ha ezt a lehetőséget választja, meg kell adnia hello Azure Event Hubs toouse kívánt hello részletei.

        * A beállításnak a hello túl**tooLog Analytics küldése** toouse hello Azure Naplóelemzés tooanalyze generált hello Szolgáltatásnapló-adatait. Ha ezt a lehetőséget választja, meg kell adnia, hello a kapcsolatos részleteket hello Operations Management Suite-munkaterülettel, hogy használjon hello hajtaná végre a webhelynapló elemzése.
     
   * Adja meg, hogy tooget naplókat, a kérelmek naplóit vagy mindkettőhöz.
   * Adja meg, amelynek meg kell őrizni hello adatok napok hello számát. Megőrzési csak akkor alkalmazható, ha az Azure storage-fiók tooarchive naplóadatokat használ.
   * Kattintson a **Save** (Mentés) gombra.

Miután engedélyezte a diagnosztikai beállítások, figyelheti az hello bejelentkezik hello **diagnosztikai naplók** fülre.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>A Data Lake Store-fiók diagnosztikai naplók megtekintése
Két módon tooview hello adatainak naplózása a Data Lake Store-fiók.

* A Data Lake Store-fiók hello beállítások megtekintése
* A hello hello adatokat tároló Azure Storage-fiókban

### <a name="using-hello-data-lake-store-settings-view"></a>Data Lake Store beállítások megtekintése hello használata
1. A Data Lake Store-fiókból **beállítások** panelen kattintson a **diagnosztikai naplók**.
   
    ![Diagnosztikai naplózás nézet](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "diagnosztikai naplók megtekintése") 
2. A hello **diagnosztikai naplók** panelen megtekintheti az kategorizálta hello naplók **naplók** és **kérelem naplók**.
   
   * Kérelem naplók rögzítése a Data Lake Store-fiók hello minden API kérelmet.
   * Naplók hasonló toorequest naplókat azonban sokkal részletesebben bontást hello műveletek végrehajtása hello Data Lake Store-fiók. Például egy egyetlen feltöltés API-hívás a kérelem naplókban hello naplók több "Append" műveletei eredményezheti.
3. Kattintson a hello **letöltése** egyes hivatkozás jelentkezzen bejegyzés toodownload hello naplókat.

### <a name="from-hello-azure-storage-account-that-contains-log-data"></a>Az Azure Storage-fiók hello naplóadatokat tartalmazó
1. Nyissa meg a Data Lake Store társított naplózás hello Azure Storage-fiók panelen, és kattintson a Blobok. Hello **Blob szolgáltatás** panel két tárolók listája.
   
    ![Diagnosztikai naplózás nézet](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "diagnosztikai naplók megtekintése")
   
   * hello tároló **insights-logs-naplózási** hello naplók tartalmazza.
   * hello tároló **insights-logs-kérelmek** hello kérelmek naplóit tartalmazza.
2. Ezek a tárolók belül hello struktúra a következő tárolt hello naplókat.
   
    ![Diagnosztikai naplózás nézet](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "diagnosztikai naplók megtekintése")
   
    Tegyük fel a teljes elérési tooan napló hello lehet`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    Similary, teljes elérési útja tooa napló hello lehet`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-hello-structure-of-hello-log-data"></a>Hello naplóadatokat hello szerkezete ismertetése
hello naplózási és kérelem feldolgozásra JSON formátumban. Ez a szakasz azt hello szerkezete JSON kérelem tekintse meg és a naplók.

### <a name="request-logs"></a>Naplók kérése
Íme egy minta bejegyzés hello JSON-formátumú kérelem naplóban. Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** , amely tartalmazza a napló objektumokból álló tömb.

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Kérelem séma
| Név | Típus | Leírás |
| --- | --- | --- |
| time |Karakterlánc |hello időbélyegzőnek (UTC) hello napló |
| resourceId |Karakterlánc |Helyezze el, hogy a művelet hello erőforrás hello azonosítója |
| category |Karakterlánc |hello napló kategóriát. Például **kérelmek**. |
| operationName |Karakterlánc |Bejelentkezve hello művelet neve. Például getfilestatus. |
| resultType |Karakterlánc |hello művelet, például 200 hello állapotát. |
| callerIpAddress |Karakterlánc |hello kérés hello ügyfél hello IP-címe |
| correlationId |Karakterlánc |hello azonosítója, amelyet használhat toogroup egymáshoz kapcsolódó naplóbejegyzések készlete hello napló |
| identity |Objektum |hello napló okozó hello identitás |
| properties |JSON |További információ alább olvasható |

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
Íme egy minta bejegyzés hello JSON-formátumú naplóban. Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** napló objektumok tömbjét tartalmazza, amelyek

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Naplózási séma
| Név | Típus | Leírás |
| --- | --- | --- |
| time |Karakterlánc |hello időbélyegzőnek (UTC) hello napló |
| resourceId |Karakterlánc |Helyezze el, hogy a művelet hello erőforrás hello azonosítója |
| category |Karakterlánc |hello napló kategóriát. Például **naplózási**. |
| operationName |Karakterlánc |Bejelentkezve hello művelet neve. Például getfilestatus. |
| resultType |Karakterlánc |hello művelet, például 200 hello állapotát. |
| correlationId |Karakterlánc |hello azonosítója, amelyet használhat toogroup egymáshoz kapcsolódó naplóbejegyzések készlete hello napló |
| identity |Objektum |hello napló okozó hello identitás |
| properties |JSON |További információ alább olvasható |

#### <a name="audit-log-properties-schema"></a>Naplózási tulajdonságai séma
| Név | Típus | Leírás |
| --- | --- | --- |
| StreamName |Karakterlánc |hello elérési hello művelet végrehajtásának ideje |

## <a name="samples-tooprocess-hello-log-data"></a>Minták tooprocess hello naplóadatok
Azure Data Lake Store biztosít egy minta tooprocess és elemezheti a hello naplóadatokat. Hello minta a található [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 

## <a name="see-also"></a>Lásd még:
* [Az Azure Data Lake Store áttekintése](data-lake-store-overview.md)
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)

