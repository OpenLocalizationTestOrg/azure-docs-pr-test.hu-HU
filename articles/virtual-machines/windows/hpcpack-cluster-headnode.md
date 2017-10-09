---
title: "egy Azure virtuális gép egy HPC Pack átjárócsomópont aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure portál és hello erőforrás-kezelő telepítési modell toocreate a Microsoft HPC Pack 2012 R2 átjárócsomópont egy Azure virtuális Gépen."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: e6a13eaf-9124-47b4-8d75-2bc4672b8f21
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 3ddefb74b053a48a15f1ba1ca8edbc0192da51a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hello-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Egy Azure virtuális gép egy Piactéri lemezképhez hello átjárócsomópont HPC Pack fürt létrehozása
Használja a [Microsoft HPC Pack 2012 R2 virtuálisgép-lemezkép](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) csomópontjáról hello Azure piactér és hello Azure portál toocreate hello központi HPC-fürt. A HPC Pack Virtuálisgép-lemezkép a Windows Server 2012 R2 Datacenter HPC Pack 2012 R2 Update 3 előtelepített alapul. Az átjárócsomópont használata az Azure-ban HPC csomag koncepció telepítését igazolása. Felveheti a számítási csomópontok toohello toorun HPC munkaterhelések.

> [!TIP]
> toodeploy egy teljes HPC Pack 2012 R2-fürt hello átjárócsomópont és a számítási csomópontok az Azure-ban, azt javasoljuk, hogy egy automatizált módot használja. A választható lehetőségek hello [HPC Pack IaaS telepítési parancsfájl](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) és a Resource Manager-sablonok például hello [munkaterhelések Windows HPC Pack fürt](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/). Resource Manager-sablonok is elérhetők a [Microsoft HPC Pack 2016 fürtök](https://github.com/MsHpcPack/HPCPack2016/tree/master/newcluster-templates). 
> 
> 

## <a name="planning-considerations"></a>Tervezési megfontolások
Hello a következő ábrán az látható, hogy átjárócsomópont hello HPC Pack Azure virtuális hálózat az Active Directory-tartományhoz.

![HPC Pack átjárócsomópont][headnode]

* **Active Directory-tartomány**: hello HPC Pack 2012 R2 átjárócsomópont kell tooan Active Directory tartományhoz csatlakozott az Azure-ban hello HPC-szolgáltatások a virtuális gép hello megkezdése előtt. Ahogy az ebben a cikkben a koncepció telepítési igazolása előléptetheti hello virtuális Géphez létrejön a hello átjárócsomópont tartományvezérlőként hello HPC-szolgáltatások indítása előtt. Egy másik lehetőség toodeploy külön tartományvezérlő és csatlakoztatása az Azure toowhich erdőbe hello átjárócsomópont virtuális gép.

* **Telepítési modell**: új központi telepítésére, a Microsoft javasolja, hogy hello Resource Manager üzembe helyezési modellben. Ez a cikk feltételezi, hogy ezt a központi telepítési modellt használja.

* **Azure-beli virtuális hálózat**: hello erőforrás-kezelő telepítési modell toodeploy hello átjárócsomópont használata esetén adja meg, vagy hozzon létre egy Azure virtuális hálózatot. Virtuális hálózati hello használni, ha toojoin hello átjárócsomópont tooan meglévő Active Directory-tartományban van szüksége. Akkor is kell az újabb tooadd számítási csomópont virtuális gépek toohello fürt.

## <a name="steps-toocreate-hello-head-node"></a>Lépéseket toocreate hello átjárócsomópont
Az alábbiakban magas szintű lépései toouse hello Azure portál toocreate egy Azure virtuális Gépet a hello HPC Pack átjárócsomópont hello Resource Manager telepítési modell használatával. 

1. Ha azt szeretné, hogy egy új Active Directory-erdők az Azure-ban külön tartományvezérlő virtuális gépek toocreate, egy elem toouse egy [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc). A koncepció telepítési egyszerű igazolása rendelkezik finom tooomit ezt a lépést, és hello átjárócsomópont virtuális gépért konfigurálása tartományvezérlőként működik. Ez a beállítás később ebben a cikkben ismertetett.
2. A hello [HPC Pack 2012 R2, Windows Server 2012 R2 oldalon](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) hello Azure piactérről, kattintson **virtuális gép létrehozása**. 
3. A hello hello portálról **HPC Pack 2012 R2, Windows Server 2012 R2** lapra, jelölje be hello **erőforrás-kezelő** üzembe helyezési modellt, és kattintson **létrehozása**.
   
    ![HPC Pack kép][marketplace]
4. Hello portál tooconfigure hello beállításokat alkalmazza, és hozzon létre virtuális gép hello. Ha új tooAzure, az oktatóanyag hello [Windows virtuális gép létrehozása az Azure-portálon hello](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). A koncepció telepítési igazolása általában elfogadhatja az alapértelmezett hello, vagy ajánlott beállítások.
   
   > [!NOTE]
   > Ha azt szeretné, hogy a meglévő Azure Active Directory-tartomány toojoin hello átjárócsomópont tooan, győződjön meg arról hello virtuális gép létrehozásakor adja meg hello virtuális hálózati ehhez a tartományhoz.
   > 
   > 
5. Miután hello virtuális gép létrehozása és hello virtuális gép fut, [csatlakozzon a virtuális gép toohello](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) által a távoli asztal. 
6. Csatlakozás hello VM tooan Active Directory-tartomány erdő hello alábbi beállítások egyikének kiválasztásával:
   
   * Ha hello VM hozott létre egy Azure virtuális hálózatban egy meglévő tartomány erdő az, csatlakozás hello VM toohello erdő szabványos a Kiszolgálókezelő vagy a Windows PowerShell-eszközök segítségével. Indítsa újra.
   * Ha létrehozott egy új virtuális hálózat (nélkül egy meglévő tartomány erdő) hello VM, előreléphet hello VM tartományvezérlőként működik. Standard lépéseket tooinstall használja, és hello átjárócsomópont hello Active Directory tartományi szolgáltatások szerepkör konfigurálása. Részletes útmutató: [egy új Windows Server 2012 Active Directory-erdő telepítése](https://technet.microsoft.com/library/jj574166.aspx).
7. Hello virtuális gép fut, valamint illesztett tooan Active Directory-erdőben, után indítsa el az alábbiak szerint hello HPC Pack szolgáltatásokat:
   
    a. Csatlakozás toohello átjárócsomópont VM hello helyi Rendszergazdák csoport tagja a tartományi fiókkal. Például használja a hello rendszergazdai fiók beállítása hello átjárócsomópont virtuális gép létrehozása után.
   
    b. Alapértelmezett átjáró-csomóponti konfiguráció esetén indítsa el a Windows Powershellt rendszergazdaként, és írja be a következő hello:
   
    ```PowerShell
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```
   
    Hello HPC Pack szolgáltatások toostart több percet is igénybe vehet.
   
    Átjárócsomópont további konfigurációs beállítások, írja be a következőt `get-help HPCHNPrepare.ps1`.

## <a name="next-steps"></a>Következő lépések
* A HPC Pack fürt átjárócsomópontjához hello most dolgozhat. Például a HPC Cluster Manager és a teljes hello start [telepítési feladatlista](https://technet.microsoft.com/library/jj884141.aspx).
* Ha azt szeretné, hogy tooincrease hello fürt számítási kapacitás igény, vegye fel [Azure-kapacitásnövelés csomópontok](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) felhőszolgáltatásban. 
* Futtassa a test-munkaterhelések hello fürtön. Egy vonatkozó példáért lásd: hello HPC Pack [– első lépések útmutató](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/hpcpack-cluster-headnode/marketplace.png
