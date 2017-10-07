---
title: "egy Azure virtuális gép hibakeresési SSH aaaDetailed |} Microsoft Docs"
description: "Részletesebb hibaelhárítási problémák csatlakozás tooan Azure virtuális gép SSH"
keywords: "ssh kapcsolat visszautasította, ssh hiba és az azure-ssh, SSH-kapcsolat sikertelen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: b8e8be5f-e8a6-489d-9922-9df8de32e839
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: support-article
ms.date: 07/06/2017
ms.author: iainfou
ms.openlocfilehash: 3f711e53a8251f8c06dbb589a258222566a4ae1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-ssh-troubleshooting-steps-for-issues-connecting-tooa-linux-vm-in-azure"></a>Részletes SSH hibaelhárítási problémák csatlakozás tooa Linux virtuális gép az Azure-ban
Számos oka lehet, hogy hello SSH-ügyfél nem feltétlenül tudja tooreach hello SSH szolgáltatást a virtuális gép hello van. Ha követte keresztül hello több [hibaelhárítási lépések általános SSH](troubleshoot-ssh-connection.md), toofurther kell hello csatlakozási probléma elhárításához. Ez a cikk végigvezeti részletes hibaelhárítási lépéseket toodetermine adott hello SSH-kapcsolat nem működik, és hogyan tooresolve azt.

## <a name="take-preliminary-steps"></a>Előzetes lépések
hello alábbi ábrán látható hello összetevők, amelyek szerepet játszanak.

![SSH-szolgáltatás összetevője bemutató diagram, amelyek](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

hello következő lépések segítenek elkülöníteni hello forrás hello hiba, és hogy megoldások, illetve a lehetséges megoldások.

1. Virtuális gép hello hello portálon hello állapotának ellenőrzéséhez.
   A hello [Azure-portálon](https://portal.azure.com), jelölje be **virtuális gépek** > *nevet a virtuális gép*.

   hello hello VM állapotának ablaktáblában megjelennek az **futtató**. Görgessen lefelé tooshow közelmúltbeli tevékenységeit, a számítási, tárolási és hálózati erőforrásokat.

2. Válassza ki **beállítások** tooexamine végpontok, IP-címek, hálózati biztonsági csoportok és egyéb beállításokat.

   hello VM rendelkeznie kell egy SSH-forgalomhoz, megtekintheti a definiált végpontot **végpontok** vagy  **[hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md)**. Az erőforrás-kezelő használatával létrehozott virtuális gépeken végpontok hálózati biztonsági csoport vannak tárolva. Azt is ellenőrizze, hogy hello szabályok alkalmazott toohello hálózati biztonsági csoport és, hogy azok által hivatkozott hello alhálózaton.

tooverify hálózati kapcsolatot, konfigurált hello végpontok ellenőrzést, és ellenőrizze, ha hello virtuális gép egy másik protokoll, például http- vagy egy másik szolgáltatás keresztül érhető el.

Ezeket a lépéseket alatti próbálja újra a hello SSH kapcsolat.

## <a name="find-hello-source-of-hello-issue"></a>Hello probléma forrása hello keresése
hello SSH-ügyfél a számítógépen tooreach hello SSH szolgáltatást az Azure virtuális gép hello tooissues vagy hibás konfigurációi által a következő területek hello miatt meghiúsulhat:

* [SSH-ügyfélszámítógép](#source-1-ssh-client-computer)
* [Szervezet a peremhálózati eszköz](#source-2-organization-edge-device)
* [A felhőalapú szolgáltatás végpontjának és hozzáférés-vezérlési lista (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Hálózati biztonsági csoportok](#source-4-network-security-groups)
* [Linux-alapú Azure virtuális gép](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>1. forrás: SSH ügyfélszámítógép
tooeliminate a számítógép hello forrásaként hello hiba, ellenőrizze, hogy segítségükkel SSH kapcsolatok tooanother helyszíni, Linux-alapú számítógép.

![Diagram, amely kiemeli az SSH-ügyfél számítógép összetevői](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Ha hello létesített kapcsolat megszakad, ellenőrizze, hogy hello problémákat a számítógépen a következő:

* A beállítás, amely helyi tűzfal blokkolja a bejövő vagy kimenő SSH forgalom (22-es TCP)
* Helyileg telepített proxy ügyfélszoftvert, amely megakadályozza az SSH-kapcsolatokhoz
* Helyileg telepített hálózatfigyelési szoftver, amely megakadályozza az SSH-kapcsolatokhoz
* Más típusú biztonsági szoftverek felügyeljék a forgalmat, vagy az adott típusú forgalom engedélyezése vagy letiltása

Ha ezek a feltételek vonatkoznak, ideiglenesen tiltsa le a hello szoftver, és próbálkozzon egy SSH kapcsolat tooan a helyi számítógép toofind kimenő hello OK hello kapcsolat megjelenítését blokkolják a számítógépen. A hálózati rendszergazda toocorrect hello szoftverfrissítési beállítások tooallow SSH-kapcsolatok majd működik.

Ha a tanúsítvány alapú hitelesítést használ, ellenőrizze, hogy rendelkezik-e ezen engedélyek toohello .ssh mappa a kezdőkönyvtárban:

* Chmod 700 ~/.ssh
* Chmod 644 ~/.ssh/\*.pub
* Chmod 600 ~/.ssh/id_rsa (vagy bármely más fájlokat a rajtuk tárolt titkos kulcsok)
* Chmod 644 ~/.ssh/known_hosts (tartalmazza a gazdagépet, hogy csatlakozott-e toovia SSH)

## <a name="source-2-organization-edge-device"></a>2. forrás: Szervezet peremhálózati eszköz
tooeliminate a szervezet peremhálózati eszköz hello forrásaként hello hiba, ellenőrizze, hogy olyan számítógépre, amely közvetlenül csatlakoztatott van toohello Internet tehet SSH-kapcsolatok tooyour Azure virtuális Gépen. Ha a telephelyek közötti VPN-vagy Azure ExpressRoute kapcsolat hello VM érnek el, hagyja ki túl[forrás 4: a hálózati biztonsági csoportok](#nsg).

![Diagram, amely kiemeli a szervezet a peremhálózati eszköz](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Ha olyan számítógépre, amely közvetlenül csatlakoztatott toohello Internet nem rendelkezik, saját erőforrás vagy felhőhatókört szolgáltatásban hozzon létre egy új Azure virtuális gép, és használja azt. További információkért lásd: [Azure-ban futó Linux virtuális gép létrehozása](quick-create-cli.md). Amikor elkészült, a tesztet, törölje a hello erőforrás csoport vagy a virtuális gép és a felhőalapú szolgáltatást.

Ha az SSH-kapcsolat létrehozhat egy olyan számítógéppel, közvetlenül csatlakoztatott toohello Internet van, ellenőrizze a szervezet peremhálózati eszközön:

* Az SSH-forgalmat az hello Internet megakadályozó belső tűzfal
* Egy proxykiszolgálóra, amely megakadályozza az SSH-kapcsolatokhoz
* Behatolásérzékelési vagy hálózatfigyelési a peremhálózati hálózat, amely megakadályozza az SSH-kapcsolatok eszközökön futó szoftverek

A hálózati rendszergazda toocorrect hello beállításokat a szervezet biztonsági eszközök tooallow SSH forgalom hello Internet dolgozni.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>3. forrás: Felhő szolgáltatásvégpont és hozzáférés-vezérlési lista
> [!NOTE]
> Ebből a forrásból csak hello klasszikus telepítési modell segítségével létrehozott tooVMs vonatkozik. Virtuális gépek erőforrás-kezelő használatával létrehozott, hagyja ki túl[forrás-4: a hálózati biztonsági csoportok](#nsg).

tooeliminate hello felhőalapú szolgáltatás végpontjának és hozzáférés-vezérlési lista hello forrásaként hello hiba, ellenőrizze, hogy az Azure virtuális gép egy másik hello azonos virtuális hálózati SSH-kapcsolatok tooyour VM tehet.

![Diagram, amely kiemeli a felhőalapú szolgáltatás végpontjának és hozzáférés-vezérlési lista](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Ha egy másik virtuális gép nem rendelkezik a hello azonos virtuális hálózatban, egyszerűen létrehozhat egyet. További információkért lásd: [Linux virtuális gép létrehozása az Azure-ban a parancssori felület hello](quick-create-cli.md). Törlés hello extra virtuális gép, ha a tesztelés befejezése.

Ha az SSH-kapcsolat hozhatja létre a virtuális gép hello azonos virtuális hálózat, ellenőrizze a következő területeken hello:

* **hello végpont-konfiguráció a hello cél virtuális gép SSH-forgalmat.** hello titkos TCP-port hello végpont melyik hello SSH hello VM szolgáltatást figyel hello TCP-portot meg kell felelnie. (hello alapértelmezett port: 22-es). Hello SSH TCP-port számát a hello Azure portálon ellenőrizze a kiválasztásával **virtuális gépek** > *nevet a virtuális gép* > **beállítások**  >  **Végpontok**.
* **hello ACL hello SSH forgalom végpont hello cél virtuális gépen.** Hozzáférés-vezérlési Listában lehetővé teszi toospecify engedélyezett vagy megtagadott a bejövő forgalom a hello Internet, a forrás IP-címe alapján. Helytelenül konfigurált hozzáférés-vezérlési listák előfordulhat, hogy a bejövő SSH forgalom toohello végpont. Ellenőrizze a hozzáférés-vezérlési listák tooensure hello nyilvános IP-címek a proxy vagy más biztonsági kiszolgálóra érkező bejövő forgalmat engedélyezett. További információkért lásd: [hálózati hozzáférés-vezérlési listák (ACL)](../../virtual-network/virtual-networks-acl.md).

tooeliminate hello végpont hello probléma forrásaként hello aktuális végpont távolítsa el, egy másik végpont létrehozásához, és adja meg hello SSH nevét (TCP-portját 22 hello nyilvános és magánhálózati port számát). További információkért lásd: [beállítása az Azure virtuális gép végpontja](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

<a id="nsg"></a>

## <a name="source-4-network-security-groups"></a>4. forrás: Hálózati biztonsági csoportok
Hálózati biztonsági csoportok lehetővé teszik a toohave részletesebben vezérelhető, engedélyezett bejövő és kimenő forgalom. Létrehozhat szabályokat, amelyek span alhálózatok és a felhőalapú szolgáltatások egy Azure virtuális hálózatra. Ellenőrizze a hálózati biztonsági csoport szabályok tooensure hello Internet engedélyezett az adott SSH forgalom tooand.
További információkért lásd: [hálózati biztonsági csoportok](../../virtual-network/virtual-networks-nsg.md).

IP-ellenőrzése toovalidate hello NSG-konfigurációt is használhatja. További információkért lásd: [áttekintése Azure hálózatfigyelési](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="source-5-linux-based-azure-virtual-machine"></a>5. forrás: Az Azure virtuális Linux-alapú számítógép
hello utolsó lehetséges problémák forrása hello Azure virtuális gép saját magát.

![Diagram, amely kiemeli a Linux-alapú Azure virtuális gépek](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Ha még nem tette meg, lépésekkel hello [tooreset jelszó vagy SSH a Linux-alapú virtuális gépek](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

Próbáljon újra kapcsolódni a számítógépről. Ha továbbra is sikertelen, hello közé tartoznak a következők hello lehetséges problémák:

* hello SSH szolgáltatás nem fut a hello cél virtuális gépen.
* hello SSH szolgáltatás nem figyeli a 22-es TCP-portot. tootest, a telnet ügyfélprogram telepítése a helyi számítógépen, és futtatása "telnet *cloudServiceName*. cloudapp.net 22". Ez a lépés határozza meg, ha hello virtuális gép lehetővé teszi a bejövő és kimenő kommunikáció toohello SSH-végpontot.
* hello helyi hello cél virtuális gép tűzfalának szabályokat, amelyek akadályozzák a bejövő vagy kimenő SSH-forgalmat.
* Behatolásérzékelési vagy hálózatfigyelési hello Azure virtuális gépen futó szoftverek megakadályozza az SSH-kapcsolatokhoz.

## <a name="additional-resources"></a>További források
Alkalmazás-hozzáférés hibaelhárítással kapcsolatos további információkért lásd: [kapcsolatos problémák elhárítása access tooan alkalmazást egy Azure virtuális gépen futó](troubleshoot-app-connection.md)
