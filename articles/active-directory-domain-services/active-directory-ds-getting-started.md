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
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 47507096a6245d4f1ba57a652ddf5167b3776ae9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a>Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure portál használatával
Ez a cikk bemutatja, hogyan engedélyezze az Azure Active Directory tartományi szolgáltatások (az Azure Active Directory tartományi szolgáltatások) az Azure portál használatával.


Elindíthatja a **engedélyezése az Azure AD tartományi szolgáltatások** varázslóban kövesse az alábbi lépéseket:

1. Nyissa meg az [Azure Portal](https://portal.azure.com).
2. A bal oldali ablaktáblájában kattintson a **új**.
3. Az a **új** panelen, írja be **tartományi szolgáltatások** azokat a keresési sávon.

    ![Keresse meg a tartományi szolgáltatások](./media/getting-started/search-domain-services.png)

4. Kattintással jelölje ki **Azure AD tartományi szolgáltatások** keresési javaslatok listája. Az a **Azure AD tartományi szolgáltatások** panelen kattintson a **létrehozása** gombra.

    ![Tartományi szolgáltatások panel](./media/getting-started/domain-services-blade.png)

5. A **engedélyezése az Azure AD tartományi szolgáltatások** varázsló elindul.


## <a name="task-1-configure-basic-settings"></a>1. feladat: konfigurálja az alapbeállításokat
Az a **alapjai** lap a varázslóban megadhatja a DNS-tartománynév, a felügyelt tartomány számára. Választhatja azt is, az erőforráscsoport és az Azure-beli hely, amelyhez a felügyelt tartományra kell telepíteni.

![Alapvető beállítások konfigurálása](./media/getting-started/domain-services-blade-basics.png)

1. Válassza ki a **DNS-tartománynév** a felügyelt tartományok.

   * A címtár alapértelmezett tartományneve (az egy **. onmicrosoft.com** utótag) alapértelmezés szerint van megadva.

   * A egy egyéni tartománynevet is megadható. Ebben a példában az egyéni tartománynév a *contoso100.com*.

     > [!WARNING]
     > A megadott tartománynév előtagja (a *contoso100.com* tartománynévben például a *contoso100* tag) legfeljebb 15 karaktert tartalmazhat. Nem hozható létre egy felügyelt tartomány 15 karakternél hosszabb előtaggal kezdődik.
     >
     >

2. Ellenőrizze, hogy a felügyelt tartomány számára választott DNS-tartománynév még nem létezik a virtuális hálózatban. Pontosabban, ellenőrizze, hogy:

   * Már létezik-e tartomány ugyanezzel a DNS-tartománynévvel a virtuális hálózaton.

   * A virtuális hálózat, ahol a felügyelt tartományra engedélyezni szeretné a helyszíni hálózat VPN-kapcsolattal rendelkezik. Ebben a forgatókönyvben győződjön meg arról, nincs olyan tartomány ugyanezzel a DNS-tartománynévvel a helyi hálózaton.

   * Létezik-e egy már meglévő felhőszolgáltatás ugyanezen a néven a virtuális hálózaton.

3. Válassza ki a **a fajta virtuális hálózatot**. Alapértelmezés szerint a **erőforrás-kezelő** virtuális hálózati típus van kijelölve. Ez a fajta virtuális hálózatot az összes felügyelt tartományok az újonnan létrehozott használatát javasoljuk.

4. Válassza ki az Azure **előfizetés** a szeretné létrehozni a felügyelt tartományra.

5. Válassza ki a **erőforráscsoport** a felügyelt tartományra tartozik kell. Ön választhatja a **hozzon létre új** vagy **meglévő** lehetőséget választhat ki az erőforráscsoportot.

6. Válassza ki az Azure **hely** a a felügyelt tartományra meg kell létrehozni. Az a **hálózati** oldalon a varázsló csak virtuális hálózatok, amelyek a kijelölt helyre látja.

7. Amikor elkészült, kattintson a **OK** a áthelyezése a **hálózati** a varázsló.


## <a name="next-step"></a>Következő lépés
[2. feladat: a hálózati beállítások konfigurálása](active-directory-ds-getting-started-network.md)
