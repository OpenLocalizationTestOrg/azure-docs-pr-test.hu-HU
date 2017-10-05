---
title: "Tudnivalók az Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Ismerje meg, hogyan DevTest Labs szolgáltatásban is megkönnyítheti létrehozására, kezelésére és az Azure virtuális gépek figyelése"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: 62e2d214d6d685c7f27c8c45cae161eb25ed1cbd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="about-azure-devtest-labs"></a><span data-ttu-id="8ef13-103">Tudnivalók az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="8ef13-103">About Azure DevTest Labs</span></span>
## <a name="overview"></a><span data-ttu-id="8ef13-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8ef13-104">Overview</span></span>
<span data-ttu-id="8ef13-105">A fejlesztők és a tesztelést hasznosítani létrehozását és kezelését a környezetük nyissa meg a felhő tapasztalt késést megoldására.</span><span class="sxs-lookup"><span data-stu-id="8ef13-105">Developers and testers are looking to solve the delays in creating and managing their environments by going to the cloud.</span></span>  <span data-ttu-id="8ef13-106">Azure megoldja a problémát, a környezet késések, és lehetővé teszi, hogy önkiszolgáló új költség hatékony struktúrán belül.</span><span class="sxs-lookup"><span data-stu-id="8ef13-106">Azure solves the problem of environment delays and allows self-service within a new cost efficient structure.</span></span>  <span data-ttu-id="8ef13-107">Azonban a fejlesztők és a tesztelést továbbra is szeretné önálló szolgálatban környezetük konfigurálása jelentős időt.</span><span class="sxs-lookup"><span data-stu-id="8ef13-107">However, developers and testers still need to spend considerable time configuring their self-served environments.</span></span> <span data-ttu-id="8ef13-108">Emellett döntéshozók nem biztosak abban, hogyan használhatók ki a felhő maximalizálása a költséghatékony túl sok folyamat terhelés hozzáadása nélkül kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="8ef13-108">Also, decision makers are uncertain about how to leverage the cloud to maximize their cost savings without adding too much process overhead.</span></span>

<span data-ttu-id="8ef13-109">Az Azure DevTest Labs szolgáltatásban egy szolgáltatás, amellyel a fejlesztők és tesztelők webhelyről gyorsan tesztkörnyezetek létrehozása az Azure-ban minimalizálja a pazarlás és költség ellenőrzése közben.</span><span class="sxs-lookup"><span data-stu-id="8ef13-109">Azure DevTest Labs is a service that helps developers and testers quickly create environments in Azure while minimizing waste and controlling cost.</span></span> <span data-ttu-id="8ef13-110">Az alkalmazás legújabb verzióját a Windows- és Linux-környezetek újból felhasználható sablonokkal és összetevőkkel történő gyors kiépítésével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8ef13-110">You can test the latest version of your application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span> <span data-ttu-id="8ef13-111">A telepítési folyamat könnyen integrálhatja DevTest Labs igény környezetek kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8ef13-111">Easily integrate your deployment pipeline with DevTest Labs to provision on-demand environments.</span></span> <span data-ttu-id="8ef13-112">Növelheti a kiépítés több teszt ügynök által végzett terhelését, és hozza létre a képzési és bemutatók előtti ponton aktívvá vált környezetekben.</span><span class="sxs-lookup"><span data-stu-id="8ef13-112">Scale up your load testing by provisioning multiple test agents, and create pre-provisioned environments for training and demos.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

<span data-ttu-id="8ef13-113">DevTest Labs előnyei a következők a létrehozásához, konfigurálásához és kezeléséhez a felhőben fejlesztői és tesztkörnyezetek</span><span class="sxs-lookup"><span data-stu-id="8ef13-113">DevTest Labs provides the following benefits in creating, configuring, and managing developer and test environments in the cloud</span></span>

## <a name="worry-free-self-service"></a><span data-ttu-id="8ef13-114">Megbízható önkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="8ef13-114">Worry-free self-service</span></span>
<span data-ttu-id="8ef13-115">DevTest Labs megkönnyíti költségek szabályozása: lehetővé teszi a szabályzatok beállítását, a laborban a – például a virtuális gépek (VM) felhasználónként számát és a virtuális gépek száma tesztkörnyezetenként száma.</span><span class="sxs-lookup"><span data-stu-id="8ef13-115">DevTest Labs makes it easier to control costs by allowing you to set policies on your lab - such as number of virtual machines (VM) per user and number of VMs per lab.</span></span> <span data-ttu-id="8ef13-116">DevTest Labs szolgáltatásban is lehetővé teszi a házirendek automatikusan állítsa le és indítsa el a virtuális gépek létrehozására.</span><span class="sxs-lookup"><span data-stu-id="8ef13-116">DevTest Labs also enables you to create policies to automatically shut down and start VMs.</span></span>

## <a name="quickly-get-to-ready-to-test"></a><span data-ttu-id="8ef13-117">Készen áll-teszt gyors megjelenítése</span><span class="sxs-lookup"><span data-stu-id="8ef13-117">Quickly get to ready-to-test</span></span>
<span data-ttu-id="8ef13-118">DevTest Labs lehetővé teszi minden fejlesztés és tesztelés az alkalmazások elindításához szükséges a csoport létrehozása előtti ponton aktívvá vált környezetekben.</span><span class="sxs-lookup"><span data-stu-id="8ef13-118">DevTest Labs enables you to create pre-provisioned environments with everything your team needs to start developing and testing applications.</span></span> <span data-ttu-id="8ef13-119">Egyszerűen jogcím a környezetekben, ahol a legutolsó helyes összeállítása az alkalmazás telepítve van, és azonnal működik beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8ef13-119">Simply claim the environments where the last good build of your application is installed and get working right away.</span></span> <span data-ttu-id="8ef13-120">Vagy tárolók használata akkor is gyorsabb és Karcsúbb környezet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="8ef13-120">Or, use containers for even faster and leaner environment creation.</span></span>

## <a name="create-once-use-everywhere"></a><span data-ttu-id="8ef13-121">Ha egyszer létrehozta, bárhol használhatja</span><span class="sxs-lookup"><span data-stu-id="8ef13-121">Create once, use everywhere</span></span>
<span data-ttu-id="8ef13-122">Rögzítése, és megoszthatja a környezet sablonok és a belül a csapat vagy szervezet - mind a verziókövetési - összetevők fejlesztői létrehozásához, és könnyen tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="8ef13-122">Capture and share environment templates and artifacts within your team or organization - all in source control - to create developer and test environments easily.</span></span>

## <a name="integrates-with-your-existing-toolchain"></a><span data-ttu-id="8ef13-123">Integráció a meglévő eszközlánccal</span><span class="sxs-lookup"><span data-stu-id="8ef13-123">Integrates with your existing toolchain</span></span>
<span data-ttu-id="8ef13-124">Éljen az előre elkészített beépülő modulok és az API fejlesztési és tesztelési célú környezetek közvetlenül az előnyben részesített folyamatos integrációt (CI) eszközt, az integrált fejlesztési környezeti (IDE), vagy kiadási folyamat automatikus kiépítését.</span><span class="sxs-lookup"><span data-stu-id="8ef13-124">Leverage pre-made plug-ins or our API to provision Dev/Test environments directly from your preferred continuous integration (CI) tool, integrated development environment (IDE), or automated release pipeline.</span></span> <span data-ttu-id="8ef13-125">Is használhatja az átfogó parancssori eszközt.</span><span class="sxs-lookup"><span data-stu-id="8ef13-125">You can also use our comprehensive command-line tool.</span></span>


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="8ef13-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ef13-126">Next steps</span></span>
[<span data-ttu-id="8ef13-127">DevTest Labs-fogalmak</span><span class="sxs-lookup"><span data-stu-id="8ef13-127">DevTest Labs concepts</span></span>](devtest-lab-concepts.md)

