---
title: "a Microsoft Azure SUSE Linux virtuális gépeken SAP NetWeaver aaaTesting |} Microsoft Docs"
description: "Az SAP NetWeaver tesztelése Microsoft Azure-beli SUSE Linux-alapú virtuális gépeken"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 645e358b-3ca1-4d3d-bf70-b0f287498d7a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: hermannd
ms.openlocfilehash: 0e3faab05417a1a15541e2b79aa7eddacda44611
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Az SAP NetWeaver futtatása Microsoft Azure-beli SUSE Linux-alapú virtuális gépeken
Ez a cikk ismerteti a különböző dolgok tooconsider, amikor a Microsoft Azure SUSE Linux virtuális gépek (VM) SAP NetWeaver futtatja. 2016. május 19-től SAP NetWeaver hivatalosan támogatott SUSE Linux virtuális gépek Azure-on. Linux-verziók, SAP kernel verziók és egyéb kapcsolatos összes részleteket található SAP Megjegyzés 1928533 "SAP-alkalmazások az Azure-on: a támogatott és az Azure Virtuálisgép-típusokon".
A Linux virtuális gépeken SAP kapcsolatos további dokumentáció itt található: [SAP használata a Linux virtuális gépek (VM)](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

a következő információk hello segítséget néhány nehézségek elkerülése érdekében.

## <a name="suse-images-on-azure-for-running-sap"></a>A SAP futó Azure SUSE lemezképek
SAP NetWeaver Azure-on futó, használja csak SUSE Linux Enterprise Server SLES 12 (SPx) – a Lásd még: SAP Megjegyzés 1928533. Egy különös SUSE kép hello Azure piactér ("SLES 11 SP3 SAP CAL"), de ez nem általános használatra készült. Nem használható ez a rendszerkép, mert le van foglalva a hello [felhő készülék teszteléshez](https://cal.sap.com/) megoldás.  

Az összes új tesztek és telepítése az Azure-on Azure Resource Manager kell használnia. toolook SUSE SLES lemezképeket és az Azure PowerShell vagy hello Azure parancssori felület (CLI), a következő parancsok használata hello segítségével-verzió. Hello kimeneti, például toodefine hello operációsrendszer-lemezképek egy JSON-sablonban új SUSE Linux virtuális gép üzembe helyezéséhez használhatja.
PowerShell-parancsokkal is érvényes az Azure PowerShell-verzió 1.0.1-es és újabb verziók.

Bár ez továbbra is lehetséges toouse hello szabványos SLES képek SAP telepítések ajánlott toomake használatát hello új SLES a rendelkezésre álló SAP-lemezképek mostantól a hello Azure kép gyűjteménye. Ezeket a lemezképeket további információ található hello megfelelő [Azure piactér lap]( https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SUSE.SLES-SAP ) vagy hello [SUSE gyakran ismételt kérdések weblap tudnivalók az SAP SLES]( https://www.suse.com/products/sles-for-sap/frequently-asked-questions/ ).


* Meglévő közzétevők, beleértve a SUSE keresése:
  
   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```
* Keresse meg a SUSE meglévő ajánlatok:
  
   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```
* SUSE SLES ajánlatok keresése:
  
   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP"
   CLI : azure vm image list-skus westeurope SUSE SLES
   CLI : azure vm image list-skus westeurope SUSE SLES-SAP
   ```
* Keresse meg a SLES SKU egy adott verziójához:
  
   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12-SP2"
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP" -skus "12-SP2"
   CLI : azure vm image list westeurope SUSE SLES 12-SP2
   CLI : azure vm image list westeurope SUSE SLES-SAP 12-SP2
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>A virtuális gép SUSE WALinuxAgent telepítése
hello ügynök WALinuxAgent nevű hello SLES képek hello Azure piactér részét képezi. Manuálisan (például egy SLES operációs rendszer virtuális merevlemez (VHD) a helyszíni feltöltésekor) telepítéséről további információkért lásd:

* [OpenSUSE](http://software.opensuse.org/package/WALinuxAgent)
* [Azure](../../linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SUSE](https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP "fokozott figyelés"
Egy kötelező előfeltétel toorun Azure SAP SAP "fokozott figyelés". Ellenőrizze, hogy az SAP részletek vegye figyelembe, hogy 2191498 "SAP lévő Linux rendelkező Azure: fokozott figyelés".

## <a name="attaching-azure-data-disks-tooan-azure-linux-vm"></a>Az Azure data lemezek tooan Azure Linux virtuális gép csatlakoztatása
Kell az Azure data lemezek tooan Azure Linux virtuális gép soha ne csatlakoztassa hello eszköz azonosítójával. Ehelyett használjon hello univerzálisan egyedi azonosítóval (UUID). Legyen óvatos toomount az Azure data lemezek grafikus eszközök, például használatakor. Gondosan ellenőrizze /etc/fstab hello bejegyzéseket.

hello eszközazonosítójú hello probléma, előfordulhat, hogy módosítsa, és majd a hello Azure virtuális gép leállását előfordulhat, hogy hello rendszerindítási folyamat során. toomitigate hello probléma hello nofail paraméter hozzáadhatja a /etc/fstab. De legyen óvatos rendelkező nofail mert alkalmazások használhatja a hello csatlakoztatási pontot előtt, és előfordulhat, hogy írni az hello legfelső szintű fájlrendszer, abban az esetben, ha egy külső az Azure data lemez hello rendszerindítás során nincs csatlakoztatva.

hello egyetlen kivétel toomounting UUID keresztül van csatlakoztatása egy operációsrendszer-lemez a hibaelhárításhoz, hello a következő szakaszban leírtak szerint.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>A SUSE virtuális gép, amely nem érhető el többé hibaelhárítása
Előfordulhat, hogy olyan helyzetekben, ahol az Azure virtuális gép SUSE lefagy hello rendszerindítási folyamat során (például a lemezek csatlakoztatása kapcsolatos hiba). A probléma hello Azure-portálon az Azure virtuális gépek v2 hello rendszerindítási diagnosztikai funkciót használatával ellenőrizheti. További információkért lásd: [rendszerindítási diagnosztika](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Egyirányú toosolve hello probléma a tooattach hello operációsrendszer-lemez hello a virtuális gép tooanother SUSE Azure virtuális gép sérült. Hello a következő szakaszban leírt módon végezze el például /etc/fstab szerkesztésével, vagy eltávolítani hálózati udev megfelelő módosításokat.

Egy fontos tooconsider van. Az operációs rendszer telepítése több SUSE VMs hello azonos Azure Piactéri lemezképhez (például az SLES 11 SP4) hatására a hello lemez tooalways csatlakoztatni hello azonos UUID azonosítója. Ezért használatával hello UUID tooattach egy operációsrendszer-lemez egy másik virtuális gépről, amelyek azonos Azure Piactéri lemezképhez eredményez két azonos UUID-k hello használatával lett telepítve. Ez problémákat okoz, és azt is jelentheti, hogy azt jelentette, hogy a hibaelhárítási VM hello ténylegesen indul el a csatolt hello és ahelyett, hogy sérült az operációsrendszer-lemez hello egy-egy.

Nincsenek két módon tooavoid ezt:

* Használjon egy másik Azure piactér lemezképet hello hibaelhárítás a virtuális gép (például SLES 11 SPx SLES 12 helyett).
* Ne csatlakoztassa sérült hello operációsrendszer-lemez egy másik virtuális gép segítségével UUID--használata valami mással.

## <a name="uploading-a-suse-vm-from-on-premises-tooazure"></a>A helyszíni tooAzure SUSE virtuális feltöltése
Hello lépéseket tooupload egy helyszíni tooAzure SUSE virtuális gép ismertetését lásd: [SLES vagy openSUSE virtuális gép előkészítése Azure](../../linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ha azt szeretné, hogy egy virtuális gép nélküli hello deprovision lépést (például SAP telepített tookeep, valamint hello állomásnév) hello végén tooupload, ellenőrizze a következő elemek hello:

* Győződjön meg arról, hogy az operációs rendszer hello lemez csatlakoztatva van a UUID és nem hello eszközazonosító. Csak a /etc/fstab tooUUID módosítása nincs elég hello operációsrendszer-lemezzel. Továbbá ne feledje tooadapt hello rendszertöltőt YaST vagy /boot/grub/menu.lst szerkesztésével.
* Ha hello VHDX formátum használata hello SUSE operációsrendszer-lemez, és alakítsa át a tooVHD tooAzure feltöltése, nagyon valószínű, hogy hello hálózati eszköz állapotúról eth0 tooeth1. tooavoid problémák indítása során átnevezés, még az Azure-on később, módosítsa a hátsó tooeth0, a [kijavítása az eth0 klónozott SLES 11 VMware](https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Ezenkívül toowhat tartozó hello cikkben ismertetett, javasoljuk, hogy eltávolítja ezt:

   /lib/udev/rules.d/75-persistent-NET-Generator.Rules

Hello Azure Linux ügynök (waagent) toohelp elkerülheti a lehetséges problémák kezeléséhez, mindaddig, amíg nincsenek több hálózati adapterrel is telepíthet.

## <a name="deploying-a-suse-vm-on-azure"></a>A SUSE Azure virtuális gép telepítése
Új SUSE virtuális gépek hello új Azure Resource Manager modellt a JSON-sablon fájlok használatával kell létrehoznia. Hello JSON-sablon fájl létrehozása után telepíthet hello VM hello egy alternatív tooPowerShell CLI parancsot a következő használatával:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
JSON-sablon fájlokkal kapcsolatos további részletekért lásd: [Azure Resource Manager-sablonok készítése](../../../resource-group-authoring-templates.md) és [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/).

Parancssori felület és az Azure Resource Manager kapcsolatos további tudnivalókért lásd: [használata hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager](../../../xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP licenc- és hardver kulcs
Egy új mechanizmus hello hivatalos SAP-Azure hitelesítő bevezetett toocalculate hello SAP hardver kulcs hello SAP licenc használt volt. hello SAP kernel kellett igazítani toobe toomake használata az e. Linux-SAP kernel korábbi verziók nem tartalmazta a kód módosítása. Ezért, bizonyos esetekben (például az Azure virtuális gép átméretezésével), hello SAP hardver kulcs megváltozott, és hello SAP licenc lett már nem érvényes. Ez a hello legújabb SAP Linux kernelek lehet megoldani. A részletekért ellenőrizze a SAP Megjegyzés 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf csomag / beállított adm
SUSE "sapconf", amely felügyeli az SAP-specifikus beállításokat készlete nevű csomagot biztosít. Milyen ezzel a csomaggal kapcsolatos további részletekért konfigurációszolgáltatóval, és hogyan tooinstall és használni, lásd: [sapconf tooprepare a SUSE Linux Enterprise Server toorun SAP rendszert használnak](https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) és [sapconf Újdonságok vagy hogyan tooprepare a SUSE Linux Enterprise Az SAP rendszert futtató kiszolgáló? ](http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Hello addig van egy új eszközt, amely a felváltja sapconf - beállított .adm kiterjesztésre Ez az eszköz a következő hello két a lenti hivatkozásokra kattintva további információt talál egy.

SLES beállított adm profil sap-hana dokumentációjában található [Itt](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

A beállított-adm - SAP munkaterheléseknél hangolási rendszerek található [Itt](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) fejezet 6.2

## <a name="nfs-share-in-distributed-sap-installations"></a>Az NFS-megosztások SAP elosztott telepítés
Ha egy elosztott telepítés – például, amelyen szeretné, hogy tooinstall hello adatbázis és külön virtuális gépek – SAP alkalmazáskiszolgálóinak hello megoszthatja hello /sapmnt directory keresztül hálózati fájlrendszer (NFS). Ha problémába ütközik az hello telepítési lépéseket hello /sapmnt az NFS-megosztás létrehozása után, toosee ellenőrizze, hogy a "no_root_squash" hello megosztás van beállítva.

## <a name="logical-volumes"></a>Logikai kötetek
Az elmúlt hello Ha több Azure adatok lemezre (például hello SAP-adatbázist), a logikai nagy egy szükséges volt célszerű toouse mdadm, lvm nincs teljesen ellenőrizve még az Azure-on. Hogyan tooset mentése az Azure-on Linux RAID mdadm, használatával: toolearn [szoftveres RAID Linux konfigurálása](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 2016. május elejére frissítésétől addig hello is lvm teljes mértékben támogatja az Azure-on és egy alternatív toomdadm használható. Az Azure-on lvm kapcsolatos további információk: [LVM konfigurálása a Linux virtuális gép az Azure-ban](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-suse-repository"></a>Azure SUSE-tárházat
Hozzáférés toohello szabványos Azure SUSE tárház probléma lehet, ha egy egyszerű parancs tooreset használhatja azt. Ez akkor fordulhat elő, a saját operációsrendszer-lemezképek létrehozásakor az Azure-régiót, majd a Másolás hello kép tooa más régióban toodeploy, ahová a saját operációsrendszer-lemezképek alapuló új virtuális gépeket. Egyszerűen csak futtatnia kell a következő parancs belül hello VM hello:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome asztali
Toouse hello Gnome asztali tooinstall egy teljes SAP bemutató rendszer belül egyetlen virtuális gép – SAP grafikus felhasználói Felülettel, beleértve a böngésző és SAP felügyeleti konzol--használja a mutató tooinstall hello Azure SLES képek azt:

   Az SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   A SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-hello-cloud"></a>Oracle Linux hello felhőben SAP támogatása
Nincs a virtualizált környezetet az Oracle Linux támogatás korlátozás. Bár ez nem egy Azure-specifikus témakör, továbbra is fontos toounderstand. SAP nem támogatja a Oracle SUSE vagy nyilvános felhőben működő Azure például Red Hat. toodiscuss ebben a témakörben Oracle forduljon közvetlenül.

