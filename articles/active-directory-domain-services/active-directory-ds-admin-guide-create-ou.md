---
title: "Az Azure Active Directory tartományi szolgáltatások: Felügyeleti útmutató |} Microsoft Docs"
description: "Hozzon létre egy szervezeti egység (OU) az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: ce7539e5d5c7c1bf9505ef229f2d31d84c00da05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Hozzon létre egy szervezeti egység (OU) az az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz
Az Azure AD tartományi szolgáltatások felügyelt tartományok közé tartoznak a "AADDC számítógépek" és "AADDC felhasználók" néven rendre két beépített tárolók. hello tároló rendelkezik, amelyek minden számítógép számítógép-objektumai "AADDC számítógépek" toohello felügyelt tartományhoz csatlakozott. hello "AADDC" felhasználótároló hello Azure AD-bérlő felhasználók és csoportok tartalmazza. Alkalmanként a felügyelt hello tartomány toodeploy munkaterhelések szükséges toocreate szolgáltatásfiókok lehet. Erre a célra hozzon létre egy egyéni szervezeti egységet (OU) hello kezelt tartományban, és a szervezeti egységre belül szolgáltatásfiókok létrehozása. Ez a cikk bemutatja, hogyan toocreate a felügyelt tartományok szervezeti egység.

## <a name="before-you-begin"></a>Előkészületek
a cikkben szereplő tooperform hello feladatok lesz szüksége:

1. Egy érvényes **Azure-előfizetés**.
2. Egy **Azure AD-címtár** -vagy egy helyszíni címtár vagy egy csak felhőalapú directory szinkronizálva.
3. **Azure AD tartományi szolgáltatások** hello Azure Active directory engedélyezni kell. Ha még nem tette meg, kövesse a hello ismertetett feladatok hello [első lépések útmutató](active-directory-ds-getting-started.md).
4. A tartományhoz csatlakozó virtuális gépek, amelyből hello Azure AD tartományi szolgáltatások felügyeletéhez felügyelt tartomány. Ha egy virtuális gép nem rendelkezik, hajtsa végre a hello a című cikkben ismertetett feladatok hello [tartományhoz egy Windows virtuális gép tooa felügyelt](active-directory-ds-admin-guide-join-windows-vm.md).
5. Hello hitelesítő adatait kell egy **felhasználói fiókhoz tartozó toohello "AAD DC rendszergazdák" csoport** a könyvtárban, a felügyelt tartományra egyéni szervezeti toocreate.

## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>A Távfelügyelet tartományhoz csatlakoztatott virtuális gépen AD felügyeleti eszközök telepítése
Az Azure AD tartományi szolgáltatások felügyelt tartományok kezelheti távolról már ismerős eszközökkel Active Directory felügyeleti például az Active Directory felügyeleti központ (ADAC) vagy AD PowerShell hello. A bérlői rendszergazdák nem rendelkezik jogosultságokkal tooconnect toodomain tartományvezérlők hello által kezelt tartomány távoli asztalon keresztül. tooadminister hello által felügyelt tartományokhoz, hello AD felügyeleti eszközök szolgáltatást telepítheti egy virtuális géphez csatlakoztatott toohello által felügyelt tartományokhoz. Tekintse meg a toohello cikk címe [felügyelheti az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz](active-directory-ds-admin-guide-administer-domain.md) utasításokat.

## <a name="create-an-organizational-unit-on-hello-managed-domain"></a>Hello felügyelt tartomány szervezeti egység létrehozása
Most, hogy telepítve vannak a felügyeleti eszközök hello AD hello a tartományhoz csatlakoztatott virtuális gép, használhatjuk ezeket eszközök toocreate szervezeti egységet hello által felügyelt tartományon. Hajtsa végre az alábbi lépésekkel hello:

> [!NOTE]
> Csak hello "AAD DC rendszergazdák" csoport tagjai egyéni szervezeti rendelkezik hello toocreate szükséges jogosultságokkal. Győződjön meg arról, hogy végezzen hello lépésekkel egy felhasználói fiókkal toothis csoport tartozik.
>
>

1. Hello kezdőképernyőről kattintson **felügyeleti eszközök**. Meg kell jelennie hello AD felügyeleti eszközök hello virtuális gépre telepítve.

    ![A kiszolgálón telepített felügyeleti eszközök](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Kattintson a **Active Directory felügyeleti központ**.

    ![Active Directory felügyeleti központ](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. tooview hello tartomány, kattintson a hello tartománynév hello bal oldali ablaktáblán (például "contoso100.com").

    ![Az ADAC - nézet tartomány](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)
4. Hello jobb oldalán **feladatok** ablaktáblán kattintson a **új** hello tartomány neve csomópont alatt. Ebben a példában elemre kattint **új** hello jobb oldalán hello "contoso100(local)" csomópontban **feladatok** ablaktáblán.

    ![Az ADAC - új szervezeti egység](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)
5. Hello beállítás toocreate szervezeti egység kell megjelennie. Kattintson a **szervezeti egység** toolaunch hello **szervezeti egység létrehozása** párbeszédpanel.
6. A hello **szervezeti egység létrehozása** párbeszédpanelen adja meg a **neve** az új szervezeti egység hello. Adjon meg egy rövid leírást hello OU. Hello állíthatók **kezelő** hello szervezeti egység mezőt. toocreate hello egyéni szervezeti Egységet, kattintson a **OK**.

    ![Az ADAC - létrehozás OU párbeszédpanel](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)
7. Ekkor megjelenik az újonnan létrehozott OU hello hello AD felügyeleti központban (ADAC).

    ![Az ADAC - szervezeti egység létrehozása](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="permissionssecurity-for-newly-created-ous"></a>Engedélyek/biztonsági az újonnan létrehozott szervezeti egységek
Alapértelmezés szerint a hello (hello "AAD DC rendszergazdák" csoport tagjai) létrehozó felhasználó egyéni szervezeti rendszergazdai jogosultsággal (teljes hozzáférés) keresztül hello hello szervezeti Egységet. hello felhasználói ezután használatától és jogosultságokkal tooother felhasználók vagy toohello "AAD DC rendszergazdák" csoportot tetszés szerint. A következő képernyőkép hello alapegységét hello felhasználói "bob@domainservicespreview.onmicrosoft.com" számára létrehozott hello új "MyCustomOU" szervezeti egység teljes hozzáférésük kap.

 ![Az ADAC - új szervezeti biztonsági](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)

## <a name="notes-on-administering-custom-ous"></a>Tudnivalók a egyéni szervezeti egységek felügyelete
Most, hogy létrehozott egy egyéni szervezeti egység, lépjen tovább, és a szervezeti felhasználók, csoportok, számítógépek és szolgáltatásfiókok létrehozása. Felhasználók vagy csoportok nem helyezhetők át hello "AADDC felhasználók" szervezeti egység toocustom szervezeti egységekhez.

> [!WARNING]
> Felhasználói fiókok, csoportok, szolgáltatásfiókok és az Ön által létrehozott egyéni szervezeti egységek alapján számítógép-objektumok nem találhatók az Azure AD-bérlő. Más szóval ezek az objektumok ne jelenjen meg hello Azure AD Graph API-jával vagy a hello Azure AD felhasználói felületén. Ezek az objektumok találhatók csak az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.
>
>

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
* [A csoportházirend konfigurálja a felügyelt tartományhoz](active-directory-ds-admin-guide-administer-group-policy.md)
* [Az Active Directory felügyeleti központ: Első lépések](https://technet.microsoft.com/library/dd560651.aspx)
* [Szolgáltatásfiókok részletes útmutatója](https://technet.microsoft.com/library/dd548356.aspx)
