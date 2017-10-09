---
title: "Azure AD alkalmazás Proxy alkalmazások csoportban aaaAccess |} Microsoft Docs"
description: "Használhatja az Azure AD-alkalmazásproxy tooaccess a helyszíni Microsoft Teams keresztül."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 13c36e43ae6349df09272e308ad4f40451cbbeb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Microsoft Teams keresztül férnek hozzá a helyszíni alkalmazások

Az Azure Active Directory Alkalmazásproxyjával lehetővé teszi az egyszeri bejelentkezés tooon helyszíni alkalmazások függetlenül attól, hogy hol áll, és a Microsoft Teams leegyszerűsíti a együttműködési próbálkozások egy helyen. Hello integrálása két együtt azt jelenti, hogy a felhasználók hatékony legyen a teammates minden helyzetben lehetnek. 

A felhasználók is hozzáadhat a felhőalapú alkalmazások tootheir csapatok csatornák [lapok használatával](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), de mi történik, ha a SharePoint-webhely és a tervezési eszköz, azok összes helyben tárolt? Alkalmazásproxy egy hello megoldás. Akkor is hozzáadhat alkalmazásokat alkalmazásproxy tootheir közzétett csatornák használatával hello azonos külső URL-címek mindig használata tooaccess alkalmazások távolról. És mivel alkalmazásproxy hitelesíti az Azure Active Directoryban, egyszeri bejelentkezéses felhasználói élmény ugyanaz hordoz magában, keresztül hello keresztül.


## <a name="install-hello-application-proxy-connector-and-publish-your-app"></a>Hello alkalmazásproxy-összekötő telepítése és az alkalmazás közzététele

Ha még nem tette, [-Proxy konfigurálása a bérlő számára, és hello összekötő telepítéséhez](active-directory-application-proxy-enable.md). Ezt követően [a helyszíni alkalmazások közzététele](application-proxy-publish-azure-portal.md) táveléréshez. Hello app közzétételekor most jegyezze fel a hello külső URL-cím, mert a végfelhasználók számára, hogy információra van szükség, amikor hello app tooTeams.

Ha már rendelkezik a közzétett alkalmazásokhoz, de nem emlékszik a külső URL-címeit, keresse meg őket hello [Azure-portálon](https://portal.azure.com). Jelentkezzen be, majd keresse meg a túl**Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás** > Válassza ki az alkalmazást > **Alkalmazásproxy**.

## <a name="add-your-app-tooteams"></a>Az alkalmazás tooTeams hozzáadása

Ha közzéteszi a proxyn keresztül történő hello app, tudassa a felhasználókkal, tudja, hogy azok is megadhatja azt a lapján közvetlenül a csapatok csatornák. Kövesse a fenti három lépést rendelkezik:

1. Keresse meg a toohello csoportok csatorna tooadd, ahová ezt az alkalmazást, és válassza ki  **+**  tooadd egy lapon.

   ![Válassza a lap](./media/application-proxy-teams/add-tab.png)

2. Válassza ki **webhely** hello lapon lehetőségek közül.

   ![Webhely hozzáadása](./media/application-proxy-teams/website.png)

3. Hello lapon adjon egy nevet, és hello URL-cím toohello proxyval külső URL-Címének beállítása. 

   ![Lap neve és az URL-cím konfigurálása](./media/application-proxy-teams/tab-name-url.png)

Után a csoport egyik tagjának hello lapon ad hozzá, mutatja a hello csatorna mindenki számára. Bármely access toohello alkalmazásnak rendelkező felhasználónál egyszeri bejelentkezéses hozzáférést Microsoft Teams használata hello hitelesítő adatokkal. Hozzáférés toohello app nem rendelkező felhasználók csoportban hello lapon megjelenik, de le vannak tiltva, és az engedélyek toohello helyszíni app számukra hello hello alkalmazás az Azure portálon közzétett verziója. 

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, hogyan túl[tegye közzé a helyszíni SharePoint helyeket](application-proxy-enable-remote-access-sharepoint.md) az alkalmazásproxy.
- Konfigurálja az alkalmazások toouse [egyéni tartományok](active-directory-application-proxy-custom-domains.md) a külső URL-címhez. 
