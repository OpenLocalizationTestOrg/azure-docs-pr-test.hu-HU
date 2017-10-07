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
ms.openlocfilehash: 8bde872a13bc9960d1e62c74017ff78a8953a0a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a>Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure-portálon hello használata


## <a name="task-3-configure-administrative-group"></a>3. feladat: a felügyeleti csoport konfigurálása
A konfigurációs feladat létrehoz egy felügyeleti csoport az Azure AD-címtárát. Ez a különleges felügyeleti csoport neve *AAD DC rendszergazdák*. A csoport tagjai a gépeken, amelyek a tartományhoz csatlakoztatott toohello által kezelt tartomány rendszergazdai jogosultsággal rendelkező. A tartományhoz csatlakoztatott számítógépeken ez a csoport toohello rendszergazdák csoport jelenik meg. Emellett a csoport tagjai is használ a távoli asztal tooconnect távolról toodomain csatlakoztatott számítógépeken.

> [!NOTE]
> A felügyelt tartományra hello Azure Active Directory tartományi szolgáltatások által létrehozott nincs tartományi rendszergazda vagy a vállalati rendszergazda engedélyekkel kell rendelkeznie. A felügyelt tartományok ezek az engedélyek hello szolgáltatás által fenntartott és az nem elérhető toousers hello bérlői belül. Azonban használhat hello speciális felügyeleti csoport létrehozása a konfigurációs feladat tooperform a néhány privilegizált műveleteket. Ezek a műveletek közé tartoznak a számítógépek toohello tartományhoz való csatlakozás, tartományhoz csatlakoztatott számítógépeken toohello felügyeleti csoporthoz tartozó és a csoportházirend konfigurálásához.
>

hello varázsló automatikusan létrehozza a hello felügyeleti csoport, az Azure AD-címtárát. Ez a csoport neve "AAD DC rendszergazdák". Ha van ilyen nevű meglévő csoport az Azure AD-címtár, hello varázsló kiválasztása ehhez a csoporthoz. Csoporttagság hello használatával konfigurálhat **rendszergazdai csoport** varázsló lapja.

1. tooconfigure csoporttagság, kattintson a **AAD DC rendszergazdák**.

    ![Csoporttagság konfigurálása](./media/getting-started/domain-services-blade-admingroup.png)

2. Kattintson a hello **tagok hozzáadása** gombra kattint, az Azure AD directory toohello rendszergazda-csoport tooadd felhasználóit.

3. Amikor elkészült, kattintson a **OK** a toohello toomove **összegzés** hello varázsló.

4. A hello **összegzés** hello varázsló, felülvizsgálati hello a hello által kezelt tartomány beállításait. Lépjen vissza tooany lépés hello varázsló toomake változásokat, ha szükséges. Amikor elkészült, kattintson a **OK** toocreate hello új felügyelt tartomány.

    ![Összefoglalás](./media/getting-started/domain-services-blade-summary.png)

5. Megjelenik egy értesítés, hogy hello az Azure AD tartományi szolgáltatásokhoz központi telepítés végrehajtási állapotát jeleníti meg. Kattintson a hello értesítés toosee részletes hello telepítés folyamatban van.

    ![Értesítés - telepítés folyamatban](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a>A felügyelt tartományok kiépítése
a felügyelt tartományok kiépítés hello folyamat akár tooan óra is igénybe vehet.

1. Amíg a telepítés van folyamatban, hello tartományi szolgáltatásokban keresheti **keresési erőforrások** keresőmezőbe. Válassza ki **Azure AD tartományi szolgáltatások** hello keresési eredmény alapján. Hello **Azure AD tartományi szolgáltatások** panel megjeleníti hello által felügyelt tartományokhoz, amelyek telepítése folyamatban van.

    ![Található felügyelt tartomány telepítése folyamatban](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Kattintson hello hello felügyelt tartomány (például "contoso100.com") toosee hello tartománnyal kapcsolatos további részletekért.

    ![Tartományi szolgáltatások - üzembe helyezési állapota](./media/getting-started/domain-services-provisioning-state.png)

3. Hello **áttekintése** lapon láthatók, melyek hello tartományhoz jelenleg folyamatban van. Hello által kezelt tartomány teljesen kiépítéséig nem konfigurálható. A felügyelt tartományra toobe teljesen kiépítve tooan órát igénybe vehet.

    ![Tartományi szolgáltatások - áttekintés lap során hello üzembe helyezési állapota ](./media/getting-started/domain-services-provisioning-state-details.png)

4. Amikor hello által kezelt tartomány teljesen ki van építve, hello **áttekintése** lapon láthatók hello tartomány állapotának **futtató**.

    ![Tartományi szolgáltatások – Áttekintés lap a teljes kiépítés után](./media/getting-started/domain-services-provisioned.png)

5. A hello **tulajdonságok** lapon látni tartományi tartományvezérlők elérhetők hello virtuális hálózat két IP-címet.

    ![Tartományi szolgáltatások - teljesen kiépítése után tulajdonságai lap](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="next-step"></a>Következő lépés
[4. feladat: hello hello Azure-beli virtuális hálózat DNS-beállításainak frissítése](active-directory-ds-getting-started-dns.md)
