---
title: "Az Azure Active Directory tartományi szolgáltatások: Frissítse a hello Azure-beli virtuális hálózat DNS-beállítások |} Microsoft Docs"
description: "Első lépések az Azure Active Directory tartományi szolgáltatások használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: maheshu
ms.openlocfilehash: e6eaff555cb9b7bb89ab7581d8de0b8cfc844529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-preview"></a>Az Azure Active Directory Domain Services engedélyezése (előzetes verzió)

## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a>4. feladat: hello Azure-beli virtuális hálózat DNS-beállításainak frissítése
Hello megelőző konfigurációs feladatok Azure Active Directory tartományi szolgáltatások a címtáron sikeresen engedélyezte. hello következő feladata, hogy hello virtuális hálózaton belüli számítógépek csatlakozhat, és ezek a szolgáltatások felhasználásához tooensure. Ebben a cikkben frissítenie meg a virtuális hálózati toopoint toohello két IP-címek esetén érhető el a virtuális hálózati hello Azure Active Directory tartományi szolgáltatások hello DNS-kiszolgáló beállításai.

tooupdate hello DNS-kiszolgáló beállítása a hello virtuális hálózatot, amelyben engedélyezte az Azure Active Directory tartományi szolgáltatások teljes hello a következő lépéseket:

1. Hello **áttekintése** lapon számos olyan **szükséges konfigurációs lépések** toobe hajtson végre, miután a felügyelt tartományok teljesen ki van építve. hello első konfigurációs lépés **DNS-kiszolgáló beállításainak frissítése a virtuális hálózat**.

    ![Tartományi szolgáltatások – Áttekintés lap a teljes kiépítés után](./media/getting-started/domain-services-provisioned-overview.png)

2. A tartomány teljes kiépítését követően két IP-cím jelenik meg ezen a csempén. Mindkét IP-cím a felügyelt tartományhoz tartozó tartományvezérlőt jelöli.

3. toocopy hello első IP tooclipboard cím, kattintson a Tovább tooit a hello Másolás gombra. Kattintson a hello **konfigurálja a DNS-kiszolgálók** gombra.

4. Illessze be a hello első IP-címet a hello **hozzáadása a DNS-kiszolgáló** textbox hello **DNS-kiszolgálók** panelen. Görgessen vízszintesen toohello bal oldali toocopy hello második IP-címet, majd illessze be hello **hozzáadása a DNS-kiszolgáló** szövegmező.

    ![Tartományi szolgáltatások – DNS frissítése](./media/getting-started/domain-services-update-dns.png)

5. Kattintson a **mentése** Ha elkészült a tooupdate hello DNS-kiszolgálók hello virtuális hálózat.

> [!NOTE]
> Hello hálózatban lévő virtuális gépek hello új DNS-beállítások csak get újraindítás után. Ha módosítania kell azokat tooget hello frissített DNS-beállítások azonnal, indítás, akár hello portálon, a PowerShell vagy a hello CLI újraindítás.
>
>

## <a name="next-step"></a>Következő lépés
[5. feladat: jelszó-szinkronizálás tooAzure Active Directory tartományi szolgáltatások engedélyezése](active-directory-ds-getting-started-password-sync.md)
