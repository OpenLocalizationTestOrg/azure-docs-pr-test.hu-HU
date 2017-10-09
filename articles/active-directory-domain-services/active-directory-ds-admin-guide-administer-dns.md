---
title: "Az Azure Active Directory tartományi szolgáltatások: Felügyelni a DNS a felügyelt tartományok |} Microsoft Docs"
description: "A felügyelt tartományok Azure Active Directory tartományi szolgáltatások DNS felügyelete"
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
ms.openlocfilehash: f2085283649eadd3c9e89f708b0eecf10b2d7d70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Felügyelni a DNS a az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz
Az Azure Active Directory tartományi szolgáltatások hello felügyelt tartomány DNS-feloldását biztosító (tartománynév feloldásával) DNS-kiszolgáló tartalmazza. Alkalmanként szükség lehet tooconfigure DNS hello által felügyelt tartományon. A gépeket, amelyek nem tartományhoz csatlakoztatott toohello, a terheléselosztók virtuális IP-címek konfigurálásához, vagy a telepítés külső DNS-továbbítók szükség lehet toocreate DNS-rekordokat. Emiatt toohello "AAD DC rendszergazdák" csoportba tartozó felhasználók kapnak DNS felügyeleti jogosultságokkal hello felügyelt tartományon.

## <a name="before-you-begin"></a>Előkészületek
a cikkben szereplő tooperform hello feladatok lesz szüksége:

1. Egy érvényes **Azure-előfizetés**.
2. Egy **Azure AD-címtár** -vagy egy helyszíni címtár vagy egy csak felhőalapú directory szinkronizálva.
3. **Azure AD tartományi szolgáltatások** hello Azure Active directory engedélyezni kell. Ha még nem tette meg, kövesse a hello ismertetett feladatok hello [első lépések útmutató](active-directory-ds-getting-started.md).
4. A **tartományhoz csatlakoztatott virtuális gép** amely felügyelhető a hello Azure AD tartományi szolgáltatások által felügyelt tartományokhoz. Ha egy virtuális gép nem rendelkezik, hajtsa végre a hello a című cikkben ismertetett feladatok hello [tartományhoz egy Windows virtuális gép tooa felügyelt](active-directory-ds-admin-guide-join-windows-vm.md).
5. Hello hitelesítő adatait kell egy **felhasználói fiókhoz tartozó toohello "AAD DC rendszergazdák" csoport** a könyvtárban, a felügyelt tartományok DNS tooadminister.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-tooremotely-administer-dns-for-hello-managed-domain"></a>1 - Provision egy tartományhoz csatlakoztatott virtuális gép tooremotely feladat DNS hello által kezelt tartomány felügyelete
Az Azure AD tartományi szolgáltatások felügyelt tartományok kezelheti távolról már ismerős eszközökkel Active Directory felügyeleti például az Active Directory felügyeleti központ (ADAC) vagy AD PowerShell hello. Ehhez hasonlóan hello által kezelt tartomány DNS felügyelhető távolról a hello DNS-kiszolgáló felügyeleti eszközök segítségével.

Az Azure AD-címtár rendszergazdái nem rendelkezik jogosultságokkal tooconnect toodomain tartományvezérlők hello által kezelt tartomány távoli asztalon keresztül. A Windows Server-ügyfél számítógépről, amely felügyelt tartományhoz csatlakoztatott toohello távolról DNS-kiszolgáló eszközeivel felügyelt tartományok DNS hello "AAD DC rendszergazdák" csoportba felügyelheti. DNS-kiszolgáló eszközök hello távoli kiszolgáló felügyeleti eszközei (RSAT) választható szolgáltatás a Windows Server részeként telepíthető, és ügyfélgépek felügyelt toohello tartományhoz csatlakozott.

hello első feladat tooprovision egy Windows Server virtuális gépre, amelyik illesztett toohello által kezelt tartomány. Útmutatásért tekintse meg a toohello cikk címe [csatlakozás egy Windows Server virtuális gép tooan Azure AD tartományi szolgáltatások által kezelt tartomány](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-dns-server-tools-on-hello-virtual-machine"></a>2. feladat – telepítés DNS-kiszolgáló eszközök hello virtuális gépen
Hajtsa végre a következő lépések tooinstall hello DNS felügyeleti eszközök hello tartományhoz csatlakoztatott virtuális gépen hello. További információ a [telepítéséről és használatáról a Távoli kiszolgálófelügyelet eszközei](https://technet.microsoft.com/library/hh831501.aspx), tekintse meg a TechNet webhelyén.

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
9. A hello **szolgáltatások** tooexpand hello kattintson **távoli kiszolgálófelügyelet eszközei** csomópontot, majd a tooexpand hello **szerepkör-felügyeleti eszközök** csomópont. Válassza ki **DNS-kiszolgálói eszközök** szerepkör-felügyeleti eszközök listájából hello szolgáltatást.

    ![Szolgáltatások lapon](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)
10. A hello **megerősítő** kattintson **telepítése** tooinstall hello DNS-kiszolgáló eszközök szolgáltatás hello virtuális gépen. Ha a szolgáltatás telepítése sikeresen befejeződött, kattintson **Bezárás** tooexit hello **szerepkörök és szolgáltatások hozzáadása** varázsló.

    ![Jóváhagyás lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)

## <a name="task-3---launch-hello-dns-management-console-tooadminister-dns"></a>A feladat 3 - indítási hello DNS felügyeleti konzol tooadminister DNS
Most, hogy hello DNS-kiszolgálói eszközök összetevő telepítve van a tartományhoz csatlakoztatott virtuális gép hello, a hello DNS eszközök tooadminister DNS hello által felügyelt tartományon is használjuk.

> [!NOTE]
> Toobe hello "AAD DC rendszergazdák" csoport, tooadminister DNS hello felügyelt tartomány tagjának kell.
>
>

1. Hello kezdőképernyőről kattintson **felügyeleti eszközök**. Megtekintheti az hello **DNS** konzol hello virtuális gépre telepítve.

    ![Felügyeleti eszközök – DNS-konzol](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)
2. Kattintson a **DNS** toolaunch hello DNS-kezelőben.
3. A hello **tooDNS kiszolgáló csatlakozzon** párbeszédpanelen című hello lehetőségre **számítógépre következő hello**, és írja be a DNS-tartománynevet hello hello felügyelt tartomány (például "contoso100.com").

    ![DNS-konzol - csatlakozás toodomain](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)
4. DNS-konzol hello toohello felügyelt tartományhoz csatlakozik.

    ![DNS-konzol - tartomány felügyelete](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)
5. Most már használhatja hello DNS konzol tooadd DNS-bejegyzések hello virtuális hálózatot, amelyben engedélyezve van a AAD tartományi szolgáltatásokra belül olyan számítógépekhez.

> [!WARNING]
> Ügyeljen, amikor a DNS felügyelete hello felügyelt tartomány DNS-felügyeleti eszközök segítségével. Győződjön meg arról, hogy **törölni vagy módosítani a hello beépített DNS-rekordok hello tartomány tartományi szolgáltatások által használt**. Beépített DNS-rekordok közé tartozik a tartomány DNS-rekordokat, a névkiszolgáló rekordjait és a DC helyen használt egyéb rögzíti. Ha módosítja ezeket a rekordokat, a tartományi szolgáltatások hello virtuális hálózaton megszakadnak.
>
>

Lásd: hello [DNS-eszközök a következő cikket a TechNet webhelyén](https://technet.microsoft.com/library/cc753579.aspx) DNS kezelésével kapcsolatos további információt.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Csatlakozás egy Windows Server virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartományokhoz](active-directory-ds-admin-guide-join-windows-vm.md)
* [Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
* [A DNS-felügyeleti eszközök](https://technet.microsoft.com/library/cc753579.aspx)
