---
title: "Az Azure Active Directory tartományi szolgáltatások: RHEL virtuális gép csatlakoztatása felügyelt tartományhoz |} Microsoft Docs"
description: "Red Hat Enterprise Linux virtuális gépek csatlakoztatása az Azure AD tartományi szolgáltatások"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d76ae997-2279-46dd-bfc5-c0ee29718096
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: maheshu
ms.openlocfilehash: b48ba1a1a47bc27e1d394e6fa56826df1eb742dd
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Red Hat Enterprise Linux 7 virtuális gépek csatlakoztatása felügyelt tartományokhoz
Ez a cikk bemutatja, hogyan Red Hat Enterprise Linux (RHEL) 7 virtuális gép csatlakoztatása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.

## <a name="before-you-begin"></a>Előkészületek
A cikkben szereplő feladatok elvégzéséhez szüksége:  
1. Egy érvényes **Azure-előfizetés**.
2. Egy **Azure AD-címtár** -vagy egy helyszíni címtár vagy egy csak felhőalapú directory szinkronizálva.
3. **Azure AD tartományi szolgáltatások** az Azure AD-címtár engedélyezni kell. Ha még nem tette meg, az összes ismertetett feladatok végrehajtásával a [első lépések útmutató](active-directory-ds-getting-started.md).
4. Győződjön meg arról, hogy a virtuális hálózat DNS-kiszolgálóként konfigurálta a felügyelt tartományra IP-címét. További információkért lásd: [az Azure virtuális hálózat DNS-beállításainak frissítése](active-directory-ds-getting-started-dns.md)
5. Végezze el a szükséges lépéseket [szinkronizálja a jelszavakat az Azure AD tartományi szolgáltatások által kezelt tartomány](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Red Hat Enterprise Linux virtuális gép kiépítése
Az Azure, a következő módszerekkel történő RHEL 7 virtuális gép kiépítése:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * A virtuális gép telepítése a **ugyanazt a virtuális hálózatot, amelyben engedélyezte az Azure AD tartományi szolgáltatások**.
> * Válasszon egy **másik alhálózat** fut, amelyen engedélyezte az Azure AD tartományi szolgáltatásokat.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Távoli csatlakozás az újonnan kiépített Linux virtuális gép
Az RHEL 7.2 rendszerű virtuális gép az Azure-ban van kiépítve. A következő feladata távolról csatlakozni a virtuális gépet a virtuális gép kiépítése során létrehozott helyi rendszergazdai fiók használatával.

A cikk utasításait [Linuxot futtató virtuális gép bejelentkezés](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>A Linux virtuális gép a gazdafájl konfigurálása
Az SSH terminálban az/etc/hosts fájl szerkesztése, és frissítse a gép IP-cím és állomásnevet.

```
sudo vi /etc/hosts
```

A hosts fájlt adja meg a következő értéket:

```
127.0.0.1 contoso-rhel.contoso100.com contoso-rhel
```
"Contoso100.com" Íme a felügyelt tartományok DNS-tartomány nevét. "contoso-rhel" az állomásnevet, amelyhez csatlakozik, a felügyelt tartományra RHEL virtuális gép.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>A Linux virtuális gépre telepíti a szükséges csomagokat
Következő lépésként telepítse a virtuális gépen a tartományhoz való csatlakozást a szükséges csomagokat. Az SSH terminált írja be a szükséges csomagok telepítése a következő parancsot:

    ```
    sudo yum install realmd sssd krb5-workstation krb5-libs
    ```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>A Linux virtuális gép csatlakoztatása felügyelt tartományhoz
Most, hogy a szükséges csomagokat a Linux virtuális gépek vannak telepítve, a következő feladat a virtuális gép csatlakoztatása felügyelt tartományhoz.

1. Az AAD tartományi szolgáltatásokra által kezelt tartomány felderítése. Az SSH terminálban írja be a következő parancsot:

    ```
    sudo realm discover CONTOSO100.COM
    ```

     > [!NOTE] 
     > **Hibaelhárítás:** Ha *a kezdőtartomány felderítése* nem találja a felügyelt tartományok:
     * Győződjön meg arról, hogy a tartomány elérhető-e a virtuális gépről (próbálja ping).
     * Ellenőrizze, hogy a virtuális gép valóban már alkalmazva van az azonos virtuális hálózatban, amelyben a felügyelt tartományra érhető el.
     * Ellenőrizze, hogy ha a DNS-kiszolgáló beállításait, a virtuális hálózat úgy, hogy a tartományvezérlők, a felügyelt tartományra mutasson frissítése befejeződött.
     >

2. Kerberos inicializálni. Az SSH terminálban írja be a következő parancsot: 

    > [!TIP] 
    > * Adjon meg egy felhasználót, aki a "AAD DC rendszergazdák" csoportba tartozik. 
    > * Adja meg a tartomány nevét nagybetűvel, más kinit sikertelen lesz.
    >

    ```
    kinit bob@CONTOSO100.COM
    ```

3. A számítógép csatlakoztatása a tartományhoz. Az SSH terminálban írja be a következő parancsot: 

    > [!TIP] 
    > Használja az előző lépést ("kinit") megadott ugyanazzal a fiókkal.
    >

    ```
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'
    ```

Kell ("sikeresen regisztrált számítógép tartomány") üzenet jelenik meg, ha a gép sikeresen csatlakozott a felügyelt tartományra.


## <a name="verify-domain-join"></a>Ellenőrizze a tartományhoz való csatlakozást
Győződjön meg arról, hogy a gép sikeresen csatlakoztatva lett a felügyelt tartományra. Csatlakozás a tartományhoz csatlakoztatott RHEL virtuális gép egy másik SSH-kapcsolat használatával. Tartományi felhasználói fiókot használja, és ellenőrizze, hogy megoldódott a felhasználói fiók megfelelően.

1. A Terminálszolgáltatások az SSH-típus a következő parancsot a tartományhoz való csatlakozáshoz csatlakoztatott RHEL virtuális gép SSH használatával. A felügyelt tartományhoz tartozó tartományi fiókot használni (például "bob@CONTOSO100.COM" Ebben az esetben.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-rhel.contoso100.com
    ```

2. Az SSH terminált írja be a következő parancsot, hogy ha a kezdőkönyvtár megfelelően inicializálva.
    ```
    pwd
    ```

3. Az SSH terminálban az alábbi paranccsal tekintheti meg, ha a csoporttagság megfelelően lehet megoldani.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>Hibaelhárítás a tartományhoz való csatlakozást
Tekintse meg a [hibaelhárítás tartományhoz való csatlakozást](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshooting-domain-join) cikk.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Windows Server virtuális gép csatlakoztatása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz](active-directory-ds-admin-guide-join-windows-vm.md)
* [Bejelentkezés a Linux rendszerű virtuális gép](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Kerberos telepítése](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Windows-integrációs útmutatója](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
