---
title: "A Linux virtuális gép távoli asztal |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse és konfigurálja a klasszikus telepítési modell a Microsoft Azure Linux virtuális gép kapcsolódni a távoli asztal"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: mingzhan
ms.openlocfilehash: 68031d548bdbeda9a83d1bceaaea7c5bbcab3188
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a><span data-ttu-id="863be-103">Kapcsolódás Microsoft Azure-beli linuxos VM-hez a Távoli asztal használatával</span><span class="sxs-lookup"><span data-stu-id="863be-103">Using Remote Desktop to connect to a Microsoft Azure Linux VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="863be-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="863be-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="863be-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="863be-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="863be-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="863be-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="863be-107">Ez a cikk frissített Resource Manager verziója, lásd: [Itt](../use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="863be-107">For the updated Resource Manager version of this article, see [here](../use-remote-desktop.md).</span></span>

## <a name="overview"></a><span data-ttu-id="863be-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="863be-108">Overview</span></span>
<span data-ttu-id="863be-109">RDP (Remote Desktop Protocol) a saját fejlesztésű protokollja használt Windows.</span><span class="sxs-lookup"><span data-stu-id="863be-109">RDP (Remote Desktop Protocol) is a proprietary protocol used for Windows.</span></span> <span data-ttu-id="863be-110">A Microsoft használatát RDP távolról csatlakozni a Linux virtuális gépek (virtuális gép)?</span><span class="sxs-lookup"><span data-stu-id="863be-110">How can we use RDP to connect to a Linux VM (virtual machine) remotely?</span></span>

<span data-ttu-id="863be-111">Ez az útmutató Erre azért van szükség a válasz!</span><span class="sxs-lookup"><span data-stu-id="863be-111">This guidance will give you the answer!</span></span> <span data-ttu-id="863be-112">Azt, hogy telepítse és a Microsoft Azure Linux virtuális gép meg, amely lehetővé teszi, hogy csatlakozni a távoli asztalról a Windows-gépről a config xrdp segítségével.</span><span class="sxs-lookup"><span data-stu-id="863be-112">It will help you to install and config xrdp on your Microsoft Azure Linux VM, which lets you connect to it with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="863be-113">Ubuntu, OpenSUSE vagy az ebben az útmutatóban példaként futó Linux virtuális gép használjuk.</span><span class="sxs-lookup"><span data-stu-id="863be-113">We will use Linux VM running Ubuntu or OpenSUSE as the example in this guidance.</span></span>

<span data-ttu-id="863be-114">A xrdp eszköze egy nyílt forráskódú RDP-kiszolgáló, amely lehetővé teszi a kapcsolódást a Linux-kiszolgálóra a távoli asztalról a Windows-gépről.</span><span class="sxs-lookup"><span data-stu-id="863be-114">The xrdp tool is an open source RDP server that allows you to connect your Linux server with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="863be-115">RDP (virtuális hálózat számítástechnikai) VNC jobb teljesítményt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="863be-115">RDP has better performance than VNC (Virtual Network Computing).</span></span> <span data-ttu-id="863be-116">VNC Renderelés JPEG minőségű grafikus használatával, és a lassú lehet, mivel az RDP gyors és egyszerű crystal.</span><span class="sxs-lookup"><span data-stu-id="863be-116">VNC renders using JPEG-quality graphics and can be slow, whereas RDP is fast and crystal clear.</span></span>

> [!NOTE]
> <span data-ttu-id="863be-117">Már rendelkeznie kell egy Microsoft Azure virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="863be-117">You must already have an Microsoft Azure VM running Linux.</span></span> <span data-ttu-id="863be-118">Hozzon létre, és a Linux virtuális gépet, a [Azure Linux virtuális gép oktatóanyag](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="863be-118">To create and set up a Linux VM, see the [Azure Linux VM tutorial](createportal.md).</span></span>
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a><span data-ttu-id="863be-119">Hozzon létre egy végpontot a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="863be-119">Create an endpoint for Remote Desktop</span></span>
<span data-ttu-id="863be-120">Használjuk az alapértelmezett végpont 3389-es távoli asztal Ez a dokumentum.</span><span class="sxs-lookup"><span data-stu-id="863be-120">We will use the default endpoint 3389 for Remote Desktop in this doc.</span></span> <span data-ttu-id="863be-121">Állítsa be, 3389 végpont `Remote Desktop` a Linux virtuális gépekre például alatt:</span><span class="sxs-lookup"><span data-stu-id="863be-121">Set up 3389 endpoint as `Remote Desktop` to your Linux VM like below:</span></span>

![Kép](./media/remote-desktop/endpoint-for-linux-server.png)

<span data-ttu-id="863be-123">Ha nem tudja, hogyan állíthatja be a végpont a virtuális Gépet, tekintse meg [Ez az útmutató](setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="863be-123">If you don't know how to set up an endpoint for your VM, see [this guidance](setup-endpoints.md).</span></span>

## <a name="install-gnome-desktop"></a><span data-ttu-id="863be-124">Gnome asztali telepítése</span><span class="sxs-lookup"><span data-stu-id="863be-124">Install Gnome Desktop</span></span>
<span data-ttu-id="863be-125">Csatlakoztassa a Linux virtuális Gépet keresztül `putty`, és telepítse `Gnome Desktop`.</span><span class="sxs-lookup"><span data-stu-id="863be-125">Connect to your Linux VM through `putty`, and install `Gnome Desktop`.</span></span>

<span data-ttu-id="863be-126">Ubuntu használja:</span><span class="sxs-lookup"><span data-stu-id="863be-126">For Ubuntu, use:</span></span>

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


<span data-ttu-id="863be-127">OpenSUSE használja:</span><span class="sxs-lookup"><span data-stu-id="863be-127">For OpenSUSE, use:</span></span>

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a><span data-ttu-id="863be-128">Xrdp telepítése</span><span class="sxs-lookup"><span data-stu-id="863be-128">Install xrdp</span></span>
<span data-ttu-id="863be-129">Ubuntu használja:</span><span class="sxs-lookup"><span data-stu-id="863be-129">For Ubuntu, use:</span></span>

    #sudo apt-get install xrdp

<span data-ttu-id="863be-130">OpenSUSE használja:</span><span class="sxs-lookup"><span data-stu-id="863be-130">For OpenSUSE, use:</span></span>

> [!NOTE]
> <span data-ttu-id="863be-131">Frissítse a OpenSUSE a verziójával, az alábbi parancsot használja.</span><span class="sxs-lookup"><span data-stu-id="863be-131">Update the OpenSUSE version with the version you are using in the following command.</span></span> <span data-ttu-id="863be-132">Az alábbi példa `OpenSUSE 13.2`.</span><span class="sxs-lookup"><span data-stu-id="863be-132">The example below is for `OpenSUSE 13.2`.</span></span>
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a><span data-ttu-id="863be-133">Indítsa el a xrdp, és állítsa be xdrp szolgáltatás állítja</span><span class="sxs-lookup"><span data-stu-id="863be-133">Start xrdp and set xdrp service at boot-up</span></span>
<span data-ttu-id="863be-134">OpenSUSE használja:</span><span class="sxs-lookup"><span data-stu-id="863be-134">For OpenSUSE, use:</span></span>

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

<span data-ttu-id="863be-135">Ubuntu, a xrdp elindul és eanbled: állítja automatikusan a telepítés után.</span><span class="sxs-lookup"><span data-stu-id="863be-135">For Ubuntu, xrdp will be started and eanbled at boot-up automatically after installation.</span></span>

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a><span data-ttu-id="863be-136">Ha a Ubuntu 12.04LTS később egy Ubuntu verzióját használja xfce használatával</span><span class="sxs-lookup"><span data-stu-id="863be-136">Using xfce if you are using an Ubuntu version later than Ubuntu 12.04LTS</span></span>
<span data-ttu-id="863be-137">Mert xrdp jelenlegi verziója nem támogatja a Gnome asztali Ubuntu verzióihoz Ubuntu 12.04LTS később, használjuk `xfce` asztali helyette.</span><span class="sxs-lookup"><span data-stu-id="863be-137">Because the current version of xrdp does not support Gnome Desktop for  Ubuntu versions later than Ubuntu 12.04LTS, we will use `xfce` Desktop instead.</span></span>

<span data-ttu-id="863be-138">A telepítendő `xfce`, használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="863be-138">To install `xfce`, use this command:</span></span>

    #sudo apt-get install xubuntu-desktop

<span data-ttu-id="863be-139">Engedélyezze a `xfce` használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="863be-139">Then enable `xfce` using this command:</span></span>

    #echo xfce4-session >~/.xsession

<span data-ttu-id="863be-140">A konfigurációs fájl szerkesztése `/etc/xrdp/startwm.sh`:</span><span class="sxs-lookup"><span data-stu-id="863be-140">Edit the config file `/etc/xrdp/startwm.sh`:</span></span>

    #sudo vi /etc/xrdp/startwm.sh   

<span data-ttu-id="863be-141">Adja hozzá a sort `xfce4-session` sora elé `/etc/X11/Xsession`.</span><span class="sxs-lookup"><span data-stu-id="863be-141">Add the line `xfce4-session` before the line `/etc/X11/Xsession`.</span></span>

<span data-ttu-id="863be-142">Indítsa újra a xrdp szolgáltatást, használja ezt:</span><span class="sxs-lookup"><span data-stu-id="863be-142">To restart the xrdp service, use this:</span></span>

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a><span data-ttu-id="863be-143">Csatlakozás a Linux virtuális Gépet egy Windows-gépről</span><span class="sxs-lookup"><span data-stu-id="863be-143">Connect your Linux VM from a Windows machine</span></span>
<span data-ttu-id="863be-144">A Windows-gépen a távoli asztal ügyfél elindítása, és adjon meg a Linux virtuális gép DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="863be-144">In a Windows machine, start the Remote Desktop client and input your Linux VM DNS name.</span></span> <span data-ttu-id="863be-145">Vagy a virtuális gép az Azure-portálon az irányítópult megnyitásához, és kattintson a `Connect` a Linux virtuális gép csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="863be-145">Or go to the Dashboard of your VM in the Azure portal and click `Connect` to connect your Linux VM.</span></span> <span data-ttu-id="863be-146">Ebben az esetben a bejelentkezési ablak jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="863be-146">In that case, you see the login window:</span></span>

![Kép](./media/remote-desktop/no2.png)

<span data-ttu-id="863be-148">Jelentkezzen be a felhasználónevet és jelszót a Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="863be-148">Log in with the user name and password of your Linux VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="863be-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="863be-149">Next steps</span></span>
<span data-ttu-id="863be-150">Xrdp használatával kapcsolatos további információkért lásd: [http://www.xrdp.org/](http://www.xrdp.org/).</span><span class="sxs-lookup"><span data-stu-id="863be-150">For more information about using xrdp, see [http://www.xrdp.org/](http://www.xrdp.org/).</span></span>
