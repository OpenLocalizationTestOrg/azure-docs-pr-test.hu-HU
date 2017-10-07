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
# <a name="move-a-windows-vm-from-amazon-web-services-aws-tooazure-using-powershell"></a><span data-ttu-id="9b89e-103">A Windows virtuális gépek áthelyezése az Amazon Web Services (AWS) tooAzure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="9b89e-103">Move a Windows VM from Amazon Web Services (AWS) tooAzure using PowerShell</span></span>

<span data-ttu-id="9b89e-104">Ha éppen kipróbálja az Azure virtuális gépek a munkaterhelések, meglévő Amazon Web Services (AWS) EC2 Windows Virtuálisgép-példány exportálása, majd töltse fel a virtuális merevlemez (VHD) tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="9b89e-104">If you are evaluating Azure virtual machines for hosting your workloads, you can export an existing Amazon Web Services (AWS) EC2 Windows VM instance then upload hello virtual hard disk (VHD) tooAzure.</span></span> <span data-ttu-id="9b89e-105">Egyszer hello VHD feltöltése, létrehozhat egy új virtuális Gépet az Azure-ban hello VHD-t.</span><span class="sxs-lookup"><span data-stu-id="9b89e-105">Once hello VHD is uploaded, you can create a new VM in Azure from hello VHD.</span></span> 

<span data-ttu-id="9b89e-106">Ez a témakör ismerteti, hogy egyetlen virtuális gép áthelyezése AWS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="9b89e-106">This topic covers moving a single VM from AWS tooAzure.</span></span> <span data-ttu-id="9b89e-107">Ha azt szeretné, hogy az AWS tooAzure léptékű toomove VMs, lásd: [telepítse át virtuális gépeket az Azure Site Recovery szolgáltatással Amazon Web Services (AWS) tooAzure](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9b89e-107">If you want toomove VMs from AWS tooAzure at scale, see [Migrate virtual machines in Amazon Web Services (AWS) tooAzure with Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span></span>

## <a name="prepare-hello-vm"></a><span data-ttu-id="9b89e-108">Hello virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="9b89e-108">Prepare hello VM</span></span> 
 
<span data-ttu-id="9b89e-109">Általános és a speciális VHD-k tooAzure feltölthet.</span><span class="sxs-lookup"><span data-stu-id="9b89e-109">You can upload both generalized and specialized VHDs tooAzure.</span></span> <span data-ttu-id="9b89e-110">Egyes elő kell készíteni hello VM AWS exportálása előtt.</span><span class="sxs-lookup"><span data-stu-id="9b89e-110">Each type requires that you prepare hello VM before exporting from AWS.</span></span> 

- <span data-ttu-id="9b89e-111">**Virtuális merevlemez általánosítva** -általánosított virtuális Merevlemezt volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el.</span><span class="sxs-lookup"><span data-stu-id="9b89e-111">**Generalized VHD** - a generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="9b89e-112">Ha azt tervezi, hogy a virtuális Merevlemezt egy kép toocreate toouse hello kell az új virtuális gépeket:</span><span class="sxs-lookup"><span data-stu-id="9b89e-112">If you intend toouse hello VHD as an image toocreate new VMs from, you should:</span></span> 
 
    * <span data-ttu-id="9b89e-113">[A Windows virtuális gép előkészítése](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="9b89e-113">[Prepare a Windows VM](prepare-for-upload-vhd-image.md).</span></span>  
    * <span data-ttu-id="9b89e-114">Generalize hello virtuális gépet a Sysprep használatával.</span><span class="sxs-lookup"><span data-stu-id="9b89e-114">Generalize hello virtual machine using Sysprep.</span></span>  

 
- <span data-ttu-id="9b89e-115">**Virtuális merevlemez kifejezetten** -speciális virtuális merevlemez hello felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép kezeli.</span><span class="sxs-lookup"><span data-stu-id="9b89e-115">**Specialized VHD** - a specialized VHD maintains hello user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="9b89e-116">Ha azt tervezi, hogy toouse virtuális Merevlemezt hello-toocreate van egy új virtuális Gépet, győződjön meg arról, hello lépések elvégzése.</span><span class="sxs-lookup"><span data-stu-id="9b89e-116">If you intend toouse hello VHD as-is toocreate a new VM, ensure hello following steps are completed.</span></span>  
    * <span data-ttu-id="9b89e-117">[Készítse elő a Windows virtuális merevlemez tooupload tooAzure](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="9b89e-117">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md).</span></span> <span data-ttu-id="9b89e-118">**Ne** generalize hello Sysprep használó virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="9b89e-118">**Do not** generalize hello VM using Sysprep.</span></span> 
    * <span data-ttu-id="9b89e-119">Távolítsa el a Vendég virtualizációs eszközök és a virtuális gép (azaz a VMware-eszközök) hello telepített ügynökök.</span><span class="sxs-lookup"><span data-stu-id="9b89e-119">Remove any guest virtualization tools and agents that are installed on hello VM (i.e. VMware tools).</span></span> 
    * <span data-ttu-id="9b89e-120">Ellenőrizze a virtuális gép hello konfigurált toopull van, az IP-cím és DNS-beállítások DHCP.</span><span class="sxs-lookup"><span data-stu-id="9b89e-120">Ensure hello VM is configured toopull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="9b89e-121">Ez biztosítja, hogy hello kiszolgálón hello virtuális hálózaton belül egy IP-címet kap, indításkor.</span><span class="sxs-lookup"><span data-stu-id="9b89e-121">This ensures that hello server obtains an IP address within hello VNet when it starts up.</span></span>  


## <a name="export-and-download-hello-vhd"></a><span data-ttu-id="9b89e-122">Exportálni, és töltse le a virtuális merevlemez hello</span><span class="sxs-lookup"><span data-stu-id="9b89e-122">Export and download hello VHD</span></span> 

<span data-ttu-id="9b89e-123">Exportálja a hello EC2 példány tooa az Amazon S3 gyűjtő VHD-n.</span><span class="sxs-lookup"><span data-stu-id="9b89e-123">Export hello EC2 instance tooa VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="9b89e-124">Hello Amazon dokumentáció témakörben leírt hello lépésekkel [exportálása egy példányát, a virtuális gép használatával virtuális gép importálási/exportálási](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) és futtatási hello [-példány-export-feladat létrehozása](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) parancs tooexport hello EC2 példány tooa VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="9b89e-124">Follow hello steps described in hello Amazon documentation topic [Exporting an Instance as a VM Using VM Import/Export](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) and run hello [create-instance-export-task](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) command tooexport hello EC2 instance tooa VHD file.</span></span> 

<span data-ttu-id="9b89e-125">hello exportált VHD-fájl kerül hello Amazon S3 gyűjtő megadott.</span><span class="sxs-lookup"><span data-stu-id="9b89e-125">hello exported VHD file is saved in hello Amazon S3 bucket you specify.</span></span> <span data-ttu-id="9b89e-126">hello alapvető szintaxisa a hello VHD exportálása nem éri el, helyettesítse a hello helyőrző szöveget <brackets> a adatokkal.</span><span class="sxs-lookup"><span data-stu-id="9b89e-126">hello basic syntax for exporting hello VHD is below, just replace hello placeholder text in <brackets> with your information.</span></span>

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

<span data-ttu-id="9b89e-127">Hello VHD exportálása után hello utasításokat követve [hogyan tegye le az objektum az az S3 gyűjtő?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) hello S3 gyűjtő toodownload hello VHD fájlt.</span><span class="sxs-lookup"><span data-stu-id="9b89e-127">Once hello VHD has been exported, follow hello instructions in [How Do I Download an Object from an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) toodownload hello VHD file from hello S3 bucket.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="9b89e-128">AWS adatok átvitel díjak letöltéséhez hello VHD költségek.</span><span class="sxs-lookup"><span data-stu-id="9b89e-128">AWS charges data transfer fees for downloading hello VHD.</span></span> <span data-ttu-id="9b89e-129">Lásd: [Amazon S3 árképzési](https://aws.amazon.com/s3/pricing/) további információt.</span><span class="sxs-lookup"><span data-stu-id="9b89e-129">See [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/) for more information.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9b89e-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b89e-130">Next steps</span></span>

<span data-ttu-id="9b89e-131">Most töltse fel a hello VHD tooAzure, és hozzon létre egy új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="9b89e-131">Now you can upload hello VHD tooAzure and create a new VM.</span></span> 

- <span data-ttu-id="9b89e-132">Ha túl a forrás futtatta a Sysprep**generalize** azt az exportálás előtt tekintse meg [egy általánosított virtuális merevlemez feltöltéséhez, és ezzel toocreate egy új virtuális gépek Azure-ban](upload-generalized-managed.md)</span><span class="sxs-lookup"><span data-stu-id="9b89e-132">If you ran Sysprep on your source too**generalize** it before exporting, see [Upload a generalized VHD and use it toocreate a new VMs in Azure](upload-generalized-managed.md)</span></span>
- <span data-ttu-id="9b89e-133">A Sysprep futtatása exportálása előtt, ha hello VHD tekinthető **speciális**, lásd: [egy speciális VHD tooAzure feltöltése és új virtuális gép létrehozása](create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="9b89e-133">If you did not run Sysprep before exporting, hello VHD is considered **specialized**, see [Upload a specialized VHD tooAzure and create a new VM](create-vm-specialized.md)</span></span>

 
