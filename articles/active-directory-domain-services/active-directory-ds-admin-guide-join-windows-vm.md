---
title: "Az Azure Active Directory tartományi szolgáltatások: A Windows Server virtuális gép csatlakoztatása felügyelt tartományhoz |} Microsoft Docs"
description: "A Windows Server virtuális gépek csatlakoztatása az Azure AD tartományi szolgáltatások"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 74dbdb33-05db-4d47-badc-0d7bb6d0c8cb
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 9f8d21f6964d26a2e17e31d1f2947e7eb07c177d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Windows Server virtuális gépek csatlakoztatása felügyelt tartományokhoz
> [!div class="op_single_selector"]
> * [Klasszikus Azure portál – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

Ez a cikk bemutatja, hogyan csatlakozzon az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz, a klasszikus Azure portál használatával Windows Server 2012 R2 rendszert futtató virtuális gép.

## <a name="step-1-create-the-windows-server-virtual-machine"></a>1. lépés: A Windows Server virtuális gép létrehozása
Című rész utasításait kövesse a [a klasszikus Azure portálon Windows rendszerű virtuális gép létrehozása](../virtual-machines/windows/classic/tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) oktatóanyag. Fontos, hogy az újonnan létrehozott virtuális gépet egy tartományhoz az azonos virtuális hálózatban, amelyben engedélyezte az Azure AD tartományi szolgáltatások biztosításához. A "Gyors létrehozása" beállítás nem engedélyezi, hogy a virtuális gép csatlakoztatása egy virtuális hálózatot. Ezért szeretné a virtuális gép létrehozása a "A gyűjtemény" kapcsoló használatával.

A következő lépésekkel, amelyben engedélyezve van a Azure AD tartományi szolgáltatások a virtuális hálózathoz csatlakozó Windows virtuális gép létrehozása.

1. A klasszikus Azure portálon, a parancssávon az ablak alján kattintson **új**.
2. A **számítási**, kattintson a **virtuális gép**, majd kattintson a **a gyűjtemény**.
3. Az első képernyőn lehetővé teszi, hogy **kép kiválasztása** a virtuális gép elérhető rendszerkép közül. Válassza ki a megfelelő lemezképet.

    ![Válassza ki a lemezképet](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. A második képernyő lehetővé teszi a számítógép nevét, méretét, és rendszergazdai felhasználónév és jelszó. Használja a réteg és az alkalmazás vagy a munkaterhelések futtatásához szükséges méret. Itt válassza ki a felhasználónevet a helyi rendszergazda felhasználó a számítógépen. Ne adja meg itt egy tartományi felhasználói fiók hitelesítő adatait.

    ![Virtuális gép konfigurálása](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)
5. A harmadik képernyő lehetővé teszi a hálózati, tárolási és rendelkezésre állás erőforrásait. Győződjön meg arról, amelyiken engedélyezte az Azure AD tartományi szolgáltatások a virtuális hálózatot választ a **régió/affinitás csoport/virtuális hálózati** legördülő menüből. Adjon meg egy **felhőalapú szolgáltatás DNS-név** a virtuális gép szükség szerint.

    ![Válassza ki a virtuális gép virtuális hálózatot](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

   > [!WARNING]
   > Győződjön meg arról, a virtuális gép csatlakoztatása az azonos virtuális hálózatban, amelyben engedélyezve van a Azure AD tartományi szolgáltatásokat. Ennek eredményeképpen a virtuális gép "Lásd" a tartomány és feladatokat végezhet, például a tartományhoz való csatlakozás. Ha egy másik virtuális hálózatban a virtuális gép létrehozása mellett dönt, a virtuális hálózatban csatlakozni a virtuális hálózat, amelyen engedélyezve van a Azure AD tartományi szolgáltatások.
   >
   >
6. A negyedik képernyő lehetővé teszi a Virtuálisgép-ügynök telepítése és konfigurálása a rendelkezésre álló bővítések egy része.

    ![Kész](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
7. A virtuális gép létrehozása után a klasszikus portál megjeleníti-e az új virtuális gépek a **virtuális gépek** csomópont. Mind a virtuális gép, mind a felhőszolgáltatás automatikusan elindul, és **Fut** állapotúként jelenik meg.

    ![Virtuális gép megfelelően működik, és](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)

## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>2. lépés: Csatlakozás a Windows Server virtuális gép a helyi rendszergazda fiók használatával
Most azt csatlakoztassa az újonnan létrehozott Windows Server virtuális géphez való csatlakoztatása a tartományhoz. Használja a helyi rendszergazdai hitelesítő adatokat, a csatlakozáshoz a virtuális gép létrehozásakor megadott.

A következő lépésekkel csatlakozzon a virtuális géphez.

1. Navigáljon a **virtuális gépek** csomópont a klasszikus portálon. Válassza ki az 1. lépésben létrehozott virtuális gépet, és kattintson a **Connect** a parancssávon az ablak alján.

    ![Windows virtuális géphez](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. A klasszikus portál felszólítja, hogy a fájl megnyitása vagy mentése egy, a virtuális géphez való kapcsolódáshoz használt ".rdp" kiterjesztéssel. Kattintson a letöltés után, nyissa meg a fájlt.
3. A bejelentkezési parancssorba írja be a **helyi rendszergazdai hitelesítő adatokat**, amely a virtuális gép létrehozása a megadott. Például ebben a példában "localhost\mahesh" már használtuk.

Ezen a ponton be kell jelentkeznie az újonnan létrehozott Windows rendszerű virtuális gép helyi rendszergazdai hitelesítő adatokkal való. A következő lépés, hogy a virtuális gép csatlakoztatása a tartományhoz.

## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>3. lépés: Csatlakozás az AAD-felügyelt tartományi szolgáltatások a Windows Server virtuális gépen
Hajtsa végre az alábbi lépéseket a Windows Server virtuális gép csatlakoztatása az AAD-Directory tartományi szolgáltatások által felügyelt tartományokhoz.

1. Csatlakoztassa a Windows Server a 2. A kezdőképernyőről nyissa meg a **Kiszolgálókezelő**.
2. Kattintson a **helyi kiszolgáló** a Kiszolgálókezelő ablakban a bal oldali ablaktáblán.

    ![Indítsa el a Kiszolgálókezelőt a virtuális gépen](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Kattintson a **munkacsoport** alatt a **tulajdonságok** szakasz. Az a **Rendszertulajdonságok** lapot, kattintson a **módosítása** a tartományhoz való csatlakozáshoz.

    ![Rendszer Tulajdonságok lap](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Adja meg az Azure AD tartományi szolgáltatások által kezelt tartomány a tartomány nevét a **tartomány** szövegmezőben kattintson **OK**.

    ![Adja meg a tartományt, amelyet egyesíteni](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. A tartományhoz való csatlakozásnál a hitelesítő adatok megadását kéri. Győződjön meg arról, hogy **adja meg a hitelesítő adatait az aad-ben DC rendszergazdák tartozó felhasználó** csoport. Csak a csoport tagjai jogosultak gépeket csatlakoztatni a felügyelt tartományra.

    ![Adja meg a tartományhoz való csatlakozás hitelesítő adatait](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Hitelesítő adatokat adhat meg a következő módon:

   * Egyszerű felhasználónév formátuma: Adja meg a felhasználói fiókhoz az egyszerű Felhasználónévi utótagot az Azure AD-be. Ebben a példában a "bob" felhasználói UPN-utótagját van "bob@domainservicespreview.onmicrosoft.com".
   * SAMAccountName formátum: a SAMAccountName formátumban is adja meg a fiók nevét. Ebben a példában a felhasználó "bob" kell "CONTOSO100\bob" adja meg.

     > [!NOTE]
     > **Azt javasoljuk, hogy az egyszerű felhasználónév formátumban adja meg a hitelesítő adatokat.** A SAMAccountName előfordulhat, hogy lehet automatikus-jön létre, ha a felhasználói UPN-előtag túl hosszú (például az "joereallylongnameuser"). Ha több felhasználó azonos UPN-előtagot (például Bálint) az Azure AD-bérlő rendelkezik, azok SAMAccountName formátuma lehet a szolgáltatás által automatikusan generált. Ezekben az esetekben az UPN-formátum használható megbízhatóan bejelentkezni a tartományba.
     >
     >
7. Tartományhoz való csatlakozást befejezését követően megjelenik a következő üzenet, és Üdvözli Önt a tartományban. Indítsa újra a virtuális gépet, a tartományhoz csatlakoztatás művelet elvégzéséhez.

    ![Üdvözli a tartományhoz](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

## <a name="troubleshooting-domain-join"></a>Hibaelhárítás a tartományhoz való csatlakozást
### <a name="connectivity-issues"></a>Csatlakozási problémák
Ha a virtuális gép nem található a tartomány, tekintse meg a következő hibaelhárítási lépéseket:

* Győződjön meg arról, hogy a virtuális gép csatlakozik ugyanahhoz a virtuális hálózathoz, mint a tartományi szolgáltatások engedélyezése. Ha nem, akkor a virtuális gép nem tud csatlakozni a tartományhoz, és ezért nem tud csatlakozni a tartományhoz.
* Ha a virtuális gép egy másik virtuális hálózathoz csatlakozik, győződjön meg arról, hogy ez a virtuális hálózat csatlakozik-e a virtuális hálózat, amelyen engedélyezve van a tartományi szolgáltatások.
* Próbálja pingelni a tartományban, a tartománynév, a felügyelt tartomány (például "ping contoso100.com") használatával. Ha nem sikerül, akkor próbálja meg Pingelje meg az IP-címek, amelyen engedélyezve van az Azure AD tartományi szolgáltatásokat az oldalon megjelenített tartomány (például "Pingelje meg 10.0.0.4"). Ha tudni pingelni a IP-címet, de nem a tartomány, DNS esetleg nem megfelelően van konfigurálva. Előfordulhat, hogy nem konfigurálta az IP-címek, a tartomány a virtuális hálózat DNS-kiszolgálóként.
* Próbálja meg a DNS-feloldási gyorsítótárból a virtuális gépen ("ipconfig/flushdns" paranccsal).

Ha a párbeszédpanelt, amely a tartományhoz való csatlakozásnál a hitelesítő adatokat kér, nincs kapcsolódási problémák.

### <a name="credentials-related-issues"></a>Hitelesítő adatok kapcsolatos problémák
Ha hiba történt a hitelesítő adatokkal, és nem tud csatlakozni a tartományhoz tekintse meg az alábbi lépéseket.

* Próbálja az UPN-formátum használatával adja meg a hitelesítő adatokat. A fiók SAMAccountName lehet, automatikusan létrehozott, ha több felhasználó az Ön bérelt szolgáltatásának azonos UPN előtaggal, vagy ha az UPN-előtag túl hosszú. Ezért a fiók SAMAccountName formátumát eltérhet mi várható és használatához el a helyszíni tartományban.
* Próbálja meg a "AAD DC rendszergazdák" csoportba gépeket csatlakoztatni a felügyelt tartományra tartozó felhasználói fiók hitelesítő adatait.
* Győződjön meg arról, hogy [engedélyezte a jelszó-szinkronizálást](active-directory-ds-getting-started-password-sync.md) az első lépéseket ismertető útmutató lépéseinek megfelelően.
* Győződjön meg arról, hogy a felhasználó egyszerű Felhasználónevét az Azure AD-be (például "bob@domainservicespreview.onmicrosoft.com") való bejelentkezéshez.
* Győződjön meg arról, hogy amíg a jelszó-szinkronizálás végrehajtásához az első lépések útmutató a várt.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
