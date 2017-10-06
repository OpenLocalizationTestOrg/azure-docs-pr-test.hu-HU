---
title: "Az Azure Active Directory tartományi szolgáltatások: Csatlakozás a Windows Server virtuális gép tooa felügyelt tartományhoz |} Microsoft Docs"
description: "A Windows Server virtuális gép tooAzure AD tartományi szolgáltatások"
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
ms.openlocfilehash: 1e85833b42bd51f3b3df067d6c5b69253459bec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-windows-server-virtual-machine-tooa-managed-domain"></a>Csatlakozás egy Windows Server virtuális gép tooa felügyelt tartományhoz
> [!div class="op_single_selector"]
> * [Klasszikus Azure portál – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

Ez a cikk bemutatja, hogyan toojoin egy virtuális gép futó Windows Server 2012 R2 tooan Azure AD tartományi szolgáltatások által felügyelt tartományba, hello klasszikus Azure portál használatával.

## <a name="step-1-create-hello-windows-server-virtual-machine"></a>1. lépés: Hello Windows Server virtuális gép létrehozása
Útmutatás alapján hello hello leírt [Windows hello a klasszikus Azure portálon futó virtuális gép létrehozása](../virtual-machines/windows/classic/tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) oktatóanyag. Fontos, hogy az újonnan létrehozott virtuális gép tooensure csatlakoztatott toohello ugyanazt a virtuális hálózatot, amelyben engedélyezte az Azure AD tartományi szolgáltatásokat. hello gyors Létrehozás beállítás nem engedélyezi az toojoin hello virtuális gép tooa virtuális hálózaton. Ezért toouse hello "A gyűjtemény" lehetőséget toocreate hello virtuális gép van szüksége.

Hajtsa végre egy Windows virtuális gép csatlakoztatott toohello virtuális hálózatot, amelyben engedélyezve van a Azure AD tartományi szolgáltatások a következő lépéseket toocreate hello.

1. Hello hello parancssávon hello aljához hello ablak, klasszikus Azure portálon kattintson **új**.
2. A **számítási**, kattintson a **virtuális gép**, majd kattintson a **a gyűjtemény**.
3. első üdvözlő képernyőt lehetővé teszi, hogy **kép kiválasztása** a virtuális gép elérhető rendszerkép hello listája. Válassza ki a megfelelő lemezképet hello.

    ![Válassza ki a lemezképet](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. második üdvözlő képernyőt lehetővé teszi a számítógép nevét, méretét, és rendszergazdai felhasználónév és jelszó. Az alkalmazás vagy a munkaterhelés, használja a hello réteg és a szükséges méret toorun. Itt választ hello felhasználónév megadása a helyi rendszergazda felhasználó hello gépen. Ne adja meg itt egy tartományi felhasználói fiók hitelesítő adatait.

    ![Virtuális gép konfigurálása](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)
5. harmadik üdvözlő képernyőt lehetővé teszi a hálózati, tárolási és rendelkezésre állás erőforrásait. Győződjön meg arról, amelyiken engedélyezte az hello Azure AD tartományi szolgáltatások hello virtuális hálózatot választ **régió/affinitás csoport/virtuális hálózati** legördülő menüből. Adjon meg egy **felhőalapú szolgáltatás DNS-név** hello virtuális gép szükség szerint.

    ![Válassza ki a virtuális gép virtuális hálózatot](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

   > [!WARNING]
   > Győződjön meg arról, hogy csatlakozik-e virtuális gép toohello hello ugyanazt a virtuális hálózatot, amelyben engedélyezve van a Azure AD tartományi szolgáltatásokat. Ennek eredményeképpen hello virtuális gép "Lásd a" hello tartományi és feladatokat végezhet, például hello tartományhoz való csatlakozás. Ha toocreate hello virtuális gép egy másik virtuális hálózatban, csatlakoztassa a virtuális hálózati toohello virtuális hálózatban, amelyben engedélyezve van a Azure AD tartományi szolgáltatásokat.
   >
   >
6. negyedik üdvözlő képernyőt lehetővé teszi a hello Virtuálisgép-ügynök telepítéséhez és konfigurálásához elérhető hello bővítések egy része.

    ![Kész](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
7. Hello virtuális gép létrehozása után hello klasszikus portál megjeleníti-e új virtuális gépek hello hello **virtuális gépek** csomópont. Hello virtuális gép és a felhőalapú szolgáltatás automatikusan elindulnak, és azok állapottal jelenik meg, **futtató**.

    ![Virtuális gép megfelelően működik, és](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)

## <a name="step-2-connect-toohello-windows-server-virtual-machine-using-hello-local-administrator-account"></a>2. lépés: Csatlakozás toohello Windows Server virtuális gép hello helyi rendszergazdai fiók használatával
Most, a Microsoft connect toohello az újonnan létrehozott Windows Server virtuális gép, toojoin azt toohello tartomány. Hello helyi rendszergazdai hitelesítő adatok használata hello virtuális gép, tooconnect tooit létrehozásakor megadott.

Hajtsa végre a következő lépéseket tooconnect toohello virtuális gép hello.

1. Keresse meg a túl**virtuális gépek** csomópont hello a klasszikus portálon. Válassza ki az 1. lépésben létrehozott hello virtuális gépet, és kattintson a **Connect** hello parancssávon hello ablak hello alján.

    ![Csatlakoztassa tooWindows virtuális gépet](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. hello klasszikus portál felszólítja tooopen vagy ".rdp" kiterjesztésű fájl, amely használt tooconnect toohello virtuális gép. Kattintson a tooopen hello fájl letöltése után.
3. Hello bejelentkezési parancssorba írja be a **helyi rendszergazdai hitelesítő adatokat**, a megadott hello virtuális gép létrehozása során. Például ebben a példában "localhost\mahesh" már használtuk.

Ezen a ponton be kell jelentkeznie toohello az újonnan létrehozott Windows rendszerű virtuális gép helyi rendszergazdai hitelesítő adatokkal a. hello következő lépés az toojoin hello virtuális gép toohello tartomány.

## <a name="step-3-join-hello-windows-server-virtual-machine-toohello-aad-ds-managed-domain"></a>3. lépés: Csatlakozás hello Windows Server virtuális gép toohello AAD-DS felügyelt tartományhoz
Hajtsa végre a következő lépéseket toojoin hello Windows Server virtuális gép toohello AAD-Directory tartományi szolgáltatások által kezelt tartomány hello.

1. Csatlakoztassa a Windows Server toohello 2. lépés. Hello kezdőképernyőről nyissa meg a **Kiszolgálókezelő**.
2. Kattintson a **helyi kiszolgáló** hello hello Kiszolgálókezelő ablak bal oldali ablaktábláján.

    ![Indítsa el a Kiszolgálókezelőt a virtuális gépen](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Kattintson a **munkacsoport** alatt hello **tulajdonságok** szakasz. A hello **Rendszertulajdonságok** tulajdonságlapját, kattintson a **módosítás** toojoin hello tartomány.

    ![Rendszer Tulajdonságok lap](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Adja meg az Azure AD tartományi szolgáltatások által kezelt tartomány hello tartománynevet hello **tartomány** szövegmezőben kattintson **OK**.

    ![Adja meg a hello tartomány toobe csatlakoztatva](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. Ön felszólító tooenter a hitelesítő adatok toojoin hello tartományban vannak. Győződjön meg arról, hogy **adja meg a felhasználó tartozó toohello AAD DC rendszergazdák hello hitelesítő adatait** csoport. Csak a csoport tagjai rendelkeznek jogosultsággal toojoin gépek toohello által felügyelt tartományokhoz.

    ![Adja meg a tartományhoz való csatlakozás hitelesítő adatait](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Hitelesítő adatokat adhat meg a következő módokon hello valamelyikét:

   * Egyszerű felhasználónév formátuma: hello egyszerű Felhasználónévi utótagot hello felhasználói fiókhoz, adja meg, az Azure AD-be. Ebben a példában a hello egyszerű Felhasználónévi utótagot hello felhasználó "bob" a "bob@domainservicespreview.onmicrosoft.com".
   * SAMAccountName formátum: hello fióknév hello SAMAccountName formátumban is megadhat. Ebben a példában a "bob" hello felhasználói kellene tooenter "CONTOSO100\bob".

     > [!NOTE]
     > **Hello UPN formátum toospecify hitelesítő adatok használatát javasoljuk.** hello SAMAccountName lehet automatikus-jön létre, ha a felhasználói UPN-előtag túl hosszú (például az "joereallylongnameuser"). Ha több felhasználó hello azonos UPN-előtagot (például Bálint) Azure AD-bérlőn, a SAMAccountName formátum automatikus-generálhatja hello szolgáltatást. Ezekben az esetekben hello UPN formátum használható megbízhatóan toolog toohello tartományban.
     >
     >
7. Tartományhoz való csatlakozást befejezését követően hello toohello tartomány üdvözlőlapjának üzenet a következő témakörben talál. Indítsa újra a virtuális gép hello hello tartomány illesztési művelet toocomplete.

    ![Üdvözli a toohello tartomány](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

## <a name="troubleshooting-domain-join"></a>Hibaelhárítás a tartományhoz való csatlakozást
### <a name="connectivity-issues"></a>Csatlakozási problémák
Ha hello virtuális gép nem toofind hello tartomány, tekintse meg a következő hibaelhárítási lépéseket toohello:

* Győződjön meg arról, hogy hello virtuális géphez csatlakoztatott toohello, amelyek azonos virtuális hálózaton a tartományi szolgáltatások bekapcsolását. Ha nem, akkor hello virtuális gép nem tooconnect toohello tartományt, és ezért nem tudja toojoin hello tartomány.
* Ha hello virtuális gép virtuális hálózati csatlakoztatott tooanother, győződjön meg arról, hogy a virtuális hálózat-e a csatlakoztatott toohello virtuális hálózatot, amelyben engedélyezve van a tartományi szolgáltatások.
* Próbálja meg tooping hello tartomány tartománynév hello hello felügyelt tartomány (például "ping contoso100.com") használatával. Ha tehát nem toodo, próbálja tooping hello IP-címek, ahol engedélyezve van az Azure AD tartományi szolgáltatások hello oldalon megjelenített hello tartomány (például "Pingelje meg 10.0.0.4"). Ha tudja tooping hello IP-címet, de nem hello tartomány, DNS esetleg nem megfelelően van konfigurálva. Előfordulhat, hogy nem konfigurálta hello IP-címek hello tartomány hello virtuális hálózat DNS-kiszolgálóként.
* Próbálja hello hello ("ipconfig/flushdns") virtuális gépen a DNS-feloldási gyorsítótár kiürítése.

Ha toohello párbeszédpanelt, amely kéri a hitelesítő adatok toojoin hello tartomány, csatlakozási problémák nem rendelkeznek.

### <a name="credentials-related-issues"></a>Hitelesítő adatok kapcsolatos problémák
Tekintse meg a következő lépéseket, ha hiba történt a hitelesítő adatokkal, és nem toojoin hello tartomány toohello.

* Próbálja meg hello UPN formátum toospecify hitelesítő adatok használatával. hello SAMAccountName fiókja lehet, hogy lehet automatikusan generált hello azonos UPN-bérlőben előtag, vagy ha az UPN-előtag túl hosszú a több felhasználó esetén. Ezért hello SAMAccountName formátum fiókja eltérhet mi várható és használatához el a helyszíni tartományban.
* Próbálja meg toouse hello tartozó toohello "AAD DC rendszergazdák" csoportot toojoin gépek toohello kezelt tartományi felhasználói fiók hitelesítő adatait.
* Győződjön meg arról, hogy [engedélyezve a jelszó-szinkronizálás](active-directory-ds-getting-started-password-sync.md) hello a kezdeti lépések útmutatóban található lépések hello megfelelően.
* Győződjön meg arról, hogy az Azure ad-ben konfigurált használja hello hello felhasználó egyszerű Felhasználónevét (például "bob@domainservicespreview.onmicrosoft.com") a toosign.
* Győződjön meg arról, hogy amíg a jelszó-szinkronizálás toocomplete hello első lépések útmutató a várt.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
