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
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 79cbb21c4a50194f5ad8ca1a4a8493ee4a260a9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a>Engedélyezze az Azure Active Directory tartományi szolgáltatások (előzetes verzió) Azure-portálon hello használata
Ez a cikk bemutatja, hogyan tooenable Azure Active Directory tartományi szolgáltatások (az Azure AD DS) használatával hello Azure-portálon.


toolaunch hello **engedélyezése az Azure AD tartományi szolgáltatások** varázsló, teljes hello a következő lépéseket:

1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com).
2. Hello bal oldali ablaktáblában kattintson a **új**.
3. A hello **új** panelen, írja be **tartományi szolgáltatások** hello keresősáv be.

    ![Keresse meg a tartományi szolgáltatások](./media/getting-started/search-domain-services.png)

4. Kattintson a tooselect **Azure AD tartományi szolgáltatások** hello keresési javaslatok listája. A hello **Azure AD tartományi szolgáltatások** panelen kattintson hello **létrehozása** gombra.

    ![Tartományi szolgáltatások panel](./media/getting-started/domain-services-blade.png)

5. Hello **engedélyezése az Azure AD tartományi szolgáltatások** varázsló elindul.


## <a name="task-1-configure-basic-settings"></a>1. feladat: konfigurálja az alapbeállításokat
A hello **alapjai** lap hello varázsló hello hello által kezelt tartomány DNS-tartománynevet adhat meg. Hello erőforráscsoportot is beállíthatja, és az Azure-beli hely toowhich hello által felügyelt tartományokhoz kell telepíteni.

![Alapvető beállítások konfigurálása](./media/getting-started/domain-services-blade-basics.png)

1. Válassza ki a hello **DNS-tartománynév** a felügyelt tartományok.

   * hello hello címtár alapértelmezett tartományneve (az egy **. onmicrosoft.com** utótag) alapértelmezés szerint van megadva.

   * A egy egyéni tartománynevet is megadható. Ebben a példában hello egyéni tartománynév megadása *contoso100.com*.

     > [!WARNING]
     > a megadott tartománynév előtagja hello (például *contoso100* a hello *contoso100.com* tartománynevet) kell tartalmaznia a 15 karaktert. Nem hozható létre egy felügyelt tartomány 15 karakternél hosszabb előtaggal kezdődik.
     >
     >

2. Győződjön meg arról, hogy hello DNS-tartománynevet választott hello felügyelt tartomány már nem létezik hello virtuális hálózatban. Pontosabban, ellenőrizze, hogy:

   * Már létezik hello tartomány hello virtuális hálózaton azonos DNS-tartománynevet.

   * hello tooenable hello által kezelt tartomány tervezett virtuális hálózat a helyszíni hálózat VPN-kapcsolattal rendelkezik. Ebben a forgatókönyvben a ellenőrizze, hogy nincs olyan tartomány hello azonos DNS-tartománynevet a helyi hálózaton.

   * Hello virtuális hálózaton van ilyen nevű létező felhőalapú szolgáltatást.

3. Válassza ki a hello **a fajta virtuális hálózatot**. Alapértelmezés szerint hello **erőforrás-kezelő** virtuális hálózati típus van kijelölve. Ez a fajta virtuális hálózatot az összes felügyelt tartományok az újonnan létrehozott használatát javasoljuk.

4. Jelölje be hello Azure **előfizetés** , amelyben szeretné toocreate hello által felügyelt tartományokhoz.

5. Jelölje be hello **erőforráscsoport** toowhich hello felügyelt tartományhoz kell tartoznia. Dönthet úgy, vagy hello **hozzon létre új** vagy **meglévő** beállítások tooselect hello erőforráscsoportot.

6. Válassza ki a hello Azure **hely** mely hello által felügyelt tartományokhoz kell létrehozni. A hello **hálózati** lap hello varázsló csak virtuális hálózatok tartozó kijelölt toohello helyét látja.

7. Amikor elkészült, kattintson a **OK** a toohello toomove **hálózati** hello varázsló.


## <a name="next-step"></a>Következő lépés
[2. feladat: a hálózati beállítások konfigurálása](active-directory-ds-getting-started-network.md)
