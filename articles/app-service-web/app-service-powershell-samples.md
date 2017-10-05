---
title: "Az Azure PowerShell minták – az App Service |} Microsoft Docs"
description: "Az Azure PowerShell minták – az App Service"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 3254fdd57cfcd170f22374c1e3b058e6081d8e8e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples"></a><span data-ttu-id="88aba-103">Az Azure PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="88aba-103">Azure PowerShell Samples</span></span>

<span data-ttu-id="88aba-104">A következő táblázat a bash parancsfájlok az Azure PowerShell használatával készített mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="88aba-104">The following table includes links to bash scripts built using the Azure PowerShell.</span></span>

| | |
|-|-|
|<span data-ttu-id="88aba-105">**Alkalmazás létrehozása**</span><span class="sxs-lookup"><span data-stu-id="88aba-105">**Create app**</span></span>||
| [<span data-ttu-id="88aba-106">Hozzon létre egy webalkalmazást telepítése a Githubból</span><span class="sxs-lookup"><span data-stu-id="88aba-106">Create a web app with deployment from GitHub</span></span>](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="88aba-107">Létrehoz egy Azure webalkalmazás, amely a kód lekéri a Githubról.</span><span class="sxs-lookup"><span data-stu-id="88aba-107">Creates an Azure web app which pulls code from GitHub.</span></span> |
| [<span data-ttu-id="88aba-108">Webalkalmazás létrehozása folyamatos üzembe helyezéssel a GitHubról</span><span class="sxs-lookup"><span data-stu-id="88aba-108">Create a web app with continuous deployment from GitHub</span></span>](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="88aba-109">Egy Azure webalkalmazás, amely folyamatosan telepíti a GitHub-kódot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="88aba-109">Creates an Azure web app which continuously deploys code from GitHub.</span></span> |
| [<span data-ttu-id="88aba-110">Hozzon létre egy webalkalmazást és FTP kód telepítése</span><span class="sxs-lookup"><span data-stu-id="88aba-110">Create a web app and deploy code with FTP</span></span>](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="88aba-111">Fájlokat hozza létre az Azure web app és feltöltése egy helyi könyvtárból FTP használatával.</span><span class="sxs-lookup"><span data-stu-id="88aba-111">Creates an Azure web app and upload files from a local directory using FTP.</span></span> |
| [<span data-ttu-id="88aba-112">Webalkalmazás létrehozása és kód üzembe helyezése helyi Git-tárházból</span><span class="sxs-lookup"><span data-stu-id="88aba-112">Create a web app and deploy code from a local Git repository</span></span>](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="88aba-113">Azure-webalkalmazás létrehozása, és konfigurálja a helyi Git-tárház kód leküldéses.</span><span class="sxs-lookup"><span data-stu-id="88aba-113">Creates an Azure web app and configures code push from a local Git repository.</span></span> |
| [<span data-ttu-id="88aba-114">Webalkalmazás létrehozása és kód üzembe helyezése átmeneti környezetbe</span><span class="sxs-lookup"><span data-stu-id="88aba-114">Create a web app and deploy code to a staging environment</span></span>](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="88aba-115">Azure-webalkalmazás egy üzembe helyezési pont kódmódosításokat átmeneti hoz létre.</span><span class="sxs-lookup"><span data-stu-id="88aba-115">Creates an Azure web app with a deployment slot for staging code changes.</span></span> |
|<span data-ttu-id="88aba-116">**Alkalmazás konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="88aba-116">**Configure app**</span></span>||
| [<span data-ttu-id="88aba-117">Egyéni tartomány leképezése egy webalkalmazásra</span><span class="sxs-lookup"><span data-stu-id="88aba-117">Map a custom domain to a web app</span></span>](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="88aba-118">Azure-webalkalmazás létrehozása és egy egyéni tartománynevet rendel.</span><span class="sxs-lookup"><span data-stu-id="88aba-118">Creates an Azure web app and maps a custom domain name to it.</span></span> |
| [<span data-ttu-id="88aba-119">Egyéni SSL-tanúsítvány kötése a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="88aba-119">Bind a custom SSL certificate to a web app</span></span>](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="88aba-120">Azure-webalkalmazás létrehozása, és az SSL-tanúsítvány egy egyéni tartománynév kötődik.</span><span class="sxs-lookup"><span data-stu-id="88aba-120">Creates an Azure web app and binds the SSL certificate of a custom domain name to it.</span></span> |
|<span data-ttu-id="88aba-121">**Skála alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="88aba-121">**Scale app**</span></span>||
| [<span data-ttu-id="88aba-122">Webalkalmazások manuális méretezése</span><span class="sxs-lookup"><span data-stu-id="88aba-122">Scale a web app manually</span></span>](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="88aba-123">Azure-webalkalmazás létrehozása és méretezi a 2 példányok között.</span><span class="sxs-lookup"><span data-stu-id="88aba-123">Creates an Azure web app and scales it across 2 instances.</span></span> |
| [<span data-ttu-id="88aba-124">Webalkalmazások globális méretezése magas rendelkezésre állású architektúrával</span><span class="sxs-lookup"><span data-stu-id="88aba-124">Scale a web app worldwide with a high-availability architecture</span></span>](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="88aba-125">Két Azure-webalkalmazásokban két különböző földrajzi régió hoz létre, és egy végpontot, használja az Azure Traffic Manager keresztül elérhetővé válnak.</span><span class="sxs-lookup"><span data-stu-id="88aba-125">Creates two Azure web apps in two different geographical regions and makes them available through a single endpoint using Azure Traffic Manager.</span></span> |
|<span data-ttu-id="88aba-126">**Alkalmazás kapcsolódni tudjon erőforrásokhoz**</span><span class="sxs-lookup"><span data-stu-id="88aba-126">**Connect app to resources**</span></span>||
| [<span data-ttu-id="88aba-127">Webes alkalmazás csatlakoztatása SQL-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="88aba-127">Connect a web app to a SQL Database</span></span>](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="88aba-128">Létrehoz egy Azure web app és az SQL-adatbázis, majd az adatbázis-kapcsolati karakterláncot ad hozzá az alkalmazás beállításaiban.</span><span class="sxs-lookup"><span data-stu-id="88aba-128">Creates an Azure web app and a SQL database, then adds the database connection string to the app settings.</span></span> |
| [<span data-ttu-id="88aba-129">Webalkalmazás csatlakoztatása tárfiókhoz</span><span class="sxs-lookup"><span data-stu-id="88aba-129">Connect a web app to a storage account</span></span>](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="88aba-130">Azure-webalkalmazás és a storage-fiók létrehozása, majd hozzáadja a tárolási kapcsolati karakterlánc az alkalmazás beállításaiban.</span><span class="sxs-lookup"><span data-stu-id="88aba-130">Creates an Azure web app and a storage account, then adds the storage connection string to the app settings.</span></span> |
|<span data-ttu-id="88aba-131">**Alkalmazás monitorozása**</span><span class="sxs-lookup"><span data-stu-id="88aba-131">**Monitor app**</span></span>||
| [<span data-ttu-id="88aba-132">Webalkalmazás figyelése a webkiszolgáló-naplókkal</span><span class="sxs-lookup"><span data-stu-id="88aba-132">Monitor a web app with web server logs</span></span>](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="88aba-133">Azure-webalkalmazás létrehozása, engedélyezi a naplózást, és a naplók letölti a helyi számítógépre.</span><span class="sxs-lookup"><span data-stu-id="88aba-133">Creates an Azure web app, enables logging for it, and downloads the logs to your local machine.</span></span> |
| | |
