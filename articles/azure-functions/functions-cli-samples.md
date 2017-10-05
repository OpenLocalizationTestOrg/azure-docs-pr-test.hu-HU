---
title: "Az Azure CLI minták – az Azure Functions |} Microsoft Docs"
description: "Az Azure CLI minták – az Azure Functions"
services: functions
documentationcenter: functions
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 577d2f13-de4d-40d2-9dfc-86ecc79f3ab0
ms.service: functions
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: functions
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 6830ab9ada50da99871592e2c0911ac9cb8f5797
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-cli-samples"></a><span data-ttu-id="b14f0-103">Az Azure CLI-minták</span><span class="sxs-lookup"><span data-stu-id="b14f0-103">Azure CLI Samples</span></span>

<span data-ttu-id="b14f0-104">A következő táblázat a bash parancsfájlok az Azure Functions az Azure CLI-t használó mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b14f0-104">The following table includes links to bash scripts for Azure Functions that use the Azure CLI.</span></span>

| | |
|-|-|
|<span data-ttu-id="b14f0-105">**Alkalmazás létrehozása**</span><span class="sxs-lookup"><span data-stu-id="b14f0-105">**Create app**</span></span>||
| [<span data-ttu-id="b14f0-106">A kiszolgáló nélküli végrehajtáshoz függvény-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b14f0-106">Create a function app for serverless execution</span></span>](scripts/functions-cli-create-serverless.md) | <span data-ttu-id="b14f0-107">A fogyasztás terv függvény alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b14f0-107">Creates an function app in a Consumption plan.</span></span>  |
| [<span data-ttu-id="b14f0-108">Egy függvény-alkalmazás létrehozása az App Service-csomagot</span><span class="sxs-lookup"><span data-stu-id="b14f0-108">Create a function app in an App Service plan</span></span>](scripts/functions-cli-create-app-service-plan.md) | <span data-ttu-id="b14f0-109">Függvény-alkalmazás létrehozása a kijelölt App Service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="b14f0-109">Create an function app in an dedicated App Service plan.</span></span> |
| | |
|<span data-ttu-id="b14f0-110">**Integrálása**</span><span class="sxs-lookup"><span data-stu-id="b14f0-110">**Integrate**</span></span>||
| [<span data-ttu-id="b14f0-111">Hozzon létre egy függvény alkalmazást, és csatlakozzon a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="b14f0-111">Create a function app and connect to a storage account</span></span>](scripts/functions-cli-create-function-app-connect-to-storage-account.md) | <span data-ttu-id="b14f0-112">Hozzon létre egy függvény alkalmazást, és csatlakoztassa a storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="b14f0-112">Create a function app and connect it to a storage account.</span></span> |
| [<span data-ttu-id="b14f0-113">Hozzon létre egy függvény alkalmazást, és csatlakozzon az Azure Cosmos Adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b14f0-113">Create a function app and connect to an Azure Cosmos DB</span></span>](scripts/functions-cli-create-function-app-connect-to-cosmos-db.md) | <span data-ttu-id="b14f0-114">Hozzon létre egy függvény alkalmazást, és csatlakoztassa egy Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b14f0-114">Create a function app and connect it to an Azure Cosmos DB</span></span> |
| | |
|<span data-ttu-id="b14f0-115">**Alkalmazás konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="b14f0-115">**Configure app**</span></span>||
| [<span data-ttu-id="b14f0-116">Egyéni tartomány leképezése egy függvény alkalmazást</span><span class="sxs-lookup"><span data-stu-id="b14f0-116">Map a custom domain to a function app</span></span>](scripts/functions-cli-configure-custom-domain.md) | <span data-ttu-id="b14f0-117">Adja meg a funkciók egy egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="b14f0-117">Define a custom domain for your functions.</span></span>  |
| [<span data-ttu-id="b14f0-118">Egy SSL-tanúsítvány kötését függvény alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="b14f0-118">Bind an SSL certificate to a function app</span></span>](scripts/functions-cli-configure-ssl-certificate.md)  |  <span data-ttu-id="b14f0-119">A funkciók egy egyéni tartomány SSL-tanúsítványok feltöltése.</span><span class="sxs-lookup"><span data-stu-id="b14f0-119">Upload SSL certificates for functions in a custom domain.</span></span> |
<!--

|**Scale app**||

|**Connect app to resources**||
-->
