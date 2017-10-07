---
title: "Virtuális gépek – áttekintés aaaWindows |} Microsoft Docs"
description: "Tudnivalók Windows rendszerű virtuális gépek létrehozásáról és kezeléséről az Azure-ban."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: fbae9c8e-2341-4ed0-bb20-fd4debb2f9ca
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 8015b1aa4bcdaac2e721f25d85a5bc995a22f0f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-windows-virtual-machines-in-azure"></a>Windows rendszerű virtuális gépek áttekintése az Azure-ban

Az Azure Virtual Machines (VM) az Azure által kínált számos különböző típusú, [igény szerinti, méretezhető számítási erőforrás](../../app-service-web/choose-web-site-cloud-service-vm.md) közé tartozik. Általában választható a virtuális gépek számítási környezet hello teljesebb körű vezérlése hello egyéb lehetőségek kínálnak, mint van szüksége. Ez a cikk bemutatja, hogy mit kell szem előtt tartania egy virtuális gép létrehozása előtt, valamint hogy hogyan hozhatja létre és kezelheti azt.

Egy Azure virtuális gép által biztosított biztosítják a virtualizálás rugalmasságát hello anélkül, hogy toobuy és karbantartása hello fizikai hardver, amely futtatja. Azonban továbbra is szükség toomaintain hello VM feladatokat, például a, javítását és a rajta futó hello szoftverek telepítése.

Az Azure virtuális gépek különféle módon használhatóak. Néhány példa:

* **Fejlesztési és tesztelési** – Azure virtuális gépek ajánlatot egy gyors és egyszerű módot toocreate konfigurációkkal rendelkező számítógép szükséges toocode és alkalmazás.
* **Alkalmazások hello felhőben** – az alkalmazás iránti igény is ingadozik, mert azt tehetik gazdaságossági megfontolásból érdemes toorun azt a virtuális gép az Azure-ban. A további virtuális gépekért csak akkor kell fizetnie, amikor szüksége van rájuk, amikor pedig nincs, akkor leállíthatja őket.
* **Datacenter kiterjesztett** – egy Azure virtuális hálózatban lévő virtuális gépek is lehetnek csatlakoztatott tooyour szervezet hálózatához.

hello beállított virtuális gépeket, az alkalmazás által költenie és kimenő toowhatever szükséges toomeet igényeinek.

## <a name="what-do-i-need-toothink-about-before-creating-a-vm"></a>Mit kell kapcsolatos toothink a virtuális gépek létrehozása előtt?
Az Azure-ban futó alkalmazás-infrastruktúrák kiépítésekor mindig számos [kialakítási szempontot](/architecture/reference-architectures/virtual-machines-linux?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kell figyelembe venni. A virtuális gépek ezeket az jellemzőket készül fontos toothink megkezdése előtt:

* az alkalmazás-erőforrásokat hello nevei
* hello hello erőforrások tárolási helyének
* virtuális gép hello hello mérete
* hello hozható létre virtuális gépek maximális száma
* virtuális gép hello hello operációs rendszer fut.
* Miután elindult VM hello hello konfigurálása
* hello kapcsolódó erőforrások VM kell, hogy hello

### <a name="naming"></a>Elnevezés
A virtuális gépek egy [neve](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hozzárendelt tooit és hello operációs rendszer részeként konfigurált számítógép neve van. a virtuális gépek hello neve mentése too15 karakter lehet.

Ha az Azure toocreate hello operációsrendszer-lemez, hello számítógépnevet használhatja, és hello virtuális gép neve hello azonos. Ha Ön [feltöltése és saját rendszerkép használata](upload-generalized-managed.md) , amely tartalmazza a korábban konfigurált operációs rendszer, és használja a virtuális gépek toocreate, hello nevek eltérő lehet. Azt javasoljuk, hogy a saját képfájl feltöltésekor tegyen hello számítógép hello operációs rendszerben és hello virtuális gép nevét hello azonos.

### <a name="locations"></a>Helyek
Az Azure-ban létrehozott összes erőforrás több különböző pontjain [földrajzi régió](https://azure.microsoft.com/regions/) hello world körül. Általában hello régió nevezik **hely** egy virtuális gép létrehozásakor. A virtuális gépek hello hely határozza meg, hello virtuális merevlemezek tárolására.

Az alábbi táblázatban néhány hello többféleképpen kaphat a rendelkezésre álló helyek listáját.

| Módszer | Leírás |
| --- | --- |
| Azure Portal |Jelöljön ki egy helyet hello listában, ha a virtuális gép létrehozása. |
| Azure PowerShell |Használjon hello [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation) parancsot. |
| REST API |Használjon hello [helyek listában](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_ListLocations) műveletet. |

### <a name="vm-size"></a>Virtuális gép mérete
Hello [mérete](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello, amelyekkel VM meg hello munkaterhelést, amelyet az toorun. hello telepítheti majd határozza meg tényezőket, mint a tápellátáshoz, a memória és a tárolási kapacitás feldolgozása. Az Azure kínál méretek toosupport számos számos különböző típusú használ.

Azure percalapú egy [óránkénti ár](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) hello virtuális gép mérete és az operációs rendszer alapján. A megkezdett órák esetében az Azure csak percig hello használt költségek. A tárhely árazása és felszámítása külön történik.

### <a name="vm-limits"></a>A virtuális gépekre korlátai
Az előfizetéshez tartozik alapértelmezett [kvótakorlát](../../azure-subscription-service-limits.md) , amely jelentős hatással lehet a projekthez sok virtuális gép hello telepítését. hello / előfizetés alapján a jelenlegi korlátozását régiónként 20 virtuális gép. A határértékek megemelhetők egy emelést kérvényező támogatási jegy benyújtásával.

### <a name="operating-system-disks-and-images"></a>Operációsrendszer-lemezek és -rendszerképek
A virtuális gépek használhatnak [virtuális merevlemezeket (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toostore az operációs rendszer és az adataikat. Virtuális merevlemezek választhat egy operációs rendszer tooinstall hello lemezképek is használhatók. 

Azure biztosítja a [piactéren elérhető rendszerkép](https://azure.microsoft.com/marketplace/virtual-machines/) toouse a különböző verziói és a Windows Server operációs rendszerek. A piactérről származó rendszerképek azonosítása a rendszerkép közzétevője, ajánlat, termékváltozat és verzió alapján lehetséges (a verzió általában mint „legfrissebb” van megadva). 

Az alábbi táblázatban néhány módját, amely a rendszerképek hello információt.

| Módszer | Leírás |
| --- | --- |
| Azure Portal |Amikor kiválaszt egy kép toouse hello értékek automatikusan megadva meg. |
| Azure PowerShell |[Get-AzureRMVMImagePublisher](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimagepublisher) -Location "location"<BR>[Get-AzureRMVMImageOffer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimageoffer) -Location "location" -Publisher "publisherName"<BR>[Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) -Location "location" -Publisher "publisherName" -Offer "offerName" |
| REST API-k |[Rendszerkép-közzétevők listázása](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<BR>[Rendszerkép-ajánlatok listázása](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<BR>[Rendszerkép-termékváltozatok listázása](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus) |

Dönthet úgy, túl[feltöltése és saját rendszerkép használata](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account) , és ha így tesz, hello közzétevő neve, az ajánlat és a termékváltozat nem használja.

### <a name="extensions"></a>Bővítmények
A virtuális gépek [bővítményei](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) további hozzáadott képességekkel ruházzák fel a virtuális gépeket az üzembe helyezést követő konfigurálás és automatizált feladatok útján.

A bővítményekkel a következő gyakori feladatok végezhetők el:

* **Egyéni parancsfájlok futtathatók** – hello [egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) segít munkaterhelések konfigurálja a virtuális gép hello amikor hello virtuális gép ki van építve a parancsfájl futtatásával.
* **Telepíthetnek és kezelhetnek olyan konfigurációk** – hello [PowerShell kívánt állapot konfigurációs szolgáltatása (DSC) bővítmény](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) segítséget nyújt a virtuális gép toomanage a állít be a DSC-konfiguráció és a környezetben.
* **Diagnosztikai adatok gyűjtéséhez** – hello [Azure Diagnostics bővítmény](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello VM toocollect diagnosztikai adatok, amelyek lehetnek használt toomonitor konfigurálása segíti az alkalmazás állapotának hello.

### <a name="related-resources"></a>Kapcsolódó források (lehet, hogy a cikkek angol nyelvűek)
Ebben a táblázatban szereplő hello erőforrások hello virtuális gép által használt tooexist kell, vagy hozható létre, ha a virtuális gép hello jön létre.

| Erőforrás | Kötelező | Leírás |
| --- | --- | --- |
| [Erőforráscsoport](../../azure-resource-manager/resource-group-overview.md) |Igen |virtuális gép hello szerepelnie kell egy erőforráscsoportot. |
| [Storage-fiók](../../storage/common/storage-create-storage-account.md) |Igen |virtuális gép hello hello tárolási fiók toostore annak virtuális merevlemezei van szüksége. |
| [Virtuális hálózat](../../virtual-network/virtual-networks-overview.md) |Igen |hello virtuális gép virtuális hálózat tagjának kell lennie. |
| [Nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) |Nem |hello VM rendelkezhet egy nyilvános IP cím tooit tooremotely-e férni. |
| [Hálózati illesztő](../../virtual-network/virtual-network-network-interface.md) |Igen |virtuális gép hello hello hálózati illesztő toocommunicate hello hálózatban kell. |
| [Adatlemezek](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |Nem |hello VM tartalmazhatnak adatokat lemezek tooexpand tárolási lehetőségeket. |

## <a name="how-do-i-create-my-first-vm"></a>Hogyan hozhatom létre az első virtuális gépemet?
A virtuális gépek létrehozásakor számos választási lehetőség áll rendelkezésre. hello választott, akkor a hello környezettől függ. 

Ez a táblázat információkat tooget használatba a virtuális gép létrehozása.

| Módszer | Cikk |
| --- | --- |
| Azure Portal |[Hozzon létre egy olyan virtuális géphez a Windows hello-portál használatával](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Sablonok |[Windows rendszerű virtuális gép létrehozása egy Resource Manager-sablonnal](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure PowerShell |[Windows rendszerű virtuális gép létrehozása a PowerShell használatával](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Ügyfél-SDK-k |[Azure erőforrások üzembe helyezés a C# használatával](csharp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| REST API-k |[Virtuális gépek létrehozása vagy frissítése](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-create-or-update) |

Reménykedhet, hogy sosem következik be, de várhatóan időnként elromlik valami. Ha ez a helyzet akkor fordul elő, tooyou, tekintse meg hello a [egy Windows virtuális gép létrehozása az Azure erőforrás-kezelő hibaelhárítása telepítési problémákat](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="how-do-i-manage-hello-vm-that-i-created"></a>Hogyan kezelhetők a virtuális Gépet, amely a létrehozott hello?
A virtuális gépek felügyelhetők egy böngészőalapú portállal, parancsfájlkezelést támogató parancssori eszközökkel, vagy közvetlenül az API-kon keresztül. Néhány tipikus felügyeleti feladatokhoz, előfordulhat, hogy végre kihozhatják információ a virtuális gépek, virtuális gépek rendelkezésre állásának kezelése, és a biztonságimásolat-készítő tooa bejelentkezés.

### <a name="get-information-about-a-vm"></a>Virtuális gép adatainak lekérése
Az alábbi táblázatban, néhány hello módon, hogy a virtuális gépek információkat kaphat.

| Módszer | Leírás |
| --- | --- |
| Azure Portal |Hello központ menüben kattintson a **virtuális gépek** majd válassza ki a virtuális gép hello hello listából. Virtuális gép hello hello paneljén, hogy toooverview adatok eléréséhez, beállításértékek és figyelési metrikákat. |
| Azure PowerShell |PowerShell toomanage virtuális gépek használatával kapcsolatos információkért lásd: [létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| REST API |Használjon hello [beolvasása Virtuálisgép-adatok](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get) művelet tooget információ a virtuális gép. |
| Ügyfél-SDK-k |C# toomanage virtuális gépek használatával kapcsolatos információkért lásd: [kezelése Azure virtuális gépek Azure Resource Manager és a C# használatával](csharp-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |

### <a name="log-on-toohello-vm"></a>Jelentkezzen be a virtuális gép toohello
Hello Connect vissza gombját használja hello Azure-portálon túl[indítsa el a távoli asztal (RDP) munkamenet](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dolgot is néha hiba lép fel a távoli kapcsolat toouse tett kísérlet során. Ha ez a helyzet akkor fordul elő, tooyou, tekintse meg a hello információk [hibaelhárítása távoli asztali kapcsolatok tooan Azure Windows rendszerű virtuális gép](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="manage-availability"></a>Rendelkezésre állás kezelése
Fontos, toounderstand hogyan túl[magas rendelkezésre állásának biztosításához](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) az alkalmazáshoz. Ez a konfiguráció magában foglalja a több virtuális gépek tooensure legalább egy futtató létrehozása.

Ahhoz, hogy a központi telepítési tooqualify a 99.95 VM szolgáltatási szint szerződés, kell toodeploy belül a munkaterhelést futtató két vagy több virtuális gépet egy [rendelkezésre állási csoport](tutorial-availability-sets.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ez a konfiguráció biztosítja, hogy a virtuális gépek több tartalék tartomány között oszoljanak meg, és az őket futtató gazdagépeknek különböző karbantartási időszakaik legyenek. teljes hello [Azure garantált szolgáltatási szintje](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) ismerteti hello Azure egész rendelkezésre állását garantálja.

### <a name="back-up-hello-vm"></a>Készítsen biztonsági másolatot a virtuális gép hello
A [Recovery Services-tároló](../../backup/backup-introduction-to-azure-backup.md) használt tooprotect az adatok és eszközök is az Azure Backup, és az Azure Site Recovery szolgáltatásban. Recovery Services-tároló használhatja túl[telepítéséhez és kezeléséhez biztonsági mentések erőforrás-kezelő telepített virtuális gépek PowerShell-lel](../../backup/backup-azure-vms-automation.md). 

## <a name="next-steps"></a>Következő lépések
* Ha a szándéka az, a Linux virtuális gépek toowork, tekintse meg [Azure és a Linux](../linux/overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* További tudnivalók az infrastruktúrát a hello beállítása körül hello irányelvek [példa Azure infrastruktúra forgatókönyv](infrastructure-example.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
