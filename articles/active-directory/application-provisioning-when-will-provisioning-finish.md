---
title: "A felhasználók átadása egy Azure AD-katalógusában alkalmazás véve óra vagy több |} Microsoft Docs"
description: "Annak ellenőrzése, ezért az alkalmazás történő előfordulhat, hogy kell a vártnál tovább tart, mint amennyi várt"
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
ms.openlocfilehash: 183d60cbbbc8d02f7cd3cacc160453c45717ef0d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a><span data-ttu-id="8bd73-103">A felhasználók átadása egy Azure AD-katalógusában alkalmazás véve óra vagy több</span><span class="sxs-lookup"><span data-stu-id="8bd73-103">User provisioning to an Azure AD Gallery application is taking hours or more</span></span>

<span data-ttu-id="8bd73-104">Ha először engedélyezve van az alkalmazás automatikus kiépítés, a kezdeti szinkronizálás is igénybe vehet 20 percet az Azure AD-címtár és a felhasználók kialakítási hatókörében számát méretétől függően több órát.</span><span class="sxs-lookup"><span data-stu-id="8bd73-104">When first enabling automatic provisioning for an application, the initial sync can take anywhere from 20 minutes to several hours, depending on the size of the Azure AD directory and the number of users in scope for provisioning.</span></span> 

<span data-ttu-id="8bd73-105">A kezdeti szinkronizálás után az ezt követő szinkronizálások lehet gyorsabb, mint a létesítési szolgáltatás, amelyben a kezdeti szinkronizálást, és ezt követő szinkronizálások teljesítményének javítása után mindkét állapota egy vízjelek tárolja.</span><span class="sxs-lookup"><span data-stu-id="8bd73-105">Subsequent syncs after the initial sync be faster, as the provisioning service stores watermarks that represent the state of both systems after the initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-to-improve-provisioning-performance"></a><span data-ttu-id="8bd73-106">Üzembe helyezési teljesítményének javításával</span><span class="sxs-lookup"><span data-stu-id="8bd73-106">How to improve provisioning performance</span></span>

<span data-ttu-id="8bd73-107">A kezdeti szinkronizálás csak néhány óra tart, ha van egy művelet, amelyet teljesítmény javítása érdekében:</span><span class="sxs-lookup"><span data-stu-id="8bd73-107">If the initial sync is taking more than a few hours, there is one thing you can do to improve performance:</span></span>

-   <span data-ttu-id="8bd73-108">**Felhasználói tartalmazó szűrők.**</span><span class="sxs-lookup"><span data-stu-id="8bd73-108">**User scoping filters.**</span></span> <span data-ttu-id="8bd73-109">Hatókörként szűrők lehetővé teszik finom hangolási az adatokat, amelyek az Azure AD által meghatározott attribútumértékek alapján felhasználók kiszűrése bontja ki a létesítési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8bd73-109">Scoping filters allow you to fine tune the data that the provisioning service extracts from Azure AD by filtering out users based on specific attribute values.</span></span> <span data-ttu-id="8bd73-110">A helyezése hatókörszűrőkkel további információkért lásd: [alkalmazások Attribútumalapú üzembe helyezése hatókörszűrőkkel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span><span class="sxs-lookup"><span data-stu-id="8bd73-110">For more information on scoping filters, see [Attribute-based application provisioning with scoping filters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bd73-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8bd73-111">Next steps</span></span>
[<span data-ttu-id="8bd73-112">Felhasználói kiépítésének és megszüntetésének biztosítása SaaS-alkalmazásokhoz az Azure Active Directoryval történő automatizálásához</span><span class="sxs-lookup"><span data-stu-id="8bd73-112">Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)

