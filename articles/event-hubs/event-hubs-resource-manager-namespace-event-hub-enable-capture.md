---
title: "Azure Event Hubs-névtér létrehozása és a Rögzítés funkció engedélyezése sablon használatával | Microsoft Docs"
description: "Azure Event Hubs-névtér létrehozása egy eseményközponttal és a Rögzítés funkció engedélyezése az Azure Resource Manager sablonjának használatával"
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 8bdda6a2-5ff1-45e3-b696-c553768f1090
ms.service: event-hubs
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 19bbb51868e767aa1d15f4574628b7fd36607207
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-event-hubs-namespace-with-an-event-hub-and-enable-capture-using-an-azure-resource-manager-template"></a><span data-ttu-id="e4c0a-103">Event Hubs-névtér létrehozása egy eseményközponttal és a Rögzítés funkció engedélyezése az Azure Resource Manager-sablonjának használatával</span><span class="sxs-lookup"><span data-stu-id="e4c0a-103">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>

<span data-ttu-id="e4c0a-104">Ez a cikk ismerteti egy olyan Azure Resource Manager-sablon használatát, amely egy Event Hubs-névteret hoz létre egy Event Hubs-példánnyal, valamint leírja az eseményközpont [Capture funkciójának](event-hubs-capture-overview.md) engedélyezését is.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-104">This article shows how to use an Azure Resource Manager template that creates an Event Hubs namespace, with one event hub instance, and also enables the [Capture feature](event-hubs-capture-overview.md) on the event hub.</span></span> <span data-ttu-id="e4c0a-105">A cikk leírja továbbá, hogyan kell meghatározni, hogy mely erőforrások lesznek üzembe helyezve, és hogyan kell meghatározni az üzembe helyezés végrehajtásakor megadandó paramétereket.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-105">The article describes how to define which resources are deployed, and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="e4c0a-106">Ez a sablont használhatja a saját környezeteiben, vagy testre is szabhatja a saját követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="e4c0a-107">Ez a cikk azt is bemutatja, hogyan adhatja meg, hogy a rendszer az Azure Storage-blobokban vagy egy Azure Data Lake Store-ban rögzítse az eseményeket, az Ön által kiválasztott célhely alapján.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-107">This article also shows how to specify that events are captured into either Azure Storage Blobs or an Azure Data Lake Store, based on the destination you choose.</span></span>

<span data-ttu-id="e4c0a-108">A sablonok létrehozásáról további információkat az [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates] (Azure Resource Manager-sablonok készítése) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-108">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="e4c0a-109">További információk az Azure-erőforrások elnevezési szabályainak mintáiról és gyakorlati megoldásairól: [Az Azure-erőforrások elnevezési szabályai][Azure Resources naming conventions].</span><span class="sxs-lookup"><span data-stu-id="e4c0a-109">For more information about patterns and practices for Azure Resources naming conventions, see [Azure Resources naming conventions][Azure Resources naming conventions].</span></span>

<span data-ttu-id="e4c0a-110">Az összes sablon eléréséhez kattintson az alábbi GitHub-hivatkozásokra:</span><span class="sxs-lookup"><span data-stu-id="e4c0a-110">For the complete templates, click the following GitHub links:</span></span>

- <span data-ttu-id="e4c0a-111">[Eseményközpont és a Rögzítés tárolóban sablon engedélyezése][Event Hub and enable Capture to Storage template]</span><span class="sxs-lookup"><span data-stu-id="e4c0a-111">[Event hub and enable Capture to Storage template][Event Hub and enable Capture to Storage template]</span></span> 
- <span data-ttu-id="e4c0a-112">[Eseményközpont és a Rögzítés Azure Data Lake Store-ban sablon engedélyezése][Event Hub and enable Capture to Azure Data Lake Store template]</span><span class="sxs-lookup"><span data-stu-id="e4c0a-112">[Event hub and enable Capture to Azure Data Lake Store template][Event Hub and enable Capture to Azure Data Lake Store template]</span></span>

> [!NOTE]
> <span data-ttu-id="e4c0a-113">A legújabb sablonokért keresse fel az [Azure-gyorssablonok][Azure Quickstart Templates] gyűjteményt, és keressen az Event Hubs kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-113">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="e4c0a-114">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="e4c0a-114">What will you deploy?</span></span>

<span data-ttu-id="e4c0a-115">Ezzel a sablonnal egy Event Hubs-névteret helyez üzembe eseményközponttal, továbbá engedélyezi az [az Event Hubs Rögzítés funkcióját](event-hubs-capture-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e4c0a-115">With this template, you deploy an Event Hubs namespace with an event hub, and also enable [Event Hubs Capture](event-hubs-capture-overview.md).</span></span>

<span data-ttu-id="e4c0a-116">Az [Event Hubs](event-hubs-what-is-event-hubs.md) egy eseményfeldolgozási szolgáltatás, amely az Azure-ba irányuló, nagy léptékű esemény- és telemetriabevitelt biztosít alacsony késéssel és nagy megbízhatósággal.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-116">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used to provide event and telemetry ingress to Azure at massive scale, with low latency and high reliability.</span></span> <span data-ttu-id="e4c0a-117">Az Event Hubs Capture funkciója lehetővé teszi a streamelt Event Hubs-adatok automatikus továbbítását az Azure Blob Storage-ba vagy az Azure Data Lake Store-ba egy megadott időtartamon belül vagy az Ön által kiválasztott méretegységekben.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-117">Event Hubs Capture enables you to automatically deliver the streaming data in Event Hubs to Azure Blob storage or Azure Data Lake Store, within a specified time or size interval of your choosing.</span></span>

<span data-ttu-id="e4c0a-118">Kattintson az alábbi gombra az Event Hubs Azure Storage-ba való rögzítésének engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="e4c0a-118">Click the following button to enable Event Hubs Capture into Azure Storage:</span></span>

<span data-ttu-id="e4c0a-119">[![Üzembe helyezés az Azure-ban](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e4c0a-119">[![Deploy to Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span></span>

<span data-ttu-id="e4c0a-120">Kattintson az alábbi gombra az Event Hubs Azure Data Lake Store-ba való rögzítésének engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="e4c0a-120">Click the following button to enable Event Hubs Capture into Azure Data Lake Store:</span></span>

<span data-ttu-id="e4c0a-121">[![Üzembe helyezés az Azure-ban](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e4c0a-121">[![Deploy to Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="e4c0a-122">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="e4c0a-122">Parameters</span></span>

<span data-ttu-id="e4c0a-123">Az Azure Resource Managerrel meghatározhatja a sablon üzembe helyezésekor megadandó értékek paramétereit.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-123">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="e4c0a-124">A sablonban található egy `Parameters` nevű rész, amely magába foglalja az összes paraméterértéket.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-124">The template includes a section called `Parameters` that contains all the parameter values.</span></span> <span data-ttu-id="e4c0a-125">Azokhoz az értékekhez adjon meg paramétert, amelyek az üzembe helyezendő projekt vagy az üzembe helyezési környezet alapján változhatnak.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-125">You should define a parameter for those values that vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="e4c0a-126">Ne adjon meg olyan paramétereket olyan értékhez, amelyek nem változnak.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-126">Do not define parameters for values that always stay the same.</span></span> <span data-ttu-id="e4c0a-127">A sablonban minden egyes paraméterérték az üzembe helyezendő erőforrások megadásához lesz felhasználva.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-127">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="e4c0a-128">A sablon a következő paramétereket adja meg.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-128">The template defines the following parameters.</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="e4c0a-129">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="e4c0a-129">eventHubNamespaceName</span></span>

<span data-ttu-id="e4c0a-130">A létrehozni kívánt [Event Hubs-névtér](event-hubs-create.md) neve.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-130">The name of the [Event Hubs namespace](event-hubs-create.md) to create.</span></span>

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a><span data-ttu-id="e4c0a-131">eventHubName</span><span class="sxs-lookup"><span data-stu-id="e4c0a-131">eventHubName</span></span>

<span data-ttu-id="e4c0a-132">Az [Event Hubs-névtérben](event-hubs-create.md) létrehozott eseményközpont neve.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-132">The name of the event hub created in the [Event Hubs namespace](event-hubs-create.md).</span></span>

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the event hub"
    }
}
```

### <a name="messageretentionindays"></a><span data-ttu-id="e4c0a-133">messageRetentionInDays</span><span class="sxs-lookup"><span data-stu-id="e4c0a-133">messageRetentionInDays</span></span>

<span data-ttu-id="e4c0a-134">A napok száma, amíg az üzenetek meg lesznek őrizve az eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-134">The number of days to retain the messages in the event hub.</span></span> 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in event hub"
     }
 }
```

### <a name="partitioncount"></a><span data-ttu-id="e4c0a-135">partitionCount</span><span class="sxs-lookup"><span data-stu-id="e4c0a-135">partitionCount</span></span>

<span data-ttu-id="e4c0a-136">Az eseményközpontban létrehozandó partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-136">The number of partitions to create in the event hub.</span></span>

```json
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="captureenabled"></a><span data-ttu-id="e4c0a-137">captureEnabled</span><span class="sxs-lookup"><span data-stu-id="e4c0a-137">captureEnabled</span></span>

<span data-ttu-id="e4c0a-138">A Rögzítés funkció engedélyezése az eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-138">Enable Capture on the event hub.</span></span>

```json
"captureEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Capture for your event hub"
    }
 }
```
### <a name="captureencodingformat"></a><span data-ttu-id="e4c0a-139">captureEncodingFormat</span><span class="sxs-lookup"><span data-stu-id="e4c0a-139">captureEncodingFormat</span></span>

<span data-ttu-id="e4c0a-140">Az eseményadat szerializálásához megadott kódolási formátum.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-140">The encoding format you specify to serialize the event data.</span></span>

```json
"captureEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format in which Capture serializes the EventData"
    }
}
```

### <a name="capturetime"></a><span data-ttu-id="e4c0a-141">captureTime</span><span class="sxs-lookup"><span data-stu-id="e4c0a-141">captureTime</span></span>

<span data-ttu-id="e4c0a-142">Az az időintervallum, amelyben az Event Hubs Capture elkezdi az adatok rögzítését.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-142">The time interval in which Event Hubs Capture starts capturing the data.</span></span>

```json
"captureTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the capture"
    }
}
```

### <a name="capturesize"></a><span data-ttu-id="e4c0a-143">captureSize</span><span class="sxs-lookup"><span data-stu-id="e4c0a-143">captureSize</span></span>
<span data-ttu-id="e4c0a-144">Az a méretegység, amelynek elérésekor a Capture funkció elkezdi az adatok rögzítését.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-144">The size interval at which Capture starts capturing the data.</span></span>

```json
"captureSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"The size window in bytes for capture"
    }
}
```

###<a name="capturenameformat"></a><span data-ttu-id="e4c0a-145">captureNameFormat</span><span class="sxs-lookup"><span data-stu-id="e4c0a-145">captureNameFormat</span></span>

<span data-ttu-id="e4c0a-146">Az Event Hubs Capture által használt névformátum az Avro-fájlok írásakor.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-146">The name format used by Event Hubs Capture to write the Avro files.</span></span> <span data-ttu-id="e4c0a-147">Vegye figyelembe, hogy a Capture névformátumának tartalmaznia kell a következő mezőket: `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}` és `{Second}`.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-147">Note that a Capture name format must contain `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}`, and `{Second}` fields.</span></span> <span data-ttu-id="e4c0a-148">Ezek a mezők bármilyen sorrend szerint rendezhetők, elválasztó karakterekkel vagy azok nélkül.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-148">These can be arranged in any order, with or without delimiters.</span></span>
 
```json
"captureNameFormat": {
      "type": "string",
      "defaultValue": "{Namespace}/{EventHub}/{PartitionId}/{Year}/{Month}/{Day}/{Hour}/{Minute}/{Second}",
      "metadata": {
        "description": "A Capture Name Format must contain {Namespace}, {EventHub}, {PartitionId}, {Year}, {Month}, {Day}, {Hour}, {Minute} and {Second} fields. These can be arranged in any order with or without delimeters. E.g.  Prod_{EventHub}/{Namespace}\\{PartitionId}_{Year}_{Month}/{Day}/{Hour}/{Minute}/{Second}"
      }
    }
  }
```

### <a name="apiversion"></a><span data-ttu-id="e4c0a-149">apiVersion</span><span class="sxs-lookup"><span data-stu-id="e4c0a-149">apiVersion</span></span>

<span data-ttu-id="e4c0a-150">A sablon API-verziója.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-150">The API version of the template.</span></span>

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

<span data-ttu-id="e4c0a-151">Ha az Azure Storage-ot választja célhelyként, használja a következő paramétereket.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-151">Use the following parameters if you choose Azure Storage as your destination.</span></span>

### <a name="destinationstorageaccountresourceid"></a><span data-ttu-id="e4c0a-152">destinationStorageAccountResourceId</span><span class="sxs-lookup"><span data-stu-id="e4c0a-152">destinationStorageAccountResourceId</span></span>

<span data-ttu-id="e4c0a-153">A Rögzítés használatához egy Azure Storage-fiókhoz tartozó erőforrás-azonosító szükséges, amellyel engedélyezheti a rögzítést a választott Storage-fiókban.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-153">Capture requires an Azure Storage account resource ID to enable capturing to your desired Storage account.</span></span>

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing Storage account resource ID where you want the blobs be captured"
    }
 }
```

### <a name="blobcontainername"></a><span data-ttu-id="e4c0a-154">blobContainerName</span><span class="sxs-lookup"><span data-stu-id="e4c0a-154">blobContainerName</span></span>

<span data-ttu-id="e4c0a-155">A blobtároló, amelyben rögzíti az eseményadatokat.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-155">The blob container in which to capture your event data.</span></span>

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container in which you want the blobs captured"
    }
}
```

<span data-ttu-id="e4c0a-156">Ha az Azure Data Lake Store-t választja célhelyként, használja az alábbi paramétereket.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-156">Use the following parameters if you choose Azure Data Lake Store as your destination.</span></span> <span data-ttu-id="e4c0a-157">Az engedélyeket arra a Data Lake Store-útvonalra kell beállítania, ahol az eseményeket rögzíteni kívánja.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-157">You must set permissions on your Data Lake Store path, in which you want to Capture the event.</span></span> <span data-ttu-id="e4c0a-158">Az engedélyek beállítását lásd [ebben a cikkben](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span><span class="sxs-lookup"><span data-stu-id="e4c0a-158">To set permissions, see [this article](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span></span>

###<a name="subscriptionid"></a><span data-ttu-id="e4c0a-159">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="e4c0a-159">subscriptionId</span></span>

<span data-ttu-id="e4c0a-160">Az Event Hubs-névtér és az Azure Data Lake Store előfizetés-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-160">Subscription ID for the Event Hubs namespace and Azure Data Lake Store.</span></span> <span data-ttu-id="e4c0a-161">Mindkét erőforráshoz azonos előfizetés-azonosítónak kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-161">Both these resources must be under the same subscription ID.</span></span>

```json
"subscriptionId": {
    "type": "string",
    "metadata": {
        "description": "Subscription Id of both Azure Data Lake Store and Event Hub namespace"
     }
 }
```

###<a name="datalakeaccountname"></a><span data-ttu-id="e4c0a-162">dataLakeAccountName</span><span class="sxs-lookup"><span data-stu-id="e4c0a-162">dataLakeAccountName</span></span>

<span data-ttu-id="e4c0a-163">A rögzített események Azure Data Lake Store-neve.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-163">The Azure Data Lake Store name for the captured events.</span></span>

```json
"dataLakeAccountName": {
    "type": "string",
    "metadata": {
        "description": "Azure Data Lake Store name"
    }
}
```

###<a name="datalakefolderpath"></a><span data-ttu-id="e4c0a-164">dataLakeFolderPath</span><span class="sxs-lookup"><span data-stu-id="e4c0a-164">dataLakeFolderPath</span></span>

<span data-ttu-id="e4c0a-165">A rögzített események célmappájának elérési útja.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-165">The destination folder path for the captured events.</span></span>

```json
"dataLakeFolderPath": {
    "type": "string",
    "metadata": {
        "description": "Destination archive folder path"
    }
}
```

## <a name="resources-to-deploy-for-azure-storage-as-destination-to-captured-events"></a><span data-ttu-id="e4c0a-166">Az Azure Storage-ban történő eseményrögzítéshez üzembe helyezendő erőforrások</span><span class="sxs-lookup"><span data-stu-id="e4c0a-166">Resources to deploy for Azure Storage as destination to captured events</span></span>

<span data-ttu-id="e4c0a-167">Létrehoz egy **EventHubs** típusú névteret egy eseményközponttal, valamint engedélyezi az Azure Blob Storage-ba való rögzítést is.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-167">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture to Azure Blob Storage.</span></span>

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "CaptureDescription":{
                        "enabled":"[parameters('captureEnabled')]",
                        "encoding":"[parameters('captureEncodingFormat')]",
                        "intervalInSeconds":"[parameters('captureTime')]",
                        "sizeLimitInBytes":"[parameters('captureSize')]",
                        "destination":{
                            "name":"EventHubCapture.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }

               }

            }
         ]
      }
   ]
```

## <a name="resources-to-deploy-for-azure-data-lake-store-as-destination"></a><span data-ttu-id="e4c0a-168">Az Azure Data Lake Store célhelyként való használatához üzembe helyezendő erőforrások</span><span class="sxs-lookup"><span data-stu-id="e4c0a-168">Resources to deploy for Azure Data Lake Store as destination</span></span>

<span data-ttu-id="e4c0a-169">Létrehoz egy **EventHubs** típusú névteret egy eseményközponttal, valamint engedélyezi az Azure Data Lake Store-ba való rögzítést is.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-169">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture to Azure Data Lake Store.</span></span>

```json
 "resources": [
        {
            "apiVersion": "2015-08-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "2015-08-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('eventHubName')]",
                        "ArchiveDescription": {
                            "enabled": "true",
                            "encoding": "[parameters('archiveEncodingFormat')]",
                            "intervalInSeconds": "[parameters('archiveTime')]",
                            "sizeLimitInBytes": "[parameters('archiveSize')]",
                            "destination": {
                                "name": "EventHubArchive.AzureDataLake",
                                "properties": {
                                    "DataLakeSubscriptionId": "[parameters('subscriptionId')]",
                                    "DataLakeAccountName": "[parameters('dataLakeAccountName')]",
                                    "DataLakeFolderPath": "[parameters('dataLakeFolderPath')]",
                                    "ArchiveNameFormat": "[parameters('archiveNameFormat')]"
                                }
                            }
                        }
                    }
                }
            ]
        }
    ]
```

## <a name="commands-to-run-deployment"></a><span data-ttu-id="e4c0a-170">Az üzembe helyezést futtató parancsok</span><span class="sxs-lookup"><span data-stu-id="e4c0a-170">Commands to run deployment</span></span>

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="e4c0a-171">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e4c0a-171">PowerShell</span></span>

<span data-ttu-id="e4c0a-172">Helyezze üzembe a sablont az Azure Storage-ba történő Event Hubs-rögzítés engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="e4c0a-172">Deploy your template to enable Event Hubs Capture into Azure Storage:</span></span>
 
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json
```

<span data-ttu-id="e4c0a-173">Helyezze üzembe a sablont az Azure Data Lake Store-ba történő Event Hubs-rögzítés engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="e4c0a-173">Deploy your template to enable Event Hubs Capture into Azure Data Lake Store:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="e4c0a-174">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e4c0a-174">Azure CLI</span></span>

<span data-ttu-id="e4c0a-175">Az Azure Blob Storage kiválasztása célhelyként:</span><span class="sxs-lookup"><span data-stu-id="e4c0a-175">Choosing Azure Blob Storage as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json][]
```

<span data-ttu-id="e4c0a-176">Az Azure Data Lake Store kiválasztása célhelyként:</span><span class="sxs-lookup"><span data-stu-id="e4c0a-176">Choosing Azure Data Lake Store as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="e4c0a-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4c0a-177">Next steps</span></span>

<span data-ttu-id="e4c0a-178">Az Event Hubs Rögzítés funkcióját az [Azure Portal](https://portal.azure.com) segítségével is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-178">You can also configure Event Hubs Capture via the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e4c0a-179">További információért tekintse meg az [Enable Event Hubs Capture using the Azure portal](event-hubs-capture-enable-through-portal.md) (Az Event Hubs Rögzítés funkciójának engedélyezése az Azure Portalon) című témakört.</span><span class="sxs-lookup"><span data-stu-id="e4c0a-179">For more information, see [Enable Event Hubs Capture using the Azure portal](event-hubs-capture-enable-through-portal.md).</span></span>

<span data-ttu-id="e4c0a-180">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="e4c0a-180">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="e4c0a-181">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="e4c0a-181">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="e4c0a-182">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4c0a-182">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="e4c0a-183">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="e4c0a-183">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Azure Resources naming conventions]: https://azure.microsoft.com/documentation/articles/guidance-naming-conventions/
[Event hub and enable Capture to Storage template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture
[Event hub and enable Capture to Azure Data Lake Store template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture-for-adls