---
title: "az Azure architekturális tervek aaaIntegrate biztonságossá |} Microsoft Docs"
description: " Ez a cikk segít megérteni a hello alkalmazás- és architektúra Azure toomake azt könnyebb toointegrate biztonsági tervezési és megvalósítási be. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 4f2d9386-bda3-4ec8-8b1a-cd0c11242ffc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: cfca8a1a2766f72bc3340c4e3df0019eb8b5a1e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-architecture-on-azure"></a><span data-ttu-id="c585d-103">Alkalmazásarchitektúra az Azure-on</span><span class="sxs-lookup"><span data-stu-id="c585d-103">Application architecture on Azure</span></span>
<span data-ttu-id="c585d-104">toohelp biztonságos erős architekturális alaprendszert fontos a felhőalapú megoldások a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c585d-104">toohelp secure your cloud-based solutions on Microsoft Azure, a strong architectural foundation is critical.</span></span> <span data-ttu-id="c585d-105">Mérnökök, tervezők és implementers minden alkalmazás- és architektúra erős ismerete kihasználhassa.</span><span class="sxs-lookup"><span data-stu-id="c585d-105">Architects, designers, and implementers all benefit from a strong knowledge of application and services architecture.</span></span> <span data-ttu-id="c585d-106">A legalapvetőbb Tudásbázis segítségével ismerje meg a felhőalapú megoldások összes hello összetevőit, és könnyebben toointegrate biztonsági lehetőségeinek összes szempontja a tervezési és megvalósítási teszi.</span><span class="sxs-lookup"><span data-stu-id="c585d-106">This foundational knowledge helps you understand all hello components of your cloud-based solutions and make it easier toointegrate security into all aspects of your design and implementation.</span></span>

<span data-ttu-id="c585d-107">A Microsoft rendelkezik hello toohelp követően, a architekturális vizsgálatok és tervek:</span><span class="sxs-lookup"><span data-stu-id="c585d-107">We have hello following toohelp you with your architectural investigations and designs:</span></span>

* <span data-ttu-id="c585d-108">Az architektúra infographics</span><span class="sxs-lookup"><span data-stu-id="c585d-108">Architectural infographics</span></span>
* <span data-ttu-id="c585d-109">Az architektúra tervrajzokat</span><span class="sxs-lookup"><span data-stu-id="c585d-109">Architectural blueprints</span></span>
* <span data-ttu-id="c585d-110">Felhőalapú és nagyvállalati szimbólum beállítása</span><span class="sxs-lookup"><span data-stu-id="c585d-110">Cloud and enterprise symbol set</span></span>
* <span data-ttu-id="c585d-111">3D tervezetének Visio sablon</span><span class="sxs-lookup"><span data-stu-id="c585d-111">3D blueprint Visio template</span></span>

## <a name="architectural-infographics"></a><span data-ttu-id="c585d-112">Az architektúra infographics</span><span class="sxs-lookup"><span data-stu-id="c585d-112">Architectural infographics</span></span>
<span data-ttu-id="c585d-113">A Microsoft több architekturális kapcsolódó poszterek/infographics közzéteszi.</span><span class="sxs-lookup"><span data-stu-id="c585d-113">Microsoft publishes several architectural related posters/infographics.</span></span> <span data-ttu-id="c585d-114">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="c585d-114">They include:</span></span>

* [<span data-ttu-id="c585d-115">Épület valós felhőalapú alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="c585d-115">Building Real-World Cloud Applications</span></span>](https://azure.microsoft.com/documentation/infographics/building-real-world-cloud-apps/)
* [<span data-ttu-id="c585d-116">A Cloud Serviceshez skálázás</span><span class="sxs-lookup"><span data-stu-id="c585d-116">Scaling with Cloud Services</span></span>](https://azure.microsoft.com/documentation/infographics/cloud-services/)

## <a name="architectural-blueprints"></a><span data-ttu-id="c585d-117">Az architektúra tervrajzokat</span><span class="sxs-lookup"><span data-stu-id="c585d-117">Architectural blueprints</span></span>
<span data-ttu-id="c585d-118">A Microsoft tesz közzé egy készletét magas szintű [architekturális tervrajzokat](http://aka.ms/azblueprints) megjelenítő hogyan toobuild Microsoft termékeket rendszerek adott típusú.</span><span class="sxs-lookup"><span data-stu-id="c585d-118">Microsoft publishes a set of high-level [architectural blueprints](http://aka.ms/azblueprints) showing how toobuild specific types of systems using Microsoft products.</span></span>
<span data-ttu-id="c585d-119">Minden egyes tervezetének alábbiakat:</span><span class="sxs-lookup"><span data-stu-id="c585d-119">Each blueprint includes a:</span></span>

* <span data-ttu-id="c585d-120">Egyszerű 2D Visio 2003-alapú fájl letöltése és módosítása</span><span class="sxs-lookup"><span data-stu-id="c585d-120">Flat 2D Visio 2003-based file that you can download and modify</span></span>
* <span data-ttu-id="c585d-121">Színes meghatározó 3D perspektíva PDF fájl toointroduce hello tervezetének tooless műszaki célcsoportok</span><span class="sxs-lookup"><span data-stu-id="c585d-121">Colorful 3D perspective PDF file toointroduce hello blueprint tooless technical audiences</span></span>
* <span data-ttu-id="c585d-122">Videót, amely végigvezeti a hello 3D verziója</span><span class="sxs-lookup"><span data-stu-id="c585d-122">Video that walks through hello 3D version</span></span>

## <a name="cloud-and-enterprise-symbol-set"></a><span data-ttu-id="c585d-123">Felhőalapú és nagyvállalati szimbólum beállítása</span><span class="sxs-lookup"><span data-stu-id="c585d-123">Cloud and enterprise symbol set</span></span>
<span data-ttu-id="c585d-124">[Hello Visio és szimbólumok betanítása videó megtekintése](http://aka.ms/CnESymbolsVideo) , majd [hello felhőalapú és nagyvállalati szimbólum set letöltése](http://aka.ms/CnESymbols) toohelp létrehozása az Azure, a Windows Server, SQL Server és további műszaki anyagok.</span><span class="sxs-lookup"><span data-stu-id="c585d-124">[View hello Visio and symbols training video](http://aka.ms/CnESymbolsVideo) and then [download hello Cloud and Enterprise Symbol set](http://aka.ms/CnESymbols) toohelp create technical materials that describe Azure, Windows Server, SQL Server and more.</span></span> <span data-ttu-id="c585d-125">Ha hello könyv szerelvények személyek toouse Microsoft-termékek hello szimbólumok architektúra, oktatóanyagok, bemutatók, adatlapok, infographics, tanulmányok és harmadik féltől származó könyvek-még akkor is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c585d-125">You can use hello symbols in architecture diagrams, training materials, presentations, datasheets, infographics, whitepapers, and even third party books if hello book trains people toouse Microsoft products.</span></span> <span data-ttu-id="c585d-126">Azonban nem használhatók felhasználói felületek használható.</span><span class="sxs-lookup"><span data-stu-id="c585d-126">However, they are not meant for use in user interfaces.</span></span>

## <a name="3d-blueprint-visio-template"></a><span data-ttu-id="c585d-127">3D tervezetének Visio sablon</span><span class="sxs-lookup"><span data-stu-id="c585d-127">3D blueprint Visio template</span></span>
<span data-ttu-id="c585d-128">3D verziói hello hello [Microsoft architektúra tervrajzokat](http://aka.ms/azblueprints) volt egy nem Microsoft-eszközzel hoztak létre.</span><span class="sxs-lookup"><span data-stu-id="c585d-128">hello 3D versions of hello [Microsoft Architecture Blueprints](http://aka.ms/azblueprints) were initially created in a non-Microsoft tool.</span></span> <span data-ttu-id="c585d-129">A 2015. augusztus 5 részeként rendszerrel szállított új Visio 2013 (vagy újabb verzió) sablont egy [Microsoft Architecture hitelesítő működés során, a terjesztés EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span><span class="sxs-lookup"><span data-stu-id="c585d-129">A new Visio 2013 (and later) template shipped on August 5, 2015 as part of a [Microsoft Architecture certification course distributed on EDX.ORG](https://docs.microsoft.com/azure/architecture/#microsoft-architecture-certification-course).</span></span>

<span data-ttu-id="c585d-130">hello sablon hello állomásokon kívül is érhető el.</span><span class="sxs-lookup"><span data-stu-id="c585d-130">hello template is also available outside hello course.</span></span>

* <span data-ttu-id="c585d-131">[Hello képzési videó](http://aka.ms/3dBlueprintTemplateVideo) első, így megtudhatja, miért lehet hasznos</span><span class="sxs-lookup"><span data-stu-id="c585d-131">[View hello training video](http://aka.ms/3dBlueprintTemplateVideo) first so you know what it can do</span></span>
* <span data-ttu-id="c585d-132">Töltse le a hello [Microsoft 3d tervezetének Visio sablon](http://aka.ms/3DBlueprintTemplate)</span><span class="sxs-lookup"><span data-stu-id="c585d-132">Download hello [Microsoft 3d Blueprint Visio Template](http://aka.ms/3DBlueprintTemplate)</span></span>
* <span data-ttu-id="c585d-133">Töltse le a hello [felhőalapú és nagyvállalati szimbólumok](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) toouse hello 3D sablonnal</span><span class="sxs-lookup"><span data-stu-id="c585d-133">Download hello [Cloud and Enterprise Symbols](https://docs.microsoft.com/azure/architecture/#drawing-symbol-and-icon-sets) toouse with hello 3D template</span></span>
