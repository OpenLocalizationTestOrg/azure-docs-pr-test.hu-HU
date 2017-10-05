---
title: "Az Azure Active Directory tartományi szolgáltatások: RHEL virtuális gép csatlakoztatása felügyelt tartományhoz |} Microsoft Docs"
description: "Red Hat Enterprise Linux virtuális gépek csatlakoztatása az Azure AD tartományi szolgáltatások"
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
ms.openlocfilehash: 69f1850bfed90392e9a4695e2443ffaa6bfc746d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Red Hat Enterprise Linux 7 virtuális gépek csatlakoztatása felügyelt tartományokhoz
Ez a cikk bemutatja, hogyan Red Hat Enterprise Linux (RHEL) 7 virtuális gép csatlakoztatása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Red Hat Enterprise Linux virtuális gép kiépítése
A következő lépésekkel rendszerű RHEL 7 virtuális gép az Azure portál használatával.

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).

    ![Azure-portál irányítópultjának](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. Kattintson a **új** a bal oldali ablaktáblán, és írja be **Red Hat** azokat a keresési sávon, az alábbi képernyőfelvételen látható módon. A Red Hat Enterprise Linux jelennek meg a keresési eredmények között. Kattintson a **Red Hat Enterprise Linux 7.2**.

    ![Válassza ki az RHEL eredmények](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. A Keresés a **mindent** ablaktábla szerepelnie kell a Red Hat Enterprise Linux 7.2 kép. Kattintson a **Red Hat Enterprise Linux 7.2** a virtuálisgép-lemezkép kapcsolatos további információk megjelenítéséhez.

    ![Válassza ki az RHEL eredmények](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. Az a **Red Hat Enterprise Linux 7.2** ablaktáblán, a virtuálisgép-lemezkép további információt kell megjelennie. Az a **telepítési modell kiválasztása** legördülő menüben válasszon ki **klasszikus**. Kattintson a **létrehozása** gombra.

    ![Kép részleteinek megtekintése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. A a **alapjai** oldalán a **hozzon létre virtuális gépet** varázslóban adja meg a **állomásnév** az új virtuális gép. Is a helyi rendszergazdai felhasználónév megadása a **felhasználónév** mező és egy **jelszó**. Választhatja azt is, a helyi rendszergazda felhasználó hitelesítéséhez SSH-kulcsot használ. Jelölje ki a **Tarifacsomagot** a virtuális géphez.

    ![Hozzon létre virtuális Gépet - alapismeretek lap](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. Az a **mérete** oldalán a **hozzon létre virtuális gépet** varázsló, válassza ki a virtuális gép méretét.

    ![Hozzon létre Virtuálisgép - méret kiválasztása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. Az a **beállítások** oldalán a **hozzon létre virtuális gépet** lapján jelölje be a tárolási fiókot használja a virtuális gép. Kattintson a **virtuális hálózati** jelölje be a virtuális hálózatot, amelyhez a Linux virtuális gép kell telepíteni. Az a **virtuális hálózati** panelen válassza az Azure AD tartományi szolgáltatásokat a virtuális hálózat érhető el. Ebben a példában a "MyPreviewVNet" virtuális hálózati mentse azt.

    ![Virtuális gép létrehozása – válassza ki a virtuális hálózat](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. Az a **összegzés** oldalán a **hozzon létre virtuális gépet** varázsló, tekintse át, és kattintson a **OK** gombra.

    ![VM - kiválasztott virtuális hálózat létrehozása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. Központi telepítés az RHEL 7.2 lemezképen alapuló új virtuális gép elindul.

    ![Hozzon létre virtuális Gépet - KözpontiTelepítés elindítva](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. Néhány perc múlva a virtuális gép kell telepíteni sikeresen és készítenie való használatra.

    ![Hozzon létre virtuális Gépet - telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Távoli csatlakozás az újonnan kiépített Linux virtuális gép
Az RHEL 7.2 rendszerű virtuális gép az Azure-ban van kiépítve. A következő feladathoz távolról csatlakozni a virtuális gép.

**Csatlakozás az RHEL 7.2 rendszerű virtuális géphez** kövesse a cikkben lévő utasítások [Linuxot futtató virtuális gép bejelentkezés](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

A további lépések végrehajtása azt feltételezi, hogy a PuTTY SSH-ügyfél segítségével csatlakozzon az RHEL virtuális géphez. További információkért lásd: a [PuTTY letöltési oldalát](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. A PuTTY programot nyithatja meg.
2. Adja meg a **állomásnév** az újonnan létrehozott RHEL virtuális géphez. Ebben a példában a virtuális gép a gazdagép neve "contoso-rhel.cloudapp .net" van. Ha nem biztos a virtuális gép állomásnevét, tekintse meg a virtuális gép irányítópult az Azure portálon.

    ![A puTTY-csatlakozás](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. Jelentkezzen be a virtuális gépet a helyi rendszergazdai hitelesítő adatokat adja meg, hogy a virtuális gép létrehozásának használatával. Ebben a példában a helyi rendszergazdai fiók "mahesh" használtuk.

    ![PuTTY bejelentkezés](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-the-linux-virtual-machine"></a>A Linux virtuális gépre telepíti a szükséges csomagokat
A virtuális géphez való csatlakozás után a következő feladathoz tartományhoz való csatlakozást, a virtuális gépen a szükséges csomagokat telepíteni fogja. Hajtsa végre a következő lépéseket:

1. **Telepítse a realmd:** a realmd csomag szolgál a tartományhoz való csatlakozást. A PuTTY terminál írja be a következő parancsot:

    sudo yum telepítés realmd

    ![Realmd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Néhány perc múlva a realmd csomag telepítve kell beolvasni a virtuális gépen.

    ![telepített realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. **Telepítse a sssd:** a realmd csomag függ sssd tartomány összekapcsolási műveletek végrehajtásához. A PuTTY terminál írja be a következő parancsot:

    sudo yum telepítés sssd

    ![Sssd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Néhány perc múlva a sssd csomag telepítve kell beolvasni a virtuális gépen.

    ![telepített realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. **Telepítse a kerberos:** a PuTTY terminál, írja be a következő parancsot:

    sudo yum telepítés krb5-munkaállomás krb5-függvénytárak

    ![Kerberos telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Néhány perc múlva a realmd csomag telepítve kell beolvasni a virtuális gépen.

    ![A Kerberos telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>A Linux virtuális gép csatlakoztatása felügyelt tartományhoz
Most, hogy a szükséges csomagokat a Linux virtuális gépek vannak telepítve, a következő feladat a virtuális gép csatlakoztatása felügyelt tartományhoz.

1. Az AAD tartományi szolgáltatásokra által kezelt tartomány felderítése. A PuTTY terminál írja be a következő parancsot:

    sudo tartomány CONTOSO100.COM felderítése

    ![A kezdőtartomány felderítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Ha **a kezdőtartomány felderítése** nem tudja a kezelt tartományban található, győződjön meg arról, hogy a tartomány elérhető-e a virtuális gépről (próbálja ping). Bizonyosodjon meg arról, hogy a virtuális gép valóban már alkalmazva van az azonos virtuális hálózatban, amelyben a felügyelt tartományra érhető el.
2. Kerberos inicializálni. A PuTTY terminál írja be a következő parancsot. Adjon meg egy felhasználót, aki a "AAD DC rendszergazdák" csoportba tartozik. Csak ezek a felhasználók kapcsolódhatnak a számítógépek a felügyelt tartományra.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Győződjön meg arról, hogy adja meg a tartomány nevét a nagybetűvel, más kinit sikertelen lesz.
3. A számítógép csatlakoztatása a tartományhoz. A PuTTY terminál írja be a következő parancsot. Adja meg az előző lépést ("kinit") megadott ugyanahhoz a felhasználóhoz.

    sudo tartomány illesztési – részletes CONTOSO100.COM -U 'bob@CONTOSO100.COM"

    ![A kezdőtartomány-csatlakozás](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Kell ("sikeresen regisztrált számítógép tartomány") üzenet jelenik meg, ha a gép sikeresen csatlakozott a felügyelt tartományra.

## <a name="verify-domain-join"></a>Ellenőrizze a tartományhoz való csatlakozást
Gyorsan ellenőrizheti, hogy a gép sikeresen csatlakoztatva lett a felügyelt tartományra. Csatlakozás az újonnan tartományhoz RHEL virtuális gép SSH és a tartományi felhasználói fiókot, és ellenőrizze a megoldódott a felhasználói fiók megfelelően.

1. A PuTTY terminálon, írja be a kapcsolódni a következő parancsot az újonnan tartományhoz RHEL virtuális gép SSH használatával. A felügyelt tartományhoz tartozó tartományi fiókot használni (például "bob@CONTOSO100.COM" Ebben az esetben.)

    ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net
2. A PuTTY terminál írja be a következő parancsot, hogy ha a kezdőkönyvtár megfelelően inicializálva.

    pwd
3. A PuTTY terminál írja be a következő parancsot, ha a csoporttagságot lehet megoldani az helyes megjelenítéséhez.

    id

Egy minta kimenet parancs a következőképpen:

![Ellenőrizze a tartományhoz való csatlakozást](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a>Hibaelhárítás a tartományhoz való csatlakozást
Tekintse meg a [hibaelhárítás tartományhoz való csatlakozást](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) cikk.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Windows Server virtuális gép csatlakoztatása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz](active-directory-ds-admin-guide-join-windows-vm.md)
* [Bejelentkezés a Linux rendszerű virtuális gép](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Kerberos telepítése](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Windows-integrációs útmutatója](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
