---
title: "az Active Directory-erdő, egy Azure virtuális hálózat aaaInstall |} Microsoft Docs"
description: "Ez az oktatóanyag azt ismerteti, hogyan toocreate egy új Active Directory erdő egy Azure virtuális hálózat (VM) virtuális gépen."
services: active-directory, virtual-network
keywords: "az Active directory virtuális gép, telepítés active directory-erdőben, az azure active directory-videót "
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
tags: 
ms.assetid: eb7170d0-266a-4caa-adce-1855589d65d1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/06/2017
ms.author: joflore
ms.openlocfilehash: 08121130777cc3c206d7b5b38974982884dca1c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Egy új Active Directory-erdő telepítése Azure virtuális hálózaton
Ez a témakör bemutatja, hogyan toocreate egy új Windows Server Active Directory környezetben, a virtuális gépen (VM) az Azure virtuális hálózat egy [Azure-beli virtuális hálózat](../virtual-network/virtual-networks-overview.md). Ebben az esetben hello Azure-beli virtuális hálózat nincs csatlakoztatott tooan a helyi hálózaton.

Is érdekelheti az alábbi kapcsolódó témakörök:

* Ezeket a lépéseket bemutató videót: [hogyan tooinstall egy új Active Directory-erdőben egy Azure virtuális hálózaton](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* Igény szerint is [a telephelyek közötti VPN konfigurálása](../vpn-gateway/vpn-gateway-site-to-site-create.md) és majd egy új erdő telepítéséhez vagy egy helyszíni erdő tooan Azure-beli virtuális hálózat kiterjesztése. Ezeket a lépéseket lásd: [egy replika az Active Directory-tartományvezérlő telepítése egy Azure virtuális hálózat](active-directory-install-replica-active-directory-domain-controller.md).
* Active Directory tartományi szolgáltatások (AD DS) telepítése egy Azure virtuális hálózatra vonatkozó fogalmi útmutatóért lásd: [telepítése Windows Server Active Directory telepítése Azure virtuális gépekre vonatkozó irányelvek](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>A forgatókönyv diagramja
Ebben a forgatókönyvben a külső felhasználók kell tartományhoz csatlakozó kiszolgálón futó tooaccess alkalmazást. hello hello alkalmazáskiszolgálók futtató virtuális gépeken és a tartományvezérlők futtató hello virtuális gépeken telepített Azure virtuális hálózat a saját felhő szolgáltatásának telepítve. Ezek megtalálhatók is a rendelkezésre állási készlet továbbfejlesztett hibatűrést.

![Active Directory-erdő Azure virtuális hálózatban a virtuális gépek ][1] 7

## <a name="how-does-this-differ-from-on-premises"></a>Ez eltérések a helyszíni?
Nincs sok tartományvezérlő telepítése az Azure-on és a helyszíni különbsége. hello fő különbségek hello a következő táblázatban láthatók.

| tooconfigure... | Helyszíni követelmények | Azure virtuális hálózat |
| --- | --- | --- |
| **Hello tartományvezérlő IP-címe** |Rendeljen statikus IP-címet hello hálózati adapter tulajdonságai |Futtassa a hello Set-AzureStaticVNetIP parancsmag tooassign egy statikus IP-cím |
| **DNS-ügyfél feloldó** |Előnyben részesített és másodlagos DNS-kiszolgáló címét be hello tartományi tagok hálózati adapter tulajdonságai |DNS-kiszolgáló címét be hello hello virtuális hálózati tulajdonságok |
| **Active Directory adatbázistár** |Igény szerint módosítsa a C:\ hello alapértelmezett tárolási helyét |Toochange alapértelmezett tárolási helyét a C:\ van szüksége |

## <a name="create-an-azure-virtual-network"></a>Az Azure virtuális hálózat létrehozása
1. Jelentkezzen be toohello a klasszikus Azure portálon.
2. Hozzon létre egy virtuális hálózatot. Kattintson a **hálózatok** > **hozzon létre egy virtuális hálózatot**. A következő tábla toocomplete hello varázsló hello hello értékeket használja.

   | A varázsló e lapján... | Adja meg ezeket az értékeket |
   | --- | --- |
   |  **Virtuális hálózat adatai** |<p>Name: Adjon meg egy nevet a virtuális hálózat</p><p>Régió: Hello legközelebbi régiót kiválasztása</p> |
   |  **DNS- és VPN** |<p>Hagyja üresen a DNS-kiszolgáló</p><p>Ne válassza ki bármelyik VPN-beállítás</p> |
   |  **Virtuális hálózat** |<p>Alhálózati név: Adja meg az alhálózat nevét</p><p>Kezdő IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p> |

## <a name="create-vms-toorun-hello-domain-controller-and-dns-server-roles"></a>Virtuális gépek toorun hello tartományvezérlő és DNS-kiszolgálói szerepkörökre létrehozása
Ismételje meg a következő lépéseket toocreate virtuális gépek toohost hello DC szerepkör igény szerint hello. Legalább két virtuális tartományvezérlők tooprovide hibatűrést, és redundanciát kell telepíteni. Ha hello Azure-beli virtuális hálózat tartalmazza-e legalább két tartományvezérlő hasonló módon konfigurált (Ez azt jelenti, hogy mindkét juthatnak ebbe a generációba, futtassa a DNS-kiszolgáló, és nem rendelkezik a műveleti Főkiszolgálói szerepkört, és így tovább) helyezze a hello futtató virtuális gépeken a tartományvezérlők a rendelkezésre állási készlet továbbfejlesztett hibatűrést.

Tekintse meg a Windows PowerShell segítségével, hello felhasználói felületén, a virtuális gépek toocreate hello [használata Azure PowerShell toocreate és előre konfigurálása a Windows-alapú virtuális gépek](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. Hello klasszikus portálon kattintson **új** > **számítási** > **virtuális gép** > **a gyűjtemény**. A következő értékek toocomplete hello varázsló hello használata. Fogadja el a beállítás alapértelmezett értéke hello, kivéve, ha egy másik érték javasolt vagy szükséges.

   | A varázsló e lapján... | Adja meg ezeket az értékeket |
   | --- | --- |
   |  **Kép kiválasztása** |Windows Server 2012 R2 Datacenter |
   |  **Virtuálisgép-konfiguráció** |<p>Virtuális gép neve: (Például AzureDC1) egycímkés nevet írja be.</p><p>Új felhasználónevet: Írja be a felhasználó hello nevét. Ez a felhasználó hello VM hello helyi Rendszergazdák csoport tagja lesz. Szüksége lesz a neve toosign a virtuális gép toohello hello első alkalommal. beépített fiók hello nevű rendszergazda nem fog működni.</p><p>Új jelszó megerősítése: Adjon meg egy jelszót</p> |
   |  **Virtuálisgép-konfiguráció** |<p>Felhőalapú szolgáltatás: Válassza ki <b>hozzon létre egy új felhőalapú szolgáltatás</b> hello az első virtuális gép, és válassza ki, hogy további virtuális gépeket üzemeltető létrehozásakor szolgáltatás neve azonos felhőalapú hello DC szerepkör.</p><p>Felhőalapú szolgáltatás DNS-neve: Globálisan egyedi nevet adjon meg.</p><p>Régió/Affinitáscsoport/virtuális hálózat: hello virtuális hálózat nevét (például WestUSVNet) adja meg.</p><p>Tárfiók: Válassza ki <b>automatikusan létrehozott tárfiók használata</b> hello az első virtuális gép, és válassza ki, amely ugyanazt a tárfiókot, amely helyt további virtuális gépek létrehozásakor nevet hello DC szerepkör.</p><p>Rendelkezésre állási csoportot: Válassza ki <b>egy rendelkezésre állási csoport létrehozása</b>.</p><p>Rendelkezésre állási csoport neve: hello rendelkezésre állási csoport a létrehozásakor nevet hello első virtuális gép, és válassza ki, hogy azonos nevet további virtuális gépek létrehozásakor típusa.</p> |
   |  **Virtuálisgép-konfiguráció** |<p>Válassza ki <b>Virtuálisgép-ügynök telepítése hello</b> és bármely más bővítmények van szüksége.</p> |
2. Rendeljen egy lemez tooeach virtuális Gépet, amely hello tartományvezérlő kiszolgálói szerepkör fog futni. hello további lemez szükséges toostore hello AD adatbázis, a naplókat, és a SYSVOL mappa. (Például 10 GB-os) hello lemez méretet adjon meg, és hagyja hello **állomás gyorsítótár preferencia** túl beállítása**nincs**. Hello lépéseiért lásd: [hogyan tooAttach egy adatlemez tooa Windows rendszerű virtuális gép](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Miután először bejelentkezik toohello VM, nyissa meg **Kiszolgálókezelő** > **fájl- és tárolási szolgáltatások** toocreate egy kötetet a lemezen NTFS fájlrendszerrel.
4. Egy statikus IP-címet lefoglalni hello DC szerepkör által futtatott virtuális gépek esetén. tooreserve egy statikus IP-címet, töltse le a Microsoft Webplatform-telepítő hello és [Azure PowerShell telepítése](/powershell/azure/overview) és futtassa a Set-AzureStaticVNetIP hello parancsmagot. Példa:

    `Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM`

Egy statikus IP-cím beállításával kapcsolatos további információkért lásd: [egy statikus belső IP-cím konfigurálása a virtuális gépek](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Windows Server Active Directory telepítése
Használjon ugyanazon rutin túl hello[Active Directory tartományi szolgáltatások telepítése](https://technet.microsoft.com/library/jj574166.aspx) használatát a helyszíni (Ez azt jelenti, hogy használható hello felhasználói felület, a válaszfájl vagy a Windows PowerShell). Tooprovide rendszergazdai hitelesítő adatok tooinstall egy új erdő van szüksége. Active Directory-adatbázis hello toospecify hello helyét, a naplókat, és a SYSVOL, módosítsa a hello alapértelmezett tárolási helyét a hello operációs rendszer meghajtó toohello további adatokat lemezről, hogy a virtuális gép toohello csatlakoztatva.

Hello tartományvezérlő telepítésének befejezése után csatlakozzon újra a virtuális gép toohello, és jelentkezzen be toohello tartományvezérlő. Ne felejtse el toospecify tartományi hitelesítő adatokat.

## <a name="reset-hello-dns-server-for-hello-azure-virtual-network"></a>Hello hello Azure-beli virtuális hálózat DNS-kiszolgáló visszaállítása
1. Alaphelyzetbe állítja a hello DNS-továbbító beállítása hello új tartományvezérlő/DNS-kiszolgálón.
   1. A Kiszolgálókezelőben kattintson **eszközök** > **DNS**.
   2. A **DNS-kezelő**, kattintson a jobb gombbal a hello hello DNS-kiszolgáló nevét, és kattintson a **tulajdonságok**.
   3. A hello **továbbítók** lapon kattintson a hello továbbító hello IP-címét, majd kattintson **szerkesztése**.  Jelöljön ki hello IP-címet, majd **törlése**.
   4. Kattintson a **OK** tooclose hello szerkesztő és **Ok** újra a tooclose hello DNS-kiszolgáló tulajdonságait.
2. Frissítés hello DNS-kiszolgáló beállítása a virtuális hálózati hello.
   1. Kattintson a **virtuális hálózatok** > kattintson duplán a létrehozott virtuális hálózat hello > **konfigurálása** > **DNS-kiszolgálók**írja be a hello nevét és az egyik DIP hello virtuális gépek hello hello tartományvezérlő/DNS-kiszolgálói szerepkör, és kattintson futtató **mentése**.
   2. Válassza ki a virtuális gép hello, és kattintson a **indítsa újra a** tootrigger hello VM tooconfigure DNS-feloldási beállítások hello új DNS-kiszolgáló hello IP-címmel.

## <a name="create-vms-for-domain-members"></a>Tartományhoz csatlakoztatott virtuális gépek létrehozása
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

## <a name="see-also"></a>Lásd még:
* [Hogyan tooinstall egy új Active Directory-erdőben egy Azure virtuális hálózaton](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* [Windows Server Active Directory az Azure virtuális gépeken telepítési útmutatója](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [A telephelyek közötti VPN konfigurálása](../vpn-gateway/vpn-gateway-site-to-site-create.md)
* [A replika Active Directory-tartományvezérlő telepítése egy Azure virtuális hálózatban](active-directory-install-replica-active-directory-domain-controller.md)
* [A Microsoft Azure informatikai szakemberek IaaS: (01) virtuális gép – alapok](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [A Microsoft Azure informatikai szakemberek IaaS: (05) létrehozása virtuális hálózatok és a létesítmények közötti kapcsolat](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md)
* [Hogyan tooinstall Azure PowerShell és konfigurálása](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure parancsmag-referencia](/powershell/azure/get-started-azureps)
* [Azure virtuális gép statikus IP-cím beállítása](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
* [Hogyan tooassign statikus IP-címet tooAzure méretű VM](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
* [Egy új Active Directory-erdő telepítése](https://technet.microsoft.com/library/jj574166.aspx)
* [Bevezetés tooActive Directory tartományi szolgáltatások (AD DS) virtualizálása (100-as szint)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
