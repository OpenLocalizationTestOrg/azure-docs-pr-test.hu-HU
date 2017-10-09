---
title: "Azure-szolgáltatásokhoz aaaVirtual méreteket |} Microsoft Docs"
description: "Az Azure cloud service webes és feldolgozói szerepkörök hello különböző virtuális gépek méretét (és azonosítók) ismerteti."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1127c23e-106a-47c1-a2e9-40e6dda640f6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 93d91a67afc352f3d18c31e0dd5cf976bf46350c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-cloud-services"></a>A Felhőszolgáltatások mérete
Ez a témakör ismerteti a hello elérhető méretek és a felhőalapú szolgáltatás szerepkörpéldányokat (webes és feldolgozói szerepkörök) beállításokat. Telepítési szempontok toobe ügyelnie, ha a tervezési toouse ezeket az erőforrásokat is biztosít. Minden méretét helyezett Azonosítóval rendelkezik a [szolgáltatásdefiníciós fájl](cloud-services-model-and-package.md#csdef). Egyes árak állnak rendelkezésre hello [Cloud Services díjszabása](https://azure.microsoft.com/pricing/details/cloud-services/) lap.

> [!NOTE]
> toosee kapcsolatos Azure korlátozását, lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md)
>
>

## <a name="sizes-for-web-and-worker-role-instances"></a>Webes és feldolgozói szerepkör-példányok méretek
Nincsenek több standard méretű toochoose az Azure-on. A méretek némelyikével kapcsolatos megfontolások a következők:

* D sorozatú virtuális gépek rendszer nagyobb számítási teljesítményt és a ideiglenes lemezek igény tervezett toorun alkalmazás. D sorozatú virtuális gépek hello mennyiségű ideiglenes lemezes gyorsabb processzorok, memória-core magasabb arány és egy SSD-meghajtót (SSD) adja meg. További információkért lásd: hello közlemény a hello Azure blog [D sorozatú új virtuálisgép-méretek](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).
* Dv2-sorozat, utólagos toohello eredeti D sorozat nagyobb teljesítményű CPU szolgáltatásokat. hello Dv2-sorozat CPU pedig körülbelül 35 % gyorsabb, mint az hello D sorozatú Processzor. Alapul hello legújabb generációját 2,4 GHz-es Intel Xeon® E5-2673 v3 (Haswell) processzor, és a hello Intel Turbo program technológia 2.0, lépjen be too3.1 GHz-es. hello Dv2-sorozat rendelkezik hello memóriát és lemezterületet ugyanazokat a konfigurációkat, a D sorozat hello.
* G sorozatú virtuális gépek hello kínálnak a legtöbb memóriát, majd futtassa rendelkező Intel Xeon E5 V3 rendszerű gazdagépeken.
* A-sorozatú virtuális gépek hello is telepíthető a különböző hardvertípusok, és a processzorok. hello mérete folyamatban van, a hello hardver, egységes processzorteljesítmény toooffer, függetlenül a telepítették hello hardver-példányt futtató hello alapján. toodetermine hello fizikai hardver ekkora telepítve lekérdezés hello virtuális hardver a virtuális gép hello belül.
* A0 méretű hello túlzott előfizetett hello fizikai hardveren. Csak a meghatározott méretet, a többi felhasználói telepítés hatással lehet a futó számítási feladatok teljesítményére hello. hello relatív teljesítménye tulajdonos tooan hozzávetőleges variancia 15 százalékos várt hello alapjául az alábbiakban leírt.

hello virtuális gép hello mérete befolyásolja a hello árképzési. hello mérete befolyásolja a hello feldolgozás, a memória és a tárolási kapacitás hello virtuális gép is. Tárolási költségek hello tárfiókban használt lapok számítása külön alapul. További információkért lásd: [Cloud Services díjszabása](https://azure.microsoft.com/pricing/details/cloud-services/) és [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).

a következő szempontok hello segítséget eldöntheti, hogy a egy mérete:

* hello A8-A11 és H-sorozat értékek is *számítási igényű példányok*. hello hardver, ezek mérete futtató terveztek és számítási igényű optimalizálva, és a hálózati igényű alkalmazásokat, beleértve a nagy teljesítményű számítástechnikai (HPC) alkalmazások, modellezési és szimulációja fürtre. A8-A11 adatsorozat hello Intel Xeon E5-2670 @ 2.6-os GHz-es pedig hello H-sorozat Intel Xeon E5-2667 v3 @ 3,2 GHz-es használja. Részletes információkat és szempontokat, ezek mérete használatáról, [nagy teljesítményű számítási Virtuálisgép-méretek](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Dv2-sorozat, D sorozatú, G-sorozat, alkalmazásokat, amelyek a gyorsabb CPU igény ideálisak, jobb helyi lemez teljesítményét, vagy hogy a nagyobb memória iránti igények kielégítése érdekében. Nagyon hatékony kombinációt kínálnak számos nagyvállalati szintű alkalmazáshoz.
* Fizikai gazdagépek hello Azure-adatközpont némelyike nem támogatja a nagyobb virtuális gépek méretét, például A5 – A11. Ennek eredményeképpen előfordulhat, hogy hibaüzenet jelenik meg hello **sikertelen tooconfigure virtuális gépet {számítógépnév}** vagy **sikertelen toocreate virtuális gépet {számítógépnév}** egy meglévő virtuális gép új tooa átméretezésekor méret; 2013. április 16.; előtt létrehozott virtuális hálózatban egy új virtuális gép létrehozása vagy hozzáadása egy új virtuális gép tooan meglévő felhőalapú szolgáltatást. Lásd: [hiba: "Nem sikerült tooconfigure virtuális gép"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) hello támogatási fórumon a lehetséges megoldások az egyes központi telepítési forgatókönyvek esetén.
* Az előfizetés is korlátozhatja bizonyos mérete családok telepítheti magok hello száma. a kvóta tooincrease Azure ügyfélszolgálatához.

## <a name="performance-considerations"></a>A teljesítménnyel kapcsolatos megfontolások
Létrehoztunk hello Azure számítási egység (ACU) tooprovide egy mód (CPU) számítási teljesítmény összehasonlítása Azure termékváltozatok és tooidentify, amely SKU valószínűleg toosatisfy hello fogalma a teljesítmény szükséges.  Az ACU jelenlegi standard alapjaként a Kisméretű (Standard_A1) virtuális gép 100-as értéket képvisel, és a többi termékváltozat értéke ehhez képest jelöli, hogy mennyivel gyorsabban futtatja az adott termékváltozat a standard teljesítménytesztet.

> [!IMPORTANT]
> hello ACU eszközöket csak. a terhelést eredmények hello változhat.
>
>

<br>

| Termékváltozat-család | ACU/mag |
| --- | --- |
| [ExtraSmall](#a-series) |50 |
| [Kis ExtraLarge](#a-series) |100 |
| [A5-7](#a-series) |100 |
| [Standard_A1-8v2](#av2-series) |100 |
| [Standard_A2m-8mv2](#av2-series) |100 |
| [A8-A11](#a-series) |225* |
| [D1-14](#d-series) |160 |
| [D1-15v2](#dv2-series) |210 - 250* |
| [G1-5](#g-series) |180 - 240* |
| [H](#h-series) |290 - 300* |

Jelölésű ACUs egy * Intel® Turbo technológia tooincrease CPU gyakoriságot használja, és adja meg a Teljesítmény program. hello hello program mennyisége függően változhat hello Virtuálisgép-méretet, a munkaterhelés és a más hello futó munkaterhelések ugyanazon a gazdagépen.

## <a name="size-tables"></a>Mérettáblázatok
hello alábbi táblázatokban hello méretű és hello kapacitást biztosítanak.

* A tárolókapacitás mértékegysége GiB (gibibájt = 1024^3 bájt). Amikor mért lemezek összehasonlítása GB (1000 ^ 3 bájt) toodisks mért GiB (1024 ^ 3) ne feledje, hogy a megadott GiB kapacitás számok kisebb lehet, hogy megjelennek. Például: 1023 GiB = 1098,4 GB
* A lemezteljesítmény másodpercenkénti bemeneti/kimeneti műveletek (IOPS) mennyiségeként van kifejezve, valamint MBps-ben, ahol 1 MBps = 10^6 bájt/másodperc.
* Az adatlemezek gyorsítótárazott és gyorsítótárazás nélküli módban üzemelhetnek. A gyorsítótárazott adatokat a lemez művelethez, hello állomás gyorsítótáras üzemmód túl van beállítva**ReadOnly** vagy **ReadWrite**. Nem gyorsítótárazott adatok lemez művelethez, hello állomás gyorsítótáras üzemmód túl van beállítva**nincs**.
* Maximális hálózati sávszélességének hello maximális összesített sávszélesség lefoglalt és VM típusonkénti rendelve. hello maximális sávszélesség nyújt útmutatást hello jobb VM típus tooensure megfelelő hálózati kapacitás kijelöléséhez. Ha alacsony, közepes, magas és nagyon magas közötti áthelyezése, hello átviteli ennek megfelelően növekszik. A tényleges hálózati teljesítmény számos tényezőtől függ, többek közt a hálózat és az alkalmazás terhelésétől, valamint az alkalmazás hálózati beállításaitól.

## <a name="a-series"></a>A-sorozat
| Méret            | Processzormagok | Memória: GiB  | Helyi merevlemez: GiB       | Hálózati adapterek max. száma/hálózati sávszélesség |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| ExtraSmall      | 1         | 0.768        | 20                   | 1/alacsony |
| Kicsi           | 1         | 1.75         | 225                  | 1/közepes |
| Közepes          | 2         | 3,5 GB       | 490                  | 1/közepes |
| Nagy           | 4         | 7            | 1000                 | 2/magas |
| ExtraLarge      | 8         | 14           | 2040                 | 4/magas |
| A5              | 2         | 14           | 490                  | 1/közepes |
| A6              | 4         | 28           | 1000                 | 2/magas |
| A7              | 8         | 56           | 2040                 | 4/magas |

## <a name="a-series---compute-intensive-instances"></a>A-sorozat – nagy számítási igényű példányok
További tudnivalókat és szempontokat, ezek mérete használatáról, tekintse meg a [nagy teljesítményű számítási Virtuálisgép-méretek](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Méret            | Processzormagok | Memória: GiB  | Helyi merevlemez: GiB       | Hálózati adapterek max. száma/hálózati sávszélesség |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| A8 *             |8          | 56           | 1817                 | 2/magas |
| A9-ES *             |16         | 112          | 1817                 | 4/nagyon magas |
| A10             |8          | 56           | 1817                 | 2/magas |
| A11             |16         | 112          | 1817                 | 4/nagyon magas |

\*RDMA-kompatibilis

## <a name="av2-series"></a>Av2-sorozat

| Méret            | Processzormagok | Memória: GiB  | Helyi SSD: GiB       | Hálózati adapterek max. száma/hálózati sávszélesség |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_A1_v2  | 1         | 2            | 10                   | 1/közepes                 |
| Standard_A2_v2  | 2         | 4            | 20                   | 2/közepes                 |
| Standard_A4_v2  | 4         | 8            | 40                   | 4/magas                     |
| Standard_A8_v2  | 8         | 16           | 80                   | 8/magas                     |
| Standard_A2m_v2 | 2         | 16           | 20                   | 2/közepes                 |
| Standard_A4m_v2 | 4         | 32           | 40                   | 4/magas                     |
| Standard_A8m_v2 | 8         | 64           | 80                   | 8/magas                     |


## <a name="d-series"></a>D-sorozat
| Méret            | Processzormagok | Memória: GiB  | Helyi SSD: GiB       | Hálózati adapterek max. száma/hálózati sávszélesség |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1     | 1         | 3.5          | 50                   | 1/közepes |
| Standard_D2     | 2         | 7            | 100                  | 2/magas |
| Standard_D3     | 4         | 14           | 200                  | 4/magas |
| Standard_D4     | 8         | 28           | 400                  | 8/magas |
| Standard_D11    | 2         | 14           | 100                  | 2/magas |
| Standard_D12    | 4         | 28           | 200                  | 4/magas |
| Standard_D13    | 8         | 56           | 400                  | 8/magas |
| Standard_D14    | 16        | 112          | 800                  | 8/nagyon magas |

## <a name="dv2-series"></a>Dv2-sorozat
| Méret            | Processzormagok | Memória: GiB  | Helyi SSD: GiB       | Hálózati adapterek max. száma/hálózati sávszélesség |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1_v2  | 1         | 3.5          | 50                   | 1/közepes |
| Standard_D2_v2  | 2         | 7            | 100                  | 2/magas |
| Standard_D3_v2  | 4         | 14           | 200                  | 4/magas |
| Standard_D4_v2  | 8         | 28           | 400                  | 8/magas |
| Standard_D5_v2  | 16        | 56           | 800                  | 8/rendkívül magas |
| Standard_D11_v2 | 2         | 14           | 100                  | 2/magas |
| Standard_D12_v2 | 4         | 28           | 200                  | 4/magas |
| Standard_D13_v2 | 8         | 56           | 400                  | 8/magas |
| Standard_D14_v2 | 16        | 112          | 800                  | 8/rendkívül magas |
| Standard_D15_v2 | 20        | 140          | 1,000                | 8/rendkívül magas |

## <a name="g-series"></a>G-sorozat
| Méret            | Processzormagok | Memória: GiB  | Helyi SSD: GiB       | Hálózati adapterek max. száma/hálózati sávszélesség |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_G1     | 2         | 28           | 384                  |1/magas |
| Standard_G2     | 4         | 56           | 768                  |2/magas |
| Standard_G3     | 8         | 112          | 1,536                |4/nagyon magas |
| Standard_G4     | 16        | 224          | 3,072                |8/rendkívül magas |
| Standard szintű, G5     | 32        | 448          | 6,144                |8/rendkívül magas |

## <a name="h-series"></a>H-sorozat
Az Azure H sorozatú virtuális gépek hello következő generációs nagy teljesítményű számítástechnikai a virtuális gépek számítási igények felső határát, például molekuláris modellezési és számítási folyadékból dynamics célzó. Ezek 8 és 16 mag virtuális gépek hello Intel Haswell E5-2667 V3 processzor technológia DDR4 memória és a helyi SSD-alapú tárolási kiemeli épül.

Ezenkívül toohello jelentős processzorteljesítményre, hello H-sorozat különböző lehetőséget kínál kis késleltetésű RDMA hálózati FDR InfiniBand és több memória konfigurációk toosupport intenzív számítási memóriakövetelményei használatával.

| Méret            | Processzormagok | Memória: GiB  | Helyi SSD: GiB       | Hálózati adapterek max. száma/hálózati sávszélesség |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_H8     | 8         | 56           | 1000                 | 8/magas |
| Standard_H16    | 16        | 112          | 2000                 | 8/nagyon magas |
| Standard_H8m    | 8         | 112          | 1000                 | 8/magas |
| Standard_H16m   | 16        | 224          | 2000                 | 8/nagyon magas |
| Standard_H16r*  | 16        | 112          | 2000                 | 8/nagyon magas |
| Standard_H16mr* | 16        | 224          | 2000                 | 8/nagyon magas |

\*RDMA-kompatibilis

## <a name="configure-sizes-for-cloud-services"></a>A Felhőszolgáltatások mérete konfigurálása
Hello szerint hello modell részeként is megadhat a hello virtuális gép méretétől, a szerepkör példánya [szolgáltatásdefiníciós fájl](cloud-services-model-and-package.md#csdef). hello szerepkör hello mérete hello CPU magok száma, hello memóriakapacitása, és hello helyi rendszer fájlméret-példányt futtató tooa lefoglalt határozza meg. Válassza ki a hello szerepkör méretét az alkalmazás erőforrás követelmény alapján.

Példa hello szerepkör méretének toobe beállításához [D2](#general-purpose-d) a webes szerepkör példány:

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2">
...
</WorkerRole>
```

## <a name="changing-hello-size-of-an-existing-role"></a>Egy meglévő szerepkör hello méretének módosítása

Hello jellegét a munkaterhelési változások vagy új Virtuálisgép-méretek elérhető legyen, mint érdemes lehet a szerepkör toochange hello méretét. toodo úgy kell módosítását hello Virtuálisgép-méretet a szolgáltatásdefiníciós fájlban (ahogy fent látható), a felhőalapú szolgáltatás csomagolja újra és telepítheti azt. Már nem lehetséges toochange Virtuálisgép-méretek közvetlenül a hello portál vagy PowerShell.

>[!TIP]
> Érdemes lehet toouse másik Virtuálisgép-méretek a szerepkör különböző környezetekben (például) teszt vs éles környezet). Egyirányú toodo ez toocreate több szolgáltatás definíciós (.csdef) fájlt a projektben, akkor hozzon létre másik felhőt / környezet szervizcsomagok során az automatikus létrehozási hello CSPack eszközzel. További információ a felhő hello elemeinek toolearn services csomag, és hogyan toocreate őket, [modell hello felhő Újdonságok szolgáltatását, és hogyan tegye csomag azt?](cloud-services-model-and-package.md)
>
>

## <a name="get-a-list-of-sizes"></a>Méretek listáját
Használhatja a PowerShell vagy a hello REST API tooget méretének megtekintéséhez. hello REST API dokumentált [Itt](https://msdn.microsoft.com/library/azure/dn469422.aspx). hello alábbira egy PowerShell-parancsot, amely a jelenleg rendelkezésre álló összes hello méretének felsorolja a felhőalapú szolgáltatáshoz.

```powershell
Get-AzureRoleSize | where SupportedByWebWorkerRoles -eq $true | select InstanceSize
```

## <a name="next-steps"></a>Következő lépések
* Ismerje meg [az Azure-előfizetések és -szolgáltatások korlátozásait, kvótáit és megkötéseit](../azure-subscription-service-limits.md).
* További [kapcsolatos nagy teljesítményű számítási a Virtuálisgép-méretek](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) HPC-munkaterhelések.
