---
title: "Azure IaaS virtuális gép lemezeivel kapcsolatos gyakori kérdések (GYIK) |} Microsoft Docs"
description: "Azure IaaS virtuális gép és a premium lemezek (felügyelt és nem felügyelt) kapcsolatos gyakori kérdések"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 31d0aa67b6ca58b75b432ae94f93ebcf6d730380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Azure IaaS virtuális gép és a kezelt és kezeletlen premium lemezek kapcsolatos gyakori kérdések

Ebben a cikkben megválaszolunk néhány Azure felügyelt lemezek és a prémium szintű Azure Storage kapcsolatos gyakori kérdésekre.

## <a name="managed-disks"></a>Felügyelt lemezek

**Mi az Azure Managed lemezek?**

Felügyelt lemezek olyan szolgáltatás, amely egyszerűbbé teszi a Lemezkezelés Azure IaaS virtuális gépek kezelése a tárolási fiók kezelése az Ön által. További információkért lásd: hello [felügyelt lemezekhez – áttekintés](storage-managed-disks-overview.md).

**Ha egy standard szintű felügyelt lemezes I készíteni egy meglévő VHD-t, amely 80 GB, mennyi lesz, amely költségeket, me?**

80 GB-os virtuális merevlemezről létrehozott egy standard szintű felügyelt lemezes hello tovább érhető el standard méretű lemez méretet, amelyet egy S10 lemez, a rendszer. Függően toohello S10 lemez árképzési most számítjuk fel. További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/storage).

**Vannak-e a standard szintű felügyelt lemez tranzakciós költségeket?**

Igen. Minden tranzakció még fizetnie. További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/storage).

**Az egy standard szintű felügyelt lemezes I megterheljük hello hello lemezen tárolt adatok tényleges mérete hello vagy hello lemez kapacitását kiépített hello?**

Most felszámított hello lemez kapacitását kiépített hello alapján. További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/storage).

**Hogyan azonos a nem felügyelt lemezek felügyelt premium lemezek árképzési?**

hello árképzési felügyelt premium lemezek a ugyanaz, mint a nem felügyelt premium lemezek hello.

**Módosíthatja a hello tárfiók típusa (Standard vagy prémium) a kezelt lemezek?**

Igen. Hello tárfióktípus a felügyelt lemezek hello Azure-portálon, a PowerShell vagy a hello Azure parancssori felület használatával módosíthatja.

**Van mód, hogy lehet másolni, vagy egy felügyelt lemezes tooa titkos tárfiók exportálása?**

Igen. A felügyelt lemezek hello Azure-portálon, a PowerShell vagy a hello Azure CLI segítségével exportálhatja.

**Használható az Azure storage-fiók toocreate felügyelt lemezes a VHD-fájl egy másik előfizetést?**

Nem.

**Használható az Azure storage-fiók toocreate felügyelt lemezes a VHD-fájl egy másik régióban?**

Nem.

**Vannak-e a felügyelt lemezeket használó ügyfelek skálázási korlátozások?**

Felügyelt lemezek kiküszöböli hello korlátok társított tárfiókokat. Előfizetésenként felügyelt lemezek hello száma azonban korlátozott too2, alapértelmezés szerint 000. Támogatási tooincrease hívhatják ezt a számot.

**Eltarthat egy kezelt lemez növekményes pillanatképet?**

Nem. hello aktuális pillanatkép-készítés révén felügyelt lemezes teljes másolata. Azonban azt tervezi, toosupport növekményes pillanatképek a jövőbeli hello.

**Virtuális gépek rendelkezésre állási csoport által felügyelt és nem kezelt lemezek kombinációja állhat?**

Nem. hello virtuális gépek rendelkezésre állási csoportba kell használnia, vagy az összes felügyelt lemeznek, vagy a nem felügyelt összes lemez. Amikor létrehoz egy rendelkezésre állási csoportot, válassza a lemezek milyen típusú toouse szeretné.

**Hello Azure-portálon kezelt lemezek hello alapértelmezett beállítás van?**

Jelenleg nem de válik a jövőbeli hello hello alapértelmezett.

**Hozhat létre egy üres felügyelt lemezes?**

Igen. Üres lemez hozhat létre. Felügyelt lemezes hozhatók létre függetlenül a virtuális gépek például tooa virtuális gép csatlakoztatás nélkül.

**Mi az a hello támogatott tartalék tartományainak száma egy rendelkezésre állási csoport által felügyelt lemezeket használó?**

Attól függően, hogy hello régió, ahol felügyelt lemezeket használó hello rendelkezésre állási csoport a támogatott hello tartalék tartományainak száma 2 vagy 3.

**Standard szintű tárfiók hello beállítása diagnosztika hogyan van?**

A Virtuálisgép-diagnosztika titkos tárolási fiók beállítása. A jövőbeli hello, hogy terv tooswitch diagnosztika tooManaged lemezeket is.

**Milyen típusú szerepköralapú hozzáférés-vezérlés támogatásának felügyelt lemezek érhető el?**

Felügyelt lemezek által támogatott három fő alapértelmezett szerepkörök:

* Tulajdonos: Mindent felügyelhetnek, beleértve a hozzáférést
* Társszerző: Hozzáférés kivételével mindent felügyelhetnek
* Olvasó: Mindent megtekinthetnek, de nem módosítható

**Van mód, hogy lehet másolni, vagy egy felügyelt lemezes tooa titkos tárfiók exportálása?**

A csak olvasható közös hozzáférésű jogosultságkód felügyelt hello URI-JÁNAK lemez, és ezzel toocopy hello tartalma tooa titkos tárolási fiók vagy a helyszíni tárolási kérheti le.

**A felügyelt lemezes másolatát hozhat létre?**

Ügyfelek pillanatképet készít a felügyelt lemezeket, és egy másik felügyelt lemezes hello pillanatkép toocreate használhatja.

**Nem felügyelt lemezek továbbra is támogatottak?**

Igen. Nem felügyelt és a felügyelt támogatjuk. Javasoljuk, hogy új munkaterhelések esetén használja a felügyelt lemezeit, és telepítse át a jelenlegi munkaterhelés toomanaged lemezek.


**Ha megkezdtem 128 GB-os lemez létrehozása, és növelje hello mérete too130 GB, I megterheljük a hello következő lemezméretet (512 GB)?**

Igen.

**Létrehozhatok helyileg redundáns tárolás, a georedundáns tárolás és zónaredundáns tárolás által kezelt lemezeken?**

Azure-lemezeket felügyelt jelenleg csak a helyileg redundáns tárolás felügyelt lemezeit támogatja.

**Csökkenhessen vagy a felügyelt lemezek downsize?**

Nem. Ez a funkció jelenleg nem támogatott. 

**Módosítható hello számítógép name tulajdonság esetén egy speciális (nem hello rendszer-előkészítő eszköz használatával létrehozott vagy általánosított) operációs rendszer tárolására használt tooprovision egy virtuális Gépet?**

Nem. Hello számítógép name tulajdonság nem frissíthető. hello új virtuális gép örökli azt hello szülő virtuális gép, amely használt toocreate hello operációsrendszer-lemez volt. 

**Hol találnak a minta Azure Resource Manager sablonok toocreate virtuális gépek felügyelt lemezek?**
* [Felügyelt lemezekkel sablonok listája](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

## <a name="managed-disks-and-storage-service-encryption"></a>Lemezek és a Storage szolgáltatás titkosítási felügyelt 

**Azure Storage szolgáltatás titkosítási alapértelmezés szerint engedélyezve van egy felügyelt lemezes létrehozásakor?**

Igen.

**Kezelő hello titkosítási kulcsokat?**

A Microsoft kezeli a hello titkosítási kulcsokat.

**Képes-e letiltása Storage szolgáltatás titkosítási a felügyelt lemezek?**

Nem.

**Érhető Storage szolgáltatás titkosítási csak meghatározott régióiba?**

Nem. Érhető el minden, amennyiben rendelkezésre áll-e felügyelt lemezek hello régióban. Felügyelt lemezek minden nyilvános régiók és Németországban érhető el.

**Hogyan tudhatom meg titkosítottak, ha a felügyelt lemezes?**

Felügyelt lemezes létrehozásának hello Azure-portálon hello Azure CLI és PowerShell hello időpontja talál. Ha hello idő után 2017. június 9., a lemez titkosítva. 

**Hogyan titkosíthatja a meglévő lemezek 2017. június 10. előtt létrehozott?**

2017. június 10., frissítésétől felügyelt tooexisting lemezek új adatokat automatikusan titkosítva. Azt is tervez tooencrypt meglévő adatok és hello titkosítási hello háttérben aszinkron módon történik. Ha meglévő adatokat most titkosítani kell, hozzon létre egy másolatot a lemezen. Új lemez titkosítva legyen.

* [Másolja át a felügyelt lemezek hello Azure parancssori felület használatával](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)
* [Felügyelt lemezeket másolja a PowerShell használatával](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

**Felügyelt pillanatfelvételek és lemezképek titkosítva van?**

Igen. Az összes kezelt pillanatképeket és lemezképek létrehozása után 2017. június 9., automatikusan titkosítva vannak. 

**Képes virtuális gépek is átalakítása nem felügyelt lemezek tárfiókokban vagy korábban titkosított toomanaged lemez található?**

Igen

**A felügyelt lemezes vagy a pillanatképet egy exportált VHD-t is titkosítva lesznek?**

Nem. De ha egy virtuális merevlemez tooan exportálnia titkosított tárfiók egy titkosított felügyelt lemezes vagy a pillanatkép, majd titkosítva van. 

## <a name="premium-disks-managed-and-unmanaged"></a>Prémium szintű lemezekhez: felügyelt és nem felügyelt

**Egy virtuális gép használ, amely támogatja a prémium szintű Storage, például egy DSv2 mérete több premium és a szabványos adatlemezek csatolható?** 

Igen.

**Prémium szintű és a standard lemezek tooa mérete adatsorozat, amely nem támogatja a prémium szintű Storage, például a D, Dv2, G vagy F adatsorozat csatolható?**

Nem. Csak szabványos adatok lemezek tooVMs, amelyek nem használják, amely támogatja a prémium szintű Storage mérete több csatolható.

**Ha egy meglévő virtuális merevlemezről, de a 80 GB prémium adatlemezt hozok létre, mennyire, amely ára?**

80 GB-os virtuális merevlemezről létrehozott prémium adatlemezt P10 lemez hello tovább érhető el prémium szintű lemez méretének kell kezelni. Függően toohello P10 lemez árképzési most számítjuk fel.

**Vannak-e tranzakciós költségek toouse prémium szintű Storage?**

Nincs minden lemez méretét, amelyet adott határral kiosztott iops-érték és az átvitel meg egy rögzített költséget. hello egyéb költségek olyan kimenő sávszélesség és a pillanatkép-kapacitást, ha van ilyen. További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/storage).

**Az iops-érték és a teljesítményt, amelyek szeretnék hello lemezgyorsítótár lekérheti hello korlátai**

gyorsítótár kombinált korlátairól hello és DS azokat a helyi SSD core 4000 iops-értéket és core másodpercenként 33 MB. hello GS adatsorozat 5000 iops teljesítményt nyújtanak core és 50 MB / s / core kínál.

**A lemezek felügyelt virtuális gépek támogatott helyi SSD hello van?**

hello helyi SSD az ideiglenes tároló, amely megtalálható a felügyelt lemezek virtuális Géphez. Nincs ingyenesen a ideiglenes tárolására. Azt javasoljuk, hogy nem használja a helyi SSD toostore az alkalmazásadatok, mert azt az Azure Blob storage nem megőrzött.

**Nincs hello bármely következményekkel használatban vannak a VÁGÁSHOZ a premium lemezek?**

Nincs Nincs vágás vagy prémium szintű Azure-lemezeket vagy a standard lemezek hátránya toohello használatát.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>Új lemezméret: felügyelt és nem felügyelt

**Mi az az hello támogatott operációs rendszer és az adatlemezek legnagyobb lemez méretét?**

hello partíció, amely Azure támogatja az operációsrendszer-lemez típus hello fő rendszertöltő rekord (MBR). hello MBR formátumhoz too2 TB fel a lemez méretét. hello legnagyobb Azure támogatja az operációsrendszer-lemez mérete 2 TB. Azure legfeljebb too4 TB adatlemezek támogat. 

**Mi az az hello legnagyobb blob oldalméret támogatott?**

hello legnagyobb lap blob, amely támogatja az Azure mérete 8 TB (8,191 GB). Nagyobb, mint 4 TB-os (4095 GB) kapcsolódó tooa VM adatok vagy az operációs rendszer lemezek lapblobokat nem támogatottak.

**Toouse újabb verzióra kell az Azure-eszközök toocreate, csatolja, méretezze át, és töltse fel az 1 TB-nál nagyobb lemezek?**

Nem kell tooupgrade a meglévő Azure-eszközök toocreate, csatolja vagy 1 TB-nál nagyobb lemezek átméretezése. a VHD fájlt tooupload helyszíni közvetlenül tooAzure oldalakra vonatkozó blob vagy nem felügyelt lemez, toouse hello legújabb eszköz beállítása szükséges:

|Azure-eszközök      | Támogatott verziók                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | 4.1.0 verziószám: június 2017 kiadás vagy újabb verzió|
|Az Azure CLI 1-es verzió     | Verziószám 0.10.13: Előfordulhat, hogy 2017 kiadás vagy újabb verzió|
|AzCopy           | 6.1.0 verziószám: június 2017 kiadás vagy újabb verzió|

az Azure parancssori felület v2 és Azure Tártallózó hello támogatása hamarosan elérhető. 

**Nem felügyelt lemezek vagy lapblobokat támogatott P4 és P6 mérete?**

Nem. P4 (32 GB) és P6 (64 GB-os) lemezméret csak felügyelt lemezek támogatottak. Nem felügyelt lemezek és a lapblobokat támogatása hamarosan elérhető.

**Ha a meglévő prémium szintű felügyelt lemez kevesebb, mint 64 GB-os hozták létre, mielőtt hello kisebb lemezt engedélyezése (körülbelül 2017. június 15.), hogyan azt számlázása?**

64 GB-nál kisebb meglévő kis premium lemezek továbbra is számlázva toobe függően toohello P10 tarifacsomagot. 

**Hogyan átválthatja hello lemez réteg 64 GB-nál kisebb kis premium lemezek P10 tooP4 vagy P6?**

Pillanatkép készítése a kis kapacitású lemezeket, és hozzon létre egy lemez tooautomatically kapcsoló hello árképzési szint tooP4 vagy P6 kiépített hello mérete alapján. 


## <a name="what-if-my-question-isnt-answered-here"></a>Mi történik, ha a fentiekben itt nem választ?

Ha kérdését itt nem látható, ossza meg velünk, és segítünk talált választ. Ez a cikk végén hello kérdés beküldheti hello megjegyzésekben. tooengage hello Azure Storage-csapattal és a Közösség más tagjaival kapcsolatos ebben a cikkben használja hello MSDN [Azure Storage fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).

toorequest szolgáltatásokat nyújt a kérelmek és ötleteket toohello [Azure Storage-visszajelzési fórumon](https://feedback.azure.com/forums/217298-storage).
