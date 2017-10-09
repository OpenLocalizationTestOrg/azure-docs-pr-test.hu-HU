---
title: "aaaAzure IoT Suite és az Azure Active Directory |} Microsoft Docs"
description: "Ismerteti, hogyan használja az Azure IoT Suite az Azure Active Directory-toomanage engedélyek."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: dobett
ms.openlocfilehash: 4768630f2de4bb431731fbd4e8929232bc80b9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-on-hello-azureiotsuitecom-site"></a>Engedélyek hello azureiotsuite.com webhelyen

## <a name="what-happens-when-you-sign-in"></a>Mi történik, amikor bejelentkezik

első bejelentkezéskor, hello [azureiotsuite.com][lnk-azureiotsuite], hello hely határozza meg hello jogosultsági szintek rendelkezik alapján hello jelenleg kijelölt Azure Active Directory (AAD) bérlőt és Azure előfizetés.

1. Először látott bérlők következő tooyour felhasználónév toopopulate hello listája, hello hely szerzi meg az Azure-ból mely AAD-bérlő tartozik. Jelenleg hello hely csak szerezhet be egy bérlői felhasználói jogkivonatokhoz egyszerre. Ezért amikor bérlők hello legördülő lista használatával hello jobb felső sarokban, hello hely rendszer toothat bérlői tooobtain hello jogkivonatokba, hogy a bérlő számára.

2. A következő hello hely szerzi meg az Azure melyik előfizetések hello társított kiválasztott bérlő. Amikor létrehoz egy új előkonfigurált megoldást hello az elérhető előfizetések láthatja.

3. Végül hello hely hello előfizetésekhez hello erőforrásokat kéri le és erőforráscsoportok előkonfigurált megoldásokat címkével rendelkeznek, és feltölti a hello csempék hello kezdőlapján.

a következő szakaszok hello hello szerepkörök, hozzáférési előre konfigurált toohello megoldásokban szabályozó ismertetik.

## <a name="aad-roles"></a>Az AAD-szerepkörök

hello AAD szerepkörök hello képes előre konfigurált rendelkezés megoldások szabályozza, és előre konfigurált megoldás a felhasználók kezeléséhez.

Tudnivalók a rendszergazdai szerepkörökről további információkat talál az aad-ben a [rendszergazdai szerepkörök hozzárendelése az Azure AD][lnk-aad-admin]. hello aktuális cikk foglalkozik a hello **globális rendszergazda** és hello **felhasználói** directory szerepkörök hello által használt, előre konfigurált megoldások.

### <a name="global-administrator"></a>Globális rendszergazda

Az AAD bérlőnként globális rendszergazdák közül sokan lehet:

* Amikor létrehoz egy AAD-bérlőt, alapértelmezett hello globális rendszergazdának, hogy a bérlő áll.
* globális rendszergazda hello előkonfigurált megoldást hozhat létre, és hozzá van rendelve egy **Admin** szerepkör hello alkalmazás az AAD-bérlőt belül.
* Ha egy másik felhasználója hello azonos AAD-bérlőt alkalmazást hoz létre, hello alapértelmezett nyújtott toohello globális rendszergazdai szerepköre **ReadOnly**.
* A globális rendszergazda felhasználók tooroles hello használó alkalmazások esetében rendelhet [Azure-portálon][lnk-portal].

### <a name="domain-user"></a>Tartományi felhasználó

AAD-bérlőt sok tartományi felhasználók lehet:

* A tartományi felhasználók építhető ki hello előkonfigurált megoldást [azureiotsuite.com] [ lnk-azureiotsuite] hely. Alapértelmezés szerint hello tartomány kap hello **Admin** hello szerepköre kiépített alkalmazás.
* Egy olyan tartományi felhasználó hozhat létre egy alkalmazást, az hello build.cmd parancsfájl a hello [azure iot-távoli-ellenőrző][lnk-rm-github-repo], [azure iot – prediktív-karbantartás] [ lnk-pm-github-repo], vagy [azure iot-csatlakoztatott-gyári] [ lnk-cf-github-repo] tárházba. Azonban hello alapértelmezett szerepkör toohello tartományi felhasználó kap-e **ReadOnly**, mert egy olyan tartományi felhasználó nem rendelkezik engedéllyel tooassign szerepkörök.
* Az AAD-bérlőt hello egy másik felhasználó alkalmazást hoz létre, ha a hello tartományi felhasználó van-e hozzárendelve a hello **ReadOnly** szerepkör az alkalmazás alapértelmezés szerint.
* Egy olyan tartományi felhasználó nem rendelhető hozzá a szerepkört az alkalmazások számára. ezért egy olyan tartományi felhasználó nem vehető fel felhasználók vagy szerepkörök a felhasználók számára az alkalmazás akkor is, ha azok kiépítése azt.

### <a name="guest-user"></a>Vendég felhasználó

Az AAD bérlőnként számos Vendég felhasználó lehet. Vendégfelhasználók rendelkeznek korlátozott jogok hello AAD-bérlőben. Ennek eredményeképpen a vendégfelhasználók hello AAD-bérlőt az előkonfigurált megoldás nem használhatók.

Felhasználók és szerepkörök az aad-ben kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [Felhasználók létrehozása az Azure ad-ben][lnk-create-edit-users]
* [Felhasználók tooapps hozzárendelése][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure-előfizetéshez rendszergazdai szerepkörök

hello Azure rendszergazdai szerepkörök hello képességét toomap az Azure-előfizetés tooan AD-bérlő szabályozzák.

További információk hello Azure rendszergazdai szerepkörök hello cikkben [hogyan tooadd, vagy módosítsa az Azure Társadminisztrátoraként, a szolgáltatás-rendszergazda és a fiók rendszergazdájához][lnk-admin-roles].

## <a name="application-roles"></a>Alkalmazás-szerepkörök

hello alkalmazási szerepköröknek hozzáférés toodevices az előkonfigurált megoldás szabályozzák.

Két megadott szerepkörök és meghatározott egy telepített alkalmazás egy implicit szerepkör vannak:

* **Felügyeleti:** rendelkezik teljes vezérlés tooadd, felügyeletéhez, távolítsa el az eszközök és beállítások módosítása.
* **Csak olvasható:** eszközök, szabályok, műveletek, feladatok és telemetriai adatok megtekintéséhez.

Hello jogosultságokkal hello tooeach szerepkör található [RolePermissions.cs] [ lnk-resource-cs] forrásfájl.

### <a name="changing-application-roles-for-a-user"></a>A felhasználó alkalmazás-szerepkörök módosítása

A következő eljárás toomake az Active Directoryban az előkonfigurált megoldás egy rendszergazda felhasználó hello is használhatja.

Egy felhasználó egy aad-ben globális rendszergazda toochange szerepköreinek kell lennie:

1. Nyissa meg toohello [Azure-portálon][lnk-portal].
2. Válassza ki **az Azure Active Directory**.
3. Ellenőrizze, hogy használ hello directory úgy döntött, hogy a azureiotsuite.com létesített a megoldás. Ha az Ön előfizetéséhez rendelve több címtárral rendelkezik, közöttük, ha hello jobb felső részén hello portálon, a fiók nevére kattintva válthat.
4. Kattintson a **vállalati alkalmazások**, majd **összes alkalmazás**.
4. Megjelenítése **összes alkalmazás** rendelkező **bármely** állapotát. Majd keresse meg az előkonfigurált megoldás nevű kérelmet.
5. Kattintson a hello alkalmazás, amely megfelel az előkonfigurált megoldás neve hello nevét.
6. Kattintson a **felhasználók és csoportok**.
7. Válassza ki a kívánt tooswitch szerepkörök hello felhasználót.
8. Kattintson a **hozzárendelése** és select hello szerepkör (például **Admin**) tooassign toohello felhasználói, milyen kattintson pipára hello.

## <a name="faq"></a>GYIK

### <a name="im-a-service-administrator-and-id-like-toochange-hello-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>A szolgáltatás-rendszergazdáknak vagyok, és szeretnék toochange hello könyvtár leképezése az előfizetésem és egy adott AAD-bérlőt között. Hogyan hajthatja végre ezt a feladatot?

1. Toohello lépjen [a klasszikus Azure portálon][lnk-classic-portal], kattintson a **beállítások** hello hello bal oldalán szolgáltatások közül.
2. Válassza ki azt szeretné, hogy toochange hello könyvtár leképezése a hello előfizetést.
3. Kattintson a **könyvtár**.
4. Jelölje be hello **Directory** hello legördülő toouse szeretné. Hello előre nyílra.
5. Erősítse meg a hello könyvtár leképezése és társrendszergazdák hatással. Ha áthelyez egy másik címtárból, a rendszer eltávolítja minden hello társrendszergazdák hello eredeti könyvtárból.

### <a name="im-a-domain-usermember-on-hello-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Egy tartományi felhasználói/tagot hello AAD-bérlőt vagyok, és létrehozott egy előre konfigurált megoldás. Hogyan tegye beolvasása kiosztott egy szerepkört az alkalmazáshoz?

Kérje meg egy globális rendszergazda toomake, egy globális rendszergazdának hello aad-ben a bérlői, majd rendelje hozzá szerepkörök toousers magát. Azt is megteheti, kérje meg egy globális rendszergazda tooassign meg közvetlenül egy szerepkört. Ha szeretné toochange hello AAD-bérlőt az előkonfigurált megoldás már alkalmazva van, tekintse meg a következő kérdés hello.

### <a name="how-do-i-switch-hello-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>A távoli felügyeleti előkonfigurált megoldás és az alkalmazás hozzárendelni hello AAD-bérlőt átváltása?

A felhő üzembe helyezése a futtatása <https://github.com/Azure/azure-iot-remote-monitoring> és telepítse újra az újonnan létrehozott AAD-bérlőt. Mivel, alapértelmezés szerint egy AAD-bérlőt létrehozásakor a globális rendszergazdai engedélyek tooadd felhasználói és szerepkörök toothose felhasználók hozzárendelése.

1. Hozzon létre egy AAD-címtárában hello [Azure-portálon][lnk-portal].
2. Nyissa meg túl<https://github.com/Azure/azure-iot-remote-monitoring>.
3. Futtatás `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (például `build.cmd cloud debug myRMSolution`)
4. Amikor a rendszer kéri, állítsa be a hello **tenantid** toobe az újonnan létrehozott bérlő az előző bérlő helyett.

### <a name="i-want-toochange-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>A szolgáltatás rendszergazdai vagy társadminisztrátori amikor egy szervezeti fiókkal jelentkezik be toochange kívánt

Lásd: hello támogatási cikk [módosítása szolgáltatás-rendszergazda és Társadminisztrátoraként, amikor egy szervezeti fiókkal jelentkezik be][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-hello-proper-permissions-toocreate-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Miért jelenik meg ezt a hibát? "A fiók nem rendelkezik megfelelő engedélyekkel toocreate hello megoldást. Ellenőrizze a fiók rendszergazdájához, vagy próbálkozzon egy másik fiókkal."

Tekintse meg a következő diagram útmutatót hello:

![][img-flowchart]

> [!NOTE]
> Ha Ön most folytatja, toosee hello hiba ellenőrzése után egy globális rendszergazdája hello AAD-bérlőt és egy közös hello előfizetés rendszergazdája, a fiók rendszergazdájához, távolítsa el a hello felhasználói és újbóli hozzárendelése a szükséges engedélyekkel az itt megadott sorrendben kell. Először globális rendszergazdaként hello felhasználó hozzáadása, és adja hozzá a felhasználó hello Azure-előfizetés társadminisztrátoraként. Ha a probléma továbbra is fennáll, forduljon a [Súgó és támogatás][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-toocreate-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Miért jelenik meg a hiba, ha az Azure-előfizetésre van? "Az Azure-előfizetés nem szükséges toocreate előkonfigurált megoldásokat. Létrehozhat egy ingyenes próbafiók néhány percig."

Ha bizonyos Azure-előfizetéssel rendelkezik, az előfizetéshez tartozó leképezési hello bérlői, és ellenőrizheti hello megfelelő bérlői hello legördülő van kiválasztva. Ha ellenőrizte, hogy hello szükséges bérlői helyességéről, kövesse az előző ábrán hello és az előfizetés és az AAD-bérlőt hello leképezés érvényesítése.

## <a name="next-steps"></a>Következő lépések
IoT Suite megtanulni toocontinue tekintse meg, hogyan zajlik [előkonfigurált megoldás testreszabása][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
