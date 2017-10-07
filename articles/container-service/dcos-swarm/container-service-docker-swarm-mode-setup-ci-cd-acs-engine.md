---
title: "az Azure-tároló szolgáltatás vezérlőprogramja és Swarm mód aaaCI/CD |} Microsoft Docs"
description: "Azure-tároló szolgáltatás vezérlőprogramja használata a Docker Swarm-módban, egy Azure-tároló beállításjegyzék és a Visual Studio Team Services toodeliver folyamatosan egy több tároló .NET Core-alkalmazás"
services: container-service
documentationcenter: " "
author: diegomrtnzg
manager: esterdnb
tags: acs, azure-container-service, acs-engine
keywords: "Swarm-docker, tárolók, Micro-szolgáltatások, Azure, a Visual Studio Team Services, a DevOps, ACS-motor"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 040522c452f7ea0ce3c92f2fe57b1c141b97e380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a>Teljes CI/CD-feldolgozási folyamat toodeploy Azure tárolószolgáltatás egy több tároló alkalmazást az ACS-motor és a Docker Swarm mód Visual Studio Team Services használatával

*Ez a cikk alapján [teljes CI/CD-feldolgozási folyamat toodeploy egy Azure tárolószolgáltatás a Docker Swarmmal Visual Studio Team Services használatával több tároló alkalmazás](container-service-docker-swarm-setup-ci-cd.md) dokumentáció*

Napjainkban egyik hello legnagyobb kihívást képes toodeliver alatt hello felhő modern alkalmazások fejlesztése során ezeknek az alkalmazásoknak folyamatosan. Ebből a cikkből megismerheti, hogyan tooimplement egy teljes folyamatos integrációt és a központi telepítés (CI/CD) a következő feldolgozási sorban használatával: 
* Azure-tárolót szolgáltatás motorja a Docker Swarm mód
* Azure Container Registry
* Visual Studio Team Services

Ez a cikk alapján egy egyszerű alkalmazást, a rendelkezésre álló [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), az ASP.NET Core fejlett. négy különböző szolgáltatások hello alkalmazások állnak: három webes API-k és egy webes előtér:

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

hello cél toodeliver van az alkalmazás folyamatosan fürtben Docker Swarm-módban, a Visual Studio Team Services használatával. hello következő részletek az folyamatos kézbesítési adatcsatornát. ábra:

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

Hello lépéseket rövid leírása itt található:

1. Kód változtatások véglegesített toohello forráskódraktárban (Itt GitHub) 
2. GitHub elindítja a Visual Studio Team Services build 
3. A Visual Studio Team Services lekérdezi hello hello források legújabb verzióját, és hello alkalmazást alkotó összes hello-lemezképet állít össze 
4. A Visual Studio Team Services minden hello Azure tároló beállításjegyzék szolgáltatás használatával létrehozott lemezképet tooa Docker beállításjegyzék leküldéses értesítések 
5. A Visual Studio Team Services új verziót váltja ki. 
6. hello kiadás néhány SSH használatával fürtcsomóponton hello Azure tároló szolgáltatás fő parancsokat futtat 
7. Docker Swarm mód hello fürtön hello hello képek legújabb verzióját kéri le. 
8. hello alkalmazás új verziójának hello Docker verem használatával van telepítve 

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag elindítása előtt kell toocomplete hello a következő feladatokat:

- [Az Azure Tárolószolgáltatásban ACS motorral mód Swarm-fürt létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [Csatlakozás az Azure Tárolószolgáltatásban hello Swarm-fürthöz](../container-service-connect.md)
- [Hozzon létre egy Azure-tárolót beállításjegyzék](../../container-registry/container-registry-get-started-portal.md)
- [Egy Visual Studio Team Services fiók és a team projekt létrehozása](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [Elágazás hello GitHub tárház tooyour GitHub-fiók](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> az Azure Tárolószolgáltatásban hello Docker Swarm orchestrator örökölt önálló Swarm használja. Jelenleg az integrált hello [Swarm mód](https://docs.docker.com/engine/swarm/) (Docker 1.12 és újabb) értéke nem egy támogatott orchestrator, az Azure Tárolószolgáltatásban. Emiatt használjuk [ACS motor](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), közösségi hozzájárult [gyorsindítási sablonon](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), vagy egy Docker-megoldás a hello [Azure piactér](https://azuremarketplace.microsoft.com).
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>1. lépés: A Visual Studio Team Services-fiók konfigurálása 

Ebben a szakaszban konfigurálhatja a Visual Studio Team Services-fiók. tooconfigure VSTS Services végpontjainak, a Visual Studio Team Services projektben kattintson hello **beállítások** ikonra hello eszköztár, és válassza ki a **szolgáltatások**.

![Nyissa meg a szolgáltatások végpont](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a>Csatlakozás a Visual Studio Team Services és az Azure-fiók

Állítsa be a kapcsolatot a VSTS-projektet és az Azure-fiókjával között.

1. Hello bal oldalon kattintson **új szolgáltatási végpont** > **Azure Resource Manager**.
2. tooauthorize VSTS toowork az Azure-fiókjával, és válassza ki a **előfizetés** kattintson **OK**.

    ![A Visual Studio Team Services - Azure engedélyezése](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a>Csatlakoztassa a Visual Studio Team Services, GitHub

Állítsa be a kapcsolatot a VSTS-projektet és a GitHub-fiók között.

1. Hello bal oldalon kattintson **új szolgáltatási végpont** > **GitHub**.
2. a GitHub-fiókjában, tooauthorize VSTS toowork kattintson **engedélyezés** és hello eljárás a hello ablakban.

    ![A Visual Studio Team Services - engedélyezik a Githubon](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-tooyour-azure-container-service-cluster"></a>Csatlakozás VSTS tooyour Azure Tárolószolgáltatás-fürt

hello utolsó be hello CI/CD adatcsatorna elérése előtt lépésekre tooconfigure külső kapcsolatok tooyour Docker Swarm-fürt az Azure-ban. 

1. Hello Docker Swarm-fürt, a típusú végpont hozzáadása **SSH**. Majd adja meg a hello SSH csatlakozási adatait a Swarm-fürt (főcsomópont).

    ![A Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

Minden hello konfigurálást most. A következő lépések hello hello CI/CD-feldolgozási folyamat létrehozza és telepíti hello alkalmazás toohello Docker Swarm-fürt létrehozása. 

## <a name="step-2-create-hello-build-definition"></a>2. lépés: Hello build definíció létrehozása

Ebben a lépésben a VSTS project build definícióját beállítása és hello létrehozási munkafolyamat megadása a tároló lemezképek

### <a name="initial-definition-setup"></a>Kezdeti beállításának

1. toocreate build definícióját, tooyour Visual Studio Team Services projektre, és kattintson a csatlakozás **Build & kiadási**. A hello **definíciók Build** kattintson **+ új**. 

    ![A Visual Studio Team Services - új definíció létrehozása](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. Jelölje be hello **üres folyamat**.

    ![A Visual Studio Team Services - új üres Build definíció](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. Kattintson a hello **változók** lapra, és hozzon létre két új változót: **RegistryURL** és **AgentURL**. Illessze be a beállításjegyzék és a fürt ügynökök DNS hello értékének.

    ![A Visual Studio Team Services - Build változók konfigurációját](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. A hello **Build definíciók** lap, nyissa meg hello **eseményindítók** lapra, és hello build toouse folyamatos integrációt hello elágazás hello MyShop projekt a hello Előfeltételek konfigurálása. Ezt követően válassza **módosítások kötegelt**. Győződjön meg arról, hogy kiválassza *docker-linux* , hello **specification fiókirodai**.

    ![Visual Studio Team Services - Build tárház konfigurációja](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. Végül kattintson a hello **beállítások** lapra, és konfigurálja a hello alapértelmezett ügynök várólista túl**üzemeltetett Linux előzetes**.

    ![A Visual Studio Team Services - Gazdagépügynök-konfigurálási](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-hello-build-workflow"></a>Adja meg a hello létrehozási munkafolyamat
hello lépések hello létrehozási munkafolyamat határozza meg. Először hello kód tooconfigure hello forrását. toodo, jelölje be **GitHub** és a **tárház** és **fiókirodai** (docker-linux).

![Konfigurálja a Visual Studio Team Services - kód forrása](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

Nincsenek hello öt tároló képek toobuild *MyShop* alkalmazás. Minden egyes lemezképének összeállítása a hello projekt mappákban lévő Dockerfile hello használata:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* ShopFront

Az egyes lemezképek két Docker lépéseket, egy toobuild hello lemezképét és egy toopush hello kép hello Azure tároló beállításjegyzék van szüksége. 

1. tooadd hello létrehozási munkafolyamat, egy lépésben kattintson **+ Hozzáadás összeállítása lépés** válassza **Docker**.

    ![A Visual Studio Team Services - Build lépéseket adhat hozzá.](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. Az egyes lemezképek konfigurálása egy lépésben hello használó `docker build` parancsot.

    ![A Visual Studio Team Services - Docker-Build](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    Hello művelet létrehozni, jelölje be az Azure-tároló beállításjegyzék hello **lemezképet készít** művelet, és egyes lemezképek definiáló Dockerfile hello. Set hello **munkakönyvtár** hello Dockerfile gyökérkönyvtár, adja meg a hello **kép neve**, és válassza ki **legújabb címkét tartalmaz**.
    
    hello Lemezképnév toobe rendelkezik a következő formátumban: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```. Cserélje le **[NAME]** hello kép nevű:
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. Az egyes lemezképek beállítása egy második lépésben hello használó `docker push` parancsot.

    ![A Visual Studio Team Services - Docker leküldéses](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    Hello leküldéses művelethez, válassza ki az Azure-tárolót beállításjegyzék hello **lemezkép leküldéses** művelet, adja meg a hello **kép neve** , amely hello előző lépést, és válassza ki a beépített **legújabb címke közé tartozik** .

4. Hello build konfigurálását követően és leküldéses lépéseket az egyes hello öt képek, adja hozzá a hello három további lépéseket munkafolyamat létrehozása.

   ![A Visual Studio Team Services - parancssori feladat hozzáadása](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. A parancssori feladatot, amely használja a bash parancsfájlok tooreplace hello *RegistryURL* hello docker-compose.yml fájlt hello RegistryURL változóval előfordulása. 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio Team Services - frissítés Compose fájl beállításjegyzék URL-címe](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. A parancssori feladatot, amely használja a bash parancsfájlok tooreplace hello *AgentURL* hello docker-compose.yml fájlt hello AgentURL változóval előfordulása.
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. Egy feladatot, amely hello esik Compose fájl frissíteni, mivel a egy összeállítási összetevő így hello verzióban is használható. Tekintse meg a következő képernyő Részletek hello.

         ![A Visual Studio Team Services - összetevő közzététele](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![A Visual Studio Team Services - Compose közzététele fájl](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. Kattintson a **mentés és a feldolgozási sor** tootest a build definícióját.

   ![Visual Studio Team Services - mentés- és várólista](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![A Visual Studio Team Services - új sor](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. Ha hello **Build** helyes-e, rendelkezik toosee ezen a képernyőn látható:

  ![A Visual Studio Team Services - Build sikeres volt.](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-hello-release-definition"></a>3. lépés: Hello kiadás definíció létrehozása

A Visual Studio Team Services lehetővé teszi túl[kiadásokban kezelése környezetek között](https://www.visualstudio.com/team-services/release-management/). Folyamatos üzembe helyezés toomake meg arról, hogy az alkalmazás telepítve van a különböző környezetekben (például a fejlesztői, tesztelési, éles üzem előtti és éles) zökkenőmentes úgy is engedélyezheti. Létrehozhat egy olyan környezetben, az Azure tároló szolgáltatás Docker Swarm mód fürt jelöli.

![A Visual Studio Team Services - kiadás tooACS](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Eredeti kiadásának beállítása

1. toocreate kiadás definícióját, kattintson a **kiadásokban** > **+ kiadás**

2. tooconfigure hello összetevő forrás, kattintson **összetevők** > **hivatkozás egy összetevő forrás**. Itt csatolja az új kiadási definition toohello build hello előző lépésben meghatározott. Ezt követően hello docker-compose.yml fájlt érhető hello kiadás során.

    ![A Visual Studio Team Services - kiadás összetevők](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. tooconfigure hello kiadás eseményindító, kattintson a **eseményindítók** válassza **folyamatos üzembe helyezés**. Hello beállítása hello eseményindítón ugyanazon összetevő-forrás. Ez a beállítás biztosítja, hogy egy új kiadási kezdődik, amikor hello build sikeresen befejeződik.

    ![A Visual Studio Team Services - kiadás eseményindítók](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. tooconfigure hello kiadás változók, kattintson a **változók** válassza **+ változó** toocreate három új változók hello beállításjegyzék hello információval: **docker.username**, **docker.password**, és **docker.registry**. Illessze be a beállításjegyzék és a fürt ügynökök DNS hello értékének.

    ![Visual Studio Team Services - Build tárház konfigurációja](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > Amint az előző üdvözlő képernyőt, kattintson a hello **zárolási** docker.password jelölőnégyzet. Ez a beállítás az fontos toorestrict hello jelszó.
    >

### <a name="define-hello-release-workflow"></a>Adja meg a hello kiadás munkafolyamat

hello kiadás munkafolyamat két feladatot hozzáadott tevődik össze.

1. Egy feladat toosecurely másolási hello konfigurálása állítható össze a fájl tooa *telepítése* mappa a hello Docker Swarm fő csomóponton korábban konfigurált hello SSH-kapcsolat használatával. Tekintse meg a következő képernyő Részletek hello.
    
    Forrásmappa:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```

    ![Visual Studio Team Services - kiadás SCP-je](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. Egy második feladat tooexecute a bash parancs toorun konfigurálása `docker` és `docker stack deploy` parancsok hello fő csomóponton. Tekintse meg a következő képernyő Részletek hello.

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![A Visual Studio Team Services - kiadás Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    hello fő végrehajtott hello parancs hello Docker parancssori felületén és hello Docker Compose CLI toodo hello feladatok a következő használja:

    - Jelentkezzen be Azure-tárolót beállításjegyzék toohello (hello definiált három build-változókat használ **változók** lapon)
    - Adja meg a hello **DOCKER_HOST** hello Swarm végponthoz változó toowork (: 2375)
    - Keresse meg a toohello *telepítése* , amelyek biztonságos másolási feladat megelőző hello hozta létre, és hello docker-compose.yml fájlt tartalmazó mappa 
    - Végrehajtás `docker stack deploy` parancsokat, amelyek hello új képek lekéréses és hello tárolók létrehozása.

    >[!IMPORTANT]
    > Amint az előző üdvözlő képernyőt, hagyja hello **stderr-en sikertelen** jelölőnégyzet nincs bejelölve. Ez a beállítás lehetővé teszi toocomplete hello kiadás folyamat miatt túl`docker-compose` több diagnosztikai üzeneteket, jelenít meg, például a tárolók leállítása vagy törlése, a standard hiba kimeneti hello. Ha hello jelölőnégyzetet a Visual Studio Team Services jelenti, hogy hello kiadás során hibák jelentkeztek akkor is, ha minden megfelelően működik.
    >
3. Mentse az új kiadási definíciója.

## <a name="step-4-test-hello-cicd-pipeline"></a>4. lépés: Hello CI/CD folyamat tesztelése

Most, hogy hello konfiguráció befejezése után a rendszer idő tootest Ez új CI/CD folyamat. a GitHub-tárház hello tooupdate hello forrás kódot, és aztán hello legegyszerűbb módja tootest változik. Néhány másodperccel azután leküldéses hello kód jelenik meg a Visual Studio Team Services rendszert futtató új buildverziót. Sikeres befejezést követően egy új kiadási elindul, és hello hello Azure Tárolószolgáltatás-fürt hello alkalmazás új verziójának telepítése.

## <a name="next-steps"></a>Következő lépések

* Visual Studio Team Services CI/CD kapcsolatos további információkért lásd: hello [VSTS összeállítása – áttekintés](https://www.visualstudio.com/docs/build/overview).
* Az ACS-motor kapcsolatos további információkért lásd: hello [ACS motor GitHub-tárház](https://github.com/Azure/acs-engine).
* Docker Swarm móddal kapcsolatos további információkért lásd: hello [Docker Swarm – áttekintés](https://docs.docker.com/engine/swarm/).
