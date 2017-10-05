---
title: "Rendszerindítási diagnosztika a Linux virtuális gépek Azure-ban |} Microsoft Doc"
description: "A Linux virtuális gépek Azure-ban két hibakeresési funkcióinak áttekintése"
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
ms.openlocfilehash: 70254d39b5c6326166f7e29fdfc99533835502f9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-linux-virtual-machines-in-azure"></a><span data-ttu-id="18dcf-103">Rendszerindítási diagnosztika használata a Linux virtuális gépek Azure-ban hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="18dcf-103">How to use boot diagnostics to troubleshoot Linux virtual machines in Azure</span></span>

<span data-ttu-id="18dcf-104">Mostantól két hibakereső szolgáltatás is elérhető az Azure-ban: konzolkimenet és képernyőkép is támogatott az Azure virtuális gépeken a Resource Manager-alapú üzemi modellben.</span><span class="sxs-lookup"><span data-stu-id="18dcf-104">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="18dcf-105">Amikor a saját rendszerképét használja az Azure-ban, vagy valamelyik platform rendszerképét indítja, számos oka lehet annak, hogy egy virtuális gép rendszerindításra képtelen állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="18dcf-105">When bringing your own image to Azure or even booting one of the platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="18dcf-106">Ezekkel a szolgáltatásokkal könnyedén diagnosztizálhatja és helyreállíthatja a virtuális gépeket a rendszerindítási hibák után.</span><span class="sxs-lookup"><span data-stu-id="18dcf-106">These features enable you to easily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="18dcf-107">Linux virtuális gépek esetén a konzol naplófájljának kimenetét egyszerűen megtekintheti a Portalon:</span><span class="sxs-lookup"><span data-stu-id="18dcf-107">For Linux Virtual Machines, you can easily view the output of your console log from the Portal:</span></span>

![Azure Portal](./media/boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="18dcf-109">Azonban az Azure Windows és Linux virtuális gépeken is lehetővé teszi, hogy megtekintsen egy képernyőképet a virtuális gépről a hipervizortól:</span><span class="sxs-lookup"><span data-stu-id="18dcf-109">However, for both Windows and Linux Virtual Machines, Azure also enables you to see a screenshot of the VM from the hypervisor:</span></span>

![Hiba](./media/boot-diagnostics/screenshot2.png)

<span data-ttu-id="18dcf-111">Mindkét szolgáltatás támogatott az Azure virtuális gépeken minden régióban.</span><span class="sxs-lookup"><span data-stu-id="18dcf-111">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="18dcf-112">Ne feledje, akár 10 percet is igénybe vehet, hogy a képernyőképek és a kimenet megjelenjen a tárfiókjában.</span><span class="sxs-lookup"><span data-stu-id="18dcf-112">Note, screenshots, and output can take up to 10 minutes to appear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="18dcf-113">Gyakori rendszerindítási hibák</span><span class="sxs-lookup"><span data-stu-id="18dcf-113">Common boot errors</span></span>

- [<span data-ttu-id="18dcf-114">Kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="18dcf-114">File system issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [<span data-ttu-id="18dcf-115">Kernel-problémák</span><span class="sxs-lookup"><span data-stu-id="18dcf-115">Kernel Issues</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [<span data-ttu-id="18dcf-116">FSTAB hibák</span><span class="sxs-lookup"><span data-stu-id="18dcf-116">FSTAB errors</span></span>](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="18dcf-117">Diagnosztika engedélyezése egy új virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="18dcf-117">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="18dcf-118">Amikor új virtuális gépet hoz létre a Betekintő portálon, válassza az **Azure Resource Manager** lehetőséget az üzemi modell legördülő menüből:</span><span class="sxs-lookup"><span data-stu-id="18dcf-118">When creating a new Virtual Machine from the Preview Portal, select the **Azure Resource Manager** from the deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="18dcf-120">Konfigurálja a Figyelés beállítást, és válassza ki a tárfiókot, ahova el kívánja helyezni ezeket a diagnosztikai fájlokat.</span><span class="sxs-lookup"><span data-stu-id="18dcf-120">Configure the Monitoring option to select the storage account where you would like to place these diagnostic files.</span></span>
 
    ![Virtuális gép létrehozása](./media/boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="18dcf-122">Ha egy Azure Resource Manager-sablonból végzi a központi telepítést, keresse meg a virtuális gép erőforrást, és fűzze hozzá a diagnosztikai profil szakaszt.</span><span class="sxs-lookup"><span data-stu-id="18dcf-122">If you are deploying from an Azure Resource Manager template, navigate to your Virtual Machine resource and append the diagnostics profile section.</span></span> <span data-ttu-id="18dcf-123">Fontos, hogy a „2015-06-15” API-verzió fejlécet használja.</span><span class="sxs-lookup"><span data-stu-id="18dcf-123">Remember to use the “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="18dcf-124">A diagnosztikai profil lehetővé teszi, hogy kiválassza a tárfiókot, ahol el kívánja helyezni ezeket a naplókat.</span><span class="sxs-lookup"><span data-stu-id="18dcf-124">The diagnostics profile enables you to select the storage account where you want to put these logs.</span></span>

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

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="18dcf-125">Meglévő virtuális gép frissítése</span><span class="sxs-lookup"><span data-stu-id="18dcf-125">Update an existing virtual machine</span></span>

<span data-ttu-id="18dcf-126">Ahhoz, hogy a rendszerindítási diagnosztika a portálon keresztül, egy meglévő virtuális gépet a portálon keresztül is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="18dcf-126">To enable boot diagnostics through the portal, you can also update an existing virtual machine through the portal.</span></span> <span data-ttu-id="18dcf-127">Válassza a Rendszerindítási diagnosztika lehetőséget, majd kattintson a Mentés elemre.</span><span class="sxs-lookup"><span data-stu-id="18dcf-127">Select the Boot Diagnostics option and Save.</span></span> <span data-ttu-id="18dcf-128">A módosítás a virtuális gép újraindítása után lép életbe.</span><span class="sxs-lookup"><span data-stu-id="18dcf-128">Restart the VM to take effect.</span></span>

![Létező virtuális gép frissítése](./media/boot-diagnostics/screenshot5.png)