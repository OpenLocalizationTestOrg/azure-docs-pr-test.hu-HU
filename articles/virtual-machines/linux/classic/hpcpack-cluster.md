---
title: "aaaLinux a HPC Pack-fürtben lévő virtuális gépek számítási |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate használja egy HPC Pack fürtön és az Azure-ban a Linux nagy teljesítményű számítástechnikai (HPC) munkaterhelések"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 9ed20d6cd69a6472a00666caf8965e9d022698a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Az Azure-beli HPC Pack-fürtök linuxos számítási csomópontjaival kapcsolatos alapvető tudnivalók
Állítson be egy [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) Azure egy központi fut a Windows Server és több csomópontot tartalmazó fürt számítási csomópontjain futó támogatott Linux-disztribúció. Adatokba beállítások toomove hello Linux csomópontokat és hello Windows átjárócsomópont hello fürt között. Ismerje meg, hogyan toosubmit Linux HPC feladatok toohello fürt.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Magas szinten, hello alábbi ábrán látható hello HPC Pack fürt létrehozása és használata.

![Linux-csomópontok HPC Pack fürt][scenario]

Az egyéb beállítások toorun Linux HPC munkaterhelések az Azure, lásd: [kötegelt és nagy teljesítményű számítástechnikai rendszerek technikai erőforrások](../../../batch/big-compute-resources.md).

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a>HPC Pack-fürt üzembe helyezése a Linux számítási csomópont
Ez a cikk bemutatja két lehetőség közül választhat toodeploy egy HPC Pack fürt Azure Linux számítási csomópontokkal. Mindkét módszer a Windows Server Piactéri rendszerkép HPC Pack toocreate hello átjárócsomópont használja. 

* **Az Azure Resource Manager-sablon** -hello Azure piactér egy sablon, vagy a következő gyorsindítási sablonon hello Közösségtől hello fürt hello Resource Manager üzembe helyezési modellel tooautomate létrehozása. Például hello [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) hello Azure piactér-sablon hoz létre egy teljes HPC Pack fürt infrastruktúra Linux HPC a munkaterhelések.
* **PowerShell parancsfájl** -használata hello [Microsoft HPC Pack IaaS telepítési parancsfájl](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) tooautomate a teljes fürtre történő telepítés hello klasszikus üzembe helyezési modellben. Az Azure PowerShell-parancsfájl HPC Pack VM-lemezkép hello Azure piactér a gyors telepítés használ, és számos átfogó konfigurációs paraméterek toodeploy Linux számítási csomópontok.

További információ a HPC Pack fürt telepítési lehetőségek az Azure-ban: [toocreate lehetőségek és az Azure-ban a Microsoft HPC Pack nagy teljesítményű számítástechnikai rendszerek (HPC) fürt felügyeletére](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="prerequisites"></a>Előfeltételek
* **Azure-előfizetés** -előfizetés vagy hello Azure globális vagy Azure Kína szolgáltatás használhatja. Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) néhány percig.
* **Magok kvóta** -szükség lehet tooincrease hello beállított kvótát mag, különösen akkor, ha úgy dönt, toodeploy multicore Virtuálisgép-méretek a több fürtcsomóponton. tooincrease kvóta, nyissa meg az online támogatás ügyfélkérés díjmentesen.
* **Linux-disztribúció** -jelenleg HPC Pack támogatja a következő Linux terjesztésekről, a számítási csomópontok hello. Ezek terjesztéseket piactér verzióját használják, ahol az rendelkezésre áll, vagy adja meg a saját.
  
  * **CentOS-alapú**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1-es HPC
  * **Red Hat Enterprise Linux**: 6.7, 6.8, 7.2
  * **SUSE Linux Enterprise Server**: SLES 12, a SLES 12 (prémium), a SLES 12 SP1, a SLES 12 SP1 (prémium) SLES 12 a HPC, SLES 12 a HPC (prémium)
  * **Ubuntu Server**: 14.04 LTS, 16.04 LTS
    
    > [!TIP]
    > toouse hello Azure RDMA hálózati egy Virtuálisgép-méretek hello RDMA-kompatibilisek-e, adjon meg a hello Azure Piactérről származó SUSE Linux Enterprise Server 12 HPC vagy CentOS-alapú HPC-lemezképet. További információkért lásd: [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

További Előfeltételek toodeploy hello fürt hello HPC Pack IaaS telepítési parancsfájl használatával:

* **Ügyfélszámítógép** -ügyfél Windows-alapú számítógép toorun hello fürt telepítési parancsfájl van szüksége.
* **Az Azure PowerShell** - [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) (0.8.10 verzió vagy újabb) az ügyfélszámítógépen.
* **HPC Pack IaaS telepítési parancsfájl** - töltse le és csomagolja ki a legújabb verziójának hello hello hello parancsfájlt [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Hello verziója hello parancsfájl futtatásával ellenőrizheti `.\New-HPCIaaSCluster.ps1 –Version`. Ez a cikk 4.4.1 verzió vagy újabb hello parancsfájl alapul.

### <a name="deployment-option-1-use-a-resource-manager-template"></a>1. telepítési lehetőséget. A Resource Manager-sablon használata
1. Nyissa meg toohello [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) sablon hello Azure piactéren, és kattintson **telepítés**.
2. Az Azure-portálon hello, hello tekintse át, és kattintson a **létrehozása**.
   
    ![Portál létrehozásához][portal]
3. A hello **alapjai** panelen adjon meg egy nevet hello fürtté is a hello átjárócsomópont virtuális gép nevét. Válasszon egy meglévő erőforráscsoportot, vagy olyan helyre, amely elérhető tooyou csoport hello központi telepítés létrehozása. hello hely befolyásolja hello rendelkezésre állását, valamint az egyes Virtuálisgép-méretek más Azure-szolgáltatásokkal (lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/)).
4. A hello **csomópont beállítások Head** panelen első üzembe helyezés esetén általában elfogadhatja hello alapértelmezett beállításokat. 
   
   > [!NOTE]
   > Hello **utáni konfigurációs parancsprogram URL-Címének** van egy nem kötelező beállítás toospecify egy nyilvánosan elérhető Windows PowerShell-parancsfájlt, amelyet a hello átjárócsomópont VM toorun után fut-e. 
   > 
   > 
5. A hello **számítási csomópont beállítások** panelen hello csomópontok, hello száma és mérete hello csomópontok egy elnevezési mintát választ, és Linux terjesztési toodeploy hello.
6. A hello **infrastruktúrabeállításai** panelen írja be a hello virtuális hálózat és az Active Directory tartomány, a tartomány és a virtuális gép rendszergazdai hitelesítő adatokat és a egy elnevezési mintája hello storage-fiókok.
   
   > [!NOTE]
   > HPC Pack hello Active Directory tartományi tooauthenticate fürt felhasználók használja. 
   > 
   > 
7. Miután hello Érvényesítési tesztek futtatásához, és áttekintheti a hello használati feltételeket, kattintson **beszerzési**.

### <a name="deployment-option-2-use-hello-iaas-deployment-script"></a>Telepítési beállítás 2. Hello IaaS telepítési parancsfájl használata
Az alábbiakban további Előfeltételek toodeploy hello fürt hello HPC Pack IaaS telepítési parancsfájl használatával:

* **Ügyfélszámítógép** -ügyfél Windows-alapú számítógép toorun hello fürt telepítési parancsfájl van szüksége.
* **Az Azure PowerShell** - [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) (0.8.10 verzió vagy újabb) az ügyfélszámítógépen.
* **HPC Pack IaaS telepítési parancsfájl** - töltse le és csomagolja ki a legújabb verziójának hello hello hello parancsfájlt [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Hello verziója hello parancsfájl futtatásával ellenőrizheti `.\New-HPCIaaSCluster.ps1 –Version`. Ez a cikk 4.4.1 verzió vagy újabb hello parancsfájl alapul.

**Konfigurációs XML-fájl**

hello HPC Pack IaaS telepítési parancsfájl egy konfigurációs XML-fájl bemeneti toodescribe hello HPC-fürt használja. hello következő minta konfigurációs fájlt adja meg egy HPC Pack átjárócsomópont és a számítási csomópontok két mérete A7 CentOS 7.0 Linux kisebb fürt. 

A környezet és a kívánt fürtkonfigurációt szükség szerint módosítsa a hello fájlt, és mentse a neve, pl. HPCDemoConfig.xml. Például toosupply kell, az előfizetés nevét és egy egyedi tárfiók neve vagy a felhőszolgáltatás neve. Emellett érdemes lehet egy másik toochoose támogatott Linux kép hello számítási csomópontok. Hello elemek hello konfigurációs fájlban kapcsolatos további információkért lásd: hello Manual.rtf fájlban hello parancsfájl és [HPC-fürt létrehozása hello HPC Pack IaaS telepítési parancsfájl](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

**toorun hello HPC Pack IaaS telepítési parancsfájl**

1. Nyissa meg a Windows PowerShell hello ügyfélszámítógépen rendszergazdaként.
2. Változás directory toohello mappát, ahol hello parancsfájl telepítve (E:\IaaSClusterScript ebben a példában).
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. Futtassa a következő parancs toodeploy hello HPC Pack fürt hello. Ebben a példában feltételezzük, hogy hello a konfigurációs fájl található E:\HPCDemoConfig.xml
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    a. Mivel hello **AdminPassword** nincs megadva hello megelőző parancs, a felhasználó jelszava felszólító tooenter hello *MyAdminName*.
   
    b. hello parancsfájl ezután elindítja a toovalidate hello konfigurációs fájlt. Attól függően, hogy a hálózati kapcsolat hello tooseveral percig is eltarthat.
   
    ![Ellenőrzés][validate]
   
    c. Érvényesítést átadni, miután hello parancsfájl hello fürt erőforrások toocreate sorolja fel. Adja meg *Y* toocontinue.
   
    ![Erőforrások][resources]
   
    d. hello parancsfájl toodeploy hello HPC Pack fürt elindul, és további manuális lépések nélkül hello konfigurálása befejeződött. hello parancsfájlt néhány percig.
   
    ![Üzembe helyezés][deploy]
   
   > [!NOTE]
   > Ebben a példában hello parancsfájl naplófájlt állítja elő automatikusan óta hello **- LogFile** paraméter nincs megadva. hello naplók valós időben nem írt, de összegyűjtött hello érvényesítési és hello telepítési hello végén. Ha hello PowerShell folyamat hello parancsfájl futása közben leállt, néhány naplók elvesznek.
   > 
   > 

## <a name="connect-toohello-head-node"></a>Csatlakozás toohello átjárócsomópont
Az Azure hello HPC Pack fürt telepítése után [csatlakozzon a távoli asztal](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello átjárócsomópont VM használatával hello hello fürt telepítésekor megadott tartományi hitelesítő adatok (például *hpc\\ clusteradmin*). Hello átjárócsomópont származó hello fürt kezelése.

Indítsa el a HPC Fürtkezelő toocheck hello állapot hello HPC Pack fürt hello központi csomópontján. Kezelheti és figyelheti a Linux számítási csomópontok hello használata Windows ugyanúgy számítási csomópontjain. Például lásd: hello Linux csomópontok felsorolt **erőforrás-kezelés** (ezek a csomópontok telepített hello **LinuxNode** sablon).

![Csomópont-felügyelet][management]

Hello Linux csomópontjának hello is látni **Hőtérkép** nézet.

![Hőtérkép][heatmap]

## <a name="how-toomove-data-in-a-cluster-with-linux-nodes"></a>Hogyan toomove adatok Linux csomópontot tartalmazó fürtben
Számos választási lehetőségek toomove adatokat Linux csomópontokat és hello Windows átjárócsomópont hello fürt között van. Az alábbiakban a három általános módszer, részletesebben a következő részekben hello:

* **Az Azure File** -egy felügyelt SMB toostore fájlmegosztásadatokra-fájlok az Azure storage tesz elérhetővé. Windows-csomópontok és a Linux-csomópontok is csatlakoztathatja egy Azure fájlmegosztás olyan meghajtó vagy a mappát a hello azonos időben, még akkor is, ha azok a különböző virtuális hálózatokon van telepítve.
* **Átjárócsomópont SMB-fájlmegosztás** -csatlakoztatja a szabványos Windows megosztott mappába hello átjárócsomópont Linux csomópontján.
* **HEAD csomópont NFS-kiszolgáló** -fájlmegosztási megoldást kínál a Windows és Linux használnak.

### <a name="azure-file-storage"></a>Az Azure File storage
Hello [Azure File](https://azure.microsoft.com/services/storage/files/) szolgáltatás elérhetővé teszi a fájlmegosztások hello standard SMB 2.1-es protokoll használatával. Az Azure virtuális gépek és felhőszolgáltatások megosztott csatlakoztatott megosztásokon keresztül alkalmazások összetevői között, és a helyszíni alkalmazások férhetnek hozzá olyan megosztáson található fájladatokat hello fájl storage API-JÁN keresztül. 

Egy Azure-fájl megosztása, és csatlakoztassa azt hello átjárócsomópont részletes lépéseket toocreate, lásd: [Ismerkedés az Azure File storage on Windows](../../../storage/files/storage-how-to-use-files-windows.md). toomount hello Azure fájlmegosztás hello Linux csomópont, lásd: [hogyan toouse Linux Azure File storage](../../../storage/files/storage-how-to-use-files-linux.md). tooset létre tárolásakor kapcsolatokat, lásd: [Persisting kapcsolatok tooMicrosoft Azure fájlok](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

A következő példa hello Azure-fájlmegosztás létrehozása a storage-fiók. toomount hello megosztása csomóponton hello központi, nyissa meg egy parancssort, és írja be a következő parancsok hello:

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

Ebben a példában allvhdsje a tárfiók nevére, storageaccountkey a tárfiók kulcsára, pedig rdma hello Azure fájlmegosztás neve. hello Azure fájlmegosztás Z:, hello átjárócsomópontjához csatlakozik.

toomount hello Azure fájlmegosztás Linux csomópont, futtassa a **clusrun** hello átjárócsomópont parancs. **[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  van egy hasznos HPC Pack eszköz toocarry több csomóponton felügyeleti feladatokat. (Lásd még: [Clusrun Linux csomópontok](#Clusrun-for-Linux-nodes) ebben a cikkben.)

Nyisson meg egy Windows PowerShell-ablakot, és írja be a következő parancsok hello:

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /rdma nevű mappát. hello második parancs hello Azure File megosztás allvhdsjw.file.core.windows.net/rdma hello /rdma mappa dir és fájl mód bits set too777 alakzatot csatlakoztatja. A második parancs hello allvhdsje a tárfiók neve pedig storageaccountkey a tárfiók kulcsára.

> [!NOTE]
> hello "\`" hello második parancs a szimbólum értéke egy escape szimbólum PowerShell. "\`,"azt jelenti, hogy hello"," (vessző karakter) hello parancs részét képezi.
> 
> 

### <a name="head-node-share"></a>Átjárócsomópont megosztás
Azt is megteheti csatlakoztatása Linux csomópontok hello átjárócsomópont megosztott mappába. A megosztás hello legegyszerűbb módja tooshare fájlokat, de az hello átjárócsomópont és az összes Linux csomópontra hello kell telepíteni ugyanazt a virtuális hálózatot. Az alábbiakban hello lépéseket.

1. Hozzon létre egy mappát a hello átjárócsomópont, és megoszthatják tooEveryone az olvasási/írási engedéllyel. Például megosztás D:\OpenFOAM hello átjárócsomópont, \\CentOS7RDMA-HN\OpenFOAM. A HN CentOS7RDMA ez hello átjárócsomópont hello állomásnevét.
   
    ![Fájlmegosztási engedélyek][fileshareperms]
   
    ![Fájlmegosztás][filesharing]
2. Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsok hello:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /openfoam nevű mappát. hello második parancs csatlakoztatja hello megosztott mappa //CentOS7RDMA-HN/OpenFOAM alakzatot dir és fájl mód bits set too777 hello mappában. hello hello parancs levő felhasználónevet és jelszót kell hello felhasználónevét és jelszavát hello átjárócsomópont fürt felhasználója. (Lásd: [hozzáadását vagy eltávolítását fürt felhasználók](https://technet.microsoft.com/library/ff919330.aspx).)

> [!NOTE]
> hello "\`" hello második parancs a szimbólum értéke egy escape szimbólum PowerShell. "\`,"azt jelenti, hogy hello"," (vessző karakter) hello parancs részét képezi.
> 
> 

### <a name="nfs-server"></a>NFS-kiszolgáló
hello NFS szolgáltatás lehetővé teszi a tooshare, és telepítse át a fájlok hello SMB protokollt használó hello Windows Server 2012 operációs rendszert futtató és hello NFS-protokollt használó Linux-alapú számítógépek között. hello NFS-kiszolgáló és az összes csomóponton rendelkezik telepített hello toobe ugyanazt a virtuális hálózatot. Linux-csomópontokkal SMB-megosztáson képest jobb kompatibilitást biztosít. Például fájlhivatkozások támogatja.

1. tooinstall, és állítsa be az NFS-kiszolgáló kövesse hello [Server, a rendszer első Fájlmegosztására-végpontok](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).
   
    Hozzon létre például egy NFS-megosztás nfs nevű hello következő tulajdonságai:
   
    ![NFS-engedélyezés][nfsauth]
   
    ![NFS-megosztás engedélyeinek][nfsshare]
   
    ![NFS-NTFS-engedélyek][nfsperm]
   
    ![NFS-felügyeleti tulajdonságai][nfsmanage]
2. Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsok hello:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /nfsshared nevű mappát. hello második parancs csatlakoztatások hello NFS megosztása CentOS7RDMA-HN: / alakzatot nfs hello mappát. Itt CentOS7RDMA-HN: / nfs hello távoli elérési útja az NFS-megosztások.

## <a name="how-toosubmit-jobs"></a>Hogyan toosubmit feladatok
Van több módon toosubmit feladatok toohello HPC Pack fürt:

* HPC Fürtkezelő vagy a HPC Job Manager eszköz grafikus felhasználói felületen
* HPC-webportál
* REST API

HPC Pack grafikus eszközöket és hello HPC-webportál az Azure-ban toohello fürt vannak hello ugyanaz, mint Windows számítási csomópontok feladat elküldése. Lásd: [HPC Pack Feladatkezelő](https://technet.microsoft.com/library/ff919691.aspx) és [hogyan toosubmit feladatokat a helyszíni ügyfél-számítógépről](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

toosubmit feladatok keresztül hello REST API-t, tekintse meg a túl[létrehozása és elküldése a feladatokat a szerint hello REST API-t a Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). a Linux ügyfélben toosubmit feladatok is tekintse meg a toohello Python minta hello [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).

## <a name="clusrun-for-linux-nodes"></a>Linux-csomópontok Clusrun
HPC Pack hello [clusrun](https://technet.microsoft.com/library/cc947685.aspx) használt tooexecute parancsok Linux csomópontokon keresztül, a parancssor vagy a HPC Cluster Manager eszköz lehet. A következő példák alapvető.

* Az aktuális felhasználónevek megjelenítése hello fürt összes csomópontján.
  
    ```command
    clusrun whoami
    ```
* Hello telepítése **gdb** hibakereső eszközt a **yum** az összes csomóponton hello linuxnodes csoportban, és indítsa újra hello csomópontok 10 perc múlva.
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* Hozzon létre egy héjparancsfájlt, minden egyes számának 1 és 10 minden egyes Linux csomóponton egy második hello fürt megjelenítése, majd futtassa és hello csomópontok eredményének megjelenítése azonnal.
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> Toouse szükség lehet egyes escape-karakter **clusrun** parancsok. Ebben a példában látható, használjon ^ egy parancssor tooescape hello a ">" szimbólumot.
> 
> 

## <a name="next-steps"></a>Következő lépések
* Próbálkozzon a hello fürt tooa több csomópontot vertikális felskálázásával, vagy a Linux-munkaterhelések futtatásával hello fürtön. Egy vonatkozó példáért lásd: [NAMD futtassa a Microsoft HPC Pack Linux számítási csomópontok az Azure-ban](hpcpack-cluster-namd.md).
* Próbálja meg a fürt [RDMA-kompatibilisek-e, a számítási igényű virtuális gépek](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun MPI munkaterhelések. Egy vonatkozó példáért lásd: [OpenFOAM futtassa a Microsoft HPC Pack egy Linux RDMA a fürtön, az Azure-ban](hpcpack-cluster-openfoam.md).
* Ha érdekli a helyszíni HPC Pack fürtben lévő Linux-csomópont működik, tekintse meg a hello [TechNet útmutatást](https://technet.microsoft.com/library/mt595803.aspx).

<!--Image references-->
[scenario]:media/hpcpack-cluster/scenario.png
[portal]:media/hpcpack-cluster/portal.png
[validate]:media/hpcpack-cluster/validate.png
[resources]:media/hpcpack-cluster/resources.png
[deploy]:media/hpcpack-cluster/deploy.png
[management]:media/hpcpack-cluster/management.png
[heatmap]:media/hpcpack-cluster/heatmap.png
[fileshareperms]:media/hpcpack-cluster/fileshare1.png
[filesharing]:media/hpcpack-cluster/fileshare2.png
[nfsauth]:media/hpcpack-cluster/nfsauth.png
[nfsshare]:media/hpcpack-cluster/nfsshare.png
[nfsperm]:media/hpcpack-cluster/nfsperm.png
[nfsmanage]:media/hpcpack-cluster/nfsmanage.png
