---
title: "HPC Pack hibrid mentése aaaSet fürtön, az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Microsoft HPC Pack és Azure tooset legfeljebb egy kisméretű, hibrid nagy teljesítményű számítástechnikai (HPC) fürt"
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: 5ad30d78dcd0c6a95d2a61f25015232edab3563c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>A hibrid nagy teljesítményű számítástechnikai igény Azure számítási csomópontok és a Microsoft HPC Pack (HPC) fürt beállítása
Használja a Microsoft HPC Pack 2012 R2 és az Azure tooset (HPC) fürt a számítástechnikai, small, hibrid nagy teljesítményű fel. Ez a cikk látható hello fürt tartalmaz egy helyszíni HPC Pack átjárócsomópont, és néhány számítási csomópont telepítése egy Azure-ban igény a felhőalapú szolgáltatás. Ezt követően futtathatja számítási feladatok hello hibrid fürtön.

![Hibrid HPC-fürt][Overview] 

Ez az oktatóanyag az egyik módszer, más néven a fürt "kapacitásnövelés toohello felhő," toouse méretezhető, igény szerinti Azure-erőforrások toorun számítási igényű alkalmazásokat.

Ez az oktatóanyag azt feltételezi, hogy a számítási fürtök vagy HPC Pack nincs korábbi tapasztalata kapcsolatban. Tervezett csak toohelp gyorsan bemutatási célokra hibrid számítási fürt telepítése is. További szempontok és lépések toodeploy HPC Pack hibrid fürtön, nagyobb léptékű termelési környezetben, vagy toouse HPC Pack 2016: hello [részletes útmutatást](http://go.microsoft.com/fwlink/p/?LinkID=200493). HPC Pack egyéb forgatókönyvek, beleértve a fürtöt tartalmazó környezetben az Azure virtuális gépeken automatikus című [HPC-fürt beállítások Microsoft HPC Pack az Azure-ban](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Előfeltételek
* **Azure-előfizetés** – Ha nem rendelkezik Azure-előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.
* **Egy Windows Server 2012 R2 vagy Windows Server 2012 rendszert futtató helyszíni számítógép** -hello hello HPC-fürt átjárócsomópontjához használni ezen a számítógépen. Ha már nem futtat Windows Server, töltse le és telepítse az [próbaverzió](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).
  
  * hello számítógép illesztett tooan Active Directory-tartományban kell lennie. Tesztelési célokra beállíthatja hello átjárócsomópont számítógépet tartományvezérlőként működik. tooadd hello Active Directory tartományi szolgáltatások kiszolgálói szerepkör, és léptesse elő hello átjárócsomópont számítógépet tartományvezérlőként, hello Windows Server dokumentációjában talál.
  * toosupport HPC Pack, hello operációs rendszer telepítenie kell az ezeken a nyelveken egyik: angol, japán vagy kínai (egyszerűsített).
  * Győződjön meg arról, hogy a fontos és kritikus frissítések telepítve vannak-e.
* **HPC Pack 2012 R2** - [letöltése](http://go.microsoft.com/fwlink/p/?linkid=328024) hello telepítési csomag legújabb verzióhoz hello ingyenes, a költség, és másolja hello fájlok toohello központi csomópont-számítógépén. Válassza ki a telepítési fájlok hello azonos nyelv, a Windows Server telepítése.

    >[!NOTE]
    > Ha azt szeretné, hogy a HPC Pack 2016 toouse helyett HPC Pack 2012 R2-ben, további konfigurációra van szükség. Lásd: hello [részletes útmutatást](http://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Tartományi fiók** -ennek a fióknak helyi rendszergazdai engedélyekkel hello átjárócsomópont tooinstall HPC Pack kell konfigurálni.
* **TCP-kapcsolatot a 443-as porton** a hello átjárócsomópont tooAzure.

## <a name="install-hpc-pack-on-hello-head-node"></a>HPC Pack hello átjárócsomópont telepítése
Először telepítenie kell a Microsoft HPC Pack a helyszíni Windows Server rendszert futtató számítógépen. Ez a számítógép hello hello fürt átjárócsomópontjához válik.

1. Jelentkezzen be toohello átjárócsomópont egy tartományi fiókkal, amely helyi rendszergazdai engedélyekkel rendelkezik.

2. Hello HPC Pack telepítési varázsló elindításához futtassa a Setup.exe hello HPC Pack telepítési fájlokból.

3. A hello **HPC Pack 2012 R2 telepítő** kattintson **új telepítést, vagy adja hozzá az új funkciók tooan példánya**.

    ![HPC Pack 2012 telepítése][install_hpc1]

4. A hello **Microsoft szoftver felhasználói oldal**, kattintson a **következő**.

5. A hello **telepítés típusának kiválasztása** kattintson **hozzon létre egy új HPC-fürtöt hozzon létre egy átjárócsomóponttal**, és kattintson a **következő**.

6. hello varázsló számos telepítés előtti tesztet futtatja. Kattintson a **következő** a hello **telepítési szabályokat** lapon, ha az összes teszt sikeresen lezajlott. Ellenkező esetben ellenőrizze a hello útmutatását, és végezze el a szükséges módosításokat a környezetben. Majd futtassa újra hello teszteket, vagy ha szükséges start újra hello-e telepítési varázsló.
7. A hello **HPC DB konfigurációs** lapon, győződjön meg arról, hogy **Átjárócsomópont** van kiválasztva a HPC-adatbázisok, és kattintson **következő**. 

    ![Adatbázis-konfiguráció][install_hpc4]

8. Fogadja el a hello hello varázsló fennmaradó oldalain alapértelmezett beállításokat. A hello **szükséges összetevők telepítése** kattintson **telepítése**.
   
    ![Telepítés][install_hpc6]

9. Hello telepítés befejezése után törölje a jelet **indítsa el a HPC Cluster Manager** majd **Befejezés**. (A start HPC Cluster Manager egy későbbi lépésben.)
   
    ![Befejezés][install_hpc7]

## <a name="prepare-hello-azure-subscription"></a>Készítse elő a hello Azure-előfizetés
Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com) Azure-előfizetéséhez. A lépések elvégzése után telepíthet hello helyszíni átjáró-csomóponti az Azure csomópontokat. 
  
  > [!NOTE]
  > Továbbá jegyezze fel a Azure-előfizetése Azonosítóját, ami később szüksége. A hello azonosító található **előfizetések** hello portálon.
  > 

### <a name="upload-hello-default-management-certificate"></a>Hello alapértelmezett felügyeleti tanúsítvány feltöltése
HPC Pack egy önaláírt tanúsítványt telepít hello átjárócsomópont, hello alapértelmezett Microsoft HPC Azure felügyeleti tanúsítvány, amely feltöltheti az Azure felügyeleti tanúsítvány neve. Ez a tanúsítvány előírt tesztelése, és a koncepció igazolása központi telepítések toosecure hello közötti kapcsolat hello átjárócsomópont és az Azure.

1. Hello átjárócsomópont számítógépről toohello bejelentkezés [Azure-portálon](https://portal.azure.com).

2. Kattintson a **előfizetések** > *your_subscription_name*.

3. Kattintson a **felügyeleti tanúsítványok** > **feltöltése**.4. Keresse meg a hello fájl C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer hello központi csomóponton. Kattintson a **feltöltése**.

   
Hello **alapértelmezett HPC Azure felügyeleti** tanúsítvány megjelenik a felügyeleti tanúsítványok hello listájában.

### <a name="create-an-azure-cloud-service"></a>Azure-felhőszolgáltatás létrehozása
> [!NOTE]
> A legjobb teljesítmény érdekében hozzon létre hello felhőszolgáltatás és a storage-fiók hello (egy későbbi lépésben) hello ugyanabban a földrajzi régióban.
> 
> 

1. Hello portálon kattintson **Felhőszolgáltatásokhoz (klasszikus)** > **+ Hozzáadás**.

2.  Hello szolgáltatás DNS-nevet, válasszon egy erőforráscsoportot és egy helyet, és kattintson **létrehozása**.


### <a name="create-an-azure-storage-account"></a>Azure-tárfiók létrehozása
1. Hello portálon kattintson **tárfiókok (klasszikus)** > **+ Hozzáadás**.

2. Írja be a hello fiók nevét, majd válassza ki a hello **klasszikus** üzembe helyezési modellben.

3. Válasszon egy erőforráscsoportot és egy helyet, és egyéb beállításokat hagyja alapértelmezett értékeket. Ezt követően kattintson a **Create** (Létrehozás) gombra.

## <a name="configure-hello-head-node"></a>Átjárócsomópont hello konfigurálása
HPC Fürtkezelő toodeploy toouse Azure-csomópontok és toosubmit feladatok, először hajtsa végre néhány szükséges konfigurációs lépéseket.

1. Indítsa el a HPC Fürtkezelő hello központi csomópontján. Ha hello **Head csomópont kiválasztása** párbeszédpanel, kattintson a **helyi számítógép**. Hello **telepítési feladatlista** jelenik meg.

2. A **szükséges telepítési feladatok**, kattintson a **a hálózat konfigurálásához**.
   
    ![Hálózat konfigurálása][config_hpc2]

3. Hello hálózat beállítása varázsló, jelölje ki **minden csomópont csak a vállalati hálózaton** (topológia 5). Ez a hálózati konfiguráció hello legegyszerűbb bemutatási célokra.
   
    ![5 topológia][config_hpc3]
   
4. Kattintson a **következő** hello varázsló lapjain fennmaradó hello tooaccept alapértelmezett értékét. Ezután a hello **felülvizsgálati** lapra, majd **konfigurálása** toocomplete hello hálózati konfiguráció.

5. A hello **telepítési feladatlista**, kattintson a **telepítési hitelesítő adatok**.

6. A hello **telepítés hitelesítő adatainál** párbeszédpanelen hello tartományi fiók, amellyel tooinstall HPC Pack típus hello hitelesítő adatait. Ezután kattintson az **OK** gombra. 
   
    ![Telepítési hitelesítő adatok][config_hpc6]
   
7. A hello **telepítési feladatlista**, kattintson a **hello elnevezési új csomópontok konfigurálása**.

8. A hello **csomópont adja meg az adatsorozat elnevezési** párbeszédpanel mezőben fogadja el a hello alapértelmezett névhasználati a sorozat, és kattintson **OK**. E lépés elvégzése után, annak ellenére, hogy hello ebben az oktatóanyagban hozzá Azure-csomópontok megnevezett automatikusan.
   
    ![Csomópont elnevezése][config_hpc8]
   
9. A hello **telepítési feladatlista**, kattintson a **csomópont-sablon létrehozása**. Hello oktatóanyag későbbi részében hello csomópont sablon tooadd Azure-csomópontok toohello fürtöt használ.

10. A csomópont-sablon létrehozása varázsló hello, hajtsa végre a következő hello:
    
    a. A hello **csomópont-sablon típusának kiválasztása** kattintson **csomópontsablonhoz a Windows Azure**, és kattintson a **következő**.
    
    ![Csomópont sablon][config_hpc10]
    
    b. Kattintson a **következő** tooaccept hello alapértelmezett nevet.
    
    c. A hello **előfizetés adatainak megadása** lapján adja meg az Azure előfizetés-Azonosítóval (az Azure-fiók adatainak érhető el). Ezt követően a **felügyeleti tanúsítvány**, tallózással keresse meg **alapértelmezett Microsoft HPC Azure Management.** Ezután kattintson a **Next** (Tovább) gombra.
    
    ![Csomópont sablon][config_hpc12]
    
    d. A hello **szolgáltatás adatainak megadása** lap, válassza hello felhőalapú szolgáltatás és az előző lépésben létrehozott tárfiók hello. Ezután kattintson a **Next** (Tovább) gombra.
    
    ![Csomópont sablon][config_hpc13]
    
    e. Kattintson a **következő** hello varázsló lapjain fennmaradó hello tooaccept alapértelmezett értékét. Ezután a hello **felülvizsgálati** lapra, majd **létrehozása** toocreate hello csomópont sablont.
    
    > [!NOTE]
    > Alapértelmezés szerint hello Azure csomópont sablon beállításait tartalmazza (kiépíteni) toostart és stop hello csomópontok manuálisan, HPC Cluster Manager használatával. Egy ütemezés toostart igény szerint konfigurálható és az Azure-csomópontok automatikusan hello leállítása.
    > 
    > 

## <a name="add-azure-nodes-toohello-cluster"></a>Azure-csomópontok toohello fürt hozzáadása
Most hello csomópont sablon tooadd Azure-csomópontok toohello fürt használatára. Hello csomópontok toohello fürt tárolja a konfigurációs adatokat, így a elindíthatja (kiépíteni) hozzáadása hello felhőszolgáltatáshoz bármikor őket. Az előfizetés csak lekérdezi többletfizetésre volna szükség Azure-csomópontok hello hello felhőszolgáltatásban runbookpéldányok futása után.

Kövesse a lépéseket tooadd két kis csomópont.

1. A HPC-kezelőben kattintson **csomópont-felügyelet** (nevű **erőforrás-kezelés** HPC Pack aktuális verziói) > **csomópont hozzáadása**.
   
    ![Csomópont hozzáadása][add_node1]

2. A hello csomópont hozzáadása varázsló, a hello **módszer kiválasztása** kattintson **hozzáadása Windows Azure-csomópontokra**, és kattintson a **következő**.
   
    ![Az Azure csomópont hozzáadása][add_node1_1]

3. A hello **adja meg az új csomópontok** lapra, jelölje be hello Azure csomópontsablonhoz a korábban létrehozott (alapértelmezés szerint nevű **alapértelmezett AzureNode sablon**). Adja meg **2** méretű csomópontok **kis**, és kattintson a **következő**.
   
    ![Adja meg a csomópontok][add_node2]
   
4. A hello **befejezése hello csomópont hozzáadása varázsló** kattintson **Befejezés**.
    
     Két Azure-csomópontok, nevű **AzureCN-0001** és **AzureCN-0002**, mostantól a HPC Cluster Manager jelenik meg. Mindkettő a hello **nem telepített** állapotát.
   
    ![Felvett csomópontjain][add_node3]

## <a name="start-hello-azure-nodes"></a>Indítsa el az Azure csomópontok hello
Ha azt szeretné, toouse hello fürterőforrások HPC Cluster Manager toostart (kiépíteni) használja, az Azure-ban Azure-csomópontok hello és online állapotba kerüljön.

1. A HPC-kezelőben kattintson **csomópont-felügyelet** (nevű **erőforrás-kezelés** HPC Pack aktuális verziói), és válassza ki az Azure-csomópontok hello.

2. Kattintson a **Start**, és kattintson a **OK**.
   
   ![Kiinduló csomópont][add_node4]
   
    hello csomópontok átmenet toohello **kiépítési** állapotát. Nézet hello kiépítés napló tootrack hello kiépítés folyamatban van.
   
    ![Kiépítés csomópontok][add_node6]

3. Néhány perc elteltével hello Azure-csomópontok üzembe helyezése és a hello **Offline** állapotát. Ebben az állapotban lévő hello szerepkörpéldányokat futnak, de még nem a fürt feladatok fogadására.

4. amely a szerepkörpéldányok hello tooconfirm futnak, hello Azure-portálon, kattintson a **Felhőszolgáltatások (klasszikus)** > *your_cloud_service_name*.
   
   Két kell megjelennie **HpcWorkerRole** hello szolgáltatásban futó példányok (csomópontok). HPC Pack is automatikusan telepíti a két **HpcProxy** (közepes méretű) toohandle kommunikációját hello átjárócsomópont és az Azure-példányok.

   ![Futó példányát tekintve][view_instances1]

5. toobring hello Azure-csomópontok online toorun feladatok, jelölje be hello csomópontot, kattintson a jobb gombbal, és kattintson a fürt **online állapotba hozás**.
   
    ![Kapcsolat nélküli csomópontok][add_node7]
   
    HPC Fürtkezelő azt jelzi, hogy hello csomópontok a hello **Online** állapotát.

## <a name="run-a-command-across-hello-cluster"></a>A parancsok a hello fürtön
toocheck hello telepítése, használata hello HPC Pack **clusrun** parancs toorun parancsot vagy az alkalmazás egy vagy több fürtcsomóponton. Egyszerű példaként használható **clusrun** tooget hello IP-konfigurációja, hello Azure-csomópontok.

1. A hello átjárócsomópont nyisson meg egy parancssort rendszergazdaként.

2. Írja be a következő parancs hello:
   
    `clusrun /nodes:azurecn* ipconfig`

3. Ha a rendszer kéri, adja meg a fürt rendszergazdai jelszót. Meg kell jelennie a parancskimeneti hasonló toohello következő.
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a>Egy tesztelési feladat futtatása
Most hello hibrid fürtön futó teszt feladat elküldése. Ebben a példában egy egyszerű paraméteres ismétlés feladat (belsőleg párhuzamos számítási típusú). Ebben a példában fut, adja hozzá az egész tooitself hello segítségével résztevékenység **/a beállítása** parancsot. Hello fürt összes csomópontjának hello toofinishing hello résztevékenység az egész számok 1 too100 a függ.

1. A HPC-kezelőben kattintson **feladatkezelés** > **új paraméteres Sweep feladat**.
   
    ![Új feladat][test_job1]

2. A hello **új paraméteres Sweep feladat** párbeszédpanel **parancssori**, típus `set /a *+*` (felülírja a hello alapértelmezett parancssort, amely akkor jelenik meg). Hagyja a fennmaradó beállításokat hello tartozó alapértelmezett értékeket, és kattintson **Submit** toosubmit hello feladat.
   
    ![Paraméteres ismétlés][param_sweep1]

3. Ha hello feladat befejeződött, kattintson duplán a hello **saját Sweep feladat** feladat.

4. Kattintson a **nézet feladatai**, majd kattintson az adott részfeladatnál annak regisztrálása egy részfeladatnál annak regisztrálása tooview számított hello kimenetét.
   
    ![A feladat eredménye][view_job361]

5. melyik csomópontján hello számítási elvégzi a részfeladatnál annak regisztrálása, toosee kattintson **lefoglalt csomópontok**. (A fürt előfordulhat, hogy megjelenítése egy másik csomópont neve).
   
    ![A feladat eredménye][view_job362]

## <a name="stop-hello-azure-nodes"></a>Állítsa le az Azure csomópontok hello
Hello fürt kipróbálhatja, hello Azure-csomópontok tooavoid felesleges költségek tooyour fiók le. Ebben a lépésben hello felhőalapú szolgáltatás leáll, és eltávolítja a hello Azure szerepkörpéldányokat.

1. A HPC Cluster Manager, a **csomópont-felügyelet** (nevű **erőforrás-kezelés** HPC Pack korábbi verzióiban), válassza ki mindkét Azure-csomópontok. Kattintson a **leállítása**.
   
    ![Csomópont leállítása][stop_node1]

2. A hello **állítsa le a Windows Azure-csomópontokra** párbeszédpanel, kattintson a **leállítása**. 
   
3. hello csomópontok átmenet toohello **leállítása** állapotát. Néhány perc elteltével HPC Cluster Manager azt mutatja, hogy hello csomópontok **nem telepített**.
   
    ![Nem telepített csomópontok][stop_node4]

4. hello szerepkörpéldányokat már nem az Azure, a futó tooconfirm hello Azure portálon kattintson **Felhőszolgáltatásokhoz (klasszikus)** > *your_cloud_service_name*. Nincs példány hello éles környezetben vannak telepítve. 
   
    Ezzel befejezte a hello oktatóanyag.

## <a name="next-steps"></a>Következő lépések
* Hello dokumentációja felfedezés [HPC Pack](https://technet.microsoft.com/library/cc514029).
* HPC Pack fürtöt tartalmazó környezetben, nagyobb léptékű hibrid mentése tooset lásd: [szerepkör Feldolgozópéldányok tooAzure Microsoft HPC Pack kapacitásnövelés](http://go.microsoft.com/fwlink/p/?LinkID=200493).
* Más módokon toocreate egy HPC Pack fürthöz az Azure-ban, beleértve az Azure Resource Manager-sablonokkal, lásd: [HPC-fürt beállítások Microsoft HPC Pack az Azure-ban](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
