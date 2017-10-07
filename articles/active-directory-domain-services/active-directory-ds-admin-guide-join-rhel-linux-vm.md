---
title: "Az Azure Active Directory tartományi szolgáltatások: Csatlakozás az RHEL VM tooa felügyelt tartományhoz |} Microsoft Docs"
description: "A Red Hat Enterprise Linux virtuális gép tooAzure AD tartományi szolgáltatások"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 87291c47-1280-43f8-8fb2-da1bd61a4942
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 41ca2aaf2eefbf9c403d2b834d61a1aa0943d950
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-tooa-managed-domain"></a>Csatlakozás Red Hat Enterprise Linux 7 virtuális gép tooa felügyelt tartományhoz
Ez a cikk bemutatja, hogyan toojoin a Red Hat Enterprise Linux (RHEL) 7 virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartomány.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Red Hat Enterprise Linux virtuális gép kiépítése
Hajtsa végre a következő lépéseket tooprovision egy RHEL 7 virtuális gép hello Azure-portál használatával hello.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

    ![Azure-portál irányítópultjának](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. Kattintson a **új** a bal oldali ablaktábla és a típus hello **Red Hat** hello keresősávban, ahogy az alábbi képernyőfelvétel a hello be. Red Hat Enterprise Linux bejegyzéseket hello találatok jelennek meg. Kattintson a **Red Hat Enterprise Linux 7.2**.

    ![Válassza ki az RHEL eredmények](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. Keresés hello hello **mindent** ablaktábla szerepelnie kell a Red Hat Enterprise Linux 7.2 hello kép. Kattintson a **Red Hat Enterprise Linux 7.2** tooview hello virtuálisgép-lemezkép további információt.

    ![Válassza ki az RHEL eredmények](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. A hello **Red Hat Enterprise Linux 7.2** ablaktáblán hello virtuálisgép-lemezkép további információt kell megjelennie. A hello **telepítési modell kiválasztása** legördülő menüben válasszon ki **klasszikus**. Kattintson a hello **létrehozása** gombra.

    ![Kép részleteinek megtekintése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. A hello **alapjai** hello oldalán **hozzon létre virtuális gépet** varázsló, adja meg a hello **állomásnév** hello új virtuális gép. Ezenfelül adja meg a helyi rendszergazdai felhasználónév hello **felhasználónév** mező és egy **jelszó**. Dönthet úgy is toouse az SSH-kulcs tooauthenticate hello helyi rendszergazda felhasználó. Jelölje ki a **Tarifacsomagot** hello virtuális géphez.

    ![Hozzon létre virtuális Gépet - alapismeretek lap](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. A hello **mérete** hello oldalán **hozzon létre virtuális gépet** hello virtuális gép varázsló, jelölje be hello mérete.

    ![Hozzon létre Virtuálisgép - méret kiválasztása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. A hello **beállítások** hello oldalán **hozzon létre virtuális gépet** hello virtuális gép varázsló, jelölje be hello storage-fiókjához. Kattintson a **virtuális hálózati** tooselect hello virtuális hálózati toowhich hello Linux virtuális Gépre kell telepíteni. A hello **virtuális hálózati** panelen, jelölje be hello virtuális hálózat, amelyben az Azure AD tartományi szolgáltatások érhető el. Ebben a példában azt válasszon hello "MyPreviewVNet" virtuális hálózatot.

    ![Virtuális gép létrehozása – válassza ki a virtuális hálózat](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. A hello **összegzés** hello oldalán **hozzon létre virtuális gépet** varázsló, tekintse át, és kattintson hello **OK** gombra.

    ![VM - kiválasztott virtuális hálózat létrehozása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. Hello hello RHEL 7.2 lemezképen alapuló új virtuális gép telepítését kell kezdődnie.

    ![Hozzon létre virtuális Gépet - KözpontiTelepítés elindítva](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. Néhány perc elteltével hello virtuális gép sikeresen telepített és használatra kész állapotba kell lennie.

    ![Hozzon létre virtuális Gépet - telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-toohello-newly-provisioned-linux-virtual-machine"></a>Távoli csatlakozás az újonnan kiépített toohello Linux virtuális gép
hello RHEL 7.2 rendszerű virtuális gép az Azure-ban van kiépítve. hello következő feladata tooconnect távolról toohello virtuális gépet.

**Csatlakozás toohello RHEL 7.2 rendszerű virtuális gép** hello hello cikk utasításait követve [hogyan toolog a Linux rendszerű virtuális gép tooa](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

hello további hello lépéseit azt feltételezik, hogy hello PuTTY SSH ügyfél tooconnect toohello RHEL virtuális gép használja. További információkért lásd: hello [PuTTY letöltési oldalát](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Nyissa meg hello PuTTY program.
2. Adja meg a hello **állomásnév** az RHEL virtuális gép található, újonnan létrehozott hello. Ebben a példában a virtuális gép van hello állomás neve "contoso-rhel.cloudapp .net". Ha nem biztos a virtuális gép hello állomásnevét, tekintse meg a toohello VM irányítópult hello Azure-portálon.

    ![A puTTY-csatlakozás](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. Jelentkezzen be toohello virtuális gép hello virtuális gép létrehozásakor megadott hello helyi rendszergazdai hitelesítő adatokkal. Ebben a példában hello helyi rendszergazdai fiók "mahesh" használtuk.

    ![PuTTY bejelentkezés](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-hello-linux-virtual-machine"></a>Telepíti a szükséges csomagokat hello Linux virtuális gépen
Csatlakozó toohello virtuális gép után hello következő feladata a tooinstall csomagok szükséges a tartományhoz való csatlakozást hello virtuális gépen. Hajtsa végre az alábbi lépésekkel hello:

1. **Telepítse a realmd:** hello realmd csomag szolgál a tartományhoz való csatlakozást. A PuTTY terminál írja be a következő parancs hello:

    sudo yum telepítés realmd

    ![Realmd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Néhány perc múlva hello realmd csomag hello virtuális gép első telepíthető.

    ![telepített realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. **Telepítse a sssd:** hello realmd csomag sssd tooperform tartomány összekapcsolási műveletek függ. A PuTTY terminál írja be a következő parancs hello:

    sudo yum telepítés sssd

    ![Sssd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Néhány perc múlva hello sssd csomag hello virtuális gép első telepíthető.

    ![telepített realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. **Telepítse a kerberos:** a PuTTY terminál, írja be a következő parancs hello:

    sudo yum telepítés krb5-munkaállomás krb5-függvénytárak

    ![Kerberos telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Néhány perc múlva hello realmd csomag hello virtuális gép első telepíthető.

    ![A Kerberos telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-hello-linux-virtual-machine-toohello-managed-domain"></a>Csatlakozás hello Linux virtuális gép toohello felügyelt tartományhoz
Most, hogy a szükséges hello csomagok hello Linux virtuális gépek vannak telepítve, hello következő feladata toojoin hello virtuális gép toohello által felügyelt tartományokhoz.

1. Hello AAD tartományi szolgáltatásokra által kezelt tartomány felderítése. A PuTTY terminál írja be a következő parancs hello:

    sudo tartomány CONTOSO100.COM felderítése

    ![A kezdőtartomány felderítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Ha **a kezdőtartomány felderítése** értéke nem lehet toofind a felügyelt tartományok, bizonyosodjon meg arról, hogy hello tartomány elérhető hello (próbálja ping) virtuális gépről. Gondoskodjon arról is, hogy hello a virtuális gép valóban már telepített toohello ugyanazt a virtuális hálózatot, mely hello által kezelt tartomány érhető el.
2. Kerberos inicializálni. A PuTTY terminál írja be a következő parancs hello. Adjon meg egy felhasználót, aki toohello "AAD DC rendszergazdák" csoportba tartozik. Csak ezek a felhasználók kapcsolódhatnak számítógépek toohello által felügyelt tartományokhoz.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Győződjön meg arról, hogy hello tartománynév nagybetűvel adja meg, más kinit sikertelen lesz.
3. Hello gép toohello tartományhoz. A PuTTY terminál írja be a következő parancs hello. Adja meg a hello hello megelőző lépést ("kinit") megadott ugyanahhoz a felhasználóhoz.

    sudo tartomány illesztési – részletes CONTOSO100.COM -U 'bob@CONTOSO100.COM"

    ![A kezdőtartomány-csatlakozás](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Egy üzenet ("sikeresen regisztrált számítógép tartomány") kell kapnia hello gép sikeresen csatlakoztatva toohello által kezelt tartomány esetén.

## <a name="verify-domain-join"></a>Ellenőrizze a tartományhoz való csatlakozást
Gyorsan ellenőrizheti, hogy hello gép megtörtént-e sikeresen csatlakoztatva toohello által felügyelt tartományokhoz. Csatlakozás toohello újonnan tartományhoz RHEL virtuális gép SSH és a tartományi felhasználói fiókot, majd ellenőrizze toosee megoldódott hello felhasználói fiókja helyes.

1. A Terminálszolgáltatások, PuTTY a következő parancs tooconnect toohello újonnan típus hello a tartományhoz csatlakoztatott RHEL virtuális gép SSH használatával. Toohello felügyelt tartományhoz tartozó tartományi fiókot használni (például "bob@CONTOSO100.COM" Ebben az esetben.)

    ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net
2. A PuTTY terminálon írja be a következő parancs toosee, ha hello kezdőkönyvtár megfelelően inicializálva hello.

    pwd
3. Írja be a PuTTY terminál, ha hello csoporttagságot lehet megoldani az megfelelően a következő parancs toosee hello.

    id

Egy minta kimenet parancs a következőképpen:

![Ellenőrizze a tartományhoz való csatlakozást](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a>Hibaelhárítás a tartományhoz való csatlakozást
Tekintse meg a toohello [hibaelhárítás tartományhoz való csatlakozást](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) cikk.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Csatlakozás egy Windows Server virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartományokhoz](active-directory-ds-admin-guide-join-windows-vm.md)
* [Hogyan toolog a Linux rendszerű virtuális gép tooa](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Kerberos telepítése](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Windows-integrációs útmutatója](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
