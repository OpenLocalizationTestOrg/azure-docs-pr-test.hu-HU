---
title: "mentése a Windows RDMA fürt toorun MPI alkalmazások aaaSet |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate mérete H16r, H16mr, A8 és A9 virtuális gépek toouse Windows HPC Pack fürt hello Azure RDMA hálózati toorun MPI alkalmazásokat."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 7d9f5bc8-012f-48dd-b290-db81c7592215
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 23bc8740dbd05a7c7ab3f998489a41d0df4520a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-toorun-mpi-applications"></a>HPC Pack toorun MPI alkalmazásokkal Windows RDMA fürt beállítása
Az Azure-ban Windows RDMA fürt beállítása [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) és [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun párhuzamos Message Passing Interface (MPI) alkalmazások. Az RDMA-kompatibilis, a Windows Server-alapú HPC Pack-fürtben lévő csomópontok beállításakor MPI alkalmazások hatékonyan kis késéssel, a távoli közvetlen memória-hozzáférés (RDMA) technológia alapuló Azure magas teljesítmény hálózati protokollt használó kommunikációra.

Ha azt szeretné, hogy hozzáférési hello Azure RDMA hálózati Linux virtuális gépeken toorun MPI terhelések, lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](../../linux/classic/rdma-cluster.md).

## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack fürt üzembe helyezési lehetőségei
A Microsoft HPC Pack egy olyan eszköz, feltéve, nem jelent többletköltséget toocreate HPC-fürtök a helyszíni vagy Azure toorun Windows vagy Linux HPC-alkalmazásokhoz. HPC Pack hello Microsoft általi implementációja hello üzenet átadásakor felület for Windows (MS-MPI) futtatókörnyezetének tartalmazza. Ha használja az RDMA-kompatibilis osztályt egy támogatott Windows Server operációs rendszert futtató, HPC Pack lehetővé teszi egy hatékony beállítás toorun Windows MPI, hogy hozzáférési hello Azure RDMA hálózati. 

Ez a cikk két forgatókönyv bemutatja, és hivatkozásokat tartalmaz a Microsoft HPC Pack Windows RDMA fürt toodetailed útmutatást tooset. 

* 1. forgatókönyv. Számítási igényű feldolgozói szerepkör-példányok (PaaS) telepítése
* 2. forgatókönyv. Telepítse a számítási igényű VMs (IaaS) számítási csomópontokat

Általános Előfeltételek toouse számítási igényű példányok Windows, lásd: [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>1. forgatókönyv: Központi telepítése a számítási igényű munkavégző szerepkörpéldányokat (PaaS)
HPC Pack meglévő fürtök adjon hozzá további számítási erőforrásokat Azure feldolgozói szerepkör példányát (Azure-csomópontok) (PaaS) felhőszolgáltatásban fut. Ez a szolgáltatás, más néven "kapacitásnövelés tooAzure" HPC Pack számos különböző méretű hello szerepkör feldolgozópéldányok támogatja. Ha Azure-csomópontok hozzáadása hello, adjon meg egy RDMA-kompatibilis hello méretű.

Az alábbiakban szempontjait és lépéseit tooburst tooRDMA-kompatibilis Azure-példányokon a meglévő (jellemzően helyszíni) fürthöz. Hasonló eljárások tooadd feldolgozói szerepkör példányok tooan HPC Pack átjárócsomópont, amely telepítve van egy Azure virtuális gép használja.

> [!NOTE]
> A HPC Pack oktatóanyag tooburst tooAzure, lásd: [HPC Pack hibrid fürt beállítása](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). A lépéseket, amelyek érvényesek a kifejezetten tooRDMA-kompatibilis Azure-csomópontok hello hello-érdemes figyelembe venni.
> 
> 

![Kapacitásnövelés tooAzure][burst]

### <a name="steps"></a>Lépések
1. **Telepíthet és konfigurálhat egy átjáró HPC Pack 2012 R2-csomóponti**
   
    Hello legújabb HPC Pack telepítési csomag letöltését hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Követelmények és utasításokat tooprepare egy Azure-kapacitásnövelés a telepítést, lásd: [tooAzure Worker-példány és a Microsoft HPC Pack kapacitásnövelés](https://technet.microsoft.com/library/gg481749.aspx).
2. **Felügyeleti tanúsítvány konfigurálása az Azure-előfizetés hello**
   
    Egy tanúsítvány-toosecure hello hello átjárócsomópont és az Azure közötti kapcsolat konfigurálása. Beállítások és eljárások: [forgatókönyvek tooConfigure hello Azure felügyeleti tanúsítvány a HPC Pack](http://technet.microsoft.com/library/gg481759.aspx). Tesztelési célú telepítések esetén a HPC Pack alapértelmezett Microsoft HPC Azure felügyeleti tanúsítvánnyal gyorsan feltöltheti az Azure-előfizetés tooyour telepíti.
3. **Új felhőalapú szolgáltatás és a storage-fiók létrehozása**
   
    Az Azure portál toocreate hello egy felhőalapú szolgáltatás és a storage-fiókok hello telepítéshez használni egy régióban, ahol az RDMA-kompatibilis hello példányok elérhetők.
4. **Egy Azure csomópont-sablon létrehozása**
   
    Hello létrehozása csomópont sablon varázsló használata a HPC Cluster Manager. Útmutató: [hozzon létre egy Azure csomópont sablont](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) az "Azure-csomópontok és a Microsoft HPC Pack lépéseket tooDeploy".
   
    A kezdeti tesztek javasoljuk, hogy manuális rendelkezésre állási házirend beállítása hello sablonban.
5. **Csomópontok toohello fürt hozzáadása**
   
    Hello hozzáadása csomópont varázslóját használja a HPC Cluster Manager. További információkért lásd: [Azure csomópontok hozzáadása toohello Windows HPC-fürt](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).
   
    Hello csomópontok hello méretének megadása esetén válasszon ki egy hello RDMA-kompatibilisek-e példány mérete.
   
   > [!NOTE]
   > Minden egyes kapacitásnövelés tooAzure központi telepítés hello számítási igényű osztályt HPC Pack automatikusan telepíti a legalább két RDMA-kompatibilisek-példányok (például A8) proxy csomópontként, továbbá toohello Azure feldolgozói szerepkör-példányok megadása. hello proxy csomópontok toohello előfizetés foglal le, és költségek mellett hello Azure feldolgozói szerepkör példányok magok használja.
   > 
   > 
6. **Indítsa el a (kiépíteni) hello csomópontok és azok online toorun feladatok**
   
    Jelöljön ki hello csomópontokat, és hello **Start** HPC-Fürtkezelőben művelet. Ha kiépítése befejeződött, válasszon hello csomópontot, majd használja a hello **online állapotba hozás** HPC-Fürtkezelőben művelet. hello csomópontok készen toorun feladatok.
7. **Küldje el a feladatok toohello fürt**
   
   HPC Pack feladat elküldése eszközök toorun fürt feladatok használja. Lásd: [Microsoft HPC Pack: feladatkezelés](http://technet.microsoft.com/library/jj899585.aspx).
8. **Állítsa le a (deprovision) hello csomópontok**
   
   Futó feladat befejezése, hello csomópontok offline állapotba, és használja a hello **leállítása** HPC-Fürtkezelőben művelet.

## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>2. forgatókönyv: Központi telepítése a számítási csomópontok számítási igényű VMs (IaaS)
Ebben a forgatókönyvben telepít hello HPC Pack átjárócsomópont és számítási fürtcsomópontok virtuális gépeken egy Azure virtuális hálózatban. HPC Pack biztosít több [az Azure virtuális gépeken a központi telepítési beállítások](../../linux/hpcpack-cluster-options.md)automatikus telepítési parancsfájl és Azure gyors üzembe helyezési sablonokat is beleértve. Például hello a következő szempontok és lépések útmutató toouse hello [HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md) hello telepítése az Azure-ban HPC Pack 2012 R2 fürt automatizálásához.

![Az Azure virtuális gépeken fürt][iaas]

### <a name="steps"></a>Lépések
1. **Hozzon létre egy fürt átjárócsomópontjába, és a számítási csomópont virtuális gépek egy ügyfélszámítógépen hello HPC Pack IaaS telepítési parancsfájl futtatásával**
   
    Hello HPC Pack IaaS telepítési parancsfájl csomag letöltését hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922).
   
    tooprepare hello ügyfélszámítógép hello parancsfájl konfigurációs fájlja és futtatási hello parancsfájl létrehozása című [HPC-fürt létrehozása hello HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md). 
   
    az RDMA-kompatibilis toodeploy számítási csomópontokat, Megjegyzés hello további szempontokról a következő:
   
   * **Virtuális hálózati**: Adjon meg egy új virtuális hálózatot, mely hello RDMA-kompatibilis példányméretének a régióban szeretné toouse érhető el.
   * **Windows Server operációs rendszer**: toosupport RDMA-kapcsolatot, adjon meg egy Windows Server 2012 R2 vagy Windows Server 2012 operációs rendszert hello számítási csomópont virtuális gépeket.
   * **A felhőalapú szolgáltatások**: javasoljuk, hogy az átjárócsomópont egy felhőalapú szolgáltatás és a másik felhőalapú szolgáltatást a számítási csomópontok telepítésével.
   * **Csomópont méretének HEAD**: A jelen esetben fontolja meg egy méretének legalább A4 (Extra nagy) hello központi csomópont.
   * **HpcVmDrivers bővítmény**: hello telepítési parancsfájl hello Azure Virtuálisgép-ügynök és hello HpcVmDrivers bővítmény automatikusan telepíti mérete A8 és A9 számítási csomópontok a Windows Server operációs rendszer központi telepítésekor. HpcVmDrivers, hogy csatlakozhassanak a toohello RDMA hálózati hello számítási csomópont virtuális gépeket telepít illesztőprogramokat. Az RDMA-kompatibilis H sorozatú virtuális gépeken manuálisan kell telepítenie a hello HpcVmDrivers bővítmény. Lásd: [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   * **Fürthálózat-konfiguráció**: hello telepítési parancsfájl automatikusan beállítja a hello HPC Pack fürt topológia 5 (hello vállalati hálózaton található összes csomópontra). Ez a topológia összes HPC Pack fürtök telepítésének a virtuális gépek szükség. Ne változtassa meg később hello fürt hálózati topológia.
2. **Kapcsolja a számítási csomópontok hello online toorun feladatok**
   
    Jelöljön ki hello csomópontokat, és hello **online állapotba hozás** HPC-Fürtkezelőben művelet. hello csomópontok készen toorun feladatok.
3. **Küldje el a feladatok toohello fürt**
   
    Toohello átjárócsomópont toosubmit feladatok csatlakoztassa, vagy állítson be egy helyi számítógép toodo ez. További információ: [nyújt feladatok tooan HPC cluster az Azure-ban](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
4. **Hajtsa végre a megfelelő hello csomópontok offline és leállíthatja (felszabadítása) őket**
   
    Futó feladatok befejezése után vegye offline hello csomópontok HPC Cluster Manager. Ezután használja az Azure felügyeleti eszközök tooshut le őket.

## <a name="run-mpi-applications-on-hello-cluster"></a>MPI-alkalmazások futtatása hello fürtön
### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Példa: Mpipingpong futtatása HPC Pack fürtökön
egy RDMA-kompatibilis hello példányok futtatása HPC Pack telepítését tooverify hello HPC Pack **mpipingpong** hello fürtön parancsot. **mpipingpong** küld csomagokat párosított csomópontok közötti ismételten toocalculate késést és átviteli mérések és hello RDMA-kompatibilis alkalmazások hálózati statisztikája. Ez a példa bemutatja egy tipikus mintája egy MPI feladat fut (ebben az esetben **mpipingpong**) hello fürttel **mpiexec** parancsot.

Ebben a példában feltételezzük, hogy a "kapacitásnövelés tooAzure" konfigurációs hozzáadott Azure-csomópontok ([1. forgatókönyv](#scenario-1.-deploy-compute-intensive-worker-role-instances-\(PaaS\) in this article). Ha Azure virtuális gépek fürt telepített HPC Pack, akkor lesz toomodify hello parancs szintaxisát toospecify egy másik csomópont csoportot kell, és további környezeti változók toodirect hálózati forgalom toohello RDMA hálózati.

toorun mpipingpong hello fürtön:

1. Hello központi csomóponton, vagy egy megfelelően konfigurált ügyfélszámítógépen nyisson meg egy parancssort.
2. a négy csomópont közül parancs toosubmit egy feladat toorun mpipingpong kis csomagméret és sok a közelítés a következő típus hello egy Azure-kapacitásnövelés telepítési csomópontok közötti késés tooestimate:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```
   
    hello parancs hello feladat elküldött hello Azonosítóját adja vissza.
   
    Ha telepítette az Azure virtuális gépeken telepített hello HPC Pack fürtöt, adjon meg egy csomópont csoportot tartalmazó számítási csomópont virtuális gépek telepítve egyetlen felhőszolgáltatás, és módosítsa a hello **mpiexec** parancsot a következőképpen:
   
    ```Command
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```
3. Hello feladat befejezése után tooview hello kimeneti (ebben az esetben az 1. feladat hello feladat kimenetének hello), a következő típus hello
   
    ```Command
    task view <JobID>.1
    ```
   
    Ha &lt; *JobID* &gt; hello azonosító hello feladat, amely el lett küldve.
   
    hello kimeneti késés eredmények hasonló toohello következőket tartalmazza.
   
    ![Ping pong késés][pingpong1]
4. az Azure közötti tooestimate átviteli kapacitásnövelés csomópontok, típus hello következő parancsot a toosubmit egy feladat toorun **mpipingpong** a csomagok nagy méretű és néhány ismétlési:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```
   
    hello parancs hello feladat elküldött hello Azonosítóját adja vissza.
   
    Egy Azure virtuális gépeken telepített HPC Pack fürt módosítsa a hello parancs, a 2.
5. Hello feladat befejezése után tooview hello kimeneti (ebben az esetben az 1. feladat hello feladat kimenetének hello), a következő típus hello:
   
    ```Command
    task view <JobID>.1
    ```
   
   hello kimeneti átviteli eredmények hasonló toohello következőket tartalmazza.
   
   ![Ping pong átviteli sebesség][pingpong2]

### <a name="mpi-application-considerations"></a>MPI alkalmazás kapcsolatos szempontok
Az alábbiakban szempontok az Azure-ban HPC Pack MPI-alkalmazások futtatására. Néhány alkalmazása csak toodeployments Azure csomópontok (szerepkör feldolgozópéldányok "kapacitásnövelés tooAzure" konfigurációban hozzá).

* Szerepkör feldolgozópéldányok felhőszolgáltatásban vannak rendszeresen újra kiépíteni minden külön értesítés nélkül az Azure-ban (például karbantartási, és a példány sikertelen). Egy példány van újra kiépíteni, egy MPI-feladat futtatása, ha a hello példány elveszíti az adatokat, és toohello állapotba tér vissza, ha először telepítették, ami hello MPI feladat toofail okozhat. hello több olyan csomópontot, amely egyetlen MPI feladat használja, és hello már hello feladat fut, hello nagyobb a valószínűsége annak, hogy hello példányok egyik újra van kiépíteni a feladat futása közben. Is megfontolandó szempontok hello telepítésben egyetlen csomópont fájlkiszolgálóként megadásakor.
* toorun MPI-feladatok az Azure-ban, nincs toouse hello RDMA-kompatibilis példányok. HPC Pack által támogatott bármely példányméretének is használhatja. Azonban a hello RDMA-kompatibilis példányok ajánlott, amelyek bizalmas toohello késés és a hello csomópontok csatlakozó hello hálózati sávszélesség hello viszonylag nagy méretű MPI-feladatok futtatásához. Ha használ egyéb méretek toorun és sávszélesség-késésérzékeny MPI-feladatok, ajánlott futó kis feladatok, amelyben fut egy feladat csak néhány csomópont.
* Központilag telepített alkalmazások tooAzure példányok rendszer licencelési időszakonként hello alkalmazáshoz kapcsolódó tulajdonos toohello. Ellenőrizze minden kereskedelmi alkalmazás licenckezelési vagy más korlátozásokat hello felhőben futó hello gyártójánál. Nem minden szállító kínál használatalapú licencet.
* Azure-példányokon további kell beállítani, tooaccess helyszíni csomópontok, megosztásokon és licenckiszolgálókat. Például a tooenable hello Azure-csomópontok tooaccess helyszíni licenckiszolgáló, konfigurálhatja a pont-pont Azure virtuális hálózat.
* Azure-példányokon toorun MPI alkalmazások minden MPI alkalmazás regisztrálása a Windows tűzfal hello példányokon hello futtatásával **hpcfwutil** parancsot. Ez lehetővé teszi a MPI kommunikációs tootake hely hello tűzfal által dinamikusan hozzárendelt porton.
  
  > [!NOTE]
  > Kapacitásnövelés tooAzure telepítések esetén is beállíthatja egy tűzfal kivétel parancs toorun automatikusan minden csomóponton új Azure tooyour fürt hozzáadott. Miután lefuttatta a hello **hpcfwutil** parancsot, és győződjön meg arról, hogy az alkalmazás akkor működik, hello parancs tooa indítási parancsfájl az Azure csomópontok hozzáadása. További információkért lásd: [indítási parancsfájl használata Azure csomópontok](https://technet.microsoft.com/library/jj899632.aspx).
  > 
  > 
* HPC Pack hello CCP_MPI_NETMASK fürt környezeti változó toospecify elfogadható címtartományt MPI kommunikációhoz használ. A HPC Pack 2012 R2-től kezdődően hello CCP_MPI_NETMASK fürt környezeti változó csak MPI kommunikációt érinti a tartományhoz csatlakoztatott számítási fürtcsomópontok közötti (helyi vagy az Azure virtuális gépeken). hello változó kapacitásnövelés tooAzure konfigurációban hozzáadott csomópontok figyelmen kívül hagyja.
* MPI-feladatok futtatása nem Azure-példányokon különböző felhőszolgáltatások (például kapacitásnövelés tooAzure telepítések esetén másik csomópont sablonok vagy Azure virtuális gép számítási csomópontok több felhőszolgáltatásra telepítése) üzembe helyezett. Ha több Azure csomópont-telepítés másik csomópont sablonok kezdődnek, csak egy Azure-csomópontok készlete hello MPI feladatot kell futtatnia.
* Ha Azure-csomópontok tooyour fürt hozzáadása és vetheti online, hello HPC Job Feladatütemező szolgáltatás azonnal megpróbál toostart feladatok hello csomópontján. Ha csak egy részét a számítási feladatok Azure futtathatja, győződjön meg arról, frissítésekor vagy feladat sablonok toodefine milyen típusú futtatható Azure feladat létrehozása. Például, hogy a feladat sablon csak az Azure csomóponton, futtassa az elküldött feladatok hello csomópont csoportok tulajdonság toohello feladat sablon hozzáadása és a AzureNodes hello tooensure szükséges érték. az Azure-csomópontok toocreate egyéni csoportok hello Add-HpcGroup HPC PowerShell-parancsmagot használhatja.

## <a name="next-steps"></a>Következő lépések
* Alternatív toousing HPC Pack, mint a hello Azure Batch szolgáltatás toorun MPI alkalmazások fejlesztéséhez a számítási csomópontok az Azure-készlethez. Lásd: [használata többpéldányos feladatok toorun Message Passing Interface (MPI) alkalmazások az Azure Batch](../../../batch/batch-mpi.md).
* Ha azt szeretné, hogy toorun Linux MPI hello Azure RDMA hálózati, elérhető alkalmazásokat lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](../../linux/classic/rdma-cluster.md).

<!--Image references-->
[burst]:media/hpcpack-rdma-cluster/burst.png
[iaas]:media/hpcpack-rdma-cluster/iaas.png
[pingpong1]:media/hpcpack-rdma-cluster/pingpong1.png
[pingpong2]:media/hpcpack-rdma-cluster/pingpong2.png
