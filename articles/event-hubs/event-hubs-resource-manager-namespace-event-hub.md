---
title: "az Azure Event Hubs-névteret és a fogyasztói csoportot sablon aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy Event Hubs-névtér az eseményközpontok és egy felhasználói csoport Azure Resource Manager-sablonok használatával"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/12/2017
ms.author: sethm;shvija
ms.openlocfilehash: 74b0d6b3fbe848705e2c20e628aa4e5269b53edb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a><span data-ttu-id="0ec0e-103">Az Event Hubs-névtér létrehozása Azure Resource Manager-sablonnal esemény hub és felhasználói csoport</span><span class="sxs-lookup"><span data-stu-id="0ec0e-103">Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template</span></span>

<span data-ttu-id="0ec0e-104">Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely hoz létre egy névtér, típus az Event Hubs egy eseményközpontba és egy fogyasztói csoporton.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-104">This article shows how toouse an Azure Resource Manager template that creates a namespace of type Event Hubs, with one event hub and one consumer group.</span></span> <span data-ttu-id="0ec0e-105">hello hogyan cikkre mutat be toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-105">hello article shows how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="0ec0e-106">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet a követelmények</span><span class="sxs-lookup"><span data-stu-id="0ec0e-106">You can use this template for your own deployments, or customize it toomeet your requirements</span></span>

<span data-ttu-id="0ec0e-107">A sablonok létrehozásáról további információkat az [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates] (Azure Resource Manager-sablonok készítése) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="0ec0e-108">Hello teljes sablon, lásd: hello [hub és felhasználói csoport eseménysablon] [ Event Hub and consumer group template] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-108">For hello complete template, see hello [Event hub and consumer group template][Event Hub and consumer group template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="0ec0e-109">toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] -dokumentumtárban és keressen rá az Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-109">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="0ec0e-110">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="0ec0e-110">What will you deploy?</span></span>
<span data-ttu-id="0ec0e-111">Ezzel a sablonnal egy Event Hubs névtér eseményközpontban, és egy felhasználói csoport fog üzembe helyezni.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-111">With this template, you will deploy an Event Hubs namespace with an event hub and a consumer group.</span></span>

<span data-ttu-id="0ec0e-112">[Az Event Hubs](event-hubs-what-is-event-hubs.md) egy Eseményfeldolgozási szolgáltatás használt tooprovide esemény- és telemetriabevitelt érkező tooAzure nagy méretű, alacsony késéssel és nagy megbízhatósággal.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-112">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used tooprovide event and telemetry ingress tooAzure at massive scale, with low latency and high reliability.</span></span>

<span data-ttu-id="0ec0e-113">toorun telepítési hello automatikusan, kattintson a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="0ec0e-113">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="0ec0e-114">[![TooAzure telepítése](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="0ec0e-114">[![Deploy tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="0ec0e-115">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="0ec0e-115">Parameters</span></span>
<span data-ttu-id="0ec0e-116">Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-116">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="0ec0e-117">hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-117">hello template includes a section called `Parameters` that contains all hello parameter values.</span></span> <span data-ttu-id="0ec0e-118">Ezeket az értékeket, amelyek változhatnak, hello projekt telepít vagy telepít hello környezet toowhich alapján egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-118">You should define a parameter for those values that will vary, based on hello project you are deploying or based on hello environment toowhich you are deploying.</span></span> <span data-ttu-id="0ec0e-119">Paraméterei nem adják meg, az értékek, amelyek mindig azonos hello.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-119">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="0ec0e-120">Minden paraméter értéke, hello sablonban hello telepített erőforrások esetén határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-120">Each parameter value in hello template defines hello resources that are deployed.</span></span>

<span data-ttu-id="0ec0e-121">hello sablon meghatározza, hogy a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="0ec0e-121">hello template defines hello following parameters:</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="0ec0e-122">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="0ec0e-122">eventHubNamespaceName</span></span>
<span data-ttu-id="0ec0e-123">hello Event Hubs névtér toocreate hello neve.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-123">hello name of hello Event Hubs namespace toocreate.</span></span>

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a><span data-ttu-id="0ec0e-124">eventHubName</span><span class="sxs-lookup"><span data-stu-id="0ec0e-124">eventHubName</span></span>
<span data-ttu-id="0ec0e-125">hello hello eseményközpont létre hello Event Hubs névtér neve.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-125">hello name of hello event hub created in hello Event Hubs namespace.</span></span>

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a><span data-ttu-id="0ec0e-126">eventHubConsumerGroupName</span><span class="sxs-lookup"><span data-stu-id="0ec0e-126">eventHubConsumerGroupName</span></span>
<span data-ttu-id="0ec0e-127">hello fogyasztói csoportot hello eseményközpont létre hello neve.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-127">hello name of hello consumer group created for hello event hub.</span></span>

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a><span data-ttu-id="0ec0e-128">apiVersion</span><span class="sxs-lookup"><span data-stu-id="0ec0e-128">apiVersion</span></span>
<span data-ttu-id="0ec0e-129">hello sablon hello API verzióját.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-129">hello API version of hello template.</span></span>

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a><span data-ttu-id="0ec0e-130">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="0ec0e-130">Resources toodeploy</span></span>
<span data-ttu-id="0ec0e-131">Létrehoz egy névtér, típus **EventHubs**, az event hubs és a fogyasztói csoportot.</span><span class="sxs-lookup"><span data-stu-id="0ec0e-131">Creates a namespace of type **EventHubs**, with an event hub and a consumer group.</span></span>

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
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
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-toorun-deployment"></a><span data-ttu-id="0ec0e-132">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="0ec0e-132">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="0ec0e-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0ec0e-133">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="0ec0e-134">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0ec0e-134">Azure CLI</span></span>
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="0ec0e-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0ec0e-135">Next steps</span></span>
<span data-ttu-id="0ec0e-136">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="0ec0e-136">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="0ec0e-137">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="0ec0e-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="0ec0e-138">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="0ec0e-138">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="0ec0e-139">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="0ec0e-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
