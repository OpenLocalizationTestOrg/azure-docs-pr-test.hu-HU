---
title: "Active Directory Domain Services: Virtuális hálózat létrehozása vagy kiválasztása | Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 212c741b20e846742d94f70342c4263ce8984153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a>Virtuális hálózat létrehozása vagy kiválasztása az Azure Active Directory Domain Services-hez
## <a name="before-you-begin"></a>Előkészületek
Tekintse meg a túl[Azure Active Directory tartományi szolgáltatások hálózati szempontjai](active-directory-ds-networking.md).

## <a name="task-2-create-an-azure-virtual-network"></a>2. feladat: Azure-alapú virtuális hálózat létrehozása
hello következő konfigurációs feladat toocreate van egy Azure virtuális hálózatra és egy alhálózaton belül. Engedélyezze az Azure Active Directory Domain Services-t a virtuális hálózatának ezen az alhálózatán. Ha egy meglévő virtuális hálózatot, hogy szeretne-e toouse, kihagyhatja ezt a lépést.

> [!NOTE]
> Győződjön meg arról, hogy hello Azure virtuális hálózat létrehozása vagy kiválasztása az Azure Active Directory tartományi szolgáltatások toouse tooan Azure-régió, Azure Active Directory tartományi szolgáltatások által támogatott tartozik. tooascertain hello Azure-régiók, amelyben az Azure Active Directory tartományi szolgáltatások című [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/).
>
>Megjegyzés: hello neve hello virtuális hálózati tooensure hello megfelelő virtuális hálózatot válassza ki, ha engedélyezte az Azure Active Directory tartományi szolgáltatások egy későbbi konfigurációs lépésben.


toocreate kívánt tooenable Azure Active Directory tartományi szolgáltatások, Azure virtuális hálózat kövesse az alábbi konfigurációs utasításokat:

1. Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldali ablaktáblában jelöljön ki **hálózatok**.

    ![Networks (Hálózatok) csomópont](./media/active-directory-domain-services-getting-started/networks-node.png)  
    Hello **virtuális hálózatok** ablak nyílik meg.
3. Hello munkaablakban hello ablak hello alján, kattintson a **új**.

    ![Virtual Networks (Virtuális hálózatok) ablak](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. Kattintson a **Network Services** (Hálózati szolgáltatások), majd a **Virtual Network** (Virtuális hálózat) elemre.

    ![Virtuális hálózat – gyors létrehozás](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. Kattintson egy virtuális hálózati toocreate **Gyorslétrehozás**.

6. Adjon meg egy **neve** a virtuális hálózat, és fontolja meg az alábbiak hello:
    * Kiválaszthatja a tooconfigure **Címtéren** vagy **virtuális gépek maximális számát** ehhez a hálózathoz.
    * Meghagyhatja hello **DNS-kiszolgáló** beállítását **nincs** most. Azure Active Directory tartományi szolgáltatások engedélyezése után hello beállítás frissítheti.
7. A hello **hely** legördülő listára, válassza ki a támogatott Azure-régiót.  
    tooascertain hello Azure-régiók, amelyben az Azure Active Directory tartományi szolgáltatások című [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/).
8. toocreate a virtuális hálózaton, kattintson a **hozzon létre egy virtuális hálózatot**.

    ![Virtuális hálózat létrehozása az Azure Active Directory Domain Services-hez](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. Miután létrehozta a virtuális hálózaton, válassza ki a virtuális hálózat hello hello nevét, és kattintson a hello **konfigurálása** fülre.

    ![Alhálózat létrehozása](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. A **virtuális hálózat**, kattintson a **alhálózat hozzáadása**, majd adja meg az alhálózat hello nevű **AaddsSubnet**.

    ![Alhálózat létrehozása az Azure Active Directory Domain Services-hez](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. toocreate hello alhálózati, kattintson a **mentése**.


## <a name="next-step"></a>Következő lépés
[3. feladat: Az Active Directory Domain Services engedélyezése](active-directory-ds-getting-started-enableaadds.md)
