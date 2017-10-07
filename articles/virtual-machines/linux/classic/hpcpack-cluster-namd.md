---
title: "a Microsoft HPC Pack Linux virtuális gépeken aaaNAMD |} Microsoft Docs"
description: "Azure a Microsoft HPC Pack fürt központi telepítése, és futtassa a NAMD szimuláció charmrun több Linux számítási csomóponton"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 76072c6b-ac35-4729-ba67-0d16f9443bd7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/13/2016
ms.author: danlep
ms.openlocfilehash: 90c722f66b494335a62c0613079366ae99002076
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Az NAMD futtatása a Microsoft HPC Packkal Azure-beli linuxos számítási csomópontokon
A cikkből megtudhatja, egyirányú toorun egy Linux nagy teljesítményű számítástechnikai rendszerek (HPC) munkaterhelés Azure virtuális gépeken. Itt, állítsa be a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) -fürtöt az Azure-on Linux számítási csomópontokat, és futtassa a [NAMD](http://www.ks.uiuc.edu/Research/namd/) szimuláció toocalculate egy nagy biomolekuláris rendszer hello szerkezetét, és.  

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (Nanoscale molekuláris Dynamics program) van egy nagy teljesítményű szimulálása a nagy biomolekuláris rendszerekhez készült párhuzamos molekuláris dynamics csomagot tartalmazó az Atom toomillions fel. Ezek a rendszerek közé vírusok, cella és a nagy fehérjék. NAMD arányosan toohundreds tipikus szimulációja és toomore mint 500 000 magos hello legnagyobb szimulációja magszámra vonatkozó követelménynek.
* **A Microsoft HPC Pack** toorun funkciókat biztosít a nagyméretű HPC és a helyszíni számítógépek vagy az Azure virtuális gépek fürtök párhuzamos alkalmazásait. Eredetileg fejlesztett munkaterheléseknél Windows HPC HPC Pack megoldásként mostantól támogatja a futó Linux HPC alkalmazások Linux rendszeren telepített HPC Pack-fürtben lévő csomópont virtuális gépek számítási. Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) bevezető.

Az egyéb beállítások toorun Linux HPC munkaterhelések az Azure, lásd: [kötegelt és nagy teljesítményű számítástechnikai rendszerek technikai erőforrások](../../../batch/batch-hpc-solutions.md).

## <a name="prerequisites"></a>Előfeltételek
* **A számítási csomópontok HPC Pack fürt Linux** -fürt üzembe helyezése HPC Pack Linux számítási csomópontok az Azure-ban vagy egy [Azure Resource Manager sablon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) vagy egy [Azure PowerShell-parancsfájl](hpcpack-cluster-powershell-script.md). Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) hello Előfeltételek és az egyik beállítási mód lépéseit. Ha hello PowerShell parancsfájl központi telepítés lehetőséget választja, tekintse meg a hello minta konfigurációs fájlt a mintafájlok hello hello Ez a cikk végén. Ez a fájl az Azure-alapú HPC Pack fürtöt egy Windows Server 2012 R2 átjárócsomópont és a számítási csomópontok négy mérete nagy CentOS 6.6 konfigurálja. Testre szabhatja ezt a fájlt, a saját környezetéhez szükséges módon.
* **Szoftver- és oktatóanyag fájlok NAMD** -letöltése NAMD szoftverek Linux hello [NAMD](http://www.ks.uiuc.edu/Research/namd/) hely (regisztráció szükséges). Ez a cikk NAMD verzió 2.10 alapul, és használja a hello [Linux-x86_64 (64 bites Intel vagy AMD Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archív. Töltse le a is hello [NAMD oktatóanyag fájlok](http://www.ks.uiuc.edu/Training/Tutorials/#namd). hello letöltések .tar fájlokat, és egy Windows eszköz tooextract hello fájlok hello átjárócsomóponthoz van szüksége. tooextract hello fájlok, kövesse a cikkben később hello utasításokat. 
* **VMD** (nem kötelező) – a NAMD feladat eredményeinek toosee hello töltse le és telepítse a hello molekuláris képi megjelenítés program [VMD](http://www.ks.uiuc.edu/Research/vmd/) a kiválasztott számítógépen. hello nem 1.9.2. Lásd: hello VMD letöltési hely tooget elindult.  

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Kölcsönös, a számítási csomópontok közötti megbízhatósági kapcsolat beállítása
A kereszt-csomópont feladat több Linux csomópontokon futó szükséges hello csomópontok tootrust egymáshoz (által **rsh** vagy **ssh**). Hello HPC Pack fürt létrehozásakor a Microsoft HPC Pack IaaS telepítési parancsfájl hello hello parancsfájl automatikusan beállítja állandó kölcsönös megbízhatósági hello rendszergazdai fiókhoz megadott. A nem rendszergazda felhasználók hello fürt a tartományban létrehozta meg kell tooset mentése hello csomópontok közötti ideiglenes kölcsönös megbízhatósági Ha egy feladat le van foglalva toothem. Majd semmisítse meg hello kapcsolat hello feladat befejezése után. toodo minden felhasználónak, így egy RSA kulcspár toohello fürt melyik HPC Pack tooestablish hello megbízhatósági kapcsolatot használ. Kövesse az utasításokat.

### <a name="generate-an-rsa-key-pair"></a>Egy RSA kulcspár létrehozása
Könnyen toogenerate egy RSA kulcsból álló kulcspárt, amely tartalmazza a nyilvános kulcsot és titkos kulccsal, futtatásával hello Linux **ssh-keygen** parancsot.

1. Jelentkezzen be tooa Linux-számítógép.
2. Futtassa a következő parancs hello:
   
   ```bash
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > Nyomja le az **Enter** toouse hello alapértelmezett beállítások hello parancs befejeződéséig. Ne adjon meg egy jelszót. Amikor a rendszer kéri a jelszót, csak Entert **Enter**.
   > 
   > 
   
   ![Egy RSA kulcspár létrehozása][keygen]
3. Módosítsa a könyvtárat toohello ~/.ssh könyvtárat. hello titkos kulcs id_rsa.pub a id_rsa és hello nyilvános kulcs tárolja.
   
   ![Nyilvános és titkos kulcsai][keys]

### <a name="add-hello-key-pair-toohello-hpc-pack-cluster"></a>Hello kulcspár toohello HPC Pack fürt hozzáadása
1. [Csatlakozás távoli asztal](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello átjárócsomópont VM használatával hello (például hpc\clusteradmin) hello fürt telepítésekor megadott tartományi hitelesítő adatokat. Hello átjárócsomópont származó hello fürt kezelése.
2. Szabványos Windows Server eljárások toocreate tartományi felhasználói fiókot használja a hello fürt Active Directory-tartományhoz. Hello Active Directory – felhasználók és számítógépek eszközzel például hello átjárócsomópont használja. hello a cikkben szereplő példák azt feltételezik, hpcuser hello hpclab tartományban (hpclab\hpcuser) nevű tartományi felhasználót kell létrehozni.
3. Adja hozzá a fürt felhasználói hello tartományi felhasználói toohello HPC Pack fürtöt. Útmutatásért lásd: [hozzáadását vagy eltávolítását fürt felhasználók](https://technet.microsoft.com/library/ff919330.aspx).
4. Hozzon létre egy fájlt C:\cred.xml, és másolja hello RSA kulcsadatokat bele. Található példa hello mintafájlok hello Ez a cikk végén.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
5. Nyisson meg egy parancssort, és írja be a következő parancs tooset hello adatok hello hpclab\hpcuser fiók hitelesítő adatai hello. Hello használata **extendeddata** paraméter toopass hello hello C:\cred.xml fájljának létrehozott hello kulcsadatokat nevét.
   
   ```command
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Ez a parancs sikeresen kimeneti nélkül befejeződött. Miután beállította a hello toorun feladatok kell hello felhasználói fiókok hitelesítő adatait, hello cred.xml fájlt tárolja biztonságos helyen, vagy törölje azt.
6. Ha RSA kulcspár hello a Linux-csomópontok egyikén jön létre, azok használatának befejezése után ne feledje toodelete hello kulcsok. Ha úgy találja, hogy egy meglévő id_rsa vagy id_rsa.pub fájl nincs beállítva HPC Pack kölcsönös megbízhatósági.

> [!IMPORTANT]
> Nem ajánlott a Linux feladat futtatásával a fürt rendszergazdai megosztott fürtön, mivel hello Linux csomópontján hello legfelső szintű fiók alatt fut egy feladat, a rendszergazda által küldött. Egy feladat, a nem rendszergazda felhasználók által benyújtott Linux helyi felhasználói fiók alatt fut hello azonos nevet hello feladat felhasználóként. Ebben az esetben HPC Pack állít be a Linux-felhasználó kölcsönös megbízhatósági toohello feladathoz kiosztott összes hello csomópont. Hello feladat futtatása előtt manuálisan csomópontján hello Linux hello Linux felhasználói állíthat be, vagy a HPC Pack hello felhasználó létrehozása automatikusan hello feladat elküldésekor. HPC Pack hello felhasználó hoz létre, ha HPC Pack törli azt hello feladat befejezése után. tooreduce biztonsági kockázatot jelentenek, hello kulcsok el lesznek távolítva, befejeződött hello feladatok hello csomópontján.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Linux-csomópontok fájlmegosztás beállítása
Most beállítása az SMB-fájlmegosztásra, és a csatlakoztatási hello megosztott mappában található összes Linux csomópontok tooallow hello Linux csomópontok tooaccess NAMD fájlt egy közös elérési utat. Az alábbiakban lépéseket toomount hello átjárócsomópont lévő megosztott mappához. A megosztás például a CentOS 6.6 terjesztéseket, amelyek jelenleg nem támogatja a hello Azure File service ajánlott. Ha a Linux-csomópontok támogatja egy Azure-fájlmegosztás, látható [hogyan toouse Linux Azure File storage](../../../storage/files/storage-how-to-use-files-linux.md). További fájl megosztási beállítások HPC Pack, lásd: [Ismerkedés az Azure-ban HPC Pack fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).

1. Hozzon létre egy mappát a hello átjárócsomópont, és ossza meg úgy, hogy olvasási/írási jogosultsággal tooEveryone. Ebben a példában \\ \\CentOS66HN\Namd hello mappát, ahol a CentOS66HN hello átjárócsomópont hello állomásnevét hello neve.
2. Hozzon létre egy almappát namd2 hello megosztott mappában. Namd2 hozzon létre egy másik almappát namdsample.
3. Bontsa ki a hello NAMD fájlt hello mappában által a Windows verziójával **bont** vagy egy másik Windows-segédprogram, amely .tar kiterjesztett archívumokat. 
   
   * Hello NAMD bont archívum kibontása túl\\\\CentOS66HN\Namd\namd2.
   * Bontsa ki a hello oktatóanyag fájlt a \\ \\CentOS66HN\Namd\namd2\namdsample.
4. Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsok toomount hello hello Linux csomópontok megosztott mappa hello.
   
    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /namd2 nevű mappát. hello második parancs csatlakoztatja hello megosztott mappa //CentOS66HN/Namd/namd2 alakzatot dir_mode és file_mode bit beállítása too777 hello mappában. Hello *felhasználónév* és *jelszó* hello parancs hello hello átjárócsomópont a felhasználó hitelesítő adatait kell lennie.

> [!NOTE]
> hello "\`" hello második parancs a szimbólum értéke egy escape szimbólum PowerShell. "\`,"azt jelenti, hogy hello"," (vessző karakter) hello parancs részét képezi.
> 
> 

## <a name="create-a-bash-script-toorun-a-namd-job"></a>A Bash parancsfájlok toorun NAMD feladat létrehozása
A NAMD feladatot kell egy *csomópontlista* fájlt **charmrun** toodetermine hello száma csomópontok toouse NAMD folyamatok indításakor. A Bash parancsfájlt, amely hello csomópontlista fájlt hoz létre, és futtatja **charmrun** a csomópontlista fájllal. Majd elküldheti a HPC-Fürtkezelőben ezt a parancsfájlt meghívó NAMD feladat.

Az Ön által választott szövegszerkesztő használatával hozzon létre egy Bash parancsfájlok hello NAMD programfájljait tartalmazó hello /namd2 mappában, és nevezze el hpccharmrun.sh. A koncepció igazolása kész, másolja át a cikk végén hello megadott hello példa hpccharmrun.sh parancsfájl, és nyissa meg túl[NAMD feladat elküldése](#submit-a-namd-job).

> [!TIP]
> Mentse a parancsfájlt a Linux és szövegfájlként sorvégeket a (csak LF, nem a CR LF). Ez biztosítja, hogy fusson megfelelően hello Linux csomópontján.
> 
> 

Az alábbiakban a bash parancsfájlok funkciója részleteit. 

1. Adja meg a bizonyos változókat.
   
   ```bash
   #!/bin/bash
   
   # hello path of this script
   SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
   # Charmrun command
   CHARMRUN=${SCRIPT_PATH}/charmrun
   # Argument of ++nodelist
   NODELIST_OPT="++nodelist"
   # Argument of ++p
   NUMPROCESS="+p"
   ```
2. Csomópont-információk lekérése hello környezeti változók. $NODESCORES $CCP_NODES_CORES vegyes szavak listáját tárolja. $COUNT $NODESCORES hello méretét.
   
   ```
   # Get node information from hello environment variables
   NODESCORES=(${CCP_NODES_CORES})
   COUNT=${#NODESCORES[@]}
   ```    
   
   hello hello $CCP_NODES_CORES változó formátuma a következő:
   
   ```
   <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
   ```
   
   Ez a változó hello száma csomópontok csomópontnevek és minden egyes csomóponton a toohello feladat lefoglalt magok száma sorolja fel. Például ha a hello feladat 10 magok toorun, hello $CCP_NODES_CORES értéke hasonló:
   
   ```
   3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
   ```
3. Ha hello $CCP_NODES_CORES változó nincs beállítva, indítsa el **charmrun** közvetlenül. (Ez csak történjen, ha a parancsfájl futtatását közvetlenül a Linux-csomóponton.)
   
   ```
   if [ ${COUNT} -eq 0 ]
   then
     # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
     #echo ${CHARMRUN} $*
     ${CHARMRUN} $*
   ```
4. Vagy hozzon létre egy csomópontlista fájlt **charmrun**.
   
   ```
   else
     # Create hello nodelist file
     NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$
   
     # Write hello head line
     echo "group main" > ${NODELIST_PATH}
   
     # Get every node name and number of cores and write into hello nodelist file
     I=1
     while [ ${I} -lt ${COUNT} ]
     do
         echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
         let "I=${I}+2"
     done
   ```
5. Futtatás **charmrun** hello csomópontlista fájl, a visszatérési állapotának beolvasása és eltávolítása hello csomópontlista fájl hello végén.
   
   {CCP_NUMCPUS} $ egy másik környezeti változó hello HPC Pack átjárócsomópont állítja be. Teljes toothis feladat lefoglalt magok száma hello tárol. A Microsoft használhatjuk a folyamatok száma toospecify hello charmrun.
   
   ```
   # Run charmrun with nodelist arg
   #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   
   RTNSTS=$?
   rm -f ${NODELIST_PATH}
   fi
   
   ```
6. Kilépés a hello **charmrun** visszatérési állapota.
   
   ```
   exit ${RTNSTS}
   ```

Az alábbiakban látható hello adatokat hello csomópontlista fájlban, mely hello parancsfájlt hoz létre:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Példa:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>NAMD feladat elküldése
Most már készen áll a toosubmit a NAMD feladatok HPC-Fürtkezelőben áll.

1. Csatlakozás tooyour fürt átjárócsomópontjával, és indítsa el a HPC Cluster Manager.
2. A **erőforrás-kezelés**, akkor győződjön meg arról, hogy hello Linux számítási csomópontok hello **Online** állapotát. Ha nem, válassza ki azokat, és kattintson a **online állapotba hozás**.
3. A **feladatkezelés**, kattintson a **új feladat**.
4. Írjon be egy feladat nevét, például *hpccharmrun*.
   
   ![Új HPC-feladat][namd_job]
5. A hello **feladat részletei** lapján, a **feladat erőforrások**, válassza ki, mint erőforrás hello típusú **csomópont** és set hello **minimális** too3. , a Microsoft hello feladat futtatása a három Linux csomópontok, és minden fürtcsomópont négy magok.
   
   ![Feladat erőforrások][job_resources]
6. Kattintson a **szerkesztése feladatok** a bal oldali navigációs hello, és kattintson a **Hozzáadás** tooadd egy feladat toohello feladat.    
7. A hello **feladat részleteinek és az i/o-átirányítás** lapon, a következő értékek set hello:
   
   * **Parancssori**-
     `/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`
     
     > [!TIP]
     > hello előző parancssora sortörés nélkül egyetlen paranccsal. Az tooappear becsomagolja alatt több sorban **parancssori**.
     > 
     > 
   * **Munkakönyvtár** -/namd2
   * **Minimális** – 3
     
     ![Feladat részletei][task_details]
     
     > [!NOTE]
     > Itt beállította az hello munkakönyvtár mert **charmrun** toonavigate toohello megpróbál azonos munkakönyvtára, minden egyes csomóponton. Ha hello munkakönyvtár nem van beállítva, HPC Pack hello parancs indítja el hello Linux csomópontok egyikén létrehozni egy véletlenszerűen előállított nevű mappát. Ennek hatására hello a következő hiba hello más csomópontok: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` tooavoid a probléma, adja meg a mappa elérési útját hello munkakönyvtára, minden csomópontja által elérhető.
     > 
     > 
8. Kattintson a **OK** , majd **Submit** toorun a feladatot.
   
   Alapértelmezés szerint a HPC Pack hello feladat, a jelenlegi bejelentkezett felhasználói fiók küldi el. A párbeszédpanel kérhetik tooenter hello felhasználónevet és jelszót kattintás után **Submit**.
   
   ![Feladat hitelesítő adatai][creds]
   
   Bizonyos körülmények között a HPC Pack emlékszik hello felhasználói adatok előtt adjon meg, és nem jeleníti meg ezen a párbeszédpanelen. HPC Pack toomake újbóli, hello a következő parancsot a parancssorba írja be, és majd elküldeni a hello feladatot.
   
   ```command
   hpccred delcreds
   ```
9. hello feladat végrehajtásához szükséges néhány perc toofinish.
10. Hello feladat naplójának található \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log és hello kimeneti fájlok \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.
11. Szükség esetén indítsa el a VMD tooview a feladat eredményeinek. hello hello NAMD a kimenő fájlok (ebben az esetben a vízjel területén ubiquitin fehérje molekula) megjelenítésére lépésekre Ez a cikk hello terjed. Lásd: [NAMD oktatóanyag](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) részleteiről.
    
    ![Feladat eredményeinek][vmd_view]

## <a name="sample-files"></a>Mintafájlok
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>A minta XML konfigurációs fájl fürttelepítés PowerShell-parancsprogram
```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a>Mintafájl cred.xml
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```

### <a name="sample-hpccharmrunsh-script"></a>Mintaparancsfájl hpccharmrun.sh
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run hello charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create hello nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write hello head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into hello nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 

<!--Image references-->
[keygen]:media/hpcpack-cluster-namd/keygen.png
[keys]:media/hpcpack-cluster-namd/keys.png
[namd_job]:media/hpcpack-cluster-namd/namd_job.png
[job_resources]:media/hpcpack-cluster-namd/job_resources.png
[creds]:media/hpcpack-cluster-namd/creds.png
[task_details]:media/hpcpack-cluster-namd/task_details.png
[vmd_view]:media/hpcpack-cluster-namd/vmd_view.png
