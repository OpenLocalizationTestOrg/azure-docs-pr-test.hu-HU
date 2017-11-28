---
title: "az Azure AD tooan gyűjtemény alkalmazás kiépítés aaaUser véve óra vagy több |} Microsoft Docs"
description: "Hogyan, miért tooyour alkalmazás is lehet tovább tart, mint amennyi toofind várt"
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
ms.openlocfilehash: 0658f041724c91ddd1997cc7759393b46680f13a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="user-provisioning-tooan-azure-ad-gallery-application-is-taking-hours-or-more"></a><span data-ttu-id="4dea1-103">Felhasználók átadásához tooan az Azure AD-gyűjtemény alkalmazás véve óra vagy több</span><span class="sxs-lookup"><span data-stu-id="4dea1-103">User provisioning tooan Azure AD Gallery application is taking hours or more</span></span>

<span data-ttu-id="4dea1-104">Ha először engedélyezve van az alkalmazás automatikus kiépítés, hello kezdeti szinkronizálás is igénybe vehet 20 perc tooseveral óra, hello Azure AD-címtár és a felhasználók kialakítási hatókörében hello száma hello méretétől függően.</span><span class="sxs-lookup"><span data-stu-id="4dea1-104">When first enabling automatic provisioning for an application, hello initial sync can take anywhere from 20 minutes tooseveral hours, depending on hello size of hello Azure AD directory and hello number of users in scope for provisioning.</span></span> 

<span data-ttu-id="4dea1-105">Ezt követő szinkronizálások hello kezdeti szinkronizálás után lehet gyorsabb, mint hello szolgáltatás kiépítését, amelyek megfelelnek a hello állapot mindkét rendszer hello a kezdeti szinkronizálás ezt követő szinkronizálások teljesítményének javítása után vízjelek tárolja.</span><span class="sxs-lookup"><span data-stu-id="4dea1-105">Subsequent syncs after hello initial sync be faster, as hello provisioning service stores watermarks that represent hello state of both systems after hello initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-tooimprove-provisioning-performance"></a><span data-ttu-id="4dea1-106">Hogyan tooimprove teljesítmény kiépítése</span><span class="sxs-lookup"><span data-stu-id="4dea1-106">How tooimprove provisioning performance</span></span>

<span data-ttu-id="4dea1-107">Ha csak néhány óra hello kezdeti szinkronizálás tovább tart, van egy művelet, amelyet tooimprove teljesítmény:</span><span class="sxs-lookup"><span data-stu-id="4dea1-107">If hello initial sync is taking more than a few hours, there is one thing you can do tooimprove performance:</span></span>

-   <span data-ttu-id="4dea1-108">**Felhasználói tartalmazó szűrők.**</span><span class="sxs-lookup"><span data-stu-id="4dea1-108">**User scoping filters.**</span></span> <span data-ttu-id="4dea1-109">Hatókörként szűrők toofine hangolási hello adatok, amelyek a létesítési szolgáltatás kivonatok Azure ad-felhasználókat adott attribútumértékek alapján kiszűrésével hello engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4dea1-109">Scoping filters allow you toofine tune hello data that hello provisioning service extracts from Azure AD by filtering out users based on specific attribute values.</span></span> <span data-ttu-id="4dea1-110">A helyezése hatókörszűrőkkel további információkért lásd: [alkalmazások Attribútumalapú üzembe helyezése hatókörszűrőkkel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span><span class="sxs-lookup"><span data-stu-id="4dea1-110">For more information on scoping filters, see [Attribute-based application provisioning with scoping filters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4dea1-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4dea1-111">Next steps</span></span>
[<span data-ttu-id="4dea1-112">Felhasználói kiépítés és megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval automatizálásához</span><span class="sxs-lookup"><span data-stu-id="4dea1-112">Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)

