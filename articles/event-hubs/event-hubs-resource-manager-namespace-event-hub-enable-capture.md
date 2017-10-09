---
title: "egy Azure Event Hubs névtér és az engedélyezés rögzítése a sablon használatával aaaCreate |} Microsoft Docs"
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
ms.openlocfilehash: a43b4e8d690ae825047e8a9d609bfda89cf2a06f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-with-an-event-hub-and-enable-capture-using-an-azure-resource-manager-template"></a><span data-ttu-id="79a22-103">Event Hubs-névtér létrehozása egy eseményközponttal és a Rögzítés funkció engedélyezése az Azure Resource Manager-sablonjának használatával</span><span class="sxs-lookup"><span data-stu-id="79a22-103">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>

<span data-ttu-id="79a22-104">Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely hoz létre egy Event Hubs névtér egy esemény hub-példány és is lehetővé teszi, hogy hello [rögzítése funkció](event-hubs-capture-overview.md) hello event hub.</span><span class="sxs-lookup"><span data-stu-id="79a22-104">This article shows how toouse an Azure Resource Manager template that creates an Event Hubs namespace, with one event hub instance, and also enables hello [Capture feature](event-hubs-capture-overview.md) on hello event hub.</span></span> <span data-ttu-id="79a22-105">hello cikkből megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="79a22-105">hello article describes how toodefine which resources are deployed, and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="79a22-106">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="79a22-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="79a22-107">Ez a következő cikket is bemutatja, hogyan az, hogy a események kerülnek rögzítésre Azure Storage Blobsba vagy egy Azure Data Lake Store toospecify hello alapján választja cél.</span><span class="sxs-lookup"><span data-stu-id="79a22-107">This article also shows how toospecify that events are captured into either Azure Storage Blobs or an Azure Data Lake Store, based on hello destination you choose.</span></span>

<span data-ttu-id="79a22-108">A sablonok létrehozásáról további információkat az [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates] (Azure Resource Manager-sablonok készítése) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="79a22-108">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="79a22-109">További információk az Azure-erőforrások elnevezési szabályainak mintáiról és gyakorlati megoldásairól: [Az Azure-erőforrások elnevezési szabályai][Azure Resources naming conventions].</span><span class="sxs-lookup"><span data-stu-id="79a22-109">For more information about patterns and practices for Azure Resources naming conventions, see [Azure Resources naming conventions][Azure Resources naming conventions].</span></span>

<span data-ttu-id="79a22-110">Hello teljes sablonok kattintson a GitHub-hivatkozásokat a következő hello:</span><span class="sxs-lookup"><span data-stu-id="79a22-110">For hello complete templates, click hello following GitHub links:</span></span>

- <span data-ttu-id="79a22-111">[Event hub és engedélyezése rögzítési tooStorage sablon][Event Hub and enable Capture tooStorage template]</span><span class="sxs-lookup"><span data-stu-id="79a22-111">[Event hub and enable Capture tooStorage template][Event Hub and enable Capture tooStorage template]</span></span> 
- <span data-ttu-id="79a22-112">[Event hub és engedélyezése rögzítési tooAzure Data Lake Store sablon][Event Hub and enable Capture tooAzure Data Lake Store template]</span><span class="sxs-lookup"><span data-stu-id="79a22-112">[Event hub and enable Capture tooAzure Data Lake Store template][Event Hub and enable Capture tooAzure Data Lake Store template]</span></span>

> [!NOTE]
> <span data-ttu-id="79a22-113">toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] -dokumentumtárban és keressen rá az Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="79a22-113">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="79a22-114">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="79a22-114">What will you deploy?</span></span>

<span data-ttu-id="79a22-115">Ezzel a sablonnal egy Event Hubs-névteret helyez üzembe eseményközponttal, továbbá engedélyezi az [az Event Hubs Rögzítés funkcióját](event-hubs-capture-overview.md).</span><span class="sxs-lookup"><span data-stu-id="79a22-115">With this template, you deploy an Event Hubs namespace with an event hub, and also enable [Event Hubs Capture](event-hubs-capture-overview.md).</span></span>

<span data-ttu-id="79a22-116">[Az Event Hubs](event-hubs-what-is-event-hubs.md) egy Eseményfeldolgozási szolgáltatás használt tooprovide esemény- és telemetriabevitelt érkező tooAzure nagy méretű, alacsony késéssel és nagy megbízhatósággal.</span><span class="sxs-lookup"><span data-stu-id="79a22-116">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used tooprovide event and telemetry ingress tooAzure at massive scale, with low latency and high reliability.</span></span> <span data-ttu-id="79a22-117">Esemény hubok rögzítése lehetővé teszi, hogy Ön tooautomatically hello adatfolyam-adatokat az Event Hubs tooAzure Blob storage szolgáltatásban vagy az Azure Data Lake Store, egy adott időszakra vagy mérete időköze a fájlmegosztásba.</span><span class="sxs-lookup"><span data-stu-id="79a22-117">Event Hubs Capture enables you tooautomatically deliver hello streaming data in Event Hubs tooAzure Blob storage or Azure Data Lake Store, within a specified time or size interval of your choosing.</span></span>

<span data-ttu-id="79a22-118">Kattintson a gombra tooenable Event Hubs rögzítheti az Azure Storage a következő hello:</span><span class="sxs-lookup"><span data-stu-id="79a22-118">Click hello following button tooenable Event Hubs Capture into Azure Storage:</span></span>

<span data-ttu-id="79a22-119">[![TooAzure telepítése](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="79a22-119">[![Deploy tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span></span>

<span data-ttu-id="79a22-120">Kattintson a következő gomb tooenable Event Hubs rögzítheti az Azure Data Lake Store hello:</span><span class="sxs-lookup"><span data-stu-id="79a22-120">Click hello following button tooenable Event Hubs Capture into Azure Data Lake Store:</span></span>

<span data-ttu-id="79a22-121">[![TooAzure telepítése](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="79a22-121">[![Deploy tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="79a22-122">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="79a22-122">Parameters</span></span>

<span data-ttu-id="79a22-123">Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="79a22-123">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="79a22-124">hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="79a22-124">hello template includes a section called `Parameters` that contains all hello parameter values.</span></span> <span data-ttu-id="79a22-125">Ezeket az értékeket, amelyek módosítják a hello projekt telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="79a22-125">You should define a parameter for those values that vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="79a22-126">Paraméterei nem adják meg, az értékek, amelyek mindig azonos hello.</span><span class="sxs-lookup"><span data-stu-id="79a22-126">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="79a22-127">Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="79a22-127">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="79a22-128">hello a sablon a következő paraméterek hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="79a22-128">hello template defines hello following parameters.</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="79a22-129">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="79a22-129">eventHubNamespaceName</span></span>

<span data-ttu-id="79a22-130">hello hello neve [Event Hubs névtér](event-hubs-create.md) toocreate.</span><span class="sxs-lookup"><span data-stu-id="79a22-130">hello name of hello [Event Hubs namespace](event-hubs-create.md) toocreate.</span></span>

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of hello EventHub namespace"
      }
}
```

### <a name="eventhubname"></a><span data-ttu-id="79a22-131">eventHubName</span><span class="sxs-lookup"><span data-stu-id="79a22-131">eventHubName</span></span>

<span data-ttu-id="79a22-132">hello event hubs hello létrehozott hello nevének [Event Hubs névtér](event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="79a22-132">hello name of hello event hub created in hello [Event Hubs namespace](event-hubs-create.md).</span></span>

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of hello event hub"
    }
}
```

### <a name="messageretentionindays"></a><span data-ttu-id="79a22-133">messageRetentionInDays</span><span class="sxs-lookup"><span data-stu-id="79a22-133">messageRetentionInDays</span></span>

<span data-ttu-id="79a22-134">nap tooretain köszönőüzenetei hello eseményközpont hello száma.</span><span class="sxs-lookup"><span data-stu-id="79a22-134">hello number of days tooretain hello messages in hello event hub.</span></span> 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long tooretain hello data in event hub"
     }
 }
```

### <a name="partitioncount"></a><span data-ttu-id="79a22-135">partitionCount</span><span class="sxs-lookup"><span data-stu-id="79a22-135">partitionCount</span></span>

<span data-ttu-id="79a22-136">az eseményközpont hello partíciók toocreate hello száma.</span><span class="sxs-lookup"><span data-stu-id="79a22-136">hello number of partitions toocreate in hello event hub.</span></span>

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

### <a name="captureenabled"></a><span data-ttu-id="79a22-137">captureEnabled</span><span class="sxs-lookup"><span data-stu-id="79a22-137">captureEnabled</span></span>

<span data-ttu-id="79a22-138">Engedélyezze a rögzítési a hello eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="79a22-138">Enable Capture on hello event hub.</span></span>

```json
"captureEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable hello Capture for your event hub"
    }
 }
```
### <a name="captureencodingformat"></a><span data-ttu-id="79a22-139">captureEncodingFormat</span><span class="sxs-lookup"><span data-stu-id="79a22-139">captureEncodingFormat</span></span>

<span data-ttu-id="79a22-140">hello kódolási formátum tooserialize hello esemény adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="79a22-140">hello encoding format you specify tooserialize hello event data.</span></span>

```json
"captureEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"hello encoding format in which Capture serializes hello EventData"
    }
}
```

### <a name="capturetime"></a><span data-ttu-id="79a22-141">captureTime</span><span class="sxs-lookup"><span data-stu-id="79a22-141">captureTime</span></span>

<span data-ttu-id="79a22-142">hello alatt az időtartam alatt, amelyben Event Hubs rögzítése kezdődik hello adatok rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="79a22-142">hello time interval in which Event Hubs Capture starts capturing hello data.</span></span>

```json
"captureTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"hello time window in seconds for hello capture"
    }
}
```

### <a name="capturesize"></a><span data-ttu-id="79a22-143">captureSize</span><span class="sxs-lookup"><span data-stu-id="79a22-143">captureSize</span></span>
<span data-ttu-id="79a22-144">hello mérete időköz, amit rögzítési kezdi hello adatok rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="79a22-144">hello size interval at which Capture starts capturing hello data.</span></span>

```json
"captureSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"hello size window in bytes for capture"
    }
}
```

###<a name="capturenameformat"></a><span data-ttu-id="79a22-145">captureNameFormat</span><span class="sxs-lookup"><span data-stu-id="79a22-145">captureNameFormat</span></span>

<span data-ttu-id="79a22-146">hello névformátum Event Hubs rögzítése toowrite hello az Avro-fájlok által használt.</span><span class="sxs-lookup"><span data-stu-id="79a22-146">hello name format used by Event Hubs Capture toowrite hello Avro files.</span></span> <span data-ttu-id="79a22-147">Vegye figyelembe, hogy a Capture névformátumának tartalmaznia kell a következő mezőket: `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}` és `{Second}`.</span><span class="sxs-lookup"><span data-stu-id="79a22-147">Note that a Capture name format must contain `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}`, and `{Second}` fields.</span></span> <span data-ttu-id="79a22-148">Ezek a mezők bármilyen sorrend szerint rendezhetők, elválasztó karakterekkel vagy azok nélkül.</span><span class="sxs-lookup"><span data-stu-id="79a22-148">These can be arranged in any order, with or without delimiters.</span></span>
 
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

### <a name="apiversion"></a><span data-ttu-id="79a22-149">apiVersion</span><span class="sxs-lookup"><span data-stu-id="79a22-149">apiVersion</span></span>

<span data-ttu-id="79a22-150">hello sablon hello API verzióját.</span><span class="sxs-lookup"><span data-stu-id="79a22-150">hello API version of hello template.</span></span>

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by hello template"
    }
 }
```

<span data-ttu-id="79a22-151">A következő paraméterek, ha úgy dönt, Azure Storage a célként hello használja.</span><span class="sxs-lookup"><span data-stu-id="79a22-151">Use hello following parameters if you choose Azure Storage as your destination.</span></span>

### <a name="destinationstorageaccountresourceid"></a><span data-ttu-id="79a22-152">destinationStorageAccountResourceId</span><span class="sxs-lookup"><span data-stu-id="79a22-152">destinationStorageAccountResourceId</span></span>

<span data-ttu-id="79a22-153">Rögzítési van szükség, egy Azure Storage fiók erőforrás azonosítója tooenable tooyour rögzítése Storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="79a22-153">Capture requires an Azure Storage account resource ID tooenable capturing tooyour desired Storage account.</span></span>

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing Storage account resource ID where you want hello blobs be captured"
    }
 }
```

### <a name="blobcontainername"></a><span data-ttu-id="79a22-154">blobContainerName</span><span class="sxs-lookup"><span data-stu-id="79a22-154">blobContainerName</span></span>

<span data-ttu-id="79a22-155">hello mely toocapture a blob-tároló az eseményadatok.</span><span class="sxs-lookup"><span data-stu-id="79a22-155">hello blob container in which toocapture your event data.</span></span>

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container in which you want hello blobs captured"
    }
}
```

<span data-ttu-id="79a22-156">Ha úgy dönt, az Azure Data Lake Store a célként a következő paraméterek hello használja.</span><span class="sxs-lookup"><span data-stu-id="79a22-156">Use hello following parameters if you choose Azure Data Lake Store as your destination.</span></span> <span data-ttu-id="79a22-157">A Data Lake Store elérési úton, tooCapture hello esemény használni kívánt engedélyeket kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="79a22-157">You must set permissions on your Data Lake Store path, in which you want tooCapture hello event.</span></span> <span data-ttu-id="79a22-158">tooset engedélyek, lásd: [Ez a cikk](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span><span class="sxs-lookup"><span data-stu-id="79a22-158">tooset permissions, see [this article](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span></span>

###<a name="subscriptionid"></a><span data-ttu-id="79a22-159">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="79a22-159">subscriptionId</span></span>

<span data-ttu-id="79a22-160">Hello Event Hubs névtér és az Azure Data Lake Store az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="79a22-160">Subscription ID for hello Event Hubs namespace and Azure Data Lake Store.</span></span> <span data-ttu-id="79a22-161">Mindkét ezeket az erőforrásokat hello alatt kell lennie ugyanazon előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="79a22-161">Both these resources must be under hello same subscription ID.</span></span>

```json
"subscriptionId": {
    "type": "string",
    "metadata": {
        "description": "Subscription Id of both Azure Data Lake Store and Event Hub namespace"
     }
 }
```

###<a name="datalakeaccountname"></a><span data-ttu-id="79a22-162">dataLakeAccountName</span><span class="sxs-lookup"><span data-stu-id="79a22-162">dataLakeAccountName</span></span>

<span data-ttu-id="79a22-163">hello Azure Data Lake Store nevét hello eseményeket rögzíteni.</span><span class="sxs-lookup"><span data-stu-id="79a22-163">hello Azure Data Lake Store name for hello captured events.</span></span>

```json
"dataLakeAccountName": {
    "type": "string",
    "metadata": {
        "description": "Azure Data Lake Store name"
    }
}
```

###<a name="datalakefolderpath"></a><span data-ttu-id="79a22-164">dataLakeFolderPath</span><span class="sxs-lookup"><span data-stu-id="79a22-164">dataLakeFolderPath</span></span>

<span data-ttu-id="79a22-165">hello célmappa elérési útját a hello eseményeket rögzíteni.</span><span class="sxs-lookup"><span data-stu-id="79a22-165">hello destination folder path for hello captured events.</span></span>

```json
"dataLakeFolderPath": {
    "type": "string",
    "metadata": {
        "description": "Destination archive folder path"
    }
}
```

## <a name="resources-toodeploy-for-azure-storage-as-destination-toocaptured-events"></a><span data-ttu-id="79a22-166">Cél toocaptured eseményként is rögzíti az Azure Storage erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="79a22-166">Resources toodeploy for Azure Storage as destination toocaptured events</span></span>

<span data-ttu-id="79a22-167">Létrehoz egy névtér, típus **EventHubs**egy eseményközpontot, valamint lehetővé teszi, hogy a Blob Storage tooAzure rögzítése.</span><span class="sxs-lookup"><span data-stu-id="79a22-167">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture tooAzure Blob Storage.</span></span>

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

## <a name="resources-toodeploy-for-azure-data-lake-store-as-destination"></a><span data-ttu-id="79a22-168">Az Azure Data Lake Store jelölje ki célként erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="79a22-168">Resources toodeploy for Azure Data Lake Store as destination</span></span>

<span data-ttu-id="79a22-169">Létrehoz egy névtér, típus **EventHubs**, egy eseményközpontot, és is lehetővé teszi, hogy rögzítési tooAzure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="79a22-169">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture tooAzure Data Lake Store.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="79a22-170">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="79a22-170">Commands toorun deployment</span></span>

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="79a22-171">PowerShell</span><span class="sxs-lookup"><span data-stu-id="79a22-171">PowerShell</span></span>

<span data-ttu-id="79a22-172">Az Event Hubs rögzítése sablon tooenable üzembe helyezés Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="79a22-172">Deploy your template tooenable Event Hubs Capture into Azure Storage:</span></span>
 
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json
```

<span data-ttu-id="79a22-173">Az Event Hubs rögzítése sablon tooenable üzembe helyezés az Azure Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="79a22-173">Deploy your template tooenable Event Hubs Capture into Azure Data Lake Store:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="79a22-174">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="79a22-174">Azure CLI</span></span>

<span data-ttu-id="79a22-175">Az Azure Blob Storage kiválasztása célhelyként:</span><span class="sxs-lookup"><span data-stu-id="79a22-175">Choosing Azure Blob Storage as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json][]
```

<span data-ttu-id="79a22-176">Az Azure Data Lake Store kiválasztása célhelyként:</span><span class="sxs-lookup"><span data-stu-id="79a22-176">Choosing Azure Data Lake Store as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="79a22-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="79a22-177">Next steps</span></span>

<span data-ttu-id="79a22-178">Beállíthatja úgy is Event Hubs rögzítése keresztül hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79a22-178">You can also configure Event Hubs Capture via hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="79a22-179">További információkért lásd: [engedélyezése Event Hubs rögzítése segítségével hello Azure-portálon](event-hubs-capture-enable-through-portal.md).</span><span class="sxs-lookup"><span data-stu-id="79a22-179">For more information, see [Enable Event Hubs Capture using hello Azure portal](event-hubs-capture-enable-through-portal.md).</span></span>

<span data-ttu-id="79a22-180">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="79a22-180">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="79a22-181">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="79a22-181">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="79a22-182">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="79a22-182">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="79a22-183">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="79a22-183">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Azure Resources naming conventions]: https://azure.microsoft.com/documentation/articles/guidance-naming-conventions/
[Event hub and enable Capture tooStorage template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture
[Event hub and enable Capture tooAzure Data Lake Store template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture-for-adls
