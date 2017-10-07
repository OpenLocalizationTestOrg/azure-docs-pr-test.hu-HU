---
title: "Az Azure Active Directory tartományi szolgáltatások: Első lépések |} Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure-portálon hello használata"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: 7695dabb11df8d9dcfdac24996edf021af2e7f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a>Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure-portálon hello használata


## <a name="before-you-begin"></a>Előkészületek
Tekintse meg a túl[Azure Active Directory tartományi szolgáltatások hálózati szempontjai](active-directory-ds-networking.md).


## <a name="task-2-configure-network-settings"></a>2. feladat: a hálózati beállítások konfigurálása
hello következő konfigurációs feladat toocreate van egy Azure virtuális hálózatra és egy dedikált alhálózaton belül. Engedélyezze az Azure Active Directory Domain Services-t a virtuális hálózatának ezen az alhálózatán. Előfordulhat, hogy válasszon egy meglévő virtuális hálózathoz, és hozzon létre dedikált hello alhálózaton belül.

1. Kattintson a **virtuális hálózati** tooselect egy virtuális hálózatot.
2. A hello **válasszon virtuális hálózati** panelen, megjelenik az összes meglévő virtuális hálózatot. Csak hello virtuális hálózatok toohello erőforráscsoport és az Azure-beli hely hello a kijelölt tartozó látja **alapjai** varázsló lapja.

3. Válasszon hello virtuális hálózatot, amelyben a Azure AD tartományi szolgáltatások engedélyezni kell. Kattintson a **hozzon létre új**, ha szeretné toocreate egy új virtuális hálózat. Erősen ajánlott egy dedikált alhálózati használata az Azure AD tartományi szolgáltatásokhoz. Ha egy meglévő virtuális hálózatot választ [hozzon létre egy külön alhálózatot hello virtuális hálózatok kiterjesztéssel](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) , majd válassza ki a alhálózaton használni. 

    ![Válassza ki a virtuális hálózat](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. Kattintson a **alhálózati** toopick hello dedikált alhálózatot a virtuális hálózaton, mely tooenable belül az új kezelt tartományban található. A hello **alhálózati létrehozása** panelen hello alhálózat nevét adja meg, és kattintson a **OK** befejezése. Hozzon létre például egy alhálózat hello neve "DomainServices", így könnyen más rendszergazdák toounderstand mi hello alhálózaton belül történik.

    ![Válassza ki az alhálózatot hello virtuális hálózaton belül](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Egy alhálózat kiválasztására vonatkozó irányelvek**
  > 1. Használja a dedikált alhálózatot az Azure AD tartományi szolgáltatásokhoz. Ne telepítsen más virtuális gépek toothis alhálózathoz. Ez a konfiguráció lehetővé teszi tooconfigure hálózati biztonsági csoportokkal (NSG-k) a munkaterhelések virtuális gépek számára a felügyelt tartományok megszakítása nélkül. További információkért lásd: [hálózat az Azure Active Directory tartományi szolgáltatások szempontjai](active-directory-ds-networking.md).
  2. Csak akkor válassza hello átjáró-alhálózatot az Azure AD tartományi szolgáltatások telepítése, mert már nem támogatott konfiguráció.
  3. Győződjön meg arról, választott hello alhálózaton nincs elegendő elérhető címek lemezterület - legalább 3-5 elérhető IP-címek.
  >

5. Amikor elkészült, kattintson a **OK** a toohello toomove **rendszergazdai csoport** hello varázsló.


## <a name="next-step"></a>Következő lépés
[3. feladat: a felügyeleti csoport konfigurálása és Azure AD tartományi szolgáltatások engedélyezése](active-directory-ds-getting-started-admingroup.md)
