---
title: "a távoli asztal aaaDetailed hibaelhárítása az Azure-ban |} Microsoft Docs"
description: "Tekintse át a távoli asztali hibákat. Ha nem tudja tooa Windows virtuális gépek Azure-ban a részletes hibaelhárítási lépéseket"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: "nem csatlakozás tooremote asztal, távoli asztal hibaelhárítása a távoli asztal nem lehet kapcsolódni, távoli asztali hibák, a távoli asztal – hibaelhárítás, a távoli asztali problémák"
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 07/25/2017
ms.author: genli
ms.openlocfilehash: fcb0d06aa66b748f3ebbbbe3431471d3cbe7c60d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-toowindows-vms-in-azure"></a>A távoli asztali kapcsolat részletes hibaelhárítási problémák tooWindows virtuális gépek Azure-ban
A cikkben részletes hibaelhárítási összetett távoli asztal hibák toodiagnose és javítsa ki az Azure virtuális gépek Windows-alapú.

> [!IMPORTANT]
> tooeliminate hello gyakori távoli asztal hibáit, győződjön meg arról, hogy tooread [a távoli asztal hibaelhárítási cikke alapszintű hello](troubleshoot-rdp-connection.md) a folytatás előtt.

Felmerülhet a távoli asztal hibaüzenetet, amely nem hasonlítanak hello adott hibaüzenetek ismertetett [hibaelhárítási útmutatója alapszintű távoli asztali hello](troubleshoot-rdp-connection.md). Kövesse ezeket lépéseket toodetermine hello távoli asztal (RDP) ügyfél nem tooconnect toohello RDP szolgáltatást az Azure virtuális gép hello oka.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure hello és fórumok Stack Overflow hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl is az Azure támogatási incidens. Nyissa meg toohello [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) kattintson **támogatás**. Az Azure támogatási használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure támogatás – gyakori kérdések](https://azure.microsoft.com/support/faq/).

## <a name="components-of-a-remote-desktop-connection"></a>Távoli asztali kapcsolat összetevői
a következő összetevők hello részt egy RDP-kapcsolat:

![](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

A folytatás előtt ellenőrizze, hogy mi módosult az hello utolsó sikeres távoli asztali kapcsolat toohello VM toomentally hasznos lehet. Példa:

* nyilvános IP-cím hello VM vagy hello felhőszolgáltatás hello VM tartalmazó hello (más néven hello virtuális IP-cím [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) megváltozott. a DNS-ügyfél gyorsítótára még hello hello RDP hiba okozhatja *korábbi IP-címet* regisztrált hello DNS-nevét. A DNS-ügyfél-gyorsítótár ürítése, és próbáljon újra kapcsolódni a virtuális gép hello. Vagy próbáljon közvetlenül az új VIP hello.
* Használ egy külső alkalmazás toomanage a távoli asztali kapcsolatok hello Azure-portál által előállított hello kapcsolat használata helyett. Győződjön meg arról, hogy hello alkalmazás konfigurálása magában foglalja a távoli asztal forgalom hello hello megfelelő TCP-portot. Ellenőrizheti, hogy ezt a portot, a klasszikus virtuális gép hello [Azure-portálon](https://portal.azure.com), kattintva hello Virtuálisgép-beállítások > Végpontok.

## <a name="preliminary-steps"></a>Első lépések
A folytatás előtt toohello részletes hibaelhárítási,

* Hello Azure-portál nyilvánvaló probléma merül fel a virtuális gép hello hello állapotának ellenőrzéséhez.
* Hajtsa végre a hello [gyorsjavítást lépéseket közös RDP hibáinak hello alapvető hibaelhárítási útmutató](troubleshoot-rdp-connection.md#quick-troubleshooting-steps).

Próbálja meg a távoli asztalon keresztül a virtuális gép toohello újracsatlakozás után ezeket a lépéseket.

## <a name="detailed-troubleshooting-steps"></a>Részletes hibaelhárítási útmutató
hello távoli asztali ügyfél nem feltétlenül tudja tooreach hello távoli asztal szolgáltatás hello Azure virtuális Gépen, a következő források hello esedékes tooissues:

* [Távoli asztali ügyfél számítógép](#source-1-remote-desktop-client-computer)
* [Szervezeti intraneten peremhálózati eszköz](#source-2-organization-intranet-edge-device)
* [A felhőalapú szolgáltatás végpontjának és hozzáférés-vezérlési lista (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Hálózati biztonsági csoportok](#source-4-network-security-groups)
* [Windows-alapú Azure virtuális gép](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>1. forrás: Távoli asztali ügyfél számítógép
Győződjön meg arról, hogy a számítógép biztosíthat a távoli asztali kapcsolatok tooanother a helyszínen, a Windows-alapú számítógép.

![](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

Ha nem, keressen a beállításokat a számítógépen a következő hello:

* Egy helyi beállítástól, amely blokkolja tűzfal a távoli asztal-forgalmat.
* Helyileg telepített ügyfél proxy szoftver, amely megakadályozza, hogy a távoli asztali kapcsolatok.
* Helyileg telepített szoftver, amely megakadályozza, hogy a távoli asztali kapcsolatok hálózatfigyelési.
* Más típusú biztonsági szoftver felügyeljék a forgalmat, vagy a megadott típusú forgalom, amely megakadályozza, hogy a távoli asztali kapcsolatok engedélyezése vagy letiltása.

Ilyen esetben ideiglenesen tiltsa le a hello szoftver, és próbálkozzon tooconnect tooan a helyi számítógép távoli asztalon keresztül. Hello tényleges OK így talál, ha működik a hálózati rendszergazda toocorrect hello szoftverfrissítési beállítások tooallow távoli asztali kapcsolatok.

## <a name="source-2-organization-intranet-edge-device"></a>2. forrás: Szervezet intranetes peremhálózati eszköz
Ellenőrizze, hogy a számítógép közvetlenül toohello Internet biztosíthat a távoli asztali kapcsolatok tooyour Azure virtuális géphez.

![](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

Ha még nem rendelkezik olyan számítógépre, amely közvetlenül csatlakoztatott toohello Internet, hozzon létre, és egy új Azure virtuális gép egy erőforrás vagy felhőhatókört szolgáltatás teszteléséhez. További információkért lásd: [Windows Azure-ban futó virtuális gép létrehozása](../virtual-machines-windows-hero-tutorial.md). Hello virtuális gép és hello erőforráscsoportba vagy hello felhőalapú szolgáltatás, hello vizsgálat után törölheti.

Ha egy távoli asztali kapcsolat létrehozhat egy közvetlenül csatlakoztatott számítógép toohello Internet, ellenőrizze a szervezeti intraneten peremhálózati eszköz a:

* Egy belső tűzfal blokkolja a HTTPS-kapcsolatok toohello Internet.
* Megakadályozza a távoli asztali kapcsolatok proxykiszolgálót.
* Behatolásérzékelési vagy hálózatfigyelési a peremhálózati hálózat, amely megakadályozza, hogy a távoli asztali kapcsolatok eszközökön futó szoftver.

A hálózati rendszergazda toocorrect hello beállításait a szervezeti intraneten peremhálózati eszköz tooallow HTTPS-alapú távoli asztali kapcsolatok toohello Internet dolgozni.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>3. forrás: Felhő szolgáltatásvégpont és hozzáférés-vezérlési lista
A létrehozott virtuális gépek hello klasszikus telepítési modell segítségével ellenőrizze, hogy egy másik Azure virtuális Gépen, amely a hello ugyanaz a felhőalapú szolgáltatás, vagy virtuális hálózati tehet a távoli asztali kapcsolatok tooyour Azure virtuális Gépen.

![](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> A virtuális gépek erőforrás-kezelőt hozott létre, hagyja ki túl[forrás 4: hálózati biztonsági csoportok](#source-4-network-security-groups).

Ha nem rendelkezik egy másik virtuális gép ugyanazon a felhőalapú szolgáltatás, vagy a virtuális hálózati hello, hozzon létre egyet. Hello kövesse [Windows Azure-ban futó virtuális gép létrehozása](../virtual-machines-windows-hero-tutorial.md). Hello teszt befejezése után hello teszt virtuális gép törlése

Ha a távoli asztal tooa virtuális gép hello keresztül is elérheti azonos felhőalapú szolgáltatás vagy a virtuális hálózat, ellenőrizze, hogy ezek a beállítások:

* a távoli asztal forgalmat hello cél virtuális gép hello végpont-konfiguráció: hello titkos TCP-port hello végpont meg kell egyeznie a mely hello a virtuális gép távoli asztal szolgáltatást figyel hello TCP-portot (alapértelmezett érték a 3389-es).
* hozzáférés-vezérlési lista hello hello távoli asztal forgalom végpont hello cél virtuális gép: hozzáférés-vezérlési listák lehetővé teszik toospecify engedélyezett vagy megtagadott a bejövő forgalmat az Internet alapú forrás IP-címe hello. Helytelenül konfigurált hozzáférés-vezérlési listák előfordulhat, hogy a bejövő távoli asztal forgalom toohello végpont. Ellenőrizze a hozzáférés-vezérlési listák tooensure, amely a proxy vagy más biztonsági kiszolgáló a nyilvános IP-címekről érkező bejövő forgalom engedélyezve van. További információkért lásd: [Mi az a hálózati hozzáférés-vezérlési lista (ACL)?](../../virtual-network/virtual-networks-acl.md)

toocheck Ha hello végpont hello forrását hello probléma, távolítsa el a hello aktuális végpont, és hozzon létre egy újat, véletlenszerű port kiválasztásával hello tartomány 49152 – 65535 hello külső port számát. További információkért lásd: [hogyan végpontok tooa virtuális gép tooset](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="source-4-network-security-groups"></a>4. forrás: Hálózati biztonsági csoportok
Hálózati biztonsági csoportok lehetővé teszik a részletesebben vezérelhető, engedélyezett bejövő és kimenő forgalom. Alhálózatok átfedés szabályokat hozhat létre, és a felhőalapú szolgáltatások egy Azure virtuális hálózatra.

Használjon [IP folyamata ellenőrizze](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) tooconfirm, ha egy szabály a hálózati biztonsági csoport egy virtuális gép forgalom tooor blokkolja. Emellett áttekintheti a hatékony biztonsági csoport tooensure a "Engedélyezése" NSG bejövő szabályok szabály létezik-e, és a rendszer előrébb RDP-porthoz (3389-es alapértelmezett). További információkért lásd: [hatékony biztonsági szabályok használatával tootroubleshoot VM forgalom bonyolódjon](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).

## <a name="source-5-windows-based-azure-vm"></a>5. forrás: Windows-alapú Azure virtuális gép
![](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

Hello utasításait követve [Ez a cikk](reset-rdp.md). Ez a cikk hello virtuális gépen hello a távoli asztal szolgáltatás alaphelyzetbe állítása:

* Hello "Távoli asztal" Windows tűzfal alapértelmezett szabály (3389-es TCP-port) engedélyezése.
* Engedélyezze a távoli asztali kapcsolatok hello HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections beállításjegyzékbeli érték too0 beállításával.

Próbálja meg hello újra a kapcsolat a számítógépről. Ha még nem tud tooconnect távoli asztalon keresztül, ellenőrizze a következő lehetséges problémákat hello:

* hello a távoli asztal szolgáltatás nem fut a hello megcélzott virtuális Gépre.
* a távoli asztal szolgáltatás hello nem figyeli a 3389-es TCP-port.
* A Windows tűzfal vagy egy másik helyi tűzfal van egy kimenő forgalomra vonatkozó szabály, amely megakadályozza a távoli asztal forgalmat.
* Távoli asztali kapcsolatok behatolásérzékelési vagy hálózatfigyelési hello Azure virtuális gépen futó szoftverek megakadályozza.

A virtuális gépek hello klasszikus telepítési modellel készült használhatja a távoli Azure PowerShell-munkamenet toohello Azure virtuális géphez. Először tooinstall tanúsítvány hello virtuálisgép üzemeltetési felhőalapú szolgáltatás. Nyissa meg túl[konfigurálása biztonságos távoli PowerShell-elérés tooAzure virtuális gépek](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) , és töltse le a hello **InstallWinRMCertAzureVM.ps1** parancsfájl fájl tooyour helyi számítógépen.

Következő lépésként telepítse az Azure PowerShell, ha még nem tette meg. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

Ezután nyissa meg az Azure PowerShell-parancssort, és módosítani hello aktuális mappa toohello hello **InstallWinRMCertAzureVM.ps1** parancsfájlt. az Azure PowerShell-parancsfájl toorun, meg kell adni a hello megfelelő végrehajtási házirendet. Futtassa a hello **Get-ExecutionPolicy** toodetermine az aktuális házirendszint parancsot. További információ a megfelelő szint hello beállítása: [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

Ezután töltse ki az Azure-előfizetés nevét, hello felhőszolgáltatás neve és a virtuális gép nevét (hello < és > karakter eltávolítása), majd futtassa az alábbi parancsokat.

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of hello cloud service that contains hello target virtual machine>"
$vmName="<Name of hello target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

Hello megfelelő előfizetés neve letölthető hello *SubscriptionName* hello hello megjelenítését tulajdonságának **Get-AzureSubscription** parancsot. Hello felhőszolgáltatás neve hello virtuális gép letölthető hello *szolgáltatásnév* a hello hello megjelenített oszlop **Get-AzureVM** parancsot.

Ellenőrizze, hogy van-e hello új tanúsítványt. Nyissa meg a tanúsítványok beépülő hello aktuális felhasználó számára, és a hely hello **megbízható legfelső szintű hitelesítésszolgáltatók\Tanúsítványok** mappa. Láthatja, hogy egy tanúsítványt a felhőalapú szolgáltatás, a tulajdonos toocolumn hello hello DNS-nevét (például: cloudservice4testing.cloudapp.net).

A következő Azure PowerShell távoli munkamenet kezdeményezése az alábbi parancsokkal.

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

Érvényes rendszergazdai hitelesítő adatok megadása, megtekintheti az Azure PowerShell-parancssorba a következő valami hasonló toohello:

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

hello első a kérdéshez része a felhőszolgáltatás neve, amely tartalmazza a hello megcélzott virtuális Gépre, amely eltér a "cloudservice4testing.cloudapp.net" lehet. Ezután adja ki a felhőre vonatkozó Azure PowerShell-parancsok szolgáltatás tooinvestigate hello problémák említett és hello-konfiguráció kijavítása.

### <a name="toomanually-correct-hello-remote-desktop-services-listening-tcp-port"></a>toomanually megfelelő hello távoli asztali szolgáltatások figyelő TCP-port
A parancssorból hello távoli Azure PowerShell munkamenetben futtassa a parancsot.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

hello portszám tulajdonság hello aktuális port számát mutatja. Ha szükséges, a parancs segítségével módosítsa a hello távoli asztal port hátsó tooits alapértelmezett számértéket (3389-es).

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

Győződjön meg arról, hogy a hello port megtörtént-e a módosított too3389 a parancs segítségével.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Ezzel a paranccsal bezárásához hello távoli Azure PowerShell-munkamenetben.

```powershell
Exit-PSSession
```

Győződjön meg arról, hogy a távoli asztal hello Azure virtuális gép végpontja hello is 3398 TCP-portot használja, mint a belső portjára. Indítsa újra a hello Azure virtuális Gépen, majd próbálja újra a hello távoli asztali kapcsolat.

## <a name="additional-resources"></a>További források
[Hogyan tooreset jelszó vagy a távoli asztal hello szolgáltatás fut a Windows virtuális gépek](reset-rdp.md)

[Hogyan tooinstall Azure PowerShell és konfigurálása](/powershell/azure/overview)

[Secure Shell (SSH) kapcsolatok tooa Linux-alapú Azure virtuális gép hibakeresése](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Egy Azure virtuális gépen futó access tooan alkalmazás hibaelhárítása](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

