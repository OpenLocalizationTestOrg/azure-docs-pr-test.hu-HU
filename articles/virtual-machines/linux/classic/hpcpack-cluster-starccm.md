---
title: "aaaRun csillag-CCM + HPC Pack Linux virtuális gépeken |} Microsoft Docs"
description: "Az Azure a Microsoft HPC Pack fürt központi telepítése és futtatása egy csillag-feladat CCM + több Linux a számítási csomópontok az RDMA-hálózaton."
services: virtual-machines-linux
documentationcenter: 
author: xpillons
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 75523406-d268-4623-ac3e-811c7b74de4b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 09/13/2016
ms.author: xpillons
ms.openlocfilehash: 8265013cb295f53d6d4354ab2f100ef20d9f4c8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Futtassa a csillag-CCM + Microsoft HPC Pack egy Linux RDMA a fürt az Azure-ban
Ez a cikk bemutatja, hogyan toodeploy a Microsoft HPC Pack fürtön Azure, és futtassa a [CD-adapco csillag-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) feladat több Linux számítási csomópontokra, amelyeket kötik össze, az InfiniBand.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

A Microsoft HPC Pack számos szolgáltatások toorun nagyméretű HPC és párhuzamos alkalmazások, beleértve a MPI alkalmazásokat, a Microsoft Azure virtuális gépek fürtjein. HPC Pack a telepített HPC Pack-fürtben lévő Linux számítási csomópont virtuális gépeken futó Linux HPC-alkalmazásokhoz is támogatja. Egy bevezető toousing Linux számítási csomópontok HPC Pack, lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Egy HPC Pack fürt beállítása
Hello HPC Pack IaaS üzembe helyezési parancsfájlok letöltését hello [letöltőközpontból](https://www.microsoft.com/en-us/download/details.aspx?id=44949) és helyileg bontsa ki.

Az Azure PowerShell feltétele. Ha a PowerShell nincs konfigurálva a helyi gépén, olvassa el a hello cikk [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

Írásának hello időpontban hello Linux képek hello Azure piactér (tartalmazó hello InfiniBand illesztőprogramokat az Azure-bA) a SLES 12 rendszert, a CentOS 6.5-ös és a CentOS 7.1 vannak. Ez a cikk az SLES 12 hello használata alapul. tooretrieve hello nevét, amely támogatja az HPC hello piactér minden Linux-lemezképek, a következő PowerShell-paranccsal hello is futtathatja:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

hello kimeneti tartalmazza ezeket a lemezképeket érhetők el és lemezképnév hello hello helye (**ImageName**) toobe később hello központi telepítési sablont használni.

Hello fürt központi telepítése érdekében, hogy toobuild egy HPC Pack telepítési sablon fájlt. Mivel azt még célzó kis fürt, hello átjárócsomópont hello a tartományvezérlőn lennie, és egy helyi SQL-adatbázist.

hello következő sablon fog átjárócsomópont ilyen egy, nevű XML-fájl létrehozása **MyCluster.xml**, és cserélje le a hello értékének **; előfizetés-azonosító**, **StorageAccount**,  **Hely**, **VMName**, és **szolgáltatásnév** legyen.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Indítsa el a hello átjárócsomópont létrehozási hello PowerShell-parancs futtatásával egy rendszergazda jogú parancssorból:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Too30 20 perc elteltével hello átjárócsomópont készen áll. Lehet csatlakoztatni tooit hello Azure-portálon hello kattintva **Connect** hello virtuális gép ikonja.

Idővel előfordulhat, hogy toofix hello DNS-továbbító. toodo Igen, a DNS-kezelő elindítása.

1. Kattintson a jobb gombbal hello kiszolgálónév a DNS-kezelőben válassza **tulajdonságok**, majd kattintson a hello **továbbítók** lapon.
2. Kattintson a hello **szerkesztése** tooremove továbbítókat gombra, majd a **OK**.
3. Győződjön meg arról, hogy hello **gyökérmutató használatára, ha nincs továbbítók elérhető** jelölőnégyzet be van jelölve, és kattintson a **OK**.

## <a name="set-up-linux-compute-nodes"></a>Linux számítási csomópontok beállítása
Hello Linux számítási csomópontok hello segítségével telepítheti, hogy használja-e toocreate hello átjárócsomópont azonos központi telepítési sablont.

Fájl másolása hello **MyCluster.xml** a helyi számítógép toohello átjárócsomópont, és a frissítés hello **NodeCount** hello számát, amelyet az toodeploy csomópontok kód (< = 20). Lehet, gondos toohave elegendő rendelkezésre álló magot a Azure kvótájának A9 feltünteti az előfizetésében 16 mag fog használni. A8 példányok (8 mag) is A9 helyett használja, ha szeretné toouse hello további virtuális gépek ugyanazt a költségvetést.

Hello átjárócsomópont hello HPC Pack IaaS üzembe helyezési parancsfájlok másolja.

Futtassa a következő Azure PowerShell-parancsok rendszergazda jogú parancssorból hello:

1. Futtatás **Add-AzureAccount** tooconnect tooyour Azure-előfizetés.
2. Ha több előfizetéssel rendelkezik, akkor futtassa **Get-AzureSubscription** toolist őket.
3. Állítsa az alapértelmezett előfizetés hello futtatásával **válasszon-AzureSubscription - SubscriptionName xxxx-alapértelmezett** parancsot.
4. Futtatás **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** toostart Linux számítási csomópontok telepítésével.
   
   ![Átjárócsomópont telepítési művelet:][hndeploy]

Hello HPC Pack Cluster Manager eszköz megnyitásához. Néhány perc múlva a Linux számítási csomópontok rendszeresen megjelennek a listában számítási csomópont. A klasszikus üzembe helyezési mód hello IaaS virtuális gépek jönnek létre egymás után. Ezért ha a csomópontok számát hello fontos, rávenni a felhasználókat az összes telepített is igénybe vehet jelentős mennyiségű időt.

![Linux-csomópontok HPC Pack-Fürtkezelőben][clustermanager]

Most, hogy minden csomópont működik, és hello fürtben, nincsenek további infrastruktúra beállítások toomake.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Egy Azure fájlmegosztás Windows és Linux csomópontok beállítása
Hello Azure File service toostore parancsfájlok, alkalmazásokat és az adatfájlok is használhatja. Az Azure File állandó tároló Azure Blob storage-CIFS képességeket biztosít. Vegye figyelembe, hogy ez nem hello legjobban méretezhető megoldás, de hello legegyszerűbb egy és nem igényel dedikált virtuális gépek.

Hozzon létre egy Azure fájlmegosztás hello hello cikk utasításait követve [Ismerkedés az Azure File storage on Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).

Hello neve a tárfiókja, továbbra is **saname**, hello fájlmegosztás neve szerint **megosztásnév**, és hello tárfiók hívóbetűjét, **sakey**.

### <a name="mount-hello-azure-file-share-on-hello-head-node"></a>Hello átjárócsomópont hello Azure fájlmegosztás csatlakoztatása
Nyisson meg egy rendszergazda jogú parancssort, és futtassa a következő parancs toostore hello hitelesítő hello helyi számítógép tárolóban hello:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Ezt követően toomount hello Azure fájlmegosztás, futtassa:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-hello-azure-file-share-on-linux-compute-nodes"></a>A Linux számítási csomópontok hello Azure fájlmegosztás csatlakoztatása
HPC Pack részeként elérhető egy hasznos eszköz egy hello clusrun eszköz. Ez ugyanaz a parancs parancssori eszköz toorun hello egyidejűleg is használhat a számítási csomópontok készlete. Ebben az esetben ez toomount hello Azure fájlmegosztás által használt, és továbbra is fennáll, toosurvive újraindítások.
Egy rendszergazda jogú parancssort a hello átjárócsomópont futtassa a következő parancsok hello.

toocreate hello csatlakoztatási könyvtár:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

toomount hello Azure fájlmegosztás:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

toopersist hello csatlakoztatási megosztás:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Telepítse a csillag-CCM +
Azure Virtuálisgép-példányok A8 és A9 InfiniBand támogatási és RDMA-képességeinek biztosítanak. hello kernel illesztőprogramokat, amelyek lehetővé teszik a szolgáltatásokat a Windows Server 2012 R2, SUSE 12, CentOS 6.5, valamint CentOS 7.1 hello Azure piactér érhetők el. Microsoft MPI és Intel MPI (5.x) olyan hello két MPI könyvtárak, amelyek támogatják a kívánt illesztőprogramokat az Azure-ban.

CD-adapco csillag-CCM + 11.x kiadási és újabb rendszer kötegelt Intel MPI verziójával 5.x, így Azure InfiniBand támogatása is megtalálható.

Első hello Linux64 csillag-hello csomagot CCM + [CD-adapco portal](https://steve.cd-adapco.com). Ebben az esetben vegyes pontosság 11.02.010 verziót használtuk.

A hello átjárócsomópont, hello **/hpcdata** Azure fájl megosztásához hozzon létre egy héjparancsfájlt nevű **setupstarccm.sh** a tartalom a következő hello. Ez a parancsfájl fog futni minden számítási csomópont tooset be csillag a-CCM + helyileg.

#### <a name="sample-setupstarcmsh-script"></a>Mintaparancsfájl setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh tooset up STAR-CCM+ locally

    # Create hello CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy hello STAR-CCM package from hello file share toohello local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract hello package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without hello FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Most, tooset be csillag-CCM + a Linux számítási csomópontok, nyisson meg egy rendszergazda jogú parancssort és futtassa a következő parancs hello:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Hello parancs futása közben, a figyelheti hello CPU-használat hello hőtérkép a fürtkezelő használatával. Néhány perc múlva minden csomópontja megfelelően létre kell hozni.

## <a name="run-star-ccm-jobs"></a>Futtassa a csillag-CCM + feladatok
HPC Pack használt rendelés toorun csillag feladat ütemezési platformképességei-CCM + feladatok. toodo esetben azt kell hello támogatási néhány olyan parancsfájlok, amelyek használt toostart hello feladatot, és futtassa a csillag-CCM +. hello bemeneti adatok másolatok hello Azure fájlmegosztás első az egyszerűség érdekében.

a következő PowerShell-parancsfájl hello használt tooqueue csillag-CCM + feladat. Három argumentummal van:

* hello modell neve
* használt csomópontok toobe hello száma
* a minden egyes csomópont toobe használt magok hello száma

Mivel a csillag-CCM + töltheti hello memória sávszélesség, az általában jobb toouse kevesebb processzormag, egy számítási csomópontok és új csomópontok hozzáadása. hello pontos csomópont magok számát hello processzorcsaládját és hello memóriamodulok közötti sebességétől függ.

hello csomópontok kizárólag hello feladathoz kiosztott, és nem osztható meg más feladatok. hello feladat nem indult el MPI feladatként közvetlenül. Hello **runstarccm.sh** héjparancsfájlt hello MPI indítója elindul.

hello bemeneti modell és hello **runstarccm.sh** parancsfájl tárolódnak hello **/hpcdata** korábban csatlakoztatott megosztást.

Naplófájlok hello feladatazonosítóval vannak nevezve, és vannak tárolva hello **/hpcdata megosztás**, együtt hello csillag-CCM + kimeneti fájlok.

#### <a name="sample-submitstarccmjobps1-script"></a>Mintaparancsfájl SubmitStarccmJob.ps1
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us hello job ID that's used tooidentify hello name of hello uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit hello job     
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Cserélje le **runner.java** az előnyben részesített csillag-CCM + Java modell indítója és naplózási kódot.

#### <a name="sample-runstarccmsh-script"></a>Mintaparancsfájl runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # hello path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set hello mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into hello hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with hello hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

Ebben a tesztben használtuk egy igény szerinti Power licenc token. A token tooset hello rendelkezik **$CDLMD_LICENSE_FILE** környezeti változó túl **1999@flex.cd-adapco.com**  és hello hello kulcs **- podkey** hello parancssor beállítás .

Néhány inicializálás után hello parancsfájl kivonatok – hello **$CCP_NODES_CORES** környezeti változókat, amelyek beállítása HPC Pack – hello csomópontok toobuild egy hostfile, amely MPI indítója hello használja listáját. A hostfile számítási csomópont hello feladat, soronként egy név használt nevek hello listáját tartalmazza.

hello formátuma **$CCP_NODES_CORES** ezt a mintát követi:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Az elemek magyarázata:

* `<Number of nodes>`nem lefoglalt toothis feladat csomópontok hello száma.
* `<Name of node_n_...>`az egyes csomópontok toothis feladat lefoglalt hello név.
* `<Cores of node_n_...>`hello csomóponton toothis feladat lefoglalt magok száma hello van.

magok száma hello (**$NBCORES**) egyben csomópontok száma alapján számított hello (**$NBNODES**) és a csomópont magok számát hello (paraméterként megadott **$NBCORESPERNODE**).

Hello MPI beállítások hello Azure Intel MPI használt állók közül a következők:

* `-mpi intel`Intel MPI toospecify.
* `-fabric UDAPL`toouse Azure InfiniBand műveletek.
* `-cpubind bandwidth,v`a csillag MPI toooptimize sávszélesség-CCM +.
* `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Intel MPI toomake Azure InfiniBand dolgozni, és tooset hello szükséges csomópont magok számát.
* `-batch`toostart csillag-CCM + kötegelt módban nincs felhasználói felületén.

Végezetül toostart egy feladatot, győződjön meg arról, hogy a csomópontok megfelelően működik-e, és a kezelő online állapotban. A PowerShell-parancssorból futtassa ezt:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Csomópont leállítása
Meg Miután befejezte a teszteket, használhatja a következő HPC Pack PowerShell-parancsok toostop hello és indítsa el a csomópontok:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Következő lépések
Próbálja más Linux feladatokat futtatni. Például lásd:

* [A Microsoft HPC Pack NAMD futó Linux számítási csomópontok az Azure-ban](hpcpack-cluster-namd.md)
* [Microsoft HPC Pack OpenFOAM futtassa az Azure Linux RDMA fürtön](hpcpack-cluster-openfoam.md)

<!--Image references-->
[hndeploy]:media/hpcpack-cluster-starccm/hndeploy.png
[clustermanager]:media/hpcpack-cluster-starccm/ClusterManager.png
