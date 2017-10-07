---
title: "aaaUse számítási igényű Azure virtuális gépek kötegelt |} Microsoft Docs"
description: "Hogyan tootake előny az RDMA-kompatibilisek-e vagy GPU-engedélyezve van a virtuális gép méretének Azure Batch-készletek"
services: batch
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: 
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: danlep
ms.openlocfilehash: 6a462a5f2a44ddcec8bf4e5c200d444cac8fafe6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Használja az RDMA-kompatibilisek-e vagy GPU-kompatibilis példány kötegelt készletek

toorun bizonyos kötegelt feladatok, érdemes lehet a nagyméretű számítási készült Azure Virtuálisgép-méretek tootake előnyeit. Például toorun többpéldányos [MPI terhelések](batch-mpi.md), dönthet úgy, A8 és A9, vagy H-sorozat méreteket egy hálózati csatoló a távoli közvetlen memória hozzáférés (RDMA). Ezek méretek tooan InfiniBand hálózati csomópontok közötti kommunikációhoz, amely meggyorsíthatja MPI alkalmazások csatlakozzon. Vagy CUDA alkalmazások esetén dönthet úgy, amely tartalmazza az NVIDIA Tesla grafikus processzor a processzorok kártyák N-sorozat mérete.

Ez a cikk nyújt útmutatást és példákat toouse Speciális méretű Azure Batch készletek néhány. A specifikációk és a háttérben lásd:

* Nagy teljesítményű számítási Virtuálisgép-méretek ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* Virtuálisgép-méretek GPU-t ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Előfizetés és korlátai

* **Kvóták** -korlátozhatja, hogy egy vagy több Azure-kvótákról hello vagy típus csomópontok tooa Batch-készlet adhat hozzá. Jelenleg korlátozott, ha úgy dönt, RDMA-kompatibilisek-e, GPU-t, nagy valószínűséggel toobe vagy más multicore Virtuálisgép-méretek. A Batch-fiókhoz létrehozott hello típusától függően hello kvóták toohello fiókon vagy tooyour előfizetés sikerült vonatkoznak.

    * A Batch-fiókhoz létrehozott hello **a Batch szolgáltatás** konfigurációs, hello által korlátozott [dedikált magok kvóta értéke Batch-fiók](batch-quota-limit.md#resource-quotas). Alapértelmezés szerint ez a kvóta 20 mag. Egy külön kvóta túl vonatkozik[alacsony prioritású virtuális gépek](batch-low-pri-vms.md), ha használja őket. 

    * Ha hello hello fiók létrehozott **felhasználói előfizetési** konfigurációs, az előfizetés virtuális gép magok régiónként hello számának korlátozása. Lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md). Az előfizetés egy regionális kvóta toocertain Virtuálisgép-méretek, beleértve a HPC és GPU-példányok is vonatkozik. Hello felhasználó előfizetés konfigurációja nem további kvóták toohello Batch-fiók vonatkoznak. 

  Szükség lehet tooincrease egy vagy több kvóták kötegben egy speciális Virtuálisgép-méretet használatakor. a kvóta növelését, nyissa meg toorequest egy [online felhasználói támogatási kérelem](../azure-supportability/how-to-create-azure-support-request.md) díjmentesen.

* **Régiónkénti elérhetőség** - számítási igényű virtuális gépek nem állnak rendelkezésre hello régiókban, ahol a Batch-fiókokat hozhat létre. toocheck, hogy a mérete nem érhető el, lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Függőségek

hello RDMA és a GPU képességeket számítási igényű méretű csak az egyes operációs rendszerek támogatottak. Az operációs rendszertől függően előfordulhat, hogy tooinstall kell, vagy további illesztőprogram vagy egyéb szoftverek konfigurálása. a következő táblák hello foglalják össze ezeket a függőségeket. Tekintse meg a csatolt cikkek részleteiről. Beállítások tooconfigure kötegelt készletek lásd: a cikk későbbi részében.


### <a name="linux-pools---virtual-machine-configuration"></a>Linux-készletek - virtuálisgép-konfiguráció

| Méret | Képesség | Operációs rendszerek | Szükséges szoftverek | Az alkalmazáskészlet beállításai |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr A8, a9 csomag](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | SUSE Linux Enterprise Server 12 HPC vagy<br/>CentOS-alapú HPC<br/>(Az azure piactéren) | Intel MPI 5 | Csomópontok közötti kommunikáció engedélyezéséhez egyidejű feladat a végrehajtás letiltása |
| [Hálózati vezérlő által adatsorozat *](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms) | NVIDIA Tesla K80 GPU | Ubuntu 16.04 LTS.<br/>Red Hat Enterprise Linux 7.3, vagy<br/>CentOS-alapú 7.3<br/>(Az azure piactéren) | NVIDIA CUDA eszközkészlet 8.0 illesztőprogramok | N/A | 
| [Portok HV-sorozat](../virtual-machines/linux/n-series-driver-setup.md#install-grid-drivers-for-nv-vms) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS<br/>Red Hat Enterprise Linux 7.3<br/>CentOS-alapú 7.3<br/>(Az azure piactéren) | NVIDIA rács 4.3 illesztőprogramok | N/A |

* 7.3 HPC CentOS alapú Intel MPI RDMA-kapcsolatot NC24r virtuális gépeken támogatott.



### <a name="windows-pools---virtual-machine-configuration"></a>Windows-készletek - virtuálisgép-konfiguráció

| Méret | Képesség | Operációs rendszerek | Szükséges szoftverek | Az alkalmazáskészlet beállításai |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr A8, a9 csomag](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2 vagy<br/>A Windows Server 2012 (az Azure piactéren) | Microsoft MPI 2012 R2 vagy újabb verzióját, vagy<br/> Intel MPI 5<br/><br/>HpcVMDrivers Azure Virtuálisgép-bővítmény | Csomópontok közötti kommunikáció engedélyezéséhez egyidejű feladat a végrehajtás letiltása |
| [Hálózati vezérlő által adatsorozat *](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla K80 GPU | Windows Server 2016 vagy <br/>Windows Server 2012 R2 (az Azure piactéren) | NVIDIA Tesla illesztőprogramok vagy CUDA eszközkészlet 8.0 illesztőprogramok| N/A | 
| [Portok HV-sorozat](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Windows Server 2016 vagy<br/>Windows Server 2012 R2 (az Azure piactéren) | NVIDIA rács 4.3 illesztőprogramok | N/A |

* RDMA-kapcsolatot NC24r virtuális gépeken támogatott, a Windows Server 2012 R2 HpcVMDrivers bővítmény és Microsoft MPI vagy Intel MPI.

### <a name="windows-pools---cloud-services-configuration"></a>Windows-készletek - Felhőszolgáltatások konfigurálása

> [!NOTE]
> N-sorozat méretek hello felhő konfigurálása a kötegelt készletek nem támogatottak.
>

| Méret | Képesség | Operációs rendszerek | Szükséges szoftverek | Az alkalmazáskészlet beállításai |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr A8, a9 csomag](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2 rendszerben<br/>Windows Server 2012-ben, vagy<br/>Windows Server 2008 R2 (Vendég operációsrendszer-család) | Microsoft MPI 2012 R2 vagy újabb verzióját, vagy<br/>Intel MPI 5<br/><br/>HpcVMDrivers Azure Virtuálisgép-bővítmény | Csomópontok közötti kommunikáció engedélyezése<br/> Tiltsa le az egyidejű feladat a végrehajtás |





## <a name="pool-configuration-options"></a>Tárolókészlet konfigurációs beállítások

tooconfigure speciális Virtuálisgép-méretet a kötegelt készlet, hello kötegelt API-k és eszközök nyújt több beállítások tooinstall szükséges szoftverek illesztőprogramok, beleértve:

* [Indítsa el a feladat](batch-api-basics.md#start-task) -egy csomag feltöltése egy erőforrás-fájl tooan hello Azure storage-fiókot, és hello Batch-fiók ugyanabban a régióban. Hozzon létre egy kezdő tevékenység parancssori tooinstall hello erőforrás csendes hello készlet indításakor. További információkért lásd: hello [REST API-dokumentáció](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask).

  > [!NOTE] 
  > hello kezdő tevékenység emelt szintű (admin) engedélyekkel kell futtatni, és azt kell várnia, hogy sikeres.
  >

* [Alkalmazáscsomag](batch-application-packages.md) – egy ZIP-telepítési csomag tooyour Batch-fiók hozzáadása és konfigurálása a csomag hivatkozást hello készletben. Ez a beállítás feltölti és unzips hello csomag hello készlet összes csomópontján. Ha hello csomag telepítő, start feladat parancssori toosilently telepítés hello alkalmazás létrehozása minden alkalmazáskészlet-csomóponton. Szükség esetén telepítse a hello csomagot, ha a feladat ütemezett toorun csomóponton.

* [Egyéni készlet kép](batch-api-basics.md#pool) – hozzon létre egyéni Windows vagy Linux Virtuálisgép-lemezkép illesztőprogram-, szoftver- vagy egyéb beállításokat tartalmazó szükséges hello Virtuálisgép-méretet. A Batch-fiók hello felhasználó előfizetés konfigurációja létrehozott, adja meg a Batch-készlet hello egyéni lemezképet. (Egyéni lemezképek nem támogatottak a hello kötegelt szolgáltatáskonfiguráció fiókok.) Egyéni lemezképek csak hello virtuálisgép-konfigurációjához célkészleteknek használható.

  > [!IMPORTANT]
  > A Batch-készletek jelenleg felügyelt lemezzel, vagy a prémium szintű Storage létrehozott egyéni lemezképe nem használható.
  >



* [A Batch-hajógyárnak](https://github.com/Azure/batch-shipyard) automatikusan beállítja hello GPU és RDMA toowork transzparens módon az Azure Batch tárolóalapú munkafolyamatok. Kötegelt hajógyárnak teljesen célja a konfigurációs fájlok. Nincsenek sok minta módszereivel konfiguráció érhető el, amelyek lehetővé teszik a GPU és RDMA munkaterhelések, például hello [CNTK GPU módszereivel](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) amely GPU illesztőprogramok N sorozatú virtuális gépeken a következő előre konfigurálhatja, és betölti a Docker-lemezkép Microsoft kognitív eszközkészlet szoftverként.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Példa: Microsoft MPI címkészlet A8 méretű VM

toorun Windows MPI alkalmazások Azure A8 csomópontok készletét, meg kell tooinstall a támogatott MPI megvalósítása. Az alábbiakban a minta lépéseket tooinstall [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) Windows tárolókészlet egy kötegelt alkalmazáscsomagot használ.

1. Töltse le a hello [telepítőcsomagját](http://go.microsoft.com/FWLink/p/?LinkID=389556) (MSMpiSetup.exe) hello Microsoft MPI legújabb verziója.
2. Hozzon létre egy zip-fájl hello csomag.
3. Töltse fel a hello csomag tooyour Batch-fiókhoz. Útmutató: hello [alkalmazáscsomagok](batch-application-packages.md) útmutatást. Adja meg, mint az alkalmazás azonosítóját *MSMPI*, és többek között verzióval *8.1*. 
4. Hello felhőszolgáltatások konfigurálása szükséges hello számú csomópontok és a skála hello kötegelt API-k vagy az Azure portál használata esetén egy készletet létrehozni. hello következő táblázatban minta beállítások tooset mentése MPI felügyelet nélküli módban, a kezdő tevékenység használatával:

| Beállítás | Érték |
| ---- | ----- | 
| **Képtípus** | Cloud Services |
| **Operációsrendszer-család** | Windows Server 2012 R2 (4 operációsrendszer-család) |
| **Csomópont mérete** | A8 Standard |
| **Fürtök csomóponton belüli kommunikációjához kommunikáció engedélyezve** | True (Igaz) |
| **Csomópontonkénti tevékenységek maximális száma** | 1 |
| **Alkalmazás csomaghivatkozásokhoz** | MSMPI |
| **Indítsa el a feladat engedélyezve van** | True (Igaz)<br>**Parancssor** - `"cmd /c %AZ_BATCH_APP_PACKAGE_MSMPI#8.1%\\MSMpiSetup.exe -unattend -force"`<br/>**Felhasználói azonosító** -készlet autouser, a rendszergazda<br/>**Várjon, amíg a sikeres** – igaz

## <a name="example-nvidia-tesla-drivers-on-nc-vm-pool"></a>Példa: NVIDIA Tesla illesztőprogramok NC Virtuálisgép-készlet

toorun CUDA alkalmazások Linux NC csomópontok készletét, meg kell tooinstall CUDA eszközkészlet 8.0 hello csomópontján. hello eszközkészlet hello szükséges NVIDIA Tesla GPU illesztőprogramokat telepít. Az alábbiakban a minta lépéseket toodeploy hello GPU-illesztőprogramok egyéni Ubuntu 16.04 LTS lemezkép:

1. Egy Azure NC6 virtuális gép fut az Ubuntu 16.04 LTS telepíthető. Például hozzon létre hello VM hello Velünk Dél központi régióban. Győződjön meg arról, hogy hozzon létre virtuális gép hello standard szintű tárolóval és *nélkül* által kezelt lemezeken.
2. Hajtsa végre a hello lépéseket tooconnect toohello VM és [CUDA illesztőprogramok telepítése](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms).
3. Linux-ügynök hello kiosztásának megszüntetése és rögzítse Linux Virtuálisgép-lemezkép hello Azure CLI 1.0-parancsok használatával. Útmutató: [Azure-on futó Linux virtuális gép rögzítése](../virtual-machines/linux/capture-image-nodejs.md). Jegyezze fel a hello lemezkép URI.
  > [!IMPORTANT]
  > Az Azure Batch Azure CLI 2.0 parancsok toocapture hello lemezképe nem használható. Hello CLI 2.0 parancsok jelenleg csak felügyelt lemezek használatával létrehozott virtuális gépek rögzíti.
  >
4. A Batch-fiók létrehozása hello felhasználói előfizetési konfiguráció esetén, amely támogatja a hálózati vezérlő által virtuális gépek régióban.
5. A kötegelt API-k hello vagy Azure-portálon hello egyéni lemezkép használatával készlet létrehozása és hello a szükségeskonfiguráció-csomópontok és a skála száma. hello következő táblázatban hello lemezképek minta készlet beállításait:

| Beállítás | Érték |
| ---- | ---- |
| **Képtípus** | Kép: egyéni |
| **Kép: egyéni** | Lemezkép URI hello képernyő`https://yourstorageaccountdisks.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd` |
| **Csomópont ügynök SKU** | Batch.node.ubuntu 16.04 |
| **Csomópont mérete** | NC6 Standard |



## <a name="next-steps"></a>Következő lépések

* toorun MPI-feladatok az Azure Batch-készlet, lásd: hello [Windows](batch-mpi.md) vagy [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) példák.

* A kötegelt GPU-munkaterhelések, tekintse meg a hello [kötegelt hajógyárnak](https://github.com/Azure/batch-shipyard/) receptet.
