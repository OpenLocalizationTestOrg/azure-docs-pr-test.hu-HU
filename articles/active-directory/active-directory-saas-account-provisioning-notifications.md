---
title: "Fiók kiépítése értesítések |} Microsoft Docs"
description: "Győződjön meg arról, hogy értesítést kap a felhasználók átadása kapcsolatos problémák, fiók kiépítése értesítések engedélyezésével figyelmet igénylő útmutató."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.openlocfilehash: 16ec2b320f733d8828b0046c20f7f7b0e341daf9
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/16/2018
---
# <a name="account-provisioning-notifications"></a>Alkalmazáskiépítési értesítések
A felhasználók átadása, harmadik féltől származó SaaS-alkalmazások a felhasználók kezelése folyamata automatizálható. <br>
Ez nem egy automatikus folyamat, és ezt a folyamatot a közötti interakció időről időre szükség. <br>
Ez helyzet, például az, ha a fiók már konfigurálta az exchange-adatok és a harmadik féltől származó SaaS-alkalmazás lejárt. 

Azáltal, hogy a fiók kiépítése értesítések, biztosíthatja, hogy értesítést kap a felhasználók átadása kapcsolatos problémák, figyelmet igénylő.

Aktiválása vagy inaktiválása fiók értesítések kiépítése a felhasználók egy harmadik féltől származó SaaS-alkalmazás konfigurációja átadásához részeként.

![A felhasználók átadása][1] 

Kiépítés értesítések fiók aktiválásához kapcsolódó jelölőnégyzet bejelölésével a a **megerősítő** párbeszédpanel lapra, majd írja be az e-mail aliast a címzett.

![Alkalmazáskiépítési értesítések][2]

Megadhat terjesztési lista címzettjeként; Fontos azonban vegye figyelembe, hogy az értesítő e-mailt, amelyek csak az Azure AD-rendszergazdák által elérhető jelentések mutató hivatkozásokat tartalmaz.

Ha engedélyezett értesítések kiépítés fiók, e-mailt, amely kapcsolódik a felhasználók átadása kritikus fontosságú problémákkal kapcsolatos fog kapni. Azonban e-mailek túlterhelés elkerülése érdekében csak kapni fog egy e-mailben értesítést naponta minden Szolgáltatottszoftver-alkalmazáshoz az értesítő e-mailt engedélyezve van.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Felhasználói létesítési vagy megszüntetési SaaS-alkalmazásokhoz való automatizálásához](active-directory-saas-app-provisioning.md)
* [A felhasználók átadása attribútum-leképezésekhez testreszabása](active-directory-saas-customizing-attribute-mappings.md)
* [Attribútum-leképezésekhez kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez](active-directory-saas-scoping-filters.md)
* [SCIM használata a felhasználók és csoportok automatikus üzembe helyezésének engedélyezéséhez az Azure Active Directoryból az alkalmazásokba](active-directory-scim-provisioning.md)
* [SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
