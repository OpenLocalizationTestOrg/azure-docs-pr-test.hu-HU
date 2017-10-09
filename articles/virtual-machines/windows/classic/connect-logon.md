---
title: "aaaLog tooa a klasszikus Azure virtuális gép |} Microsoft Docs"
description: "Az Azure portál toolog hello virtuális gépen használni tooa Windows hello klasszikus telepítési modellel létrehozott."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 2e32b7036c2538e73b46580e0f5f8f4979e8a685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-on-tooa-windows-virtual-machine-using-hello-azure-portal"></a><span data-ttu-id="9e5e7-103">Jelentkezzen be tooa Windows virtuális gép hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="9e5e7-103">Log on tooa Windows virtual machine using hello Azure portal</span></span>
<span data-ttu-id="9e5e7-104">Hello Azure-portálon, használhatja az hello **Connect** egy távoli asztali munkamenet toostart gombra, és jelentkezzen be Windows virtuális gép tooa.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-104">In hello Azure portal, you use hello **Connect** button toostart a Remote Desktop session and log on tooa Windows VM.</span></span>

<span data-ttu-id="9e5e7-105">Linux virtuális gép tooconnect tooa szeretne?</span><span class="sxs-lookup"><span data-stu-id="9e5e7-105">Do you want tooconnect tooa Linux VM?</span></span> <span data-ttu-id="9e5e7-106">Lásd: [hogyan toolog a Linux rendszerű virtuális gép tooa](../../linux/mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="9e5e7-106">See [How toolog on tooa virtual machine running Linux](../../linux/mac-create-ssh-keys.md).</span></span>

<!--
Deleting, but not 100% sure
Learn how too[perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> <span data-ttu-id="9e5e7-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9e5e7-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9e5e7-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="9e5e7-109">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="9e5e7-110">Hogyan tooa VM használatával toolog hello erőforrás-kezelő információt objektummodell [Itt](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9e5e7-110">For information about how toolog on tooa VM using hello Resource Manager model, see [here](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="9e5e7-111">Csatlakoztassa toohello virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="9e5e7-111">Connect toohello virtual machine</span></span>
1. <span data-ttu-id="9e5e7-112">Bejelentkezés toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-112">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="9e5e7-113">Kattintson a hello virtuális gép, amelyet az tooaccess.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-113">Click on hello virtual machine that you want tooaccess.</span></span> <span data-ttu-id="9e5e7-114">hello szerepel-e hello **összes erőforrás** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-114">hello name is listed in hello **All resources** pane.</span></span>

    ![Virtuális-machine-helyek](./media/connect-logon/azureportaldashboard.png)

3. <span data-ttu-id="9e5e7-116">Kattintson a **Connect** hello parancssávon elveire hello virtuális gép irányítópult.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-116">Click **Connect** on hello command bar atop hello virtual machine dashboard.</span></span>

    ![Csatlakozás hello virtuális gép ikonja](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If hello **Connect** button isn't available, see hello troubleshooting tips at hello end of this article.
>
>
-->

## <a name="log-on-toohello-virtual-machine"></a><span data-ttu-id="9e5e7-118">Jelentkezzen be a virtuális gép toohello</span><span class="sxs-lookup"><span data-stu-id="9e5e7-118">Log on toohello virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="9e5e7-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9e5e7-119">Next steps</span></span>
* <span data-ttu-id="9e5e7-120">Ha hello **Connect** gomb inaktív vagy egyéb problémák hello távoli asztali kapcsolattal, vagy próbálja hello-konfigurációjának visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-120">If hello **Connect** button is inactive or you are having other problems with hello Remote Desktop connection, try resetting hello configuration.</span></span> <span data-ttu-id="9e5e7-121">Kattintson a **távelérés alaphelyzetbe állítása** hello virtuális gép irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-121">click **Reset remote access** from hello virtual machine dashboard.</span></span>

    ![Távelérés alaphelyzetbe állítása](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* <span data-ttu-id="9e5e7-123">A jelszó kapcsolatos problémák próbálkozzon a visszaállításával.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-123">For problems with your password, try resetting it.</span></span> <span data-ttu-id="9e5e7-124">Kattintson a **jelszó-átállítási** mentén hello bal szélétől virtuális gép irányítópult a **támogatási + hibaelhárítás**.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-124">Click **Reset password** along hello left edge of virtual machine dashboard, under **Support + Troubleshooting**.</span></span>

    ![Jelszó alaphelyzetbe állítása](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

<span data-ttu-id="9e5e7-126">Ha ezek a tippek nem működnek, vagy nem kapcsolódnak, mire van szüksége, tekintse meg [hibaelhárítása távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gépek](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9e5e7-126">If those tips don't work or aren't what you need, see [Troubleshoot Remote Desktop connections tooa Windows-based Azure Virtual Machine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="9e5e7-127">Ez a cikk útmutatást nyújt a gyakori problémák diagnosztizálásához és elhárításához.</span><span class="sxs-lookup"><span data-stu-id="9e5e7-127">This article walks you through diagnosing and resolving common problems.</span></span>
