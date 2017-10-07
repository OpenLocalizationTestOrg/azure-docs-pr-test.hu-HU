---
title: a tooAzure a hello CLI aaaLog |} Microsoft Docs
description: "Azure-előfizetés tooyour csatlakoztatja a hello Azure parancssori felület (CLI) Mac, Linux és Windows rendszerekhez"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 42682c00c8dea78b2c624e640379716d1d4d7a2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-in-tooazure-from-hello-azure-cli"></a>Jelentkezzen be az Azure CLI hello tooAzure
hello Azure parancssori felület nyílt forráskódú, platformok közötti parancsokat az Azure-erőforrások használata. Ez a cikk ismerteti hello különböző módokon tooprovide az Azure-fiók hitelesítő adatait tooconnect hello Azure CLI tooyour Azure-előfizetés:

* Futtassa a hello `azure login` CLI parancs tooauthenticate Azure Active Directoryn keresztül. A metódus hozzáférést tud biztosítani, hogy mindkét tooCLI parancsok [módok parancs](#cli-command-modes). További beállítások nélkül hello parancs futtatásakor `azure login` webes portálon keresztül interaktív bejelentkezés toocontinue kéri. A további `azure login` beállítások parancsot, tekintse meg a cikket, vagy a típus hello forgatókönyvek `azure login --help`.
* Ha csak toouse Azure szolgáltatásfelügyelet módban parancssori felület parancsait (nem ajánlott az új telepítések esetén), töltse le, és telepítse a közzétételi beállítások fájlja a számítógépen.

Ha még nem telepítette a hello CLI, lásd: [telepítés hello Azure CLI](cli-install-nodejs.md). Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/free/).

Másik fiók identitások és az Azure-előfizetések kapcsolatos háttér, lásd: [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="scenario-1-azure-login-with-interactive-login"></a>1. forgatókönyv: azure bejelentkezési interaktív bejelentkezéskor
Az egyes fiókok hello CLI használatához toorun `azure login` és folytassa a bejelentkezési folyamat hello webböngészőn keresztül egy webes portál nevezett folyamat *interaktív bejelentkezési*. A leggyakoribb oka, hogy munkahelyi vagy iskolai fiókkal (más néven egy *szervezeti fiók*), amely toorequire többtényezős hitelesítés be van állítva. Is használhat, interaktív bejelentkezés Microsoft-fiókjával, ha azt szeretné, hogy toouse erőforrás-kezelő módban parancsok.

Interaktív bejelentkezési adatai könnyen: típus `azure login` --nélkül bármely beállításai – ahogy az alábbi példa hello:

```
azure login
```                                                                                             

hello eredmény az alábbihoz hasonló hello jelenik meg:

```         
info:    Executing command login
info:    toosign in, use a web browser tooopen hello page http://aka.ms/devicelogin. Enter hello code XXXXXXXXX tooauthenticate.
```
Másolja a hello parancs kimenetében tooyou kínált hello kódot, és nyissa meg a böngésző toohttp://aka.ms/devicelogin vagy más lap, ha meg van adva. (Megnyithat egy böngészőt a hello ugyanarra a számítógépre, vagy egy másik számítógépen vagy eszközön.) Hello kód megadása, és ezután felszólító tooenter hello felhasználónév és jelszó hello identitás toouse keresi. A folyamat befejezése után a hello parancs-rendszerhéj hello bejelentkezési befejezése. Ez lehet hasonlót:

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> Az interaktív bejelentkezés hitelesítés és engedélyezés történik az Azure Active Directoryval. Ha egy Microsoft-fiók identitást használja, a hello bejelentkezési folyamat az Azure Active Directory alapértelmezett tartomány éri el. (Ha volt egy ingyenes Azure-fiókot, Azure Active Directory automatikusan létrejön egy alapértelmezett tartományt a fiókhoz.)
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a>2. forgatókönyv: azure jelentkezzen be egy felhasználónevet és jelszót
Használjon hello `azure login` hello felhasználónév parancsot (`-u`) paraméter tooauthenticate, ha azt szeretné, hogy toouse munkahelyi vagy iskolai fiókkal, amely nem igényel a többtényezős hitelesítést. Parancssorból hello hello jelszót kéri (vagy opcionálisan átadhatók hello jelszó hello további paraméterként `azure login` parancs). hello alábbi példa továbbítja hello szervezeti fiók felhasználóneve:

    azure login -u myUserName@contoso.onmicrosoft.com

Rendszer már tooenter kéri a jelszót:

    info:    Executing command login
    Password: *********

hello bejelentkezési folyamat majd befejeződik.

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

Ha ezeket a hitelesítő adatokat az első alkalommal bejelentkezik, tooverify megkérdezi, hogy kívánja toocache egy hitelesítési jogkivonatot. Ez a kérdés is akkor fordul elő, ha korábban már használt hello `azure logout` (hello cikk későbbi részében leírt) parancsot. toobypass automation-forgatókönyvek esetén a parancssor futtatása `azure login` a hello `-q` paraméter.

## <a name="scenario-3-azure-login-with-a-service-principal"></a>3. forgatókönyv: azure jelentkezzen be egy egyszerű szolgáltatásnév
Ha az Active Directory-alkalmazás szolgáltatásnevet létrehozni, és hello szolgáltatás egyszerű megfelelő jogosultságokkal rendelkezik az előfizetés, használhatja a hello `azure login` parancs tooauthenticate hello szolgáltatás egyszerű. A forgatókönyvtől függően biztosíthatja hello hitelesítő adatait egyszerű hello szolgáltatást a hello explicit paraméterekként `azure login` parancsot. Például hello parancsban továbbítja hello egyszerű szolgáltatásnév és az Active Directory-bérlőazonosító beszerzése:

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

Ön nem, akkor a kért tooprovide hello jelszót. A parancssori felület parancsprogram vagy alkalmazás kód hello hitelesítő, vagy nem interaktív használni egy tanúsítvány tooauthenticate hello szolgáltatás egyszerű automatizálási esetekben is. Részletek és példák: [hitelesítéséhez az Azure Resource Manager szolgáltatásnevet](resource-group-authenticate-service-principal-cli.md).

## <a name="scenario-4-use-a-publish-settings-file"></a>4. forgatókönyv: Használja a közzétételi beállítások fájlja
Ha csak toouse hello Azure szolgáltatásfelügyelet módban parancssori felület parancsait (például toodeploy Azure virtuális gépek hello klasszikus üzembe helyezési modellel), akkor csatlakozhatnak a közzétételi beállítások fájlja. Ez a módszer tanúsítványt telepít a helyi számítógépen, amely lehetővé teszi tooperform kapcsolatos felügyeleti feladatok mindaddig, amíg hello előfizetés és a hello tanúsítvány érvényesek.

* **toodownload hello közzététele beállításfájl** a fiókot, győződjön meg arról, hogy CLI szolgáltatásfelügyelet módban van, írja be a hello `azure config mode asm`. Ezután futtassa a következő parancs hello:

        azure account download

Ez megnyitja az alapértelmezett böngészőt, és kéri a toohello toosign [a klasszikus Azure portálon](https://manage.windowsazure.com). Miután bejelentkezik, egy `.publishsettings` fájl letöltését. Jegyezze fel a letöltött fájl elérési útját.

> [!NOTE]
> Ha a fiók több Azure Active Directory-bérlő tartozik, esetleg felszólító tooselect, amely Active Directory, a közzétételi beállítások kívánja toodownload fájlt.
>
>

A kijelölt hello letöltési oldal használatával, vagy a klasszikus Azure portálon hello felkeresésével hello kijelölt Active Directory lesz hello alapértelmezett hello klasszikus portál és a letöltési oldal használják. Ha alapértelmezett lett létrehozva, akkor tekintse meg a hello szöveg "**ide tooreturn toohello kiválasztása lapon**" hello letöltési oldala hello tetején. A megadott hivatkozás tooreturn toohello kiválasztása lapon hello használja.

* **tooimport hello közzététele beállításfájl**- ben futtassa hello következő parancsot:

        azure account import <path tooyour .publishsettings file>

> [!IMPORTANT]
> Importálás után a közzétételi beállítások, törölnie kell a hello `.publishsettings` fájlt. Hello Azure CLI által már nem szükséges, és biztonsági kockázatot jelent, akkor lehet, hogy használt toogain hozzáférés tooyour előfizetés.
>
>

## <a name="cli-command-modes"></a>Parancssori felület üzemmódok
hello Azure CLI használata az Azure-erőforrások, különböző parancs készletekkel két üzemmódok ismerteti:

* **Resource Manager módra** – az Azure-erőforrások hello Resource Manager üzembe helyezési modellel működik. tooset ebben a módban futtassa `azure config mode arm`.
* **Szolgáltatásfelügyeleti módban** – az Azure-erőforrások hello klasszikus üzembe helyezési modellel működik. tooset ebben a módban futtassa `azure config mode asm`.

Első telepítésekor hello jelenlegi kiadásában hello CLI erőforrás-kezelő módban van.

> [!NOTE]
> hello erőforrás-kezelő és a szolgáltatás felügyeleti üzemmód, egymást kölcsönösen kizáró. Ez azt jelenti, hogy egy módban létrehozott erőforrások nem felügyelhetők a hello más módot.
>
>

## <a name="multiple-subscriptions"></a>Több előfizetés
Ha több Azure-előfizetéssel rendelkezik, csatlakozás tooAzure engedélyezi a hozzáférést a hitelesítő adatok társított tooall előfizetések. Egy előfizetés hello alapértelmezett beállítást választotta, és hello Azure CLI által használt műveletek végrehajtása során. Megtekintheti a hello előfizetések, beleértve a hello alapértelmezett előfizetésben, hello segítségével `azure account list` parancsot. Ez a parancs visszaadja az adatokat hasonló toohello következő:

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

A listát megelőző hello, hello **aktuális** oszlopban látható, hogy az alapértelmezett előfizetésben hello Azure-al-1. toochange hello alapértelmezett előfizetés, használjon hello `azure account set` parancsot, és adja meg, hogy kívánja-e toobe hello alapértelmezett hello előfizetés. Példa:

    azure account set Azure-sub-2

Hello alapértelmezett előfizetés tooAzure-al-2 értékre változik.

> [!NOTE]
> Hello alapértelmezett előfizetés módosítása azonnal érvénybe lép, és globális módosítva; új Azure parancssori felület parancsait, hogy futtatja azokat hello azonos parancssori vagy egy másik példány, hello új alapértelmezett előfizetés használatára.
>
>

Ha kívánja toouse egy nem alapértelmezett előfizetés hello Azure CLI-t, de nem szeretné toochange hello aktuális alapértelmezett, használhatja a hello `--subscription` hello parancs lehetőséget, majd adjon meg hello hello előfizetés kívánja toouse hello a művelethez.

Ha csatlakoztatott tooyour Azure-előfizetéssel, megkezdheti a hello Azure CLI parancsok toowork használata Azure-erőforrások.

## <a name="storage-of-cli-settings"></a>Parancssori beállítások tárolására
E bejelentkezés hello `azure login` parancs vagy importálása a közzétételi beállítások, a CLI-profil és a naplók tárolja egy `.azure` könyvtárban található a `user` könyvtár. A `user` könyvtár az operációs rendszer által védett. Javasoljuk azonban, hogy a további lépéseket tooencrypt el a `user` könyvtár. Ehhez a következő módokon hello:

* A Windows hello directory tulajdonságainak módosítása vagy a BitLocker használja.
* A Mac kapcsolja be a FileVault hello könyvtár.
* Ubuntu, a szolgáltatással hello titkosított otthoni könyvtár. Más Linux terjesztésekről hasonló funkciókat kínál.

## <a name="logging-out"></a>Kijelentkezés
toolog ki, a következő parancs használata hello:

    azure logout -u <username>

Ha hello előfizetések társított hello fiók csak hitelesítése az Active Directory kijelentkezés helyi hello-profil törlése hello előfizetés információit. Azonban a közzétételi beállítások fájlja is importált hello elő, ha kijelentkezés csak törli az Active Directory kapcsolatos információkat hello helyi profilt.

## <a name="next-steps"></a>Következő lépések
* Azure parancssori felület parancsait toouse, lásd: [Azure parancssori felület parancsait erőforrás-kezelő módban](virtual-machines/azure-cli-arm-commands.md) és [Azure parancssori felület parancsait szolgáltatásfelügyelet módban](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* toolearn hello Azure parancssori Felülettel kapcsolatos további információkért töltse le a forráskódot, jelentse az esetleges problémákat, vagy toohello projekt közre, látogasson el a hello [hello Azure CLI GitHub-tárházban](https://github.com/azure/azure-xplat-cli).
* Ha az Azure CLI hello vagy Azure problémákat tapasztal, keresse fel a hello [Azure fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).
