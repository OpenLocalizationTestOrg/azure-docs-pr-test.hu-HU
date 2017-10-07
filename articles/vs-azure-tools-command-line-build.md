---
title: "Azure aaaCommand külön buildet |} Microsoft Docs"
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
ms.openlocfilehash: 295b61ba162dd4373ee3f56cc1462decb3e16762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="building-azure-projects-from-hello-command-line"></a><span data-ttu-id="0f479-103">Az Azure projektek hello parancssorból felépítése</span><span class="sxs-lookup"><span data-stu-id="0f479-103">Building Azure projects from hello command line</span></span>
<span data-ttu-id="0f479-104">Hello Microsoft Build Engine (MSBuild) használ, termékek, amelyen nincs telepítve a Visual Studio build-laborkörnyezetekben hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="0f479-104">Using hello Microsoft Build Engine (MSBuild), you can build products in build-lab environments where Visual Studio is not installed.</span></span> <span data-ttu-id="0f479-105">MSBuild bővíthető és teljes mértékben támogatja a Microsoft project fájlok XML formátumot használ.</span><span class="sxs-lookup"><span data-stu-id="0f479-105">MSBuild uses an XML format for project files that's extensible and fully supported by Microsoft.</span></span> <span data-ttu-id="0f479-106">Hello MSBuild fájlformátum használatával, leírhatja elemeket kell egy vagy több platformon, és konfigurációk készült.</span><span class="sxs-lookup"><span data-stu-id="0f479-106">Using hello MSBuild file format, you can describe what items must be built for one or more platforms and configurations.</span></span>

<span data-ttu-id="0f479-107">MSBuild hello parancssorból is futtathatja, és ez a témakör ismerteti azt a módszert.</span><span class="sxs-lookup"><span data-stu-id="0f479-107">You can also run MSBuild at hello command line, and this topic describes that approach.</span></span> <span data-ttu-id="0f479-108">Hello parancssorban hozhat létre a projekt specifikus konfigurációk állítható be.</span><span class="sxs-lookup"><span data-stu-id="0f479-108">By setting properties on hello command line, you can build specific configurations of a project.</span></span> <span data-ttu-id="0f479-109">Ehhez hasonlóan is definiálhat MSBuild épülő hello célokat.</span><span class="sxs-lookup"><span data-stu-id="0f479-109">Similarly, you can also define hello targets that MSBuild builds.</span></span> <span data-ttu-id="0f479-110">Parancssori paraméterek és MSBuild kapcsolatos további információkért lásd: [MSBuild parancssori útmutatóban](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f479-110">For more information about command-line parameters and MSBuild, see [MSBuild Command-Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

## <a name="msbuild-parameters"></a><span data-ttu-id="0f479-111">MSBuild-paraméterek</span><span class="sxs-lookup"><span data-stu-id="0f479-111">MSBuild parameters</span></span>
<span data-ttu-id="0f479-112">hello legegyszerűbb módja toocreate csomag a hello MSBuild toorun `/t:Publish` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0f479-112">hello simplest way toocreate a package is toorun MSBuild with hello `/t:Publish` option.</span></span> <span data-ttu-id="0f479-113">Alapértelmezés szerint ez a parancs létrehoz egy könyvtárat kapcsolat toohello gyökérmappában hello projekthez, például a `<ProjectDirectory>\bin\Configuration\app.publish\`.</span><span class="sxs-lookup"><span data-stu-id="0f479-113">By default, this command creates a directory in relation toohello root folder for hello project, such as `<ProjectDirectory>\bin\Configuration\app.publish\`.</span></span> <span data-ttu-id="0f479-114">Azure-projekt összeállításakor két fájlok jönnek létre: hello csomagfájl és hello hozzá tartozó konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="0f479-114">When you build an Azure project, two files are generated: hello package file itself and hello accompanying configuration file:</span></span>

* <span data-ttu-id="0f479-115">Alkalmazáscsomag fájl (`project.cspkg`)</span><span class="sxs-lookup"><span data-stu-id="0f479-115">Package File (`project.cspkg`)</span></span>
* <span data-ttu-id="0f479-116">Konfigurációs fájl (`ServiceConfiguration.TargetProfile.cscfg`)</span><span class="sxs-lookup"><span data-stu-id="0f479-116">Configuration File (`ServiceConfiguration.TargetProfile.cscfg`)</span></span>

<span data-ttu-id="0f479-117">Alapértelmezés szerint minden Azure-projekt helyi (hibakereséshez) buildek ki egy szolgáltatás-konfigurációs fájlt, majd egy másikat a felhő (átmeneti vagy üzemi) buildek foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="0f479-117">By default, each Azure project includes one service-configuration file for local (debugging) builds and another for cloud (staging or production) builds.</span></span> <span data-ttu-id="0f479-118">Azonban hozzáadása vagy eltávolítása a szolgáltatás-konfigurációs fájlok, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="0f479-118">However, you can add or remove service-configuration files as needed.</span></span> <span data-ttu-id="0f479-119">Visual Studio csomag összeállításakor mellett hello csomag mely szolgáltatás-konfigurációs fájl tooinclude kell adnia.</span><span class="sxs-lookup"><span data-stu-id="0f479-119">When you build a package within Visual Studio, you are asked which service-configuration file tooinclude alongside hello package.</span></span> <span data-ttu-id="0f479-120">Ha egy csomag MSBuild hozhat létre, hello helyi szolgáltatás-konfigurációs fájl veszi fel alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="0f479-120">When you build a package by using MSBuild, hello local service-configuration file is included by default.</span></span> <span data-ttu-id="0f479-121">tooinclude egy másik szolgáltatás-konfigurációs fájl, set hello `TargetProfile` hello MSBuild parancs tulajdonságát (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span><span class="sxs-lookup"><span data-stu-id="0f479-121">tooinclude a different service-configuration file, set hello `TargetProfile` property of hello MSBuild command (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span></span>

<span data-ttu-id="0f479-122">Ha azt szeretné, hogy toouse hello a másik könyvtár tárolt csomag- és konfigurációs fájljait, set hello elérési hello segítségével `/p:PublishDir=Directory\` beállítást, többek között a záró perjelet elválasztó hello.</span><span class="sxs-lookup"><span data-stu-id="0f479-122">If you want toouse an alternate directory for hello stored package and configuration files, set hello path by using hello `/p:PublishDir=Directory\` option, including hello trailing backslash separator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f479-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0f479-123">Next steps</span></span>
<span data-ttu-id="0f479-124">Miután összeállította hello csomag, ezután telepítheti azt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="0f479-124">After hello package is built, you can deploy it tooAzure.</span></span> <span data-ttu-id="0f479-125">Az oktatóanyag azt mutatja be, hogyan tooautomate, amely feldolgozni, lásd: [Azure felhőszolgáltatások folyamatos kézbesítési](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="0f479-125">For a tutorial that demonstrates how tooautomate that process, see [Continuous Delivery for Cloud Services in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span></span>

