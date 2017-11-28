---
title: "Nincs munkacsoport összekötő található az alkalmazásproxy-alkalmazás |} Microsoft Docs"
description: "Ha nem működik egy összekötő csoportot az alkalmazásba az Azure AD-alkalmazásproxy összekötőjét esetleg felmerülő problémák megoldása"
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
ms.openlocfilehash: 4945958deedc8a1d9989ff901192c03a5363b4dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a><span data-ttu-id="1b362-103">Nem található az alkalmazásproxy alkalmazáshoz működő összekötő csoport</span><span class="sxs-lookup"><span data-stu-id="1b362-103">No working connector group found for an Application Proxy application</span></span>

<span data-ttu-id="1b362-104">Ez a cikk segít nincs észlelhető a következőnél: az alkalmazásproxy alkalmazás összekötő tapasztalt gyakori problémák megoldásához Azure Active Directory integrált része.</span><span class="sxs-lookup"><span data-stu-id="1b362-104">This article help you to resolve the common issues faced when there is not a connector detected for an Application Proxy application integrated with Azure Active Directory.</span></span>

## <a name="overview-of-steps"></a><span data-ttu-id="1b362-105">A lépések – áttekintés</span><span class="sxs-lookup"><span data-stu-id="1b362-105">Overview of steps</span></span>
<span data-ttu-id="1b362-106">Ha nem működő alkalmazás összekötő csoportban összekötő, néhány módon a probléma megoldása:</span><span class="sxs-lookup"><span data-stu-id="1b362-106">If there is no working Connector in a Connector Group for your application, there are a few ways to resolve the problem:</span></span>

-   <span data-ttu-id="1b362-107">Ha nincs összekötők a csoportban található, akkor a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="1b362-107">If you have no connectors in the group, you can:</span></span>

    -   <span data-ttu-id="1b362-108">A megfelelő helyszíni kiszolgálón új összekötő letöltése, és rendelje hozzá ehhez a csoporthoz</span><span class="sxs-lookup"><span data-stu-id="1b362-108">Download a new Connector on the right on-prem server, and assign it to this group</span></span>

    -   <span data-ttu-id="1b362-109">Az aktív csatlakozó áthelyezi a csoport</span><span class="sxs-lookup"><span data-stu-id="1b362-109">Move an active Connector into the group</span></span>

-   <span data-ttu-id="1b362-110">Ha nincs aktív összekötők a csoportban található, akkor a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="1b362-110">If you have no active connectors in the group, you can:</span></span>

    -   <span data-ttu-id="1b362-111">Az összekötő nem aktív ok azonosítása és elhárítása</span><span class="sxs-lookup"><span data-stu-id="1b362-111">Identify the reason your Connector is inactive and resolve</span></span>

    -   <span data-ttu-id="1b362-112">Az aktív csatlakozó áthelyezi a csoport</span><span class="sxs-lookup"><span data-stu-id="1b362-112">Move an active Connector into the group</span></span>

<span data-ttu-id="1b362-113">Tudjuk, hogy ezek közül a probléma, az alkalmazásban a "Alkalmazásproxy" menü megnyitásához, és nézze meg az összekötő csoport figyelmeztető üzenet.</span><span class="sxs-lookup"><span data-stu-id="1b362-113">To know which of these is the issue, open the “Application Proxy” menu in your Application, and look at the Connector Group warning message.</span></span> <span data-ttu-id="1b362-114">Azt adja meg, hogy a csoport kell legalább egy összekötő (rendelkezik a csoport nincs), vagy nem aktív összekötők rendelkezik (bár valószínűleg inaktív összekötők).</span><span class="sxs-lookup"><span data-stu-id="1b362-114">It specify either that the group needs at least one Connector (you have none in the group) or that it has no active Connectors (though you likely have inactive Connectors).</span></span>

   ![Az Azure portálon összekötő csoport kijelölése](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

<span data-ttu-id="1b362-116">Ezen lehetőségek a részletekért lásd: az alábbi megfelelő szakaszához.</span><span class="sxs-lookup"><span data-stu-id="1b362-116">For details on each of these options, see the corresponding section below.</span></span> <span data-ttu-id="1b362-117">Ezek mindegyikének azt feltételezi, hogy az összekötő felügyeleti oldalról indítja.</span><span class="sxs-lookup"><span data-stu-id="1b362-117">Each of these assumes that you are starting from the Connector management page.</span></span> <span data-ttu-id="1b362-118">Ha a fenti hibaüzenet nézi, ezen a lapon elvégezheti a figyelmeztető üzenet kattintva.</span><span class="sxs-lookup"><span data-stu-id="1b362-118">If you are looking at the error message above, you can go to this page by clicking on the warning message.</span></span> <span data-ttu-id="1b362-119">Ellenkező esetben ez található címen **Azure Active Directory**a gombra, majd **vállalati alkalmazások**, majd **alkalmazásproxy.**</span><span class="sxs-lookup"><span data-stu-id="1b362-119">Otherwise this can be found by going to **Azure Active Directory**, clicking on **Enterprise Applications**, then **Application Proxy.**</span></span>

   ![Összekötő csoportok kezelése az Azure portálon](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a><span data-ttu-id="1b362-121">Új összekötő letöltése</span><span class="sxs-lookup"><span data-stu-id="1b362-121">Download a new Connector</span></span>

<span data-ttu-id="1b362-122">Új összekötő letöltése, használja a "-összekötő letöltése" gombra az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="1b362-122">To download a new Connector, use the “Download Connector” button at the top of the page.</span></span>

<span data-ttu-id="1b362-123">Megjegyzés: az összekötő a háttéralkalmazás közvetlen sor a láthatáron a gépen kell telepíteni, és általában helyezkedik el ugyanazon a kiszolgálón, megegyezik az alkalmazáséval.</span><span class="sxs-lookup"><span data-stu-id="1b362-123">note the Connector needs to be installed on a machine with direct line of sight to the backend application, and is typically placed on the same server as the application.</span></span> <span data-ttu-id="1b362-124">Az összekötő a letöltés után meg kell jelennie az ebben a menüben.</span><span class="sxs-lookup"><span data-stu-id="1b362-124">After downloading, the Connector should appear in this menu.</span></span> <span data-ttu-id="1b362-125">Kattintson az összekötő, és a "összekötő csoport" legördülő győződjön meg arról, hogy a megfelelő csoporthoz tartozik.</span><span class="sxs-lookup"><span data-stu-id="1b362-125">click the Connector, and use the “Connector Group” drop-down to make sure it belongs to the right group.</span></span> <span data-ttu-id="1b362-126">A módosítás mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="1b362-126">Save the change.</span></span>

   ![Az összekötő letöltése az Azure portálról](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a><span data-ttu-id="1b362-128">Helyezze át egy aktív összekötő</span><span class="sxs-lookup"><span data-stu-id="1b362-128">Move an Active Connector</span></span>

<span data-ttu-id="1b362-129">Ha az aktív csatlakozó, a csoporthoz kell tartoznia, és a háttér célalkalmazásnak történő sor a láthatáron, áthelyezheti az összekötő a hozzárendelt csoportjához.</span><span class="sxs-lookup"><span data-stu-id="1b362-129">If you have an active Connector that should belong to the group and has line of sight to the target backend application, you can move the Connector into the assigned group.</span></span> <span data-ttu-id="1b362-130">Ehhez kattintson az összekötőt.</span><span class="sxs-lookup"><span data-stu-id="1b362-130">To do so, click the Connector.</span></span> <span data-ttu-id="1b362-131">"Összekötő csoport" mezőben használja a legördülő listán válassza ki a megfelelő csoportot, és kattintson a Mentés gombra.</span><span class="sxs-lookup"><span data-stu-id="1b362-131">In the “Connector Group” field, use the drop-down to select the correct group, and click Save.</span></span>

## <a name="resolve-an-inactive-connector"></a><span data-ttu-id="1b362-132">Hárítsa el az inaktív csatlakozó</span><span class="sxs-lookup"><span data-stu-id="1b362-132">Resolve an inactive Connector</span></span>

<span data-ttu-id="1b362-133">Ha a csoportban csak összekötők nem működnek, valószínűleg olyan gépen, amely nem rendelkezik a szükséges portok feloldva.</span><span class="sxs-lookup"><span data-stu-id="1b362-133">If the only Connectors in the group are inactive, they are likely on a machine that does not have all the necessary ports unblocked.</span></span>

<span data-ttu-id="1b362-134">a portok kapcsolatos problémák elhárítása a dokumentum a részletekért tekintse meg a probléma kivizsgálása.</span><span class="sxs-lookup"><span data-stu-id="1b362-134">see the ports Troubleshoot document for details on investigating this problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b362-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b362-135">Next steps</span></span>
[<span data-ttu-id="1b362-136">Az Azure AD-alkalmazásproxy összekötők ismertetése</span><span class="sxs-lookup"><span data-stu-id="1b362-136">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)


