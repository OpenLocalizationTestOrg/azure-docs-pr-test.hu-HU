---
title: "a Linux virtuális gépeken HPC Pack OpenFOAM aaaRun |} Microsoft Docs"
description: "Az Azure a Microsoft HPC Pack fürt központi telepítése, és egy OpenFOAM feladat futtatása több Linux számítási csomóponton az RDMA-hálózaton."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: 1a51ffe6804abb5156f01d580c9b7a4a9649377a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Az OpenFoam futtatása a Microsoft HPC Packkal Azure-beli linuxos RDMA-fürtön
Ez a cikk bemutatja, egyirányú toorun OpenFoam az Azure virtuális gépeken. Itt, a Microsoft HPC Pack-fürt Linux számítási csomópontok az Azure és a Futtatás telepít egy [OpenFoam](http://openfoam.com/) Intel MPI feladatot. Használhatja az RDMA-kompatibilis Azure virtuális gépek hello számítási csomópontokat, így hello számítási csomópontok hello Azure RDMA hálózati protokollt használó kommunikációra. Egyéb beállítások toorun teljesen tartalmazza az Azure-ban OpenFoam konfigurált hello UberCloud meg például a piactér kereskedelmi lemezképeket [OpenFoam 2.3 a CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), és futtatja a [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

(A nyissa meg a mező művelet és adatkezelési) OpenFOAM egy nyílt forráskódú számítási folyadékból dynamics (CFD) csomagot, amely széles körben használt mérnöki és tudományos, kereskedelmi és a academic szervezetekben. Ez magában foglalja az eszközök meshing, nevezetesen snappyHexMesh, egy parallelized mesher összetett CAD-geometriája, és előtti és utáni feldolgozásra. Szinte minden folyamatok engedélyezése a felhasználók tootake teljes mértékben a rendelkezésükre hardverek párhuzamosan futnak.  

A Microsoft HPC Pack szolgáltatások toorun biztosít a nagyméretű HPC és párhuzamos alkalmazások, beleértve a MPI alkalmazásokat, a Microsoft Azure virtuális gépek fürtjein. HPC Pack támogatja a virtuális gépeken telepített HPC Pack-fürtben lévő Linux alkalmazások csomópontjain futó Linux HPC is. Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) egy bevezető toousing a Linux számítási csomópontok HPC Pack.

> [!NOTE]
> Ez a cikk bemutatja, hogyan toorun egy Linux MPI-feladatok HPC Pack. Azt feltételezi, hogy rendelkezik-e bizonyos fokú ismeretét, Linux rendszer felügyeleti és a Linux fürtökön MPI munkaterheket futtatnak. MPI és OpenFOAM hello állók közül. Ez a cikk látható a különböző verzióit használhatja, ha lehetséges, hogy toomodify néhány telepítési és konfigurációs lépéseket. 
> 
> 

## <a name="prerequisites"></a>Előfeltételek
* **HPC Pack fürt RDMA-kompatibilisek-e a Linux és a számítási csomópontok** - fürt üzembe helyezése HPC Pack méretű A8, A9, H16r, vagy H16rm Linux számítási csomópontok használatával egy [Azure Resource Manager sablon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) vagy egy [Azure PowerShell parancsfájl](hpcpack-cluster-powershell-script.md). Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) hello Előfeltételek és az egyik beállítási mód lépéseit. Ha hello PowerShell parancsfájl központi telepítés lehetőséget választja, tekintse meg a hello minta konfigurációs fájlt a mintafájlok hello hello Ez a cikk végén. A méret A8 Windows Server 2012 R2 átjárócsomópont álló konfigurációs egy Azure-alapú toodeploy HPC Pack fürtöt használ, és 2 mérete A8 SUSE Linux Enterprise Server 12 számítási csomópontjain. Az előfizetés és a szolgáltatás nevének megfelelő értékeit helyettesítse. 
  
  **További dolgot tooknow**
  
  * Linux RDMA hálózati Előfeltételek az Azure-ban, lásd: [nagy teljesítményű számítási Virtuálisgép-méretek](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  * Ha hello Powershell parancsprogram rendszerbe állítási beállításának használata esetén telepítse az összes hello Linux számítási csomópontokat belül egy felhőalapú szolgáltatás toouse hello RDMA hálózati kapcsolatot.
  * Miután üzembe hello Linux csomópontok, csatlakozzon az SSH tooperform minden további felügyeleti feladatot. Hello SSH-kapcsolat adatai az egyes Linux virtuális gépek található hello Azure-portálon.  
* **Intel MPI** -toorun OpenFOAM SLES 12 HPC a számítási csomópontok az Azure-ban, a hello tooinstall hello Intel MPI könyvtár 5 futásidejű kell [Intel.com hely](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 előre telepítve van a CentOS-alapú HPC képek.)  Egy későbbi lépésben szükség esetén telepítse az Intel MPI a Linux számítási csomóponton. Ebben a lépésben tooprepare Intel, hogy a regisztrálása után hivatkozásra hello hello megerősítő e-mail toohello kapcsolódó weblapon. Ezt követően másolási hello letöltése hello .tgz fájl hello Intel MPI megfelelő verziójának hivatkozását. Ez a cikk Intel MPI verzió 5.0.3.048 alapul.
* **OpenFOAM forrás Pack** -Linux hello OpenFOAM forrás Pack szoftverek letöltését hello [OpenFOAM Foundation webhely](http://openfoam.org/download/2-3-1-source/). Ez a cikk a forrás csomag verziója letölthető 2.3.1 OpenFOAM-2.3.1.tgz van alapján. Később Ez a cikk toounpack hello utasításokat követve, és OpenFOAM fordítási hello Linux számítási csomóponton.
* **EnSight** (nem kötelező) – toosee hello eredményeit a OpenFOAM szimuláció, töltse le és telepítse a hello [EnSight](https://www.ceisoftware.com/download/) képi megjelenítés és elemzések program. Licencelési és letöltési információkat hello EnSight helyen vannak.

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Kölcsönös, a számítási csomópontok közötti megbízhatósági kapcsolat beállítása
A kereszt-csomópont feladat több Linux csomópontokon futó szükséges hello csomópontok tootrust egymáshoz (által **rsh** vagy **ssh**). Hello HPC Pack fürt létrehozásakor a Microsoft HPC Pack IaaS telepítési parancsfájl hello hello parancsfájl automatikusan beállítja állandó kölcsönös megbízhatósági hello rendszergazdai fiókhoz megadott. A nem rendszergazda felhasználók hello fürt tartományt hoz létre meg kell tooset hello csomópontok közötti ideiglenes kölcsönös megbízhatósági fel egy feladat le van foglalva toothem, és szüntesse meg a hello kapcsolat hello feladat befejezése után. minden felhasználó esetében tooestablish megbízhatósági adjon meg egy RSA kulcspár toohello fürt használó HPC Pack hello megbízhatósági kapcsolathoz.

### <a name="generate-an-rsa-key-pair"></a>Egy RSA kulcspár létrehozása
Könnyen toogenerate egy RSA kulcsból álló kulcspárt, amely tartalmazza a nyilvános kulcsot és titkos kulccsal, futtatásával hello Linux **ssh-keygen** parancsot.

1. Jelentkezzen be tooa Linux-számítógép.
2. Futtassa a következő parancs hello:
   
   ```
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
1. Ellenőrizze a távoli asztali kapcsolat tooyour átjárócsomópont a HPC Pack rendszergazdai fiókkal (hello rendszergazdai fiók beállítása hello telepítési parancsfájl futtatásakor).
2. Szabványos Windows Server eljárások toocreate tartományi felhasználói fiókot használja a hello fürt Active Directory-tartományhoz. Hello Active Directory – felhasználók és számítógépek eszközzel például hello átjárócsomópont használja. hello a cikkben szereplő példák azt feltételezik, hpclab\hpcuser nevű tartományi felhasználót kell létrehozni.
3. Hozzon létre egy fájlt C:\cred.xml, és másolja hello RSA kulcsadatokat bele. A mintafájl cred.xml van ez a cikk végén hello.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. Nyisson meg egy parancssort, és írja be a következő parancs tooset hello adatok hello hpclab\hpcuser fiók hitelesítő adatai hello. Hello használata **extendeddata** paraméter toopass hello C:\cred.xml fájl létrehozott hello kulcs adatok nevét.
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Ez a parancs sikeresen kimeneti nélkül befejeződött. Miután beállította a hello toorun feladatok kell hello felhasználói fiókok hitelesítő adatait, hello cred.xml fájlt tárolja biztonságos helyen, vagy törölje azt.
5. Ha RSA kulcspár hello a Linux-csomópontok egyikén jön létre, azok használatának befejezése után ne feledje toodelete hello kulcsok. Ha HPC Pack talál egy meglévő id_rsa vagy id_rsa.pub fájl, akkor nem kölcsönös megbízhatóság kialakításához.

> [!IMPORTANT]
> Nem ajánlott a Linux feladat futtatásával a fürt rendszergazdai megosztott fürtön, mivel hello Linux csomópontján hello legfelső szintű fiók alatt fut egy feladat, a rendszergazda által küldött. A feladat a nem rendszergazda felhasználók által benyújtott azonban Linux helyi felhasználói fiók alatt fut, az azonos név hello feladat felhasználóként hello. Ebben az esetben HPC Pack állít be a Linux-felhasználó kölcsönös megbízhatósági toohello feladat lefoglalt hello csomópontjai között. Hello feladat futtatása előtt manuálisan csomópontján hello Linux hello Linux felhasználói állíthat be, vagy a HPC Pack hello felhasználó létrehozása automatikusan hello feladat elküldésekor. HPC Pack hello felhasználó hoz létre, ha HPC Pack törli azt hello feladat befejezése után. tooreduce biztonsági fenyegetések HPC Pack hello kulcsok feladat befejeződése után eltávolítja.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Linux-csomópontok fájlmegosztás beállítása
Most már készen egy szabványos SMB-megosztás hello átjárócsomópont az egyik mappájába. tooallow hello Linux csomópontok tooaccess alkalmazásfájlok közös elérési úttal rendelkező, a csatlakoztatási hello megosztott mappa hello Linux csomópontján. Ha azt szeretné, használhatja a beállítást, például egy Azure fájlok fájlmegosztás - számos forgatókönyv - vagy az NFS-megosztások ajánlott egy másik fájlmegosztás. Hello fájlmegosztási információk és részletes lépéseit lásd: [Ismerkedés az Azure-ban HPC Pack fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md).

1. Hozzon létre egy mappát a hello átjárócsomópont, és ossza meg úgy, hogy olvasási/írási jogosultsággal tooEveryone. Például megosztás C:\OpenFOAM hello átjárócsomópont, \\ \\SUSE12RDMA-HN\OpenFOAM. Itt *SUSE12RDMA-HN* hello átjárócsomópont hello állomás neve.
2. Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsok hello:
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

hello első parancs létrehoz egy hello LinuxNodes csoport minden csomópontján /openfoam nevű mappát. hello második parancs a Linux-csomópont hello dir_mode és file_mode bit beállítása too777 hello megosztott mappa //SUSE12RDMA-HN/OpenFOAM csatlakoztatja. Hello *felhasználónév* és *jelszó* hello parancs hello hello átjárócsomópont a felhasználó hitelesítő adatait kell lennie.

> [!NOTE]
> hello "\`" hello második parancs a szimbólum értéke egy escape szimbólum PowerShell. "\`,"azt jelenti, hogy hello"," (vessző karakter) hello parancs részét képezi.
> 
> 

## <a name="install-mpi-and-openfoam"></a>MPI és OpenFOAM telepítése
MPI feladatként hello RDMA hálózati OpenFOAM toorun, toocompile OpenFOAM hello Intel MPI könyvtárakkal kell. 

Először futtassa több **clusrun** tooinstall Intel MPI függvénytárak (Ha még nincs telepítve) és OpenFOAM parancsok a Linux-csomóponton. Használjon hello átjárócsomópont megosztás korábban konfigurált tooshare hello telepítőfájlok hello Linux csomópontok között.

> [!IMPORTANT]
> Ezeket a telepítési és fordítása lépések példái. Meg kell néhány a Linux rendszer felügyeleti tooensure, hogy a függő compilers – és a szalagtárak megfelelően vannak telepítve. Előfordulhat, hogy szüksége toomodify bizonyos környezeti változók és más beállítások az Intel MPI és OpenFOAM verzióit. További információkért lásd: [Intel MPI Library for Linux – telepítési útmutató](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) és [OpenFOAM forrás csomag telepítése](http://openfoam.org/download/2-3-1-source/) alkalmaz környezetében.
> 
> 

### <a name="install-intel-mpi"></a>Intel MPI telepítése
Hello letöltött telepítési csomag mentéséhez az Intel MPI (ebben a példában l_mpi_p_5.0.3.048.tgz) a C:\OpenFoam hello központi csomóponton, hogy hello Linux csomópontok /openfoam hozzáfér ehhez a fájl. Ezután futtassa **clusrun** tooinstall Intel MPI könyvtár minden hello Linux csomópontján.

1. hello alábbi hello telepítési csomag másolása a parancsokat, és bontsa ki túl/opt/intel minden egyes csomóponton.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. Intel MPI könyvtár tooinstall csendes módban, a silent.cfg fájl használható. Található példa hello mintafájlok hello Ez a cikk végén. Hely a fájlt az hello megosztott mappa /openfoam. Hello silent.cfg fájllal kapcsolatos részletekért lásd: [Intel MPI Library for Linux – telepítési útmutató - csendes telepítést](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).
   
   > [!TIP]
   > Győződjön meg arról, hogy Ön a silent.cfg fájl szövegfájl, amelynek Linux sorvégeket (csak LF, nem a CR LF). Ez a lépés biztosítja, hogy ez megfelelően hello Linux csomópontján.
   > 
   > 
3. Intel MPI könyvtár telepítése csendes módban.
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a>MPI konfigurálása
Tesztelési, adja hozzá mindegyik hello Linux csomópontok sorok toohello /etc/security/limits.conf következő hello:

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Indítsa újra a hello Linux csomópontok hello limits.conf fájl frissítése után. Például használja az hello következő **clusrun** parancs:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Az újraindítás után győződjön meg arról, mint /openfoam csatlakoztatva van a hello megosztott mappában.

### <a name="compile-and-install-openfoam"></a>Fordítsa le és OpenFOAM telepítése
Letöltött telepítőcsomag hello hello OpenFOAM forrás Pack (OpenFOAM 2.3.1.tgz ebben a példában) tooC:\OpenFoam mentheti hello átjárócsomópont, hogy hello Linux csomópontok /openfoam hozzáfér ehhez a fájlt. Ezután futtassa **clusrun** toocompile OpenFOAM az összes Linux csomópontok hello parancsok.

1. Hozzon létre egy mappát /opt/OpenFOAM minden egyes csomóponton Linux másolási hello csomag toothis forrásmappa, és bontsa ki van.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. toocompile OpenFOAM hello Intel MPI könyvtár, először állítsa be néhány környezetiblokk-változót Intel MPI és OpenFOAM együtt. Nevű settings.sh tooset hello változók bash parancsfájl használatára. Található példa hello mintafájlok hello Ez a cikk végén. Hely a hello (mentett rendelkező Linux sorvégződések) fájlhoz megosztott mappa /openfoam. Ez a fájl is, hogy használja-e újabb toorun egy OpenFOAM feladat hello MPI és OpenFOAM futtatókörnyezetek beállításait tartalmazza.
3. Függő csomagok telepítése toocompile OpenFOAM szükséges. Attól függően, hogy a Linux-disztribúció először meg kell tooadd tára. Futtatás **clusrun** hasonló toohello következő parancsokat:
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    Ha szükséges, az SSH tooeach Linux csomópont toorun hello megfelelően futásuk tooconfirm parancsokat.
4. Futtassa a következő parancs toocompile OpenFOAM hello. hello összeállítási folyamat egyes idő toocomplete vesz igénybe, és nagy mennyiségű információ toostandard naplót hoz létre, ezért hello **/ időosztásos** toodisplay hello kimeneti időosztásos lehetőséget.
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > hello "\`" szimbólum hello parancsban értéke egy escape szimbólum PowerShell. "\`&" azt jelenti, hogy a hello "&" hello parancs része.
   > 
   > 

## <a name="prepare-toorun-an-openfoam-job"></a>Toorun OpenFOAM feladatok előkészítése
Most get készen toorun egy MPI feladat sloshingTank3D, amely hello OpenFoam minta egyike, két Linux csomópont hívása. 

### <a name="set-up-hello-runtime-environment"></a>Hello futásidejű környezet beállítása
tooset hello futásidejű környezetek MPI és OpenFOAM hello Linux csomóponton, futtassa a következő parancsot egy Windows PowerShell-ablakban hello központi csomóponton hello fel. (Ez a parancs érvénytelen a SUSE Linux csak.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Mintaadatok előkészítése
Használjon hello átjárócsomópont megosztás korábban konfigurált tooshare fájlok közötti hello Linux csomópontok (mint /openfoam csatlakoztatott).

1. A Linux SSH tooone számítási csomópontjain.
2. A következő parancs tooset hello OpenFOAM futásidejű környezet hello üzemeltetéséhez, még nem tette meg.
   
   ```
   $ source /openfoam/settings.sh
   ```
3. Másolja hello sloshingTank3D minta toohello megosztott mappát, és keresse meg a tooit.
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. Ez a minta alapértelmezett paraméterértékeivel hello használata esetén is igénybe vehet perc toorun, ahol, érdemes toomodify egyes paraméterek toomake gyorsabban futnak. Egy egyszerű lehetőséget toomodify hello idő lépés változók deltaT és writeInterval hello rendszer/controlDict fájlban. Ez a fájl tartalmazza az idő és adatok írásakor vagy olvasásakor megoldás toohello irányítását vonatkozó összes bemeneti adatot. Például megváltoztathatja a 0,05 too0.5 deltaT hello értékének és a 0,05 too0.5 writeInterval hello értékét.
   
   ![Lépés változók módosítása][step_variables]
5. Adja meg a kívánt értékeket hello változók hello rendszer/decomposeParDict fájlban. A példában két Linux-csomópont minden 8 magos, úgy állítsa be numberOfSubdomains too16 és n hierarchicalCoeffs too(1 1 16), ami azt jelenti, 16 folyamatokkal párhuzamosan futnak a OpenFOAM. További információkért lásd: [OpenFOAM felhasználói útmutatója: 3.4 futó alkalmazások párhuzamosan](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).
   
   ![Folyamatok felbontani][decompose]
6. Futtassa a parancsokat követően – hello sloshingTank3D directory tooprepare hello mintaadatok hello.
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. A hello átjárócsomópont meg kell jelennie hello mintaadatfájlok C:\OpenFoam\sloshingTank3D másol. (C:\OpenFoam hello hello átjárócsomópont lévő megosztott mappához.)
   
   ![Hello átjárócsomópont adatfájlokat][data_files]

### <a name="host-file-for-mpirun"></a>Állomás fájlt mpirun
Ebben a lépésben létrehozott állomás-fájlokat (számítási csomópontok listáját) mely hello **mpirun** parancsot használja.

1. Hello Linux csomópontok egyik hozzon létre egy fájlt hostfile alatt /openfoam, ezért ezt a fájlt az összes Linux csomóponton /openfoam/hostfile címen érhető el.
2. A Linux csomópontnevek írni az ezt a fájlt. Ebben a példában a hello fájl nevét a következő hello tartalmazza:
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > Ez a fájl C:\OpenFoam\hostfile: hello központi csomóponton is létrehozhat. Ha ezt a lehetőséget választja, a Linux és szövegfájlként menteni sorvégeket (csak LF, nem a CR LF). Ez biztosítja, hogy fusson megfelelően hello Linux csomópontján.
   > 
   > 
   
   **Bash parancsfájlok burkoló**
   
   Ha sok Linux-csomópont van, és azt szeretné, hogy a feladat toorun csak az egyes, nincs egy jó ötlet toouse egy rögzített gazdagépet a fájlt, mert nem tudja, melyik csomópontok tooyour feladat fognak helyezkedni. Ebben az esetben írni a bash parancsfájlok burkolót **mpirun** toocreate hello állomás fájl automatikusan. Egy példa bash parancsfájl burkoló nevű hpcimpirun.sh hello Ez a cikk végén található, és mentse /openfoam/hpcimpirun.sh. Ez a példa parancsfájl hello a következő:
   
   1. Beállítja a hello környezeti változók **mpirun**, és néhány hozzáadása parancs paraméterei toorun hello MPI feladat hello RDMA a hálózaton keresztül. Ebben az esetben azt állítja be a következő változók hello:
      
      * I_MPI_FABRICS = shm:dapl
      * I_MPI_DAPL_PROVIDER bájtméretét-v2-ib0 =
      * I_MPI_DYNAMIC_CONNECTION = 0
   2. Létrehoz egy gazdagép szerint toohello környezeti változó $CCP_NODES_CORES hello HPC átjárócsomópont szerint beállított hello feladat aktiválásakor.
      
      $CCP_NODES_CORES hello formátuma ezt a mintát követi:
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      Ha
      
      * `<Number of nodes>`-csomópontok száma hello lefoglalt toothis feladat.  
      * `<Name of node_n_...>`-az egyes csomópontok hello neve lefoglalt toothis feladat.
      * `<Cores of node_n_...>`-hello hello csomópont lefoglalt toothis feladaton magok száma.
      
      Például ha két csomópont toorun igényeinek hello feladat $CCP_NODES_CORES hasonlít
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. Hívások hello **mpirun** parancsot, és hozzáfűzi a két paraméter toohello parancssor.
      
      * `--hostfile <hostfilepath>: <hostfilepath>`-hello elérési útja hello állomás hello parancsfájlját hoz létre.
      * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-olyan környezeti változó hello HPC Pack átjárócsomópont, amely tárolja a teljes magok száma hello által beállított lefoglalt toothis feladat. Ebben az esetben adja meg a folyamatok száma hello **mpirun**.

## <a name="submit-an-openfoam-job"></a>Egy OpenFOAM feladat elküldése
Most elküldheti a HPC Cluster Manager feladat. Szükség van toopass hello parancsfájl hpcimpirun.sh a hello parancssorokat hello feladat feladatokat.

1. Csatlakozás tooyour fürt átjárócsomópontjával, és indítsa el a HPC Cluster Manager.
2. **Az erőforrás-kezelés**, akkor győződjön meg arról, hogy hello Linux számítási csomópontok hello **Online** állapotát. Ha nem, válassza ki azokat, és kattintson a **online állapotba hozás**.
3. A **feladatkezelés**, kattintson a **új feladat**.
4. Írjon be egy feladat nevét, például *sloshingTank3D*.
   
   ![Feladat részletei][job_details]
5. A **feladat-erőforrások**, válassza ki az erőforrás "Csomópont" hello típusú, majd állítsa be a hello minimális too2. Ez a konfiguráció Linux két csomópont, amelyek mindegyikének nyolc processzormaggal ebben a példában hello feladatot futtat.
   
   ![Feladat erőforrások][job_resources]
6. Kattintson a **szerkesztése feladatok** a bal oldali navigációs hello, és kattintson a **Hozzáadás** tooadd egy feladat toohello feladat. Hello négy feladatok toohello feladat adja hozzá a következő parancssorok és a beállításokat.
   
   > [!NOTE]
   > Futó `source /openfoam/settings.sh` beállítja hello OpenFOAM és MPI futtatási környezet, így az egyes feladatok a következő hello meghívja az hello OpenFOAM parancs előtt.
   > 
   > 
   
   * **1. feladat**. Futtatás **decomposePar** toogenerate adatfájlok futtatásához **interDyMFoam** párhuzamosan.
     
     * Rendelje hozzá egy csomópont toohello feladat
     * **Parancssor** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
     * **Munkakönyvtár** -/ openfoam/sloshingTank3D
     
     Tekintse meg a következő ábra hello. Hasonlóképpen hello hátralévő feladatok konfigurálása.
     
     ![1. feladat részletei][task_details1]
   * **2. feladat**. Futtatás **interDyMFoam** párhuzamos toocompute hello mintában.
     
     * Két csomópont toohello feladatot
     * **Parancssor** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`
     * **Munkakönyvtár** -/ openfoam/sloshingTank3D
   * **3. feladat**. Futtatás **reconstructPar** toomerge hello beállítása idő könyvtárak minden processor_N_ könyvtárból egyetlen be.
     
     * Rendelje hozzá egy csomópont toohello feladat
     * **Parancssor** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`
     * **Munkakönyvtár** -/ openfoam/sloshingTank3D
   * **4. feladat**. Futtatás **foamToEnsight** párhuzamos tooconvert hello OpenFOAM eredmény fájlok EnSight formázása és hello EnSight fájlokat helyezze Ensight nevű hello eset könyvtárban.
     
     * Két csomópont toohello feladatot
     * **Parancssor** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`
     * **Munkakönyvtár** -/ openfoam/sloshingTank3D
7. Adja hozzá a függőségek toothese feladatok növekvő sorrendben feladat.
   
   ![Tevékenységfüggőségek][task_dependencies]
8. Kattintson a **Submit** toorun a feladatot.
   
   Alapértelmezés szerint a HPC Pack hello feladat, a jelenlegi bejelentkezett felhasználói fiók küldi el. Miután rákattintott **Submit**, megjelenhet egy tooenter hello felhasználónevet és jelszót kérő párbeszédpanel.
   
   ![Feladat hitelesítő adatai][creds]
   
   Bizonyos körülmények között a HPC Pack emlékszik hello felhasználói adatok előtt adjon meg, és nem jeleníti meg ezen a párbeszédpanelen. HPC Pack toomake újbóli, hello a következő parancsot a parancssorba írja be, és majd elküldeni a hello feladatot.
   
   ```
   hpccred delcreds
   ```
9. hello feladat telik el több tíz perc tooseveral órás toohello paraméterek hello minta beállítása alapján történik. Hello hőtérkép hello Linux csomópontokon futó hello feladat látható. 
   
   ![Hőtérkép][heat_map]
   
   Minden egyes csomóponton nyolc folyamatok indulnak el.
   
   ![Linux-folyamat][linux_processes]
10. Hello feladat befejezése után található hello feladat eredményeinek C:\OpenFoam\sloshingTank3D és hello naplófájlokat a következő C:\OpenFoam alapján mappákban.

## <a name="view-results-in-ensight"></a>EnSight az eredmények megtekintése
Ezenkívül szükség [EnSight](https://www.ceisoftware.com/) toovisualize és elemezze hello hello OpenFOAM feladat eredményét. További információt a képi megjelenítés és EnSight animációt megjelenik ez [videó útmutató](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1. Miután EnSight hello átjárócsomópont telepítette, indítsa el.
2. Nyissa meg a C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.
   
   Megjelenik egy tartály hello megjelenítőben.
   
   ![A EnSight tartály][tank]
3. Hozzon létre egy **Isosurface** a **internalMesh**, majd válassza a hello változó **alpha_water**.
   
   ![Hozzon létre egy isosurface][isosurface]
4. Állítsa be a hello színét **Isosurface_part** hello előző lépésben létrehozott. Például állítsa be toowater kék.
   
   ![Isosurface szín módosítása][isosurface_color]
5. Hozzon létre egy **Iso-kötet** a **falvastagságát** kiválasztásával **falvastagságát** a hello **részek** panelen, majd kattintson a hello **Isosurfaces**  hello eszköztár gombjára.
6. Hello párbeszédpanelen válassza ki a **típus** , **Isovolume** és állítsa be a Min hello **Isovolume tartomány** too0.5. toocreate hello isovolume, kattintson a **létrehozása a kijelölt alkotórészek**.
7. Állítsa be a hello színét **Iso_volume_part** hello előző lépésben létrehozott. Például állítsa be toodeep vízjel kék.
8. Állítsa be a hello színét **falvastagságát**. Például állítsa be azt tootransparent fehér.
9. Most kattintson **lejátszása** hello szimuláció toosee hello eredményeit.
   
    ![Tartály eredménye][tank_result]

## <a name="sample-files"></a>Mintafájlok
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>A minta XML konfigurációs fájl fürttelepítés PowerShell-parancsprogram
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
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
### <a name="sample-silentcfg-file-tooinstall-mpi"></a>A minta silent.cfg fájl tooinstall MPI
```
# Patterns used toocheck silent configuration file
#
# anythingpat - any string
# filepat     - hello file location pattern (/file/location/to/license.lic)
# lspat       - hello license server address pattern (0123@hostname)
# snpat       - hello serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components tooinstall, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path toohello cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes tooenable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes tooupdate ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Mintaparancsfájl settings.sh
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a>Mintaparancsfájl hpcimpirun.sh
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run hello mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into hello hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]:media/hpcpack-cluster-openfoam/keygen.png
[keys]:media/hpcpack-cluster-openfoam/keys.png
[step_variables]:media/hpcpack-cluster-openfoam/step_variables.png
[data_files]:media/hpcpack-cluster-openfoam/data_files.png
[decompose]:media/hpcpack-cluster-openfoam/decompose.png
[job_details]:media/hpcpack-cluster-openfoam/job_details.png
[job_resources]:media/hpcpack-cluster-openfoam/job_resources.png
[task_details1]:media/hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]:media/hpcpack-cluster-openfoam/task_dependencies.png
[creds]:media/hpcpack-cluster-openfoam/creds.png
[heat_map]:media/hpcpack-cluster-openfoam/heat_map.png
[tank]:media/hpcpack-cluster-openfoam/tank.png
[tank_result]:media/hpcpack-cluster-openfoam/tank_result.png
[isosurface]:media/hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]:media/hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]:media/hpcpack-cluster-openfoam/linux_processes.png
