---
title: "Egy szolgáltatás számára a piactér létrehozását bemutató útmutatónak |} Microsoft Docs"
description: "Részletes utasításokra van szüksége, hogy hogyan hozhatja létre, hitelesíthet és az adatok szolgáltatás telepítése az Azure piactéren vásárolhat."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 148f8638-ee80-4100-8d63-5afa4167ca1b
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 2ab624941fc385f14b62bb5d743927f157955845
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="examples-of-mapping-an-existing-web-service-to-odata-through-csdls"></a><span data-ttu-id="ad6af-103">Meglévő webszolgáltatás leképezése CSDLs keresztül OData példák</span><span class="sxs-lookup"><span data-stu-id="ad6af-103">Examples of mapping an existing web service to OData through CSDLs</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ad6af-104">**Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.**</span><span class="sxs-lookup"><span data-stu-id="ad6af-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="ad6af-105">Ha egy SaaS-üzleti AppSource a közzétenni kívánt alkalmazás további információt talál [Itt](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="ad6af-105">If you have a SaaS business application you would like to publish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="ad6af-106">Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatás szeretne közzétenni az Azure piactérről, további információt talál [Itt](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="ad6af-106">If you have an IaaS applications or developer service you would like to publish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

## <a name="example-functionimport-for-raw-data-returned-using-post"></a><span data-ttu-id="ad6af-107">Példa: FunctionImport "Raw" adatokat adott vissza "POST" használatával</span><span class="sxs-lookup"><span data-stu-id="ad6af-107">Example: FunctionImport for "Raw" data returned using "POST"</span></span>
<span data-ttu-id="ad6af-108">POST nyers adatok használatával hozzon létre egy új alárendelt, és a kiszolgáló URL(location) vagy frissíteni a kiszolgálón az alárendelt részeként definiált URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ad6af-108">Use POST Raw data to create a new subordinate and return its server defined URL(location) or to update part of the subordinate at the server defined URL.</span></span>  <span data-ttu-id="ad6af-109">Ahol a alárendelt az adatfolyam, azaz</span><span class="sxs-lookup"><span data-stu-id="ad6af-109">Where the subordinate is a stream, i.e.</span></span> <span data-ttu-id="ad6af-110">strukturálatlan, amelyekben az ex.</span><span class="sxs-lookup"><span data-stu-id="ad6af-110">unstructured, ex.</span></span> <span data-ttu-id="ad6af-111">szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="ad6af-111">a text file.</span></span>  <span data-ttu-id="ad6af-112">Ügyeljen arra POST a nem az idempotent hely nélkül.</span><span class="sxs-lookup"><span data-stu-id="ad6af-112">Beware POST in not idempotent without a location.</span></span>

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="AddUsageEvent" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Add usage event (data acquisition)</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-delete"></a><span data-ttu-id="ad6af-113">Példa: FunctionImport "DELETE" használatával</span><span class="sxs-lookup"><span data-stu-id="ad6af-113">Example: FunctionImport using "DELETE"</span></span>
<span data-ttu-id="ad6af-114">TÖRLÉS segítségével távolítsa el a megadott URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="ad6af-114">Use DELETE to remove a specified URI.</span></span>

        <EntitySet Name="DeleteUsageFileEntitySet" EntityType="MyOffer.DeleteUsageFileEntity" />
        <FunctionImport Name="DeleteUsageFile" EntitySet="DeleteUsageFileEntitySet" ReturnType="Collection(MyOffer.DeleteUsageFileEntity)"  d:AllowedHttpMethods="DELETE" d:EncodeParameterValues="true” d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643" >
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Delete usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="DeleteUsageFileEntity" d:Map="//boolean">
        <Property Name="boolean" Type="String" Nullable="true" d:Map="./boolean" />
        </EntityType>

## <a name="example-functionimport-using-post"></a><span data-ttu-id="ad6af-115">Példa: FunctionImport "POST" használatával</span><span class="sxs-lookup"><span data-stu-id="ad6af-115">Example: FunctionImport using "POST"</span></span>
<span data-ttu-id="ad6af-116">POST nyers adatok használatával hozzon létre egy új alárendelt, és a kiszolgáló URL(location) vagy frissíteni a kiszolgálón az alárendelt részeként definiált URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ad6af-116">Use POST Raw data to create a new subordinate and return its server defined URL(location) or to update part of the subordinate at the server defined URL.</span></span>  <span data-ttu-id="ad6af-117">Az alárendelt esetén struktúrában.</span><span class="sxs-lookup"><span data-stu-id="ad6af-117">Where the subordinate is a structure.</span></span> <span data-ttu-id="ad6af-118">Ügyeljen arra POST nincs idempotent hely nélkül.</span><span class="sxs-lookup"><span data-stu-id="ad6af-118">Beware POST is not idempotent without a location.</span></span>

        <EntitySet Name="CreateANewModelEntitySet2" EntityType=" MyOffer.CreateANewModelEntity2" />
        <FunctionImport Name="CreateModel" EntitySet="CreateANewModelEntitySet2" ReturnType="Collection(MyOffer.CreateANewModelEntity2)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Create A New Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-put"></a><span data-ttu-id="ad6af-119">Példa: FunctionImport "PUT" használatával</span><span class="sxs-lookup"><span data-stu-id="ad6af-119">Example: FunctionImport using "PUT"</span></span>
<span data-ttu-id="ad6af-120">PUT segítségével hozzon létre egy új alárendelt, vagy frissíteni a teljes alárendelt egy kiszolgálón megadott URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="ad6af-120">Use PUT to create a new subordinate or to update the entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="ad6af-121">Ha az alárendelt struktúra, PUT-e a idempotent, így a több hatására olyan állapotban, azaz</span><span class="sxs-lookup"><span data-stu-id="ad6af-121">Where the subordinate is a structure, PUT is idempotent so multiple occurrences will result in the same state, i.e</span></span> <span data-ttu-id="ad6af-122">x = 5.</span><span class="sxs-lookup"><span data-stu-id="ad6af-122">x=5.</span></span>  <span data-ttu-id="ad6af-123">A megadott erőforrás a teljes tartalommal PUT kell használni.</span><span class="sxs-lookup"><span data-stu-id="ad6af-123">Put should be used with the full content of the specified resource.</span></span>

        <EntitySet Name="UpdateAnExistingModelEntitySet" EntityType="MyOffer.UpdateAnExistingModelEntity" />
        <FunctionImport Name="UpdateModel" EntitySet="UpdateAnExistingModelEntitySet" ReturnType="Collection(MyOffer.UpdateAnExistingModelEntity)" d:EncodeParameterValues="true" d:AllowedHttpMethods="PUT" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Update an Existing Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="UpdateAnExistingModelEntity" d:Map="//string">
        <Property Name="string"     Type="String" Nullable="true" d:Map="./string" />
        </EntityType>


## <a name="example-functionimport-for-raw-data-returned-using-put"></a><span data-ttu-id="ad6af-124">Példa: FunctionImport "Raw" adatokat adott vissza "PUT" használatával</span><span class="sxs-lookup"><span data-stu-id="ad6af-124">Example: FunctionImport for "Raw" data returned using "PUT"</span></span>
<span data-ttu-id="ad6af-125">Hozzon létre egy új nyers PUT adatokat használjon alárendelt, vagy frissíteni a teljes alárendelt egy kiszolgálón megadott URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="ad6af-125">Use PUT Raw data to create a new subordinate or to update the entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="ad6af-126">Ahol a alárendelt az adatfolyam, azaz</span><span class="sxs-lookup"><span data-stu-id="ad6af-126">Where the subordinate is a stream, i.e.</span></span> <span data-ttu-id="ad6af-127">strukturálatlan, amelyekben az ex.</span><span class="sxs-lookup"><span data-stu-id="ad6af-127">unstructured, ex.</span></span> <span data-ttu-id="ad6af-128">szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="ad6af-128">a text file.</span></span>  <span data-ttu-id="ad6af-129">A PUT idempotent, így a több hatására olyan állapotban, azaz</span><span class="sxs-lookup"><span data-stu-id="ad6af-129">PUT is idempotent so multiple occurrences will result in the same state, i.e</span></span> <span data-ttu-id="ad6af-130">x = 5.</span><span class="sxs-lookup"><span data-stu-id="ad6af-130">x=5.</span></span>  <span data-ttu-id="ad6af-131">A megadott erőforrás a teljes tartalommal PUT kell használni.</span><span class="sxs-lookup"><span data-stu-id="ad6af-131">Put should be used with the full content of the specified resource.</span></span>

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="CancelBuild” ReturnType="Raw(text/plain)" d:AllowedHttpMethods="PUT" d:EncodeParameterValues="true" d:BaseUri=” http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Cancel Build</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>


## <a name="example-functionimport-for-raw-data-returned-using-get"></a><span data-ttu-id="ad6af-132">Példa: FunctionImport "Raw" adatokat adott vissza, használja a "GET"</span><span class="sxs-lookup"><span data-stu-id="ad6af-132">Example: FunctionImport for "Raw" data returned using "GET"</span></span>
<span data-ttu-id="ad6af-133">Nyers beolvasása a visszaadandó adat, egy alárendelt, amely a strukturálatlan, azaz szöveg használja.</span><span class="sxs-lookup"><span data-stu-id="ad6af-133">Use GET Raw data to return a subordinate that is unstructured, i.e. text.</span></span>

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="GetModelUsageFile" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="GET" d:BaseUri="https://cmla.cloudapp.net/api2/model/builder/build?buildId={buildId}&amp;apiVersion={apiVersion}">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Download A Models Usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Accept" d:Value="application/xml,application/xhtml+xml,text/html;" />
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-for-paging-through-returned-data"></a><span data-ttu-id="ad6af-134">Példa: FunctionImport "Lapozás" keresztül visszaadott adatok</span><span class="sxs-lookup"><span data-stu-id="ad6af-134">Example: FunctionImport for "Paging" through returned data</span></span>
<span data-ttu-id="ad6af-135">Az adatok megvalósítása RESTful lapozást használ GET.</span><span class="sxs-lookup"><span data-stu-id="ad6af-135">Use implement RESTful paging through your data with GET.</span></span>  <span data-ttu-id="ad6af-136">Alapértelmezett lapozás érték laponként 100 soraiban levő adatok.</span><span class="sxs-lookup"><span data-stu-id="ad6af-136">Default paging is set to 100 row per page of data.</span></span>

        <EntitySet Name=”CropEntitySet" EntityType="MyOffer.CropEntity" />
        <FunctionImport    Name="GetCropReport" EntitySet="CropEntitySet” ReturnType="Collection(MyOffer.CropEntity)" d:EmitSelfLink="false" d:EncodeParameterValues="true" d:Paging="SkipTake" d:MaxPageSize="100" d:BaseUri="http://api.mydata.org/Crop? report={report}&amp;series={series}&amp;start={$skip}&amp;size=100">
        <Parameter Name="report" Type="Int32" Mode="In" Nullable="false" d:SampleValues="4"  d:enum="1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19"  />
        <Parameter Name="series"    Type="String"    Mode="In" Nullable="false" d:SampleValues="FARM" />
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="text/xml;charset=UTF-8" />
        </d:Headers>
        <d:Namespaces>
        <d:Namespace d:Prefix="diffgr" d:Uri="urn:schemas-microsoft-com:xml-diffgram-v1" />
        </d:Namespaces>
        </FunctionImport>

## <a name="see-also"></a><span data-ttu-id="ad6af-137">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="ad6af-137">See Also</span></span>
* <span data-ttu-id="ad6af-138">Ha a teljes OData-megfeleltetési folyamat és a rendeltetés megválaszolásával, olvassa el ebben a cikkben [szolgáltatás OData megfeleltetése](marketplace-publishing-data-service-creation-odata-mapping.md) áttekintheti a definíciókat, és a utasításokat.</span><span class="sxs-lookup"><span data-stu-id="ad6af-138">If you are interested in understanding the overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) to review definitions, structures, and instructions.</span></span>
* <span data-ttu-id="ad6af-139">Ha érdekli tanulási és az adott csomópont, és a paraméterek ismertetése, olvassa el ebben a cikkben [szolgáltatás OData leképezési Adatcsomópontokat](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definíciók és magyarázatokat, példák és a nagybetűk használatát a környezetben.</span><span class="sxs-lookup"><span data-stu-id="ad6af-139">If you are interested in learning and understanding the specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="ad6af-140">Térjen vissza az előírt elérési út egy szolgáltatás-közzététel az Azure piactéren, olvassa el ebben a cikkben [adatok szolgáltatás közzétételi útmutató](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="ad6af-140">To return to the prescribed path for publishing a Data Service to the Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

