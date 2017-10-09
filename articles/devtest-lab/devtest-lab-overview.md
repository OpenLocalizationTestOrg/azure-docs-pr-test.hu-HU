---
title: "aaaAbout Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan DevTest Labs révén könnyen toocreate, felügyelheti, és figyelheti az Azure virtuális gépek"
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
ms.openlocfilehash: 5e5aa683f80144a2f6872c7fa328016120ea6253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-azure-devtest-labs"></a><span data-ttu-id="71e05-103">Tudnivalók az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="71e05-103">About Azure DevTest Labs</span></span>
## <a name="overview"></a><span data-ttu-id="71e05-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="71e05-104">Overview</span></span>
<span data-ttu-id="71e05-105">A fejlesztők és a tesztelést hasznosítani toosolve hello késések létrehozása és kezelése a környezetek toohello felhő címen.</span><span class="sxs-lookup"><span data-stu-id="71e05-105">Developers and testers are looking toosolve hello delays in creating and managing their environments by going toohello cloud.</span></span>  <span data-ttu-id="71e05-106">Azure környezetben késések hello problémáját megoldja, és lehetővé teszi, hogy önkiszolgáló új költség hatékony struktúrán belül.</span><span class="sxs-lookup"><span data-stu-id="71e05-106">Azure solves hello problem of environment delays and allows self-service within a new cost efficient structure.</span></span>  <span data-ttu-id="71e05-107">A fejlesztők és a tesztelést továbbra is kell, önálló szolgálatban környezetük konfigurálása toospend jelentős időt.</span><span class="sxs-lookup"><span data-stu-id="71e05-107">However, developers and testers still need toospend considerable time configuring their self-served environments.</span></span> <span data-ttu-id="71e05-108">Emellett döntéshozók nem biztosak abban, hogyan tooleverage hello felhő toomaximize hozzáadása nélkül a költséghatékony túlságosan feldolgozni terhet kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="71e05-108">Also, decision makers are uncertain about how tooleverage hello cloud toomaximize their cost savings without adding too much process overhead.</span></span>

<span data-ttu-id="71e05-109">Az Azure DevTest Labs szolgáltatásban egy szolgáltatás, amellyel a fejlesztők és tesztelők webhelyről gyorsan tesztkörnyezetek létrehozása az Azure-ban minimalizálja a pazarlás és költség ellenőrzése közben.</span><span class="sxs-lookup"><span data-stu-id="71e05-109">Azure DevTest Labs is a service that helps developers and testers quickly create environments in Azure while minimizing waste and controlling cost.</span></span> <span data-ttu-id="71e05-110">Az alkalmazás legújabb verziója hello által gyors kiépítése az újrafelhasználható sablonokkal és az összetevők Windows és Linux környezetben tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="71e05-110">You can test hello latest version of your application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span> <span data-ttu-id="71e05-111">A telepítési folyamat könnyen integrálhatja DevTest Labs tooprovision igény környezetekben.</span><span class="sxs-lookup"><span data-stu-id="71e05-111">Easily integrate your deployment pipeline with DevTest Labs tooprovision on-demand environments.</span></span> <span data-ttu-id="71e05-112">Növelheti a kiépítés több teszt ügynök által végzett terhelését, és hozza létre a képzési és bemutatók előtti ponton aktívvá vált környezetekben.</span><span class="sxs-lookup"><span data-stu-id="71e05-112">Scale up your load testing by provisioning multiple test agents, and create pre-provisioned environments for training and demos.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

<span data-ttu-id="71e05-113">DevTest Labs biztosít a következő előnyöket létrehozása, konfigurálása és kezelése fejlesztői és tesztelési környezetek a hello felhő hello</span><span class="sxs-lookup"><span data-stu-id="71e05-113">DevTest Labs provides hello following benefits in creating, configuring, and managing developer and test environments in hello cloud</span></span>

## <a name="worry-free-self-service"></a><span data-ttu-id="71e05-114">Megbízható önkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="71e05-114">Worry-free self-service</span></span>
<span data-ttu-id="71e05-115">DevTest Labs segítségével könnyebben toocontrol költségek a labor - például a virtuális gépek (VM) számát a tooset házirendek lehetővé minden felhasználó és a virtuális gépek száma tesztkörnyezetenként száma.</span><span class="sxs-lookup"><span data-stu-id="71e05-115">DevTest Labs makes it easier toocontrol costs by allowing you tooset policies on your lab - such as number of virtual machines (VM) per user and number of VMs per lab.</span></span> <span data-ttu-id="71e05-116">DevTest Labs szolgáltatásban is lehetővé teszi, hogy Ön toocreate házirendek tooautomatically állítsa le és indítsa el a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="71e05-116">DevTest Labs also enables you toocreate policies tooautomatically shut down and start VMs.</span></span>

## <a name="quickly-get-tooready-to-test"></a><span data-ttu-id="71e05-117">Gyorsan karban lehessen tooready-teszt</span><span class="sxs-lookup"><span data-stu-id="71e05-117">Quickly get tooready-to-test</span></span>
<span data-ttu-id="71e05-118">DevTest Labs toocreate előtti ponton aktívvá vált környezetekben, ahol minden a csapat kell toostart fejlesztés és tesztelés az alkalmazások lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="71e05-118">DevTest Labs enables you toocreate pre-provisioned environments with everything your team needs toostart developing and testing applications.</span></span> <span data-ttu-id="71e05-119">Egyszerűen jogcím hello környezetekben, ahol a hello jó utolsó buildverziót az alkalmazás telepítve van, és azonnal működik beolvasása.</span><span class="sxs-lookup"><span data-stu-id="71e05-119">Simply claim hello environments where hello last good build of your application is installed and get working right away.</span></span> <span data-ttu-id="71e05-120">Vagy tárolók használata akkor is gyorsabb és Karcsúbb környezet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="71e05-120">Or, use containers for even faster and leaner environment creation.</span></span>

## <a name="create-once-use-everywhere"></a><span data-ttu-id="71e05-121">Ha egyszer létrehozta, bárhol használhatja</span><span class="sxs-lookup"><span data-stu-id="71e05-121">Create once, use everywhere</span></span>
<span data-ttu-id="71e05-122">Rögzítése és megoszthatja a toocreate fejlesztői környezet sablonok és a csapat vagy szervezet – az összes verziókezelő - összetevők, és könnyen tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="71e05-122">Capture and share environment templates and artifacts within your team or organization - all in source control - toocreate developer and test environments easily.</span></span>

## <a name="integrates-with-your-existing-toolchain"></a><span data-ttu-id="71e05-123">Integráció a meglévő eszközlánccal</span><span class="sxs-lookup"><span data-stu-id="71e05-123">Integrates with your existing toolchain</span></span>
<span data-ttu-id="71e05-124">Használja ki az előre elkészített beépülő modulok vagy az API tooprovision fejlesztési és tesztelési célú környezetek közvetlenül az előnyben részesített folyamatos integrációt (CI) eszközt, az integrált fejlesztési környezeti (IDE), vagy a kiadás folyamat automatikus.</span><span class="sxs-lookup"><span data-stu-id="71e05-124">Leverage pre-made plug-ins or our API tooprovision Dev/Test environments directly from your preferred continuous integration (CI) tool, integrated development environment (IDE), or automated release pipeline.</span></span> <span data-ttu-id="71e05-125">Is használhatja az átfogó parancssori eszközt.</span><span class="sxs-lookup"><span data-stu-id="71e05-125">You can also use our comprehensive command-line tool.</span></span>


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="71e05-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="71e05-126">Next steps</span></span>
[<span data-ttu-id="71e05-127">DevTest Labs-fogalmak</span><span class="sxs-lookup"><span data-stu-id="71e05-127">DevTest Labs concepts</span></span>](devtest-lab-concepts.md)

