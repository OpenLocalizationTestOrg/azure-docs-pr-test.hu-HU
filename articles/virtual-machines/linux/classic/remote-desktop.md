---
title: "aaaRemote asztali tooa Linux virtuális gép |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall, és konfigurálja a távoli asztal tooconnect tooa Microsoft Azure Linux virtuális gép hello klasszikus telepítési modell"
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
ms.openlocfilehash: aadd6e87883cf9cacf9d198b680669d594206e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-tooconnect-tooa-microsoft-azure-linux-vm"></a><span data-ttu-id="cae22-103">A távoli asztal tooconnect tooa Microsoft Azure Linux virtuális gép használata</span><span class="sxs-lookup"><span data-stu-id="cae22-103">Using Remote Desktop tooconnect tooa Microsoft Azure Linux VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="cae22-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="cae22-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="cae22-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="cae22-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="cae22-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="cae22-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="cae22-107">Hello frissítése a cikk Resource Manager verziója, a következő témakörben: [Itt](../use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="cae22-107">For hello updated Resource Manager version of this article, see [here](../use-remote-desktop.md).</span></span>

## <a name="overview"></a><span data-ttu-id="cae22-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="cae22-108">Overview</span></span>
<span data-ttu-id="cae22-109">RDP (Remote Desktop Protocol) a saját fejlesztésű protokollja használt Windows.</span><span class="sxs-lookup"><span data-stu-id="cae22-109">RDP (Remote Desktop Protocol) is a proprietary protocol used for Windows.</span></span> <span data-ttu-id="cae22-110">Hogyan használhatjuk RDP tooconnect tooa Linux virtuális gép (virtuális gép) távolról?</span><span class="sxs-lookup"><span data-stu-id="cae22-110">How can we use RDP tooconnect tooa Linux VM (virtual machine) remotely?</span></span>

<span data-ttu-id="cae22-111">Ez az útmutató kap választ hello!</span><span class="sxs-lookup"><span data-stu-id="cae22-111">This guidance will give you hello answer!</span></span> <span data-ttu-id="cae22-112">Ez segítséget nyújt a Microsoft Azure Linux virtuális gép meg, amely lehetővé teszi a tooit csatlakozást a távoli asztalról a Windows-gépről a tooinstall és config xrdp.</span><span class="sxs-lookup"><span data-stu-id="cae22-112">It will help you tooinstall and config xrdp on your Microsoft Azure Linux VM, which lets you connect tooit with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="cae22-113">Linux virtuális gép futtatásához használt Ubuntu, OpenSUSE vagy hello példa az ebben az útmutatóban használjuk.</span><span class="sxs-lookup"><span data-stu-id="cae22-113">We will use Linux VM running Ubuntu or OpenSUSE as hello example in this guidance.</span></span>

<span data-ttu-id="cae22-114">hello xrdp eszköze a megnyitott forrás, amely lehetővé teszi tooconnect RDP-kiszolgáló a Linux-kiszolgálóra a távoli asztalról a Windows-gépről.</span><span class="sxs-lookup"><span data-stu-id="cae22-114">hello xrdp tool is an open source RDP server that allows you tooconnect your Linux server with Remote Desktop from a Windows machine.</span></span> <span data-ttu-id="cae22-115">RDP (virtuális hálózat számítástechnikai) VNC jobb teljesítményt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cae22-115">RDP has better performance than VNC (Virtual Network Computing).</span></span> <span data-ttu-id="cae22-116">VNC Renderelés JPEG minőségű grafikus használatával, és a lassú lehet, mivel az RDP gyors és egyszerű crystal.</span><span class="sxs-lookup"><span data-stu-id="cae22-116">VNC renders using JPEG-quality graphics and can be slow, whereas RDP is fast and crystal clear.</span></span>

> [!NOTE]
> <span data-ttu-id="cae22-117">Már rendelkeznie kell egy Microsoft Azure virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="cae22-117">You must already have an Microsoft Azure VM running Linux.</span></span> <span data-ttu-id="cae22-118">toocreate, és állítsa be a Linux virtuális gép, lásd: hello [Azure Linux virtuális gép oktatóanyag](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="cae22-118">toocreate and set up a Linux VM, see hello [Azure Linux VM tutorial](createportal.md).</span></span>
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a><span data-ttu-id="cae22-119">Hozzon létre egy végpontot a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="cae22-119">Create an endpoint for Remote Desktop</span></span>
<span data-ttu-id="cae22-120">Használjuk hello alapértelmezett végpont 3389-es távoli asztal Ez a dokumentum. Állítsa be, 3389 végpont `Remote Desktop` tooyour Linux virtuális gép alatt, például:</span><span class="sxs-lookup"><span data-stu-id="cae22-120">We will use hello default endpoint 3389 for Remote Desktop in this doc. Set up 3389 endpoint as `Remote Desktop` tooyour Linux VM like below:</span></span>

![Kép](./media/remote-desktop/endpoint-for-linux-server.png)

<span data-ttu-id="cae22-122">Ha nem tudja, hogyan végpont a virtuális gép mentése tooset: [Ez az útmutató](setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="cae22-122">If you don't know how tooset up an endpoint for your VM, see [this guidance](setup-endpoints.md).</span></span>

## <a name="install-gnome-desktop"></a><span data-ttu-id="cae22-123">Gnome asztali telepítése</span><span class="sxs-lookup"><span data-stu-id="cae22-123">Install Gnome Desktop</span></span>
<span data-ttu-id="cae22-124">Csatlakozzon a Linux virtuális gép tooyour keresztül `putty`, és telepítse `Gnome Desktop`.</span><span class="sxs-lookup"><span data-stu-id="cae22-124">Connect tooyour Linux VM through `putty`, and install `Gnome Desktop`.</span></span>

<span data-ttu-id="cae22-125">Ubuntu használja:</span><span class="sxs-lookup"><span data-stu-id="cae22-125">For Ubuntu, use:</span></span>

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


<span data-ttu-id="cae22-126">OpenSUSE használja:</span><span class="sxs-lookup"><span data-stu-id="cae22-126">For OpenSUSE, use:</span></span>

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a><span data-ttu-id="cae22-127">Xrdp telepítése</span><span class="sxs-lookup"><span data-stu-id="cae22-127">Install xrdp</span></span>
<span data-ttu-id="cae22-128">Ubuntu használja:</span><span class="sxs-lookup"><span data-stu-id="cae22-128">For Ubuntu, use:</span></span>

    #sudo apt-get install xrdp

<span data-ttu-id="cae22-129">OpenSUSE használja:</span><span class="sxs-lookup"><span data-stu-id="cae22-129">For OpenSUSE, use:</span></span>

> [!NOTE]
> <span data-ttu-id="cae22-130">Hello OpenSUSE verzióját használja a következő parancs hello hello verziójával frissíti.</span><span class="sxs-lookup"><span data-stu-id="cae22-130">Update hello OpenSUSE version with hello version you are using in hello following command.</span></span> <span data-ttu-id="cae22-131">az alábbi hello példa `OpenSUSE 13.2`.</span><span class="sxs-lookup"><span data-stu-id="cae22-131">hello example below is for `OpenSUSE 13.2`.</span></span>
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a><span data-ttu-id="cae22-132">Indítsa el a xrdp, és állítsa be xdrp szolgáltatás állítja</span><span class="sxs-lookup"><span data-stu-id="cae22-132">Start xrdp and set xdrp service at boot-up</span></span>
<span data-ttu-id="cae22-133">OpenSUSE használja:</span><span class="sxs-lookup"><span data-stu-id="cae22-133">For OpenSUSE, use:</span></span>

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

<span data-ttu-id="cae22-134">Ubuntu, a xrdp elindul és eanbled: állítja automatikusan a telepítés után.</span><span class="sxs-lookup"><span data-stu-id="cae22-134">For Ubuntu, xrdp will be started and eanbled at boot-up automatically after installation.</span></span>

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a><span data-ttu-id="cae22-135">Ha a Ubuntu 12.04LTS később egy Ubuntu verzióját használja xfce használatával</span><span class="sxs-lookup"><span data-stu-id="cae22-135">Using xfce if you are using an Ubuntu version later than Ubuntu 12.04LTS</span></span>
<span data-ttu-id="cae22-136">Mert hello xrdp jelenlegi verziója nem támogatja a Gnome asztali Ubuntu verzióihoz Ubuntu 12.04LTS később, használjuk `xfce` asztali helyette.</span><span class="sxs-lookup"><span data-stu-id="cae22-136">Because hello current version of xrdp does not support Gnome Desktop for  Ubuntu versions later than Ubuntu 12.04LTS, we will use `xfce` Desktop instead.</span></span>

<span data-ttu-id="cae22-137">tooinstall `xfce`, használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="cae22-137">tooinstall `xfce`, use this command:</span></span>

    #sudo apt-get install xubuntu-desktop

<span data-ttu-id="cae22-138">Engedélyezze a `xfce` használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="cae22-138">Then enable `xfce` using this command:</span></span>

    #echo xfce4-session >~/.xsession

<span data-ttu-id="cae22-139">Hello konfigurációs fájl szerkesztésével `/etc/xrdp/startwm.sh`:</span><span class="sxs-lookup"><span data-stu-id="cae22-139">Edit hello config file `/etc/xrdp/startwm.sh`:</span></span>

    #sudo vi /etc/xrdp/startwm.sh   

<span data-ttu-id="cae22-140">Adja hozzá a sort hello `xfce4-session` hello sor előtt `/etc/X11/Xsession`.</span><span class="sxs-lookup"><span data-stu-id="cae22-140">Add hello line `xfce4-session` before hello line `/etc/X11/Xsession`.</span></span>

<span data-ttu-id="cae22-141">toorestart hello xrdp szolgáltatást, használja ezt:</span><span class="sxs-lookup"><span data-stu-id="cae22-141">toorestart hello xrdp service, use this:</span></span>

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a><span data-ttu-id="cae22-142">Csatlakozás a Linux virtuális Gépet egy Windows-gépről</span><span class="sxs-lookup"><span data-stu-id="cae22-142">Connect your Linux VM from a Windows machine</span></span>
<span data-ttu-id="cae22-143">A Windows-gépen hello távoli asztal ügyfél elindítása, és adjon meg a Linux virtuális gép DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="cae22-143">In a Windows machine, start hello Remote Desktop client and input your Linux VM DNS name.</span></span> <span data-ttu-id="cae22-144">Vagy nyissa meg a virtuális gép az Azure-portálon hello irányítópult toohello, és kattintson `Connect` tooconnect a Linux virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="cae22-144">Or go toohello Dashboard of your VM in hello Azure portal and click `Connect` tooconnect your Linux VM.</span></span> <span data-ttu-id="cae22-145">Ebben az esetben hello bejelentkezés ablak jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="cae22-145">In that case, you see hello login window:</span></span>

![Kép](./media/remote-desktop/no2.png)

<span data-ttu-id="cae22-147">Jelentkezzen be a hello felhasználónevét és jelszavát a Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="cae22-147">Log in with hello user name and password of your Linux VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cae22-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cae22-148">Next steps</span></span>
<span data-ttu-id="cae22-149">Xrdp használatával kapcsolatos további információkért lásd: [http://www.xrdp.org/](http://www.xrdp.org/).</span><span class="sxs-lookup"><span data-stu-id="cae22-149">For more information about using xrdp, see [http://www.xrdp.org/](http://www.xrdp.org/).</span></span>
