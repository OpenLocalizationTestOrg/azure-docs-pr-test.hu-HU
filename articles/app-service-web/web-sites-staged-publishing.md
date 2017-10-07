---
title: "átmeneti környezet az Azure App Service web Apps mentése aaaSet |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse előkészített közzététele az Azure App Service web Apps."
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: 338424100a20bf823323313fb6699e439f367421
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Átmeneti környezet az Azure App Service beállítása
<a name="Overview"></a>

Ha központilag telepíti a webalkalmazás, webes alkalmazás a Linux, mobil háttér és API-alkalmazás túl[App Service](http://go.microsoft.com/fwlink/?LinkId=529714), központilag telepíthető tooa külön üzembe helyezési pont helyett hello alapértelmezett éles tárolóhelyre hello futtatásakor **Standard**vagy **prémium** App Service-csomag mód. Üzembe helyezési saját állomásnevek ténylegesen élő alkalmazások. Alkalmazás tartalmát és konfigurációk elemek lecserélhető között két üzembe helyezési, beleértve a hello éles tárolóhelyre. Az alkalmazás tooa üzembe helyezési pont telepítése a következő előnyöket hello rendelkezik:

* Egy átmeneti üzembe helyezési tárhelyet az alkalmazások változásairól előtt hello éles tárhelyének csere azt is ellenőrzi.
* Először telepítése egy tooa tárolóhelye és csere az éles környezetben biztosítja, hogy a hello tárolóhely összes példányát, mielőtt éles környezetben felcserélés folyamatban vannak tárolóhelyspecifikus. Ez megszünteti állásidő, az alkalmazás központi telepítésekor. hello forgalom átirányítása zökkenőmentes-kérelmek nem dobja swap műveletek miatt. A teljes munkafolyamat konfigurálásával automatizálható [automatikus felcserélés](#Auto-Swap) , ha nincs szükség a előtti swap érvényesítése.
* A lapozófájl-kapacitás után hello tárolóhely korábban előkészített alkalmazással most már hello előző éles alkalmazások. Ha az éles tárolóhelyre hello felcserélve hello módosítások nem a várt módon, azonos felcserélése azonnal tooget az "utolsó ismert helyes webhely" biztonsági hello végezheti el.

Minden App Service-csomag mód támogatja az üzembe helyezési pontok különböző számú. toofind hello helyek száma, az alkalmazás mód is támogat, lásd: [App Service szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/app-service/).

* Ha az alkalmazás még több üzembe helyezési ponti, hello mód nem módosítható.
* Skálázás nem érhető el a nem végleges pont felcserélése.
* Csatolt erőforrás-kezelés nem végleges pont felcserélése nem támogatott. A hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) csak, elkerülheti a célgyűjtemény az éles tárhely hello nem éles tárolóhelyre tooa másik App Service csomag mód ideiglenesen áthelyezésével. Vegye figyelembe, hogy hello nem éles tárolóhelyre ismét közös hello hello éles tárolóhelyre előtt hello két üzembe helyezési ponti kicserélheti azonos módban.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Adja hozzá egy üzembe helyezési tárhelyet
hello app futnia kell a hello **szabványos** vagy **prémium** meg tooenable több üzembe helyezési mód sorrendjének.

1. A hello [Azure Portal](https://portal.azure.com/), nyissa meg az alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Válassza ki a hello **üzembe helyezési** lehetőséget, majd kattintson az **tárhely felvétele**.
   
    ![Adja hozzá egy új üzembe helyezési tárhelyet][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Ha hello alkalmazás még nincs hello **szabványos** vagy **prémium** mód, kapni fog egy üzenetet támogatott hello módok előkészített közzététel engedélyezésére. Ezen a ponton rendelkezik hello beállítás tooselect **frissítése** , és keresse meg a toohello **méretezési** fülre az alkalmazás a folytatás előtt.
   > 
   > 
3. A hello **tárhely hozzáadása** panelen adjon hello tárolóhelye egy olyan nevet, és válassza ki e tooclone alkalmazás-konfiguráció egy másik meglévő üzembe helyezési pont. Kattintson a pipa jelre toocontinue hello.
   
    ![Konfigurációs forrás][ConfigurationSource1]
   
    hello hozzáadásakor tárhely, csak hogy két választási lehetőség: Klónozott konfigurációs hello alapértelmezett tárolóhelyről éles környezetben, vagy egyáltalán nem.
    A létrehozást követően több üzembe helyezési ponti, fogja képes tooclone konfigurációs eltérő hello éles környezetben a tárolóhelyről:
   
    ![Konfigurációs forrás][MultipleConfigurationSources]
4. Az alkalmazás erőforrás paneljén kattintson **üzembe helyezési**, majd kattintson a központi telepítés tárolóhely tooopen, hogy a tárhely erőforrás panelről, metrikákat és csakúgy, mint bármely más alkalmazás-konfigurációs. hello hello tárolóhely neve látható hello panel tooremind hello tetején, hogy a rendszer azért jelenítette hello üzembe helyezési pont.
   
    ![Központi telepítés tárolóhely cím][StagingTitle]
5. Kattintson a hello a tárhely panelen hello alkalmazás URL-CÍMÉT. Figyelje meg hello üzembe helyezési pont saját állomásnév és is egy élő alkalmazást. toolimit nyilvános hozzáférés toohello üzembe helyezési pont, lásd: [App Service Web App – blokk web access toonon éles üzembe helyezési](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Nincs tartalom központi telepítési tárolóhely létrehozása után. Egy másik tárház ágat, vagy egy teljesen más tárház toohello tárolóhely telepítése. Hello helyezési pont konfigurációját módosíthatja is. Használjon hello hello üzembe helyezési pont tartalomfrissítéseket társított profil vagy a központi telepítési hitelesítő adatok közzététele.  Például végezheti [toothis tárolóhely a git közzététele](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a>Az üzembe helyezési konfiguráció
A klón a másik üzembe helyezési pont konfigurációja hello klónozott konfigurációs esetén szerkeszthető. Továbbá néhány konfigurációs elemek követi hello tartalom a lapozófájl-kapacitás (nem tárolóhely adott) keresztül közben hello azonos tárolóhely a lapozófájl-kapacitás (tárolóhely adott) után más konfigurációs elemek marad. hello következő listákban megjelenítése változnak, ha Ön felcserélése hello konfigurációt.

**Beállítások, amelyek van cserélve**:

* Általános beállítások - keretrendszer verziója, 32 vagy 64 bites, például webes szoftvercsatornák
* Alkalmazásbeállítások (konfigurált toostick tooa tárolóhely is lehet)
* Kapcsolati karakterláncok (konfigurált toostick tooa tárolóhely is lehet)
* Kezelőleképezések
* Megfigyelési és diagnosztikai beállítások
* Webjobs-feladatok tartalom

**Beállítások, amelyek nem van cserélve**:

* Közzétételi végpontok
* Egyéni tartománynevek
* SSL-tanúsítványok és kötések
* Skálázási beállításokat
* Webjobs-feladatok bejegyzéstípusait

egy beállítás vagy a kapcsolati karakterlánc toostick tooa tárolóhelye (nem cserélhető fel), hozzáférési hello tooconfigure **Alkalmazásbeállítások** panel egy adott tárhely, majd jelölje be hello **tárolóhely beállítás** hello be konfigurációs elemek, amelyek kell odatapadjon hello tárolóhely. Vegye figyelembe, hogy jelölés egy konfigurációs elem, ha adott tárolóhely létrehozásáról szóló az elem nem cserélhető között hello alkalmazáshoz kapcsolódó összes hello üzembe helyezési hello hatását.

![Tárolóhely-beállítások][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Központi telepítés felcserélése 
Kicserélheti az üzembe helyezési a hello **áttekintése** vagy **üzembe helyezési** a app erőforráspanelen ábrázolása.

> [!IMPORTANT]
> Mielőtt egy üzembe helyezési pont alkalmazás felcserélni a éles környezetben, győződjön meg arról, hogy az összes nem tárolóhely adott beállításai toohave kívánt hello swap cél azt.
> 
> 

1. üzembe helyezési tooswap, kattintson a hello **felcserélése** hello parancssáv hello alkalmazás vagy a hello parancssávon, egy üzembe helyezési pont gombra.
   
    ![Lapozófájl-kapacitás gomb][SwapButtonBar]

2. Győződjön meg arról, hogy hello swap forrás- és a lapozófájl-kapacitás a célkiszolgáló megfelelően vannak-e beállítva. Általában hello felcserélés célja hello éles tárolóhelyre. Kattintson a **OK** toocomplete hello műveletet. Hello művelet befejezése után hello üzembe helyezési rendelkezik lett cserélve.

    ![Felcserélés befejezése](./media/web-sites-staged-publishing/SwapImmediately.png)

    A hello **felcserélés előnézettel** típus felcserélése, lásd: [felcserélés előnézettel (több fázisban swap)](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>A felcserélés előnézettel (több fázisban swap)

A felcserélés előnézettel, vagy több fázisban swap leegyszerűsíti a tárolóhely konfigurációs elemeket, például kapcsolati karakterláncok érvényesítése.
A kritikus fontosságú munkaterhelésekhez, azt szeretné, hogy az alkalmazás hello toovalidate hello éles tárolóhelyre konfiguráció alkalmazása várt viselkedik, és végezze el az ilyen érvényesítési *előtt* hello alkalmazást éles környezetben van cserélve. Felcserélés előnézettel az alábbiakra lesz szüksége.

> [!NOTE]
> A felcserélés előnézettel a webalkalmazásokban Linux rendszeren nem támogatott.

Hello használata esetén **a felcserélés előnézettel** beállítás (lásd: [telepítési felcserélése](#Swap)), az App Service hello a következő:

- A memóriában tárolja hello cél tárolóhely változatlan ezért meglévő terhelése, hogy a bővítőhely (pl. éles) nem változik.
- Hello konfigurációs elemeinek hello tárolóhely toohello forrás céltárolóhelyen, beleértve a hello tárolóhely-specifikus kapcsolati karakterláncok és Alkalmazásbeállítások vonatkozik.
- Újraindítja a munkavégző folyamatok hello hello forrás tárolási helyre, a fent említett konfigurációs elemeket.
- Hello swap befejezésekor: a kurzor hello warmed létrehozása előtti forrás tárolási helyre történő hello céltárolóhelyen. hello céltárolóhelyen hello forrás tárolási helyre, mint egy manuális felcserélés áthelyezése történik.
- Ha megszakítja a hello lapozófájl-kapacitás: hello konfigurációs elemek hello forrás tárolóhely toohello forrás tárolási helyre, újra alkalmazza.

Megtekintheti, pontosan hogyan hello app működik hello cél a tárhely konfigurációjával kapcsolatban. Ellenőrzés befejezése után el kell végeznie egy külön lépésben hello lapozófájl-kapacitás. Ez a lépés rendelkezik hello előnye, hogy hello forrás tárolási helyre már tárolóhelyspecifikus hello kívánt konfigurációval, és az ügyfelek nem tapasztalnak állásidőt fog tapasztalni.  

Hello Azure PowerShell-parancsmagok érhető el több fázisban swap minták hello Azure PowerShell-parancsmagok a központi telepítési üzembe helyezési ponti szakaszban szerepelnek.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Az automatikus felcserélés konfigurálása
Az alkalmazás a nulla hidegindítás és állásidő automatikus felcserélés leegyszerűsíti DevOps forgatókönyvek toocontinuously, ahová telepíteni a végfelhasználók hello alkalmazás. Konfigurálásakor egy üzembe helyezési pont az automatikus felcserélés éles környezetben, minden alkalommal, amikor a kód frissítés toothat aljzat leküldéses App Service lesz automatikusan felcserélése hello alkalmazást éles környezetben után azt rendelkezik már tárolóhelyspecifikus hello tárolóhelye.

> [!IMPORTANT]
> Ha engedélyezi az automatikus felcserélés a tárhely, győződjön meg arról hello bővítőhely pontosan hello konfigurációját hello cél tárolóhely (általában hello éles tárolóhelyre) számára készült.
> 
> 

> [!NOTE]
> Az automatikus felcserélés a webalkalmazásokban Linux rendszeren nem támogatott.

Az automatikus felcserélés konfigurálása tárhely is könnyen. Kövesse az alábbi hello lépéseket:

1. A **üzembe helyezési**, válassza ki, nem éles tárhely és kiválasztható **Alkalmazásbeállítások** , hogy a tárhely erőforrás panelén.  
   
    ![][Autoswap1]
2. Válassza ki **a** a **automatikus felcserélés**, jelölje be hello kívánt cél tárolóhely a **automatikus felcserélés tárolóhely**, és kattintson a **mentése** hello parancs sávon. Ellenőrizze, hogy hello bővítőhely pontosan hello cél tárolóhely szánt hello konfigurációját.
   
    Hello **értesítések** lapon villogjon, egy zöld **sikeres** hello művelet végrehajtása után.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > tootest automatikus felcserélés az alkalmazáshoz, először kiválaszthatja a cél nem éles tárhely **automatikus felcserélés tárolóhely** toobecome ismeri a hello szolgáltatást.  
   > 
   > 
3. A kód leküldéses toothat üzembe helyezési pont hajtható végre. Az automatikus felcserélés rövid idő múlva történjen, és hello frissítés jelenik meg a cél a tárhely URL-címen.

<a name="Rollback"></a>

## <a name="toorollback-a-production-app-after-swap"></a>lapozófájl-kapacitás után éles alkalmazások toorollback
Ki a hibákat a tárolóhelycsere élesben azonosít, ha roll hello üzembe helyezési ponti hátsó tootheir előtti swap állapotok hello azonos két üzembe helyezési ponti azonnal csere által.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Egyéni bemelegítési swap előtt
Néhány alkalmazás egyéni bemelegítési műveletek lehet szükség. Hello `applicationInitialization` konfigurációs elem a Web.config fájlban lehetővé teszi, hogy toospecify egyéni inicializálási műveletek toobe hajtani egy kérelem érkezik. hello cserélő művelet várakozik a egyéni bemelegítési toocomplete. Íme egy minta web.config töredéket.

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="toodelete-a-deployment-slot"></a>toodelete egy üzembe helyezési tárhelyet
Egy üzembe helyezési tárhelyet, nyitott hello telepített környezet tárolóhelye panelen hello paneljén kattintson **áttekintése** (hello alapértelmezett oldal), és kattintson a **törlése** hello parancs sávon.  

![Egy üzembe helyezési pont törlése][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a>Az üzembe helyezési tárhely az Azure PowerShell-parancsmagjai
Az Azure PowerShell egy modul, amely a parancsmagok toomanage Azure támogatják az Azure App Service üzembe helyezési kezelése Windows PowerShell használatával.

* A telepítése és konfigurálása az Azure PowerShell és az Azure PowerShell hitelesítéséhez az Azure előfizetéssel rendelkező információkért lásd: [hogyan tooinstall, és konfigurálja a Microsoft Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Webalkalmazás létrehozása
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Egy üzembe helyezési tárhely létrehozása
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-toosource-slot"></a>Indítson el egy felcserélés felülvizsgálati (több fázisban swap) és tárhely – konfiguráció toosource céltárolóhelyen alkalmazása
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Megszakítja a függőben lévő felcserélés (felülvizsgálati felcserélés) és a forrás helyezési pont konfigurációjának visszaállítása
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Központi telepítés felcserélése
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a>Üzembe helyezési pont törlése
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a>Az üzembe helyezési tárhely az Azure parancssori felület (CLI) parancsok
hello Azure CLI-platformok parancsokat biztosít működik-e az Azure-ral, hiszen támogatják az App Service üzembe helyezési pontok kezelése.

* Az utasításokat a telepítése és konfigurálása az Azure parancssori felület hello, beleértve a vonatkozó tooconnect Azure CLI tooyour Azure-előfizetéssel, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md).
* hello Azure CLI-t, az Azure App Service szolgáltatásban elérhető toolist hello parancsokat hívás `azure site -h`.

> [!NOTE] 
> A [Azure CLI 2.0](https://github.com/Azure/azure-cli) parancsok a üzembe helyezési, lásd: [az App Service web üzembe helyezési pont](/cli/azure/appservice/web/deployment/slot).

- - -
### <a name="azure-site-list"></a>az Azure webhely-lista
Ebben az előfizetésben hello hello alkalmazásokkal kapcsolatos információk hívás **azure webhelylista**, a következő példa hello a.

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a>az Azure-webhely létrehozása
a telepített környezet tárolóhelye toocreate hívja **azure-webhely létrehozása** és adja meg egy meglévő alkalmazást hello nevét és hello tárolóhely toocreate, mint például a következő hello hello nevét.

`azure site create webappslotstest --slot staging`

hello új tárhely, használjon hello tooenable verziókövetését **– git** lehetőség, mint például a következő hello.

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a>az Azure site lapozófájl-kapacitás
toomake hello frissített telepítési tárolóhely hello éles alkalmazások, használja a hello **azure hely swap** parancs tooperform csereművelet, mint például a következő hello. hello éles alkalmazások nem fog tapasztalni bármely állásidő, és nem fog változni hidegindítás.

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a>az Azure webhely törlése
toodelete üzembe helyezési pont, amely már nem szükséges, használjon hello **azure webhely törlése** parancs, mint például a következő hello.

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> Tekintsen meg működés közben egy webalkalmazást. [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) azonnal, és hozzon létre egy rövid élettartamú alapszintű alkalmazást – nincs szükség bankkártyára, nem jár kötelezettségekkel.
> 
> 

## <a name="next-steps"></a>Következő lépések
[Az Azure App Service Web App – blokkolása web access toonon éles üzembe helyezési](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[bemutatása tooApp szolgáltatás Linux](./app-service-linux-intro.md)
[Microsoft Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

