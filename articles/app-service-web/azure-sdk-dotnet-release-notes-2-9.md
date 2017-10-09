---
title: "aaaAzure SDK for .NET 2.9 kibocsátási megjegyzései"
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
ms.openlocfilehash: 96df2b80224190cc2093e6bf350eaec224ac2e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a><span data-ttu-id="cd004-103">Az Azure SDK for .NET 2.9 kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="cd004-103">Azure SDK for .NET 2.9 release notes</span></span>

<span data-ttu-id="cd004-104">Ez a témakör a .NET 2.9 és az Azure SDK 2.9.6 kibocsátási megjegyzései tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cd004-104">This topic includes release notes for versions 2.9 and 2.9.6 of Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-296-release-summary"></a><span data-ttu-id="cd004-105">Az Azure SDK for .NET 2.9.6 kiadási összegzése</span><span class="sxs-lookup"><span data-stu-id="cd004-105">Azure SDK for .NET 2.9.6 release summary</span></span>

<span data-ttu-id="cd004-106">Kiadás dátuma: 2016. 11/16</span><span class="sxs-lookup"><span data-stu-id="cd004-106">Release date: 11/16/2016</span></span>
 
<span data-ttu-id="cd004-107">Ebben a kiadásban nem a legfrissebb változtatásokat toohello Azure SDK 2.9 vezettek be.</span><span class="sxs-lookup"><span data-stu-id="cd004-107">No breaking changes toohello Azure SDK 2.9 have been introduced in this release.</span></span> <span data-ttu-id="cd004-108">Nincs még nincs szükség, a frissítési folyamat tooleverage az SDK-val meglévő Felhőszolgáltatás-projektek.</span><span class="sxs-lookup"><span data-stu-id="cd004-108">There is also no upgrade process needed tooleverage this SDK with existing Cloud Service projects.</span></span>

### <a name="visual-studio-2017-release-candidate"></a><span data-ttu-id="cd004-109">A Visual Studio 2017 kiadásra jelölt verziójára</span><span class="sxs-lookup"><span data-stu-id="cd004-109">Visual Studio 2017 Release Candidate</span></span>

- <span data-ttu-id="cd004-110">A Visual Studio 2017 RC ezen kiadásával a hello Azure SDK for .NET részét hello Azure munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="cd004-110">In Visual Studio 2017 RC, this release of hello Azure SDK for .NET is built in in hello Azure Workload.</span></span> <span data-ttu-id="cd004-111">Minden hello eszközökkel toodo Azure fejlesztési továbbítja a Visual Studio 2017 RC része lesz.</span><span class="sxs-lookup"><span data-stu-id="cd004-111">All hello tools you need toodo Azure development will be part of Visual Studio 2017 RC going forward.</span></span> <span data-ttu-id="cd004-112">A Visual Studio 2015-öt és a Visual Studio 2013 hello SDK továbbra is elérhetők WebPI keresztül.</span><span class="sxs-lookup"><span data-stu-id="cd004-112">For Visual Studio 2015 and Visual Studio 2013, hello SDK will still be available through WebPI.</span></span> <span data-ttu-id="cd004-113">Azt fogja kell megszűnő Azure SDK .NET kiadásokban a Visual Studio 2013, ha a Visual Studio 2017 feloldja a végső termék.</span><span class="sxs-lookup"><span data-stu-id="cd004-113">We will be discontinuing Azure SDK for .NET releases for Visual Studio 2013, when Visual Studio 2017 releases as a final product.</span></span> <span data-ttu-id="cd004-114">Kövesse a hivatkozást toodownload Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span><span class="sxs-lookup"><span data-stu-id="cd004-114">Follow this link toodownload Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="cd004-115">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="cd004-115">Azure Diagnostics</span></span>

- <span data-ttu-id="cd004-116">Módosított hello viselkedés tooonly részleges kapcsolati karakterlánc szerepét a Cloud Services diagnosztika tárolási kapcsolati karakterlánc egy token hello kulccsal tárolja.</span><span class="sxs-lookup"><span data-stu-id="cd004-116">Changed hello behavior tooonly store a partial connection string with hello key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="cd004-117">hello tényleges biztonságitár-kulcs most tárolja a hello felhasználói profil mappában, így a hozzáférése vezérelhető.</span><span class="sxs-lookup"><span data-stu-id="cd004-117">hello actual storage key is now stored in hello user profile folder so its access can be controlled.</span></span> <span data-ttu-id="cd004-118">A Visual Studio hello biztonságitár-kulcs olvasását helyi Hibakeresés és a közzétételi folyamat felhasználói profil mappájába.</span><span class="sxs-lookup"><span data-stu-id="cd004-118">Visual Studio will read hello storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="cd004-119">Válasz toohello módosítása a fent leírt, a Visual Studio Online csapat továbbfejlesztett hello Azure Cloud Services központi telepítési sablon, a felhasználók hello kulcs diagnosztika bővítmény beállításához folyamatos integrációt tooAzure közzétételekor kell megadni és üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="cd004-119">In response toohello change described above, Visual Studio Online team enhanced hello Azure Cloud Services deployment task template so users could specify hello storage key for setting diagnostics extension when publishing tooAzure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="cd004-120">Az hajtottunk lehetséges toostore a biztonságos kapcsolati karakterláncot és létrehozása az Azure Diagnostics (ÜVEGVATTA), toohelp konfigurációjával kapcsolatos problémák megoldása environements között.</span><span class="sxs-lookup"><span data-stu-id="cd004-120">We’ve made it possible toostore secure connection string and tokenization for Azure Diagnostics (WAD), toohelp you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="cd004-121">Windows Server 2016-os virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="cd004-121">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="cd004-122">A Visual Studio mostantól támogatja a központi telepítés Felhőszolgáltatások tooOS termékcsalád 5 (Windows Server 2016) virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="cd004-122">Visual Studio now supports deploying Cloud Services tooOS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="cd004-123">A meglévő felhőszolgáltatások, módosíthatja a beállításokat tootarget új operációsrendszer-család hello.</span><span class="sxs-lookup"><span data-stu-id="cd004-123">For existing cloud services, you can change your settings tootarget hello new OS Family.</span></span> <span data-ttu-id="cd004-124">Amikor új felhőalapú szolgáltatások hoz létre, ha úgy dönt, hogy toocreate hello szolgáltatást használó .net 4.6-os vagy újabb, az alapértelmezés szerint hello szolgáltatás toouse operációsrendszer-család 5.</span><span class="sxs-lookup"><span data-stu-id="cd004-124">When creating new cloud services, if you choose toocreate hello service using .net 4.6 or higher, it will default hello service toouse OS Family 5.</span></span>  <span data-ttu-id="cd004-125">További információkért tekintse át hello [Vendég operációsrendszer-család támogatja a tábla](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span><span class="sxs-lookup"><span data-stu-id="cd004-125">For more information, you can review hello [Guest OS Family support table](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span></span>

#### <a name="known-issues"></a><span data-ttu-id="cd004-126">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="cd004-126">Known issues</span></span>

- <span data-ttu-id="cd004-127">Az Azure .NET SDK 2.9.6 rendszerben jelent meg, amely megakadályozza a központi telepítés használata nem támogatott a .NET keretrendszer (például .NET 4.6) tooany operációsrendszer-család projektek korlátozás < 5.</span><span class="sxs-lookup"><span data-stu-id="cd004-127">Azure .NET SDK 2.9.6 introduced a restriction that blocks deployment of projects using unsupported .NET frameworks (such as .NET 4.6) tooany OS Family < 5.</span></span> <span data-ttu-id="cd004-128">A megoldás biztosítja [Itt](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span><span class="sxs-lookup"><span data-stu-id="cd004-128">A workaround is provided [here](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="cd004-129">Azure szerepköralapú gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="cd004-129">Azure In-Role Cache</span></span> 

- <span data-ttu-id="cd004-130">Támogatás a 2016. November 30 Azure szerepköralapú gyorsítótár végződik.</span><span class="sxs-lookup"><span data-stu-id="cd004-130">Support for Azure In-Role Cache ends on November 30, 2016.</span></span> <span data-ttu-id="cd004-131">További információért kattintson [Itt](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="cd004-131">For more details, click [here](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>

### <a name="azure-resource-manager-templates-for-azure-stack"></a><span data-ttu-id="cd004-132">Az Azure-bA az Azure Resource Manager-sablonok a verem</span><span class="sxs-lookup"><span data-stu-id="cd004-132">Azure Resource Manager Templates for Azure Stack</span></span>

- <span data-ttu-id="cd004-133">A Microsoft már megjelent Azure verem telepítési cél az Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="cd004-133">We’ve introduced Azure Stack as a deployment target for your Azure Resource Manager Templates.</span></span>


## <a name="azure-sdk-for-net-29-summary"></a><span data-ttu-id="cd004-134">Az Azure SDK for .NET 2.9 összegzése</span><span class="sxs-lookup"><span data-stu-id="cd004-134">Azure SDK for .NET 2.9 summary</span></span>

## <a name="overview"></a><span data-ttu-id="cd004-135">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="cd004-135">Overview</span></span>
<span data-ttu-id="cd004-136">Ez a dokumentum hello Azure SDK .NET 2.9 kiadás kibocsátási megjegyzései hello tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cd004-136">This document contains hello release notes for hello Azure SDK for .NET 2.9 release.</span></span> 

<span data-ttu-id="cd004-137">Ebben a kiadásban frissítésekkel kapcsolatos részletes információkért lásd: hello [Azure SDK 2.9 közlemény post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span><span class="sxs-lookup"><span data-stu-id="cd004-137">For detailed information about updates in this release, see hello [Azure SDK 2.9 announcement post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span></span>

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a><span data-ttu-id="cd004-138">A Visual Studio 2015 Update 2 és a Visual Studio "15" az Azure SDK 2.9 megtekintése</span><span class="sxs-lookup"><span data-stu-id="cd004-138">Azure SDK 2.9 for Visual Studio 2015 Update 2 and Visual Studio "15" Preview</span></span>
<span data-ttu-id="cd004-139">A frissítés tartalmazza a következő hibajavítások hello:</span><span class="sxs-lookup"><span data-stu-id="cd004-139">This update includes hello following bug fixes:</span></span>

* <span data-ttu-id="cd004-140">Kapcsolódó tooREST API-ügyfél generációs ki a mely hello karakterláncban "Ismeretlen Type" jelent hello hello kód-generációs mappa neve és/vagy a generált hello kódra eldobott hello névtér hello nevét.</span><span class="sxs-lookup"><span data-stu-id="cd004-140">Issue related tooREST API Client Generation in in which hello string "Unknown Type” would appear as hello name of hello code-gen folder and/or hello name of hello namespace dropped into hello generated code.</span></span>
* <span data-ttu-id="cd004-141">Ki, melyik hello a hitelesítési adatokat nem működött toohello Feladatütemező üzembe helyezési folyamat átadott toobe kapcsolódó tooScheduled webjobs-feladatok.</span><span class="sxs-lookup"><span data-stu-id="cd004-141">Issue related tooScheduled WebJobs in which hello authentication information was failing toobe passed toohello Scheduler provisioning process.</span></span>

<span data-ttu-id="cd004-142">A frissítés tartalmazza a következő új szolgáltatás hello:</span><span class="sxs-lookup"><span data-stu-id="cd004-142">This update includes hello following new feature:</span></span>

* <span data-ttu-id="cd004-143">Másodlagos alkalmazásszolgáltatások üzembe helyezési App Service párbeszédpanelen hello hello "Szolgáltatások" lapján támogatása.</span><span class="sxs-lookup"><span data-stu-id="cd004-143">Support for secondary App Services in hello "Services" tab of hello App Service provisioning dialog.</span></span> 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a><span data-ttu-id="cd004-144">Az Azure Data Lake Tools for Visual Studio 2015 Update 2</span><span class="sxs-lookup"><span data-stu-id="cd004-144">Azure Data Lake Tools for Visual Studio 2015 Update 2</span></span>
<span data-ttu-id="cd004-145">A frissítések hello következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="cd004-145">This updates includes hello following:</span></span>

* <span data-ttu-id="cd004-146">**Az Azure Data Lake Tools** a Visual Studio most egyesítve hello Azure SDK .NET kiadáshoz.</span><span class="sxs-lookup"><span data-stu-id="cd004-146">**Azure Data Lake Tools** for Visual Studio is now merged into hello Azure SDK for .NET release.</span></span> <span data-ttu-id="cd004-147">hello eszköz automatikusan települ Azure SDK telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="cd004-147">hello tool is automatically installed when you install Azure SDK.</span></span> 
  
    <span data-ttu-id="cd004-148">hello eszköz gyakran frissül, nyissa meg [Itt](http://aka.ms/datalaketool) tooget hello frissítések.</span><span class="sxs-lookup"><span data-stu-id="cd004-148">hello tool is updated frequently, go [here](http://aka.ms/datalaketool) tooget hello updates.</span></span>
* <span data-ttu-id="cd004-149">**Server Explorer** mostantól lehetővé teszi az összes tooview és néhány U-SQL-metaadatok entitásokat hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="cd004-149">**Server Explorer** now enables you tooview all and create some U-SQL metadata entities.</span></span> <span data-ttu-id="cd004-150">További információkért lásd: [ez](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.</span><span class="sxs-lookup"><span data-stu-id="cd004-150">For more information, see [this](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.</span></span>

## <a name="hdinsight-tools"></a><span data-ttu-id="cd004-151">A HDInsight Tools</span><span class="sxs-lookup"><span data-stu-id="cd004-151">HDInsight Tools</span></span>
<span data-ttu-id="cd004-152">**A HDInsight Tools** mostantól támogatja a HDInsight 3.3-as, beleértve a Tez ábrákat és egyéb nyelvi verzió kijavítja a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd004-152">**HDInsight Tools** for Visual Studio now supports HDInsight version 3.3, including showing Tez graphs and other language fixes.</span></span>

## <a name="azure-resource-manager"></a><span data-ttu-id="cd004-153">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cd004-153">Azure Resource Manager</span></span>
<span data-ttu-id="cd004-154">Ez a kiadás [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) Resource Manager-sablonok támogatása.</span><span class="sxs-lookup"><span data-stu-id="cd004-154">This release adds [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) support for Resource Manager templates.</span></span>

## <a name="see-also"></a><span data-ttu-id="cd004-155">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="cd004-155">See also</span></span>
[<span data-ttu-id="cd004-156">Az Azure SDK 2.9 közlemény post</span><span class="sxs-lookup"><span data-stu-id="cd004-156">Azure SDK 2.9 announcement post</span></span>](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

