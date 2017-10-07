---
title: "az Azure AD alkalmazásproxy aaaPublish alkalmazások |} Microsoft Docs"
description: "Tegye közzé a helyszíni alkalmazások toohello felhőalapú Azure AD-alkalmazásproxyval hello a klasszikus portálon."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 7926998314c65521ae48aebcceb33cb0c67e0b87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Alkalmazások közzététele az Azure AD-alkalmazásproxyval

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [klasszikus Azure portál](active-directory-application-proxy-publish.md)

Az Azure AD-alkalmazásproxy segít a távoli dolgozók támogatja a helyszíni alkalmazások toobe hello keresztül elérhető közzétételével internet. A pont, akkor már rendelkezik [alkalmazásproxy engedélyezve a klasszikus Azure portálon hello](active-directory-application-proxy-enable.md). Ez a cikk bemutatja, hogyan hello lépéseket toopublish alkalmazást, hogy a helyi hálózaton keresztül, és adja meg a biztonságos távoli hozzáférés a a hálózaton kívülről. Ez a cikk befejezése után készen áll a tooconfigure hello alkalmazás személyre szabott adatok vagy a biztonsági követelmények lesz.

> [!NOTE]
> Alkalmazásproxy rendszerben elérhető, csak akkor, ha frissített toohello prémium vagy alapszintű kiadására az Azure Active Directory szolgáltatás. További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások). Ha azt szeretné, hogy toouse proxyval, akkor [alkalmazások közzététele az Azure-portálon hello](application-proxy-publish-azure-portal.md).

## <a name="publish-an-app-using-hello-wizard"></a>Hello varázsló használó alkalmazások közzététele
1. Jelentkezzen be rendszergazdaként a hello [a klasszikus Azure portálon](https://manage.windowsazure.com/).
2. Nyissa meg a könyvtár tooActive, és válassza a hello könyvtárban, ahol engedélyezve van az alkalmazásproxy.
   
    ![Active Directory – ikon](./media/active-directory-application-proxy-publish/ad_icon.png)
3. Hello kattintson **alkalmazások** fülre, majd hello **Hozzáadás** üdvözlő képernyőt hello alján gomb
   
    ![Alkalmazás hozzáadása](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)
4. Válassza a **Publish an application that will be accessible from outside your network** (Hálózaton kívülről hozzáférhető alkalmazás közzététele) lehetőséget.
   
    ![Hálózaton kívülről hozzáférhető alkalmazás közzététele](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)
5. Adja meg a következő információkat az alkalmazásról hello:
   
   * **Név**: hello felhasználóbarát nevet az alkalmazásnak. Ennek a címtáron belül egyedinek kell lennie.
   * **Belső URL-cím**: alkalmazásproxy-összekötő hello hello címtől tooaccess hello alkalmazás eltávolítása a magánhálózaton belül használja. Megadott elérési útra hello háttér server toopublish biztosíthat, amíg hello rest hello kiszolgáló nem lesz közzétéve. Ezzel a módszerrel közzéteheti a különböző helyek hello ugyanarra a kiszolgálóra, és mindegyiknek saját nevet és hozzáférési szabályokat.
     
     > [!TIP]
     > Ha közzéteszi a egy elérési utat, győződjön meg arról, hogy minden hello szükséges lemezképek, parancsprogramok és stíluslapok, az alkalmazás tartalmaz. Például ha az alkalmazás https://yourapp/app, illetve lemezkép https://yourapp/media helyen, majd kell közzé tenni https://yourapp/ hello elérési útjával.
     > 
     > 
   * **Előhitelesítési módszer**: hogyan alkalmazásproxy access tooyour alkalmazás engedélyezése előtt ellenőrzi a felhasználót. Hello legördülő menüből válassza az hello lehetőségek egyikét.
     
     * Az Azure Active Directory: Alkalmazásproxy átirányítja a felhasználók toosign hitelesíti a rájuk vonatkozó engedélyek hello címtár és az alkalmazás az Azure AD-be.
     * Átengedés: A felhasználóknak nem kell tooauthenticate tooaccess hello alkalmazás.
     
     ![Az alkalmazás tulajdonságai](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  
6. toofinish hello varázsló, kattintson a pipa hello üdvözlő képernyőt hello alján. hello alkalmazás már meg van adva az Azure ad-ben.

## <a name="assign-users-and-groups-toohello-application"></a>Felhasználók és csoportok hozzárendelése toohello alkalmazás
A sorrend a felhasználók tooaccess a közzétett alkalmazáshoz, tooassign kell őket egyenként vagy csoportokat. (Ne feledje tooassign saját kezűleg fér hozzá, túl.) Minden hozzárendelt felhasználónak rendelkeznie kell egy Azure Basic vagy magasabb szintű licenccel. Külön-külön is hozzárendelhet licenceket vagy toogroups. További információkért lásd: [felhasználók tooan alkalmazás hozzárendelése](active-directory-applications-guiding-developers-assigning-users.md). 

Az előhitelesítést igénylő alkalmazások esetén engedély toouse hello alkalmazás hozzárendelése egy felhasználói biztosít. Az alkalmazások, amelyek nem igényelnek előhitelesítéssel hozzárendelése egy felhasználói azt jelenti, hogy hello felhasználó hello alkalmazást érheti el hello hozzáférési panel keresztül.

1. Után hello alkalmazás hozzáadása varázsló befejeződik lásd: hello gyors üzembe helyezési oldal az alkalmazáshoz. hozzáférés toohello app, aki toomanage kiválasztása **felhasználók és csoportok**.
   
    ![Felhasználók hozzárendelése az alkalmazásproxy első lépései oldalán – képernyőfelvétel](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)
2. Keressen rá a címtár adott csoportjára, vagy jelenítse meg az összes felhasználót. toodisplay hello keresési eredmények között kattintson hello pipára.
   
      ![Csoportok vagy felhasználók keresése – képernyőfelvétel](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)
3. Válassza ki a felhasználó vagy csoport, szeretné, hogy tooassign toothis alkalmazást, majd kattintson az **hozzárendelése**. Biztosan kéri tooconfirm ezt a műveletet.

> [!NOTE]
> Integrált Windows-hitelesítésű alkalmazások esetében kizárólag a helyszíni Active Directoryból szinkronizált felhasználók és csoportok rendelhetők hozzá az alkalmazáshoz. A Microsoft-fiókkal bejelentkezett felhasználók és a vendégek nem rendelhetők hozzá az Azure Active Directory alkalmazásproxyjával közzétett alkalmazásokhoz. Győződjön meg arról, hogy a felhasználók hello részét képező hitelesítő adatokkal jelentkezhetnek be közzétett hello alkalmazás azonos tartományban.
> 
> 

## <a name="test-your-published-application"></a>A közzétett alkalmazás tesztelése
Miután közzétette az alkalmazást, ha közzétett toohello URL-cím megnyitásával tesztelheti azt. Győződjön meg róla, hogy az alkalmazás elérhető, megfelelően jelenik meg, és minden a vártak szerint működik. Ha problémákat tapasztal, vagy egy hibaüzenet jelenik meg, próbálkozzon hello [hibaelhárítási útmutatója](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Az alkalmazás konfigurálása
Módosíthatja a közzétett alkalmazásokat, vagy speciális beállítások hello konfigurálása lapon. Ezen a lapon testre szabhatja az alkalmazás hello név módosításával vagy embléma feltöltésével. Például a előhitelesítési módszer vagy a multi-factor authentication hello hozzáférési szabályok is kezelheti.

![Speciális konfiguráció](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)

Alkalmazások közzététele után használata az Azure Active Directory Alkalmazásproxyjával történő hello alkalmazások listáját az Azure ad-ben jelennek, és kezelheti azokat van.

Ha letiltja az alkalmazásproxy szolgáltatásainak alkalmazások közzétételét követően, hello alkalmazások nem lesznek a magánhálózaton kívülről hozzáférhető. A felhasználók is továbbra is hozzáférési hello alkalmazásokhoz a helyszínen, a szokásos módon.

tooview egy alkalmazást, és győződjön meg arról, hogy az informatikai érhető el, kattintson duplán a hello alkalmazás hello nevét. Ha hello alkalmazásproxy-szolgáltatás le van tiltva, és hello alkalmazás nem áll rendelkezésre, üdvözlő képernyőt hello tetején megjelenik egy figyelmeztető üzenet.

egy alkalmazáskészletet, toodelete hello listán jelöljön ki egy alkalmazást, és kattintson a **törlése**.

## <a name="next-steps"></a>Következő lépések
* [Alkalmazások közzététele saját tartománynév használatával](active-directory-application-proxy-custom-domains.md)
* [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
* [Feltételes hozzáférés engedélyezése](active-directory-application-proxy-conditional-access.md)
* [Munkavégzés jogcímeket figyelembe vevő alkalmazásokkal](active-directory-application-proxy-claims-aware-apps.md)

Hello legfrissebb híreket és frissítéseket, tekintse meg a hello [alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/)

