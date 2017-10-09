---
title: "Távoli asztal a Azure AD alkalmazás-Proxy aaaPublish |} Microsoft Docs"
description: "Az Azure AD-alkalmazásproxy összekötők hello alapjairól ismerteti."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: kgremban
ms.custom: it-pro
ms.reviewer: harshja
ms.openlocfilehash: 1174161d0b5ef1157c334970f00ef4f0702a9545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Az Azure AD alkalmazásproxy távoli asztal közzététele

Ez a cikk ismerteti hogyan toodeploy távoli asztali szolgáltatások (RDS) a Proxy, hogy a távoli felhasználók továbbra is lehetnek hatékonyan dolgozhatnak.

hello készült célközönségét a cikkben:
- Aktuális Azure AD alkalmazásproxy felhasználóknak, akik toooffer további alkalmazások tootheir végfelhasználók által a távoli asztali szolgáltatások használatával a helyszíni alkalmazások közzététele.
- Aktuális távoli asztali szolgáltatások felhasználói tooreduce hello támadási felületét, illetve azok központi telepítésének szeretné, hogy az Azure AD-alkalmazásproxy használatával. Ebben a forgatókönyvben a kétlépéses ellenőrzést korlátozott számú biztosít, és a feltételes hozzáférés-vezérlés tooRDS.

## <a name="how-application-proxy-fits-in-hello-standard-rds-deployment"></a>Hogyan alkalmazásproxy beleilleszkedik hello normál távoli asztali szolgáltatások telepítése

A szabványos távoli asztali szolgáltatások telepítése különböző távoli asztal szerepkör-szolgáltatások a Windows Server rendszert futtató tartalmazza. Hello megnézi [távoli asztali szolgáltatások architektúrája](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture), több központi telepítési lehetőség áll rendelkezésre. hello leginkább szembetűnő különbségének hello [távoli asztali szolgáltatások telepítése az Azure AD alkalmazásproxy](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (hello a következő ábrán látható), és hello egyéb telepítési lehetőségekért hello alkalmazásproxy forgatókönyv kimenő állandó rendelkezik hello összekötő szolgáltatást futtató hello kiszolgáló közötti kapcsolat. Más központi hagyja nyitva bejövő kapcsolatok terheléselosztó keresztül.

![Alkalmazás Proxy található közötti távoli asztali szolgáltatások VM hello és nyilvános internet hello](./media/application-proxy-publish-remote-desktop/rds-with-app-proxy.png)

Egy távoli asztali szolgáltatások hello távoli asztali webes szerepkör és a távoli asztali átjáró szerepkör hello futnak internetre gépeken. Ezeket a végpontokat a következő okok miatt hello érhetők el:
- Távoli asztali webes biztosít hello felhasználói egy nyilvános végpontot toosign a, és a nézet hello különböző a helyszíni alkalmazások és asztali számítógépek eléréséhez. Erőforrás választja, akkor egy RDP-kapcsolat jön létre, az operációs rendszer hello található hello natív alkalmazás használatával.
- Távoli asztali átjáró hello képbe elérhető lesz, ha egy felhasználó elindítja hello RDP-kapcsolatokat. hello távoli asztali átjáró Titkosított hello hello interneten keresztül érkező RDP-forgalmát kezeli, és csatlakozik a helyi kiszolgáló, amely a felhasználó hello toohello fordítja le azt. Ebben a forgatókönyvben a távoli asztali átjáró fogadja hello forgalom hello hello Azure AD alkalmazásproxy származik.

>[!TIP]
>Ha még nem telepített távoli asztali szolgáltatások előtt, vagy megkezdése előtt további információra van szüksége, megtudhatja, hogyan túl[zökkenőmentesen telepíteni a távoli asztali szolgáltatások az Azure Resource Manager és az Azure piactér](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure).

## <a name="requirements"></a>Követelmények

- Mindkét hello távoli asztali webes és a végpontokat kell lennie a távoli asztali átjáró hello ugyanaz a számítógép, és egy közös gyökérközzétevővel. Távoli asztali webes és a távoli asztali átjáró teszi közzé, egyetlen alkalmazásként, tehát az egy egyszeri bejelentkezéses felhasználói élmény biztosítása hello a két alkalmazás között.

- Ön már rendelkezik [központilag telepített távoli asztali szolgáltatások](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure), és [engedélyezve van az alkalmazásproxy](active-directory-application-proxy-enable.md).

- Ebben a forgatókönyvben azt feltételezi, hogy a végfelhasználók a Windows 7 vagy Windows 10-asztalok hello távoli asztali weblapon keresztül csatlakozó ki halad át az Internet Explorer. Ha szüksége toosupport más operációs rendszerekkel, tekintse meg [más ügyfél-konfigurációs támogatása](#support-for-other-client-configurations).

  >[!NOTE]
  >Windows 10 létrehozó frissítése jelenleg nem támogatott.

- Az Internet Explorer engedélyezze a hello távoli asztali szolgáltatások ActiveX bővítmény.

## <a name="deploy-hello-joint-rds-and-application-proxy-scenario"></a>Hello közös távoli asztali szolgáltatások és a Proxy forgatókönyv telepítése

Miután beállította a távoli asztali szolgáltatások és a környezet az Azure AD alkalmazásproxy, kövesse a hello lépéseket toocombine hello két megoldásokat. Ezeket a lépéseket ismerteti alkalmazásokként hello két webes elérhető távoli asztali szolgáltatások végpontot (a távoli asztali webes és a távoli asztali átjáró) közzététele, és majd irányítja a forgalmat a proxyn keresztül történő a távoli asztali szolgáltatások toogo keresztül.

### <a name="publish-hello-rd-host-endpoint"></a>Hello távoli asztali állomás végpont közzététele

1. [Alkalmazásproxy új alkalmazás közzététele](application-proxy-publish-azure-portal.md) a hello a következő értékeket:
   - Belső URL-címe: https://\<rdhost\>.com /, ahol \<rdhost\> hello közös gyökér, amely a távoli asztali webes és a távoli asztali átjáró.
   - Külső URL-címe: Ez a mező automatikusan feltöltődik értékkel hello alkalmazás hello neve alapján, de módosíthatja azt. A felhasználók toothis URL-cím kerül RDS elérésekor
   - Előhitelesítési módszer: az Azure Active Directory
   - URL-cím fejlécek lefordítani: nincs
2. Felhasználók hozzárendelése toohello közzétett távoli asztali alkalmazás. Ellenőrizze, hogy minden rendelkeznek-e hozzáférési tooRDS túl.
3. Hello egyszeri bejelentkezés módszer hello alkalmazásra, amely hagyja **az Azure AD az egyszeri bejelentkezés le van tiltva**. A felhasználó felkérést kap tooauthenticate után tooAzure AD, és egyszer tooRD webes, de rendelkezik egyszeri bejelentkezéshez tooRD átjáró.
4. Nyissa meg túl**Azure Active Directory** > **App regisztrációk** > *az alkalmazás* > **Beállítások**.
5. Válassza ki **tulajdonságok** és frissítés hello **kezdőlapot URL-cím** mező toopoint tooyour távoli asztali webes végpontjának (például a https://\<rdhost\>.com/RDWeb).

### <a name="direct-rds-traffic-tooapplication-proxy"></a>Közvetlen távoli asztali szolgáltatások forgalom tooApplication Proxy

Csatlakozás toohello távoli asztali szolgáltatások környezetbe rendszergazdaként, és módosítsa a hello távoli asztali átjárókiszolgáló neve hello központi telepítéshez. Ez biztosítja, hogy a kapcsolatok végighaladnia hello Azure AD alkalmazásproxy.

1. Csatlakozás toohello RDS kiszolgáló hello távoli asztali Kapcsolatszervező szerepkör futtatásához.
2. Indítsa el **Kiszolgálókezelő**.
3. Válassza ki **távoli asztali szolgáltatások** hello hello bal oldali ablaktábláján.
4. Válassza ki **áttekintése**.
5. Hello telepítési áttekintés szakaszban, a hello legördülő menüben válassza ki, és válassza a **központi telepítési tulajdonságok szerkesztése**.
6. Hello távoli asztali átjáró lapon módosítsa hello **kiszolgálónév** mező toohello külső URL-címet, amely a távoli asztali hello állomás végpont az alkalmazásproxy.
7. Változás hello **bejelentkezési használata** túl mezőben**jelszó-hitelesítés**.

  ![A távoli asztali szolgáltatások telepítési tulajdonságok képernyő](./media/application-proxy-publish-remote-desktop/rds-deployment-properties.png)

8. Minden gyűjtemény, futtassa a következő parancs hello. Cserélje le  *\<yourcollectionname\>*  és  *\<proxyfrontendurl\>*  a saját adataival. Ez a parancs lehetővé teszi, hogy az egyszeri bejelentkezés a távoli asztali webes és a távoli asztali átjáró között, és optimalizálja a teljesítmény:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl>`nrequire pre-authentication:i:1"
   ```

   **Példa:**
   ```
   Set-RDSessionCollectionConfiguration -CollectionName "QuickSessionCollection" -CustomRdpProperty "pre-authentication server address:s:https://gateway.contoso.msappproxy.net/`nrequire pre-authentication:i:1"
   ```

9. tooverify hello módosítása hello egyéni RDP tulajdonságait, valamint a hello RDP fájl tartalmának megjelenítése, amely ehhez a gyűjteményhez, futtassa a következő parancs hello RDWeb portálról letöltve:
    ```
    (get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
    ```

Most, hogy a távoli asztal konfigurálását, az Azure AD-alkalmazásproxy hello RDS részeként internetre átvette Eltávolíthatja a távoli asztali webes és a távoli asztali átjáró gépek más nyilvános internet felé néző végpontok hello.

## <a name="test-hello-scenario"></a>Hello tesztkörnyezet

Tesztelje az Internet Explorer, a Windows 7 vagy 10 számítógépen hello forgatókönyvet.

1. Nyissa meg toohello külső URL-címet állít be, vagy az alkalmazás található hello [MyApps panel](https://myapps.microsoft.com).
2. Az Active Directory tooauthenticate tooAzure kell adnia. Használjon toohello alkalmazás rendelt fiókot.
3. Webes tooauthenticate tooRD kell adnia.
4. Amikor a távoli asztali szolgáltatások hitelesítés sikeres, választhatja a hello asztali vagy alkalmazást, és megkezdheti a munkát.

## <a name="support-for-other-client-configurations"></a>Az egyéb ügyfél-konfiguráció támogatása

a cikkben ismertetett hello konfiguráció esetén a Windows 7 vagy 10, az Internet Explorer és a távoli asztali szolgáltatások ActiveX-bővítmény hello felhasználók. Ha szeretné, azonban támogathatja a más operációs rendszerek és a böngészők. hello különbség a hello hitelesítési módszere.

| Hitelesítési módszer | Támogatott ügyfél-konfigurációja |
| --------------------- | ------------------------------ |
| Előhitelesítés során    | Windows 7/10 Internet Explorer + a távoli asztali szolgáltatások ActiveX bővítmény használatával |
| PASSTHROUGH | Bármely más operációs rendszer, amely támogatja a Microsoft távoli asztal alkalmazás hello |

hello előhitelesítési folyamat előnyökkel további biztonsági mint hello áthaladás folyamata. Az előhitelesítés a helyszíni erőforrások kihasználhatja az Azure AD hitelesítési szolgáltatások, mint az egyszeri bejelentkezés, a feltételes hozzáférés és a kétlépéses ellenőrzést. Akkor is győződjön meg arról, hogy csak hitelesített forgalom éri el a hálózaton.

toouse hitelesítést, hogy csak két módosítások toohello lépéseket ebben a cikkben felsorolt:
1. A [hello távoli asztali állomás végpont közzététele](#publish-the-rd-host-endpoint) 1. lépés:, állítsa be a hello előhitelesítési módszer túl**csatlakoztatott**.
2. A [közvetlen távoli asztali szolgáltatások forgalom tooApplication Proxy](#direct-rds-traffic-to-application-proxy), hagyja ki teljesen a 8. lépés.

## <a name="next-steps"></a>Következő lépések

[Távelérés tooSharePoint az Azure AD alkalmazásproxy engedélyezése](application-proxy-enable-remote-access-sharepoint.md)  
[Biztonsági szempontok az alkalmazások az Azure AD-alkalmazásproxy használatával távelérése](application-proxy-security-considerations.md)
