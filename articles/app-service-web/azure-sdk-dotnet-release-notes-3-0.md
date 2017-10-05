---
title: "Az Azure SDK for .NET 3.0-s kibocsátási megjegyzései |} Microsoft Docs"
description: "Az Azure SDK for .NET 3.0 kibocsátási megjegyzések"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/07/2017
ms.author: juliako
ms.openlocfilehash: eea4e569ac2d0192ed7872d2fcb9bed03614832b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a><span data-ttu-id="8fb90-103">Az Azure SDK for .NET 3.0 kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="8fb90-103">Azure SDK for .NET 3.0 release notes</span></span>

<span data-ttu-id="8fb90-104">Ez a témakör fontos tudnivalók az Azure SDK 3.0-s verziójának a .NET-keretrendszerhez készült.</span><span class="sxs-lookup"><span data-stu-id="8fb90-104">This topic includes release notes for version 3.0 of the Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-30-release-summary"></a><span data-ttu-id="8fb90-105">Az Azure SDK .NET 3.0 kiadás összefoglaló</span><span class="sxs-lookup"><span data-stu-id="8fb90-105">Azure SDK for .NET 3.0 release summary</span></span>

<span data-ttu-id="8fb90-106">Kiadás dátuma: 03/07/2017</span><span class="sxs-lookup"><span data-stu-id="8fb90-106">Release date: 03/07/2017</span></span>
 
<span data-ttu-id="8fb90-107">Ebben a kiadásban nincs jelentős változásokat az Azure SDK 3.0 vezettek be.</span><span class="sxs-lookup"><span data-stu-id="8fb90-107">No breaking changes to the Azure SDK 3.0 have been introduced in this release.</span></span> <span data-ttu-id="8fb90-108">Nem szükséges az SDK-val meglévő Felhőszolgáltatás-projektek kihasználhatják frissítési folyamat is van.</span><span class="sxs-lookup"><span data-stu-id="8fb90-108">There is also no upgrade process needed to leverage this SDK with existing Cloud Service projects.</span></span> <span data-ttu-id="8fb90-109">Ahhoz, hogy az Azure SDK 3.0 használatát anélkül, hogy a frissítési folyamat, Azure SDK 3.0-s telepítése az Azure SDK 2.9 mint ugyanazon könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="8fb90-109">To allow use of the Azure SDK 3.0 without requiring an upgrade process, Azure SDK 3.0 installs to the same directories as Azure SDK 2.9.</span></span> <span data-ttu-id="8fb90-110">Leggyakrabban az összetevők 2.9 a fő verziószáma nem változott, de ehelyett frissítése a buildszáma.</span><span class="sxs-lookup"><span data-stu-id="8fb90-110">Most the components did not change the major version from 2.9 but instead just updated the build number.</span></span>

## <a name="visual-studio-2017-rtw"></a><span data-ttu-id="8fb90-111">A Visual Studio 2017 RTW</span><span class="sxs-lookup"><span data-stu-id="8fb90-111">Visual Studio 2017 RTW</span></span>

- <span data-ttu-id="8fb90-112">A Visual Studio 2017 ebben a kiadásban az Azure SDK for .NET-t az Azure-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8fb90-112">In Visual Studio 2017, this release of the Azure SDK for .NET is built in to the Azure Workload.</span></span> <span data-ttu-id="8fb90-113">Minden eszköz a végre kell hajtani az Azure fejlesztési továbbítja a Visual Studio 2017 része lesz.</span><span class="sxs-lookup"><span data-stu-id="8fb90-113">All the tools you need to do Azure development will be part of Visual Studio 2017 going forward.</span></span> <span data-ttu-id="8fb90-114">A Visual Studio 2015 az SDK-t továbbra is elérhetők WebPI keresztül.</span><span class="sxs-lookup"><span data-stu-id="8fb90-114">For Visual Studio 2015 the SDK will still be available through WebPI.</span></span> <span data-ttu-id="8fb90-115">A Microsoft megszüntetése Azure SDK .NET kiadásokban a Visual Studio 2013 most, hogy a Visual Studio 2017 kiadása.</span><span class="sxs-lookup"><span data-stu-id="8fb90-115">We have discontinued Azure SDK for .NET releases for Visual Studio 2013 now that Visual Studio 2017 has been released.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="8fb90-116">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="8fb90-116">Azure Diagnostics</span></span>

- <span data-ttu-id="8fb90-117">A viselkedés csak tárolására Cloud Services diagnosztika tárolási kapcsolati karakterlánc egy token helyett a kulccsal részleges kapcsolati karakterlánc megváltozott.</span><span class="sxs-lookup"><span data-stu-id="8fb90-117">Changed the behavior to only store a partial connection string with the key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="8fb90-118">A tényleges kulcsot most tárolja a felhasználói profil mappában, így a hozzáférése vezérelhető.</span><span class="sxs-lookup"><span data-stu-id="8fb90-118">The actual storage key is now stored in the user profile folder so its access can be controlled.</span></span> <span data-ttu-id="8fb90-119">A Visual Studio a biztonságitár-kulcs olvasását helyi Hibakeresés és a közzétételi folyamat felhasználói profil mappájába.</span><span class="sxs-lookup"><span data-stu-id="8fb90-119">Visual Studio will read the storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="8fb90-120">A fent leírt módosítás válaszul a Visual Studio Online csapata az Azure Cloud Services központi telepítési sablon fokozott, így a felhasználók a diagnosztika bővítmény beállításához, a folyamatos integrációt és telepítést Azure való közzétételkor kulcsot kell megadni.</span><span class="sxs-lookup"><span data-stu-id="8fb90-120">In response to the change described above, Visual Studio Online team enhanced the Azure Cloud Services deployment task template so users could specify the storage key for setting diagnostics extension when publishing to Azure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="8fb90-121">Hajtottunk akkor lehet tárolni a biztonságos kapcsolati karakterláncot és létrehozása az Azure Diagnostics (ÜVEGVATTA), konfigurációjával kapcsolatos problémák megoldásához environements között.</span><span class="sxs-lookup"><span data-stu-id="8fb90-121">We’ve made it possible to store secure connection string and tokenization for Azure Diagnostics (WAD), to help you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="8fb90-122">Windows Server 2016-os virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="8fb90-122">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="8fb90-123">A Visual Studio mostantól támogatja a felhő szolgáltatások telepítése az operációs rendszer családja 5 (Windows Server 2016) virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="8fb90-123">Visual Studio now supports deploying Cloud Services to OS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="8fb90-124">A meglévő felhőszolgáltatások módosíthatja a beállításokat, amelyekre az új operációsrendszer-család.</span><span class="sxs-lookup"><span data-stu-id="8fb90-124">For existing cloud services, you can change your settings to target the new OS Family.</span></span> <span data-ttu-id="8fb90-125">Létrehozásakor új felhőalapú szolgáltatások, ha úgy dönt, hogy létrehozni a szolgáltatást használó .net 4.6-os vagy újabb, az alapértelmezés szerint a szolgáltatás az operációs rendszer családja 5.</span><span class="sxs-lookup"><span data-stu-id="8fb90-125">When creating new cloud services, if you choose to create the service using .net 4.6 or higher, it will default the service to use OS Family 5.</span></span>  <span data-ttu-id="8fb90-126">További információkért tekintse át a [Vendég operációsrendszer-család támogatja a tábla](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="8fb90-126">For more information, you can review the [Guest OS Family support table](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span>

### <a name="known-issues"></a><span data-ttu-id="8fb90-127">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="8fb90-127">Known issues</span></span>

- <span data-ttu-id="8fb90-128">Az Azure .NET SDK 3.0-s bevezetett problémát, a Visual Studio 2015 párhuzamos konfigurációban a Visual Studio 2017 eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="8fb90-128">Azure .NET SDK 3.0 introduced an issue when removing Visual Studio 2017 in a side by side configuration with Visual Studio 2015.</span></span>  <span data-ttu-id="8fb90-129">Ha az Azure SDK telepítése a Visual Studio 2015, a Microsoft Azure Storage Emulator és a Microsoft Azure Compute Emulator törlődni fog a Visual Studio 2017 eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="8fb90-129">If you have the Azure SDK installed for Visual Studio 2015, the Microsoft Azure Storage Emulator and Microsoft Azure Compute Emulator will be removed if you uninstall Visual Studio 2017.</span></span>  <span data-ttu-id="8fb90-130">Ez a művelet létrehoz az létrehozásakor, és új felhőalapú szolgáltatások projekteket a Visual Studio 2015-hibakeresés hiba.</span><span class="sxs-lookup"><span data-stu-id="8fb90-130">This will produce an error when creating and debugging new Cloud services projects in Visual Studio 2015.</span></span> <span data-ttu-id="8fb90-131">Ahhoz, hogy a probléma megoldásához telepítse újra az Azure SDK a Webplatform-telepítővel.</span><span class="sxs-lookup"><span data-stu-id="8fb90-131">In order to fix this issue,  reinstall the Azure SDK from the Web Platform Installer.</span></span>  <span data-ttu-id="8fb90-132">A probléma fel lesz oldva az egy későbbi kiadásban Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="8fb90-132">The issue will be resolved in a future Visual Studio 2017 update.</span></span>  <span data-ttu-id="8fb90-133">.</span><span class="sxs-lookup"><span data-stu-id="8fb90-133">.</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="8fb90-134">Azure szerepköralapú gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="8fb90-134">Azure In-Role Cache</span></span> 

- <span data-ttu-id="8fb90-135">Azure szerepköralapú gyorsítótár terméktámogatás November 30 2016.</span><span class="sxs-lookup"><span data-stu-id="8fb90-135">Support for Azure In-Role Cache ended on November 30, 2016.</span></span> <span data-ttu-id="8fb90-136">További információért kattintson [Itt](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="8fb90-136">For more details, click [here](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>




