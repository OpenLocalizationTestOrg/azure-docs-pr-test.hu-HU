---
title: "Azure fenyegetések modellezése eszköz - aaaMicrosoft |} Microsoft Docs"
description: "Microsoft fenyegetések modellezése eszköz, hello eszközzel, beleértve a fenyegetések modellezése folyamat hello bevezető információkat tartalmazó hello főoldala"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 923225a30c592ffdb1d254000451cfaac54a0e68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-threat-modeling-tool"></a><span data-ttu-id="2b3e0-103">Microsoft fenyegetés modellezési eszköz</span><span class="sxs-lookup"><span data-stu-id="2b3e0-103">Microsoft Threat Modeling Tool</span></span>

<span data-ttu-id="2b3e0-104">hello fenyegetések modellezése eszköz a Microsoft biztonsági fejlesztési életciklus (SDL) hello alapeleme.</span><span class="sxs-lookup"><span data-stu-id="2b3e0-104">hello Threat Modeling Tool is a core element of hello Microsoft Security Development Lifecycle (SDL).</span></span> <span data-ttu-id="2b3e0-105">Ez lehetővé teszi, hogy a szoftver tooidentify építészek részére, és amelyek viszonylag egyszerű és költséghatékony tooresolve korai, csökkenthető a potenciális biztonsági problémákat.</span><span class="sxs-lookup"><span data-stu-id="2b3e0-105">It allows software architects tooidentify and mitigate potential security issues early, when they are relatively easy and cost-effective tooresolve.</span></span> <span data-ttu-id="2b3e0-106">Ennek eredményeképpen jelentősen csökkenti fejlesztési hello összköltsége.</span><span class="sxs-lookup"><span data-stu-id="2b3e0-106">As a result, it greatly reduces hello total cost of development.</span></span> <span data-ttu-id="2b3e0-107">Emellett azt tervezett hello eszköz szakértőivel nem biztonsági szem előtt, könnyítve modellezni az összes által létrehozása, és fenyegetést modellek elemzése a világos útmutatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="2b3e0-107">Also, we designed hello tool with non-security experts in mind, making threat modeling easier for all developers by providing clear guidance on creating and analyzing threat models.</span></span> 

<span data-ttu-id="2b3e0-108">hello eszköz lehetővé teszi, hogy mindenki számára:</span><span class="sxs-lookup"><span data-stu-id="2b3e0-108">hello tool enables anyone to:</span></span>

* <span data-ttu-id="2b3e0-109">Azok a rendszerek hello biztonsági tervezési kommunikáljon</span><span class="sxs-lookup"><span data-stu-id="2b3e0-109">Communicate about hello security design of their systems</span></span>
* <span data-ttu-id="2b3e0-110">A potenciális biztonsági problémákat bevált módszer használatával azokat tervekhez elemzése</span><span class="sxs-lookup"><span data-stu-id="2b3e0-110">Analyze those designs for potential security issues using a proven methodology</span></span>
* <span data-ttu-id="2b3e0-111">Javasoljuk, és azok mérséklési lehetőségeit biztonsági problémák kezelése</span><span class="sxs-lookup"><span data-stu-id="2b3e0-111">Suggest and manage mitigations for security issues</span></span>

<span data-ttu-id="2b3e0-112">Az alábbiakban néhány tooling képességek és a tárolóhardverek, csak tooname néhány:</span><span class="sxs-lookup"><span data-stu-id="2b3e0-112">Here are some tooling capabilities and innovations, just tooname a few:</span></span>

* <span data-ttu-id="2b3e0-113">**Automatizálási:** útmutatást és a modell rajzolási visszajelzés</span><span class="sxs-lookup"><span data-stu-id="2b3e0-113">**Automation:** Guidance and feedback in drawing a model</span></span>
* <span data-ttu-id="2b3e0-114">**Elemenként STRIDE:** interaktív fenyegetések és azok mérséklési elemzése</span><span class="sxs-lookup"><span data-stu-id="2b3e0-114">**STRIDE per Element:** Guided analysis of threats and mitigations</span></span>
* <span data-ttu-id="2b3e0-115">**Jelentéskészítés:** biztonsági tevékenységeket és hello ellenőrzési fázisban tesztelése</span><span class="sxs-lookup"><span data-stu-id="2b3e0-115">**Reporting:** Security activities and testing in hello verification phase</span></span>
* <span data-ttu-id="2b3e0-116">**Egyedi módszer:** lehetővé teszi a felhasználók toobetter megjelenítheti és fenyegetések ismertetése</span><span class="sxs-lookup"><span data-stu-id="2b3e0-116">**Unique Methodology:** Enables users toobetter visualize and understand threats</span></span>
* <span data-ttu-id="2b3e0-117">**A fejlesztők és a szoftverfrissítési középre:** számos különböző módszer igazított eszközök vagy a támadók.</span><span class="sxs-lookup"><span data-stu-id="2b3e0-117">**Designed for Developers and Centered on Software:** many approaches are centered on assets or attackers.</span></span> <span data-ttu-id="2b3e0-118">A Microsoft szoftverek igazított.</span><span class="sxs-lookup"><span data-stu-id="2b3e0-118">We are centered on software.</span></span> <span data-ttu-id="2b3e0-119">Azt minden szoftverfejlesztőkkel és fejlesztők jártas – például a szoftver architektúra képek rajzolási tevékenységek létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b3e0-119">We build on activities that all software developers and architects are familiar with -- such as drawing pictures for their software architecture</span></span>
* <span data-ttu-id="2b3e0-120">**Tervezési elemzés összpontosított:** hello kifejezés "modellezni" tooeither jelentheti a követelményeknek, vagy a Tervező elemzési technika.</span><span class="sxs-lookup"><span data-stu-id="2b3e0-120">**Focused on Design Analysis:** hello term "threat modeling" can refer tooeither a requirements or a design analysis technique.</span></span> <span data-ttu-id="2b3e0-121">Egyes esetekben hello két összetett keveréke tooa hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="2b3e0-121">Sometimes, it refers tooa complex blend of hello two.</span></span> <span data-ttu-id="2b3e0-122">hello Microsoft SDL megközelítés toothreat modellezési a célzott tervezési elemzési technika</span><span class="sxs-lookup"><span data-stu-id="2b3e0-122">hello Microsoft SDL approach toothreat modeling is a focused design analysis technique</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b3e0-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b3e0-123">Next steps</span></span>

<span data-ttu-id="2b3e0-124">hello az alábbi táblázat tartalmazza a fontos hivatkozások tooget hello fenyegetések modellezése eszköz kapja:</span><span class="sxs-lookup"><span data-stu-id="2b3e0-124">hello table below contains important links tooget you started with hello Threat Modeling Tool:</span></span>

| <span data-ttu-id="2b3e0-125">Lépés</span><span class="sxs-lookup"><span data-stu-id="2b3e0-125">Step</span></span>  | <span data-ttu-id="2b3e0-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b3e0-126">Description</span></span>                                                                                   |
| ----- | --------------------------------------------------------------------------------------------- |
| <span data-ttu-id="2b3e0-127">**1**</span><span class="sxs-lookup"><span data-stu-id="2b3e0-127">**1**</span></span> | [<span data-ttu-id="2b3e0-128">Töltse le a fenyegetések modellezése eszköz hello</span><span class="sxs-lookup"><span data-stu-id="2b3e0-128">Download hello Threat Modeling Tool</span></span>](https://aka.ms/tmtpreview)                                |
| <span data-ttu-id="2b3e0-129">**2**</span><span class="sxs-lookup"><span data-stu-id="2b3e0-129">**2**</span></span> | [<span data-ttu-id="2b3e0-130">Az első lépések útmutató olvasása</span><span class="sxs-lookup"><span data-stu-id="2b3e0-130">Read Our getting started guide</span></span>](./azure-security-threat-modeling-tool-getting-started.md)    |
| <span data-ttu-id="2b3e0-131">**3**</span><span class="sxs-lookup"><span data-stu-id="2b3e0-131">**3**</span></span> | [<span data-ttu-id="2b3e0-132">Ismerkedjen meg hello szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2b3e0-132">Get familiar with hello features</span></span>](./azure-security-threat-modeling-tool-feature-overview.md)   |
| <span data-ttu-id="2b3e0-133">**4**</span><span class="sxs-lookup"><span data-stu-id="2b3e0-133">**4**</span></span> | [<span data-ttu-id="2b3e0-134">További tudnivalók a generált fenyegetés kategóriák</span><span class="sxs-lookup"><span data-stu-id="2b3e0-134">Learn about generated threat categories</span></span>](./azure-security-threat-modeling-tool-threats.md)   |
| <span data-ttu-id="2b3e0-135">**5**</span><span class="sxs-lookup"><span data-stu-id="2b3e0-135">**5**</span></span> | [<span data-ttu-id="2b3e0-136">Azok mérséklési toogenerated fenyegetések keresése</span><span class="sxs-lookup"><span data-stu-id="2b3e0-136">Find mitigations toogenerated threats</span></span>](./azure-security-threat-modeling-tool-mitigations.md) |

## <a name="resources"></a><span data-ttu-id="2b3e0-137">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="2b3e0-137">Resources</span></span>

<span data-ttu-id="2b3e0-138">Az alábbiakban néhány régebbi cikkek továbbra is megfelelő toothreat modellezési ma:</span><span class="sxs-lookup"><span data-stu-id="2b3e0-138">Here are a few older articles still relevant toothreat modeling today:</span></span>

* [<span data-ttu-id="2b3e0-139">A fontosság a fenyegetések modellezése hello a következő cikket:</span><span class="sxs-lookup"><span data-stu-id="2b3e0-139">Article on hello Importance of Threat Modeling</span></span>](https://msdn.microsoft.com/magazine/dd347831.aspx)
* [<span data-ttu-id="2b3e0-140">Megbízható számítástechnika által közzétett képzési</span><span class="sxs-lookup"><span data-stu-id="2b3e0-140">Training Published by Trustworthy Computing</span></span>](https://www.microsoft.com/download/details.aspx?id=16420)

<span data-ttu-id="2b3e0-141">Tekintse meg néhány fenyegetések modellezése eszköz szakértők megtette:</span><span class="sxs-lookup"><span data-stu-id="2b3e0-141">Check out what a few Threat Modeling Tool experts have done:</span></span>

* [<span data-ttu-id="2b3e0-142">Fenyegetések Manager</span><span class="sxs-lookup"><span data-stu-id="2b3e0-142">Threats Manager</span></span>](https://simoneonsecurity.com/threatsmanagersetup-v1-5-10/)
* [<span data-ttu-id="2b3e0-143">Simone Curzi biztonsági Blog</span><span class="sxs-lookup"><span data-stu-id="2b3e0-143">Simone Curzi Security Blog</span></span>](https://simoneonsecurity.com/)