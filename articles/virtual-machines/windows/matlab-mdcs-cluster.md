---
title: "a virtuális gépek fürtök aaaMATLAB |} Microsoft Docs"
description: "Használja a Microsoft Azure virtuális gépek toocreate MATLAB elosztott számítástechnikai kiszolgáló fürtök toorun a számítási igényű párhuzamos MATLAB munkaterhelések"
services: virtual-machines-windows
documentationcenter: 
author: mscurrell
manager: timlt
editor: 
ms.assetid: e9980ce9-124a-41f1-b9ec-f444c8ea5c72
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Windows
ms.workload: infrastructure-services
ms.date: 05/09/2016
ms.author: markscu
ms.openlocfilehash: 266749dbdcfefac42c94b74aa612bfee18075a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Azure virtuális gépeken MATLAB elosztott számítási fürtök létrehozása
Használja a Microsoft Azure virtuális gépek toocreate egy vagy több MATLAB elosztott számítási fürtök toorun a számítási igényű párhuzamos MATLAB munkaterhelések. Telepítse a MATLAB elosztott számítástechnikai kiszolgáló szoftvert egy virtuális gép toouse alapjául szolgáló lemezképhez, és egy Azure gyors üzembe helyezés-sablon vagy az Azure PowerShell-parancsfájl használata (a rendelkezésre álló [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) toodeploy hello fürthöz, és kezelheti. A központi telepítést követően a munkaterhelések toohello fürt toorun csatlakozzon.

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>MATLAB és MATLAB elosztott számítástechnikai kiszolgáló
Hello [MATLAB](http://www.mathworks.com/products/matlab/) platform mérnöki és tudományos problémák megoldásához van optimalizálva. A nagyméretű szimulációja és adatfeldolgozási feladatok MATLAB felhasználók használhatják a rács szolgáltatások és számítási fürtök kihasználásával a termékek toospeed fel a számítási-igényes munkaterhelések számítástechnikai MathWorks párhuzamos. [A számítástechnikai eszközkészlet párhuzamos](http://www.mathworks.com/products/parallel-computing/) MATLAB felhasználók alkalmazások parallelize és előnyeit több magos processzorral, Feldolgozóegységekkel, és számítási fürtök. [MATLAB elosztott számítástechnikai kiszolgáló](http://www.mathworks.com/products/distriben/) lehetővé teszi, hogy a felhasználók tooutilize MATLAB sok számítógép egy számítási fürtben.

Az Azure virtuális gépek használatával, amely az összes MATLAB elosztott számítástechnikai kiszolgáló tárolófürtöket hozhat létre ugyanazon mechanizmusok elérhető toosubmit párhuzamos munkahelyi interaktív feladatok, kötegelt feladatok, független feladatokat, például a helyszíni fürtökként hello és kommunikáció a feladatokat. Számos előnyt képest tooprovisioning Azure hello MATLAB platform együtt történő rendelkezik, és a hagyományos helyszíni hardver: számos különböző virtuális gép méretének, fürtök igény, csak fizetnie hello számítási erőforrásokat használ, létrehozását és hello képességét tootest modellek méretekben.  

## <a name="prerequisites"></a>Előfeltételek
* **Ügyfélszámítógép** -telepítést követően egy ügyfél Windows-alapú számítógép toocommunicate Azure és a hello MATLAB elosztott számítástechnikai kiszolgálófürt lesz szüksége.
* **Az Azure PowerShell** -lásd [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) tooinstall azt az ügyfélszámítógépen.
* **Azure-előfizetés** – Ha nem rendelkezik előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig. Nagyobb fürtök esetén fontolja meg a használatalapú előfizetés vagy egyéb beszerzési lehetőségek.
* **Magok kvóta** -tooincrease hello core kvóta toodeploy nagy fürt és MATLAB elosztott számítástechnikai kiszolgáló egynél több fürt szükséges. a kvóta tooincrease [nyissa meg az online támogatás ügyfélkérés](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) díjmentesen.
* **MATLAB, párhuzamos számítástechnikai eszközkészlet és MATLAB elosztott számítástechnikai kiszolgáló licencek** -hello parancsfájlok azt feltételezik, hogy hello [MathWorks üzemeltetett Licenckezelő](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) használt összes licencet.  
* **MATLAB elosztott számítástechnikai kiszolgáló szoftver** -, amely a virtuális gépek hello fürt hello alapszintű Virtuálisgép-lemezkép lesz lesz telepítve.

## <a name="high-level-steps"></a>Magas szintű lépései
toouse Azure virtuális gépek a MATLAB elosztott számítástechnikai kiszolgálófürtök, hello következő magas szintű lépések szükségesek. Részletes utasításokat a hello gyors üzembe helyezés sablont és parancsfájlokat kísérő hello dokumentációjának vannak [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Alapszintű VM-lemezkép létrehozása**  

   * Töltse le és telepítse a virtuális gép MATLAB elosztott számítástechnikai kiszolgáló szoftvereket.

     > [!NOTE]
     > A folyamat eltarthat néhány óra múlva, de csak akkor kell toodo azt követően minden MATLAB verzióját használja.   
     >
     >
2. **Egy vagy több fürtök létrehozása**  

   * PowerShell-parancsfájl a megadott hello, vagy használjon hello gyors üzembe helyezés sablon toocreate hello alapszintű Virtuálisgép-lemezkép a fürtöt.   
   * Megadott hello PowerShell-parancsfájlt, amely lehetővé teszi a toolist, szüneteltetése, folytatása és delete fürtök hello-fürtök kezelése.

## <a name="cluster-configurations"></a>Fürtkonfigurációk
Jelenleg hello fürt létrehozási parancsprogrammal és a sablon lehetővé teszi egy egyetlen MATLAB elosztott számítástechnikai Kiszolgálótopológia toocreate. Ha azt szeretné, hozzon létre egy vagy több további fürtök, a virtuális gépek, a másik Virtuálisgép-méretek használatával worker másik számmal rendelkező fürtökön, és így tovább.

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB ügyfél és a fürt az Azure-ban
hello MATLAB ügyfél-csomópontnak, MATLAB Feladatütemező csomópont és MATLAB elosztott számítástechnikai kiszolgáló "munkavégző" csomópontok összes beállításuk Azure virtuális gépek virtuális hálózatban, ahogy az alábbi hello. ábra.


* toouse hello fürt esetén a távoli asztal toohello ügyfél-csomópontnak kapcsolódnak. ügyfél-csomópontnak hello hello MATLAB ügyfél futtatja.
* hello ügyfél fürtcsomópont egy fájlmegosztást, amely minden dolgozók is elérhetők.
* MathWorks üzemeltetett Licenckezelő hello licenc ellenőrzi az összes MATLAB szoftver használható.
* Alapértelmezés szerint egy MATLAB elosztott számítástechnikai kiszolgáló munkavégző / core hello munkavégző virtuális gépek jön létre, de megadhat több.

## <a name="use-an-azure-based-cluster"></a>Azure-alapú fürt használatára
Csakúgy, mint a más típusú MATLAB elosztott számítástechnikai kiszolgálófürtök toouse hello profil kezelő hello MATLAB (ügyfélen hello VM) ügyfél toocreate MATLAB Feladatütemező fürt profil a kell.

![Profil kezelő](./media/matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Következő lépések
* A részletes utasításokat toodeploy és MATLAB elosztott számítástechnikai kiszolgálófürtök kezelése az Azure-ban, tekintse meg a hello [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) hello sablonok és a parancsfájlok tartalmazó tárházba.
* Nyissa meg toohello [MathWorks hely](http://www.mathworks.com/) MATLAB és MATLAB elosztott számítástechnikai kiszolgálóra vonatkozó részletes dokumentációt.
