---
title: "egy helyszíni virtuális gép kép hello Azure piactér aaaCreating |} Microsoft Docs"
description: "Megértésére és hajtható végre hello lépéseket toocreate egy helyszíni Virtuálisgép-lemezkép központi telepítése Azure piactér toohello mások toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c7a265330f1e494db8d0e981a38ee00d85746bb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-hello-azure-marketplace"></a>A helyszíni virtuális gép kép hello Azure piactér fejlesztése
Határozottan javasoljuk, hogy fejlesztése az Azure virtuális merevlemezeket (VHD) közvetlenül hello felhő távoli asztal protokoll használatával. Azonban ha kell, akkor lehetséges toodownload virtuális Merevlemezt és fejleszthetők a helyszíni infrastruktúra segítségével.  

A helyi fejlesztési, le kell töltenie hello operációs rendszer virtuális Merevlemezének hello létrehozott virtuális gép. Ezeket a lépéseket akkor kerül sor lépésben 3.3-as, a rendszer fent.  

## <a name="download-a-vhd-image"></a>A Virtuálismerevlemez-kép letöltése
### <a name="locate-a-blob-url"></a>Keresse meg a blob URL-címe
Rendelés toodownload hello VHD-t először keresse meg az operációsrendszer-lemez hello hello blob URL-címet.

Keresse meg új hello blob URL-cím-hello [Microsoft Azure-portálon](https://portal.azure.com):

1. Nyissa meg túl**Tallózás** > **virtuális gépek**, és jelölje ki hello telepített virtuális gép.
2. A **konfigurálása**, jelölje be hello **lemezek** csempe hello lemezek paneljének megnyitása, amelyen.
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. Jelölje be hello **operációsrendszer-lemez**, amely megnyílik egy újabb panel, amely megjeleníti a lemez tulajdonságai, például hello virtuális merevlemez helye.
4. A blob URL-Címének másolása.
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. Most, törölje hello telepített virtuális gép hello biztonsági lemezek törlése nélkül. Hello VM is leállíthatja a törlés helyett. Ne töltse le az hello operációs rendszer virtuális Merevlemeze hello virtuális gép futása közben.
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a>VHD letöltése
Után tudja hello blob URL-címe, letöltheti hello segítségével hello VHD [Azure-portálon](http://manage.windowsazure.com/) vagy a PowerShell használatával.  

> [!NOTE]
> Ez az útmutató létrehozási hello időpontban hello funkció toodownload virtuális merevlemez még nincs hello új Microsoft Azure-portálon szerepel.  
> 
> 

**Töltse le a hello operációs rendszer virtuális Merevlemeze keresztül aktuális hello [Azure-portálon](http://manage.windowsazure.com/)**

1. Jelentkezzen be Azure-portálon toohello, ha még nem meg már.
2. Kattintson a hello **tárolási** fülre.
3. Válassza ki a hello tárfiók belül mely hello tárolja a virtuális merevlemez.
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. Ez megjeleníti a tárfiók tulajdonságai. Jelölje be hello **tárolók** fülre.
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. Jelölje ki a mely hello tárolja a virtuális merevlemez hello tárolót. Alapértelmezés szerint hello portálon létrehozott hello VHD tárolja a VHD-k tárolója.
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. Válassza ki a hello megfelelő operációs rendszer virtuális Merevlemeze összehasonlítja egy Ön által mentett hello URL-cím toohello.
7. Kattintson a **Letöltés** gombra.
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a>Töltse le a virtuális merevlemez PowerShell használatával
Ezenkívül toousing hello Azure-portálon, használhatja a hello [mentés-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) parancsmag toodownload hello operációs rendszer virtuális Merevlemezt.

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
Például mentés-AzureVhd-forrás "https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd" - LocalFilePath "C:\Users\Administrator\Desktop\baseimagevm.vhd" - StorageKey tulajdonságát<String>

> [!NOTE]
> **Mentés-AzureVhd** is rendelkezik egy **NumberOfThreads** lehetőség, amely lehet tooincrease párhuzamossági toomake hello lehető legjobb felhasználását rendelkezésre álló sávszélesség használt hello letölthető.
> 
> 

## <a name="upload-vhds-tooan-azure-storage-account"></a>Töltse fel a VHD-k tooan Azure storage-fiók
Felkészülés a VHD-k a helyszínen, ha szüksége tooupload őket a tárolási fiók az Azure-ban. Ez a lépés után a virtuális merevlemez létrehozása a helyszínen, de a VM-lemezkép hitelesítő megszerzése előtt történik.

### <a name="create-a-storage-account-and-container"></a>A tárfiók és tároló létrehozása
Azt javasoljuk, hogy virtuális merevlemezek egy tárfiókot hello az Amerikai Egyesült Államokban régióban kell-e töltve. Minden virtuális merevlemez egyetlen termékváltozat egy tárfiókon belül egyetlen tárolót kell helyezni.

toocreate egy tárfiókot, hello használható [Microsoft Azure-portálon](https://portal.azure.com/), PowerShell, vagy hello Linux parancssori eszközt.  

**Hozzon létre egy tárfiókot hello Microsoft Azure portálról**

1. Kattintson az **Új** lehetőségre.
2. Válassza ki **tárolási**.
3. Töltse ki a tárfiók neve hello, és válassza ki a helyet.
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. Kattintson a **Create** (Létrehozás) gombra.
5. storage-fiók létrehozása hello hello paneljén nyitva kell lennie. Ha nem, válassza ki a **Tallózás** > **Tárfiókok**. Hello tárolási fiókot panelen, jelölje ki a létrehozott hello tárfiókban.
6. Válassza ki **tárolók**.
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. Hello tárolók paneljén válassza **Hozzáadás**, majd adja meg a tároló nevét és hello tároló engedélyeit. Válassza ki **titkos** tároló engedélyek.

> [!TIP]
> Azt javasoljuk, hogy hozzon létre egy tároló, hogy azt tervezi, toopublish SKU /.
> 
> 

  ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a>A storage-fiók létrehozása a PowerShell használatával
A powershellel, hozzon létre egy tárfiókot hello segítségével [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) parancsmag.

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

Ezután hello segítségével hozhat létre egy tárolót a tárfiókon belül [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) parancsmag.

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> Azokat a parancsokat azt feltételezik, hogy hello aktuális tárfiók környezetét már be van állítva a PowerShellben.   Tekintse meg a túl[beállítása az Azure PowerShell](marketplace-publishing-powershell-setup.md) kapcsolatban további részleteket a PowerShell-telepítőt.  
> 
> ### <a name="create-a-storage-account-by-using-hello-command-line-tool-for-mac-and-linux"></a>Hozzon létre egy tárfiókot hello parancssori eszközzel a Mac és Linux
> A [Linux parancssori eszköz](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), az alábbiak szerint hozzon létre egy tárfiókot.
> 
> 

        azure storage account create mystorageaccount --location "West US"

Az alábbiak szerint hozzon létre egy tárolót.

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a>VHD feltöltése
Hello tárfiók és tároló létrehozása után feltöltheti az előkészített virtuális merevlemezeket. Használhatja a PowerShell, a hello Linux parancssori eszköz vagy az egyéb Azure Storage felügyeleti eszközöket.

### <a name="upload-a-vhd-via-powershell"></a>A PowerShell segítségével virtuális merevlemez feltöltéséhez
Használjon hello [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) parancsmag.

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-hello-command-line-tool-for-mac-and-linux"></a>A virtuális merevlemez feltöltéséhez hello parancssori eszközzel a Mac és Linux
A hello [Linux parancssori eszköz](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), hello alábbi: azure virtuálisgép-lemezkép létrehozása <image name> --hely <Location of hello data center> – operációs rendszer Linux<LocationOfLocalVHD>

## <a name="see-also"></a>Lásd még:
* [Hello piactér a virtuális gép lemezkép létrehozása](marketplace-publishing-vm-image-creation.md)
* [Azure PowerShell telepítése](marketplace-publishing-powershell-setup.md)

