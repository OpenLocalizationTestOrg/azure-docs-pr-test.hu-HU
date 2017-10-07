---
title: "aaaAzure Linux virtuális gépek és az Azure Storage |} Microsoft Docs"
description: "Ismerteti az Azure Standard és prémium szintű Storage és a felügyelt és nem felügyelt lemezek Linux virtuális gépekkel."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: d364c69e-0bd1-4f80-9838-bbc0a95af48c
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 2/7/2017
ms.author: rasquill
ms.openlocfilehash: d34441698a4e59721847685099e5fb3aa378c597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-and-linux-vm-storage"></a>Azure és a Linux virtuális gép tárolása
Az Azure Storage egy hello felhőalapú tárolómegoldás modern alkalmazások tartósságot, rendelkezésre állását és méretezhetőségét toomeet hello az ügyfelek igényeinek megfelelően.  A hozzáadása toomaking azt lehetséges a fejlesztők toobuild nagyméretű alkalmazások toosupport új forgatókönyvek használhatók, Azure Storage is hello tárolási alapot nyújt az Azure virtuális gépek.

## <a name="managed-disks"></a>Felügyelt lemezek

Azure virtuális gépek is elérhető használatával [Azure felügyelt lemezek](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), amely lehetővé teszi, hogy Ön toocreate a virtuális gépek létrehozása és kezelése bármely nélkül [Azure Storage-fiókok](../../storage/common/storage-introduction.md) magát. Megadhatja, hogy azt szeretné, hogy prémium vagy standard szintű tárolót, és mekkora hello lemez legyen, és az Azure létrehozza hello virtuális gépek lemezei. Felügyelt lemezzel rendelkező virtuális gépek számos fontos funkciót, beleértve a rendelkezik:

- Az automatikus méretezhetőség támogatása. Azure hello lemezeket hoz létre, és kezeli a hello alapul szolgáló tárolási toosupport be too10, előfizetésenként 000 lemezek.
- A rendelkezésre állási készletek megbízhatóbbak. Azure biztosítja, hogy virtuális gépek lemezei el különítve egymástól rendelkezésre állási készletek belül automatikusan.
- Jobb hozzáférés-vezérlés. Felügyelt lemezek teszi közzé a különféle műveletek által vezérelt [átruházásához hozzáférés-vezérlés (RBAC)](../../active-directory/role-based-access-control-what-is.md).

Felügyelt lemezek árképzési eltér attól az adott nem felügyelt lemezek. Ez az információ, lásd: [árak és számlázás felügyelt lemezek](../windows/managed-disks-overview.md#pricing-and-billing).

Átalakíthatja a meglévő virtuális gépekhez, és nem felügyelt lemezek felügyelt toouse lemezt használó [az virtuális gép átalakítása](/cli/azure/vm#convert). További információkért lásd: [hogyan tooconvert nem felügyelt kódból felügyelt Linux virtuális lemezek, tooAzure kezelt lemezek](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Egy nem felügyelt lemez nem konvertálható felügyelt lemezre, ha nem kezelt lemez hello, vagy bármikor már, használatával titkosított tárfiókokban [Azure Storage szolgáltatás titkosítási (SSE)](../../storage/common/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). a következő lépések részletes hogyan tootooconvert nem felügyelt vannak, illetve, hogy egy titkosított tárfiókban lévő lemezek hello:

- Hello virtuális merevlemez (VHD) másolása és [az tárolási blob másolási start](/cli/azure/storage/blob/copy#start) tooa tárfiókot, amely soha nem engedélyezett az Azure Storage szolgáltatás titkosítási.
- Hozzon létre egy virtuális Gépet, amely felügyelt lemezt használ, és adja meg, hogy a VHD-fájl létrehozása során [az virtuális gép létrehozása](/cli/azure/vm#create), vagy
- Csatlakoztassa hello másolt virtuális merevlemez [az méretű lemez csatolása](/cli/azure/vm/disk#attach) tulajdonsággal rendelkező virtuális gépet futtató tooa által kezelt lemezeken.


## <a name="azure-storage-standard-and-premium"></a>Az Azure Storage: Standard és Premium
Azure virtuális gépek – hogy lemezekkel felügyelt vagy nem felügyelt--építhetők tárolólemezek standard vagy prémium szintű storage lemezek esetén. A virtuális gép hello portál toochoose használatakor akkor kell váltani egy legördülő lista a hello **alapjai** képernyő tooview standard és a prémium szintű lemezek. Ha váltható tooSSD, csak az engedélyezett virtuális gépek megjelenik, prémium szintű storage összes üzemelnek SSD meghajtók.  Ha tooHDD váltható standard tárolási alkalmas virtuális gépek lemezmeghajtók forgó által támogatott láthatók, prémium szintű storage SSD által támogatott virtuális gépek együtt.

Virtuális gép létrehozásakor a hello `azure-cli` választhat standard és premium hello Virtuálisgép-méretet keresztül hello kiválasztásakor `-z` vagy `--vm-size` cli jelzőt.

## <a name="creating-a-vm-with-a-managed-disk"></a>A felügyelt lemezes virtuális gép létrehozása

hello alábbi példa szükséges hello Azure CLI 2.0 rendszerbe, amely akkor [telepítése Itt](/cli/azure/install-azure-cli).

Először hozzon létre egy erőforrás csoport toomanage hello rendelkező erőforrások [az csoport létrehozása](/cli/azure/group#create):

```azurecli
az group create --location westus --name myResourceGroup
```

Most létrehozza a virtuális gép hello [az virtuális gép létrehozása](/cli/azure/vm#create). Adjon meg egy egyedi `--public-ip-address-dns-name` argumentum, mint a `mypublicdns` valószínűleg használatban van.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns
```

hello előző példa egy virtuális Gépet egy standard szintű tárfiókot felügyelt lemezt hoz létre. a prémium szintű tárfiók toouse hozzáadása hello `--storage-sku Premium_LRS` argumentum, például a következő példa hello:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns \
    --storage-sku Premium_LRS
```

## <a name="standard-storage"></a>Standard szintű Storage
Az Azure szabványos Storage egy hello alapértelmezett tárolási típusát.  Standard szintű tárolót ugyanakkor továbbra is nem performant költséghatékony.  

## <a name="premium-storage"></a>Prémium szintű Storage
Prémium szintű Storage nagy teljesítményű, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek támogatása nyújt. Prémium szintű Storage használó virtuális gép (VM) lemezek (SSD) adatait tárolják. Az alkalmazás virtuális lemezek tooAzure prémium szintű Storage tootake kihasználják az hello sebesség és a lemezek teljesítményét áttelepítheti.

Prémium szintű storage szolgáltatások:

* Prémium szintű Storage lemezek: a prémium szintű Storage, amely csatolt tooDS, DSv2 és GS adatsorozat Azure virtuális gépeken futó virtuális gépek lemezei támogatja.
* Prémium szintű oldalakra vonatkozó Blob: Prémium szintű Storage támogatja az Azure Lapblobokat, amelyek használt toohold állandó lemezek Azure virtuális gépek (VM).
* Prémium szintű helyileg redundáns tárolás: A prémium szintű Storage-fiókok csak helyileg redundáns tárolás (LRS) hello replikációs beállításként támogatja és tartja hello adatok három másolatot egyetlen régión belül.
* [Prémium szintű Storage](../../storage/common/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Prémium szintű Storage támogatott virtuális gépek
Prémium szintű Storage támogatja a DS-méretek, DSv2-méretek, GS sorozatnak és Fs-sorozat Azure virtuális gépek (VM). A virtuális gépek támogatott prémium szintű Storage Standard és prémium szintű storage lemezek is használhatók. De Virtuálisgép-sorozat, amely nem kompatibilis a prémium szintű Storage nem használhatja a prémium szintű Storage lemezek.

Az alábbiakban hello Linux Terjesztéseket, amelyek azt érvényesítve prémium szintű Storage.

| Disztribúció | Verzió | Támogatott Kernel |
| --- | --- | --- |
| Ubuntu |12.04 |3.2.0-75.110+ |
| Ubuntu |14.04 |3.13.0-44.73+ |
| Debian |7.x, 8.x |3.16.7-ckt4-1+ |
| SLES |SLES 12 |3.12.36-38.1+ |
| SLES |SLES 11 SP4 |3.0.101-0.63.1+ |
| CoreOS |584.0.0+ |3.18.4+ |
| Centos |6.5, 6.6, 6.7, 7.0, 7.1 |3.10.0-229.1.2.el7+ |
| RHEL |6.8+, 7.2+ | |

## <a name="azure-file-storage"></a>Az Azure File storage
Az Azure File storage hello szabványos SMB protokollt használó hello felhőben fájlmegosztásokat kínál. Az Azure Files a fájl kiszolgálók tooAzure használó vállalati alkalmazásokat telepíthet át. Az Azure-ban futó alkalmazások könnyen csatlakoztathatnak fájlmegosztások a Linux operációs rendszert futtató Azure virtuális gépeken. És a File storage kiadása legújabb hello, is lehet csatlakoztatni egy fájlmegosztást, amely támogatja az SMB 3.0 helyszíni alkalmazásból.  Mivel a fájlmegosztások SMB-megosztások, szabványos fájlrendszere API-k használatával is elérhet.

A File storage Blob, Table és Queue tárolására, így a File storage biztosít hello rendelkezésre állási, a tartósságra, a méretezhetőség és a-georedundancia, amely ugyanazt a technológiát beépített hello az Azure storage platform hello épül. Fájl tárolási teljesítmény célozza meg, és korlátokat kapcsolatos részletekért lásd: Azure Storage méretezhetőségi és teljesítménycéloknak.

* [Hogyan toouse Azure File storage Linux](../../storage/files/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Gyakran használt adatok
hello Azure gyakran használt adatok tárolási rétege gyakran használt adatok tárolására van optimalizálva.  Gyakran használt adatok típus hello alapértelmezett tárolási blob tárolókhoz.

## <a name="cool-storage"></a>Ritkán használt adatok
hello Azure ritkán használt adatok tárolási rétege, amely ritkán használt, hosszú élettartamú adatok tárolására van optimalizálva. Példák az alkalmazási helyzetekre a ritkán használt adatok közé tartoznak a biztonsági mentések, médiatartalmak, tudományos adatok, megfelelőségi és archiválási adatok. Ritkán használt adatokat általában tökéletes jelöltségét ellenőrző ritkán használt adatok.

|  | Gyakran használt adatok tárolási rétege | Ritkán használt adatok tárolási rétege |
|:--- |:---:|:---:|
| Rendelkezésre állás |99.9% |99% |
| Rendelkezésre állás (RA-GRS olvasások) |99.99% |99.9% |
| Használati díjak |Magasabb tárolási költségek |Alacsonyabb tárolási költségek |
| Alacsonyabb hozzáférési |Magasabb hozzáférési | |
| és tranzakciós költségek |és tranzakciós költségek | |

## <a name="redundancy"></a>Redundancia
hello adatokat a Microsoft Azure storage-fiók mindig van replikálva tooensure tartósság és magas rendelkezésre állású, teljesíti-hello Azure Storage szolgáltatásiszint-szerződés még átmeneti hardverhibák esetén hello felületét a.

Amikor létrehoz egy tárfiókot, válasszon hello a következő replikációs lehetőségek egyikét:

* Helyileg redundáns tárolás (LRS)
* Zónaredundáns tárolás (ZRS)
* Georedundáns tárolás (GRS)
* Írásvédett georedundáns tárolás (RA-GRS)

### <a name="locally-redundant-storage"></a>Helyileg redundáns tárolás
Helyileg redundáns tárolás (LRS) replikálja az adatokat a storage-fiók létrehozására használt hello régión belül. toomaximize tartósság érdekében felé a tárfiókban lévő adatokat irányuló összes kérelmet a rendszer háromszor replikálja. Ezek a három replikák külön tartalék tartományok és a frissítési tartományok találhatók.  Kérelem sikeresen függvény csak akkor, ha le van írva tooall három replikák.

### <a name="zone-redundant-storage"></a>Zónaredundáns tárolás
Zónaredundáns tárolás (ZRS) keresztül két toothree létesítményekben, egy vagy két régióban LRS-nél nagyobb tartósságot biztosít az adatok replikálásra. A tárfiók a ZRS engedélyezve van, majd az adatok akkor tartós, még akkor is, a hiba egyik hello létesítményekben hello eset.

### <a name="geo-redundant-storage"></a>Georedundáns tárolás
Georedundáns tárolás (GRS) replikálja a tooa másodlagos adatterületen, amely több száz miles hello elsődleges régió befejeződött. Ha a tárfiók Georedundáns engedélyezve van, az adatok tartós, még akkor is hello esetekben egy teljes regionális kimaradás vagy egy olyan vészhelyzet esetén milyen hello elsődleges régió nincs helyreállítható.

### <a name="read-access-geo-redundant-storage"></a>Írásvédett georedundáns tárolás
Írásvédett georedundáns tárolás (RA-GRS) rendelkezésre állási a tárfiók, adja meg a csak olvasási hozzáféréssel toohello adatok hello másodlagos helyen, továbbá toohello replikációs két régióban GRS biztosítja a lehető legnagyobbra növeli. Hello esemény, hogy adatokat hello elsődleges régióban nem érhető el az alkalmazás adatokat képes olvasni hello másodlagos régióba.

A részletes bemutatója a redundancia lásd az Azure storage:

* [Azure Storage replication (Azure Storage replikáció)](../../storage/common/storage-redundancy.md)

## <a name="scalability"></a>Méretezhetőség
Az Azure Storage egy nagymértékben méretezhető, így tárolja, és több száz terabájtos adatok toosupport hello big data forgatókönyvéhez szükséges tudományos és üzleti elemzések, vagy a médiaalkalmazások feldolgozni. Vagy tárolhatja az adatokat a kisvállalkozások webhelyeihez szükséges kis mennyiségű hello. Csökkennek az igényei, csak fizetnie hello adatokat tárolja. Az Azure Storage jelenleg több tízbillió egyéni ügyfélobjektumot tárol, és másodpercenként átlagosan több millió lekérést szolgál ki.

Standard szintű storage-fiókok: egy standard szintű tárfiók van egy 20 000 IOPS maximális teljes kérelmek aránya. hello összes egy standard szintű tárfiók a virtuális gép lemezeinek teljes iops-érték nem haladhatja meg ezt a határt.

Prémium szintű storage-fiókok: A prémium szintű tárfiók van egy 50 GB/s maximális teljes átviteli sebesség mértéke. hello összes, a virtuális lemezek teljes átviteli sebesség nem haladhatja meg ezt a határt.

## <a name="availability"></a>Rendelkezésre állás
Garantáljuk, hogy legalább 99,99 % (99,9 %-a ritka hozzáférési szint) hello idő, azt fogja sikeresen kérelmek tooread adatfeldolgozásra olvasási hozzáférés-földrajzi redundáns tárolás (RA-GRS) fiókokhoz, feltéve, hogy a sikertelen kísérletek tooread adat hello elsődleges régióban újra megpróbálja a hello másodlagos régióba.

Garantáljuk, hogy legalább 99,9 %-os (99 %-os ritka hozzáférési szint) hello idő, fedi le sikeresen folyamat tooread adatokat kér a helyileg redundáns tárolás (LRS), a zóna redundáns tárolás (ZRS) és a fiókot Geo redundáns tárolás (GRS).

Garantáljuk, hogy legalább 99,9 %-os (99 %-os ritka hozzáférési szint) hello idő, fedi le sikeresen folyamat kér toowrite adatok tooLocally redundáns tárolás (LRS), zóna redundáns tárolás (ZRS), és földrajzi redundáns tárolás (GRS) fiókok és olvasási hozzáférés-földrajzi redundancia (RA-GRS) Storage-fiókok.

* [Tárolás az Azure SLA](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)

## <a name="regions"></a>Régiók
Azure általánosan elérhető hello világ 30 régiókban, és bejelentette 4 további régiókhoz tervek. Földrajzi bővítése nem Azure prioritást, mert lehetővé teszi, hogy az ügyfelek tooachieve nagyobb teljesítményt és az támogatja a követelményeket és az adatok helye vonatkozó beállítások.  Azures legújabb régió toolaunch vonatkozik.

a Microsoft Cloud Németország hello lehetővé teszi egy eltérő beállítás toohello már elérhető a Microsoft Cloud services Európa, nagyobb teret hagynak a innováció és magas szabályozott partnerek gazdasági növekedés és ügyfelek létrehozása a Németországi, hello Európai Unió és hello Európai kereskedelem társítás (EFTA).

Ezek új adatközpontokban Magdeburgban és Frankfurt, ügyféladatok egy adatok adatkezelő T-rendszerek nemzetközi, egy független német vállalati és német Telekom leányvállalata hello vezérlése alatt áll. A Microsoft kereskedelmi felhőszolgáltatások ezek adatközpontokban igazodik tooGerman adatok kezelése szabályozásainak megfelelően, és hogyan és hol adatok feldolgozása a további lehetőségeket biztosít.

* [Azure-régiók térkép](https://azure.microsoft.com/regions/)

## <a name="security"></a>Biztonság
Az Azure Storage a biztonságos alkalmazások toobuild biztonsági képességeket, amelyek együtt lehetővé teszik a fejlesztők széles választékát nyújtja. hello tárfiók magát a szerepköralapú hozzáférés-vezérlés és az Azure Active Directory használatával kell biztonságossá. A védett adatok az alkalmazás és az Azure közötti átvitel során ügyféloldali titkosítás, HTTPS és SMB 3.0-s. Adatok titkosítása automatikusan megtörténik toobe állítható be Ha a tárolókészletet a Storage szolgáltatás titkosítási (SSE) tooAzure ír. Virtuális gépek által használt operációsrendszer- és adatlemezek titkosítja az Azure Disk Encryption toobe állítható be. Delegált hozzáférést toohello adatobjektumainak az Azure Storage a megosztott hozzáférési aláírásokkal használatával engedélyezhetők.

### <a name="management-plane-security"></a>Felügyeleti Vezérlősík biztonsága
hello felügyeleti vezérlősík hello használt erőforrások toomanage áll a tárfiók. Ebben a szakaszban lesz döntésről bővebben hello Azure Resource Manager üzembe helyezési modellben, és hogyan férhetnek hozzá a toouse szerepköralapú hozzáférés-vezérlést (RBAC) toocontrol tooyour storage-fiókok. A tárfiók kulcsait kezelése is előadás és hogyan tooregenerate őket.

### <a name="data-plane-security"></a>Adatbiztonság Vezérlősík
Ebben a szakaszban megnézzük, hozzáférést toohello tényleges adatobjektumainak tárfiókba blobok, fájlok, üzenetsorokat és táblákat, például megosztott hozzáférési aláírásokkal és hozzáférési házirendek tárolja. Bemutatjuk a szolgáltatásiszint-SAS és a fiók szintű SAS. Is megtanulhatja, hogyan toolimit férnek hozzá az tooa adott IP-cím (vagy az IP-címek), hogyan toolimit hello protokoll tooHTTPS, és hogyan nem várja meg azt a közös hozzáférésű Jogosultságkód toorevoke tooexpire.

## <a name="encryption-in-transit"></a>Az átvitel során titkosítás
Ez a szakasz ismerteti hogyan toosecure adatok átvitel során, vagy abból az Azure Storage. Lesz döntésről bővebben hello HTTPS és hello használja az Azure fájlmegosztások SMB 3.0-titkosítás ajánlott. Most elindítjuk egy pillantást ügyféloldali titkosítás, mely lehetővé teszi tooencrypt hello előtt a tároló ügyfél-alkalmazásokban, és toodecrypt hello adatok tárolási kivitt után is.

## <a name="encryption-at-rest"></a>Aktívan titkosítása
Az előadás Storage Service Encryption (SSE), és hogyan engedélyezi azt egy tárfiókot, ami azt eredményezi, a blokkblobokat, lapblobokat, és hozzáfűző blobok, automatikus a titkosítás, ha a tárolási tooAzure ír. Hogyan használja az Azure Disk Encryption és felfedezés hello alapvető különbséget és az adatok titkosítása és SSE ügyféloldali titkosítás és esetek is fog keresni. Röviden: a FIPS előírásainak való megfelelést az USA fog keresni Kormánya számítógépek.

* [Az Azure Storage biztonsági útmutató](../../storage/common/storage-security-guide.md)

## <a name="temporary-disk"></a>Ideiglenes lemez
Minden virtuális gép ideiglenes lemezt tartalmaz. hello ideiglenes lemez rövid távú tárolás biztosít az alkalmazások és folyamatok, tervezett tooonly adat tárolása például lap vagy swap fájlokat. Hello ideiglenes lemezen lévő adatok elveszhetnek során egy [karbantartási esemény](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) , vagy ha Ön [újratelepíteni a virtuális gépek](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Egy szabványos hello VM újraindításkor hello ideiglenes meghajtón hello adatokat kell megőrizni.

Linux virtuális gépeken hello lemez általában **/dev/sdb** formázott és csatlakoztatott túl**/mnt** által hello Azure Linux ügynök. hello hello ideiglenes lemez mérete a függvénye hello hello virtuális gép méretét. További információkért lásd: [Linux virtuális gépek méretei](sizes.md).

Hogyan használja az Azure a hello mennyiségű ideiglenes lemezes a további információkért lásd: [hello ideiglenes meghajtó Microsoft Azure virtuális gépeken ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="cost-savings"></a>Költségmegtakarítások
* [Tárolási költségek](https://azure.microsoft.com/pricing/details/storage/)
* [A tárolási költségeket Számológép](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Tárolási korlátai
* [Storage szolgáltatásra vonatkozó korlátozások](../../azure-subscription-service-limits.md#storage-limits)
