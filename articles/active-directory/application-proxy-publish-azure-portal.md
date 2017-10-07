---
title: "az Azure AD alkalmazásproxy aaaPublish alkalmazások |} Microsoft Docs"
description: "A helyszíni alkalmazások toohello felhőalapú Azure AD-alkalmazásproxyval közzététele a hello Azure-portálon."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ed5458467fb7d4376f65a222f1ba5f23cfdfc57c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Alkalmazások közzététele az Azure AD-alkalmazásproxyval

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [klasszikus Azure portál](active-directory-application-proxy-publish.md)

Azure Active Directory (AD) alkalmazásproxy segít a távoli dolgozók támogatja a helyszíni alkalmazások toobe hello keresztül elérhető közzétételével internet. Ezeket az alkalmazásokat hello Azure portál tooprovide biztonságos távoli hozzáférés a keresztül közzéteheti a hálózaton kívülről.

Ez a cikk bemutatja, hogyan hello lépéseket toopublish az alkalmazásproxy a helyszíni alkalmazások. Ez a cikk befejezése után fog-e a felhasználók kell tudni tooaccess az alkalmazás távolról. – Ekkor készen áll a tooconfigure különleges funkciókat hello alkalmazáshoz hasonlóan az egyszeri bejelentkezés, a személyre szabott adatok és a biztonsági követelményeket.

Ha új tooApplication Proxy, kapcsolatos további Ez a szolgáltatás hello cikk [hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások](active-directory-application-proxy-get-started.md).


## <a name="publish-an-on-premises-app-for-remote-access"></a>A távoli hozzáférés a helyszíni alkalmazások közzététele

Kövesse ezeket a lépéseket toopublish az alkalmazásproxy alkalmazásait. Ha még nem már letöltötte és összekötő konfigurálva a szervezet számára, nyissa meg túl[az alkalmazásproxy első lépései és hello összekötő telepítéséhez](active-directory-application-proxy-enable.md) első, majd tegye közzé az alkalmazást.

> [!TIP]
> Ha a tesztelést kimenő alkalmazásproxy hello az első alkalommal, válasszon olyan alkalmazás, amely jelszóalapú hitelesítés van beállítva. Alkalmazásproxy más típusú hitelesítés támogatja, de jelszó-alapú alkalmazások hello legegyszerűbb tooget fel, akinek gyorsan. 

1. Jelentkezzen be rendszergazdaként a hello [Azure-portálon](https://portal.azure.com/).
2. Válassza ki **Azure Active Directory** > **vállalati alkalmazások** > **új alkalmazás**.

  ![Vállalati alkalmazás hozzáadása](./media/application-proxy-publish-azure-portal/add-app.png)

3. Válassza ki **összes**, majd jelölje be **helyszíni alkalmazás**.  

  ![Saját alkalmazás felvétele](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Adja meg a következő információkat az alkalmazásról hello:

   - **Név**: hello alkalmazás, amely megjelenik majd hello hozzáférési panel és hello Azure-portálon a hello nevét. 

   - **Belső URL-cím**: hello URL-cím a magánhálózaton belül használt tooaccess hello alkalmazást. Megadott elérési útra hello háttér server toopublish biztosíthat, amíg hello rest hello kiszolgáló nem lesz közzétéve. Ezzel a módszerrel közzéteheti a különböző helyek hello a különböző alkalmazások ugyanarra a kiszolgálóra, és mindegyiknek saját nevet és hozzáférési szabályokat.

     > [!TIP]
     > Ha közzéteszi a egy elérési utat, győződjön meg arról, hogy minden hello szükséges lemezképek, parancsprogramok és stíluslapok, az alkalmazás tartalmaz. Például ha az alkalmazás https://yourapp/app, illetve lemezkép https://yourapp/media helyen, majd kell közzé tenni https://yourapp/ hello elérési útjával. A belső URL-cím nincs toobe hello kezdőlapja a felhasználók látni. További információkért lásd: [állítsa be a közzétett alkalmazások egyéni kezdőlapját](application-proxy-office365-app-launcher.md).

   - **Külső URL-cím**: hello cím kerül, hogy a felhasználók tooin rendelés tooaccess hello alkalmazást, a hálózaton kívülről. Ha nem szeretné toouse hello alapértelmezett alkalmazásproxy-tartományt, olvassa el [egyéni tartományok az Azure AD alkalmazásproxy](active-directory-application-proxy-custom-domains.md).
   - **Előzetes hitelesítésének**: hogyan alkalmazásproxy access tooyour alkalmazás engedélyezése előtt ellenőrzi a felhasználót. 

     - Az Azure Active Directory: Alkalmazásproxy átirányítja a felhasználók toosign hitelesíti a rájuk vonatkozó engedélyek hello címtár és az alkalmazás az Azure AD-be. Azt javasoljuk, hogy ez a beállítás hello alapértelmezett, így például a feltételes hozzáférés és a többtényezős hitelesítés az Azure AD biztonsági funkciók előnyeit élvezheti.
     - Átengedés: A felhasználóknak nem kell tooauthenticate Azure Active Directory tooaccess hello alkalmazás ellen. Hitelesítési követelmények hello háttérkiszolgálón továbbra is állíthat be.
   - **Összekötő csoport**: összekötők folyamat hello távelérési tooyour alkalmazás és az összekötő csoportok megkönnyítik, összekötők és alkalmazások régió, hálózati vagy célja. Ha nincs még létrehozva összekötő csoportok, az alkalmazás túl van-e hozzárendelve**alapértelmezett**.

   ![Az alkalmazás konfigurálása](./media/application-proxy-publish-azure-portal/configure-app.png)
5. Szükség esetén további beállításokat. A legtöbb alkalmazás esetén kell venni ezeket a beállításokat az alapértelmezett állapotra. 
   - **Háttér-alkalmazás időtúllépés**: az érték túl**hosszú** csak akkor, ha az alkalmazás tooauthenticate lassú, és csatlakozzon. 
   - **Fejlécek URL-címek fordítása**: ezt az értéket tartsa **Igen** kivéve, ha az alkalmazás szükséges hello eredeti szereplő állomás fejlécével hello hitelesítési kérelmet.
   - **A kérelem törzsében URL-címek fordítása**: tartani ezt az értéket **nem** kivéve, ha szoftveresen kötött HTML hivatkozások tooother helyszíni alkalmazásokkal rendelkezik, és ne használjon egyéni tartományokat. További információkért lásd: [hivatkozásra az alkalmazásproxy fordítási](application-proxy-link-translation.md).
   
   ![Az alkalmazás konfigurálása](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. Válassza a **Hozzáadás** lehetőséget.


## <a name="add-a-test-user"></a>Teszt felhasználó hozzáadása 

az alkalmazás megfelelően, közzétett tootest teszt felhasználói fiók hozzáadása. Ellenőrizze, hogy ez a fiók rendelkezik-e engedélyek tooaccess hello alkalmazásából hello vállalati hálózaton belül.

1. Vissza a hello – első lépések panelen válassza ki a **rendelje hozzá a felhasználót tesztelési**.

  ![Rendelje hozzá a felhasználót teszteléshez](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Hello felhasználók és csoportok panelen, jelölje ki **Hozzáadás**.

  ![Egy felhasználó vagy csoport hozzáadása](./media/application-proxy-publish-azure-portal/add-user.png)

3. A hello Hozzáadás hozzárendelés panelen válassza ki a **felhasználók és csoportok** kattintson a kívánt tooadd hello fiók. 
4. Válassza ki **hozzárendelése**.

## <a name="test-your-published-app"></a>A közzétett alkalmazás tesztelése

A böngészőben nyissa meg a toohello külső URL-cím hello során konfigurált lépés közzététele. Hello kezdőképernyőn kell megjelennie, és képes toosign be hello teszt fiókkal akkor állítható be.

![A közzétett alkalmazás tesztelése](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Következő lépések
- [Töltse le az összekötők](active-directory-application-proxy-enable.md) és [összekötő csoportok létrehozása a](active-directory-application-proxy-connectors-azure-portal.md) toopublish alkalmazások külön hálózatok és helyek.

- [Egyszeri bejelentkezés beállítása](application-proxy-sso-azure-portal.md) az újonnan közzétett alkalmazások
