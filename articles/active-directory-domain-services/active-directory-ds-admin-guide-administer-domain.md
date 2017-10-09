---
title: "Az Azure Active Directory tartományi szolgáltatások: Egy felügyelt tartomány felügyelete |} Microsoft Docs"
description: "Azure Active Directory tartományi szolgáltatások felügyelt tartományok felügyelete"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4fdbc75-3e6b-4e20-8494-5dcc3bf2220a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 11acc79e06163e3193b1aa981f2cd28af812789d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Az Azure Active Directory tartományi szolgáltatások által felügyelt tartományok adminisztrációja
Ez a cikk bemutatja, hogyan kezeli az tooadminister egy Azure Active Directory (AD) tartományi szolgáltatásokban a tartományhoz.

## <a name="before-you-begin"></a>Előkészületek
a cikkben szereplő tooperform hello feladatok lesz szüksége:

1. Egy érvényes **Azure-előfizetés**.
2. Egy **Azure AD-címtár** -vagy egy helyszíni címtár vagy egy csak felhőalapú directory szinkronizálva.
3. **Azure AD tartományi szolgáltatások** hello Azure Active directory engedélyezni kell. Ha még nem tette meg, kövesse a hello ismertetett feladatok hello [első lépések útmutató](active-directory-ds-getting-started.md).
4. A **tartományhoz csatlakoztatott virtuális gép** amely felügyelhető a hello Azure AD tartományi szolgáltatások által felügyelt tartományokhoz. Ha egy virtuális gép nem rendelkezik, hajtsa végre a hello a című cikkben ismertetett feladatok hello [tartományhoz egy Windows virtuális gép tooa felügyelt](active-directory-ds-admin-guide-join-windows-vm.md).
5. Hello hitelesítő adatait kell egy **felhasználói fiókhoz tartozó toohello "AAD DC rendszergazdák" csoport** a könyvtárban, tooadminister a felügyelt tartományok.

<br>

## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Felügyeleti feladatokat hajthat végre egy felügyelt tartomány
Hello "AAD DC rendszergazdák" csoportba kapnak jogosultságai hello felügyelt tartomány tooperform feladatokat, mint:

* Tartományhoz gép toohello felügyelt.
* Konfigurálja a hello beépített csoportházirend-objektum az hello "AADDC számítógépek" és "AADDC felhasználók" tárolókat hello által felügyelt tartományokhoz.
* DNS a hello által kezelt tartomány felügyelete.
* Hozzon létre, és egyéni szervezeti egységekhez (OU-k) a hello által kezelt tartomány felügyelete.
* Nyereség rendszergazdai hozzáférés toocomputers toohello felügyelt tartományhoz csatlakozott.

## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Ön nem rendelkezik egy felügyelt tartományi rendszergazdai jogosultságokkal
a Microsoft, ideértve a tevékenységeket, például a javítás, figyelés és a biztonsági mentés hello tartomány kezeli. Ezért hello tartomány zárolva van, és Önnek nincs jogosultsága tooperform bizonyos felügyeleti feladatok hello tartományban. Néhány példa a feladatok nem hajtható végre a rendszer alatt.

* Ön nem rendelkezik az hello által felügyelt tartományokhoz tartományi rendszergazdaként vagy vállalati rendszergazdai jogosultságokkal.
* Hello felügyelt tartomány hello sémája nem bővíthető.
* Nem lehet csatlakoztatni toodomain tartományvezérlőjén hello felügyelt távoli asztali kapcsolattal.
* Tartományvezérlők toohello felügyelt tartományok nem adható hozzá.

## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-tooremotely-administer-hello-managed-domain"></a>1 - Provision egy tartományhoz csatlakoztatott Windows Server virtuális gép tooremotely feladat hello által kezelt tartomány felügyelete
Az Azure AD tartományi szolgáltatások felügyelt tartományok ismeri az Active Directory felügyeleti eszközök például az Active Directory felügyeleti központ (ADAC) hello vagy AD PowerShell segítségével kezelhetők. A bérlői rendszergazdák nem rendelkezik jogosultságokkal tooconnect toodomain tartományvezérlők hello által kezelt tartomány távoli asztalon keresztül. Ezért hello felügyelheti a "AAD DC rendszergazdák" csoport tagjai felügyelt tartományok távolról AD felügyeleti eszközök a Windows Server-ügyfél számítógépről, amely illesztett toohello által felügyelt tartományokhoz. AD felügyeleti eszközök is telepíthető, mert hello távoli kiszolgáló felügyeleti eszközei (RSAT) választható szolgáltatás Windows Server és az ügyfél gépek részét felügyelt toohello tartományhoz csatlakozott.

hello első lépéseként tooset be a Windows Server virtuális gépet, amely felügyelt illesztett toohello tartományban. Útmutatásért tekintse meg a toohello cikk címe [csatlakozás egy Windows Server virtuális gép tooan Azure AD tartományi szolgáltatások által kezelt tartomány](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-hello-managed-domain-from-a-client-computer-for-example-windows-10"></a>Távoli felügyeletéhez felügyelt tartomány hello ügyfélszámítógépről (például a Windows 10)
Ez a cikk használata a Windows Server virtuális gép tooadminister hello AAD-DS hello utasításait felügyelt tartomány. Azonban is beállíthatja toouse egy Windows ügyfél (például a Windows 10) virtuális gép toodo stb.

Is [telepíti a Távoli kiszolgálófelügyelet eszközei (RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) hello utasításokat követve a TechNet Windows ügyfél virtuális gépen.

## <a name="task-2---install-active-directory-administration-tools-on-hello-virtual-machine"></a>2. feladat – telepítés Active Directory felügyeleti eszközök hello virtuális gépen
Hajtsa végre a következő lépések tooinstall hello Active Directory-felügyeleti eszközök hello tartományhoz csatlakoztatott virtuális gépen hello. Tekintse meg a Technet további [telepítéséről és a Távoli kiszolgálófelügyelet eszközeivel](https://technet.microsoft.com/library/hh831501.aspx).

1. Keresse meg a túl**virtuális gépek** hello a klasszikus Azure portálon csomópontja. Válassza ki az 1. feladatban létrehozott hello virtuális gépet, és kattintson a **Connect** hello parancssávon hello ablak hello alján.

    ![Csatlakoztassa tooWindows virtuális gépet](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. hello klasszikus portál felszólítja tooopen vagy ".rdp" kiterjesztésű fájl, amely használt tooconnect toohello virtuális gép. Kattintson a tooopen hello fájl letöltése után.
3. A parancssorból hello bejelentkezési hello toohello "AAD DC rendszergazdák" csoportba tartozó felhasználó hitelesítő adatait használja. Például használjuk "bob@domainservicespreview.onmicrosoft.com" esetünkben.
4. Hello kezdőképernyőről nyissa meg a **Kiszolgálókezelő**. Kattintson a **szerepkörök és szolgáltatások hozzáadása** hello központi ablaktáblájában hello Kiszolgálókezelő.

    ![Indítsa el a Kiszolgálókezelőt a virtuális gépen](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. A hello **előkészületek** hello oldalán **hozzáadása szerepkörök és szolgáltatások varázsló**, kattintson a **következő**.

    ![Mielőtt elkezdené lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. A hello **telepítési típus** lapján hello hagyja **szerepköralapú vagy szolgáltatásalapú telepítés** beállítás be van jelölve, és kattintson **következő**.

    ![Telepítés típusa lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. A hello **kiszolgáló kiválasztása** lapon, válassza ki a hello aktuális virtuális gépet hello kiszolgálókészletből, és kattintson **következő**.

    ![Kiszolgáló kiválasztása lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. A hello **kiszolgálói szerepkörök** kattintson **következő**. Azt e lap kihagyása, mert jelenleg nem telepít szerepköröket hello kiszolgálón.
9. A hello **szolgáltatások** tooexpand hello kattintson **távoli kiszolgálófelügyelet eszközei** csomópontot, majd a tooexpand hello **szerepkör-felügyeleti eszközök** csomópont. Válassza ki **AD DS és AD LDS-eszközök** szerepkör-felügyeleti eszközök listájából hello szolgáltatást.

    ![Szolgáltatások lapon](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)
10. A hello **megerősítő** kattintson **telepítése** tooinstall hello AD és az AD LDS-eszközök szolgáltatás hello virtuális gépen. Ha a szolgáltatás telepítése sikeresen befejeződött, kattintson **Bezárás** tooexit hello **szerepkörök és szolgáltatások hozzáadása** varázsló.

    ![Jóváhagyás lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)

## <a name="task-3---connect-tooand-explore-hello-managed-domain"></a>3. feladat – kapcsolódás tooand felfedezés hello által felügyelt tartományokhoz
Most, hogy telepítve vannak a felügyeleti eszközök hello AD hello a tartományhoz csatlakoztatott virtuális gép, azt is használni ezen eszközök tooexplore és hello által kezelt tartomány felügyelete.

> [!NOTE]
> Egy tag toobe van szüksége hello "AAD DC rendszergazdák" csoportba, tooadminister hello felügyelt tartomány.
>
>

1. Hello kezdőképernyőről kattintson **felügyeleti eszközök**. Meg kell jelennie hello AD felügyeleti eszközök hello virtuális gépre telepítve.

    ![A kiszolgálón telepített felügyeleti eszközök](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Kattintson a **Active Directory felügyeleti központ**.

    ![Active Directory felügyeleti központ](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. tooexplore hello tartomány, kattintson a hello tartománynév hello bal oldali ablaktáblán (például "contoso100.com"). Figyelje meg, "AADDC számítógépek" és "AADDC felhasználók" nevezett, illetve két tárolók.

    ![Az ADAC - nézet tartomány](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)
4. Kattintson nevű hello tárolóra **AADDC felhasználók** toosee minden felhasználó és csoport toohello tartozó felügyelt tartomány. Felhasználói fiókokat kell megjelennie, és az Azure ad-csoportok a bérlői megjelenítése fel ebben a tárolóban. Figyelje meg ebben a példában, a felhasználói fiók hello felhasználói "Belinszky" nevű és "AAD DC rendszergazdák" nevű csoport érhetők el ebben a tárolóban.

    ![Az ADAC - tartományi felhasználók](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)
5. Kattintson nevű hello tárolóra **AADDC számítógépek** toosee hello számítógépek toothis felügyelt tartományhoz csatlakozott. Hello aktuális virtuális gépet, mely illesztett toohello tartomány bejegyzést kell megjelennie. Az összes olyan számítógépen, amelyek számítógépes fiókok Azure AD tartományi szolgáltatások által kezelt tartomány tárolódnak a AADDC számítógépfiókokra toohello tartományhoz.

    ![Az ADAC - tartományhoz csatlakoztatott számítógépek](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Csatlakozás egy Windows Server virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartományokhoz](active-directory-ds-admin-guide-join-windows-vm.md)
* [Távoli kiszolgálófelügyelet eszközeinek telepítése](https://technet.microsoft.com/library/hh831501.aspx)
