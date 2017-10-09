---
title: "Az Azure Active Directory tartományi szolgáltatások: A csoportházirend a felügyelt tartományok felügyelete |} Microsoft Docs"
description: "Csoportházirend felügyelete az Azure Active Directory tartományi szolgáltatások által felügyelt tartományok"
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
ms.openlocfilehash: d56ebf27e3015a7f3385ac5a4ddd77ea2c88387f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="administer-group-policy-on-an-azure-ad-domain-services-managed-domain"></a>Csoportházirend a egy Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete
Az Azure Active Directory tartományi szolgáltatások beépített csoportházirend-objektumok (GPO) hello "AADDC felhasználók" és "AADDC számítógépek" tárolók magában foglalja. Testre szabhatja a beépített csoportházirend-objektumok tooconfigure csoportházirend hello felügyelt tartományon. Emellett hello "AAD DC rendszergazdák" csoportba hozhat létre a saját egyéni szervezeti egységek hello kezelt tartományban. Akkor is egyéni csoportházirend-objektumok létrehozása, és kapcsolja őket toothese egyéni szervezeti egységekhez. Toohello "AAD DC rendszergazdák" csoportba tartozó felhasználók hello által kezelt tartomány csoportházirend felügyeleti jogosultságokat kapnak.

## <a name="before-you-begin"></a>Előkészületek
a cikkben szereplő tooperform hello feladatok lesz szüksége:

1. Egy érvényes **Azure-előfizetés**.
2. Egy **Azure AD-címtár** -vagy egy helyszíni címtár vagy egy csak felhőalapú directory szinkronizálva.
3. **Azure AD tartományi szolgáltatások** hello Azure Active directory engedélyezni kell. Ha még nem tette meg, kövesse a hello ismertetett feladatok hello [első lépések útmutató](active-directory-ds-getting-started.md).
4. A **tartományhoz csatlakoztatott virtuális gép** amely felügyelhető a hello Azure AD tartományi szolgáltatások által felügyelt tartományokhoz. Ha egy virtuális gép nem rendelkezik, hajtsa végre a hello a című cikkben ismertetett feladatok hello [tartományhoz egy Windows virtuális gép tooa felügyelt](active-directory-ds-admin-guide-join-windows-vm.md).
5. Hello hitelesítő adatait kell egy **felhasználói fiókhoz tartozó toohello "AAD DC rendszergazdák" csoport** a könyvtárban, a felügyelt tartományok csoportházirend tooadminister.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-tooremotely-administer-group-policy-for-hello-managed-domain"></a>1 - Provision egy tartományhoz csatlakoztatott virtuális gép tooremotely feladat csoportházirend hello által kezelt tartomány felügyelete
Az Azure AD tartományi szolgáltatások felügyelt tartományok kezelheti távolról már ismerős eszközökkel Active Directory felügyeleti például az Active Directory felügyeleti központ (ADAC) vagy AD PowerShell hello. Ehhez hasonlóan hello felügyelt tartomány csoportházirend felügyelhető távolról a hello csoportházirend felügyeleti eszközök segítségével.

Az Azure AD-címtár rendszergazdái nem rendelkezik jogosultságokkal tooconnect toodomain tartományvezérlők hello által kezelt tartomány távoli asztalon keresztül. Hello "AAD DC rendszergazdák" csoportba felügyelheti a csoportházirend felügyelt tartományok távolról. Csoportházirend eszközök használhatják a Windows Server vagy Windows-ügyfélen számítógép illesztett toohello felügyelt tartományhoz. Csoportházirend eszközei is telepíthető, mivel hello Csoportházirend kezelése szolgáltatást választható a Windows Server és az ügyfél gépek részét toohello felügyelt tartományhoz csatlakozott.

hello első feladat tooprovision egy Windows Server virtuális gépre, amelyik illesztett toohello által kezelt tartomány. Útmutatásért tekintse meg a toohello cikk címe [csatlakozás egy Windows Server virtuális gép tooan Azure AD tartományi szolgáltatások által kezelt tartomány](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-group-policy-tools-on-hello-virtual-machine"></a>2. feladat – telepítés csoportházirend eszközök hello virtuális gépen
Hajtsa végre a következő lépések tooinstall hello csoport házirend felügyeleti eszközök hello tartományhoz csatlakoztatott virtuális gépen hello.

1. Keresse meg a túl**virtuális gépek** hello a klasszikus Azure portálon csomópontja. Válassza ki az 1. feladatban létrehozott hello virtuális gépet, és kattintson a **Connect** hello parancssávon hello ablak hello alján.

    ![Csatlakoztassa tooWindows virtuális gépet](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. hello klasszikus portál felszólítja tooopen vagy ".rdp" kiterjesztésű fájl, amely használt tooconnect toohello virtuális gép. Kattintson a hello fájl letöltése után.
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
9. A hello **szolgáltatások** lapra, jelölje be hello **csoportházirend-kezelő** szolgáltatás.

    ![Szolgáltatások lapon](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)
10. A hello **megerősítő** kattintson **telepítése** tooinstall hello Csoportházirend kezelése szolgáltatást hello virtuális gépen. Ha a szolgáltatás telepítése sikeresen befejeződött, kattintson **Bezárás** tooexit hello **szerepkörök és szolgáltatások hozzáadása** varázsló.

    ![Jóváhagyás lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management-confirmation.png)

## <a name="task-3---launch-hello-group-policy-management-console-tooadminister-group-policy"></a>3 - indítási hello Csoportházirend kezelése konzol tooadminister csoportházirend feladat
Hello Csoportházirend kezelése konzolon hello tartományhoz csatlakoztatott virtuális gép tooadminister hello kezelt tartományban a csoportházirend is használhatja.

> [!NOTE]
> Meg kell toobe hello "AAD DC rendszergazdák" csoportba, tooadminister csoportházirend hello felügyelt tartomány tagja.
>
>

1. Hello kezdőképernyőről kattintson **felügyeleti eszközök**. Megtekintheti az hello **csoportházirend-kezelő** konzol hello virtuális gépre telepítve.

    ![Indítsa el a Csoportházirend kezelése](./media/active-directory-domain-services-admin-guide/gp-management-installed.png)
2. Kattintson a **csoportházirend-kezelő** toolaunch hello Csoportházirend kezelése konzolt.

    ![Csoportházirend-konzol](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

## <a name="task-4---customize-built-in-group-policy-objects"></a>4. feladat – beépített csoportházirend-objektumok testreszabása
Nincsenek két beépített csoportházirend-objektumok (GPO) – egyet-egyet a felügyelt tartományok hello "AADDC számítógépek" és "AADDC felhasználók" tárolók. Testre szabhatja a csoportházirend-objektumok tooconfigure csoportházirend hello felügyelt tartományon.

1. A hello **csoportházirend-kezelő** konzolt, kattintson a tooexpand hello **erdő: contoso100.com** és **tartományok** csomópontok toosee hello csoportházirendek a felügyelt tartományok.

    ![Beépített csoportházirend-objektumok](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)
2. Testre szabhatja, hogy a csoportházirend-objektumok tooconfigure beépített csoport házirendek a felügyelt tartományra. Kattintson a jobb gombbal a hello csoportházirend-Objektumot, és kattintson a **szerkesztése...**  toocustomize hello beépített csoportházirend-objektum. hello Helyicsoportházirend-szerkesztő konfigurációs eszköz lehetővé teszi, hogy Ön toocustomize hello csoportházirend-objektum.

    ![Beépített csoportházirend-objektum szerkesztéséhez](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)
3. Ezután már használhatja a hello **Csoportházirendkezelés-szerkesztő** konzol tooedit hello beépített csoportházirend-objektum. Ha például a következő képernyőkép hello látható, hogyan toocustomize hello beépített "AADDC számítógépek" csoportházirend-objektum.

    ![Csoportházirend-objektum testreszabása](./media/active-directory-domain-services-admin-guide/gp-editor.png)

## <a name="task-5---create-a-custom-group-policy-object-gpo"></a>5. feladat – hozzon létre egy egyéni házirend objektum (GPO)
Hozzon létre, vagy a saját egyéni csoportházirend-objektumok importálása. Egyéni csoportházirend-objektumok tooa egyéni is hozzárendelhet a kezelt tartományban létrehozott OU. Egyéni szervezeti egységek létrehozásáról további információk: [hozzon létre egy egyéni szervezeti felügyelt tartomány](active-directory-ds-admin-guide-create-ou.md).

> [!NOTE]
> Meg kell toobe hello "AAD DC rendszergazdák" csoportba, tooadminister csoportházirend hello felügyelt tartomány tagja.
>
>

1. A hello **csoportházirend-kezelő** konzolt, kattintson a tooselect a egyéni szervezeti egységhez (OU). Kattintson a jobb gombbal a hello szervezeti Egységet, és kattintson a **egy csoportházirend-objektum létrehozása ebben a tartományban, és hivatkozás létrehozása itt...** .

    ![Egy egyéni csoportházirend-objektum létrehozása](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)
2. Adjon meg egy hello új csoportházirend-objektum nevét, és kattintson a **OK**.

    ![Adja meg a csoportházirend-objektum nevét](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)
3. Létrejön egy új csoportházirend-objektum és tooyour egyéni szervezeti Egységet. Kattintson a jobb gombbal a hello csoportházirend-Objektumot, és kattintson a **szerkesztése...**  hello menüből.

    ![Az újonnan létrehozott GPO](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)
4. Hello az újonnan létrehozott csoportházirend-objektum hello segítségével testre szabható **Csoportházirendkezelés-szerkesztő**.

    ![Új csoportházirend-objektum testreszabása](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)


További információk [Csoportházirend kezelése konzol](https://technet.microsoft.com/library/cc753298.aspx) a TechNet webhelyen érhető el.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Csatlakozás egy Windows Server virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartományokhoz](active-directory-ds-admin-guide-join-windows-vm.md)
* [Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
* [Csoportházirend kezelése konzol](https://technet.microsoft.com/library/cc753298.aspx)
