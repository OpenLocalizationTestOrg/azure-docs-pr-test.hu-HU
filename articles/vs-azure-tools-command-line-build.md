---
title: Az Azure parancssori build |} Microsoft Docs
description: Az Azure parancssori build
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2017
ms.author: kraigb
ms.openlocfilehash: 5fe910e2757dd5ec783538e23e7f52e2f5725b39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="building-azure-projects-from-the-command-line"></a><span data-ttu-id="aeb82-103">A parancssorból Azure projektek felépítése</span><span class="sxs-lookup"><span data-stu-id="aeb82-103">Building Azure projects from the command line</span></span>
<span data-ttu-id="aeb82-104">A Microsoft Build motor (MSBuild) használata, amelyen nincs telepítve a Visual Studio build-laborkörnyezetekben termékek hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="aeb82-104">Using the Microsoft Build Engine (MSBuild), you can build products in build-lab environments where Visual Studio is not installed.</span></span> <span data-ttu-id="aeb82-105">MSBuild bővíthető és teljes mértékben támogatja a Microsoft project fájlok XML formátumot használ.</span><span class="sxs-lookup"><span data-stu-id="aeb82-105">MSBuild uses an XML format for project files that's extensible and fully supported by Microsoft.</span></span> <span data-ttu-id="aeb82-106">Az MSBuild formátumát használva leírhatja elemeket kell egy vagy több platformon, és konfigurációk készült.</span><span class="sxs-lookup"><span data-stu-id="aeb82-106">Using the MSBuild file format, you can describe what items must be built for one or more platforms and configurations.</span></span>

<span data-ttu-id="aeb82-107">MSBuild a parancssorból is futtathatja, és ez a témakör ismerteti azt a módszert.</span><span class="sxs-lookup"><span data-stu-id="aeb82-107">You can also run MSBuild at the command line, and this topic describes that approach.</span></span> <span data-ttu-id="aeb82-108">A parancssorban hozhat létre a projekt specifikus konfigurációk állítható be.</span><span class="sxs-lookup"><span data-stu-id="aeb82-108">By setting properties on the command line, you can build specific configurations of a project.</span></span> <span data-ttu-id="aeb82-109">Ehhez hasonlóan is meghatározhat, amelyeknek MSBuild alkot.</span><span class="sxs-lookup"><span data-stu-id="aeb82-109">Similarly, you can also define the targets that MSBuild builds.</span></span> <span data-ttu-id="aeb82-110">Parancssori paraméterek és MSBuild kapcsolatos további információkért lásd: [MSBuild parancssori útmutatóban](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="aeb82-110">For more information about command-line parameters and MSBuild, see [MSBuild Command-Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

## <a name="msbuild-parameters"></a><span data-ttu-id="aeb82-111">MSBuild-paraméterek</span><span class="sxs-lookup"><span data-stu-id="aeb82-111">MSBuild parameters</span></span>
<span data-ttu-id="aeb82-112">A legegyszerűbben-csomag létrehozása után az MSBuild futtatásához a `/t:Publish` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="aeb82-112">The simplest way to create a package is to run MSBuild with the `/t:Publish` option.</span></span> <span data-ttu-id="aeb82-113">Alapértelmezés szerint ez a parancs létrehoz egy könyvtárat a projekthez, amely a gyökérmappában található viszonyítva például `<ProjectDirectory>\bin\Configuration\app.publish\`.</span><span class="sxs-lookup"><span data-stu-id="aeb82-113">By default, this command creates a directory in relation to the root folder for the project, such as `<ProjectDirectory>\bin\Configuration\app.publish\`.</span></span> <span data-ttu-id="aeb82-114">Azure-projekt összeállításakor két fájlok jönnek létre: a csomag fájl maga és a hozzá tartozó konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="aeb82-114">When you build an Azure project, two files are generated: the package file itself and the accompanying configuration file:</span></span>

* <span data-ttu-id="aeb82-115">Alkalmazáscsomag fájl (`project.cspkg`)</span><span class="sxs-lookup"><span data-stu-id="aeb82-115">Package File (`project.cspkg`)</span></span>
* <span data-ttu-id="aeb82-116">Konfigurációs fájl (`ServiceConfiguration.TargetProfile.cscfg`)</span><span class="sxs-lookup"><span data-stu-id="aeb82-116">Configuration File (`ServiceConfiguration.TargetProfile.cscfg`)</span></span>

<span data-ttu-id="aeb82-117">Alapértelmezés szerint minden Azure-projekt helyi (hibakereséshez) buildek ki egy szolgáltatás-konfigurációs fájlt, majd egy másikat a felhő (átmeneti vagy üzemi) buildek foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="aeb82-117">By default, each Azure project includes one service-configuration file for local (debugging) builds and another for cloud (staging or production) builds.</span></span> <span data-ttu-id="aeb82-118">Azonban hozzáadása vagy eltávolítása a szolgáltatás-konfigurációs fájlok, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="aeb82-118">However, you can add or remove service-configuration files as needed.</span></span> <span data-ttu-id="aeb82-119">Visual Studio csomag összeállításakor mely szolgáltatás-konfigurációs fájlt a mellett a csomag kell adnia.</span><span class="sxs-lookup"><span data-stu-id="aeb82-119">When you build a package within Visual Studio, you are asked which service-configuration file to include alongside the package.</span></span> <span data-ttu-id="aeb82-120">Ha egy csomag MSBuild hozhat létre, a helyi szolgáltatás-konfigurációs fájl veszi fel alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="aeb82-120">When you build a package by using MSBuild, the local service-configuration file is included by default.</span></span> <span data-ttu-id="aeb82-121">Egy másik szolgáltatás-konfigurációs fájlt, adja meg a `TargetProfile` az MSBuild parancs tulajdonságának (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span><span class="sxs-lookup"><span data-stu-id="aeb82-121">To include a different service-configuration file, set the `TargetProfile` property of the MSBuild command (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span></span>

<span data-ttu-id="aeb82-122">Szeretne másik könyvtár a tárolt csomag- és konfigurációs fájljait használja, ha az elérési útjának beállítása használatával a `/p:PublishDir=Directory\` beállítást, többek között a záró perjelet elválasztó.</span><span class="sxs-lookup"><span data-stu-id="aeb82-122">If you want to use an alternate directory for the stored package and configuration files, set the path by using the `/p:PublishDir=Directory\` option, including the trailing backslash separator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aeb82-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aeb82-123">Next steps</span></span>
<span data-ttu-id="aeb82-124">Miután összeállította a csomagot, telepítheti az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="aeb82-124">After the package is built, you can deploy it to Azure.</span></span> <span data-ttu-id="aeb82-125">Ez az oktatóanyag bemutatja, hogyan folyamat automatizálásához, lásd: [Azure felhőszolgáltatások folyamatos kézbesítési](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="aeb82-125">For a tutorial that demonstrates how to automate that process, see [Continuous Delivery for Cloud Services in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span></span>

