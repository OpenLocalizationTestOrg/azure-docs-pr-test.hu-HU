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
# <a name="using-remote-desktop-tooconnect-tooa-microsoft-azure-linux-vm"></a>A távoli asztal tooconnect tooa Microsoft Azure Linux virtuális gép használata
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Hello frissítése a cikk Resource Manager verziója, a következő témakörben: [Itt](../use-remote-desktop.md).

## <a name="overview"></a>Áttekintés
RDP (Remote Desktop Protocol) a saját fejlesztésű protokollja használt Windows. Hogyan használhatjuk RDP tooconnect tooa Linux virtuális gép (virtuális gép) távolról?

Ez az útmutató kap választ hello! Ez segítséget nyújt a Microsoft Azure Linux virtuális gép meg, amely lehetővé teszi a tooit csatlakozást a távoli asztalról a Windows-gépről a tooinstall és config xrdp. Linux virtuális gép futtatásához használt Ubuntu, OpenSUSE vagy hello példa az ebben az útmutatóban használjuk.

hello xrdp eszköze a megnyitott forrás, amely lehetővé teszi tooconnect RDP-kiszolgáló a Linux-kiszolgálóra a távoli asztalról a Windows-gépről. RDP (virtuális hálózat számítástechnikai) VNC jobb teljesítményt rendelkezik. VNC Renderelés JPEG minőségű grafikus használatával, és a lassú lehet, mivel az RDP gyors és egyszerű crystal.

> [!NOTE]
> Már rendelkeznie kell egy Microsoft Azure virtuális gépet. toocreate, és állítsa be a Linux virtuális gép, lásd: hello [Azure Linux virtuális gép oktatóanyag](createportal.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Hozzon létre egy végpontot a távoli asztal
Használjuk hello alapértelmezett végpont 3389-es távoli asztal Ez a dokumentum. Állítsa be, 3389 végpont `Remote Desktop` tooyour Linux virtuális gép alatt, például:

![Kép](./media/remote-desktop/endpoint-for-linux-server.png)

Ha nem tudja, hogyan végpont a virtuális gép mentése tooset: [Ez az útmutató](setup-endpoints.md).

## <a name="install-gnome-desktop"></a>Gnome asztali telepítése
Csatlakozzon a Linux virtuális gép tooyour keresztül `putty`, és telepítse `Gnome Desktop`.

Ubuntu használja:

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


OpenSUSE használja:

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a>Xrdp telepítése
Ubuntu használja:

    #sudo apt-get install xrdp

OpenSUSE használja:

> [!NOTE]
> Hello OpenSUSE verzióját használja a következő parancs hello hello verziójával frissíti. az alábbi hello példa `OpenSUSE 13.2`.
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Indítsa el a xrdp, és állítsa be xdrp szolgáltatás állítja
OpenSUSE használja:

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

Ubuntu, a xrdp elindul és eanbled: állítja automatikusan a telepítés után.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Ha a Ubuntu 12.04LTS később egy Ubuntu verzióját használja xfce használatával
Mert hello xrdp jelenlegi verziója nem támogatja a Gnome asztali Ubuntu verzióihoz Ubuntu 12.04LTS később, használjuk `xfce` asztali helyette.

tooinstall `xfce`, használja ezt a parancsot:

    #sudo apt-get install xubuntu-desktop

Engedélyezze a `xfce` használja a következő parancsot:

    #echo xfce4-session >~/.xsession

Hello konfigurációs fájl szerkesztésével `/etc/xrdp/startwm.sh`:

    #sudo vi /etc/xrdp/startwm.sh   

Adja hozzá a sort hello `xfce4-session` hello sor előtt `/etc/X11/Xsession`.

toorestart hello xrdp szolgáltatást, használja ezt:

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Csatlakozás a Linux virtuális Gépet egy Windows-gépről
A Windows-gépen hello távoli asztal ügyfél elindítása, és adjon meg a Linux virtuális gép DNS-nevét. Vagy nyissa meg a virtuális gép az Azure-portálon hello irányítópult toohello, és kattintson `Connect` tooconnect a Linux virtuális Gépet. Ebben az esetben hello bejelentkezés ablak jelenik meg:

![Kép](./media/remote-desktop/no2.png)

Jelentkezzen be a hello felhasználónevét és jelszavát a Linux virtuális gép.

## <a name="next-steps"></a>Következő lépések
Xrdp használatával kapcsolatos további információkért lásd: [http://www.xrdp.org/](http://www.xrdp.org/).
