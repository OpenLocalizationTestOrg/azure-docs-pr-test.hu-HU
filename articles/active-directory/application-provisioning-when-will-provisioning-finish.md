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
# <a name="user-provisioning-tooan-azure-ad-gallery-application-is-taking-hours-or-more"></a>Felhasználók átadásához tooan az Azure AD-gyűjtemény alkalmazás véve óra vagy több

Ha először engedélyezve van az alkalmazás automatikus kiépítés, hello kezdeti szinkronizálás is igénybe vehet 20 perc tooseveral óra, hello Azure AD-címtár és a felhasználók kialakítási hatókörében hello száma hello méretétől függően. 

Ezt követő szinkronizálások hello kezdeti szinkronizálás után lehet gyorsabb, mint hello szolgáltatás kiépítését, amelyek megfelelnek a hello állapot mindkét rendszer hello a kezdeti szinkronizálás ezt követő szinkronizálások teljesítményének javítása után vízjelek tárolja.

## <a name="how-tooimprove-provisioning-performance"></a>Hogyan tooimprove teljesítmény kiépítése

Ha csak néhány óra hello kezdeti szinkronizálás tovább tart, van egy művelet, amelyet tooimprove teljesítmény:

-   **Felhasználói tartalmazó szűrők.** Hatókörként szűrők toofine hangolási hello adatok, amelyek a létesítési szolgáltatás kivonatok Azure ad-felhasználókat adott attribútumértékek alapján kiszűrésével hello engedélyezése. A helyezése hatókörszűrőkkel további információkért lásd: [alkalmazások Attribútumalapú üzembe helyezése hatókörszűrőkkel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Következő lépések
[Felhasználói kiépítés és megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval automatizálásához](active-directory-saas-app-provisioning.md)

