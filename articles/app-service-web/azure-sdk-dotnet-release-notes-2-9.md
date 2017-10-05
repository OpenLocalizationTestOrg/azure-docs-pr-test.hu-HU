---
title: "Az Azure SDK for .NET 2.9 kibocsátási megjegyzések"
description: "Az Azure SDK for .NET 2.9 kibocsátási megjegyzések"
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
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 199f0906f73d693d7cd4b73c928f23ae83b99596
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a><span data-ttu-id="29a42-103">Az Azure SDK for .NET 2.9 kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="29a42-103">Azure SDK for .NET 2.9 release notes</span></span>

<span data-ttu-id="29a42-104">Ez a témakör a .NET 2.9 és az Azure SDK 2.9.6 kibocsátási megjegyzései tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="29a42-104">This topic includes release notes for versions 2.9 and 2.9.6 of Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-296-release-summary"></a><span data-ttu-id="29a42-105">Az Azure SDK for .NET 2.9.6 kiadási összegzése</span><span class="sxs-lookup"><span data-stu-id="29a42-105">Azure SDK for .NET 2.9.6 release summary</span></span>

<span data-ttu-id="29a42-106">Kiadás dátuma: 2016. 11/16</span><span class="sxs-lookup"><span data-stu-id="29a42-106">Release date: 11/16/2016</span></span>
 
<span data-ttu-id="29a42-107">Ebben a kiadásban az Azure SDK 2.9 legfrissebb módosításokat nem vezettek be.</span><span class="sxs-lookup"><span data-stu-id="29a42-107">No breaking changes to the Azure SDK 2.9 have been introduced in this release.</span></span> <span data-ttu-id="29a42-108">Nem szükséges az SDK-val meglévő Felhőszolgáltatás-projektek kihasználhatják frissítési folyamat is van.</span><span class="sxs-lookup"><span data-stu-id="29a42-108">There is also no upgrade process needed to leverage this SDK with existing Cloud Service projects.</span></span>

### <a name="visual-studio-2017-release-candidate"></a><span data-ttu-id="29a42-109">A Visual Studio 2017 kiadásra jelölt verziójára</span><span class="sxs-lookup"><span data-stu-id="29a42-109">Visual Studio 2017 Release Candidate</span></span>

- <span data-ttu-id="29a42-110">A Visual Studio 2017 RC ebben a kiadásban az Azure SDK for .NET-t az Azure terheléseknél engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="29a42-110">In Visual Studio 2017 RC, this release of the Azure SDK for .NET is built in in the Azure Workload.</span></span> <span data-ttu-id="29a42-111">Minden eszköz a végre kell hajtani az Azure fejlesztési továbbítja a Visual Studio 2017 RC része lesz.</span><span class="sxs-lookup"><span data-stu-id="29a42-111">All the tools you need to do Azure development will be part of Visual Studio 2017 RC going forward.</span></span> <span data-ttu-id="29a42-112">A Visual Studio 2015-öt és a Visual Studio 2013, az SDK-t továbbra is elérhetők WebPI keresztül.</span><span class="sxs-lookup"><span data-stu-id="29a42-112">For Visual Studio 2015 and Visual Studio 2013, the SDK will still be available through WebPI.</span></span> <span data-ttu-id="29a42-113">Azt fogja kell megszűnő Azure SDK .NET kiadásokban a Visual Studio 2013, ha a Visual Studio 2017 feloldja a végső termék.</span><span class="sxs-lookup"><span data-stu-id="29a42-113">We will be discontinuing Azure SDK for .NET releases for Visual Studio 2013, when Visual Studio 2017 releases as a final product.</span></span> <span data-ttu-id="29a42-114">Hajtsa végre az erre a hivatkozásra kattintva töltse le a Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span><span class="sxs-lookup"><span data-stu-id="29a42-114">Follow this link to download Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="29a42-115">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="29a42-115">Azure Diagnostics</span></span>

- <span data-ttu-id="29a42-116">A viselkedés csak tárolására Cloud Services diagnosztika tárolási kapcsolati karakterlánc egy token helyett a kulccsal részleges kapcsolati karakterlánc megváltozott.</span><span class="sxs-lookup"><span data-stu-id="29a42-116">Changed the behavior to only store a partial connection string with the key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="29a42-117">A tényleges kulcsot most tárolja a felhasználói profil mappában, így a hozzáférése vezérelhető.</span><span class="sxs-lookup"><span data-stu-id="29a42-117">The actual storage key is now stored in the user profile folder so its access can be controlled.</span></span> <span data-ttu-id="29a42-118">A Visual Studio a biztonságitár-kulcs olvasását helyi Hibakeresés és a közzétételi folyamat felhasználói profil mappájába.</span><span class="sxs-lookup"><span data-stu-id="29a42-118">Visual Studio will read the storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="29a42-119">A fent leírt módosítás válaszul a Visual Studio Online csapata az Azure Cloud Services központi telepítési sablon fokozott, így a felhasználók a diagnosztika bővítmény beállításához, a folyamatos integrációt és telepítést Azure való közzétételkor kulcsot kell megadni.</span><span class="sxs-lookup"><span data-stu-id="29a42-119">In response to the change described above, Visual Studio Online team enhanced the Azure Cloud Services deployment task template so users could specify the storage key for setting diagnostics extension when publishing to Azure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="29a42-120">Hajtottunk akkor lehet tárolni a biztonságos kapcsolati karakterláncot és létrehozása az Azure Diagnostics (ÜVEGVATTA), konfigurációjával kapcsolatos problémák megoldásához environements között.</span><span class="sxs-lookup"><span data-stu-id="29a42-120">We’ve made it possible to store secure connection string and tokenization for Azure Diagnostics (WAD), to help you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="29a42-121">Windows Server 2016-os virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="29a42-121">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="29a42-122">A Visual Studio mostantól támogatja a felhő szolgáltatások telepítése az operációs rendszer családja 5 (Windows Server 2016) virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="29a42-122">Visual Studio now supports deploying Cloud Services to OS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="29a42-123">A meglévő felhőszolgáltatások módosíthatja a beállításokat, amelyekre az új operációsrendszer-család.</span><span class="sxs-lookup"><span data-stu-id="29a42-123">For existing cloud services, you can change your settings to target the new OS Family.</span></span> <span data-ttu-id="29a42-124">Létrehozásakor új felhőalapú szolgáltatások, ha úgy dönt, hogy létrehozni a szolgáltatást használó .net 4.6-os vagy újabb, az alapértelmezés szerint a szolgáltatás az operációs rendszer családja 5.</span><span class="sxs-lookup"><span data-stu-id="29a42-124">When creating new cloud services, if you choose to create the service using .net 4.6 or higher, it will default the service to use OS Family 5.</span></span>  <span data-ttu-id="29a42-125">További információkért tekintse át a [Vendég operációsrendszer-család támogatja a tábla](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span><span class="sxs-lookup"><span data-stu-id="29a42-125">For more information, you can review the [Guest OS Family support table](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span></span>

#### <a name="known-issues"></a><span data-ttu-id="29a42-126">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="29a42-126">Known issues</span></span>

- <span data-ttu-id="29a42-127">Az Azure .NET SDK 2.9.6 rendszerben jelent meg, amely blokkolja az telepítési projektek nem támogatott .NET-keretrendszert (például .NET 4.6) használatával egyetlen operációsrendszer-családról korlátozás < 5.</span><span class="sxs-lookup"><span data-stu-id="29a42-127">Azure .NET SDK 2.9.6 introduced a restriction that blocks deployment of projects using unsupported .NET frameworks (such as .NET 4.6) to any OS Family < 5.</span></span> <span data-ttu-id="29a42-128">A megoldás biztosítja [Itt](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span><span class="sxs-lookup"><span data-stu-id="29a42-128">A workaround is provided [here](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="29a42-129">Azure szerepköralapú gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="29a42-129">Azure In-Role Cache</span></span> 

- <span data-ttu-id="29a42-130">Támogatás a 2016. November 30 Azure szerepköralapú gyorsítótár végződik.</span><span class="sxs-lookup"><span data-stu-id="29a42-130">Support for Azure In-Role Cache ends on November 30, 2016.</span></span> <span data-ttu-id="29a42-131">További információért kattintson [Itt](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="29a42-131">For more details, click [here](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>

### <a name="azure-resource-manager-templates-for-azure-stack"></a><span data-ttu-id="29a42-132">Az Azure-bA az Azure Resource Manager-sablonok a verem</span><span class="sxs-lookup"><span data-stu-id="29a42-132">Azure Resource Manager Templates for Azure Stack</span></span>

- <span data-ttu-id="29a42-133">A Microsoft már megjelent Azure verem telepítési cél az Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="29a42-133">We’ve introduced Azure Stack as a deployment target for your Azure Resource Manager Templates.</span></span>


## <a name="azure-sdk-for-net-29-summary"></a><span data-ttu-id="29a42-134">Az Azure SDK for .NET 2.9 összegzése</span><span class="sxs-lookup"><span data-stu-id="29a42-134">Azure SDK for .NET 2.9 summary</span></span>

## <a name="overview"></a><span data-ttu-id="29a42-135">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="29a42-135">Overview</span></span>
<span data-ttu-id="29a42-136">Ez a dokumentum az Azure SDK for .NET 2.9 kiadás tartalmazza a kibocsátási megjegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="29a42-136">This document contains the release notes for the Azure SDK for .NET 2.9 release.</span></span> 

<span data-ttu-id="29a42-137">Ebben a kiadásban frissítésekkel kapcsolatos részletes információkért lásd: a [Azure SDK 2.9 közlemény post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span><span class="sxs-lookup"><span data-stu-id="29a42-137">For detailed information about updates in this release, see the [Azure SDK 2.9 announcement post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span></span>

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a><span data-ttu-id="29a42-138">A Visual Studio 2015 Update 2 és a Visual Studio "15" az Azure SDK 2.9 megtekintése</span><span class="sxs-lookup"><span data-stu-id="29a42-138">Azure SDK 2.9 for Visual Studio 2015 Update 2 and Visual Studio "15" Preview</span></span>
<span data-ttu-id="29a42-139">A frissítés tartalmazza a következő hibajavításokat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="29a42-139">This update includes the following bug fixes:</span></span>

* <span data-ttu-id="29a42-140">REST-API-ügyfél létrehozása a kapcsolatos problémát, amelyben a kód-generációs mappa nevét és/vagy a névtér nevét, a karakterlánc "Ismeretlen típusú" jelent a generált kód eldobva.</span><span class="sxs-lookup"><span data-stu-id="29a42-140">Issue related to REST API Client Generation in in which the string "Unknown Type” would appear as the name of the code-gen folder and/or the name of the namespace dropped into the generated code.</span></span>
* <span data-ttu-id="29a42-141">Ahol a hitelesítő adatok nem működött átadását az ütemező létesítésének folyamatát kell használnia az ütemezett WebJobs kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="29a42-141">Issue related to Scheduled WebJobs in which the authentication information was failing to be passed to the Scheduler provisioning process.</span></span>

<span data-ttu-id="29a42-142">A frissítés a következő új szolgáltatást tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="29a42-142">This update includes the following new feature:</span></span>

* <span data-ttu-id="29a42-143">A "Szolgáltatások" fülre az App Service üzembe helyezési párbeszédpanel a másodlagos alkalmazásszolgáltatások támogatása.</span><span class="sxs-lookup"><span data-stu-id="29a42-143">Support for secondary App Services in the "Services" tab of the App Service provisioning dialog.</span></span> 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a><span data-ttu-id="29a42-144">Az Azure Data Lake Tools for Visual Studio 2015 Update 2</span><span class="sxs-lookup"><span data-stu-id="29a42-144">Azure Data Lake Tools for Visual Studio 2015 Update 2</span></span>
<span data-ttu-id="29a42-145">A frissítés az alábbiakat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="29a42-145">This updates includes the following:</span></span>

* <span data-ttu-id="29a42-146">**Az Azure Data Lake Tools** a Visual Studio most egyesíti az Azure SDK for .NET kiadás.</span><span class="sxs-lookup"><span data-stu-id="29a42-146">**Azure Data Lake Tools** for Visual Studio is now merged into the Azure SDK for .NET release.</span></span> <span data-ttu-id="29a42-147">Az eszköz automatikusan települ Azure SDK telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="29a42-147">The tool is automatically installed when you install Azure SDK.</span></span> 
  
    <span data-ttu-id="29a42-148">Az eszköz gyakran frissül, nyissa meg [Itt](http://aka.ms/datalaketool) töltsék le a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="29a42-148">The tool is updated frequently, go [here](http://aka.ms/datalaketool) to get the updates.</span></span>
* <span data-ttu-id="29a42-149">**Server Explorer** most már lehetővé teszi az összes megtekintése és bizonyos U-SQL metaadat-entitások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="29a42-149">**Server Explorer** now enables you to view all and create some U-SQL metadata entities.</span></span> <span data-ttu-id="29a42-150">További információkért lásd: [ez](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.</span><span class="sxs-lookup"><span data-stu-id="29a42-150">For more information, see [this](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.</span></span>

## <a name="hdinsight-tools"></a><span data-ttu-id="29a42-151">A HDInsight Tools</span><span class="sxs-lookup"><span data-stu-id="29a42-151">HDInsight Tools</span></span>
<span data-ttu-id="29a42-152">**A HDInsight Tools** mostantól támogatja a HDInsight 3.3-as, beleértve a Tez ábrákat és egyéb nyelvi verzió kijavítja a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29a42-152">**HDInsight Tools** for Visual Studio now supports HDInsight version 3.3, including showing Tez graphs and other language fixes.</span></span>

## <a name="azure-resource-manager"></a><span data-ttu-id="29a42-153">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="29a42-153">Azure Resource Manager</span></span>
<span data-ttu-id="29a42-154">Ez a kiadás [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) Resource Manager-sablonok támogatása.</span><span class="sxs-lookup"><span data-stu-id="29a42-154">This release adds [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) support for Resource Manager templates.</span></span>

## <a name="see-also"></a><span data-ttu-id="29a42-155">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="29a42-155">See also</span></span>
[<span data-ttu-id="29a42-156">Az Azure SDK 2.9 közlemény post</span><span class="sxs-lookup"><span data-stu-id="29a42-156">Azure SDK 2.9 announcement post</span></span>](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

