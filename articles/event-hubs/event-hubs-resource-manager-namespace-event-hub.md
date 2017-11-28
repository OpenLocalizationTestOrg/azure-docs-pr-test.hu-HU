---
title: "Azure Event Hubs névtér és felhasználó-csoport létrehozása sablon alapján |} Microsoft Docs"
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
ms.openlocfilehash: eb9a80eec0326aaa605cb8b21aecbaeec94ff212
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a><span data-ttu-id="bd76d-103">Az Event Hubs-névtér létrehozása Azure Resource Manager-sablonnal esemény hub és felhasználói csoport</span><span class="sxs-lookup"><span data-stu-id="bd76d-103">Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template</span></span>

<span data-ttu-id="bd76d-104">Ez a cikk bemutatja, hogyan használható az Azure Resource Manager sablon, amely létrehoz egy névtér, az Event Hubs egy eseményközpontba való típusú és egy fogyasztói csoporton.</span><span class="sxs-lookup"><span data-stu-id="bd76d-104">This article shows how to use an Azure Resource Manager template that creates a namespace of type Event Hubs, with one event hub and one consumer group.</span></span> <span data-ttu-id="bd76d-105">A cikk bemutatja, hogyan határozza meg, mely erőforrásokat központilag telepíti, és hogyan adhat meg a paramétereket, amelyek megadott, amikor a központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="bd76d-105">The article shows how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="bd76d-106">Ezt a sablont saját üzembe helyezési műveleteihez is használhatja, vagy akár igényeinek megfelelően testre is szabhatja</span><span class="sxs-lookup"><span data-stu-id="bd76d-106">You can use this template for your own deployments, or customize it to meet your requirements</span></span>

<span data-ttu-id="bd76d-107">A sablonok létrehozásáról további információkat az [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates] (Azure Resource Manager-sablonok készítése) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="bd76d-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="bd76d-108">A teljes sablon, tekintse meg a [hub és felhasználói csoport eseménysablon] [ Event Hub and consumer group template] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="bd76d-108">For the complete template, see the [Event hub and consumer group template][Event Hub and consumer group template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="bd76d-109">A legújabb sablonokért keresse fel az [Azure-gyorssablonok][Azure Quickstart Templates] gyűjteményt, és keressen az Event Hubs kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="bd76d-109">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="bd76d-110">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="bd76d-110">What will you deploy?</span></span>
<span data-ttu-id="bd76d-111">Ezzel a sablonnal egy Event Hubs névtér eseményközpontban, és egy felhasználói csoport fog üzembe helyezni.</span><span class="sxs-lookup"><span data-stu-id="bd76d-111">With this template, you will deploy an Event Hubs namespace with an event hub and a consumer group.</span></span>

<span data-ttu-id="bd76d-112">Az [Event Hubs](event-hubs-what-is-event-hubs.md) egy eseményfeldolgozási szolgáltatás, amely az Azure-ba irányuló, nagy léptékű esemény- és telemetriabevitelt biztosít alacsony késéssel és nagy megbízhatósággal.</span><span class="sxs-lookup"><span data-stu-id="bd76d-112">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used to provide event and telemetry ingress to Azure at massive scale, with low latency and high reliability.</span></span>

<span data-ttu-id="bd76d-113">Az automatikus üzembe helyezéshez kattintson az alábbi gombra:</span><span class="sxs-lookup"><span data-stu-id="bd76d-113">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="bd76d-114">[![Üzembe helyezés az Azure-ban](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="bd76d-114">[![Deploy to Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="bd76d-115">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bd76d-115">Parameters</span></span>
<span data-ttu-id="bd76d-116">Az Azure Resource Managerrel meghatározhatja a sablon üzembe helyezésekor megadandó értékek paramétereit.</span><span class="sxs-lookup"><span data-stu-id="bd76d-116">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="bd76d-117">A sablonban található egy `Parameters` nevű rész, amely magába foglalja az összes paraméterértéket.</span><span class="sxs-lookup"><span data-stu-id="bd76d-117">The template includes a section called `Parameters` that contains all the parameter values.</span></span> <span data-ttu-id="bd76d-118">Ezeket az értékeket, amelyek változhatnak, a projekt telepít vagy a környezet, amely telepíti alapján egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="bd76d-118">You should define a parameter for those values that will vary, based on the project you are deploying or based on the environment to which you are deploying.</span></span> <span data-ttu-id="bd76d-119">Ne adjon meg olyan paramétereket olyan értékhez, amelyek nem változnak.</span><span class="sxs-lookup"><span data-stu-id="bd76d-119">Do not define parameters for values that always stay the same.</span></span> <span data-ttu-id="bd76d-120">A sablon minden paraméter értéke határozza meg a telepített erőforrások esetén.</span><span class="sxs-lookup"><span data-stu-id="bd76d-120">Each parameter value in the template defines the resources that are deployed.</span></span>

<span data-ttu-id="bd76d-121">A sablon meghatározza a következő:</span><span class="sxs-lookup"><span data-stu-id="bd76d-121">The template defines the following parameters:</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="bd76d-122">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="bd76d-122">eventHubNamespaceName</span></span>
<span data-ttu-id="bd76d-123">A létrehozandó Event Hubs-névtér neve.</span><span class="sxs-lookup"><span data-stu-id="bd76d-123">The name of the Event Hubs namespace to create.</span></span>

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a><span data-ttu-id="bd76d-124">eventHubName</span><span class="sxs-lookup"><span data-stu-id="bd76d-124">eventHubName</span></span>
<span data-ttu-id="bd76d-125">Az Event Hubs-névtérben létrehozott eseményközpont neve.</span><span class="sxs-lookup"><span data-stu-id="bd76d-125">The name of the event hub created in the Event Hubs namespace.</span></span>

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a><span data-ttu-id="bd76d-126">eventHubConsumerGroupName</span><span class="sxs-lookup"><span data-stu-id="bd76d-126">eventHubConsumerGroupName</span></span>
<span data-ttu-id="bd76d-127">A fogyasztói csoportot az eseményközpont létre neve.</span><span class="sxs-lookup"><span data-stu-id="bd76d-127">The name of the consumer group created for the event hub.</span></span>

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a><span data-ttu-id="bd76d-128">apiVersion</span><span class="sxs-lookup"><span data-stu-id="bd76d-128">apiVersion</span></span>
<span data-ttu-id="bd76d-129">A sablon API-verziója.</span><span class="sxs-lookup"><span data-stu-id="bd76d-129">The API version of the template.</span></span>

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a><span data-ttu-id="bd76d-130">Üzembe helyezendő erőforrások</span><span class="sxs-lookup"><span data-stu-id="bd76d-130">Resources to deploy</span></span>
<span data-ttu-id="bd76d-131">Létrehoz egy névtér, típus **EventHubs**, az event hubs és a fogyasztói csoportot.</span><span class="sxs-lookup"><span data-stu-id="bd76d-131">Creates a namespace of type **EventHubs**, with an event hub and a consumer group.</span></span>

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

## <a name="commands-to-run-deployment"></a><span data-ttu-id="bd76d-132">Az üzembe helyezést futtató parancsok</span><span class="sxs-lookup"><span data-stu-id="bd76d-132">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="bd76d-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd76d-133">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="bd76d-134">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bd76d-134">Azure CLI</span></span>
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="bd76d-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bd76d-135">Next steps</span></span>
<span data-ttu-id="bd76d-136">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="bd76d-136">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="bd76d-137">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="bd76d-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="bd76d-138">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd76d-138">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="bd76d-139">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="bd76d-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
