---
title: "aaaAdd kapacitásnövelés csomópontok tooan HPC Pack fürt |} Microsoft Docs"
description: "Ismerje meg, hogyan tooexpand egy HPC Pack fürthöz Azure igény szerinti hozzáadásával felhőszolgáltatásban fut szerepkör feldolgozópéldányok"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 24b79a8a-24ad-4002-ae76-75abc9b28c83
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 7ec40ffe76485742c9e458ec49e11805990974e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-on-demand-burst-nodes-tooan-hpc-pack-cluster-in-azure"></a>Igény szerinti "kapacitásnövelés" csomópontok tooan HPC Pack fürt hozzáadása az Azure-ban
Ha úgy konfigurálja a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) fürt az Azure-érdemes tooquickly méretezési hello fürt kapacitás felfelé vagy lefelé, egy fenntartása nélkül előre konfigurált virtuális gépek számítási csomópont úgy. A cikkből megtudhatja, hogyan igény tooadd "kapacitásnövelés" csomópontok (feldolgozói szerepkör példányok felhőszolgáltatásban fut), a számítási erőforrások tooa átjárócsomópont az Azure-ban. 

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

![Kapacitásnövelés csomópontok][burst]

a cikkben ismertetett hello segítségével gyorsan hozzá Azure-csomópontok tooa felhőalapú HPC Pack átjárócsomópont VM-teszt során vagy a koncepció igazolása központi telepítés. hello magas szintű lépéseket kell hello azonos a hello lépéseket túl "kapacitásnövelés tooAzure" tooadd felhő számítási kapacitás tooan helyszíni HPC Pack fürt. Az oktatóanyagok esetén lásd: [Microsoft HPC Pack hibrid számítási fürt beállítása](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Részletes útmutatást és az üzemi környezetek szempontokat, [tooAzure Microsoft HPC Pack kapacitásnövelés](https://technet.microsoft.com/library/gg481749.aspx).

## <a name="prerequisites"></a>Előfeltételek
* **Egy Azure virtuális Gépen telepített HPC Pack átjárócsomópont** -önálló átjárócsomópont virtuális gép vagy egy, automatikusan nagyobb fürt része. egy önálló átjárócsomópont toocreate lásd [központi telepítése egy Azure virtuális gép egy HPC Pack Átjárócsomópont](../../virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Automatizált HPC Pack fürt telepítési lehetőségeket, lásd: [toocreate lehetőségek és az Azure-ban a Microsoft HPC Pack egy Windows HPC-fürt kezeléséhez](../../virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
  > [!TIP]
  > Ha hello [HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md) toocreate hello fürtön, az Azure-ban, az Azure-kapacitásnövelés csomópontok az automatikus telepítési felvehető. Hello példák cikkben.
  > 
  > 
* **Azure-előfizetés** -tooadd Azure csomópontok, választhat hello azonos használt előfizetés toodeploy hello átjárócsomópont VM, vagy egy másik előfizetést (vagy előfizetések).
* **Magok kvóta** -szükség lehet tooincrease hello beállított kvótát mag, különösen akkor, ha több Azure csomópontokat a multicore méretek toodeploy választja. a kvóta tooincrease [nyissa meg az online támogatás ügyfélkérés](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) díjmentesen.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-hello-azure-nodes"></a>1. lépés: Hozzon létre egy felhőalapú szolgáltatás és a tárfiók hello Azure-csomópontok
Használja a klasszikus Azure portálon hello vagy egyenértékű eszközök tooconfigure hello szükséges toodeploy erőforrásokat a következő az Azure-csomópontok:

* Egy új Azure-felhőszolgáltatásban
* Egy új Azure storage-fiók

> [!NOTE]
> Az előfizetéshez létező felhőalapú szolgáltatást nem használja fel. 
> 
> 

**Megfontolások**

* Minden Azure csomópont sablon toocreate megtervezni külön felhőszolgáltatás konfigurálása. Azonban használhatja ugyanazt a tárfiókot több csomópont sablonok hello.
* Azt javasoljuk, hogy megtalálta hello felhőszolgáltatás és a tárfiók hello hello központi telepítés a hello azonos Azure-régiót.

## <a name="step-2-configure-an-azure-management-certificate"></a>2. lépés: Az Azure felügyeleti tanúsítvány konfigurálása
tooadd Azure csomópontok a számítási erőforrásokat, tanúsítványra van szükség a felügyeleti hello átjárócsomópont és feltöltés toohello hello központi telepítéshez használt Azure-előfizetéshez tartozó tanúsítvány.

Ehhez a forgatókönyvhöz kiválaszthatja hello **alapértelmezett HPC Azure felügyeleti tanúsítvánnyal** HPC Pack telepítését, és automatikusan konfigurálja a központi csomóponton. Ez a tanúsítvány akkor hasznos, tesztelési célokra és a koncepció igazolása központi telepítéseket. toouse a tanúsítvány, a fájl C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer hello központi csomópontról VM toothe előfizetés feltöltése. hello tooupload hello tanúsítványt [a klasszikus Azure portálon](https://manage.windowsazure.com), kattintson a **beállítások** > **felügyeleti tanúsítványok**.

További beállítások tooconfigure hello felügyeleti tanúsítványt, lásd: [forgatókönyvek tooConfigure hello Azure felügyeleti tanúsítvánnyal Azure-kapacitásnövelés a telepítések](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-toohello-cluster"></a>3. lépés: Azure-csomópontok toohello fürt központi telepítése
hello lépéseket tooadd, és indítsa el az Azure ebben a forgatókönyvben csomópontok általában vannak hello azonos hello lépések egy a helyszíni központi csomóponttal. További információkért lásd: a következő szakaszok a hello [tooDeploy Azure csomópontokat a Microsoft HPC Pack lépések](https://technet.microsoft.com/library/gg481758.aspx):

* Egy Azure csomópont-sablon létrehozása
* Azure-csomópontok toohello Windows HPC-fürt hozzáadása
* Indítsa el a (kiépíteni) hello Azure-csomópontok

Miután hozzáadása, és indítsa el a hello csomópontok, azok készen állnak toouse toorun fürt feladatok.

Ha Azure-csomópontok telepítésekor problémákat tapasztal, tekintse meg a [hibaelhárítása központi telepítések az Azure csomópontokat a Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Következő lépések
* a számítási igényű példányméretének hello toouse kapacitásnövelés csomópontok hello szempontok című [nagy teljesítményű számítási Virtuálisgép-méretek](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Ha azt szeretné, hogy automatikusan növekedhet és csökkenhet hello Azure számítási erőforrások szerint hello fürtmunkaterhelés című [automatikusan nő, és az Azure számítási erőforrásokat HPC Pack-fürtben lévő zsugorítása](hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/hpcpack-cluster-node-burst/burst.png
