---
title: "egy Linux RDMA fürt toorun MPI alkalmazások mentése aaaSet |} Microsoft Docs"
description: "Hello Azure RDMA hálózati toorun MPI alkalmazások mérete H16r, H16mr, A8 és A9 virtuális gépek toouse Linux fürt létrehozására"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.openlocfilehash: 3199317a37b095e80718d6724954687d30aea3a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-linux-rdma-cluster-toorun-mpi-applications"></a>A Linux RDMA fürt toorun MPI alkalmazások beállítása
Ismerje meg, hogyan tooset be egy Linux RDMA fürtön, az Azure-ban [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toorun párhuzamos Message Passing Interface (MPI) alkalmazások. Ez a cikk a Linux HPC kép toorun Intel MPI biztosít lépéseket tooprepare fürtön. Előkészítése, miután a virtuális gépek használata a lemezkép és hello RDMA-kompatibilis Azure Virtuálisgép-méretek (jelenleg H16r, H16mr, A8 és A9) egy fürt központi telepítése. Hello fürt toorun MPI alkalmazásokat használnak a távoli közvetlen memória-hozzáférés (RDMA) technológián alapulnak, alacsony késésű, nagy átviteli hálózati hatékonyan kommunikációhoz.

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus. Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

## <a name="cluster-deployment-options"></a>Fürt üzembe helyezési lehetőségei
Az alábbiakban módszert használhat toocreate Linux RDMA fürt, vagy a Feladatütemező nélkül.

* **Az Azure CLI-parancsfájlok**: a cikk későbbi részében látható, használja a hello [Azure parancssori felület](../../../cli-install-nodejs.md) (CLI) tooscript hello telepítési RDMA-kompatibilisek-e virtuális gépek a fürt. hello CLI szolgáltatásfelügyelet módban hoz létre hello fürtcsomópontok Feladattervek hello klasszikus üzembe helyezési modellel, így sok számítási csomópont telepítése több percig is eltarthat. tooenable hello RDMA hálózati kapcsolatot hello klasszikus üzembe helyezési modellel, használatakor hello virtuális gépek telepítése a hello ugyanaz a felhőalapú szolgáltatás.
* **Az Azure Resource Manager-sablonok**: hello erőforrás-kezelő telepítési modell toodeploy, amely a toohello RDMA hálózati RDMA-kompatibilisek-e virtuális gépek fürtben is használhatja. Is [létrehozhat saját sablont](../../../resource-group-authoring-templates.md), vagy ellenőrizze a hello [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/) Microsoft vagy hello közösségi toodeploy hello megoldást által közzétett sablonokat. Resource Manager-sablonok biztosíthat egy gyors és megbízható módot toodeploy egy Linux-fürt. tooenable hello RDMA hálózati kapcsolatot hello Resource Manager üzembe helyezési modelljével használatakor telepíteni hello hello virtuális gépek azonos rendelkezésre állási csoportot.
* **HPC Pack**: Microsoft HPC Pack fürt létrehozása az Azure-ban, és adja hozzá az RDMA-kompatibilis számítási csomópontokat, amelyek a támogatott Linux terjesztési tooaccess hello RDMA hálózati futnak. További információkért lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-hello-classic-model"></a>Üzembe helyezési minta hello klasszikus modellben lépések
hello következő lépések bemutatják, hogyan toouse hello Azure CLI toodeploy SUSE Linux Enterprise Server (SLES) 12 SP1 HPC virtuális gép hello Azure Piactérről származó testre szabhatja, és hozzon létre egy egyéni Virtuálisgép-lemezkép. Majd hello lemezkép tooscript hello telepítéséhez a fürt RDMA-kompatibilisek-e virtuális gépek is használhatja.

> [!TIP]
> RDMA-kompatibilisek-e virtuális gépek a fürt alapján hello Azure piactér CentOS alapú HPC képek hasonló lépéseket toodeploy használja. Néhány lépést esetekben némileg eltérőek lehetnek. 
>
>

### <a name="prerequisites"></a>Előfeltételek
* **Ügyfélszámítógép**: a Mac, Linux vagy a Windows ügyfél számítógép toocommunicate az Azure-ral van szüksége. Ezek a lépések feltételezik, hogy a Linux-ügyfelet kell használnia.
* **Azure-előfizetés**: Ha nem rendelkezik előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig. Nagyobb fürtök esetén fontolja meg a használatalapú előfizetés vagy egyéb beszerzési lehetőségek.
* **Virtuális gép mérete rendelkezésre állási**: a következő példányok hello RDMA-kompatibilis: H16r, H16mr, A8 és A9. Ellenőrizze [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/) a Azure-régiók rendelkezésre állás érdekében.
* **Magok kvóta**: szükség lehet a számítási igényű virtuális gépek a fürt magok toodeploy tooincrease hello beállított kvótát. Például legalább 128 magok kell, ha azt szeretné, hogy toodeploy 8 A9 VMs ebben a cikkben ismertetett módon. Az előfizetés is korlátozhatja az egyes virtuális gép mérete családok, többek között a hello H-sorozat telepítése magok hello száma. toorequest a kvóta növeléséhez [nyissa meg az online támogatás ügyfélkérés](../../../azure-supportability/how-to-create-azure-support-request.md) díjmentesen.
* **Az Azure CLI**: [telepítése](../../../cli-install-nodejs.md) hello Azure CLI és [csatlakozás Azure-előfizetés tooyour](../../../xplat-cli-connect.md) hello ügyfélszámítógépről.

### <a name="provision-an-sles-12-sp1-hpc-vm"></a>Az SLES 12 SP1 HPC virtuális gép kiépítése
Az Azure CLI hello tooAzure történő bejelentkezés után futtassa `azure config list` , hogy a kimeneti hello tooconfirm jeleníti meg a szolgáltatásfelügyeleti módban. Ha nem, hello mód beállítása a következő parancs futtatásával:

    azure config mode asm


Írja be a következő toolist hello engedélyezett toouse áll előfizetéseket hello:

    azure account list

hello érvényes aktív előfizetéssel, amelynél az `Current` túl beállítása`true`. Ha ez az előfizetés nem hello egy akkor toouse toocreate hello fürt, aktív előfizetéssel hello beállítása hello megfelelő előfizetés-azonosító:

    azure account set <subscription-Id>

toosee hello Azure-ban futtatni egy parancsot például hello a következő, feltéve, hogy a rendszerhéj-környezet támogatja a nyilvánosan elérhető SLES 12 SP1 HPC képek **grep**:

    azure vm image list | grep "suse.*hpc"

SLES 12 SP1 HPC képének az RDMA-kompatibilisek-e virtuális gép kiépítése hello következő parancs futtatásával:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

Az elemek magyarázata:

* hello mérete (ebben a példában A9) egyike Virtuálisgép-méretek hello RDMA-kompatibilisek-e.
* hello külső SSH-portszám (ebben a példában, amely hello SSH alapértelmezett 22) bármilyen érvényes portszámot. hello belső SSH-portszám too22 van beállítva.
* Új felhőalapú szolgáltatás létrejön hello hello hely által megadott Azure-régiót. Adjon meg egy helyet, mely hello virtuális gép méretét a választott érhető el.
* SUSE rangsorolási támogatással (amely további költségeket terhel), a SLES 12 SP1 hello kép neve jelenleg lehet ezek két lehetőség közül: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-hello-vm"></a>Hello virtuális gép testreszabása
Kiépítés hello VM befejezése után a virtuális gép SSH-toohello hello a virtuális gép külső IP-cím (vagy a DNS-név), és hello külső portszám konfigurálva, és ezután testre szabhatja. Kapcsolat részletekért lásd: [hogyan toolog a Linux rendszerű virtuális gép tooa](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Parancsok végrehajtása a virtuális gép, hello konfigurált hello felhasználóként, kivéve, ha legfelső szintű hozzáférés szükséges toocomplete egy lépést.

> [!IMPORTANT]
> A Microsoft Azure nem legfelső szintű hozzáférést biztosítanak tooLinux virtuális gépeket. toogain rendszergazdai hozzáféréssel, a felhasználó toohello VM, futtassa a parancsokat a csatlakozáskor `sudo`.
>
>

* **Frissítések**: frissítések telepítése zypper használatával. Érdemes tooinstall NFS segédprogramok is.

  > [!IMPORTANT]
  > SLES 12 SP1 HPC virtuális gépen azt javasoljuk, hogy a rendszermag-frissítéseket, amelyek problémákat okozhatnak hello Linux RDMA illesztőprogramok nem alkalmazza.
  >
  >
* **Intel MPI**: hello a következő parancs futtatásával végezze el a SLES 12 SP1 HPC VM hello hello Intel MPI telepítését:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* **Memóriát zárolni**: MPI kódok toolock hello számára rendelkezésre álló memória RDMA, hozzáadása vagy módosítása a következő beállításokat a hello /etc/security/limits.conf fájl hello. Legfelső szintű hozzáférés tooedit kell ezt a fájlt.

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > Tesztelési célokra memlock toounlimited is beállíthat. Például: `<User or group name>    hard    memlock unlimited`. További információkért lásd: [beállítás ismert legjobb módszerei zárolt memória mérete](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).
  >
  >
* **SLES virtuális gépek SSH-kulcsok**: készítése SSH kulcsok tooestablish megbízhatósági hello között a felhasználói fiókjához számítási hello SLES fürt csomópontja, MPI-feladatok futtatásakor. Ha telepítette a HPC CentOS-alapú virtuális gépek, ne ezt a lépést. Tudnivalókat később Ez a cikk tooset passwordless SSH megbízhatósági hello fürtcsomópontok között felfelé hello lemezképének és hello fürt központi telepítése után.

    toocreate SSH-kulcsok, futtassa a következő parancs hello. Amikor a bemeneti kéri, válassza ki **Enter** toogenerate hello kulcsok hello alapértelmezett helyen jelszó beállítása nélkül.

        ssh-keygen

    A mellékletfájl hello nyilvános kulcs toohello authorized_keys ismert nyilvános kulcsok.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    Hello ~/.ssh könyvtárban szerkesztése vagy hello konfigurációs fájl létrehozása. Adja meg, hogy tervezi-e az Azure (ebben a példában 10.32.0.0/16) toouse hello hello magánhálózati IP-címtartománya:

        host 10.32.0.*
        StrictHostKeyChecking no

    Azt is megteheti listában hello magánhálózati IP-cím az egyes virtuális gépek a fürt az alábbiak szerint:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > Konfigurálás `StrictHostKeyChecking no` biztonsági kockázatot hozhat létre, ha egy adott IP-cím vagy a tartomány nincs megadva.
  >
  >
* **Alkalmazások**: telepítse a szükséges, vagy más testreszabás is szerepelt végrehajtása előtt hello lemezképének alkalmazásokat.

### <a name="capture-hello-image"></a>Hello lemezképének rögzítése
toocapture hello kép, először futtassa a következő parancsot a Linux virtuális gép hello hello. Ez a parancs hello VM deprovisions, de megőrzi a felhasználói fiókok és a beállított SSH-kulcsok.

```
sudo waagent -deprovision
```

Az ügyfélszámítógépen futtassa az Azure parancssori felület parancsai toocapture hello kép a következő hello. További információkért lásd: [hogyan toocapture képként klasszikus Linuxos virtuális gép](capture-image.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Ezek a parancsok futtatása után hello Virtuálisgép-lemezkép rögzítése a használatra, és hello virtuális gép törlődik. Most, hogy az egyéni lemezkép készen toodeploy egy fürt.

### <a name="deploy-a-cluster-with-hello-image"></a>Hello lemezképpel fürt központi telepítése
A következő Bash parancsfájlok a környezetének megfelelő értékekkel hello módosítása, és futtassa az ügyfélszámítógépről. Azure virtuális hello Feladattervek hello klasszikus üzembe helyezési modellel telepíti, mert toodeploy hello nyolc A9 virtuális gépek ezt a parancsfájlt a javasolt néhány percet vesz igénybe.

```
#!/bin/bash -x
# Script toocreate a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where hello VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All hello compute-intensive instances need toobe in hello same cloud service for Linux RDMA toowork across InfiniBand.
# Note: hello current maximum number of VMs in a cloud service is 50. If you need tooprovision more than 50 VMs in hello same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want tooturn off external ports and use only internal ports toocommunicate between compute nodes via port 22, don’t use this option. Since port numbers up too10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 toocluster18. Specify your captured image in <image-name>. Specify hello username and password you used when creating hello SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment tooprovision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>A CentOS HPC-fürt szempontjai
Ha azt szeretné, hogy a HPC alapján egy hello CentOS alapú HPC lemezképet hello Azure piactér SLES 12 helyett a fürt tooset, kövesse a szakasz megelőző hello hello általános lépéseit. Megjegyzés: hello kiépítése és virtuális gép hello konfigurálása során a következő különbségek:

- Intel MPI már telepítve van a virtuális gép kiépítése a CentOS-alapú HPC-lemezkép.
- Zárolási memóriabeállításait már kerülnek hello VM /etc/security/limits.conf fájlban.
- SSH-kulcsok a virtuális gép kiépítése hello rögzítési nem hoznak létre. Ajánlja felhasználói hitelesítés beállítása hello fürt telepítése után. További információkért tekintse meg a következő szakasz hello.  

### <a name="set-up-passwordless-ssh-trust-on-hello-cluster"></a>Hello fürt passwordless SSH megbízhatóságának beállítása
A CentOS-alapú HPC-fürtben lévő hello számítási csomópontok közötti megbízhatósági kapcsolat létrehozásához két módszer áll rendelkezésre: állomás alapú hitelesítés és a felhasználó-alapú hitelesítés. Gazdagép-alapú hitelesítés Ez a cikk hello hatókörén kívül esik, és általában kell elvégezni egy bővítmény parancsfájl központi telepítése során. Felhasználó-alapú hitelesítés előnyei a megbízható kapcsolat kialakítása a telepítés után, és megköveteli a hello generációs és az SSH-kulcsok között hello megosztásának számítási hello fürt csomópontja. Ezt a módszert gyakran nevezik passwordless SSH-bejelentkezéskor, és futó MPI-feladatok esetén szükséges.

Egy minta parancsfájlt hello Közösségtől hozzájárult érhető el a [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) tooenable könnyen felhasználóhitelesítés CentOS alapú HPC-fürt. Töltse le és használja ezt a parancsfájlt a lépéseket követve hello segítségével. Módosítsa ezt a parancsfájlt vagy bármely más módszer tooestablish passwordless SSH hitelesítés hello számítási fürtcsomópontok között is.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

toorun hello parancsfájl, az alhálózati IP-címekhez szüksége tooknow hello előtag. Hello előtagja lekéréséhez futtassa a parancsot követő hello fürtcsomópontok egyik hello. A kimeneti hasonlóan kell kinéznie 10.1.3.5, és hello előtag hello 10.1.3 részét.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Most futtassa a hello parancsfájl segítségével történő három paramétert: hello közös felhasználónév hello a számítási csomópontok, hello közös jelszót hello felhasználóhoz tartozó számítási csomópontokat, és hello alhálózati előtag, amelyek adott vissza hello előző parancsot.

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Ez a parancsfájl hello a következő:

* Létrehoz egy könyvtárat nevű .ssh, ami azonban szükséges az passwordless bejelentkezési hello állomás csomóponton.
* Mappában hozza létre a konfigurációs fájl hello .ssh, amely arra utasítja a passwordless bejelentkezési tooallow bejelentkezési hello fürt bármely csomópontján.
* Hello csomópont nevét és IP-címek csomópont hello fürt összes csomópontján hello tartalmazó fájlokat hozza létre. Ezek a fájlok megmaradnak a későbbi felhasználás hello parancsfájl futtatása után.
* A privát és nyilvános kulcsból álló kulcspárt a fürt minden csomópontján (beleértve a hello gazdacsomópont) hoz létre, és létrehozza a bejegyzéseket hello authorized_keys fájlba.

> [!WARNING]
> A parancsfájl futtatása, biztonsági kockázatot is létrehozhat. Győződjön meg arról, hogy a nyilvános kulcs információja hello ~/.ssh nem terjesztése.
>
>

## <a name="configure-intel-mpi"></a>Intel MPI konfigurálása
tooconfigure szüksége toorun MPI alkalmazások Azure Linux RDMA, bizonyos környezeti változók adott tooIntel MPI. Íme egy minta Bash parancsfájlok tooconfigure hello szükséges változók toorun kérelmet. Hello elérési toompivars.sh Intel MPI-példány szükség esetén módosítása

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting hello variable tooshm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line toorun hello job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path toohello application exe> <arguments specific toohello application>

#end
```

hello hello állomás fájl formátuma a következő. Adja hozzá az egyes csomópontok egy sort a fürtön. Adja meg a korábban, nem a DNS-nevek meghatározott privát IP-címek hello virtuális hálózatról. Két gazdagépek 10.32.0.1 és 10.32.0.2 IP-címekkel rendelkező, például hello fájl hello következőket tartalmazza:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>MPI futtatnak egy alapszintű két csomópontot tartalmazó fürtben
Ha még nem tette meg, először hello környezet beállítása az Intel MPI.

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>Egy MPI parancs futtatása
Parancsot egy MPI hello számítási csomópontok tooshow MPI megfelelően van telepítve, és képes kommunikálni valamelyik legalább két számítási csomópontjai között. hello következő **mpirun** parancs futtatása hello **állomásnév** két csomópont parancs.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
A kimenetében hello csomópontjaihoz bemenetként továbbított hello nevei `-hosts`. Például egy **mpirun** két csomóponttal rendelkező parancs kimenetét hasonló hello adja vissza:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Egy MPI teljesítményteszt futtatása
a következő Intel MPI parancs hello egy pingpong referenciaalap tooverify hello fürt konfigurációs és a kapcsolat toohello RDMA hálózati fut.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Működő rendelkező fürtön két csomópont hello hasonló kimenetnek kell megjelennie. Hello Azure RDMA hálózati késés, vagy az az üzenet-méretek mentése too512 bájt 3 ezredmásodperc alatt várható.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# hello number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected toobe exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through hello flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks toorun:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Következő lépések
* Regisztrálhat és futtathat a Linux MPI alkalmazások a Linux-fürt.
* Lásd: hello [Intel MPI Library dokumentációjában](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) Intel MPI útmutatót.
* Próbálja meg egy [gyorsindítási sablonon](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) toocreate egy Intel fényesség fürt HPC CentOS-alapú lemezkép használatával. További információkért lásd: [Intel felhő Edition telepítését a Microsoft Azure fényesség](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
