---
title: "egy állandó virtuális IP-címet egy Azure felhőszolgáltatást aaaHow tooretain |} Microsoft Docs"
description: "Ismerje meg, hogyan nem változtatja meg, hogy a virtuális IP-címét (VIP) az Azure-felhőszolgáltatásban hello tooensure."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 9e27121797ffb61517b8d2c2661ec44ff7298968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retain-a-constant-virtual-ip-address-for-an-azure-cloud-service"></a>Tartsa meg az Azure-felhőszolgáltatás állandó virtuális IP-cím
Amikor frissíti egy felhőszolgáltatás, amely az Azure szolgáltatásban üzemeltetett, szükség lehet, amely nem változik a hello virtuális IP-cím (VIP) hello szolgáltatás tooensure. Sok tartományi szolgáltatások hello tartománynévrendszer (DNS) tartománynevek a regisztrációhoz használja. DNS csak hello VIP marad hello azonos működik. Használhatja a hello **közzététele varázsló** , amely a felhőszolgáltatás VIP nem változik, ha hello Azure eszközök tooensure, frissítse azt. További információ toouse DNS-tartományok felhőszolgáltatásai számára, lásd: [egy egyéni tartománynevet, az Azure-felhőszolgáltatás konfigurálása](cloud-services/cloud-services-custom-domain-name.md).

## <a name="publish-a-cloud-service-without-changing-its-vip"></a>Egy felhőalapú szolgáltatás közzététele a VIP módosítása nélkül
hello VIP felhőalapú szolgáltatások üzembe helyezésekor az adott környezetben, például hello éles környezetben tooAzure van lefoglalva. hello VIP módosítások csak akkor, ha az explicit módon törölje hello központi telepítést, vagy hello központi telepítés által implicit módon törölték a hello telepítési folyamatot nem lehet frissíteni. tooretain hello VIP, nem törölnie kell a központi telepítés, és meg kell győződnie arról, hogy a Visual Studio nem automatikusan törli a központi telepítés. 

Központi telepítési beállításokat adhat meg hello **közzététele varázsló**, amely támogatja több központi telepítési beállítások. A friss telepítés vagy frissítés központi telepítés, amely lehet növekményes vagy egyidejű adhat meg. Mindkét típusú központi telepítést hello VIP megőrzése mellett. Különböző központi telepítési típusainak-definíciók, lásd: [Azure alkalmazás közzététele varázsló](vs-azure-tools-publish-azure-application-wizard.md). Ezenkívül azt is szabályozhatja, hogy hello a korábbi központi telepítés egy felhőalapú szolgáltatás törölni, ha a hiba akkor fordul elő. Ha ez a lehetőség nincs megfelelően beállítva, hello VIP váratlanul változhatnak.

## <a name="update-a-cloud-service-without-changing-its-vip"></a>Egy felhőalapú szolgáltatás frissítése a VIP módosítása nélkül
1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása. 

2. A **Megoldáskezelőben**, kattintson a jobb gombbal hello projekt. A hello helyi menüben válassza **közzététel**.

    ![Közzététel menü](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/solution-explorer-publish-menu.png)

3. A hello **Azure-alkalmazás közzététele** párbeszédpanel megnyitásához, jelölje be hello Azure-előfizetés toowhich toodeploy szeretné. Jelentkezzen be, ha szükséges, és jelölje be **következő**.

    ![Azure alkalmazás bejelentkezési oldal közzététele](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-signin.png)

4. A hello **általános beállítások** lapra, győződjön meg arról, hogy hello neve hello cloud service toowhich telepít, hello **környezet**, hello **Build konfigurációját**, és hello **Szolgáltatáskonfiguráció** helyességét.

    ![Azure-alkalmazás általános beállítások lapon közzététele](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-common-settings.png)

5. A hello **speciális beállítások** lapra, győződjön meg arról, hogy hello **üzemelő példány címkéje** és hello **tárfiók** helyes-e. Győződjön meg arról, hogy hello **törölje a központi telepítési hiba esetén** jelölőnégyzet nincs bejelölve, és győződjön meg arról, hogy hello **üzemelő példány frissítése** jelölőnégyzet be van jelölve. Hello törlésével által **törölje a központi telepítési hiba esetén** jelölőnégyzetet, akkor győződjön meg arról, hogy a VIP akkor nem vesznek el, ha hiba történik a telepítés során. Hello kiválasztásával **üzemelő példány frissítése** jelölőnégyzetet, akkor győződjön meg arról, hogy a telepítés nem törlődik, és a VIP nem vesznek el, amikor újra közzé az alkalmazást. 

    ![Közzététele az Azure alkalmazás Speciális beállítások lap](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-advanced-settings.png)

6. toofurther határozza meg, hogyan hello szerepkörök toobe frissíti, jelölje be **beállítások** következő túl**üzemelő példány frissítése**. Válassza ki vagy **növekményes frissítés** vagy **egyidejű frissítése**, és válassza ki **OK**. Válasszon **növekményes frissítés** tooupdate az alkalmazás minden példánya egymás után, így az alkalmazás hello, mindig rendelkezésre áll. Válasszon **egyidejű frissítése** tooupdate hello: az alkalmazás összes példányát egyszerre. Egyidejű frissítése gyorsabb, de a szolgáltatás nem áll rendelkezésre hello frissítési folyamat alatt. Ha elkészült, válassza ki a **következő**.

    ![Azure-alkalmazás központi telepítési beállítások lapon közzététele](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-deployment-update-settings.png)

7. A hello **Azure-alkalmazás közzététele** párbeszédpanelen jelölje ki **következő** amíg hello **összegzés** lap is megjelenik. Ellenőrizze a beállításokat, majd válassza ki **közzététel**.
   
    ![Azure alkalmazáshoz összegzése lap közzététele](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-summary.png)

## <a name="next-steps"></a>Következő lépések
- [Hello Visual Studio Azure alkalmazás közzététele varázsló használatával](vs-azure-tools-publish-azure-application-wizard.md)

