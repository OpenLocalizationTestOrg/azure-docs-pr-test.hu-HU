---
title: "App Service - PowerShell-példák aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: b7b4a030364f797195522c56fbae5b7f530d4d1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples"></a><span data-ttu-id="4891a-103">Az Azure PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="4891a-103">Azure PowerShell Samples</span></span>

<span data-ttu-id="4891a-104">hello alábbi táblázat tartalmaz hivatkozásokat toobash parancsfájlokat hello Azure PowerShell használatával készített.</span><span class="sxs-lookup"><span data-stu-id="4891a-104">hello following table includes links toobash scripts built using hello Azure PowerShell.</span></span>

| | |
|-|-|
|<span data-ttu-id="4891a-105">**Alkalmazás létrehozása**</span><span class="sxs-lookup"><span data-stu-id="4891a-105">**Create app**</span></span>||
| [<span data-ttu-id="4891a-106">Hozzon létre egy webalkalmazást telepítése a Githubból</span><span class="sxs-lookup"><span data-stu-id="4891a-106">Create a web app with deployment from GitHub</span></span>](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="4891a-107">Létrehoz egy Azure webalkalmazás, amely a kód lekéri a Githubról.</span><span class="sxs-lookup"><span data-stu-id="4891a-107">Creates an Azure web app which pulls code from GitHub.</span></span> |
| [<span data-ttu-id="4891a-108">Webalkalmazás létrehozása folyamatos üzembe helyezéssel a GitHubról</span><span class="sxs-lookup"><span data-stu-id="4891a-108">Create a web app with continuous deployment from GitHub</span></span>](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="4891a-109">Egy Azure webalkalmazás, amely folyamatosan telepíti a GitHub-kódot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4891a-109">Creates an Azure web app which continuously deploys code from GitHub.</span></span> |
| [<span data-ttu-id="4891a-110">Hozzon létre egy webalkalmazást és FTP kód telepítése</span><span class="sxs-lookup"><span data-stu-id="4891a-110">Create a web app and deploy code with FTP</span></span>](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="4891a-111">Fájlokat hozza létre az Azure web app és feltöltése egy helyi könyvtárból FTP használatával.</span><span class="sxs-lookup"><span data-stu-id="4891a-111">Creates an Azure web app and upload files from a local directory using FTP.</span></span> |
| [<span data-ttu-id="4891a-112">Webalkalmazás létrehozása és kód üzembe helyezése helyi Git-tárházból</span><span class="sxs-lookup"><span data-stu-id="4891a-112">Create a web app and deploy code from a local Git repository</span></span>](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="4891a-113">Azure-webalkalmazás létrehozása, és konfigurálja a helyi Git-tárház kód leküldéses.</span><span class="sxs-lookup"><span data-stu-id="4891a-113">Creates an Azure web app and configures code push from a local Git repository.</span></span> |
| [<span data-ttu-id="4891a-114">Webalkalmazás létrehozása és központi telepítése átmeneti környezet kód tooa</span><span class="sxs-lookup"><span data-stu-id="4891a-114">Create a web app and deploy code tooa staging environment</span></span>](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="4891a-115">Azure-webalkalmazás egy üzembe helyezési pont kódmódosításokat átmeneti hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4891a-115">Creates an Azure web app with a deployment slot for staging code changes.</span></span> |
|<span data-ttu-id="4891a-116">**Alkalmazás konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="4891a-116">**Configure app**</span></span>||
| [<span data-ttu-id="4891a-117">Az egyéni tartomány tooa webalkalmazás leképezése</span><span class="sxs-lookup"><span data-stu-id="4891a-117">Map a custom domain tooa web app</span></span>](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="4891a-118">Azure-webalkalmazás létrehozása, és egy egyéni tartomány nevét tooit társít.</span><span class="sxs-lookup"><span data-stu-id="4891a-118">Creates an Azure web app and maps a custom domain name tooit.</span></span> |
| [<span data-ttu-id="4891a-119">Egy egyéni SSL tanúsítvány tooa webalkalmazás kötése</span><span class="sxs-lookup"><span data-stu-id="4891a-119">Bind a custom SSL certificate tooa web app</span></span>](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="4891a-120">Azure-webalkalmazás létrehozása és egy egyéni tartomány nevét tooit hello SSL-tanúsítvány van kötve.</span><span class="sxs-lookup"><span data-stu-id="4891a-120">Creates an Azure web app and binds hello SSL certificate of a custom domain name tooit.</span></span> |
|<span data-ttu-id="4891a-121">**Skála alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="4891a-121">**Scale app**</span></span>||
| [<span data-ttu-id="4891a-122">Webalkalmazások manuális méretezése</span><span class="sxs-lookup"><span data-stu-id="4891a-122">Scale a web app manually</span></span>](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="4891a-123">Azure-webalkalmazás létrehozása és méretezi a 2 példányok között.</span><span class="sxs-lookup"><span data-stu-id="4891a-123">Creates an Azure web app and scales it across 2 instances.</span></span> |
| [<span data-ttu-id="4891a-124">Webalkalmazások globális méretezése magas rendelkezésre állású architektúrával</span><span class="sxs-lookup"><span data-stu-id="4891a-124">Scale a web app worldwide with a high-availability architecture</span></span>](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="4891a-125">Két Azure-webalkalmazásokban két különböző földrajzi régió hoz létre, és egy végpontot, használja az Azure Traffic Manager keresztül elérhetővé válnak.</span><span class="sxs-lookup"><span data-stu-id="4891a-125">Creates two Azure web apps in two different geographical regions and makes them available through a single endpoint using Azure Traffic Manager.</span></span> |
|<span data-ttu-id="4891a-126">**Kapcsolódás app tooresources**</span><span class="sxs-lookup"><span data-stu-id="4891a-126">**Connect app tooresources**</span></span>||
| [<span data-ttu-id="4891a-127">Csatlakozás egy webes alkalmazás tooa SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="4891a-127">Connect a web app tooa SQL Database</span></span>](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="4891a-128">Létrehoz egy Azure web app és az SQL-adatbázis, majd hozzáadja a hello adatbázis kapcsolati karakterlánc toohello Alkalmazásbeállítások.</span><span class="sxs-lookup"><span data-stu-id="4891a-128">Creates an Azure web app and a SQL database, then adds hello database connection string toohello app settings.</span></span> |
| [<span data-ttu-id="4891a-129">Csatlakozás a webes alkalmazás tooa storage-fiók</span><span class="sxs-lookup"><span data-stu-id="4891a-129">Connect a web app tooa storage account</span></span>](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="4891a-130">Azure-webalkalmazás és a storage-fiók létrehozása, majd hozzáadja a hello tárolási kapcsolati karakterlánc toohello Alkalmazásbeállítások.</span><span class="sxs-lookup"><span data-stu-id="4891a-130">Creates an Azure web app and a storage account, then adds hello storage connection string toohello app settings.</span></span> |
|<span data-ttu-id="4891a-131">**Alkalmazás monitorozása**</span><span class="sxs-lookup"><span data-stu-id="4891a-131">**Monitor app**</span></span>||
| [<span data-ttu-id="4891a-132">Webalkalmazás figyelése a webkiszolgáló-naplókkal</span><span class="sxs-lookup"><span data-stu-id="4891a-132">Monitor a web app with web server logs</span></span>](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="4891a-133">Azure-webalkalmazás létrehozása, engedélyezi a naplózást, és letölti hello naplók tooyour helyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="4891a-133">Creates an Azure web app, enables logging for it, and downloads hello logs tooyour local machine.</span></span> |
| | |
