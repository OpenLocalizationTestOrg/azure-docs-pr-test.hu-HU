---
title: "Az alkalmazásproxy alkalmazás található aaaNo munkacsoport-összekötő |} Microsoft Docs"
description: "Ha nem működik az alkalmazás az Azure AD alkalmazásproxy hello összekötő csoportban összekötő esetleg felmerülő problémák megoldása"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c4baf296b316db131929c9a7c618fb9960713e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a><span data-ttu-id="ee56d-103">Nem található az alkalmazásproxy alkalmazáshoz működő összekötő csoport</span><span class="sxs-lookup"><span data-stu-id="ee56d-103">No working connector group found for an Application Proxy application</span></span>

<span data-ttu-id="ee56d-104">Ez a cikk segít tooresolve hello gyakori problémákat nincs észlelhető a következőnél: az alkalmazásproxy alkalmazás összekötő tapasztalt, Azure Active Directoryval integrált.</span><span class="sxs-lookup"><span data-stu-id="ee56d-104">This article help you tooresolve hello common issues faced when there is not a connector detected for an Application Proxy application integrated with Azure Active Directory.</span></span>

## <a name="overview-of-steps"></a><span data-ttu-id="ee56d-105">A lépések – áttekintés</span><span class="sxs-lookup"><span data-stu-id="ee56d-105">Overview of steps</span></span>
<span data-ttu-id="ee56d-106">Ha nem működő alkalmazás összekötő csoportban összekötő, van néhány módon tooresolve hello problémát:</span><span class="sxs-lookup"><span data-stu-id="ee56d-106">If there is no working Connector in a Connector Group for your application, there are a few ways tooresolve hello problem:</span></span>

-   <span data-ttu-id="ee56d-107">Ha címterekhez hello csoportban, akkor a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="ee56d-107">If you have no connectors in hello group, you can:</span></span>

    -   <span data-ttu-id="ee56d-108">Hello jobb helyszíni kiszolgálón új összekötő letöltése, és rendelje hozzá toothis csoport</span><span class="sxs-lookup"><span data-stu-id="ee56d-108">Download a new Connector on hello right on-prem server, and assign it toothis group</span></span>

    -   <span data-ttu-id="ee56d-109">Az aktív csatlakozó áthelyezi hello csoport</span><span class="sxs-lookup"><span data-stu-id="ee56d-109">Move an active Connector into hello group</span></span>

-   <span data-ttu-id="ee56d-110">Ha nincs aktív összekötők hello csoportban, akkor a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="ee56d-110">If you have no active connectors in hello group, you can:</span></span>

    -   <span data-ttu-id="ee56d-111">Az összekötő nem aktív hello OK azonosítása és elhárítása</span><span class="sxs-lookup"><span data-stu-id="ee56d-111">Identify hello reason your Connector is inactive and resolve</span></span>

    -   <span data-ttu-id="ee56d-112">Az aktív csatlakozó áthelyezi hello csoport</span><span class="sxs-lookup"><span data-stu-id="ee56d-112">Move an active Connector into hello group</span></span>

<span data-ttu-id="ee56d-113">tooknow, amelyek ezen hello probléma, az alkalmazás hello "Alkalmazásproxy" menü megnyitásához, és tekintse meg hello összekötő csoport figyelmeztető üzenet.</span><span class="sxs-lookup"><span data-stu-id="ee56d-113">tooknow which of these is hello issue, open hello “Application Proxy” menu in your Application, and look at hello Connector Group warning message.</span></span> <span data-ttu-id="ee56d-114">Azt adja meg, vagy hello csoport kell legalább egy összekötő (kell hello csoport "nincs"), vagy nem aktív összekötők rendelkezik (bár valószínűleg inaktív összekötők).</span><span class="sxs-lookup"><span data-stu-id="ee56d-114">It specify either that hello group needs at least one Connector (you have none in hello group) or that it has no active Connectors (though you likely have inactive Connectors).</span></span>

   ![Az Azure portálon összekötő csoport kijelölése](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

<span data-ttu-id="ee56d-116">Ezen lehetőségek a részletekért lásd: hello megfelelő című részhez.</span><span class="sxs-lookup"><span data-stu-id="ee56d-116">For details on each of these options, see hello corresponding section below.</span></span> <span data-ttu-id="ee56d-117">Ezek mindegyikének azt feltételezi, hogy hello összekötő kezelése lap kezdve.</span><span class="sxs-lookup"><span data-stu-id="ee56d-117">Each of these assumes that you are starting from hello Connector management page.</span></span> <span data-ttu-id="ee56d-118">Hello hibaüzenet jelenik meg a fenti nézi, ha a hello figyelmeztető üzenet kattintva megnyithatja toothis lap.</span><span class="sxs-lookup"><span data-stu-id="ee56d-118">If you are looking at hello error message above, you can go toothis page by clicking on hello warning message.</span></span> <span data-ttu-id="ee56d-119">Ellenkező esetben ez található túl címen**Azure Active Directory**a gombra, majd **vállalati alkalmazások**, majd **alkalmazásproxy.**</span><span class="sxs-lookup"><span data-stu-id="ee56d-119">Otherwise this can be found by going too**Azure Active Directory**, clicking on **Enterprise Applications**, then **Application Proxy.**</span></span>

   ![Összekötő csoportok kezelése az Azure portálon](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a><span data-ttu-id="ee56d-121">Új összekötő letöltése</span><span class="sxs-lookup"><span data-stu-id="ee56d-121">Download a new Connector</span></span>

<span data-ttu-id="ee56d-122">toodownload új összekötőt, használja a hello "Összekötő letöltése" gombot hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="ee56d-122">toodownload a new Connector, use hello “Download Connector” button at hello top of hello page.</span></span>

<span data-ttu-id="ee56d-123">Megjegyzés: hello összekötő igények toobe közvetlen toohello a háttéralkalmazás a gépre telepítve, és általában el van helyezve hello hello alkalmazás ugyanarra a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="ee56d-123">note hello Connector needs toobe installed on a machine with direct line of sight toohello backend application, and is typically placed on hello same server as hello application.</span></span> <span data-ttu-id="ee56d-124">A letöltés után hello összekötő meg kell jelennie az ebben a menüben.</span><span class="sxs-lookup"><span data-stu-id="ee56d-124">After downloading, hello Connector should appear in this menu.</span></span> <span data-ttu-id="ee56d-125">Kattintson a hello összekötő, és hello "Összekötő csoport" legördülő toomake meg arról, hogy toohello megfelelő csoport tartozik.</span><span class="sxs-lookup"><span data-stu-id="ee56d-125">click hello Connector, and use hello “Connector Group” drop-down toomake sure it belongs toohello right group.</span></span> <span data-ttu-id="ee56d-126">Hello módosítás mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="ee56d-126">Save hello change.</span></span>

   ![Hello összekötő letöltését hello Azure portál](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a><span data-ttu-id="ee56d-128">Helyezze át egy aktív összekötő</span><span class="sxs-lookup"><span data-stu-id="ee56d-128">Move an Active Connector</span></span>

<span data-ttu-id="ee56d-129">Ha egy aktív összekötő, amely toohello csoporthoz kell tartoznia, és nem sor a láthatáron toohello célalkalmazás háttér, áthelyezheti hello összekötő hozzárendelt hello csoportjához.</span><span class="sxs-lookup"><span data-stu-id="ee56d-129">If you have an active Connector that should belong toohello group and has line of sight toohello target backend application, you can move hello Connector into hello assigned group.</span></span> <span data-ttu-id="ee56d-130">toodo kattintson hello összekötő.</span><span class="sxs-lookup"><span data-stu-id="ee56d-130">toodo so, click hello Connector.</span></span> <span data-ttu-id="ee56d-131">A hello "Összekötő csoport" mezőben hello legördülő tooselect hello megfelelő csoportot, és kattintson a Mentés gombra.</span><span class="sxs-lookup"><span data-stu-id="ee56d-131">In hello “Connector Group” field, use hello drop-down tooselect hello correct group, and click Save.</span></span>

## <a name="resolve-an-inactive-connector"></a><span data-ttu-id="ee56d-132">Hárítsa el az inaktív csatlakozó</span><span class="sxs-lookup"><span data-stu-id="ee56d-132">Resolve an inactive Connector</span></span>

<span data-ttu-id="ee56d-133">Ha hello csak hello csoport összekötők nem működnek, akkor ezeknél valószínűleg olyan gépen, amely nem rendelkezik az összes hello szükséges portok feloldva.</span><span class="sxs-lookup"><span data-stu-id="ee56d-133">If hello only Connectors in hello group are inactive, they are likely on a machine that does not have all hello necessary ports unblocked.</span></span>

<span data-ttu-id="ee56d-134">Lásd: hello portok kapcsolatos problémák elhárítása dokumentumban talál részletes információt a probléma kivizsgálása.</span><span class="sxs-lookup"><span data-stu-id="ee56d-134">see hello ports Troubleshoot document for details on investigating this problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee56d-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee56d-135">Next steps</span></span>
[<span data-ttu-id="ee56d-136">Az Azure AD-alkalmazásproxy összekötők ismertetése</span><span class="sxs-lookup"><span data-stu-id="ee56d-136">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)


