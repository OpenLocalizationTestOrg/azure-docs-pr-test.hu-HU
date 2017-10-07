---
title: "aaaAzure prémium és standard szintű felügyelt lemez áttekintése |} Microsoft Docs"
description: "Azure felügyelt lemezek, amelyek az Ön hello tárfiókok kezeli az Azure virtuális gépek használata esetén – áttekintés"
services: storage
documentationcenter: na
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 70d45226e531b43f2142f2798bdaf40f77f057f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-disks-overview"></a>Az Azure felügyelt lemezekhez – áttekintés

Azure-lemezeket felügyelt egyszerűbbé teszi a Lemezkezelés Azure IaaS virtuális gépek kezelésével hello [tárfiókok](storage-introduction.md) hello virtuális gépek lemezei társított. Csak akkor kell toospecify hello típusa ([prémium](storage-premium-storage.md) vagy [szabványos](storage-standard-storage.md)) és hello mérete lemez van szüksége, és Azure hoz létre, és kezeli a hello lemez meg.

## <a name="benefits-of-managed-disks"></a>Felügyelt lemezek előnyei

Vessen egy pillantást, néhány hello előnyt ettől kezdve által kezelt lemezek segítségével kezdődő, és ezt a Channel 9 videót [jobb Azure VM rugalmassági felügyelt lemezzel](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency).
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>Egyszerű és skálázható módon telepíthetők a Virtuálisgép-telepítéshez

Kezelve lemezek leírók tárolási hello színfalak mögött. Toocreate fiókok toohold hello tárolólemezek (VHD-fájlokat) korábban, az Azure virtuális gépeken volt. Ha vertikális felskálázásával, kellett toomake, így nem haladhatja meg a tárolási IOPS-korláttal hello egyetlen, a lemezek létrehozott további tárfiókokat. A felügyelt lemezek kezelése tárolási, már nem korlátozott hello tárfiókok korlátai által (például a 20 000 IOPS / fiók). Már nem rendelkezik toocopy az egyéni lemezképek (VHD-fájlokat) toomultiple storage-fiókok. – Egy tárfiókot Azure régiónként – központi helyen kezelheti azokat, és használatuk toocreate több száz virtuális gépek egy előfizetésben.

Felügyelt lemezek lehetővé teszi a too10, 000 virtuális gép mentése toocreate **lemezek** egy előfizetésben található, amely lehetővé teszi a több ezer toocreate **virtuális gépek** az egy-egy előfizetéshez. Ez a szolgáltatás tovább növeli a hello méretezhetőségét [virtuális gép méretezési készletek (VMSS)](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) : lehetővé teszi a Piactéri rendszerkép használatával egy VMSS toocreate tooa ezer virtuális gépeinek.

### <a name="better-reliability-for-availability-sets"></a>A rendelkezésre állási csoportok jobb megbízhatóságot

Győződjön meg arról, hogy hello által kezelt lemezek biztosít az a rendelkezésre állási csoportok nagyobb megbízhatóságot lemezek [virtuális gépek rendelkezésre állási csoport](../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) elég elkülönül egymástól tooavoid egyetlen ponton felmerülő hibákat. Ennek érdekében automatikusan helyezi el hello lemezek különböző tárolási méretezési egységeit (bélyegek). A stamp toohardware-vagy szoftverhiba miatt meghiúsul, ha csak hello Virtuálisgép-példányok az adott bélyegzők lemezzel sikertelen lesz. Ha tegyük fel például, öt virtuális gépeken futó egy alkalmazást, és hello virtuális gépek rendelkezésre állási csoport. hello lemezeket a virtuális gépek nem tárolja a hello azonos stamp, így egyetlen stampet konfiguráljon leáll, ha hello más hello alkalmazás példányát tovább toorun.

### <a name="highly-durable-and-available"></a>Tartós és magas rendelkezésre állású

Az Azure Disks 99,999%-os elérhetőséggel büszkélkedhet. REST-könnyebb ismerete, hogy rendelkezik-e az adatok, amely lehetővé teszi a magas tartósság három replikákat. Egy vagy akár két replikák problémákba ütközhetnek, ha hello fennmaradó replikák biztosíthatja az adatok és a hibákkal szemben magas tolerancia megőrzése. Ezzel a kialakítással az Azure rendszeresen vállalati szintű tartósságot nyújthat az IaaS-lemezeknek, iparágvezető NULLA %-os Éves Hibaszázalékkal. 

### <a name="granular-access-control"></a>A részletes hozzáférés-vezérlés

Használhat [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-control-what-is.md) tooassign meghatározott engedélyek egy felügyelt lemezes tooone vagy több felhasználó. Felügyelt lemezek tesz elérhetővé a különféle műveletek, beleértve az olvasási, írási (létrehozása/frissítése), törölje és lekérése egy [közös hozzáférésű jogosultságkód (SAS) URI](storage-dotnet-shared-access-signature-part-1.md) hello lemezhez. Meg lehet adni tooonly hello műveletek egy személy kell tooperform a feladata. Például ha egy személy toocopy felügyelt lemezes tooa tárfiók nem szeretné, választhat nem toogrant hozzáférés toohello exportálási művelet a felügyelt lemezt. Hasonlóképpen, ha nem szeretné, hogy egy személy toouse egy SAS URI toocopy felügyelt lemezes, választhat nem toogrant adott engedély toohello kezelt lemezre.

### <a name="azure-backup-service-support"></a>Az Azure Backup szolgáltatás támogatása
Azure biztonsági mentési szolgáltatás használata felügyelt lemezek toocreate az idő-alapú biztonsági mentések, könnyű VM-helyreállítás és biztonsági másolatok megőrzésének házirendek segítségével a biztonsági mentési feladatot. Felügyelt lemezek csak támogatják az helyileg redundáns tárolás (LRS) hello replikációs beállítás; Ez azt jelenti, hogy egyetlen régión belül hello adatok három másolatot tart. Regionális vész-helyreállítási kell biztonsági mentést készíteni a virtuális gépek lemezei be egy másik régióban [Azure Backup szolgáltatás](../backup/backup-introduction-to-azure-backup.md) és a biztonsági mentési tárolóval Georedundáns tárfiókot. Azure Backup szolgáltatás jelenleg adatok mérete too1TB fel a biztonsági mentéshez. További információk a következő [használata Azure Backup szolgáltatás felügyelt lemezzel rendelkező virtuális gépek](../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="pricing-and-billing"></a>Árak és számlázás

Felügyelt lemezek használata esetén alkalmazza a következő számlázási szempontok hello:
* Tárolási típus

* Lemezméret

* Tranzakciók száma

* Kimenő adatforgalom

* Felügyelt lemez – pillanatfelvételek (teljes lemezmásolás)

Vegyük a következő részletes bemutatása.

**Tárolási típus:** felügyelt lemezek 2 teljesítmény rétegek kínál: [prémium](storage-premium-storage.md) (SSD-alapú) és [szabványos](storage-standard-storage.md) (HDD-alapú). egy kezelt lemez hello számlázási attól függ, hello lemez kiválasztott tárolási típusát.


**Lemezméret**: felügyelt lemezek számlázási hello lemez mérete kiépített hello függ. Azure maps hello (kerekítve) kiosztott mérete toohello legközelebbi hello az alábbi táblázatok a felügyelt lemezek lehetőséget. Minden felügyelt lemezes maps tooone a hello támogatott kiosztott méretek, és ennek megfelelően van-e terhelve. Például hozzon létre egy standard szintű felügyelt lemezes és 200 GB-os kiosztott méretet adjon meg, ha díját is felszámítjuk hello árképzése hello S20 lemeztípus szerint.

Íme egy prémium szintű felügyelt lemezes hello mérete:

| **Prémium szintű kezelt <br>lemez típusa** | **P4** | **P6** |**P10** | **P20** | **P30** | **P40** | **P50** | 
|------------------|---------|---------|---------|---------|----------------|----------------|----------------|  
| Lemezméret        | 32 GB   | 64 GB   | 128 GB  | 512 GB  | 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 


Íme egy standard szintű felügyelt lemezes hello mérete:

| **Standard felügyelt <br>lemez típusa** | **S4** | **S6** | **S10** | **S20** | **S30** | **S40** | **S50** |
|------------------|---------|---------|--------|--------|----------------|----------------|----------------| 
| Lemezméret        | 32 GB   | 64 GB   | 128 GB | 512 GB | 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 


**A tranzakciók számának**: hello hajt végre egy standard szintű felügyelt lemezes tranzakciók száma a kell fizetni. Nincs a prémium szintű felügyelt lemezes tranzakciók költség nélkül.

**Kimenő adatátvitel**: [kimenő adatátviteli](https://azure.microsoft.com/pricing/details/data-transfers/) (az adatok Azure-adatközpont kilépő) gigabájtalapú sávszélesség-használat.

Felügyelt lemezek árakkal kapcsolatos részletes információkért lásd: [lemezek árképzési felügyelt](https://azure.microsoft.com/pricing/details/managed-disks).


## <a name="managed-disk-snapshots"></a>Felügyelt lemezes pillanatképek

Felügyelt pillanatkép egy csak olvasható, teljes másolata egy felügyelt lemezes, amely alapértelmezés szerint egy standard szintű felügyelt lemezes tárolja. A pillanatképekkel akkor készíthet biztonsági másolatot a felügyelt lemezek bármikor időben. Ezeket a pillanatképeket hello forráslemez független is és lehet használt toocreate új kezelt lemezek. Azok a használt hello mérete alapján számlázzuk. Például ha 64 GB-os kiosztott kapacitását és tényleges használt adatok mérete 10 GB-os felügyelt lemezes pillanatképet hoz létre, pillanatkép alapján számlázzuk csak a használt hello adatok mérete 10 GB-os.  

[Növekményes pillanatképek](storage-incremental-snapshots.md) lemezek felügyelete jelenleg nem támogatottak, de a jövőbeli hello támogatott.

toolearn arról hogyan felügyelt lemezek pillanatképei toocreate vegye ki ezeket az erőforrásokat:

* [Felügyelt lemezként tárolt VHD másolatának létrehozása pillanatképekkel Windows alatt](../virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Felügyelt lemezként tárolt VHD másolatának létrehozása pillanatképekkel Linux alatt](../virtual-machines/linux/snapshot-copy-managed-disk.md)


## <a name="images"></a>Képek

Felügyelt lemezeket is támogatja a felügyelt egyéni lemezkép létrehozása. Kép hozhat létre, az egyéni virtuális merevlemez olyan tárfiókban illetve közvetlenül egy általánosított (sys prepped) virtuális Gépet. Ez az összes felügyelt egy virtuális Gépet, beleértve az operációs rendszer hello és adatlemezek hozzárendelt egyetlen rendszerképbe írja le. Ez lehetővé teszi, hogy az egyéni lemezképet nélkül hello használata virtuális gépek több száz létrehozása toocopy kell, vagy a storage-fiókok kezelése.

Információ az egyéni lemezképek adjon tekintse meg a következő cikkek hello:
* [Hogyan toocapture egy általánosított virtuális Gépet az Azure-ban kezelt képe](../virtual-machines/windows/capture-image-resource.md)
* [Hogyan toogeneralize és rögzítése egy Linux virtuális gép használt hello Azure CLI 2.0](../virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>A pillanatképek és lemezképek

Hello word "lemezképpel" használt virtuális gépek gyakran megjelenik, és ekkor megjelenik a "pillanatképek" is. Fontos toounderstand hello különbség a között. A felügyelt lemezek esetében is igénybe vehet egy általánosított virtuális Gépet, amely fel lettek szabadítva képe. Ez a lemezképben hello csatlakoztatott lemezek toohello VM mindegyikét. A kép toocreate egy új virtuális Gépet is használhat, és ez magában foglalja a hello lemezeket.

Pillanatkép van szükséges idő hello ponton a lemez egy példányát. Csak érvényes tooone lemez. Ha egy lemez (hello az operációs rendszer) rendelkező virtuális gép, pillanatképet, vagy egy képet, és hozzon létre egy virtuális Gépet hello pillanatkép vagy hello kép.

Mi történik, ha a virtuális gépek öt lemezzel rendelkezik, és azok csíkozott? Az egyes lemezek hello pillanatképet készíthet, de nincs észlelési hatókörén belül hello hello állapot hello lemezt – hello pillanatképek a virtuális gép csak tudni, hogy egy lemez van. Ebben az esetben hello pillanatképek egymással koordinált toobe lenne szükség, és, amely jelenleg nem támogatott.

## <a name="managed-disks-and-encryption"></a>Felügyelt lemezek és titkosítás

Nincsenek kétféle titkosítási toodiscuss hivatkozás toomanaged lemezeken. hello először egyik tárolási szolgáltatás titkosítási (SSE), amely hello tároló szolgáltatás hajtja végre. hello második az operációs rendszer hello és adatlemezek engedélyezheti a virtuális gépek Azure Disk Encryption.

### <a name="storage-service-encryption-sse"></a>Storage szolgáltatás titkosítási (SSE)

[Az Azure Storage szolgáltatás titkosítási](storage-service-encryption.md) nyugalmi titkosítási biztosít, és megakadályozhatja az adatok toomeet a szervezeti biztonsági és megfelelőségi jár kötelezettségekkel. SSE alapértelmezés szerint az összes felügyelt lemezek, a pillanatképek és a képeket minden hello régióban, amennyiben rendelkezésre áll-e felügyelt lemezek engedélyezve van. 2017. június 10., kezdve az összes új felügyelt lemezek/pillanatképek/képek és felügyelt tooexisting lemezek új adatokat automatikusan titkosítva nyugalmi Microsoft által felügyelt kulcsokkal.  A Microsoft hello [kezelt lemezek gyakori kérdéseket tartalmazó oldal](storage-faq-for-disks.md#managed-disks-and-storage-service-encryption) további részleteket.


### <a name="azure-disk-encryption-ade"></a>Az Azure Disk Encryption (ADE)

Az Azure Disk Encryption lehetővé teszi tooencrypt hello az operációs rendszer és az infrastruktúra-szolgáltatási virtuális gép által használt adatok lemezeket. Ez magában foglalja a felügyelt lemezek. A Windows hello meghajtók titkosítása szabványos BitLocker titkosítás technológia használatával. A Linux hello lemezek titkosítása hello DM-Crypt technológia használatával. Ez az integrálva van az Azure Key Vault tooallow meg toocontrol és hello lemez titkosítási kulcsok kezeléséhez. További információkért lásd: [lemez titkosítás a Windows Azure és a Linux IaaS virtuális gépeket](../security/azure-security-disk-encryption.md).

## <a name="next-steps"></a>Következő lépések

A felügyelt lemezek kapcsolatos további információkért tekintse meg a következő cikkek toohello.

### <a name="get-started-with-managed-disks"></a>Ismerkedés a Managed Disks szolgáltatással

* [Virtuális gép létrehozása a Resource Manager és a PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md)

* [Linux virtuális gép létrehozása Azure CLI 2.0 hello](../virtual-machines/linux/quick-create-cli.md)

* [Egy felügyelt adatok lemez tooa Windows virtuális gép csatlakoztatása PowerShell használatával](../virtual-machines/windows/attach-disk-ps.md)

* [Egy felügyelt lemezes tooa Linux virtuális gép hozzáadása](../virtual-machines/linux/add-disk.md)

* [Felügyelt lemezek PowerShell-mintaparancsfájlok](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [Felügyelt lemezek használhatók Azure Resource Manager-sablonok](storage-using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>Felügyelt lemezek tárolási lehetőségek összehasonlítása

* [Prémium szintű storage és a lemezek](storage-premium-storage.md)

* [Standard szintű tárolást és a lemezek](storage-standard-storage.md)

### <a name="operational-guidance"></a>Műveleti útmutató

* [Telepítse át AWS és más platformokra tooManaged lemezt az Azure-ban](../virtual-machines/windows/on-prem-to-azure.md)

* [Az Azure-ban Azure virtuális gépek toomanaged lemezek konvertálása](../virtual-machines/windows/migrate-to-managed-disks.md)
