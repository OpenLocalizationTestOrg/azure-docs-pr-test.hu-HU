---
title: "adatcsatorna HL7 FHIR erőforrások - Azure Cosmos DB aaaChange |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset be módosítani értesítések HL7 FHIR beteg egészségügyi rekordok Azure Logic Apps, az Azure Cosmos DB és a Service Bus használatával."
keywords: HL7 fhir
services: cosmos-db
author: hedidin
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 0d25c11f-9197-419a-aa19-4614c6ab2d06
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: b-hoedid
ms.openlocfilehash: d2809bf5c6d8c193c49438d20684c56caea646bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a>A Logic Apps és az Azure Cosmos DB használatával HL7 FHIR egészségügyi rekord módosítása betegek értesítése

Az Azure MVP Howard Edidin nemrég egészségügyi szervezetek, amelyek tooadd új funkciók tootheir beteg portál kívánta felvenni a kapcsolatot. Toosend értesítések toopatients azok szükség, amikor az állapot rekord frissítve lett, és akkor szükséges, hogy betegek toobe képes toosubscribe toothese frissítéseket. 

Ez a cikk végigvezeti a egészségügyi szervezet Azure Cosmos DB, a Logic Apps és a Service Bus használatával létrehozott hello módosítás adatcsatorna értesítési megoldást. 

## <a name="project-requirements"></a>Projekt követelmények
- Szolgáltatók küldése HL7 konszolidált klinikai dokumentum architektúra (C-CDA) dokumentumok XML formátumban. C-CDA dokumentumok tartalmazzák a klinikai dokumentum, beleértve a klinikai dokumentumok például családba tartozó alábbi előzményeinek és azon rögzíti, valamint a felügyeleti, a munkafolyamat és a pénzügyi dokumentumok szinte bármilyen típusú. 
- C-CDA dokumentumok alakítja a rendszer túl[HL7 FHIR erőforrások](http://hl7.org/fhir/2017Jan/resourcelist.html) JSON formátumban.
- Módosított FHIR erőforrás dokumentumokat küldött e-mailek JSON formátumban.

## <a name="solution-workflow"></a>Megoldás munkafolyamat 

Magas szinten hello projekt szükséges hello munkafolyamat a lépéseket követve: 
1. Az átalakítás C-CDA dokumentumok tooFHIR erőforrásokat.
2. Ciklikus lekérdezés a módosított FHIR erőforrások ismétlődő eseményindító végrehajtása. 
2. Egy egyéni alkalmazást, FhirNotificationApi, tooconnect tooAzure Cosmos DB és lekérdezés kérjen új vagy módosított dokumentumokat.
3. Mentse a hello válasz tootoohello Service Bus-üzenetsorba.
4. A lekérdezési az új üzenetek hello Service Bus-üzenetsorba.
5. E-mail értesítések toopatients küldése.

## <a name="solution-architecture"></a>Megoldás architektúrája
Ez a megoldás három Logic Apps toomeet hello követelmények és a teljes hello megoldás munkafolyamat felett van szükség. hello három a logic apps a következők:
1. **HL7-FHIR-leképezés app**: hello HL7 C-CDA dokumentum kap, toohello FHIR erőforrás átalakítja azt, majd menti azt tooAzure Cosmos DB.
2. **EHR app**: hello Azure Cosmos DB FHIR tárház lekérdezése, és menti a hello válasz tooa Service Bus-üzenetsorba. A logikai alkalmazást használ egy [API-alkalmazás](#api-app) tooretrieve új és módosított dokumentumokat.
3. **Folyamat értesítési app**: hello FHIR erőforrás dokumentumok és e-mailben értesítést küld a hello törzsében.

![hello három Logic Apps a HL7 FHIR egészségügyi megoldásban használt](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-hello-solution"></a>Hello megoldásban használt Azure-szolgáltatások

#### <a name="azure-cosmos-db-documentdb-api"></a>Az Azure Cosmos DB DocumentDB API
Azure Cosmos-adatbázis nem hello FHIR erőforrások hello tárháza, ahogy az ábra a következő hello.

![hello Azure Cosmos DB művelet végrehajtására használt fiók az egészségügyi HL7 FHIR oktatóanyag](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a>Logic Apps
A Logic Apps hello munkafolyamat kezeli. hello alábbi képernyőképek megjelenítése hello a Logic apps létre ehhez a megoldáshoz. 


1. **HL7-FHIR-leképezés app**: hello HL7 C-CDA dokumentum kap, és irányítópulttá, tooan FHIR erőforrás hello vállalati integrációs csomag a Logic Apps segítségével. hello vállalati integrációs csomag kezeli hello C-CDA tooFHIR erőforrások hello társítást.

    ![hello logikai alkalmazás használt tooreceive HL7 FHIR egészségügyi rekordok](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. **EHR app**: hello Azure Cosmos DB FHIR tárház lekérdezni, és mentse hello válasz tooa Service Bus várólistára. hello hello GetNewOrModifiedFHIRDocuments alkalmazás kódja nem éri el.

    ![hello használt logikai alkalmazás tooquery Azure Cosmos DB](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. **Folyamat értesítési app**: hello FHIR erőforrás dokumentumok és e-mailben értesítést küldeni a hello törzsében.

    ![hello hello HL7 FHIR erőforrás beteg e-maileket küldő hello törzsében logikai alkalmazás](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a>Service Bus
a következő ábra azt mutatja be hello betegek várólista hello. hello e-mail tárgya hello címke tulajdonság érték használható.

![hello HL7 FHIR oktatóanyagban használt Service Bus-üzenetsorba](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a>API-alkalmazás
Az API-alkalmazások tooAzure Cosmos DB és erőforrástípusok szerint FHIR dokumentumok új vagy módosított lekérdezések csatlakozik. Ez az alkalmazás rendelkezik egy tartományvezérlő, **FhirNotificationApi** egy művelettel **GetNewOrModifiedFhirDocuments**, lásd: [API-alkalmazás forrását](#api-app-source).

Hello használjuk [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) hello Azure Cosmos DB DocumentDB .NET API osztályt. További információkért lásd: hello [módosítás hírcsatorna cikk](change-feed.md). 

##### <a name="getnewormodifiedfhirdocuments-operation"></a>GetNewOrModifiedFhirDocuments művelet

**Bemenetek**
- DatabaseId
- CollectionId
- HL7 FHIR erőforrástípus neve
- Logikai érték: Indítsa el az elejétől
- Int: Visszaadott dokumentumok száma

**Kimenetek**
- Sikerült: Állapotkód: 200, válasz: dokumentumok (JSON-tömb) listája
- Hiba: Állapotkód: 404-es, válasz: "nem található a"*erőforrás neve "* erőforrástípus"

<a id="api-app-source"></a>

**Forrás hello API-alkalmazás**

```C#

    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Web.Http;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Swashbuckle.Swagger.Annotations;
    using TRex.Metadata;
    
    namespace FhirNotificationApi.Controllers
    {
        /// <summary>
        ///     FHIR Resource Type Controller
        /// </summary>
        /// <seealso cref="System.Web.Http.ApiController" />
        public class FhirResourceTypeController : ApiController
        {
            /// <summary>
            ///     Gets hello new or modified FHIR documents from Last Run Date 
            ///     or create date of hello collection
            /// </summary>
            /// <param name="databaseId"></param>
            /// <param name="collectionId"></param>
            /// <param name="resourceType"></param>
            /// <param name="startfromBeginning"></param>
            /// <param name="maximumItemCount">-1 returns all (default)</param>
            /// <returns></returns>
            [Metadata("Get New or Modified FHIR Documents",
                "Query for new or modifed FHIR Documents By Resource Type " +
                "from Last Run Date or Begining of Collection creation"
            )]
            [SwaggerResponse(HttpStatusCode.OK, type: typeof(Task<dynamic>))]
            [SwaggerResponse(HttpStatusCode.NotFound, "No New or Modifed Documents found")]
            [SwaggerOperation("GetNewOrModifiedFHIRDocuments")]
            public async Task<dynamic> GetNewOrModifiedFhirDocuments(
                [Metadata("Database Id", "Database Id")] string databaseId,
                [Metadata("Collection Id", "Collection Id")] string collectionId,
                [Metadata("Resource Type", "FHIR resource type name")] string resourceType,
                [Metadata("Start from Beginning ", "Change Feed Option")] bool startfromBeginning,
                [Metadata("Maximum Item Count", "Number of documents returned. '-1 returns all' (default)")] int maximumItemCount = -1
            )
            {
                var collectionLink = UriFactory.CreateDocumentCollectionUri(databaseId, collectionId);
    
                var context = new DocumentDbContext();  
    
                var docs = new List<dynamic>();
    
                var partitionKeyRanges = new List<PartitionKeyRange>();
                FeedResponse<PartitionKeyRange> pkRangesResponse;
    
                do
                {
                    pkRangesResponse = await context.Client.ReadPartitionKeyRangeFeedAsync(collectionLink);
                    partitionKeyRanges.AddRange(pkRangesResponse);
                } while (pkRangesResponse.ResponseContinuation != null);
    
                foreach (var pkRange in partitionKeyRanges)
                {
                    var changeFeedOptions = new ChangeFeedOptions
                    {
                        StartFromBeginning = startfromBeginning,
                        RequestContinuation = null,
                        MaxItemCount = maximumItemCount,
                        PartitionKeyRangeId = pkRange.Id
                    };
    
                    using (var query = context.Client.CreateDocumentChangeFeedQuery(collectionLink, changeFeedOptions))
                    {
                        do
                        {
                            if (query != null)
                            {
                                var results = await query.ExecuteNextAsync<dynamic>().ConfigureAwait(false);
                                if (results.Count > 0)
                                    docs.AddRange(results.Where(doc => doc.resourceType == resourceType));
                            }
                            else
                            {
                                throw new HttpResponseException(new HttpResponseMessage(HttpStatusCode.NotFound));
                            }
                        } while (query.HasMoreResults);
                    }
                }
                if (docs.Count > 0)
                    return docs;
                var msg = new StringContent("No documents found for " + resourceType + " Resource");
                var response = new HttpResponseMessage
                {
                    StatusCode = HttpStatusCode.NotFound,
                    Content = msg
                };
                return response;
            }
        }
    }
    
```

### <a name="testing-hello-fhirnotificationapi"></a>Hello FhirNotificationApi tesztelése 

hello következő kép bemutatja, hogyan swagger lett használt tootootest hello [FhirNotificationApi](#api-app-source).

![a Swagger-fájl hello használt tootest hello API-alkalmazás](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a>Azure-portál irányítópultjának

a következő kép hello mutatja hello ehhez a megoldáshoz hello Azure-portálon futó Azure-szolgáltatásokhoz.

![hello megjelenítő HL7 FHIR oktatóanyagban használt összes hello szolgáltatást Azure-portálon](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a>Összefoglalás

- Megtanulta, hogy Azure Cosmos DB natív támogatja a következő kapcsolattípust értesítéseket az új vagy módosított dokumentumokat, és milyen egyszerűen toouse rendelkezik-e. 
- A Logic Apps használatával programozás nélkül munkafolyamatokat hozhat létre.
- Azure Service Bus-üzenetsorok toohandle hello eloszlás használatával hello HL7 FHIR dokumentumokhoz.

## <a name="next-steps"></a>Következő lépések
Azure Cosmos DB kapcsolatos további információkért lásd: hello [Azure Cosmos DB kezdőlap](https://azure.microsoft.com/services/cosmos-db/). A Logic Apps kapcsolatos további információk megadására, lásd: [Logic Apps](https://azure.microsoft.com/services/logic-apps/).


