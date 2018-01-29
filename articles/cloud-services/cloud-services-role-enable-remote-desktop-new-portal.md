---
title: "Az Azure felhőalapú szolgáltatások szerepkör távoli asztali kapcsolat engedélyezése |} Microsoft Docs"
description: "Az azure cloud service alkalmazás távoli asztali kapcsolatok lehetővé tételéhez konfigurálása"
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
ms.openlocfilehash: b9ae4442f57170746eb0de94849b09625be51264
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/21/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Engedélyezze a távoli asztali kapcsolat egy szerepkör esetén az Azure Cloud Services csomag
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

A távoli asztal lehetővé teszi az asztalon, egy Azure-beli szerepkör eléréséhez. A távoli asztali kapcsolat segítségével hibaelhárítását és diagnosztizálását az alkalmazás futtatása során.

Engedélyezheti a távoli asztali kapcsolat létrehozása a szerepkörben fejlesztése során a távoli asztal modulok belefoglalja a szolgáltatás definíciós, vagy választhatja azt is, a távoli asztal bővítményével távoli asztal engedélyezése. Az előnyben részesített megoldás, a távoli asztali bővítmény használatára, mert a távoli asztal az alkalmazás nem kell újratelepíteni az alkalmazás központi telepítése után is engedélyezheti.

## <a name="configure-remote-desktop-from-the-azure-portal"></a>A távoli asztal konfigurálása az Azure-portálon
Az Azure-portálon a távoli asztali bővítmény megközelítést használ, így a távoli asztal az alkalmazás központi telepítése után is engedélyezheti. A **távoli asztal** panel a felhőalapú szolgáltatás lehetővé teszi a távoli asztal engedélyezéséhez módosítsa a helyi rendszergazda fiók használatával kapcsolódik a virtuális gépek, a tanúsítvány-hitelesítésben használt, és a lejárati dátuma.

1. Kattintson a **Felhőszolgáltatások**, kattintson a nevére, a felhőalapú szolgáltatás, és kattintson a **távoli asztal**.

    ![Cloud services távoli asztal](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Válassza ki, hogy szeretné-e a távoli asztal engedélyezése egy adott szerepkör vagy minden szerepkört, majd módosítsa a kapcsoló értékének **engedélyezve**.

3. Adja meg a felhasználónevet, jelszót, lejárati és tanúsítvány kötelező mezők.

    ![Cloud services távoli asztal](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > Összes szerepkörpéldányt újra kell indítani, amikor először a távoli asztal engedélyezése és kattintson az OK gombra (jelölő). Újraindítás megakadályozása érdekében a jelszó titkosításához használt tanúsítványról telepítenie kell a szerepkört. Egy újraindítás érdekében [töltse fel a tanúsítványt a felhőalapú szolgáltatáshoz](cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate) és térjen vissza ezt a párbeszédpanelt.
   >
   >
3. A **szerepkörök**, válassza ki a frissíteni, vagy válassza ki a szerepkör **összes** összes szerepköre tekintetében.

4. A konfigurációs módosítások befejeztével kattintson **mentése**. Eltarthat egy kis ideig, mielőtt a szerepkörpéldányok készen áll kapcsolatok fogadására.

## <a name="remote-into-role-instances"></a>A szerepkörpéldányok távoli
Távoli asztal szerepkörök engedélyezése után a kapcsolat az Azure portálról közvetlenül is kezdeményezhető:

1. Kattintson a **példányok** megnyitásához a **példányok** panelen.
2. Válassza ki a szerepkör példánya, amely a távoli asztal konfigurálva van.
3. Kattintson a **Connect** a szerepkörpéldányhoz RDP-fájl letöltésére.

    ![Cloud services távoli asztal](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Kattintson a **nyitott** , majd **Connect** a távoli asztali kapcsolat elindításához.

>[!NOTE]
> Ha a felhőszolgáltatás ülve mögött egy NSG-t, szükség lehet létrehozhat szabályokat, amelyek lehetővé teszik a forgalom a portokon **3389-es** és **20000**.  A távoli asztal-portot használja **3389-es**.  Felhőalapú szolgáltatás példányainak kerül, így nem közvetlenül szabályozhatja, hogy melyik példányt való csatlakozáshoz.  A *RemoteForwarder* és *RemoteAccess* ügynökök kezelése RDP-forgalmát, és az ügyfél küldése az RDP cookie, és adjon meg egy egyedi példány való csatlakozáshoz.  A *RemoteForwarder* és *RemoteAccess* ügynökök szükséges, hogy a port **20000*** nyitható meg, amely blokkolhatja, ha egy NSG.

## <a name="additional-resources"></a>További források

[Felhőalapú szolgáltatások konfigurálása](cloud-services-how-to-configure-portal.md)
[Cloud services – gyakori kérdések – a távoli asztal](cloud-services-faq.md)
