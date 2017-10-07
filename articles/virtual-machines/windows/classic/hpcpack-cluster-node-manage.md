---
title: "a számítási csomópontok aaaManage HPC Pack fürt |} Microsoft Docs"
description: "További tudnivalók a PowerShell parancsfájl eszközök tooadd, távolítsa el, kezdő, és állítsa le a számítási fürtcsomópontok HPC Pack 2012 R2-ben az Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 5ac1142cc5da984020779434fbb7cba5ad7c14bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Hello száma és a számítási csomópontok HPC Pack fürtben az Azure-ban rendelkezésre állásának kezelése
Ha létrehozott egy HPC Pack 2012 R2-fürt az Azure virtuális gépeken, érdemes módon tooeasily hozzáadása, eltávolítása, indítsa el a (kiépíteni), vagy állítsa le a (deprovision) néhány számítási csomópont virtuális gépek a fürt. toodo ezeket a feladatokat hello átjárócsomópont VM telepített Azure PowerShell-parancsfájlok futtatása. Ezek a parancsfájlok segítségével szabályozható hello száma és a HPC Pack fürterőforrások rendelkezésre állását, szabályozhatja költségeit.

> [!IMPORTANT] 
> Ez a cikk csak tooHPC Pack 2012 R2-fürtöket helyez hello klasszikus telepítési modellel készült Azure vonatkozik. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.
> Ebben a cikkben ismertetett PowerShell-parancsfájlok hello emellett HPC Pack 2016 nem érhetők el.

## <a name="prerequisites"></a>Előfeltételek
* **Az Azure virtuális gépeken HPC Pack 2012 R2-fürt**: hozzon létre egy HPC Pack 2012 R2-fürt hello klasszikus üzembe helyezési modellben. Például automatizálható hello telepítési hello Azure piactér hello HPC Pack 2012 R2 Virtuálisgép-lemezképet és az Azure PowerShell-parancsfájl használatával. További információkat és előfeltételeket: [HPC-fürt létrehozása hello HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md).
  
    A központi telepítést követően talál hello csomópont felügyeleti parancsfájlok hello % CCP\_OTTHONI % bin mappájában hello átjárócsomópont. Futtassa az egyes hello parancsfájlok rendszergazdaként.
* **Azure Közzététel beállításai fájl vagy a felügyeleti tanúsítvány**: hello átjárócsomópont hello következő toodo van szüksége:
  
  * **Importálás hello Azure közzététele beállításfájl**. toodo a, futtatási hello Azure PowerShell-parancsmagok hello központi csomópontján a következő:
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * **Hello Azure felügyeleti tanúsítvány konfigurálása hello átjárócsomópont**. Ha hello .cer fájlt, importálja azt az hello CurrentUser\My tanúsítványtárolójába, és futtassa a következő Azure PowerShell-parancsmaggal (AzureCloud vagy AzureChinaCloud) az Azure környezetben hello:
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Virtuális gépek számítási csomópont hozzáadása
Adja hozzá a számítási csomópontokat a hello **Add-HpcIaaSNode.ps1** parancsfájl.

### <a name="syntax"></a>Szintaxis
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Paraméterek
* **Szolgáltatásnév**: hello felhőszolgáltatás, amely új számítási csomópont virtuális gépek neve kerülnek.
* **ImageName**: Azure virtuális gép lemezképének nevét, amely hello a klasszikus Azure portálon vagy az Azure PowerShell-parancsmag érhető el **Get-AzureVMImage**. hello kép hello követelményeknek kell megfelelniük:
  
  1. A Windows operációs rendszert kell telepíteni.
  2. HPC Pack hello számítási csomópont szerepkör telepítve kell lennie.
  3. hello kép hello felhasználói kategória nem egy nyilvános Azure Virtuálisgép-lemezkép titkos képnek kell lennie.
* **Mennyiség**: számítási csomópont virtuális gépek toobe hozzáadott száma.
* **InstanceSize**: hello mérete számítási csomópont virtuális gépeket.
* **DomainUserName**: tartományi felhasználó nevét, amely használt toojoin hello új virtuális gépek toohello tartomány.
* **DomainUserPassword**: hello tartományi felhasználó jelszavát.
* **NodeNameSeries** (nem kötelező): hello mintát elnevezési számítási csomópontok. hello formátumban kell megadni &lt; *legfelső szintű\_neve*&gt;&lt;*Start\_szám*&gt;%. Például MyCN % 10 %: azon hello számítási csomópont nevének MyCN11 kezdve. Ha nincs megadva, a hello parancsfájl által használt hello csomópont elnevezési adatsorozat konfigurált hello HPC-fürtben.

### <a name="example"></a>Példa
hello következő példakóddal felveheti a 20 mérete nagy számítási csomópont virtuális gépek hello felhőszolgáltatásban *hpcservice1*hello Virtuálisgép-lemezkép alapján *hpccnimage1*.

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Távolítsa el a virtuális gépek számítási csomópont
Távolítsa el a számítási csomópontokat a hello **Remove-HpcIaaSNode.ps1** parancsfájl.

### <a name="syntax"></a>Szintaxis
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Paraméterek
* **Név**: fürt csomópontjai toobe nevek. Helyettesítő karakterek használata támogatott. hello paraméter beállított értéke nevét. Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.
* **Csomópont**: hello HpcNode objektumot hello csomópontok toobe eltávolítva, mely a HPC PowerShell-parancsmaggal hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). hello paraméter beállított értéke csomópont. Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.
* **DeleteVHD** (nem kötelező): toodelete tartozó hello lemezeket a virtuális gépek eltávolításra hello beállítása.
* **Kényszerített** (nem kötelező): kapcsolat nélküli HPC-csomópontok tooforce beállítása előtt távolítsa el.
* **Erősítse meg** (nem kötelező): Rákérdezés a hello parancs végrehajtása előtt.
* **WhatIf**: hello parancs hajtja végre ténylegesen a hello parancs végrehajtása nélkül toodescribe beállítása, mi történne.

### <a name="example"></a>Példa
hello alábbi példa arra kényszeríti a kapcsolat nélküli hello csomópontok kezdődő nevű *HPCNode-CN -* és őket hello csomópontok és a kapcsolódó lemezek távolítja el.

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Indítsa el a virtuális gépek számítási csomópont
Kezdő számítási csomópontokat a hello **Start-HpcIaaSNode.ps1** parancsfájl.

### <a name="syntax"></a>Szintaxis
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Paraméterek
* **Név**: hello fürt csomópontjai toobe nevei elindult. Helyettesítő karakterek használata támogatott. hello paraméter beállított értéke nevét. Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.
* **Csomópont**-hello HpcNode objektumot hello csomópontok toobe indult el, mely a HPC PowerShell-parancsmaggal hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). hello paraméter beállított értéke csomópont. Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.

### <a name="example"></a>Példa
hello alábbi példa kezdődik csomópontok kezdődő nevek *HPCNode-CN -*.

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Állítsa le a virtuális gépek számítási csomópont
Állítsa le a számítási csomópontokat a hello **Stop-HpcIaaSNode.ps1** parancsfájl.

### <a name="syntax"></a>Szintaxis
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Paraméterek
* **Név**-hello fürt csomópontjai toobe nevei leállt. Helyettesítő karakterek használata támogatott. hello paraméter beállított értéke nevét. Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.
* **Csomópont**: hello HpcNode objektumot hello csomópontok toobe leállt, mely a HPC PowerShell-parancsmaggal hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). hello paraméter beállított értéke csomópont. Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.
* **Kényszerített** (nem kötelező): kapcsolat nélküli HPC-csomópontok tooforce beállítás azokat leállítása előtt.

### <a name="example"></a>Példa
hello alábbi példa arra kényszeríti a kapcsolat nélküli csomópontok kezdődő nevű *HPCNode-CN -* és majd leállítja hello csomópontok.

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a>Következő lépések
* tooautomatically nő vagy zsugorítása hello fürtcsomópont megfelelően hello aktuális terhelést a feladatok és hello fürtön feladatokat, lásd: [automatikusan nő, és zsugorítása hello Azure függően toohello fürtmunkaterhelésHPCPackfürterőforrások](hpcpack-cluster-node-autogrowshrink.md).

