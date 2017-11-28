---
title: "a Linux virtuális gépek Azure-ban aaaBoot diagnosztika |} Microsoft Doc"
description: "Hello két hibakeresési funkcióinak a Linux virtuális gépek Azure-ban áttekintése"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: delhan
ms.openlocfilehash: d355d512de09d2f1d7a2718e3db3fb99c9dd9e24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-boot-diagnostics-tootroubleshoot-linux-virtual-machines-in-azure"></a><span data-ttu-id="b5659-103">Hogyan toouse rendszerindítási diagnosztika tootroubleshoot Linux virtuális gépek Azure</span><span class="sxs-lookup"><span data-stu-id="b5659-103">How toouse boot diagnostics tootroubleshoot Linux virtual machines in Azure</span></span>

<span data-ttu-id="b5659-104">Mostantól két hibakereső szolgáltatás is elérhető az Azure-ban: konzolkimenet és képernyőkép is támogatott az Azure virtuális gépeken a Resource Manager-alapú üzemi modellben.</span><span class="sxs-lookup"><span data-stu-id="b5659-104">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="b5659-105">Amikor a saját kép tooAzure vagy hello platform képek is rendszerindítás egyike, miért egy virtuális gép nem indítható állapotba lekérdezi számos oka lehet.</span><span class="sxs-lookup"><span data-stu-id="b5659-105">When bringing your own image tooAzure or even booting one of hello platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="b5659-106">E szolgáltatások engedélyezése meg tooeasily diagnosztizálhatja és a virtuális gépek helyreállítás rendszerindítási hibák esetén.</span><span class="sxs-lookup"><span data-stu-id="b5659-106">These features enable you tooeasily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="b5659-107">A Linux virtuális gépek megtekintéséhez a konzol napló, a portál hello hello kimeneti:</span><span class="sxs-lookup"><span data-stu-id="b5659-107">For Linux Virtual Machines, you can easily view hello output of your console log from hello Portal:</span></span>

![Azure Portal](./media/boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="b5659-109">Azonban a Windows és Linux rendszerű virtuális gépek Azure is lehetővé teszi, hogy egy képernyőfelvétel a hello VM hello hipervizorról toosee:</span><span class="sxs-lookup"><span data-stu-id="b5659-109">However, for both Windows and Linux Virtual Machines, Azure also enables you toosee a screenshot of hello VM from hello hypervisor:</span></span>

![Hiba](./media/boot-diagnostics/screenshot2.png)

<span data-ttu-id="b5659-111">Mindkét szolgáltatás támogatott az Azure virtuális gépeken minden régióban.</span><span class="sxs-lookup"><span data-stu-id="b5659-111">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="b5659-112">Vegye figyelembe, a pillanatképek és a kimeneti too10 perc tooappear a tárfiókban lévő is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="b5659-112">Note, screenshots, and output can take up too10 minutes tooappear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="b5659-113">Gyakori rendszerindítási hibák</span><span class="sxs-lookup"><span data-stu-id="b5659-113">Common boot errors</span></span>

- [<span data-ttu-id="b5659-114">Kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="b5659-114">File system issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [<span data-ttu-id="b5659-115">Kernel-problémák</span><span class="sxs-lookup"><span data-stu-id="b5659-115">Kernel Issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [<span data-ttu-id="b5659-116">FSTAB hibák</span><span class="sxs-lookup"><span data-stu-id="b5659-116">FSTAB errors</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="b5659-117">Diagnosztika engedélyezése egy új virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="b5659-117">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="b5659-118">Ha egy új virtuális gép létrehozása a betekintő portálon hello, válassza ki a hello **Azure Resource Manager** hello telepítési modell legördülő listából:</span><span class="sxs-lookup"><span data-stu-id="b5659-118">When creating a new Virtual Machine from hello Preview Portal, select hello **Azure Resource Manager** from hello deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="b5659-120">A diagnosztikai fájlok hello figyelés beállítás tooselect hello tárfiók tooplace helyének megadása</span><span class="sxs-lookup"><span data-stu-id="b5659-120">Configure hello Monitoring option tooselect hello storage account where you would like tooplace these diagnostic files.</span></span>
 
    ![Virtuális gép létrehozása](./media/boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="b5659-122">Ha telepíti az Azure Resource Manager sablon alapján, keresse meg a virtuálisgép-erőforrás tooyour, és a hozzáfűző hello diagnosztikai profil szakasz.</span><span class="sxs-lookup"><span data-stu-id="b5659-122">If you are deploying from an Azure Resource Manager template, navigate tooyour Virtual Machine resource and append hello diagnostics profile section.</span></span> <span data-ttu-id="b5659-123">Ne felejtse el toouse hello "2015-06-15" API version fejlécnek.</span><span class="sxs-lookup"><span data-stu-id="b5659-123">Remember toouse hello “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="b5659-124">hello diagnosztikai profiljának lehetővé teszi tooselect hello tárfiók, amelybe tooput ezek a naplók.</span><span class="sxs-lookup"><span data-stu-id="b5659-124">hello diagnostics profile enables you tooselect hello storage account where you want tooput these logs.</span></span>

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="b5659-125">Meglévő virtuális gép frissítése</span><span class="sxs-lookup"><span data-stu-id="b5659-125">Update an existing virtual machine</span></span>

<span data-ttu-id="b5659-126">hello portálon keresztül tooenable a rendszerindítási diagnosztika, is frissítheti egy meglévő virtuális gép hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="b5659-126">tooenable boot diagnostics through hello portal, you can also update an existing virtual machine through hello portal.</span></span> <span data-ttu-id="b5659-127">Válassza ki a rendszerindítási diagnosztika lehetőséget, majd mentse hello.</span><span class="sxs-lookup"><span data-stu-id="b5659-127">Select hello Boot Diagnostics option and Save.</span></span> <span data-ttu-id="b5659-128">Hello VM tootake léptetéséhez indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="b5659-128">Restart hello VM tootake effect.</span></span>

![Létező virtuális gép frissítése](./media/boot-diagnostics/screenshot5.png)