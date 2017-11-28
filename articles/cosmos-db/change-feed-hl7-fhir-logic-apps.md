---
title: "Adatcsatorna HL7 FHIR erőforrások - Azure Cosmos DB módosítása |} Microsoft Docs"
description: "Megtudhatja, hogyan állíthatja be a változási értesítéseket HL7 FHIR beteg egészségügyi rekordok Azure Logic Apps, az Azure Cosmos DB és a Service Bus használatával."
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
ms.openlocfilehash: d2b50c0b6864af41fb9cfa051721c432772b228d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a><span data-ttu-id="154a3-104">A Logic Apps és az Azure Cosmos DB használatával HL7 FHIR egészségügyi rekord módosítása betegek értesítése</span><span class="sxs-lookup"><span data-stu-id="154a3-104">Notifying patients of HL7 FHIR health care record changes using Logic Apps and Azure Cosmos DB</span></span>

<span data-ttu-id="154a3-105">Az Azure MVP Howard Edidin nemrég egészségügyi szervezetek, amelyek hozzáadni az új funkciókat a beteg portálra felvenni a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="154a3-105">Azure MVP Howard Edidin was recently contacted by a healthcare organization that wanted to add new functionality to their patient portal.</span></span> <span data-ttu-id="154a3-106">Értesítések küldéséhez meghozandó, az egészségügyi frissítették, és szeretne előfizetni a frissítések betegek szükség rájuk szükség rájuk.</span><span class="sxs-lookup"><span data-stu-id="154a3-106">They needed to send notifications to patients when their health record was updated, and they needed patients to be able to subscribe to these updates.</span></span> 

<span data-ttu-id="154a3-107">Ez a cikk végigvezeti a módosítás hírcsatorna a egészségügyi szervezet Azure Cosmos DB, a Logic Apps és a Service Bus használatával létrehozott értesítési megoldást.</span><span class="sxs-lookup"><span data-stu-id="154a3-107">This article walks through the change feed notification solution created for this healthcare organization using Azure Cosmos DB, Logic Apps, and Service Bus.</span></span> 

## <a name="project-requirements"></a><span data-ttu-id="154a3-108">Projekt követelmények</span><span class="sxs-lookup"><span data-stu-id="154a3-108">Project requirements</span></span>
- <span data-ttu-id="154a3-109">Szolgáltatók küldése HL7 konszolidált klinikai dokumentum architektúra (C-CDA) dokumentumok XML formátumban.</span><span class="sxs-lookup"><span data-stu-id="154a3-109">Providers send HL7 Consolidated-Clinical Document Architecture (C-CDA) documents in XML format.</span></span> <span data-ttu-id="154a3-110">C-CDA dokumentumok tartalmazzák a klinikai dokumentum, beleértve a klinikai dokumentumok például családba tartozó alábbi előzményeinek és azon rögzíti, valamint a felügyeleti, a munkafolyamat és a pénzügyi dokumentumok szinte bármilyen típusú.</span><span class="sxs-lookup"><span data-stu-id="154a3-110">C-CDA documents encompass just about every type of clinical document, including clinical documents such as family histories and immunization records, as well as administrative, workflow, and financial documents.</span></span> 
- <span data-ttu-id="154a3-111">C-CDA dokumentumok alakulnak [HL7 FHIR erőforrások](http://hl7.org/fhir/2017Jan/resourcelist.html) JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="154a3-111">C-CDA documents are converted to [HL7 FHIR Resources](http://hl7.org/fhir/2017Jan/resourcelist.html) in JSON format.</span></span>
- <span data-ttu-id="154a3-112">Módosított FHIR erőforrás dokumentumokat küldött e-mailek JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="154a3-112">Modified FHIR resource documents are sent by email in JSON format.</span></span>

## <a name="solution-workflow"></a><span data-ttu-id="154a3-113">Megoldás munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="154a3-113">Solution workflow</span></span> 

<span data-ttu-id="154a3-114">Magas szinten a projekt szükséges a következő munkafolyamat-lépéseket:</span><span class="sxs-lookup"><span data-stu-id="154a3-114">At a high level, the project required the following workflow steps:</span></span> 
1. <span data-ttu-id="154a3-115">C-CDA dokumentumok átalakítása FHIR erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="154a3-115">Convert C-CDA documents to FHIR resources.</span></span>
2. <span data-ttu-id="154a3-116">Ciklikus lekérdezés a módosított FHIR erőforrások ismétlődő eseményindító végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="154a3-116">Perform recurring trigger polling for modified FHIR resources.</span></span> 
2. <span data-ttu-id="154a3-117">Egyéni alkalmazás, FhirNotificationApi Azure Cosmos adatbázis és az új vagy módosított dokumentumok lekérdezés való kapcsolódáshoz hívja.</span><span class="sxs-lookup"><span data-stu-id="154a3-117">Call a custom app, FhirNotificationApi, to connect to Azure Cosmos DB and query for new or modified documents.</span></span>
3. <span data-ttu-id="154a3-118">Mentse a Service Bus várólista-válasz.</span><span class="sxs-lookup"><span data-stu-id="154a3-118">Save the response to to the Service Bus queue.</span></span>
4. <span data-ttu-id="154a3-119">A lekérdezési az új üzenetek a Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="154a3-119">Poll for new messages in the Service Bus queue.</span></span>
5. <span data-ttu-id="154a3-120">E-mail értesítéseket küldhet meghozandó.</span><span class="sxs-lookup"><span data-stu-id="154a3-120">Send email notifications to patients.</span></span>

## <a name="solution-architecture"></a><span data-ttu-id="154a3-121">Megoldás architektúrája</span><span class="sxs-lookup"><span data-stu-id="154a3-121">Solution architecture</span></span>
<span data-ttu-id="154a3-122">Ez a megoldás három Logic Apps a fenti követelményeknek, és a megoldás munkafolyamat befejezéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="154a3-122">This solution requires three Logic Apps to meet the above requirements and complete the solution workflow.</span></span> <span data-ttu-id="154a3-123">A három a logic apps a következők:</span><span class="sxs-lookup"><span data-stu-id="154a3-123">The three logic apps are:</span></span>
1. <span data-ttu-id="154a3-124">**HL7-FHIR-leképezés app**: a HL7 C-CDA dokumentum kap, átalakítja a FHIR erőforráshoz, majd menti azt az Azure Cosmos-Adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="154a3-124">**HL7-FHIR-Mapping app**: Receives the HL7 C-CDA document, transforms it to the FHIR Resource, then saves it to Azure Cosmos DB.</span></span>
2. <span data-ttu-id="154a3-125">**EHR app**: az Azure Cosmos DB FHIR tárház lekérdezése, és menti a válasz a Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="154a3-125">**EHR app**: Queries the Azure Cosmos DB FHIR repository and saves the response to a Service Bus queue.</span></span> <span data-ttu-id="154a3-126">A logikai alkalmazást használ egy [API-alkalmazás](#api-app) új és módosított dokumentumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="154a3-126">This logic app uses an [API app](#api-app) to retrieve new and changed documents.</span></span>
3. <span data-ttu-id="154a3-127">**Folyamat értesítési app**: a FHIR erőforrás dokumentumok e-mailben értesítést küld a törzsében.</span><span class="sxs-lookup"><span data-stu-id="154a3-127">**Process notification app**: Sends an email notification with the FHIR resource documents in the body.</span></span>

![A három Logic Apps a HL7 FHIR egészségügyi megoldásban használt](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-the-solution"></a><span data-ttu-id="154a3-129">A megoldásban használt Azure-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="154a3-129">Azure services used in the solution</span></span>

#### <a name="azure-cosmos-db-documentdb-api"></a><span data-ttu-id="154a3-130">Az Azure Cosmos DB DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="154a3-130">Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="154a3-131">Azure Cosmos-adatbázis a FHIR erőforrások tárháza, az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="154a3-131">Azure Cosmos DB is the repository for the FHIR resources as shown in the following figure.</span></span>

![A HL7 FHIR egészségügyi oktatóanyagban használt Azure Cosmos DB fiók](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a><span data-ttu-id="154a3-133">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="154a3-133">Logic Apps</span></span>
<span data-ttu-id="154a3-134">Logic Apps alkalmazásokat kezeléséhez a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="154a3-134">Logic Apps handle the workflow process.</span></span> <span data-ttu-id="154a3-135">Az alábbi képek a létre ebben a megoldásban a Logic apps megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="154a3-135">The following screenshots show the Logic apps created for this solution.</span></span> 


1. <span data-ttu-id="154a3-136">**HL7-FHIR-leképezés app**: megkapja a HL7 C-CDA dokumentumot, és irányítópulttá, használja a vállalati integrációs csomag Logic Apps FHIR erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="154a3-136">**HL7-FHIR-Mapping app**: Receive the HL7 C-CDA document and transform it to an FHIR resource using the Enterprise Integration Pack for Logic Apps.</span></span> <span data-ttu-id="154a3-137">A vállalati integrációs csomag kezeli a leképezés a C-CDA FHIR erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="154a3-137">The Enterprise Integration Pack handles the mapping from the C-CDA to FHIR resources.</span></span>

    ![A logikai alkalmazás HL7 FHIR egészségügyi rekordok fogadására szolgáló](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. <span data-ttu-id="154a3-139">**EHR app**: az Azure Cosmos DB FHIR tárház lekérdezése, és mentse a válasz a Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="154a3-139">**EHR app**: Query the Azure Cosmos DB FHIR repository and save the response to a Service Bus queue.</span></span> <span data-ttu-id="154a3-140">A GetNewOrModifiedFHIRDocuments alkalmazás kódja nem éri el.</span><span class="sxs-lookup"><span data-stu-id="154a3-140">The code for the GetNewOrModifiedFHIRDocuments app is below.</span></span>

    ![A logikai alkalmazást használt Azure Cosmos DB lekérdezése](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. <span data-ttu-id="154a3-142">**Folyamat értesítési app**: a FHIR erőforrás dokumentumokkal e-mail értesítés küldése a törzsében.</span><span class="sxs-lookup"><span data-stu-id="154a3-142">**Process notification app**: Send an email notification with the FHIR resource documents in the body.</span></span>

    ![A logikai alkalmazást a HL7 FHIR erőforrás beteg e-maileket küldő törzsében.](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a><span data-ttu-id="154a3-144">Service Bus</span><span class="sxs-lookup"><span data-stu-id="154a3-144">Service Bus</span></span>
<span data-ttu-id="154a3-145">Az alábbi ábrán láthatók a betegek várólista.</span><span class="sxs-lookup"><span data-stu-id="154a3-145">The following figure shows the patients queue.</span></span> <span data-ttu-id="154a3-146">A címke tulajdonság értékét használja az e-mail tárgyát.</span><span class="sxs-lookup"><span data-stu-id="154a3-146">The Tag property value is used for the email subject.</span></span>

![A Service Bus-üzenetsorba HL7 FHIR oktatóanyagban használt](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a><span data-ttu-id="154a3-148">API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="154a3-148">API app</span></span>
<span data-ttu-id="154a3-149">Az API-alkalmazások Azure Cosmos adatbázis és az új vagy módosított FHIR dokumentumok erőforrástípusok szerint lekérdezések csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="154a3-149">An API app connects to Azure Cosmos DB and queries for new or modified FHIR documents By resource type.</span></span> <span data-ttu-id="154a3-150">Ez az alkalmazás rendelkezik egy tartományvezérlő, **FhirNotificationApi** egy művelettel **GetNewOrModifiedFhirDocuments**, lásd: [API-alkalmazás forrását](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="154a3-150">This app has one controller, **FhirNotificationApi** with a one operation **GetNewOrModifiedFhirDocuments**, see [source for API app](#api-app-source).</span></span>

<span data-ttu-id="154a3-151">Használjuk a [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) osztály az Azure Cosmos DB DocumentDB .NET API-t.</span><span class="sxs-lookup"><span data-stu-id="154a3-151">We are using the [`CreateDocumentChangeFeedQuery`](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) class from the Azure Cosmos DB DocumentDB .NET API.</span></span> <span data-ttu-id="154a3-152">További információkért lásd: a [módosítás hírcsatorna cikk](change-feed.md).</span><span class="sxs-lookup"><span data-stu-id="154a3-152">For more information, see the [change feed article](change-feed.md).</span></span> 

##### <a name="getnewormodifiedfhirdocuments-operation"></a><span data-ttu-id="154a3-153">GetNewOrModifiedFhirDocuments művelet</span><span class="sxs-lookup"><span data-stu-id="154a3-153">GetNewOrModifiedFhirDocuments operation</span></span>

<span data-ttu-id="154a3-154">**Bemenetek**</span><span class="sxs-lookup"><span data-stu-id="154a3-154">**Inputs**</span></span>
- <span data-ttu-id="154a3-155">DatabaseId</span><span class="sxs-lookup"><span data-stu-id="154a3-155">DatabaseId</span></span>
- <span data-ttu-id="154a3-156">CollectionId</span><span class="sxs-lookup"><span data-stu-id="154a3-156">CollectionId</span></span>
- <span data-ttu-id="154a3-157">HL7 FHIR erőforrástípus neve</span><span class="sxs-lookup"><span data-stu-id="154a3-157">HL7 FHIR Resource Type name</span></span>
- <span data-ttu-id="154a3-158">Logikai érték: Indítsa el az elejétől</span><span class="sxs-lookup"><span data-stu-id="154a3-158">Boolean: Start from Beginning</span></span>
- <span data-ttu-id="154a3-159">Int: Visszaadott dokumentumok száma</span><span class="sxs-lookup"><span data-stu-id="154a3-159">Int: Number of documents returned</span></span>

<span data-ttu-id="154a3-160">**Kimenetek**</span><span class="sxs-lookup"><span data-stu-id="154a3-160">**Outputs**</span></span>
- <span data-ttu-id="154a3-161">Sikerült: Állapotkód: 200, válasz: dokumentumok (JSON-tömb) listája</span><span class="sxs-lookup"><span data-stu-id="154a3-161">Success: Status Code: 200, Response: List of Documents (JSON Array)</span></span>
- <span data-ttu-id="154a3-162">Hiba: Állapotkód: 404-es, válasz: "nem található a"*erőforrás neve "* erőforrástípus"</span><span class="sxs-lookup"><span data-stu-id="154a3-162">Failure: Status Code: 404, Response: "No Documents found for '*resource name'* Resource Type"</span></span>

<a id="api-app-source"></a>

<span data-ttu-id="154a3-163">**Az API-alkalmazás forrása**</span><span class="sxs-lookup"><span data-stu-id="154a3-163">**Source for the API app**</span></span>

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
            ///     Gets the new or modified FHIR documents from Last Run Date 
            ///     or create date of the collection
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

### <a name="testing-the-fhirnotificationapi"></a><span data-ttu-id="154a3-164">A FhirNotificationApi tesztelése</span><span class="sxs-lookup"><span data-stu-id="154a3-164">Testing the FhirNotificationApi</span></span> 

<span data-ttu-id="154a3-165">A következő kép bemutatja, hogyan swagger lett megadva a tesztelése a [FhirNotificationApi](#api-app-source).</span><span class="sxs-lookup"><span data-stu-id="154a3-165">The following image demonstrates how swagger was used to to test the [FhirNotificationApi](#api-app-source).</span></span>

![A Swagger-fájl, amellyel tesztelheti az API-alkalmazás](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a><span data-ttu-id="154a3-167">Azure-portál irányítópultjának</span><span class="sxs-lookup"><span data-stu-id="154a3-167">Azure portal dashboard</span></span>

<span data-ttu-id="154a3-168">A következő kép bemutatja az összes Azure-szolgáltatás fut az Azure portálon ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="154a3-168">The following image shows all of the Azure services for this solution running in the Azure portal.</span></span>

![Az Azure-portálon megjelenítő HL7 FHIR oktatóanyagban használt minden szolgáltatás](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a><span data-ttu-id="154a3-170">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="154a3-170">Summary</span></span>

- <span data-ttu-id="154a3-171">Megtanulta, hogy Azure Cosmos DB natív támogatja a következő kapcsolattípust értesítéseket az új vagy módosított dokumentumokat és milyen egyszerűen használatára van-e.</span><span class="sxs-lookup"><span data-stu-id="154a3-171">You have learned that Azure Cosmos DB has native suppport for notifications for new or modifed documents and how easy it is to use.</span></span> 
- <span data-ttu-id="154a3-172">A Logic Apps használatával programozás nélkül munkafolyamatokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="154a3-172">By leveraging Logic Apps, you can create workflows without writing any code.</span></span>
- <span data-ttu-id="154a3-173">Azure Service Bus-üzenetsorok használatával a terjesztési HL7 FHIR dokumentumok kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="154a3-173">Using Azure Service Bus Queues to handle the distribution for the HL7 FHIR documents.</span></span>

## <a name="next-steps"></a><span data-ttu-id="154a3-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="154a3-174">Next steps</span></span>
<span data-ttu-id="154a3-175">Azure Cosmos DB kapcsolatos további információkért tekintse meg a [Azure Cosmos DB kezdőlap](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="154a3-175">For more information about Azure Cosmos DB, see the [Azure Cosmos DB home page](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="154a3-176">A Logic Apps kapcsolatos további információk megadására, lásd: [Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="154a3-176">For more informaiton about Logic Apps, see [Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>


