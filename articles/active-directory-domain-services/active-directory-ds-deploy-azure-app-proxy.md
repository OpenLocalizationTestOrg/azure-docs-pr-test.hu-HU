---
title: "Az Azure Active Directory tartományi szolgáltatások: Az Azure Active Directory-proxyt üzembe helyezni |} Microsoft Docs"
description: "Az Azure AD-alkalmazásproxy használata a felügyelt tartományok Azure Active Directory tartományi szolgáltatások"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 4142111231d0256960d0c02d686d51533ba2171c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a>Központi telepítése az Azure AD-alkalmazásproxyval oldható meg az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz
Azure Active Directory (AD) alkalmazásproxy segít a távoli dolgozók támogatja a helyszíni alkalmazások toobe hello keresztül elérhető közzétételével internet. Az Azure AD tartományi szolgáltatásokat helyszíni tooAzure infrastruktúra-szolgáltatásokat futtató örökölt alkalmazások most növekedési-és-shift segítségével. Majd közzéteheti ezeket az alkalmazásokat hello Azure AD alkalmazásproxy, tooprovide biztonságos távoli hozzáférés toousers a szervezetében.

Ha új toohello az Azure AD-alkalmazásproxy, további ezzel a funkcióval kapcsolatban a következő cikket hello: [hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások](../active-directory/active-directory-application-proxy-get-started.md).


## <a name="before-you-begin"></a>Előkészületek
a cikkben szereplő tooperform hello feladatok lesz szüksége:

1. Egy érvényes **Azure-előfizetés**.
2. Egy **Azure AD-címtár** -vagy egy helyszíni címtár vagy egy csak felhőalapú directory szinkronizálva.
3. Egy **Azure AD alapszintű vagy Premium licenc** van szükség toouse hello Azure AD alkalmazásproxy.
4. **Azure AD tartományi szolgáltatások** hello Azure Active directory engedélyezni kell. Ha még nem tette meg, kövesse a hello ismertetett feladatok hello [első lépések útmutató](active-directory-ds-getting-started.md).

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a>1 - engedélyezése az Azure AD alkalmazásproxy feladatot az Azure AD-címtár
Hajtsa végre a következő lépéseket tooenable hello Azure AD-címtárát az Azure AD alkalmazásproxy hello.

1. Jelentkezzen be rendszergazdaként a hello [Azure-portálon](http://portal.azure.com).

2. Kattintson a **Azure Active Directory** toobring mentése hello directory áttekintése. Kattintson a **vállalati alkalmazások**.

    ![Válassza ki az Azure AD-címtár](./media/app-proxy/app-proxy-enable-start.png)
3. Kattintson a **alkalmazásproxy**. Ha nem rendelkezik az Azure AD alapszintű vagy Azure AD Premium előfizetéssel, lásd: egy beállítás tooenable egy próbaverzióra. Váltás **alkalmazásproxy engedélyezése?** túl**engedélyezése** kattintson **mentése**.

    ![Alkalmazás Proxy engedélyezése](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. toodownload hello összekötő, kattintson a hello **összekötő** gombra.

    ![Összekötő letöltése](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. Hello letöltési oldal, fogadja el a hello feltételeit és adatvédelmi szerződést, majd kattintson a hello **letöltése** gombra.

    ![Erősítse meg a letöltési](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-toodeploy-hello-azure-ad-application-proxy-connector"></a>2. feladat – rendelkezés tartományhoz csatlakoztatott Windows kiszolgálók toodeploy hello Azure AD alkalmazásproxy-összekötő
A tartományhoz csatlakoztatott Windows Server virtuális gépek is telepíthető, amely hello Azure AD alkalmazásproxy-összekötő van szüksége. Hello alkalmazások közzétételét, attól függően választhatja, hogy tooprovision több kiszolgáló, amelyen hello összekötő telepítve van. Központi telepítési lehetőséget ad nagyobb rendelkezésre állása, és segít nehezebb hitelesítési terhelés kezelésére.

Hello összekötő kiszolgálók ellátását a hello ugyanazt a virtuális hálózatot (vagy egy csatlakoztatott/társviszonyban virtuális hálózati), amelyhez engedélyezte az az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz. Ehhez hasonlóan hello alkalmazásproxy használatával tesz közzé üzemeltetéséhez hello hello kiszolgálók kell toobe hello telepítve azonos Azure virtuális hálózatban.

tooprovision összekötő-kiszolgálók, hajtsa végre a hello című cikkben leírt hello feladatok [tartományhoz egy Windows virtuális gép tooa felügyelt](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-3---install-and-register-hello-azure-ad-application-proxy-connector"></a>A feladat 3 - telepítése és regisztrálása hello Azure AD alkalmazásproxy-összekötő
Korábban a Windows Server virtuális gép kiépítése és tartományhoz csatlakoztatás toohello által kezelt tartomány. Ebben a feladatban telepíteni fogja az Azure AD alkalmazásproxy-összekötő hello a virtuális gépen.

1. Másolja a hello összekötő telepítési csomag toohello VM hello Azure AD-webalkalmazás-Proxy connector telepítse.

2. Futtatás **AADApplicationProxyConnectorInstaller.exe** hello virtuális gépen. Hello szoftver licencfeltételeit elfogadja.

    ![A telepítés feltételek elfogadása](./media/app-proxy/app-proxy-install-connector-terms.png)
3. A telepítés során felszólító tooregister hello hello Azure Active Directory-alkalmazásproxy-összekötő áll.
    * Adja meg a **az Azure AD globális rendszergazda hitelesítő adatait**. A globális rendszergazdai bérlő adatai eltérhetnek a Microsoft Azure rendszerre vonatkozó hitelesítő adatoktól.
    * hello rendszergazda használt fiók tooregister hello összekötő kell tartozniuk toohello ugyanabban a könyvtárban, ahol engedélyezte hello alkalmazásproxy-szolgáltatás. Például, ha hello bérlő tartománya a contoso.com, Üdvözöljük a rendszergazdákat legyen admin@contoso.com vagy más érvényes alias abban a tartományban.
    * Ha az Internet Explorer fokozott biztonsági beállítások be van kapcsolva a hello server hello összekötőt telepíti, blokkolhatja hello regisztrációs képernyő. tooallow hozzáférést, kövesse az utasításokat hello hello hibaüzenet. Győződjön meg arról, hogy az Internet Explorer fokozott biztonsági beállításai ki vannak kapcsolva.
    * Ha az összekötő regisztrálása meghiúsul, tekintse meg a [Troubleshoot Application Proxy](../active-directory/active-directory-application-proxy-troubleshoot.md) (Alkalmazásproxy hibaelhárítása) című anyagot.

    ![Összekötő telepítve](./media/app-proxy/app-proxy-connector-installed.png)
4. tooensure hello connector működik megfelelően, futtatási hello Azure AD Application Proxy Connector hibaelhárító. Meg kell jelennie egy sikeres jelentés futó hello hibaelhárító után.

    ![Hibaelhárító sikeres](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. Meg kell jelennie az Azure AD-címtár hello Application proxy lapján felsorolt újonnan telepített hello összekötő.

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> Úgy is dönthet, több kiszolgálón tooinstall összekötők tooguarantee a magas rendelkezésre állás, az Azure AD alkalmazásproxy hello nal közzétett alkalmazásokról. Hajtsa végre a hello tooinstall hello csatlakozó más kiszolgálók által kezelt tartomány illesztett tooyour a fent felsorolt lépéseket.
>
>

## <a name="next-steps"></a>Következő lépések
Az Azure AD-alkalmazásproxy hello beállítása, és azt az Azure AD tartományi szolgáltatások által kezelt tartomány integrálva.

* **Az alkalmazások tooAzure virtuális gépek áttelepítése:** növekedési és shift a tartományból a helyszíni kiszolgálók tooAzure virtuális gépek illesztett tooyour felügyelt alkalmazások is. Ezzel segít hogyan távolíthatja el a helyszíni kiszolgálók futó hello infrastruktúra fenntartási költségeit.

* **Alkalmazások közzététele az Azure AD-alkalmazásproxy használatával:** hello Azure AD alkalmazásproxy használata a Azure virtuális gépek futó alkalmazások közzététele. További információkért lásd: [az Azure AD-alkalmazásproxy használó alkalmazások közzététele](../active-directory/application-proxy-publish-azure-portal.md)


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a>Központi telepítés Megjegyzés - közzététel integrált Windows-Hitelesítést (integrált Windows-hitelesítés) alkalmazások az Azure AD-alkalmazásproxy használatával
Integrált Windows-hitelesítéssel (IWA) használatával Application Proxy összekötők engedély megadása tooimpersonate felhasználók egyszeri bejelentkezésre tooyour alkalmazások küldésére és fogadására a nevében jogkivonatokat. Konfigurálja a kerberos által korlátozott delegálás (KCD) hello összekötő toogrant hello szükséges engedélyek tooaccess erőforrások hello felügyelt tartományon. Felügyelt tartományok hello erőforrás-alapú Kerberos által korlátozott Delegálás eljárást használja a biztonság fokozása érdekében.


### <a name="enable-resource-based-kerberos-constrained-delegation-for-hello-azure-ad-application-proxy-connector"></a>Erőforrás-alapú kerberos által korlátozott delegálás hello Azure AD alkalmazásproxy-összekötő engedélyezése
hello Azure Application Proxy connector konfigurálnia kell a kerberos által korlátozott delegálás (KCD), ezért esetében megszemélyesíthet felhasználókat hello által felügyelt tartományokhoz. Az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz, a nem tartományi rendszergazdai jogosultságokkal rendelkezik. Ezért **hagyományos fiók szintű Kerberos által korlátozott Delegálás nem konfigurálható egy felügyelt tartomány**.

Erőforrás-alapú Kerberos által korlátozott Delegálás használata, ez a [cikk](active-directory-ds-enable-kcd.md).

> [!NOTE]
> Egy tag toobe van szüksége hello "AAD DC rendszergazdák" csoportba, tooadminister hello felügyelt tartomány AD PowerShell-parancsmagok használatával.
>
>

Hello Get-ADComputer PowerShell parancsmag tooretrieve hello beállítások használatát hello számítógép melyik hello Azure AD alkalmazásproxy összekötő telepítve van.
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

Ezt követően használható a hello Set-ADComputer parancsmag tooset fel erőforrás-alapú Kerberos által korlátozott Delegálás hello erőforrás-kiszolgáló.
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

Ha több alkalmazásproxy-összekötő a felügyelt tartomány állított be, szükség van-e tooconfigure erőforrás-alapú minden ilyen összekötő-példánynak a Kerberos által korlátozott Delegálás.


## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Kerberos által korlátozott delegálás konfigurálása a felügyelt tartományhoz](active-directory-ds-enable-kcd.md)
* [A Kerberos általi korlátozott delegálás áttekintése](https://technet.microsoft.com/library/jj553400.aspx)
