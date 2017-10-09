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
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a><span data-ttu-id="4eee3-104">A Logic Apps és az Azure Cosmos DB használatával HL7 FHIR egészségügyi rekord módosítása betegek értesítése</span><span class="sxs-lookup"><span data-stu-id="4eee3-104">Notifying patients of HL7 FHIR health care record changes using Logic Apps and Azure Cosmos DB</span></span>

<span data-ttu-id="4eee3-105">Az Azure MVP Howard Edidin nemrég egészségügyi szervezetek, amelyek tooadd új funkciók tootheir beteg portál kívánta felvenni a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="4eee3-105">Azure MVP Howard Edidin was recently contacted by a healthcare organization that wanted tooadd new functionality tootheir patient portal.</span></span> <span data-ttu-id="4eee3-106">Toosend értesítések toopatients azok szükség, amikor az állapot rekord frissítve lett, és akkor szükséges, hogy betegek toobe képes toosubscribe toothese frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="4eee3-106">They needed toosend notifications toopatients when their health record was updated, and they needed patients toobe able toosubscribe toothese updates.</span></span> 

<span data-ttu-id="4eee3-107">Ez a cikk végigvezeti a egészségügyi szervezet Azure Cosmos DB, a Logic Apps és a Service Bus használatával létrehozott hello módosítás adatcsatorna értesítési megoldást.</span><span class="sxs-lookup"><span data-stu-id="4eee3-107">This article walks through hello change feed notification solution created for this healthcare organization using Azure Cosmos DB, Logic Apps, and Service Bus.</span></span> 

## <a name="project-requirements"></a><span data-ttu-id="4eee3-108">Projekt követelmények</span><span class="sxs-lookup"><span data-stu-id="4eee3-108">Project requirements</span></span>
- <span data-ttu-id="4eee3-109">Szolgáltatók küldése HL7 konszolidált klinikai dokumentum architektúra (C-CDA) dokumentumok XML formátumban.</span><span class="sxs-lookup"><span data-stu-id="4eee3-109">Providers send HL7 Consolidated-Clinical Document Architecture (C-CDA) documents in XML format.</span></span> <span data-ttu-id="4eee3-110">C-CDA dokumentumok tartalmazzák a klinikai dokumentum, beleértve a klinikai dokumentumok például családba tartozó alábbi előzményeinek és azon rögzíti, valamint a felügyeleti, a munkafolyamat és a pénzügyi dokumentumok szinte bármilyen típusú.</span><span class="sxs-lookup"><span data-stu-id="4eee3-110">C-CDA documents encompass just about every type of clinical document, including clinical documents such as family histories and immunization records, as well as administrative, workflow, and financial documents.</span></span> 
- <span data-ttu-id="4eee3-111">C-CDA dokumentumok alakítja a rendszer túl[HL7 FHIR erőforrások](http://hl7.org/fhir/2017Jan/resourcelist.html) JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="4eee3-111">C-CDA documents are converted too[HL7 FHIR Resources](http://hl7.org/fhir/2017Jan/resourcelist.html) in JSON format.</span></span>
- <span data-ttu-id="4eee3-112">Módosított FHIR erőforrás dokumentumokat küldött e-mailek JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="4eee3-112">Modified FHIR resource documents are sent by email in JSON format.</span></span>

## <a name="solution-workflow"></a><span data-ttu-id="4eee3-113">Megoldás munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="4eee3-113">Solution workflow</span></span> 

<span data-ttu-id="4eee3-114">Magas szinten hello projekt szükséges hello munkafolyamat a lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="4eee3-114">At a high level, hello project required hello following workflow steps:</span></span> 
1. <span data-ttu-id="4eee3-115">Az átalakítás C-CDA dokumentumok tooFHIR erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4eee3-115">Convert C-CDA documents tooFHIR resources.</span></span>
2. <span data-ttu-id="4eee3-116">Ciklikus lekérdezés a módosított FHIR erőforrások ismétlődő eseményindító végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="4eee3-116">Perform recurring trigger polling for modified FHIR resources.</span></span> 
2. <span data-ttu-id="4eee3-117">Egy egyéni alkalmazást, FhirNotificationApi, tooconnect tooAzure Cosmos DB és lekérdezés kérjen új vagy módosított dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="4eee3-117">Call a custom app, FhirNotificationApi, tooconnect tooAzure Cosmos DB and query for new or modified documents.</span></span>
3. <span data-ttu-id="4eee3-118">Mentse a hello válasz tootoohello Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="4eee3-118">Save hello response tootoohello Service Bus queue.</span></span>
4. <span data-ttu-id="4eee3-119">A lekérdezési az új üzenetek hello Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="4eee3-119">Poll for new messages in hello Service Bus queue.</span></span>
5. <span data-ttu-id="4eee3-120">E-mail értesítések toopatients küldése.</span><span class="sxs-lookup"><span data-stu-id="4eee3-120">Send email notifications toopatients.</span></span>

## <a name="solution-architecture"></a><span data-ttu-id="4eee3-121">Megoldás architektúrája</span><span class="sxs-lookup"><span data-stu-id="4eee3-121">Solution architecture</span></span>
<span data-ttu-id="4eee3-122">Ez a megoldás három Logic Apps toomeet hello követelmények és a teljes hello megoldás munkafolyamat felett van szükség.</span><span class="sxs-lookup"><span data-stu-id="4eee3-122">This solution requires three Logic Apps toomeet hello above requirements and complete hello solution workflow.</span></span> <span data-ttu-id="4eee3-123">hello három a logic apps a következők:</span><span class="sxs-lookup"><span data-stu-id="4eee3-123">hello three logic apps are:</span></span>
1. <span data-ttu-id="4eee3-124">**HL7-FHIR-leképezés app**: hello HL7 C-CDA dokumentum kap, toohello FHIR erőforrás átalakítja azt, majd menti azt tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4eee3-124">**HL7-FHIR-Mapping app**: Receives hello HL7 C-CDA document, transforms it toohello FHIR Resource, then saves it tooAzure Cosmos DB.</span></span>
2. <span data-ttu-id="4eee3-125">**EHR app**: hello Azure Cosmos DB FHIR tárház lekérdezése, és menti a hello válasz tooa Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="4eee3-125">**EHR app**: Queries hello Azure Cosmos DB FHIR repository and saves hello response tooa Service Bus queue.</span></span> <span data-ttu-id="4eee3-126">A logikai alkalmazást használ egy [API-alkalmazás](#api-app) tooretrieve új és módosított dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="4eee3-126">This logic app uses an [API app](#api-app) tooretrieve new and changed documents.</span></span>
3. <span data-ttu-id="4eee3-127">**Folyamat értesítési app**: hello FHIR erőforrás dokumentumok és e-mailben értesítést küld a hello törzsében.</span><span class="sxs-lookup"><span data-stu-id="4eee3-127">**Process notification app**: Sends an email notification with hello FHIR resource documents in hello body.</span></span>

![hello három Logic Apps a HL7 FHIR egészségügyi megoldásban használt](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-hello-solution"></a><span data-ttu-id="4eee3-129">Hello megoldásban használt Azure-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="4eee3-129">Azure services used in hello solution</span></span>

#### <a name="azure-cosmos-db-documentdb-api"></a><span data-ttu-id="4eee3-130">Az Azure Cosmos DB DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="4eee3-130">Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="4eee3-131">Azure Cosmos-adatbázis nem hello FHIR erőforrások hello tárháza, ahogy az ábra a következő hello.</span><span class="sxs-lookup"><span data-stu-id="4eee3-131">Azure Cosmos DB is hello repository for hello FHIR resources as shown in hello following figure.</span></span>

![hello Azure Cosmos DB művelet végrehajtására használt fiók az egészségügyi HL7 FHIR oktatóanyag](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a><span data-ttu-id="4eee3-133">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="4eee3-133">Logic Apps</span></span>
<span data-ttu-id="4eee3-134">A Logic Apps hello munkafolyamat kezeli.</span><span class="sxs-lookup"><span data-stu-id="4eee3-134">Logic Apps handle hello workflow process.</span></span> <span data-ttu-id="4eee3-135">hello alábbi képernyőképek megjelenítése hello a Logic apps létre ehhez a megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="4eee3-135">hello following screenshots show hello Logic apps created for this solution.</span></span> 


1. <span data-ttu-id="4eee3-136">**HL7-FHIR-leképezés app**: hello HL7 C-CDA dokumentum kap, és irányítópulttá, tooan FHIR erőforrás hello vállalati integrációs csomag a Logic Apps segítségével.</span><span class="sxs-lookup"><span data-stu-id="4eee3-136">**HL7-FHIR-Mapping app**: Receive hello HL7 C-CDA document and transform it tooan FHIR resource using hello Enterprise Integration Pack for Logic Apps.</span></span> <span data-ttu-id="4eee3-137">hello vállalati integrációs csomag kezeli hello C-CDA tooFHIR erőforrások hello társítást.</span><span class="sxs-lookup"><span data-stu-id="4eee3-137">hello Enterprise Integration Pack handles hello mapping from hello C-CDA tooFHIR resources.</span></span>

    ![hello logikai alkalmazás használt tooreceive HL7 FHIR egészségügyi rekordok](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. <span data-ttu-id="4eee3-139">**EHR app**: hello Azure Cosmos DB FHIR tárház lekérdezni, és mentse hello válasz tooa Service Bus várólistára.</span><span class="sxs-lookup"><span data-stu-id="4eee3-139">**EHR app**: Query hello Azure Cosmos DB FHIR repository and save hello response tooa Service Bus queue.</span></span> <span data-ttu-id="4eee3-140">hello hello GetNewOrModifiedFHIRDocuments alkalmazás kódja nem éri el.</span><span class="sxs-lookup"><span data-stu-id="4eee3-140">hello code for hello GetNewOrModifiedFHIRDocuments app is below.</span></span>

    ![hello használt logikai alkalmazás tooquery Azure Cosmos DB](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. <span data-ttu-id="4eee3-142">**Folyamat értesítési app**: hello FHIR erőforrás dokumentumok és e-mailben értesítést küldeni a hello törzsében.</span><span class="sxs-lookup"><span data-stu-id="4eee3-142">**Process notification app**: Send an email notification with hello FHIR resource documents in hello body.</span></span>

    ![hello hello HL7 FHIR erőforrás beteg e-maileket küldő hello törzsében logikai alkalmazás](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a><span data-ttu-id="4eee3-144">Service Bus</span><span class="sxs-lookup"><span data-stu-id="4eee3-144">Service Bus</span></span>
<span data-ttu-id="4eee3-145">a következő ábra azt mutatja be hello betegek várólista hello.</span><span class="sxs-lookup"><span data-stu-id="4eee3-145">hello following figure shows hello patients queue.</span></span> <span data-ttu-id="4eee3-146">hello e-mail tárgya hello címke tulajdonság érték használható.</span><span class="sxs-lookup"><span data-stu-id="4eee3-146">hello Tag property value is used for hello email subject.</span></span>

![hello HL7 FHIR oktatóanyagban használt Service Bus-üzenetsorba](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a><span data-ttu-id="4eee3-148">API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="4eee3-148">API app</span></span>
<span data-ttu-id="4eee3-149">Az API-alkalmazások tooAzure Cosmos DB és erőforrástípusok szerint FHIR dokumentumok új vagy módosított lekérdezések csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="4eee3-149">An API app connects tooAzure Cosmos DB and queries for new or modified FHIR documents By resource type.</span></span> <span data-ttu-id="4eee3-150">Ez az alkalmazás rendelkezik egy tartományvezérlő, **FhirNotificationApi** egy művelettel **GetNewOrModifiedFhirDocuments**, lásd: [API-alkalmazás forrását](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="4eee3-150">This app has one controller, **FhirNotificationApi** with a one operation **GetNewOrModifiedFhirDocuments**, see [source for API app](#api-app-source).</span></span>

<span data-ttu-id="4eee3-151">Hello használjuk [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) hello Azure Cosmos DB DocumentDB .NET API osztályt.</span><span class="sxs-lookup"><span data-stu-id="4eee3-151">We are using hello [`CreateDocumentChangeFeedQuery`](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) class from hello Azure Cosmos DB DocumentDB .NET API.</span></span> <span data-ttu-id="4eee3-152">További információkért lásd: hello [módosítás hírcsatorna cikk](change-feed.md).</span><span class="sxs-lookup"><span data-stu-id="4eee3-152">For more information, see hello [change feed article](change-feed.md).</span></span> 

##### <a name="getnewormodifiedfhirdocuments-operation"></a><span data-ttu-id="4eee3-153">GetNewOrModifiedFhirDocuments művelet</span><span class="sxs-lookup"><span data-stu-id="4eee3-153">GetNewOrModifiedFhirDocuments operation</span></span>

<span data-ttu-id="4eee3-154">**Bemenetek**</span><span class="sxs-lookup"><span data-stu-id="4eee3-154">**Inputs**</span></span>
- <span data-ttu-id="4eee3-155">DatabaseId</span><span class="sxs-lookup"><span data-stu-id="4eee3-155">DatabaseId</span></span>
- <span data-ttu-id="4eee3-156">CollectionId</span><span class="sxs-lookup"><span data-stu-id="4eee3-156">CollectionId</span></span>
- <span data-ttu-id="4eee3-157">HL7 FHIR erőforrástípus neve</span><span class="sxs-lookup"><span data-stu-id="4eee3-157">HL7 FHIR Resource Type name</span></span>
- <span data-ttu-id="4eee3-158">Logikai érték: Indítsa el az elejétől</span><span class="sxs-lookup"><span data-stu-id="4eee3-158">Boolean: Start from Beginning</span></span>
- <span data-ttu-id="4eee3-159">Int: Visszaadott dokumentumok száma</span><span class="sxs-lookup"><span data-stu-id="4eee3-159">Int: Number of documents returned</span></span>

<span data-ttu-id="4eee3-160">**Kimenetek**</span><span class="sxs-lookup"><span data-stu-id="4eee3-160">**Outputs**</span></span>
- <span data-ttu-id="4eee3-161">Sikerült: Állapotkód: 200, válasz: dokumentumok (JSON-tömb) listája</span><span class="sxs-lookup"><span data-stu-id="4eee3-161">Success: Status Code: 200, Response: List of Documents (JSON Array)</span></span>
- <span data-ttu-id="4eee3-162">Hiba: Állapotkód: 404-es, válasz: "nem található a"*erőforrás neve "* erőforrástípus"</span><span class="sxs-lookup"><span data-stu-id="4eee3-162">Failure: Status Code: 404, Response: "No Documents found for '*resource name'* Resource Type"</span></span>

<a id="api-app-source"></a>

<span data-ttu-id="4eee3-163">**Forrás hello API-alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="4eee3-163">**Source for hello API app**</span></span>

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

### <a name="testing-hello-fhirnotificationapi"></a><span data-ttu-id="4eee3-164">Hello FhirNotificationApi tesztelése</span><span class="sxs-lookup"><span data-stu-id="4eee3-164">Testing hello FhirNotificationApi</span></span> 

<span data-ttu-id="4eee3-165">hello következő kép bemutatja, hogyan swagger lett használt tootootest hello [FhirNotificationApi](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="4eee3-165">hello following image demonstrates how swagger was used tootootest hello [FhirNotificationApi](#api-app-source).</span></span>

![a Swagger-fájl hello használt tootest hello API-alkalmazás](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a><span data-ttu-id="4eee3-167">Azure-portál irányítópultjának</span><span class="sxs-lookup"><span data-stu-id="4eee3-167">Azure portal dashboard</span></span>

<span data-ttu-id="4eee3-168">a következő kép hello mutatja hello ehhez a megoldáshoz hello Azure-portálon futó Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="4eee3-168">hello following image shows all of hello Azure services for this solution running in hello Azure portal.</span></span>

![hello megjelenítő HL7 FHIR oktatóanyagban használt összes hello szolgáltatást Azure-portálon](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a><span data-ttu-id="4eee3-170">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4eee3-170">Summary</span></span>

- <span data-ttu-id="4eee3-171">Megtanulta, hogy Azure Cosmos DB natív támogatja a következő kapcsolattípust értesítéseket az új vagy módosított dokumentumokat, és milyen egyszerűen toouse rendelkezik-e.</span><span class="sxs-lookup"><span data-stu-id="4eee3-171">You have learned that Azure Cosmos DB has native suppport for notifications for new or modifed documents and how easy it is toouse.</span></span> 
- <span data-ttu-id="4eee3-172">A Logic Apps használatával programozás nélkül munkafolyamatokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="4eee3-172">By leveraging Logic Apps, you can create workflows without writing any code.</span></span>
- <span data-ttu-id="4eee3-173">Azure Service Bus-üzenetsorok toohandle hello eloszlás használatával hello HL7 FHIR dokumentumokhoz.</span><span class="sxs-lookup"><span data-stu-id="4eee3-173">Using Azure Service Bus Queues toohandle hello distribution for hello HL7 FHIR documents.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4eee3-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4eee3-174">Next steps</span></span>
<span data-ttu-id="4eee3-175">Azure Cosmos DB kapcsolatos további információkért lásd: hello [Azure Cosmos DB kezdőlap](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="4eee3-175">For more information about Azure Cosmos DB, see hello [Azure Cosmos DB home page](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="4eee3-176">A Logic Apps kapcsolatos további információk megadására, lásd: [Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="4eee3-176">For more informaiton about Logic Apps, see [Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>


