---
title: "Az Azure AD reporting API a klasszikus Azure AD portálon az első lépések |} Microsoft Docs"
description: "Ismerkedés az Azure Active Directory reporting API-val"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 5e98b660fe19bb8abebf1c3b996b6295a6c4e728
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api-on-the-azure-ad-classic-portal"></a>Az Azure Active Directory reporting API-val a klasszikus Azure AD portálon az első lépések
*Ez a témakör része a [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).*

Az Azure Active Directory tartalmazza a különféle jelentések. Ezen jelentések adatai nagyon hasznosak lehetnek az alkalmazások számára, például a SIEM rendszerekhez, a naplózáshoz és az üzleti intelligencia eszközökhöz. Az Azure AD-jelentéskészítés API-k REST-alapú API-kon keresztül biztosítják az adatok szoftveres hozzáférését. Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.

Ez a cikk azt ismerteti a kell Ismerkedés az Azure AD reporting API-k.
A következő szakaszban hogy a naplózás használatával kapcsolatos további részletekért és jelentkezzen be API-k. Minden más API-k esetében, tekintse meg a [az Azure AD-jelentéseket és events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) cikk.

Kérdéseit, vagy visszajelzést, lépjen kapcsolatba [AAD jelentéskészítési súgó](mailto:aadreportinghelp@microsoft.com).

## <a name="learning-map"></a>Tanulási térkép
1. **Készítse elő** -előtt tesztelheti az API-mintákat, végre kell hajtania a [Előfeltételek az Azure AD reporting API eléréséhez](active-directory-reporting-api-prerequisites.md).
2. **Fedezze fel** -jelentéskészítési API-k első benyomást beolvasása:
   
   * [A mintákat a ellenőrzési API használatával](active-directory-reporting-api-audit-samples.md) 
   * [A mintákat a bejelentkezési tevékenység jelentés API használatával](active-directory-reporting-api-sign-in-activity-samples.md)
3. **Testre szabhatja** -saját megoldás létrehozása: 
   
   * [A naplózási API-hivatkozás használata](active-directory-reporting-api-audit-reference.md) 
   * [A bejelentkezési tevékenység jelentéshivatkozás API használatával](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a>Következő lépések
Ha fejezetét láthatja az összes elérhető Azure AD Graph API végpontjainak útvonalon [https://graph.windows.net/tenant-name/reports/$ metaadatok? api-version = beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).

