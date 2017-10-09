---
title: "Azure-portál alkalmazás aaaCreate identitást |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate egy új Azure Active Directory-alkalmazás és egyszerű, amely használható az Azure Resource Manager toomanage hello szerepköralapú hozzáférés-vezérlés szolgáltatás hozzáférés tooresources."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 9624715ac612f42df6f9e9e67b8233bd4b914174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-portal-toocreate-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Portál toocreate használja az Azure Active Directory-alkalmazást és egy egyszerű szolgáltatást, amely erőforrások eléréséhez

Ha egy alkalmazás, amely tooaccess kell, vagy módosítsa az erőforrásokat, állítson be egy Azure Active Directory (AD) alkalmazás, és rendelje hozzá a szükséges hello engedélyek tooit. Ez a módszer előnyösebb toorunning hello alkalmazás a saját credentials azért, mert:

* Engedélyeket rendelhet a saját engedélyeit eltérő toohello identitását. Ezek az engedélyek általában korlátozott tooexactly milyen hello alkalmazásnak kell toodo.
* Ha módosítja az Ön feladatkörei nincs toochange hello alkalmazás hitelesítő adatait. 
* Használhatja a tanúsítványhitelesítés tooautomate egy felügyelet nélküli parancsfájl végrehajtása közben.

Ez a témakör bemutatja, hogyan tooperform azokat lépésről-lépésre hello portálon. A single-bérlő alkalmazás hello alkalmazás esetén csak egy szervezeten belül tervezett toorun összpontosít. Általában egy bérlői alkalmazásokat használ futó üzleti alkalmazásokhoz a szervezeten belül.
 
## <a name="required-permissions"></a>Szükséges engedélyek
toocomplete ebben a témakörben kell, hogy megfelelő engedélyek tooregister egy alkalmazást az Azure AD-bérlőről, és hello alkalmazás tooa szerepkör hozzárendelése az Azure-előfizetésben. Most Meggyőződünk arról rendelkezik megfelelő engedélyekkel tooperform hello ezeket a lépéseket.

### <a name="check-azure-active-directory-permissions"></a>Azure Active Directory-engedélyek ellenőrzése
1. Jelentkezzen be Azure-fiók tooyour keresztül hello [Azure-portálon](https://portal.azure.com).
2. Válassza ki **az Azure Active Directory**.

     ![Válassza ki az azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)
3. Válassza ki az Azure Active Directoryban, **felhasználói beállítások**.

     ![Válassza ki a felhasználói beállítások](./media/resource-group-create-service-principal-portal/select-user-settings.png)
4. Ellenőrizze a hello **App regisztrációk** beállítást. Ha túl beállítása**Igen**, nem rendszergazdai felhasználók regisztrálhatják AD alkalmazásaiban. Ez a beállítás azt jelenti, hogy minden olyan felhasználó, az Azure AD-bérlő hello regisztrálhatja az alkalmazást. A Folytatás túl[Azure ellenőrizze előfizetése engedélyei között](#check-azure-subscription-permissions).

     ![alkalmazás-regisztrációk megtekintése](./media/resource-group-create-service-principal-portal/view-app-registrations.png)
5. Ha hello app regisztrációk beállítás értéke túl**nem**, csak a rendszergazda felhasználók regisztrálhatják az alkalmazásokat. Toocheck kell-e a fiók egy rendszergazda hello Azure AD-bérlő. Válassza ki **áttekintése** és **keresse meg azt a felhasználót** a gyorsan elvégezhető.

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/find-user.png)
6. Keresse meg a fiókját, és válassza ki, ha azt.

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/show-user.png)
7. Válassza ki a fiókot, **Directory szerepkör**. 

     ![Directory szerepkör](./media/resource-group-create-service-principal-portal/select-directory-role.png)
8. A hozzárendelt directory szerepkör megtekintéséhez az Azure ad-ben. Ha a fiókjához toohello felhasználói szerepkör van hozzárendelve, de hello regisztrációs Alkalmazásbeállítás (az előző lépésekben hello) korlátozott tooadmin felhasználók, kérje a rendszergazda tooeither rendelje hozzá, akkor tooan rendszergazdai szerepkör, illetve tooenable felhasználók tooregister alkalmazások.

     ![nézet szerepkör](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a>Azure-előfizetés engedélyek ellenőrzése
Az Azure-előfizetéséhez, a fiók rendelkezik `Microsoft.Authorization/*/Write` tooassign AD-alkalmazás tooa szerepet eléréséhez. Ez a művelet biztosítja hello [tulajdonos](../active-directory/role-based-access-built-in-roles.md#owner) szerepkör vagy [felhasználói hozzáférés adminisztrátora](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) szerepkör. Ha a fiók hozzá van rendelve a toohello **közreműködő** szerepkör, Önnek nincs megfelelő engedélye. Egy hibaüzenetet fog kapni, tooassign hello szolgáltatás egyszerű tooa szerepkör megkísérlése során. 

toocheck előfizetése engedélyei között:

1. Ha nem már nézi, az Azure AD-fiókot az előző lépésekben hello, válassza ki a **Azure Active Directory** hello bal oldali ablaktáblán.

2. Keresse meg az Azure AD-fiókot. Válassza ki **áttekintése** és **keresse meg azt a felhasználót** a gyorsan elvégezhető.

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/find-user.png)
2. Keresse meg a fiókját, és válassza ki, ha azt.

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/show-user.png) 
     
3. Válassza ki **Azure-erőforrások**.

     ![Erőforrások kiválasztása](./media/resource-group-create-service-principal-portal/select-azure-resources.png) 
3. A hozzárendelt szerepkörök megtekintése, és határozza meg, ha rendelkezik megfelelő engedélyekkel tooassign AD-alkalmazás tooa szerepet. Ha nem, kérje meg az előfizetés rendszergazdája tooadd meg tooUser hozzáférési rendszergazdai szerepkört. A következő kép hello hello felhasználó, hozzárendelt toohello tulajdonosi szerepkör két elő, ami azt jelenti, hogy a felhasználó a megfelelő engedélyekkel rendelkezik. 

     ![engedélyek megjelenítése](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a>Egy Azure Active Directory-alkalmazás létrehozása
1. Jelentkezzen be Azure-fiók tooyour keresztül hello [Azure-portálon](https://portal.azure.com).
2. Válassza ki **az Azure Active Directory**.

     ![Válassza ki az azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

4. Válassza ki **App regisztrációk**.   

     ![Válassza ki az alkalmazás-regisztráció](./media/resource-group-create-service-principal-portal/select-app-registrations.png)
5. Válassza a **Hozzáadás** lehetőséget.

     ![alkalmazás hozzáadása](./media/resource-group-create-service-principal-portal/select-add-app.png)

6. Adjon meg egy nevet és egy URL-cím hello alkalmazáshoz. Válassza ki vagy **Web app / API** vagy **natív** hello alkalmazástípus toocreate keresi. Miután beállította a hello értékek, válassza ki a **létrehozása**.

     ![alkalmazás neve](./media/resource-group-create-service-principal-portal/create-app.png)

Az alkalmazás hozott létre.

## <a name="get-application-id-and-authentication-key"></a>Alkalmazás azonosítója és a hitelesítési kulcs beszerzése
Bejelentkezéskor programozott módon, az alkalmazás és egy hitelesítési kulcsot kell hello azonosítója. tooget megadott értékeket használja hello a következő lépéseket:

1. A **App regisztrációk** az Azure Active Directoryban, válassza ki az alkalmazást.

     ![alkalmazás kiválasztása](./media/resource-group-create-service-principal-portal/select-app.png)
2. Másolás hello **Alkalmazásazonosító** és az alkalmazás kódjában tárolja. hello alkalmazások hello [mintaalkalmazást](#sample-applications) szakasz tekintse meg a toothis értékével megegyező hello ügyfél-azonosító.

     ![ügyfél-azonosító](./media/resource-group-create-service-principal-portal/copy-app-id.png)
3. egy hitelesítési kulcs toogenerate válasszon **kulcsok**.

     ![Válassza ki a kulcsok](./media/resource-group-create-service-principal-portal/select-keys.png)
4. Adja meg a hello-kulccsal és egy időtartamot hello kulcs leírását. Ha elkészült, válassza ki a **mentése**.

     ![kulcs mentése](./media/resource-group-create-service-principal-portal/save-key.png)

     Hello kulcs mentése, után hello hello kulcs értéke jelenik meg. Másolja ezt az értéket, mert nem tudja tooretrieve hello kulcs később. Hello alkalmazás azonosítója toolog a hello kulcsérték rendelkezésére hello alkalmazásként. Tárolja a hello kulcs értékét, ahol az alkalmazás kérheti le.

     ![kulcs mentése](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>-Bérlőazonosító beszerzése
Bejelentkezéskor programozott módon, toopass hello Bérlőazonosító van szüksége a hitelesítési kérelmet. 

1. tooget hello Bérlőazonosító, válassza ki **tulajdonságok** az Azure AD-bérlő. 

     ![Válassza ki az Azure AD-tulajdonságok](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

2. Másolás hello **könyvtár-azonosítója**. Ez az érték az a bérlő azonosítója.

     ![bérlő azonosítója](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-toorole"></a>Rendelje hozzá az alkalmazás toorole
tooaccess erőforrást az előfizetésében, hozzá kell rendelnie hello alkalmazás tooa szerepkör. Döntse el, melyik szerepkör hello a megfelelő engedélyekkel a hello alkalmazás jelöli. toolearn hello elérhető szerepkörök, lásd: [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).

Hello hatókör hello előfizetés, az erőforráscsoportot, vagy az erőforrás hello szinten állíthatja be. Az engedélyek hatóköre örökölt toolower szintű is. Például hozzáadása egy alkalmazáshoz toohello olvasó szerepkört erőforráscsoport azt jelenti, hogy az tud olvasni hello erőforráscsoport és a benne található erőforrásokat.

1. Keresse meg a kívánt tooassign hello alkalmazás hatókör toohello szintjét. Például tooassign hello előfizetés hatókörben szerepkör kiválasztása **előfizetések**. Ehelyett kiválaszthatja egy erőforráscsoport vagy az erőforrás.

     ![Válassza ki az előfizetést](./media/resource-group-create-service-principal-portal/select-subscription.png)

2. Válassza ki a hello adott előfizetés (erőforráscsoport vagy erőforrás) tooassign hello alkalmazást.

     ![Válassza ki az előfizetést a hozzárendeléshez](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

3. Válassza ki **hozzáférés-vezérlés (IAM)**.

     ![Jelölje be a hozzáférés](./media/resource-group-create-service-principal-portal/select-access-control.png)

4. Válassza a **Hozzáadás** lehetőséget.

     ![Válassza ki hozzáadása](./media/resource-group-create-service-principal-portal/select-add.png)
6. Válassza ki a hello szerepkör tooassign toohello alkalmazás kívánja. hello következő kép bemutatja hello **olvasó** szerepkör.

     ![szerepkör kiválasztása](./media/resource-group-create-service-principal-portal/select-role.png)

8. Keresse meg az alkalmazást, és válassza ki azt.

     ![alkalmazások keresése](./media/resource-group-create-service-principal-portal/search-app.png)
9. Válassza ki **OK** toofinish hello szerepkör hozzárendelése. Megjelenik a felhasználók tooa szerepkörrel rendelkeznek az adott hatókörnél hello lista alkalmazását.

## <a name="log-in-as-hello-application"></a>Jelentkezzen be hello alkalmazás

Az alkalmazás most már be van állítva az Azure Active Directoryban. Rendelkezik egy Azonosítót és a kulcs toouse hello alkalmazásként aláíráshoz. hello alkalmazás szerepét tooa, mely bizonyos műveleteket hajthat végre műveleteket. Különböző platformokon keresztül hello alkalmazásként naplózás kapcsolatos információkért lásd:

* [PowerShell](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [Azure CLI](resource-group-authenticate-service-principal-cli.md#provide-credentials-through-azure-cli)
* [REST](/rest/api/#create-the-request)
* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Következő lépések
* egy több-bérlős alkalmazást, be tooset lásd: [– fejlesztői útmutató tooauthorization hello Azure Resource Manager API-val rendelkező](resource-manager-api-authentication.md).
* toolearn biztonsági házirendek meghatározásával kapcsolatban lásd: [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).  
* Az elérhető műveleteket, lehet megadott vagy toousers megtagadva listájáért lásd: [Azure Resource Manager erőforrás-szolgáltató műveletek](../active-directory/role-based-access-control-resource-provider-operations.md).
