---
title: "egy szolgáltatás a piactér hello aaaGuide toocreating |} Microsoft Docs"
description: "Hogyan toocreate, hitelesíthet és az adatok szolgáltatás telepítése részletes utasításokat vásárolja meg a hello Azure piactéren."
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
ms.openlocfilehash: 8917a43959834d15f70866297f98d24bb83e217f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="examples-of-mapping-an-existing-web-service-tooodata-through-csdls"></a><span data-ttu-id="63291-103">Példák leképezése egy meglévő webes szolgáltatás tooOData CSDLs keresztül</span><span class="sxs-lookup"><span data-stu-id="63291-103">Examples of mapping an existing web service tooOData through CSDLs</span></span>
> [!IMPORTANT]
> <span data-ttu-id="63291-104">**Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.**</span><span class="sxs-lookup"><span data-stu-id="63291-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="63291-105">Ha rendelkezik a Szolgáltatottszoftver-üzleti alkalmazások toopublish szeretné a AppSource tekinthet meg további információkat [Itt](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="63291-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="63291-106">Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatást, akkor például az Azure piactér toopublish tekinthet meg további információkat [Itt](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="63291-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

## <a name="example-functionimport-for-raw-data-returned-using-post"></a><span data-ttu-id="63291-107">Példa: FunctionImport "Raw" adatokat adott vissza "POST" használatával</span><span class="sxs-lookup"><span data-stu-id="63291-107">Example: FunctionImport for "Raw" data returned using "POST"</span></span>
<span data-ttu-id="63291-108">Használja a FELADÁS egy vagy több nyers adatok toocreate egy új alárendelt, és a kiszolgáló URL(location) vagy alárendelt hello kiszolgálón hello tooupdate részeként definiált URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="63291-108">Use POST Raw data toocreate a new subordinate and return its server defined URL(location) or tooupdate part of hello subordinate at hello server defined URL.</span></span>  <span data-ttu-id="63291-109">Ahol a hello alárendelt adatfolyam, azaz strukturálatlan, például az.</span><span class="sxs-lookup"><span data-stu-id="63291-109">Where hello subordinate is a stream, i.e. unstructured, ex.</span></span> <span data-ttu-id="63291-110">szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="63291-110">a text file.</span></span>  <span data-ttu-id="63291-111">Ügyeljen arra POST a nem az idempotent hely nélkül.</span><span class="sxs-lookup"><span data-stu-id="63291-111">Beware POST in not idempotent without a location.</span></span>

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

## <a name="example-functionimport-using-delete"></a><span data-ttu-id="63291-112">Példa: FunctionImport "DELETE" használatával</span><span class="sxs-lookup"><span data-stu-id="63291-112">Example: FunctionImport using "DELETE"</span></span>
<span data-ttu-id="63291-113">Használja a DELETE tooremove a megadott URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="63291-113">Use DELETE tooremove a specified URI.</span></span>

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

## <a name="example-functionimport-using-post"></a><span data-ttu-id="63291-114">Példa: FunctionImport "POST" használatával</span><span class="sxs-lookup"><span data-stu-id="63291-114">Example: FunctionImport using "POST"</span></span>
<span data-ttu-id="63291-115">Használja a FELADÁS egy vagy több nyers adatok toocreate egy új alárendelt, és a kiszolgáló URL(location) vagy alárendelt hello kiszolgálón hello tooupdate részeként definiált URL-címet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="63291-115">Use POST Raw data toocreate a new subordinate and return its server defined URL(location) or tooupdate part of hello subordinate at hello server defined URL.</span></span>  <span data-ttu-id="63291-116">Hello alárendelt esetén struktúrában.</span><span class="sxs-lookup"><span data-stu-id="63291-116">Where hello subordinate is a structure.</span></span> <span data-ttu-id="63291-117">Ügyeljen arra POST nincs idempotent hely nélkül.</span><span class="sxs-lookup"><span data-stu-id="63291-117">Beware POST is not idempotent without a location.</span></span>

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

## <a name="example-functionimport-using-put"></a><span data-ttu-id="63291-118">Példa: FunctionImport "PUT" használatával</span><span class="sxs-lookup"><span data-stu-id="63291-118">Example: FunctionImport using "PUT"</span></span>
<span data-ttu-id="63291-119">PUT toocreate egy új alárendelt használja, vagy tooupdate hello teljes alárendelt egy kiszolgálón megadott URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="63291-119">Use PUT toocreate a new subordinate or tooupdate hello entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="63291-120">Hello alárendelt az struktúra, PUT-e az idempotent, így több eredményez hello azonos állapot, azaz</span><span class="sxs-lookup"><span data-stu-id="63291-120">Where hello subordinate is a structure, PUT is idempotent so multiple occurrences will result in hello same state, i.e</span></span> <span data-ttu-id="63291-121">x = 5.</span><span class="sxs-lookup"><span data-stu-id="63291-121">x=5.</span></span>  <span data-ttu-id="63291-122">PUT kell használni a megadott hello tartalom teljes hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="63291-122">Put should be used with hello full content of hello specified resource.</span></span>

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


## <a name="example-functionimport-for-raw-data-returned-using-put"></a><span data-ttu-id="63291-123">Példa: FunctionImport "Raw" adatokat adott vissza "PUT" használatával</span><span class="sxs-lookup"><span data-stu-id="63291-123">Example: FunctionImport for "Raw" data returned using "PUT"</span></span>
<span data-ttu-id="63291-124">Használja a PUT nyers adatok toocreate egy új alárendelt vagy tooupdate hello teljes alárendelt a kiszolgáló által meghatározott URL-címen.</span><span class="sxs-lookup"><span data-stu-id="63291-124">Use PUT Raw data toocreate a new subordinate or tooupdate hello entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="63291-125">Ahol a hello alárendelt adatfolyam, azaz strukturálatlan, például az.</span><span class="sxs-lookup"><span data-stu-id="63291-125">Where hello subordinate is a stream, i.e. unstructured, ex.</span></span> <span data-ttu-id="63291-126">szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="63291-126">a text file.</span></span>  <span data-ttu-id="63291-127">A PUT idempotent, így a több eredményez hello azonos állapot, azaz</span><span class="sxs-lookup"><span data-stu-id="63291-127">PUT is idempotent so multiple occurrences will result in hello same state, i.e</span></span> <span data-ttu-id="63291-128">x = 5.</span><span class="sxs-lookup"><span data-stu-id="63291-128">x=5.</span></span>  <span data-ttu-id="63291-129">PUT kell használni a megadott hello tartalom teljes hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="63291-129">Put should be used with hello full content of hello specified resource.</span></span>

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


## <a name="example-functionimport-for-raw-data-returned-using-get"></a><span data-ttu-id="63291-130">Példa: FunctionImport "Raw" adatokat adott vissza, használja a "GET"</span><span class="sxs-lookup"><span data-stu-id="63291-130">Example: FunctionImport for "Raw" data returned using "GET"</span></span>
<span data-ttu-id="63291-131">Használja az beszerzése nyers adatok tooreturn egy alárendelt, amely a strukturálatlan, azaz szöveg.</span><span class="sxs-lookup"><span data-stu-id="63291-131">Use GET Raw data tooreturn a subordinate that is unstructured, i.e. text.</span></span>

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

## <a name="example-functionimport-for-paging-through-returned-data"></a><span data-ttu-id="63291-132">Példa: FunctionImport "Lapozás" keresztül visszaadott adatok</span><span class="sxs-lookup"><span data-stu-id="63291-132">Example: FunctionImport for "Paging" through returned data</span></span>
<span data-ttu-id="63291-133">Az adatok megvalósítása RESTful lapozást használ GET.</span><span class="sxs-lookup"><span data-stu-id="63291-133">Use implement RESTful paging through your data with GET.</span></span>  <span data-ttu-id="63291-134">Alapértelmezett lapozás too100 adatsor laponként van beállítva.</span><span class="sxs-lookup"><span data-stu-id="63291-134">Default paging is set too100 row per page of data.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="63291-135">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="63291-135">See Also</span></span>
* <span data-ttu-id="63291-136">A cikkből megtudhatja, hogyan készítheti ismertetése a hello általános OData-megfeleltetési folyamat és a cél, [szolgáltatás OData megfeleltetése](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definíciókat, és a utasításokat.</span><span class="sxs-lookup"><span data-stu-id="63291-136">If you are interested in understanding hello overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definitions, structures, and instructions.</span></span>
* <span data-ttu-id="63291-137">Ha érdekli tanulási és ismertetése hello adott csomópontok és a paraméterek, olvassa el ebben a cikkben [szolgáltatás OData leképezési Adatcsomópontokat](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definíciók és magyarázatokat, példák és a nagybetűk használatát a környezetben.</span><span class="sxs-lookup"><span data-stu-id="63291-137">If you are interested in learning and understanding hello specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="63291-138">Ebben a cikkben egy adatszolgáltatás toohello Azure piactér-közzététel elérési előírt tooreturn toohello [adatok szolgáltatás közzétételi útmutató](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="63291-138">tooreturn toohello prescribed path for publishing a Data Service toohello Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

