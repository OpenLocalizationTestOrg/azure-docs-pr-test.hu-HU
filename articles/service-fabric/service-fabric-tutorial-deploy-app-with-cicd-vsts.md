---
title: "az Azure Service Fabric-alkalmazás az folyamatos integráció (Team Services) aaaDeploy |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset folyamatos integrációt és a Visual Studio Team Services használatával a Service Fabric-alkalmazás központi telepítését.  Alkalmazás tooa Service Fabric-fürt üzembe helyezése az Azure-ban."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi
ms.openlocfilehash: ba9a632b247b0f467e7b66fbe77b4ad54fb3d9ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-with-cicd-tooa-service-fabric-cluster"></a>Telepítsen egy alkalmazást CI/CD tooa Service Fabric-fürt
Ez az oktatóanyag három azokat, és ismerteti, hogyan tooset folyamatos integrációt és a Visual Studio Team Services használata az Azure Service Fabric-alkalmazások központi telepítését.  Egy meglévő Service Fabric-alkalmazás van szükség, a létrehozott hello alkalmazás [létre olyan .NET alkalmazás](service-fabric-tutorial-create-dotnet-app.md) példaként szolgál.

A hello sorozat három része megismerheti, hogyan:

> [!div class="checklist"]
> * A verziókövetési tooyour hozzáadása
> * Hozzon létre egy build definition Team Services
> * Hozzon létre egy kiadási definition Team Services
> * Automatikus központi telepítése és alkalmazás frissítése

Az oktatóanyag adatsorozat elsajátíthatja, hogyan:
> [!div class="checklist"]
> * [A .NET Service Fabric-alkalmazás létrehozása](service-fabric-tutorial-create-dotnet-app.md)
> * [Hello alkalmazás tooa távoli fürt központi telepítése](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * Konfigurálja a CI/CD Visual Studio Team Services használatával

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez:
- Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Telepítse a Visual Studio 2017](https://www.visualstudio.com/) és hello telepítése **Azure fejlesztési** és **ASP.NET és a webes fejlesztési** munkaterhelések.
- [Hello Service Fabric SDK telepítése](service-fabric-get-started.md)
- Létrehozhat például egy Service Fabric-alkalmazás által [oktatóanyag](service-fabric-tutorial-create-dotnet-app.md). 
- Létrehozhat például egy Azure, a Windows Service Fabric-fürt által [oktatóanyag](service-fabric-tutorial-create-cluster-azure-ps.md)
- Hozzon létre egy [Team Services-fiók](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-hello-voting-sample-application"></a>Hello Voting mintaalkalmazás letöltése
Ha Ön nem build hello Voting mintaalkalmazást [rész az oktatóanyag adatsorozat](service-fabric-tutorial-create-dotnet-app.md), tölthető le. A parancs-ablakban futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Készítse elő a közzétételi profilt
Most, hogy megismerte [az alkalmazások](service-fabric-tutorial-create-dotnet-app.md) és [hello alkalmazás tooAzure telepített](service-fabric-tutorial-deploy-app-to-party-cluster.md), készen áll a tooset folyamatos integrációt be most.  Először készítsen számára az alkalmazáson belül közzétételi profilt, amely végrehajtja a Team Services belül hello központi telepítési folyamat.  a közzétételi profil hello konfigurált tootarget hello fürt, amely a korábban létrehozott kell lennie.  Indítsa el a Visual Studiót, és nyissa meg a meglévő Service Fabric-alkalmazás projekt.  A **Megoldáskezelőben**, kattintson a jobb gombbal a hello alkalmazást, és válassza ki **közzététel...** .

Válasszon egy cél-profilt, az alkalmazás projekt toouse a folyamatos integrációt munkafolyamat belül, például a felhő.  Adja meg a hello fürt kapcsolat végpontja.  Ellenőrizze a hello **frissítési hello alkalmazás** jelölőnégyzetet, hogy az alkalmazás minden központi telepítést, Team Services frissíti.  Hello kattintson **mentése** hivatkozás toosave hello beállítások toohello közzétételi profillal, és kattintson a **Mégse** tooclose hello párbeszédpanel megnyitásához.  

![Leküldéses profil][publish-app-profile]

## <a name="share-your-visual-studio-solution-tooa-new-team-services-git-repo"></a>A Visual Studio megoldás tooa új Team Services Git-tárház megosztása
Ossza meg az alkalmazás forrásfájljait tooa csapatprojekt Team Services, létrehozhat alkot.  

Hozzon létre egy új helyi Git-tárház a projekthez kiválasztásával **tooSource vezérlő hozzáadása** -> **Git** hello állapotsoron a hello jobb alsó sarkában Visual Studio. 

A hello **leküldéses** megtekintése **Team Explorer**, jelölje be hello **Git-tárház közzététele** gombra kattint, a **tooVisual Studio Team Services leküldéses**.

![Leküldéses Git-tárház][push-git-repo]

Ellenőrizze az e-maileket, és válassza ki a fiókot hello **Team Services tartomány** legördülő listán. Adja meg a tárház nevét, és válassza ki **közzététel tárház**.

![Leküldéses Git-tárház][publish-code]

Hello tárház közzétételi hoz létre egy új csapatprojektbe azonos nevet, amint hello helyi tárház hello fiókját. Kattintson egy meglévő csapatprojektben toocreate hello tárház **speciális** következő túl**tárház** nevet, és válassza ki a csapatprojekt. Megtekintheti a kód hello Web kiválasztásával **megtekintéséhez hello webes**.

## <a name="configure-continuous-delivery-with-vsts"></a>Folyamatos kézbesítési VSTS konfigurálása
Team Services build definícióját egy munkafolyamatot, amelyik build ismertetett lépések egymás után végrehajtott áll ismerteti. A build definíció létrehozása, hogy a Service Fabric alkalmazáscsomag és más összetevők, toodeploy tooa Service Fabric-fürtöt hoz létre. További információ [Team Services build definíciók](https://www.visualstudio.com/docs/build/define/create). 

Team Services kiadás definícióját egy munkafolyamatot, amely az alkalmazás csomag tooa fürtöt telepít ismerteti. Együttes használatuk esetén a hello definition készítse el és kiadás definition hello teljes munkafolyamat forrás fájlok tooending a fürtön futó alkalmazással kezdve végrehajtásához. További tudnivalók Team Services [definíciók kiadási](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-definition"></a>A build definíció létrehozása
Nyisson meg egy webböngészőt, és keresse meg az új csapatprojektbe tooyour:: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting. 

SELECT hello **Build & kiadási** lapra, majd **buildek**, majd **+ új definition**.  A **válasszon olyan sablont,**, jelölje be hello **Azure Service Fabric-alkalmazás** sablont, és kattintson **alkalmaz**. 

![Build sablon kiválasztása][select-build-template] 

szavazás alkalmazás hello .NET Core projektet tartalmaz, ezért adja hozzá egy feladatot, amely visszaállítja az hello függőségek. A hello **feladatok** nézetben jelölje ki **+ Hozzáadás feladat** a hello bal alsó. Keresse a "Parancssori" toofind hello parancssori feladatot, majd kattintson a **Hozzáadás**. 

![Feladat hozzáadása][add-task] 

Hello új feladat esetén adja meg "Dotnet.exe Futtatás" **megjelenített név**, "dotnet.exe" **eszköz**, és a "visszaállítás" a **argumentumok**. 

![Új feladat][new-task] 

A hello **eseményindítók** megtekintéséhez kattintson a hello **engedélyezése ehhez az eseményindítóhoz** alatt kapcsoló **folyamatos integrációt**. 

Válassza ki **mentés és a feldolgozási sor** , és írja be a "Kihelyezett VS2017" hello, **ügynök várólista**. Válassza ki **várólista** toomanually build elindításához.  Épít is eseményindítók leküldéses vagy be.

toocheck a build folyamatban, a kapcsoló toohello **buildek** fülre.  Miután meggyőződött hello build sikeresen végrehajtja, adja meg egy kiadási-definíciót, amely telepíti az alkalmazást tooa fürt. 

### <a name="create-a-release-definition"></a>Egy kiadási definíció létrehozása  

Jelölje be hello **Build & kiadási** lapra, majd **kiadásokban**, majd **+ új definition**.  A **létrehozás kiadás definition**, jelölje be hello **Azure Service Fabric telepítési** hello listán, és kattintson a sablon **következő**.  Jelölje be hello **Build** forrás-, ellenőrizze a hello **folyamatos üzembe helyezés** mezőbe, majd kattintson **létrehozása**. 

A hello **környezetek** megtekintéséhez kattintson az **Hozzáadás** jobbra toohello **Fürtkapcsolat**.  Adja meg a kapcsolat neve a "mysftestcluster", "tcp://mysftestcluster.westus.cloudapp.azure.com:19000", és a hello Azure Active Directory vagy a tanúsítvány hitelesítő adatok a fürt végpontja hello fürthöz. Az Azure Active Directory hitelesítő adatok, adja meg a hello hitelesítő adatok toouse tooconnect toohello fürt a hello **felhasználónév** és **jelszó** mezőket. A tanúsítványalapú hitelesítéshez, adja meg a hello Base64 kódolás hello ügyfél tanúsítványfájl hello **ügyféltanúsítvány** mező.  Hello súgó előugró ablak megjelenik, hogy a mező kapcsolatos információk, amelyek érték tooget.  Ha a tanúsítvány jelszóval védett, meghatározása hello jelszó hello **jelszó** mező.  Kattintson a **mentése** toosave hello kiadás definíciója.

![Fürt-kapcsolat hozzáadása][add-cluster-connection] 

Kattintson a **ügynökön**, majd jelölje be **üzemeltetett VS2017** a **telepítési várólista**. Kattintson a **mentése** toosave hello kiadás definíciója.

![Ügynökön][run-on-agent]

Válassza ki **+ kiadási** -> **létrehozása kiadási** -> **létrehozása** toomanually verziót létrehoznia.  Ellenőrizze, hogy hello központi telepítés sikeres volt, és hello alkalmazás hello fürtben fut.  Nyisson meg egy webböngészőt, és keresse meg a túl[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Vegye figyelembe a hello Alkalmazásverzió, ebben a példában "1.0.0.20170616.3". 

## <a name="commit-and-push-changes-trigger-a-release"></a>Véglegesítse és módosításokat, kiadási indítás
tooverify, amelyek folyamatos integrációt csővezeték hello tooTeam szolgáltatások kód változást ellenőrzésével működik.    

Amikor tartalmat ír a kódot, a automatikusan kötetblokkok változásait a Visual Studio. Véglegesítse a módosításokat tooyour helyi Git-tárház függőben lévő módosítások ikonra (hello kiválasztásával![Függőben][pending]) a hello állapotsor a hello jobb alsó sarok.

A hello **módosítások** Team Intézőben, vegye fel a frissítést leíró üzenet és a változtatások véglegesítése a határidő.

![Az összes véglegesítése][changes]

Jelölje be hello közzé nem tett változás állapotikon sávjának (![közzé nem tett módosítások][unpublished-changes]) vagy a szinkronizálási nézet Team Explorer hello. Válassza ki **leküldéses** tooupdate a kódot a Team Services/TFS.

![Küldje el a módosításokat][push]

Hello módosítások tooTeam küldését szolgáltatások automatikusan eseményindítók build.  Ha hello build definition sikeresen befejeződött, a kiadási automatikusan létrejön, és elindítja a hello alkalmazás hello fürt frissítése.

toocheck a build folyamatban, a kapcsoló toohello **buildek** lapján **Team Explorer** a Visual Studióban.  Miután meggyőződött hello build sikeresen végrehajtja, adja meg egy kiadási-definíciót, amely telepíti az alkalmazást tooa fürt.

Ellenőrizze, hogy hello központi telepítés sikeres volt, és hello alkalmazás hello fürtben fut.  Nyisson meg egy webböngészőt, és keresse meg a túl[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Vegye figyelembe a hello Alkalmazásverzió, ebben a példában "1.0.0.20170815.3".

![Service Fabric Explorer][sfx1]

## <a name="update-hello-application"></a>Hello alkalmazás frissítése
Kód módosításokat hello alkalmazásban.  Mentse és hello módosítások véglegesítéséhez, hello előző lépések.

Miután hello frissítése hello alkalmazás kezd, hello a frissítési folyamat állapotát, a Service Fabric Explorer figyelemmel követheti:

![Service Fabric Explorer][sfx2]

Az alkalmazásfrissítés hello több percig is eltarthat. Hello frissítés befejeződése után hello alkalmazást futtatni fogja hello következő verziójára.  Ebben a példában "1.0.0.20170815.4".

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * A verziókövetési tooyour hozzáadása
> * A build definíció létrehozása
> * Egy kiadási definíció létrehozása
> * Automatikus központi telepítése és alkalmazás frissítése

Most, hogy egy alkalmazást telepített és konfigurált folyamatos integrációt, próbálkozzon a hello következő:
- [Alkalmazások frissítése](service-fabric-application-upgrade.md)
- [Az alkalmazás tesztelése](service-fabric-testability-overview.md) 
- [Figyelése és diagnosztizálása](service-fabric-diagnostics-overview.md)


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[add-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddTask.png
[new-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewTask.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
[run-on-agent]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/RunOnAgent.png