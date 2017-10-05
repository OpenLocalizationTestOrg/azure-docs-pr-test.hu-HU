---
title: "Az Azure Active Directory tartományi szolgáltatások: Első lépések |} Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure portál használatával"
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
ms.openlocfilehash: 7f420d60862adf61e4f21e5abac2932a742bd55d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a>Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure portál használatával


## <a name="before-you-begin"></a>Előkészületek
Tekintse át a [Hálózati megfontolások az Azure Active Directory Domain Services-hez](active-directory-ds-networking.md) című dokumentumot.


## <a name="task-2-configure-network-settings"></a>2. feladat: a hálózati beállítások konfigurálása
A következő konfigurációs feladat, ha egy Azure virtuális hálózatra és egy dedikált alhálózaton belül. Engedélyezze az Azure Active Directory Domain Services-t a virtuális hálózatának ezen az alhálózatán. Is válasszon egy meglévő virtuális hálózathoz, és hozza létre az alhálózatot, amelyek dedikált belül.

1. Kattintson a **virtuális hálózati** virtuális hálózat kiválasztásához.
2. Az a **válasszon virtuális hálózati** panelen, megjelenik az összes meglévő virtuális hálózatot. Csak a virtuális hálózatok, amelyek a kijelölt erőforráscsoport és Azure-beli hely megjelenik a **alapjai** varázsló lapja.

3. Válassza ki a virtuális hálózatot, amelyben a Azure AD tartományi szolgáltatások engedélyezni kell. Kattintson a **hozzon létre új**, ha szeretné létrehozni az új virtuális hálózat. Erősen ajánlott egy dedikált alhálózati használata az Azure AD tartományi szolgáltatásokhoz. Ha egy meglévő virtuális hálózatot választ [hozzon létre egy külön alhálózatot a virtuális hálózatok kiterjesztéssel](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) , majd válassza ki a alhálózaton használni. 

    ![Válassza ki a virtuális hálózat](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. Kattintson a **alhálózati** a dedikált alhálózat kiválasztása a virtuális hálózaton belül, amely lehetővé teszi az új felügyelt tartomány számára. Az a **hozzon létre alhálózatot** panelen adja meg az alhálózat nevét, és kattintson a **OK** befejezése. Hozzon létre például egy alhálózat neve "DomainServices", így könnyen megérteni, hogy mi történik, az alhálózaton belüli más rendszergazdák számára.

    ![Válassza ki az alhálózatot a virtuális hálózaton belül](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Egy alhálózat kiválasztására vonatkozó irányelvek**
  > 1. Használja a dedikált alhálózatot az Azure AD tartományi szolgáltatásokhoz. Más virtuális gépek nem telepíti az alhálózathoz. Ez a konfiguráció lehetővé teszi hálózati biztonsági csoportokkal (NSG-k) konfigurálja a munkaterhelések virtuális gépek számára a felügyelt tartományok megszakítása nélkül. További információkért lásd: [hálózat az Azure Active Directory tartományi szolgáltatások szempontjai](active-directory-ds-networking.md).
  2. Ne válassza az átjáró alhálózatának az Azure AD tartományi szolgáltatások telepítése, mert nem támogatott konfiguráció.
  3. Győződjön meg arról, hogy a kijelölt alhálózat rendelkezik-e elegendő elérhető címterület - legalább 3-5 elérhető IP-címek.
  >

5. Amikor elkészült, kattintson a **OK** a áthelyezése a **rendszergazdai csoport** a varázsló.


## <a name="next-step"></a>Következő lépés
[3. feladat: a felügyeleti csoport konfigurálása és Azure AD tartományi szolgáltatások engedélyezése](active-directory-ds-getting-started-admingroup.md)
