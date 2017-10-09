---
title: "az Azure felhőalapú szolgáltatások szerepkör távoli asztali kapcsolat aaaEnable |} Microsoft Docs"
description: "Hogyan tooconfigure az azure felhőalapú szolgáltatás alkalmazás tooallow távoli asztali kapcsolatok"
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: 73ea1d64-1529-4d72-b58e-f6c10499e6bb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: mmccrory
ms.openlocfilehash: 55d7043df571c2e88b04aa9ef01dc8ae1d6784f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Engedélyezze a távoli asztali kapcsolat egy szerepkör esetén az Azure Cloud Services csomag
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [klasszikus Azure portál](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)
>
>

A távoli asztal tooaccess hello asztali egy Azure-beli szerepkör lehetővé teszi. A távoli asztali kapcsolat tootroubleshoot használja, és hibáinak az alkalmazás futtatása során.

Engedélyezheti a távoli asztali kapcsolat létrehozása a szerepkörben a fejlesztés során hello távoli asztal modulok belefoglalja a szolgáltatás definíciós, vagy választhatja azt a távoli asztal tooenable hello távoli asztali bővítmény keresztül. hello előnyben részesített megoldás, toouse hello távoli asztali bővítmény, a távoli asztal hello alkalmazás anélkül, hogy tooredeploy az alkalmazás központi telepítése után is engedélyezheti.

## <a name="configure-remote-desktop-from-hello-azure-portal"></a>Állítsa a távoli asztal hello Azure-portálon
hello Azure-portálon hello a távoli asztali bővítmény módszert használ, így a távoli asztal hello alkalmazás központi telepítése után is engedélyezheti. Hello **távoli asztal** panel a felhőalapú szolgáltatás lehetővé teszi a távoli asztal tooenable használt tooconnect toohello virtuális gépek módosítása hello helyi rendszergazdai fiókot, hello tanúsítvány-hitelesítésben használt, és állítsa be a hello lejárati dátuma.

1. Kattintson a **Felhőszolgáltatások**, kattintson hello felhőszolgáltatás hello nevét, majd **távoli asztal**.

    ![Cloud services távoli asztal](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Válassza ki, hogy a távoli asztal tooenable szeretné, hogy egy adott szerepkör vagy az összes szerepkör, majd hello kapcsoló hello értékének módosítása túl**engedélyezve**.

3. Töltse ki a felhasználónevet, jelszót, lejárati és tanúsítvány hello kötelező mezőket.

    ![Cloud services távoli asztal](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > Összes szerepkörpéldányt újra kell indítani, amikor először a távoli asztal engedélyezése és kattintson az OK gombra (jelölő). Újraindítás, hello tanúsítványjelszavas használt tooencrypt hello tooprevent hello szerepkört telepíteni kell. Újraindítás, tooprevent [hello felhőszolgáltatás tanúsítvány feltöltése](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) , és visszatér a toothis párbeszédpanel.
   >
   >
3. A **szerepkörök**, jelölje be hello szerepkör tooupdate kívánt **összes** összes szerepköre tekintetében.

4. A konfigurációs módosítások befejeztével kattintson **mentése**. Eltarthat egy kis ideig, mielőtt a szerepkörpéldányok készen tooreceive kapcsolatok.

## <a name="remote-into-role-instances"></a>A szerepkörpéldányok távoli
A távoli asztal hello szerepkörök engedélyezése után közvetlenül az Azure portál hello kapcsolatot is kezdeményezhető:

1. Kattintson a **példányok** tooopen hello **példányok** panelen.
2. Válassza ki a szerepkör példánya, amely a távoli asztal konfigurálva van.
3. Kattintson a **Connect** toodownload egy RDP-fájl hello szerepkörpéldányhoz.

    ![Cloud services távoli asztal](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Kattintson a **nyitott** , majd **Connect** toostart hello távoli asztali kapcsolat.

>[!NOTE]
> Ha a felhőszolgáltatás ülve mögött egy NSG-t, szükség lehet a toocreate szabályok, amelyek lehetővé teszik a forgalom a portokon **3389-es** és **20000**.  A távoli asztal-portot használja **3389-es**.  Felhőalapú szolgáltatás példányainak kerül, így nem közvetlenül szabályozhatja, hogy melyik példányt tooconnect számára.  Hello *RemoteForwarder* és *RemoteAccess* ügynökök RDP-forgalom kezelésére és hello ügyfél toosend RDP cookie-k engedélyezése, és adjon meg egy egyedi példány tooconnect való.  Hello *RemoteForwarder* és *RemoteAccess* ügynökök szükséges, hogy a port **20000*** nyitható meg, amely blokkolhatja, ha egy NSG.

## <a name="additional-resources"></a>További források

[Hogyan tooConfigure Felhőszolgáltatások](cloud-services-how-to-configure.md)
[Cloud services – gyakori kérdések – a távoli asztal](cloud-services-faq.md)
