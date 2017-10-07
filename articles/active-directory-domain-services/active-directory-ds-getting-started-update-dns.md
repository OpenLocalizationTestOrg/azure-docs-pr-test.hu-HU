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
ms.openlocfilehash: 484ff1a197a651bccb2b416448056acf69b0d8c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-dns-settings-for-hello-azure-virtual-network"></a>Hello Azure-beli virtuális hálózat DNS-beállításainak frissítése
## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a>4. feladat: Frissítése hello Azure-beli virtuális hálózat DNS-beállításait
Hello megelőző konfigurációs feladatok Azure Active Directory tartományi szolgáltatások a címtáron sikeresen engedélyezte. hello következő feladata, hogy hello virtuális hálózaton belüli számítógépek csatlakozhat, és ezek a szolgáltatások felhasználásához tooensure. Ebben a cikkben frissítenie meg a virtuális hálózati toopoint toohello két IP-címek esetén érhető el a virtuális hálózati hello Azure Active Directory tartományi szolgáltatások hello DNS-kiszolgáló beállításai.

> [!NOTE]
> Miután engedélyezte az Azure Active Directory tartományi szolgáltatások hello könyvtár, vegye figyelembe hello IP-címek az Azure Active Directory tartományi szolgáltatások hello megjelenő **konfigurálása** a címtár lapján.
>
>

tooupdate hello DNS-kiszolgáló beállítása a hello virtuális hálózatot, amelyben engedélyezte az Azure Active Directory tartományi szolgáltatások teljes hello a következő lépéseket:

1. Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldali ablaktáblában jelöljön ki **hálózatok**.  
    Hello **hálózatok** ablak nyílik meg.

    ![Virtual networks (Virtuális hálózatok) ablak](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. A hello **virtuális hálózatok** lap, válassza hello virtuális hálózatot, amelyiken engedélyezte Azure Active Directory tartományi szolgáltatások tooview tulajdonságát.
4. Kattintson a hello **konfigurálása** fülre.

    ![Virtual networks (Virtuális hálózatok) ablak](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. A hello **DNS-kiszolgálók** területen adja meg mindkét hello IP-címek hello megjelenő **tartományi szolgáltatások** szakaszt, hello **konfigurálása** a címtár lapján.
6. toosave hello DNS-kiszolgáló beállításai a virtuális hálózat hello munkaablakban hello ablakban hello alján kattintson **mentése**.

   ![Hello hello virtuális hálózat DNS-kiszolgáló beállításainak frissítése](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  Hello hálózatban lévő virtuális gépek hello új DNS-beállítások csak get újraindítás után. Ha módosítania kell azokat tooget hello frissített DNS-beállítások azonnal, indítás, akár hello portálon, a PowerShell vagy a hello CLI újraindítás.
>
>

## <a name="next-steps"></a>Következő lépések
5. feladat: [jelszó-szinkronizálás tooAzure Active Directory tartományi szolgáltatások engedélyezése](active-directory-ds-getting-started-password-sync.md)
