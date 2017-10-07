---
title: "Linux virtuális gépek Azure-ban aaaOverview |} Microsoft Docs"
description: "A Linux virtuális gépek Azure számítási, tárolási és hálózati szolgáltatások ismerteti."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: rickstercdn
manager: timlt
editor: 
ms.assetid: 7965a80f-ea24-4cc2-bc43-60b574101902
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 958e219d53026d787a7a41f2fd13c29c739a9238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-and-linux"></a>Azure és Linux
Microsoft Azure integrált nyilvános egyre bővülő gyűjteménye a webes és a felhőalapú szolgáltatások elemzés, virtuális gépek, adatbázisok, mobil-, hálózati, tárolási, beleértve&mdash;épp ezért tökéletes választás a megoldások üzemeltetéséhez.  A Microsoft Azure, amely lehetővé teszi a valóban használt funkciókért, ha azt szeretné, hogy - anélkül, hogy tooinvest a helyszíni hardverre tooonly fizetési méretezhető számítási platformot biztosít.  Azure készen áll, amikor tooscale a megoldások fel, és a kimenő toowhatever méretezési tooservice hello az ügyfelek igényeinek megfelelően.

Ha ismeri a hello Amazon meg különböző funkcióhoz AWS, tekintse meg az Azure vs AWS hello [definition leképezési dokumentum](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

## <a name="regions"></a>Régiók
A Microsoft Azure-erőforrásokat több földrajzi régióban hello világ különböző pontjain.  Egy "régió" azonos földrajzi területen több különböző adatközponthoz jelöli.  34 általánosan elérhető régiók hello világ, egy további 4 régiók bejelentette a tudunk. Mivel jelenleg továbbra tooexpand a globális érvényességének - azt karbantartása meglévő és újonnan frissített listáját bejelentette régiók.

* [Azure-régiók](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Rendelkezésre állás
Azt jelentette be egy iparágban vezető egypéldányos virtuális gép szolgáltatásiszint-megállapodás 99,9 %-a megadott hello virtuális gép összes lemeze a prémium szintű Storage telepít.  Ahhoz, hogy a szabványos 99,95 %-a központi telepítés tooqualify VM szolgáltatásiszint-megállapodás, továbbra is meg kell toodeploy két vagy több virtuális gépek rendelkezésre állási csoportok belül a munkaterhelések futtatásával. Ezzel biztosíthatja, hogy a virtuális gépek vannak elosztva a adatközpontokban több tartalék tartományok között, valamint a különböző karbantartási időszakok gazdagépekre telepített. teljes hello [Azure garantált szolgáltatási szintje](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) ismerteti hello Azure egész rendelkezésre állását garantálja.

## <a name="managed-disks"></a>Felügyelt lemezek

Által kezelt lemezeken Azure Storage leírók fióklétrehozás és a felügyeleti hello háttérben, és biztosítja, hogy nem rendelkezik tooworry kapcsolatos hello méretezhetőségi korlátok hello tárfiók. Egyszerűen hello lemezméretet és hello teljesítményszinttel (Standard vagy prémium), és a Azure hoz létre, és kezeli a hello lemez meg. Akár lemezek hozzáadása, illetve a virtuális gép hello felfelé és lefelé méretezési, nincs használatban hello tárolás tooworry. Ha hoz létre új virtuális gépek, [hello Azure CLI 2.0 használja](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy az Azure portál toocreate virtuális gépek a felügyelt operációsrendszer- és adatlemezek hello. Ha a virtuális gépek nem felügyelt lemezzel rendelkezik, akkor [alakítsa át a virtuális gépek toobe kezelt lemezek biztonsági](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Is kezelheti az egyéni lemezképek az egyes Azure-régió, egy tárfiókot, és használhatja őket több száz. toocreate hello a virtuális gépek ugyanahhoz az előfizetéshez. A felügyelt lemezek kapcsolatos további információkért lásd: hello [felügyelt lemezekhez – áttekintés](../windows/managed-disks-overview.md).

## <a name="azure-virtual-machines--instances"></a>Az Azure Virtual Machines- & példányok
A Microsoft Azure támogatja futtató számos népszerű Linux terjesztésekről megadott és a partnerei számos tartja fenn.  Azokat a terjesztéseket, mint az Red Hat Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, freebsd rendszerű, a hello Azure piactér találja. Aktívan dolgozunk az különböző Linux Közösségek tooadd még több változatban is elkészíti toohello [Azure által támogatott Linux Disztribúciókkal](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) listája.

Ha az előnyben részesített Linux distro választott nincs, a rendszer jelenleg hello gyűjteményben "helyezheti a saját Linux" virtuális gép által [létrehozása és feltöltése az Azure Linux virtuális merevlemez](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Az Azure virtuális gépek toodeploy megoldások számítási gyors módon széles engedélyezése. Szinte bármilyen munkaterhelést és szinte bármilyen operációs rendszer – Windows, Linux, a ismeretcikkekkel is telepíthet, vagy egy egyéni létre egyet a partnerek egyre bővülő listáját. Még nem látja éppen megtekintett?  Ne aggódjon, mert a helyszíni is helyezheti a saját lemezképek.

## <a name="vm-sizes"></a>Virtuálisgép-méretek
Amikor telepít egy Azure-ban, a Virtuálisgép-méretet a sorozat méretének megfelelő tooyour munkaterhelés egyik tooselect fog. hello mérete befolyásolja a feldolgozási teljesítmény, a memória és a tárolási kapacitás hello virtuális gép hello is. A virtuális gép fut, és a kiosztott erőforrásokat fel idő hello hello mennyisége alapján kell fizetni. Teljes listáját [a virtuális gépek méretét](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Az alábbiakban néhány alapvető útmutatók kijelölése az adatsorozat (A, D, DS, G és GS) egyik Virtuálisgép-méretet.
* A-sorozatú virtuális gépek a könnyű munkaterhelések és szolgáltatásának fejlesztési/tesztelési kezdő virtuális gépek áron érték. Hogy széles körben elérhető, amelyek minden régióban és csatlakozni, és az összes szabványos erőforrások elérhető toovirtual gép.
* (A8 - A11) A-sorozatú értékek különleges számítási intenzív konfigurációk nagy teljesítményű számítástechnikai fürt alkalmazásoknál.
* D sorozatú virtuális gépek rendszer nagyobb számítási teljesítményt és a ideiglenes lemezek igény tervezett toorun alkalmazás. D sorozatú virtuális gépek hello mennyiségű ideiglenes lemezes gyorsabb processzorok, memória-core magasabb arány és egy SSD-meghajtót (SSD) adja meg.
* Dv2-sorozat, a D sorozat hello legújabb verzióját, nagyobb teljesítményű CPU szolgáltatásokat. hello Dv2-sorozat CPU pedig körülbelül 35 % gyorsabb, mint az hello D sorozatú Processzor. Alapul hello legújabb generációját 2,4 GHz-es Intel Xeon® E5-2673 v3 (Haskell) processzor, és a hello Intel Turbo program technológia 2.0, lépjen be too3.2 GHz-es. hello Dv2-sorozat rendelkezik hello memóriát és lemezterületet ugyanazokat a konfigurációkat, a D sorozat hello.
* G sorozatú virtuális gépek hello kínálnak a legtöbb memóriát, majd futtassa rendelkező Intel Xeon E5 V3 rendszerű gazdagépeken.

Megjegyzés: A DS-méretek és GS sorozatnak virtuális gépek rendelkezik hozzáférési tooPremium tárolási – az SSD biztonsági nagy teljesítményű, alacsony késésű tárolási i/o-igényes munkaterhelések esetében. A Premium Storage szolgáltatás csak bizonyos régiókban érhető el. Részletes információ:

* [Prémium szintű Storage: Nagy teljesítményű tárolást az Azure virtuális gépek terheléséhez](../../storage/common/storage-premium-storage.md)

## <a name="automation"></a>Automatizálás
egy megfelelő DevOps kulturális környezet, az összes infrastruktúra tooachieve kódot kell lennie.  Ha az összes hello infrastruktúra kód könnyen használható, be újra (Phoenix kiszolgálók).  Azure összes hello fő automation tooling eszköz Ansible, Chef, SaltStack és Puppet hasonlóan működik.  Azure automation a saját eszközt használunk erre is rendelkezik:

* [Azure-sablonok](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Az Azure vmaccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure támogatás bevezetéséről [felhő inicializálás](http://cloud-init.io/) között, amelyek támogatják ezt a legtöbb Linux-Disztribúciókkal.  Canonical tartozó Ubuntu virtuális gépek jelenleg a felhő inicializálás alapértelmezés szerint engedélyezve vannak telepítve.  Piros kalap RHEL, CentOS és felhő inicializálás támogatja, azonban hello Azure Fedora RedHat által fenntartott lemezképek nincs felhő inicializálás telepítve.  toouse felhő inicializálás egy RedHat termékcsalád operációs rendszer, létre kell hoznia egy egyéni lemezképet felhő inicializálás telepítve.

* [Az Azure Linux virtuális gépeken futó felhőalapú inicializálás használatával](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Kvóták
Minden Azure-előfizetéssel rendelkezik alapértelmezett kvótát, amely jelentős hatással lehet a hello projektjéhez tartozó virtuális gépek nagy mennyiségű központi telepítését. hello / előfizetés alapján a jelenlegi korlátozását régiónként 20 virtuális gép.  Kvóták is gyorsan következik be, és könnyen egy támogatási jegy maximális kérő tervátalakítási növelheti.  További részletekért a kvótakorlát:

* [Azure-előfizetés szolgáltatásra vonatkozó korlátozások](../../azure-subscription-service-limits.md)

## <a name="partners"></a>Partnerek
Microsoft szorosan együttműködik olyan partnereink tooensure hello lemezképeket frissített és egy Azure futtatókörnyezetet optimalizálva.  Partnereink további információt az alábbi piactér lapok ellenőrizze.

* Az Azure - Linux [által támogatott Disztribúciók](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* SUSE - [Azure piactér - SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=%27SUSE%27)
* Redhat - [Azure piactér - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)
* Canonical - [Azure piactér - Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian - [Azure piactér – Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)
* Freebsd rendszerű - [Azure piactér - freebsd rendszerű 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
* CoreOS - [Azure piactér - CoreOS (stabil)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)
* RancherOS - [Azure piactér - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)
* Bitnami - [Bitnami könyvtár az Azure-bA](https://azure.bitnami.com/)
* Mesosphere - [Azure piactér - Mesosphere DC/OS az Azure-on](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)
* Docker - [Azure piactér – a Docker Swarm Azure Tárolószolgáltatás](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)
* Jenkins - [Azure piactér - CloudBees Jenkins Platform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-started-with-linux-on-azure"></a>Ismerkedés az Azure-on Linux
az Azure-fiók, a hello Azure CLI telepítése és az SSH nyilvános és titkos kulcsok két van szükség Azure használatával toobegin.

### <a name="sign-up-for-an-account"></a>Fiók létrehozása
hello első lépéseként hello Azure felhőbe használatával toosign egy Azure-fiókot.  Nyissa meg toohello [Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/) lépések tooget lapon.

### <a name="install-hello-cli"></a>Hello parancssori felület telepítése
Az új Azure-fiókjával, a is azonnal használatának megkezdésében hello Azure portál, amely egy webalapú felügyeleti panel.  hello telepítését toomanage hello Azure felhőbe parancssori hello keresztül, `azure-cli`.  Telepítse a hello [Azure CLI 2.0](/cli/azure/install-azure-cli) a Mac vagy Linux számítógépen.

### <a name="create-an-ssh-key-pair"></a>SSH-kulcs létrehozása
Most, hogy az Azure-fiók, hello Azure webportálon és hello Azure parancssori felület.  következő lépés hello toocreate egy SSH-kulcspárral, amely használt tooSSH ból Linuxba jelszó nélkül.  [SSH-kulcsok létrehozása Linux és Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooenable jelszó nélküli bejelentkezéseket és nagyobb fokú biztonságot.

### <a name="create-a-vm-using-hello-cli"></a>Hello CLI virtuális gép létrehozása
Linux virtuális gép létrehozása hello CLI használata egy gyorsan toodeploy egy virtuális gép Hangpostaüzenetek hello terminál használata nélkül.  Hello webportál megadhatja rendelkezésre álló parancssori jelző vagy kapcsolón keresztül.  

* [Linux virtuális gép létrehozása hello parancssori felület](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-hello-portal"></a>Hozzon létre egy virtuális gép hello portálon
Linux virtuális gép létrehozása hello Azure web portal alkalmazásban módja a tooeasily ponton, majd kattintson a hello különböző beállítások tooget tooa központi telepítés.  Vagy parancssori kapcsolók helyett képes tooview töltött webes elrendezést a különböző lehetőségek és beállítások.  Mindent rendelkezésre hello parancssori felületen keresztül érhető el a hello portálon.

* [Linux virtuális gép létrehozása hello portál](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>Bejelentkezés SSH-jelszó nélkül
hello VM most már fut az Azure-on, és készen áll a toolog a.  SSH-kapcsolaton keresztül a jelszavak toolog használata nem biztonságos és sok időt vesz igénybe.  SSH-kulcsokat használó legbiztonságosabb módja, valamint a hello leggyorsabb módon toologin van hello.  Létrehozásakor, a Linux rendszerű virtuális hello portál vagy a hello CLI keresztül, két hitelesítés lehetősége van.  Ha az SSH egy jelszót, az Azure hello VM tooallow bejelentkezések jelszavak keresztül konfigurálja.  Ha úgy dönt, toouse nyilvános SSH-kulcsot, Azure konfigurálása hello VM tooonly lehetővé SSH-kapcsolaton keresztül bejelentkezések kulcsokat, és letiltja a jelszavas bejelentkezéseket. szerint csak a Linux virtuális gép SSH-kulcs bejelentkezéseket, így használja toosecure hello SSH nyilvános kulcs lehetőséggel hello hello portálon vagy a parancssori felület a Virtuálisgép-létrehozása során.

## <a name="related-azure-components"></a>Az Azure kapcsolódó összetevők
## <a name="storage"></a>Storage
* [Azure Storage bemutatása tooMicrosoft](../../storage/common/storage-introduction.md)
* [A lemez tooa Linux virtuális gép hello azure-cli hozzáadása](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Hogyan tooattach az adatok lemezre tooa Linux virtuális gép a hello Azure-portálon](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Hálózat
* [Virtual Network áttekintése](../../virtual-network/virtual-networks-overview.md)
* [IP-címek az Azure-ban](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Az Azure-ban portok tooa Linux virtuális gép megnyitása](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Hozzon létre egy teljesen minősített tartománynevét hello Azure-portálon](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Tárolók
* [Virtuális gépek és a tárolók az Azure-ban](containers.md)
* [Azure Tárolószolgáltatás bemutatása](../../container-service/container-service-intro.md)
* [Az Azure Container Service-fürt üzembe helyezése](../../container-service/dcos-swarm/container-service-deployment.md)

## <a name="next-steps"></a>Következő lépések
Most már rendelkezik Azure-on Linux áttekintését.  hello következő lépés a toodive, és néhány virtuális gépek létrehozása!

* [Mintaparancsfájlok egyre bővülő listáját általános feladatokhoz felfedezés AzureCLI keresztül](cli-samples.md)
