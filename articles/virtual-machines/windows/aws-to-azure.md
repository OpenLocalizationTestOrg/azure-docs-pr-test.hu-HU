---
title: "a Windows AWS virtuális gépek tooAzure aaaMove |} Microsoft Docs"
description: "Helyezze át az Amazon Web Services (AWS) EC2 Windows példány tooAzure virtuális gépek Azure PowerShell használatával."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: cynthn
ms.openlocfilehash: f912c28d3ffe585162c3add715a1318ac3cd4643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-tooazure-using-powershell"></a>A Windows virtuális gépek áthelyezése az Amazon Web Services (AWS) tooAzure PowerShell használatával

Ha éppen kipróbálja az Azure virtuális gépek a munkaterhelések, meglévő Amazon Web Services (AWS) EC2 Windows Virtuálisgép-példány exportálása, majd töltse fel a virtuális merevlemez (VHD) tooAzure hello. Egyszer hello VHD feltöltése, létrehozhat egy új virtuális Gépet az Azure-ban hello VHD-t. 

Ez a témakör ismerteti, hogy egyetlen virtuális gép áthelyezése AWS tooAzure. Ha azt szeretné, hogy az AWS tooAzure léptékű toomove VMs, lásd: [telepítse át virtuális gépeket az Azure Site Recovery szolgáltatással Amazon Web Services (AWS) tooAzure](../../site-recovery/site-recovery-migrate-aws-to-azure.md).

## <a name="prepare-hello-vm"></a>Hello virtuális gép előkészítése 
 
Általános és a speciális VHD-k tooAzure feltölthet. Egyes elő kell készíteni hello VM AWS exportálása előtt. 

- **Virtuális merevlemez általánosítva** -általánosított virtuális Merevlemezt volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el. Ha azt tervezi, hogy a virtuális Merevlemezt egy kép toocreate toouse hello kell az új virtuális gépeket: 
 
    * [A Windows virtuális gép előkészítése](prepare-for-upload-vhd-image.md).  
    * Generalize hello virtuális gépet a Sysprep használatával.  

 
- **Virtuális merevlemez kifejezetten** -speciális virtuális merevlemez hello felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép kezeli. Ha azt tervezi, hogy toouse virtuális Merevlemezt hello-toocreate van egy új virtuális Gépet, győződjön meg arról, hello lépések elvégzése.  
    * [Készítse elő a Windows virtuális merevlemez tooupload tooAzure](prepare-for-upload-vhd-image.md). **Ne** generalize hello Sysprep használó virtuális gépek. 
    * Távolítsa el a Vendég virtualizációs eszközök és a virtuális gép (azaz a VMware-eszközök) hello telepített ügynökök. 
    * Ellenőrizze a virtuális gép hello konfigurált toopull van, az IP-cím és DNS-beállítások DHCP. Ez biztosítja, hogy hello kiszolgálón hello virtuális hálózaton belül egy IP-címet kap, indításkor.  


## <a name="export-and-download-hello-vhd"></a>Exportálni, és töltse le a virtuális merevlemez hello 

Exportálja a hello EC2 példány tooa az Amazon S3 gyűjtő VHD-n. Hello Amazon dokumentáció témakörben leírt hello lépésekkel [exportálása egy példányát, a virtuális gép használatával virtuális gép importálási/exportálási](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) és futtatási hello [-példány-export-feladat létrehozása](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) parancs tooexport hello EC2 példány tooa VHD-fájlt. 

hello exportált VHD-fájl kerül hello Amazon S3 gyűjtő megadott. hello alapvető szintaxisa a hello VHD exportálása nem éri el, helyettesítse a hello helyőrző szöveget <brackets> a adatokkal.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

Hello VHD exportálása után hello utasításokat követve [hogyan tegye le az objektum az az S3 gyűjtő?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) hello S3 gyűjtő toodownload hello VHD fájlt. 

> [!IMPORTANT]
> AWS adatok átvitel díjak letöltéséhez hello VHD költségek. Lásd: [Amazon S3 árképzési](https://aws.amazon.com/s3/pricing/) további információt.


## <a name="next-steps"></a>Következő lépések

Most töltse fel a hello VHD tooAzure, és hozzon létre egy új virtuális Gépet. 

- Ha túl a forrás futtatta a Sysprep**generalize** azt az exportálás előtt tekintse meg [egy általánosított virtuális merevlemez feltöltéséhez, és ezzel toocreate egy új virtuális gépek Azure-ban](upload-generalized-managed.md)
- A Sysprep futtatása exportálása előtt, ha hello VHD tekinthető **speciális**, lásd: [egy speciális VHD tooAzure feltöltése és új virtuális gép létrehozása](create-vm-specialized.md)

 
