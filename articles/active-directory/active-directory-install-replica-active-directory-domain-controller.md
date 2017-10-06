---
title: "az Azure-ban replika Active Directory tartományvezérlő aaaInstall |} Microsoft Docs"
description: "Ez az oktatóanyag azt ismerteti, hogyan tooinstall egy a helyszíni Active Directoryból tartományvezérlő erdő Azure virtuális géphez."
services: virtual-network
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: virtual-network
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 74876fce2ca2a29f02c2828f9b3a21227248233a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Az Active Directory replika tartományvezérlő telepítése egy Azure virtuális hálózatban
Ez a témakör bemutatja, hogyan tooinstall további tartományvezérlők (más néven a tartományvezérlők replika) az Azure virtuális hálózat az Azure virtuális gépek (VM) a helyszíni Active Directory-tartományhoz.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.

Is érdekelheti az alábbi kapcsolódó témakörök:

* Egy új Active Directory-erdő opcionálisan egy Azure virtuális hálózatra is telepíthető. Ezeket a lépéseket lásd: [egy új Active Directory-erdő telepítése Azure virtuális hálózatra](active-directory-new-forest-virtual-machine.md).
* Active Directory tartományi szolgáltatások (AD DS) telepítése egy Azure virtuális hálózatra vonatkozó fogalmi útmutatóért lásd: [telepítése Windows Server Active Directory telepítése Azure virtuális gépekre vonatkozó irányelvek](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>A forgatókönyv diagramja
Ebben a forgatókönyvben a külső felhasználók kell tartományhoz csatlakozó kiszolgálón futó tooaccess alkalmazást. egy Azure virtuális hálózatra hello hello alkalmazáskiszolgálót futtathat a és hello replika tartományvezérlő virtuális gépek vannak telepítve. hello virtuális hálózati lehetnek a helyszíni hálózathoz csatlakoztatott toohello egy [telephelyek közötti VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) kapcsolat, ahogy az alábbi hello diagram, vagy használhatja az [ExpressRoute](../expressroute/expressroute-locations-providers.md) a gyorsabb kapcsolatok.

hello alkalmazáskiszolgálók hello tartományvezérlők telepítve van a külön felhőszolgáltatások belül toodistribute feldolgozási számítási és belül [rendelkezésre állási készletek](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) továbbfejlesztett a hibatűrés.
hello tartományvezérlők Active Directory-replikáció használatával replikálja az egymással, és a helyszíni tartományvezérlők. Egyetlen szinkronizálási eszközöket van szükség.

![Diagram pf replika Active Directory-tartományvezérlő egy Azure virtuális hálózatot][1]

## <a name="create-an-active-directory-site-for-hello-azure-virtual-network"></a>Az Active Directory-helyet hello Azure-beli virtuális hálózat létrehozása
Egy jó ötlet toocreate hello hálózati régió megfelelő toohello virtuális hálózatot jelenti az Active Directoryban egy hely. Amely segít a hitelesítés, a replikáció és az egyéb DC hely műveletek optimalizálása. hello következő részben megtudhatja, hogyan toocreate egy helyet, és további háttér, lásd: [hozzáadása egy új helyet](https://technet.microsoft.com/library/cc781496.aspx).

1. Nyissa meg az Active Directory – helyek és szolgáltatások: **Kiszolgálókezelő** > **eszközök** > **Active Directory – helyek és szolgáltatások**.
2. Egy hely toorepresent hello régióban hozta létre az Azure virtuális hálózat létrehozása: kattintson a **helyek** > **művelet** > **új hely** > típusa hello új webhely, például Azure Velünk nyugati hello neve > Válassza ki a helykapcsolatot > **OK**.
3. Hozzon létre egy alhálózatot, és rendelje hozzá hello új hely: kattintson duplán **helyek** > kattintson a jobb gombbal **alhálózatok** > **új alhálózat** > írja be a hello IP-címtartománya virtuális hálózati hello (például 10.1.0.0/16 hello forgatókönyv diagramon) > Válassza ki az új Azure hely hello > **OK**.

## <a name="create-an-azure-virtual-network"></a>Az Azure virtuális hálózat létrehozása
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), kattintson a **új** > **hálózati szolgáltatások** > **virtuális hálózati**  >  **Egyéni létrehozás** és használata hello következő értékei toocomplete hello varázsló.

   | A varázsló e lapján... | Adja meg ezeket az értékeket |
   | --- | --- |
   |  **Virtuális hálózat adatai** |<p>Name: Írja be a hello virtuális hálózatokon, például WestUSVNet nevét.</p><p>Régió: Hello legközelebbi régiót válassza.</p> |
   |  **DNS- és VPN-kapcsolat** |<p>DNS-kiszolgálók: Adja meg a hello és egy vagy több helyszíni DNS-kiszolgálók IP-címét.</p><p>Kapcsolat: Válassza ki **a telephelyek közötti VPN konfigurálása**.</p><p>Helyi hálózati: Adjon meg egy új helyi hálózatot.</p><p>Ha ExpressRoute használ VPN helyett, lásd: [Exchange szolgáltatón keresztül ExpressRoute kapcsolat konfigurálása](../expressroute/expressroute-locations-providers.md).</p> |
   |  **Hely-hely kapcsolatot** |<p>Name: Írja be a hello a helyszíni hálózat nevét.</p><p>VPN-eszköz IP-cím: hello eszköz toohello virtuális hálózathoz csatlakozó hello nyilvános IP-címét adja meg. nem található hello VPN-eszköz mögött egy hálózati címfordítást.</p><p>Cím: Adja meg a helyi hálózathoz (például hello forgatókönyv diagramon 192.168.0.0/16) hello címtartományát.</p> |
   |  **Virtuális hálózat** |<p>Címterület: Hello IP-címtartományt, amelyet az toorun hello Azure virtuális hálózat (például 10.1.0.0/16 hello forgatókönyv diagramon) a virtuális gépekhez adja meg. Ez a címtartomány hello a helyszíni hálózat hello címtartományai nem lehet átfedésben.</p><p>Alhálózatok: Adja meg, és hello alkalmazáskiszolgálók az alhálózat címét (például az előtér 10.1.1.0/24) és a tartományvezérlők hello (például a háttér 10.1.2.0/24).</p><p>Kattintson a **átjáró alhálózatának hozzáadása**.</p> |
2. A következő hello virtuális hálózati átjáró toocreate pont-pont biztonságos VPN-kapcsolat kell majd konfigurálnia. Lásd: [konfigurálja a virtuális hálózati átjáró](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) hello utasításokat.
3. Hozzon létre hello pont-pont VPN-kapcsolat hello új virtuális hálózat és egy helyszíni VPN-eszköz között. Lásd: [konfigurálja a virtuális hálózati átjáró](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) hello utasításokat.

## <a name="create-azure-vms-for-hello-dc-roles"></a>Azure virtuális gépek létrehozása hello DC szerepkörök
Ismételje meg a következő lépéseket toocreate virtuális gépek toohost hello DC szerepkör igény szerint hello. Legalább két virtuális tartományvezérlők tooprovide hibatűrést, és redundanciát kell telepíteni. Ha hello Azure-beli virtuális hálózat tartalmazza-e legalább két tartományvezérlő hasonló módon konfigurált (Ez azt jelenti, hogy mindkét juthatnak ebbe a generációba, futtassa a DNS-kiszolgáló, és nem rendelkezik a műveleti Főkiszolgálói szerepkört, és így tovább) helyezze a hello futtató virtuális gépeken a tartományvezérlők a rendelkezésre állási készlet továbbfejlesztett hibatűrést.
Tekintse meg a Windows PowerShell segítségével, hello felhasználói felületén, a virtuális gépek toocreate hello [használata Azure PowerShell toocreate és előre konfigurálása a Windows-alapú virtuális gépek](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), kattintson a **új** > **számítási** > **virtuális gép**  >  **Gyűjteményből**. A következő értékek toocomplete hello varázsló hello használata. Fogadja el a beállítás alapértelmezett értéke hello, kivéve, ha egy másik érték javasolt vagy szükséges.

   | A varázsló e lapján... | Adja meg ezeket az értékeket |
   | --- | --- |
   |  **Kép kiválasztása** |Windows Server 2012 R2 Datacenter |
   |  **Virtuálisgép-konfiguráció** |<p>Virtuális gép neve: (Például AzureDC1) egycímkés nevet írja be.</p><p>Új felhasználónevet: Írja be a felhasználó hello nevét. Ez a felhasználó hello VM hello helyi Rendszergazdák csoport tagja lesz. Szüksége lesz a neve toosign a virtuális gép toohello hello első alkalommal. beépített fiók hello nevű rendszergazda nem fog működni.</p><p>Új jelszó megerősítése: Adjon meg egy jelszót</p> |
   |  **Virtuálisgép-konfiguráció** |<p>Felhőalapú szolgáltatás: Válassza ki <b>hozzon létre egy új felhőalapú szolgáltatás</b> hello az első virtuális gép, és válassza ki, hogy további virtuális gépeket üzemeltető létrehozásakor szolgáltatás neve azonos felhőalapú hello DC szerepkör.</p><p>Felhőalapú szolgáltatás DNS-neve: Globálisan egyedi nevet adjon meg.</p><p>Régió/Affinitáscsoport/virtuális hálózat: hello virtuális hálózat nevét (például WestUSVNet) adja meg.</p><p>Tárfiók: Válassza ki <b>automatikusan létrehozott tárfiók használata</b> hello az első virtuális gép, és válassza ki, amely ugyanazt a tárfiókot, amely helyt további virtuális gépek létrehozásakor nevet hello DC szerepkör.</p><p>Rendelkezésre állási csoportot: Válassza ki <b>egy rendelkezésre állási csoport létrehozása</b>.</p><p>Rendelkezésre állási csoport neve: hello rendelkezésre állási csoport a létrehozásakor nevet hello első virtuális gép, és válassza ki, hogy azonos nevet további virtuális gépek létrehozásakor típusa.</p> |
   |  **Virtuálisgép-konfiguráció** |<p>Válassza ki <b>Virtuálisgép-ügynök telepítése hello</b> és bármely más bővítmények van szüksége.</p> |
2. Rendeljen egy lemez tooeach virtuális Gépet, amely hello tartományvezérlő kiszolgálói szerepkör fog futni. hello további lemez szükséges toostore hello AD adatbázis, a naplókat, és a SYSVOL mappa. (Például 10 GB-os) hello lemez méretet adjon meg, és hagyja hello **állomás gyorsítótár preferencia** túl beállítása**nincs**. Hello lépéseiért lásd: [hogyan tooAttach egy adatlemez tooa Windows rendszerű virtuális gép](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Miután először bejelentkezik toohello VM, nyissa meg **Kiszolgálókezelő** > **fájl- és tárolási szolgáltatások** toocreate egy kötetet a lemezen NTFS fájlrendszerrel.
4. Egy statikus IP-címet lefoglalni hello DC szerepkör által futtatott virtuális gépek esetén. tooreserve egy statikus IP-címet, töltse le a Microsoft Webplatform-telepítő hello és [Azure PowerShell telepítése](/powershell/azure/overview) és futtassa a Set-AzureStaticVNetIP hello parancsmagot. Példa:

    "Get-AzureVM - ServiceName AzureDC1-Name AzureDC1 |} Set-AzureStaticVNetIP - IP-cím 10.0.0.4 |} Frissítés-AzureVM

Egy statikus IP-cím beállításával kapcsolatos további információkért lásd: [egy statikus belső IP-cím konfigurálása a virtuális gépek](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Active Directory tartományi szolgáltatások telepítése Azure virtuális gépeken
Tooa Virtuálisgép jelentkezni, és ellenőrizze, hogy van kapcsolat hello pont-pont VPN- vagy ExpressRoute kapcsolat tooresources között a helyi hálózaton. Majd telepítse az Active Directory tartományi szolgáltatások a hello Azure virtuális gépeken. Használhatja ugyanezt az eljárást további tartományvezérlő tooinstall használhatja a helyszíni hálózaton (felhasználói felület, a Windows PowerShell vagy válaszfájl). Active Directory tartományi szolgáltatások telepítése, válasszon, hogy új kötet hello hello AD adatbázis, a naplófájlok és SYSVOL helyét hello meg. Ha az AD DS-telepítés frissítő van szüksége, tekintse meg [Active Directory tartományi szolgáltatások telepítése (100-as szint)](https://technet.microsoft.com/library/hh472162.aspx) vagy [egy replika Windows Server 2012 tartományvezérlő telepítése meglévő tartományban (200-as szint)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-hello-virtual-network"></a>Hello virtuális hálózat DNS-kiszolgáló
1. A hello [Azure-portálon](https://portal.azure.com), a hello **keresési erőforrások** mezőbe írja be *virtuális hálózatok*, majd kattintson a **virtuális hálózatok (klasszikus)** a hello keresési eredmények. Hello virtuális hálózat, hello nevét kattintson, majd [hello DNS kiszolgáló IP-címek a virtuális hálózatok újrakonfigurálása](../virtual-network/virtual-network-manage-network.md#dns-servers) toouse hello statikus IP-címek hozzárendelve toohello replika tartományvezérlők hello IP-címek egy helyszíni DNS helyett kiszolgálók.
2. tooensure, hogy minden hello replika tartományvezérlő virtuális gépek hello virtuális hálózaton vannak konfigurálva toouse DNS-kiszolgálók hello virtuális hálózatra, kattintson a **virtuális gépek**, kattintson hello állapot oszlop az egyes virtuális gépek, majd **újraindítása** . Várjon, amíg a virtuális gép hello mutatja **futtató** állapotban, mielőtt újból toosign bele.

## <a name="create-vms-for-application-servers"></a>Virtuális gépek létrehozása az alkalmazáskiszolgálók
1. Ismételje meg a hello követő lépéseket toocreate virtuális gépek toorun alkalmazás-kiszolgálóként. Fogadja el a beállítás alapértelmezett értéke hello, kivéve, ha egy másik érték javasolt vagy szükséges.

   | A varázsló e lapján... | Adja meg ezeket az értékeket |
   | --- | --- |
   |  **Kép kiválasztása** |Windows Server 2012 R2 Datacenter |
   |  **Virtuálisgép-konfiguráció** |<p>Virtuális gép neve: (Például AppServer1) egycímkés nevet írja be.</p><p>Új felhasználónevet: Írja be a felhasználó hello nevét. Ez a felhasználó hello VM hello helyi Rendszergazdák csoport tagja lesz. Szüksége lesz a neve toosign a virtuális gép toohello hello első alkalommal. beépített fiók hello nevű rendszergazda nem fog működni.</p><p>Új jelszó megerősítése: Adjon meg egy jelszót</p> |
   |  **Virtuálisgép-konfiguráció** |<p>Felhőalapú szolgáltatás: Válassza ki **hozzon létre egy új felhőalapú szolgáltatás** hello az első virtuális gép, és válassza ki, hogy ugyanaz a felhőalapú szolgáltatás neve további virtuális gépek létrehozásakor üzemelteti hello alkalmazás.</p><p>Felhőalapú szolgáltatás DNS-neve: Globálisan egyedi nevet adjon meg.</p><p>Régió/Affinitáscsoport/virtuális hálózat: hello virtuális hálózat nevét (például WestUSVNet) adja meg.</p><p>Tárfiók: Válassza ki **automatikusan létrehozott tárfiók használata** hello az első virtuális gép, és válassza ki, amely ugyanazt a tárfiókot további virtuális gépek létrehozásakor nevet fogja tárolni hello alkalmazás.</p><p>Rendelkezésre állási csoportot: Válassza ki **egy rendelkezésre állási csoport létrehozása**.</p><p>Rendelkezésre állási csoport neve: hello rendelkezésre állási csoport a létrehozásakor nevet hello első virtuális gép, és válassza ki, hogy azonos nevet további virtuális gépek létrehozásakor típusa.</p> |
   |  **Virtuálisgép-konfiguráció** |<p>Válassza ki <b>Virtuálisgép-ügynök telepítése hello</b> és bármely más bővítmények van szüksége.</p> |
2. Minden virtuális gép üzembe helyezése után jelentkezzen be, és toohello tartomány csatlakoztatására. A **Kiszolgálókezelő**, kattintson a **helyi kiszolgáló** > **munkacsoport** > **módosítása...** Válassza ki és **tartomány** és a helyszíni tartományi hello nevét. Adjon meg egy tartományi felhasználó hitelesítő adataival, majd indítsa újra hello VM toocomplete hello tartományhoz való csatlakozást.

Tekintse meg a Windows PowerShell segítségével, hello felhasználói felületén, a virtuális gépek toocreate hello [használata Azure PowerShell toocreate és előre konfigurálása a Windows-alapú virtuális gépek](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

A Windows PowerShell használatával kapcsolatos további információkért lásd: [Ismerkedés az Azure-parancsmagok](/powershell/azure/overview) és [Azure parancsmag-referencia](/powershell/azure/get-started-azureps).

## <a name="additional-resources"></a>További források
* [Windows Server Active Directory az Azure virtuális gépeken telepítési útmutatója](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Hogyan tooupload meglévő helyszíni Hyper-V tartományi vezérlők tooAzure Azure PowerShell használatával](http://support.microsoft.com/kb/2904015)
* [Egy új Active Directory-erdő telepítése Azure virtuális hálózaton](active-directory-new-forest-virtual-machine.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* [A Microsoft Azure informatikai szakemberek IaaS: (01) virtuális gép – alapok](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [A Microsoft Azure informatikai szakemberek IaaS: (05) létrehozása virtuális hálózatok és a létesítmények közötti kapcsolat](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](/powershell/azure/overview)
* [Az Azure felügyeleti parancsmagok](/powershell/module/azurerm.compute/#virtual_machines)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
