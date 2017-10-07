---
title: "SQL Server virtuális gépek aaaStorage konfigurációs |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan Azure storage tartozó konfigurálása SQL Server virtuális gépen (Resource Manager üzembe helyezési modellben) kiépítése során. Azt is bemutatja, hogyan konfigurálhatja a tárolási a meglévő SQL Server virtuális géphez."
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: ninarn
ms.openlocfilehash: b50dbd698828780cfc044fa0966e8f4e2f3bb6c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storage-configuration-for-sql-server-vms"></a>Tárolási konfigurációt az SQL Server virtuális gépen
Amikor konfigurál egy SQL Server virtuálisgép-lemezkép az Azure-ban, a portál hello a tárolási konfigurációt segít tooautomate. Ez magában foglalja, hozzácsatolása ehhez a tárolási toohello VM, így a tárhely elérhető tooSQL kiszolgáló, és ki az adott igények szerint toooptimize konfigurálása.

Ez a témakör azt ismerteti, hogyan Azure storage tartozó konfigurálása az SQL Server virtuális gépen is kiépítése során, és a meglévő virtuális gépek. Ez a konfiguráció alapján hello [teljesítmény gyakorlati tanácsok](virtual-machines-windows-sql-performance.md) SQL Servert futtató Azure virtuális gépekhez.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Előfeltételek
toouse hello automatikus tárolási beállításait, a virtuális géphez szükséges hello a következő jellemzőkkel:

* A kiépített egy [SQL Server-gyűjtemény rendszerképet](virtual-machines-windows-sql-server-iaas-overview.md#option-1-create-a-sql-vm-with-per-minute-licensing).
* Felhasználási hello [Resource Manager üzembe helyezési modellben](../../../azure-resource-manager/resource-manager-deployment-model.md).
* Használja a [prémium szintű Storage](../../../storage/common/storage-premium-storage.md).

## <a name="new-vms"></a>Új virtuális gépek
hello alábbi szakaszok azt ismertetik, hogyan tooconfigure tárhely az új SQL Server virtuális gépek.

### <a name="azure-portal"></a>Azure Portal
Ha egy Azure virtuális gép az SQL Server-gyűjtemény lemezkép használatával, dönthet úgy tooautomatically hello tárolási konfigurálása az új virtuális gép. Megadhatja, hogy hello tárméret teljesítménykorlátok és munkaterhelésének típusát. hello alábbi képernyőfelvételen látható hello tárolási konfiguráció panel során SQL virtuális gép kiépítése.

![SQL Server virtuális gép tárolási konfigurációt kiépítése során](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

A választott alapján, az Azure-ban hello tárolókonfigurálást hello virtuális gép létrehozása után a következő:

* Hoz, és csatolja a prémium szintű storage adatok lemezek toohello virtuális gépet.
* Hello adatok lemezek toobe elérhető tooSQL kiszolgáló konfigurálása.
* Hello adatlemezek konfigurálja a tárba. egy készlet hello alapján meghatározott mérete és teljesítménye (iops-érték és átviteli) követelményeknek.
* Hello tárolókészlet társít egy új meghajtót hello virtuális gépen.
* Az új meghajtó (az adatraktározás terén, a tranzakciós feldolgozását, vagy általános) a megadott munkaterhelés típusától függően optimalizálja.

További tájékoztatást talál a hogyan Azure tárolási beállítások konfigurálása, lásd: hello [tárolási konfigurációs szakasz](#storage-configuration). Hogyan toocreate hello Azure portál, az SQL Server virtuális gép: teljes részletes útmutatás [oktatóanyag kiépítés hello](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Erőforrás-kezelés sablonok
Resource Manager-sablonok a következő hello használatakor a két premium adatok lemezek vannak csatolva hozzá alapértelmezés szerint a nincs tárolókészlet konfigurációját. Azonban testre szabhatja a prémium szintű adatlemezek, amelyek a virtuális géphez csatolt toohello sablonok toochange hello száma.

* [Virtuális gép létrehozása az automatikus biztonsági mentés](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Virtuális gép létrehozása az automatikus javítás](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [Virtuális gép létrehozása az AKV-integráció](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Meglévő virtuális gépek
Meglévő SQL Server virtuális gépek módosíthatja az egyes tárolási beállításai hello Azure-portálon. Válassza ki a virtuális gép toohello beállítások területen nyissa, és válassza az SQL Server-konfigurációs. hello SQL Server-konfigurációs hello aktuális tárolási használatát a virtuális gép megjelenik. A virtuális gép meglévő összes meghajtó jelennek meg ezen a diagramon. Minden meghajtó hello tárolóhely jeleníti meg a négy részből áll:

* SQL-adatok
* SQL-napló
* Másik (az SQL tároló)
* Elérhető

![Meglévő SQL Server rendszerű virtuális Géphez a tárolás konfigurálása](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

tooconfigure tárolási tooadd új meghajtóra hello, vagy egy meglévő meghajtó bővítésével, kattintson a hello szerkesztési hivatkozáshoz hello diagram felett.

hello konfigurációs beállítások, melyek megjelennek attól függ, hogy ezt a szolgáltatást, mielőtt használja. Ha első alkalommal használja hello, az új meghajtó tárolási követelményeit is megadhat. Ha korábban már használta a szolgáltatás toocreate egy meghajtó, dönthet úgy tooextend a lemezes tárolást.

### <a name="use-for-hello-first-time"></a>Az első alkalommal hello használata
Esetén az első alkalommal ezzel a szolgáltatással, megadhat hello tárolási méretét és a teljesítmény korlátok egy új meghajtót. Ez a szolgáltatás hasonló toowhat mutatunk be, a kiépítés idő. hello fő különbség az, hogy toospecify hello munkaterhelésének típusát nem engedélyezettek. Ez a korlátozás megakadályozza, hogy hello virtuális gépen meglévő SQL Server konfigurációk megszakítása.

![Konfigurálja az SQL Server Storage csúszkák](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

A specifikációk alapján új meghajtót az Azure létrehoz. Ebben a forgatókönyvben az Azure-ban a következő tárolókonfigurálást hello:

* Hoz, és csatolja a prémium szintű storage adatok lemezek toohello virtuális gépet.
* Hello adatok lemezek toobe elérhető tooSQL kiszolgáló konfigurálása.
* Hello adatlemezek konfigurálja a tárba. egy készlet hello alapján meghatározott mérete és teljesítménye (iops-érték és átviteli) követelményeknek.
* Hello tárolókészlet társít egy új meghajtót hello virtuális gépen.

További tájékoztatást talál a hogyan Azure tárolási beállítások konfigurálása, lásd: hello [tárolási konfigurációs szakasz](#storage-configuration).

### <a name="add-a-new-drive"></a>Adjon hozzá egy új meghajtót
Ha már konfigurálta az SQL Server virtuális gép tárolási, két új beállítások tárolási bővülő megjelenik. hello első lehetőség új meghajtóra, ami növelheti a virtuális gép hello teljesítményszintjének tooadd.

![Adja hozzá az új meghajtó tooa SQL virtuális gép](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Azonban a felvett hello meghajtó, végezze el bizonyos nagyon kézi konfigurálás tooachieve hello teljesítmény növelését.

### <a name="extend-hello-drive"></a>Hello meghajtó bővítésével
hello másik lehetőség a tárolási bővített egy tooextend hello meglévő meghajtó. Ez a beállítás növeli a meghajtón rendelkezésre álló tár hello, de nem növeli a teljesítményt. A tárolókészletekben hello oszlopok száma nem módosítható, hello tárolókészlet létrehozása után. az oszlopok száma hello hello száma párhuzamos írási műveletekről, amelyek is szétteríti a hello adatlemezek határozza meg. Ezért bármely hozzáadott adatlemezek nem teljesítmény növelése érdekében. További tárhelyet csak hello adat íródik a is biztosítanak. Ezt a korlátozást is azt jelenti, hogy hello meghajtó bővítésekor hello oszlopok száma határozza meg, amely adhat hozzá adatlemezt hello minimális száma. Ezért ha négy adatlemezek tárolókészletet hoz létre, hello számú oszlopot is négy. Kiterjesztheti a hello tárolási, amikor hozzá kell adnia legalább négy adatlemezek.

![A meghajtó bővítésével egy SQL virtuális gép számára](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Tároló konfigurálása
Ez a témakör egy hivatkozást hello tárolási konfigurációs módosítások Azure automatikusan SQL Virtuálisgép-létrehozásnál vagy hello Azure Portal konfiguráció során hajtja végre.

* Ha kevesebb mint két több TB-nyi tárhelyet a virtuális Gépet jelölt ki, az Azure nem hoz létre tárolókészlet.
* Ha legalább két több TB-nyi tárhelyet a virtuális Gépet jelölt ki, Azure egy tárolókészlet konfigurálását jelenti. a jelen témakör következő szakaszában hello ismerteti hello hello tárolókészlet konfigurációját.
* Mindig automatikus tárolási konfigurációt használja [prémium szintű storage](../../../storage/common/storage-premium-storage.md) P30 adatlemezek. Következésképpen van a kijelölt szám terabájt közötti társítás 1:1, és a csatolt tooyour VM adatlemezek hello száma.

Díjszabási információkért lásd: hello [Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage) oldalon, hello **lemezegységet** fülre.

### <a name="creation-of-hello-storage-pool"></a>Hello tárolókészlet létrehozása
Azure beállítások toocreate hello tárolókészlet követően az SQL Server virtuális gépeken futó hello használja.

| Beállítás | Érték |
| --- | --- |
| Paritásos mérete |(Az adatraktározás terén); 256 KB 64 KB-os (tranzakciós) |
| Lemezméretek |1 TB |
| Gyorsítótár |Olvasás |
| Foglalási mérete |64 KB-os NTFS foglalásiegység-méret |
| Azonnali fájlinicializálása |Engedélyezve |
| A memóriában memórialapok zárolása |Engedélyezve |
| Helyreállítás |Egyszerű helyreállítási (rugalmasság nélküli) |
| Az oszlopok száma |Adatlemezek száma<sup>1</sup> |
| A TempDB helye |Az adatlemezek tárolása<sup>2</sup> |

<sup>1</sup> hello tárolókészlet létrehozása után nem változtathatja meg hello hello tárolókészlet oszlopainak száma.

<sup>2</sup> Ez a beállítás csak érvényes toohello első meghajtó hoz létre hello tárolási konfiguráció szolgáltatással.

## <a name="workload-optimization-settings"></a>Munkaterhelés optimalizálási beállításai
hello következő táblázatban hello három munkaterhelés típus lehetőségeit és a megfelelő optimalizálási lehetőségeit.

| Munkaterhelésének típusát | Leírás | Optimalizálás. |
| --- | --- | --- |
| **Általános** |Alapértelmezett beállítás, amely támogatja a legtöbb alkalmazás és szolgáltatás |None |
| **Tranzakciós feldolgozást** |Optimalizálja a hello tárhely az adatbázisok hagyományos OLTP számítási |Nyomkövetési jelző 1117<br/>Nyomkövetési jelző 1118 |
| **Adatraktározás** |Az elemzési és jelentéskészítési számítási hello tárolási optimalizálja. |Nyomkövetési jelző 610<br/>Nyomkövetési jelző 1117 |

> [!NOTE]
> Hello munkaterhelésének típusát csak adja meg, amikor kijelöli a hello tárolási konfigurációs lépés egy SQL-virtuális gép.
>
>

## <a name="next-steps"></a>Következő lépések
Egyéb témakörök kapcsolódó toorunning SQL Server Azure virtuális gépeken, a következő témakörben: [SQL Server Azure virtuális gépeken](virtual-machines-windows-sql-server-iaas-overview.md).
