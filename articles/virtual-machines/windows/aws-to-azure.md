---
title: "A Windows AWS virtuális gépek áthelyezése az Azure-bA |} Microsoft Docs"
description: "Az Amazon Web Services (AWS) EC2 Windows példány az Azure virtuális gépek Azure PowerShell használatával."
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
ms.openlocfilehash: 7d2b498d3f84c4fd6cccf97c6d7781f293f5b395
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-to-azure-using-powershell"></a><span data-ttu-id="ef556-103">Az Amazon Web Services (AWS) a Windows virtuális gépek áthelyezése az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="ef556-103">Move a Windows VM from Amazon Web Services (AWS) to Azure using PowerShell</span></span>

<span data-ttu-id="ef556-104">Ha éppen kipróbálja az Azure virtuális gépek a munkaterhelések, meglévő Amazon Web Services (AWS) EC2 Windows Virtuálisgép-példány exportálása, majd a virtuális merevlemez (VHD) feltöltése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="ef556-104">If you are evaluating Azure virtual machines for hosting your workloads, you can export an existing Amazon Web Services (AWS) EC2 Windows VM instance then upload the virtual hard disk (VHD) to Azure.</span></span> <span data-ttu-id="ef556-105">A VHD-t a feltöltést követően létrehozhat egy új virtuális gép az Azure virtuális merevlemezről.</span><span class="sxs-lookup"><span data-stu-id="ef556-105">Once the VHD is uploaded, you can create a new VM in Azure from the VHD.</span></span> 

<span data-ttu-id="ef556-106">Ez a témakör ismerteti, hogy egyetlen virtuális gép áthelyezése AWS az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="ef556-106">This topic covers moving a single VM from AWS to Azure.</span></span> <span data-ttu-id="ef556-107">Ha az áthelyezni kívánt virtuális gépeket az AWS Azure léptékű, lásd: [Amazon Web Services (AWS) virtuális gépek áttelepítése az Azure-bA az Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="ef556-107">If you want to move VMs from AWS to Azure at scale, see [Migrate virtual machines in Amazon Web Services (AWS) to Azure with Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span></span>

## <a name="prepare-the-vm"></a><span data-ttu-id="ef556-108">A virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="ef556-108">Prepare the VM</span></span> 
 
<span data-ttu-id="ef556-109">Általános és a speciális virtuális merevlemezek feltöltheti az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="ef556-109">You can upload both generalized and specialized VHDs to Azure.</span></span> <span data-ttu-id="ef556-110">Egyes elő kell készíteni a virtuális gép AWS exportálása előtt.</span><span class="sxs-lookup"><span data-stu-id="ef556-110">Each type requires that you prepare the VM before exporting from AWS.</span></span> 

- <span data-ttu-id="ef556-111">**Virtuális merevlemez általánosítva** -általánosított virtuális Merevlemezt volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el.</span><span class="sxs-lookup"><span data-stu-id="ef556-111">**Generalized VHD** - a generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="ef556-112">Ha azt tervezi, a virtuális merevlemez használata képként az új virtuális gépek létrehozásához, a következőket:</span><span class="sxs-lookup"><span data-stu-id="ef556-112">If you intend to use the VHD as an image to create new VMs from, you should:</span></span> 
 
    * <span data-ttu-id="ef556-113">[A Windows virtuális gép előkészítése](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="ef556-113">[Prepare a Windows VM](prepare-for-upload-vhd-image.md).</span></span>  
    * <span data-ttu-id="ef556-114">A virtuális gépet a Sysprep használatával generalize.</span><span class="sxs-lookup"><span data-stu-id="ef556-114">Generalize the virtual machine using Sysprep.</span></span>  

 
- <span data-ttu-id="ef556-115">**Virtuális merevlemez kifejezetten** -speciális virtuális merevlemez kezeli a felhasználói fiókok, alkalmazások és az eredeti virtuális gép más állapotba adatait.</span><span class="sxs-lookup"><span data-stu-id="ef556-115">**Specialized VHD** - a specialized VHD maintains the user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="ef556-116">Ha szeretne használni, mint a VHD-t hozzon létre egy új virtuális Gépet, győződjön meg arról, a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ef556-116">If you intend to use the VHD as-is to create a new VM, ensure the following steps are completed.</span></span>  
    * <span data-ttu-id="ef556-117">[Az Azure-bA feltölteni egy Windows virtuális merevlemez előkészítése](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="ef556-117">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md).</span></span> <span data-ttu-id="ef556-118">**Ne** generalize a virtuális Gépet a Sysprep használata.</span><span class="sxs-lookup"><span data-stu-id="ef556-118">**Do not** generalize the VM using Sysprep.</span></span> 
    * <span data-ttu-id="ef556-119">Távolítsa el a Vendég virtualizációs eszközök és a virtuális Gépre (azaz a VMware-eszközök) telepített ügynökök.</span><span class="sxs-lookup"><span data-stu-id="ef556-119">Remove any guest virtualization tools and agents that are installed on the VM (i.e. VMware tools).</span></span> 
    * <span data-ttu-id="ef556-120">Győződjön meg arról, a virtuális Géphez való lekérésére, az IP-cím és DNS-beállítások DHCP van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="ef556-120">Ensure the VM is configured to pull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="ef556-121">Ez biztosítja, hogy a kiszolgáló a Vneten belül egy IP-címet kap, indításkor.</span><span class="sxs-lookup"><span data-stu-id="ef556-121">This ensures that the server obtains an IP address within the VNet when it starts up.</span></span>  


## <a name="export-and-download-the-vhd"></a><span data-ttu-id="ef556-122">Exportálni, és töltse le a virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="ef556-122">Export and download the VHD</span></span> 

<span data-ttu-id="ef556-123">Exportálja a EC2 példány az Amazon S3 gyűjtő a virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="ef556-123">Export the EC2 instance to a VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="ef556-124">Az Amazon dokumentáció a témakörben ismertetett lépéseket követve [exportálása egy példányát, a virtuális gép használatával virtuális gép importálási/exportálási](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) , és futtassa a [-példány-export-feladat létrehozása](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) parancs a EC2 példány exportálja egy virtuális merevlemez fájl.</span><span class="sxs-lookup"><span data-stu-id="ef556-124">Follow the steps described in the Amazon documentation topic [Exporting an Instance as a VM Using VM Import/Export](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) and run the [create-instance-export-task](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) command to export the EC2 instance to a VHD file.</span></span> 

<span data-ttu-id="ef556-125">Az Amazon S3 gyűjtő megadja az exportált VHD-fájl kerül.</span><span class="sxs-lookup"><span data-stu-id="ef556-125">The exported VHD file is saved in the Amazon S3 bucket you specify.</span></span> <span data-ttu-id="ef556-126">Alapvető szintaxisa a VHD exportálása nem éri el, az imént cserélje le a helyőrző szöveget <brackets> a adatokkal.</span><span class="sxs-lookup"><span data-stu-id="ef556-126">The basic syntax for exporting the VHD is below, just replace the placeholder text in <brackets> with your information.</span></span>

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

<span data-ttu-id="ef556-127">A VHD exportálása után kövesse az utasításokat [hogyan tegye le az objektum az az S3 gyűjtő?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) töltheti le a VHD-fájlt a S3 gyűjtő.</span><span class="sxs-lookup"><span data-stu-id="ef556-127">Once the VHD has been exported, follow the instructions in [How Do I Download an Object from an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) to download the VHD file from the S3 bucket.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="ef556-128">AWS díjak adatátvitel díjak letöltéséhez a VHD-t.</span><span class="sxs-lookup"><span data-stu-id="ef556-128">AWS charges data transfer fees for downloading the VHD.</span></span> <span data-ttu-id="ef556-129">Lásd: [Amazon S3 árképzési](https://aws.amazon.com/s3/pricing/) további információt.</span><span class="sxs-lookup"><span data-stu-id="ef556-129">See [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/) for more information.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ef556-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef556-130">Next steps</span></span>

<span data-ttu-id="ef556-131">Most a virtuális merevlemez feltöltése az Azure-ba, és hozzon létre egy új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="ef556-131">Now you can upload the VHD to Azure and create a new VM.</span></span> 

- <span data-ttu-id="ef556-132">Ha a forrást a Sysprep futtatása **generalize** azt az exportálás előtt tekintse meg [egy általánosított virtuális merevlemez feltöltéséhez, és annak segítségével hozzon létre egy új virtuális gépek Azure-ban](upload-generalized-managed.md)</span><span class="sxs-lookup"><span data-stu-id="ef556-132">If you ran Sysprep on your source to **generalize** it before exporting, see [Upload a generalized VHD and use it to create a new VMs in Azure](upload-generalized-managed.md)</span></span>
- <span data-ttu-id="ef556-133">A Sysprep futtatása exportálása előtt, ha a virtuális merevlemez tekinthető **speciális**, lásd: [speciális virtuális merevlemez feltöltése az Azure-ba, és hozzon létre egy új virtuális Gépet](create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="ef556-133">If you did not run Sysprep before exporting, the VHD is considered **specialized**, see [Upload a specialized VHD to Azure and create a new VM](create-vm-specialized.md)</span></span>

 
