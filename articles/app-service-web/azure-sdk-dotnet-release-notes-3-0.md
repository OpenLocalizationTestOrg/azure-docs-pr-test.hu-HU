---
title: "aaaAzure SDK for .NET 3.0 kibocsátási megjegyzései |} Microsoft Docs"
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
ms.openlocfilehash: 8970b4c9b64de40dc29a33d69006a00ae5e38a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a><span data-ttu-id="d3281-103">Az Azure SDK for .NET 3.0 kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="d3281-103">Azure SDK for .NET 3.0 release notes</span></span>

<span data-ttu-id="d3281-104">Ez a témakör kibocsátási megjegyzések a hello Azure SDK 3.0-s verziója a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="d3281-104">This topic includes release notes for version 3.0 of hello Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-30-release-summary"></a><span data-ttu-id="d3281-105">Az Azure SDK .NET 3.0 kiadás összefoglaló</span><span class="sxs-lookup"><span data-stu-id="d3281-105">Azure SDK for .NET 3.0 release summary</span></span>

<span data-ttu-id="d3281-106">Kiadás dátuma: 03/07/2017</span><span class="sxs-lookup"><span data-stu-id="d3281-106">Release date: 03/07/2017</span></span>
 
<span data-ttu-id="d3281-107">Ebben a kiadásban nem a legfrissebb változtatásokat toohello Azure SDK 3.0 vezettek be.</span><span class="sxs-lookup"><span data-stu-id="d3281-107">No breaking changes toohello Azure SDK 3.0 have been introduced in this release.</span></span> <span data-ttu-id="d3281-108">Nincs még nincs szükség, a frissítési folyamat tooleverage az SDK-val meglévő Felhőszolgáltatás-projektek.</span><span class="sxs-lookup"><span data-stu-id="d3281-108">There is also no upgrade process needed tooleverage this SDK with existing Cloud Service projects.</span></span> <span data-ttu-id="d3281-109">hello Azure SDK 3.0 anélkül, hogy a frissítési folyamat, az Azure SDK 3.0 tooallow használatát toohello telepíti, az Azure SDK 2.9 ugyanazon könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="d3281-109">tooallow use of hello Azure SDK 3.0 without requiring an upgrade process, Azure SDK 3.0 installs toohello same directories as Azure SDK 2.9.</span></span> <span data-ttu-id="d3281-110">A legtöbb hello összetevők hello főverziója nem változott az 2.9, de ehelyett frissített hello buildszáma.</span><span class="sxs-lookup"><span data-stu-id="d3281-110">Most hello components did not change hello major version from 2.9 but instead just updated hello build number.</span></span>

## <a name="visual-studio-2017-rtw"></a><span data-ttu-id="d3281-111">A Visual Studio 2017 RTW</span><span class="sxs-lookup"><span data-stu-id="d3281-111">Visual Studio 2017 RTW</span></span>

- <span data-ttu-id="d3281-112">A Visual Studio 2017 ezen kiadásával a hello Azure SDK for .NET toohello Azure munkaterhelés részét.</span><span class="sxs-lookup"><span data-stu-id="d3281-112">In Visual Studio 2017, this release of hello Azure SDK for .NET is built in toohello Azure Workload.</span></span> <span data-ttu-id="d3281-113">Minden hello eszközökkel toodo Azure fejlesztési továbbítja a Visual Studio 2017 része lesz.</span><span class="sxs-lookup"><span data-stu-id="d3281-113">All hello tools you need toodo Azure development will be part of Visual Studio 2017 going forward.</span></span> <span data-ttu-id="d3281-114">A Visual Studio 2015 hello SDK továbbra is elérhetők WebPI keresztül.</span><span class="sxs-lookup"><span data-stu-id="d3281-114">For Visual Studio 2015 hello SDK will still be available through WebPI.</span></span> <span data-ttu-id="d3281-115">A Microsoft megszüntetése Azure SDK .NET kiadásokban a Visual Studio 2013 most, hogy a Visual Studio 2017 kiadása.</span><span class="sxs-lookup"><span data-stu-id="d3281-115">We have discontinued Azure SDK for .NET releases for Visual Studio 2013 now that Visual Studio 2017 has been released.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="d3281-116">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="d3281-116">Azure Diagnostics</span></span>

- <span data-ttu-id="d3281-117">Módosított hello viselkedés tooonly részleges kapcsolati karakterlánc szerepét a Cloud Services diagnosztika tárolási kapcsolati karakterlánc egy token hello kulccsal tárolja.</span><span class="sxs-lookup"><span data-stu-id="d3281-117">Changed hello behavior tooonly store a partial connection string with hello key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="d3281-118">hello tényleges biztonságitár-kulcs most tárolja a hello felhasználói profil mappában, így a hozzáférése vezérelhető.</span><span class="sxs-lookup"><span data-stu-id="d3281-118">hello actual storage key is now stored in hello user profile folder so its access can be controlled.</span></span> <span data-ttu-id="d3281-119">A Visual Studio hello biztonságitár-kulcs olvasását helyi Hibakeresés és a közzétételi folyamat felhasználói profil mappájába.</span><span class="sxs-lookup"><span data-stu-id="d3281-119">Visual Studio will read hello storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="d3281-120">Válasz toohello módosítása a fent leírt, a Visual Studio Online csapat továbbfejlesztett hello Azure Cloud Services központi telepítési sablon, a felhasználók hello kulcs diagnosztika bővítmény beállításához folyamatos integrációt tooAzure közzétételekor kell megadni és üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="d3281-120">In response toohello change described above, Visual Studio Online team enhanced hello Azure Cloud Services deployment task template so users could specify hello storage key for setting diagnostics extension when publishing tooAzure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="d3281-121">Az hajtottunk lehetséges toostore a biztonságos kapcsolati karakterláncot és létrehozása az Azure Diagnostics (ÜVEGVATTA), toohelp konfigurációjával kapcsolatos problémák megoldása environements között.</span><span class="sxs-lookup"><span data-stu-id="d3281-121">We’ve made it possible toostore secure connection string and tokenization for Azure Diagnostics (WAD), toohelp you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="d3281-122">Windows Server 2016-os virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="d3281-122">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="d3281-123">A Visual Studio mostantól támogatja a központi telepítés Felhőszolgáltatások tooOS termékcsalád 5 (Windows Server 2016) virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="d3281-123">Visual Studio now supports deploying Cloud Services tooOS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="d3281-124">A meglévő felhőszolgáltatások, módosíthatja a beállításokat tootarget új operációsrendszer-család hello.</span><span class="sxs-lookup"><span data-stu-id="d3281-124">For existing cloud services, you can change your settings tootarget hello new OS Family.</span></span> <span data-ttu-id="d3281-125">Amikor új felhőalapú szolgáltatások hoz létre, ha úgy dönt, hogy toocreate hello szolgáltatást használó .net 4.6-os vagy újabb, az alapértelmezés szerint hello szolgáltatás toouse operációsrendszer-család 5.</span><span class="sxs-lookup"><span data-stu-id="d3281-125">When creating new cloud services, if you choose toocreate hello service using .net 4.6 or higher, it will default hello service toouse OS Family 5.</span></span>  <span data-ttu-id="d3281-126">További információkért tekintse át hello [Vendég operációsrendszer-család támogatja a tábla](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="d3281-126">For more information, you can review hello [Guest OS Family support table](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span>

### <a name="known-issues"></a><span data-ttu-id="d3281-127">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="d3281-127">Known issues</span></span>

- <span data-ttu-id="d3281-128">Az Azure .NET SDK 3.0-s bevezetett problémát, a Visual Studio 2015 párhuzamos konfigurációban a Visual Studio 2017 eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="d3281-128">Azure .NET SDK 3.0 introduced an issue when removing Visual Studio 2017 in a side by side configuration with Visual Studio 2015.</span></span>  <span data-ttu-id="d3281-129">Ha hello Azure SDK-t a Visual Studio 2015 telepítve van, a Microsoft Azure Storage Emulator hello és a Microsoft Azure Compute Emulator eltávolítja Ha eltávolítja a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d3281-129">If you have hello Azure SDK installed for Visual Studio 2015, hello Microsoft Azure Storage Emulator and Microsoft Azure Compute Emulator will be removed if you uninstall Visual Studio 2017.</span></span>  <span data-ttu-id="d3281-130">Ez a művelet létrehoz az létrehozásakor, és új felhőalapú szolgáltatások projekteket a Visual Studio 2015-hibakeresés hiba.</span><span class="sxs-lookup"><span data-stu-id="d3281-130">This will produce an error when creating and debugging new Cloud services projects in Visual Studio 2015.</span></span> <span data-ttu-id="d3281-131">A rendezés toofix probléma, telepítse újra a hello Azure SDK-t a Webplatform-telepítő hello.</span><span class="sxs-lookup"><span data-stu-id="d3281-131">In order toofix this issue,  reinstall hello Azure SDK from hello Web Platform Installer.</span></span>  <span data-ttu-id="d3281-132">hello probléma fel lesz oldva az egy későbbi kiadásban Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d3281-132">hello issue will be resolved in a future Visual Studio 2017 update.</span></span>  <span data-ttu-id="d3281-133">.</span><span class="sxs-lookup"><span data-stu-id="d3281-133">.</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="d3281-134">Azure szerepköralapú gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="d3281-134">Azure In-Role Cache</span></span> 

- <span data-ttu-id="d3281-135">Azure szerepköralapú gyorsítótár terméktámogatás November 30 2016.</span><span class="sxs-lookup"><span data-stu-id="d3281-135">Support for Azure In-Role Cache ended on November 30, 2016.</span></span> <span data-ttu-id="d3281-136">További információért kattintson [Itt](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="d3281-136">For more details, click [here](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>




