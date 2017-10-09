---
title: "az Azure Tárolószolgáltatás és Swarm aaaCI/CD |} Microsoft Docs"
description: "Azure Tárolószolgáltatás használata a Docker Swarm, egy Azure-tároló beállításjegyzék és a Visual Studio Team Services toodeliver folyamatosan egy több tároló .NET Core-alkalmazás"
services: container-service
documentationcenter: " "
author: jcorioland
manager: pierlag
tags: acs, azure-container-service
keywords: "Docker, tárolók, Micro-szolgáltatások, a Swarm, a Azure, a Visual Studio Team Services, a DevOps"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 35348800aa620469fb62ab3e5a02b33834359446
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a>Teljes CI/CD-feldolgozási folyamat toodeploy egy több tároló alkalmazás Azure tárolószolgáltatás a Docker Swarmmal Visual Studio Team Services használatával

Egyik hello legnagyobb kihívást képes toodeliver alatt hello felhő modern alkalmazások fejlesztése során ezeknek az alkalmazásoknak folyamatosan. Ebből a cikkből megtudhatja, hogyan tooimplement egy teljes folyamatos integrációt és a Docker Swarm Azure tároló beállításjegyzék és a Visual Studio Team Services használata az Azure Tárolószolgáltatás (CI/CD) telepítési folyamat készítsen, és kiadáskezelés.

Ez a cikk alapján egy egyszerű alkalmazást, a rendelkezésre álló [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), az ASP.NET Core fejlett. négy különböző szolgáltatások hello alkalmazások állnak: három webes API-k és egy webes előtér:

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

hello cél van toodeliver ezen alkalmazás folyamatosan a Docker Swarm-fürt, a Visual Studio Team Services használatával. hello következő részletek az folyamatos kézbesítési adatcsatornát. ábra:

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Hello lépéseket rövid leírása itt található:

1. Kód változtatások véglegesített toohello forráskódraktárban (Itt GitHub) 
2. GitHub elindítja a Visual Studio Team Services build 
3. A Visual Studio Team Services lekérdezi hello hello források legújabb verzióját, és hello alkalmazást alkotó összes hello-lemezképet állít össze 
4. A Visual Studio Team Services minden hello Azure tároló beállításjegyzék szolgáltatás használatával létrehozott lemezképet tooa Docker beállításjegyzék leküldéses értesítések 
5. A Visual Studio Team Services új verziót váltja ki. 
6. hello kiadás néhány SSH használatával fürtcsomóponton hello Azure tároló szolgáltatás fő parancsokat futtat 
7. A docker Swarm hello fürt ponttá hello legújabb verziójában hello lemezképek 
8. használatával a Docker Compose hello hello alkalmazás új verziója van telepítve 

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag elindítása előtt kell toocomplete hello a következő feladatokat:

- [Swarm-fürt létrehozása az Azure Container Service-ben](container-service-deployment.md)
- [Csatlakozás az Azure Tárolószolgáltatásban hello Swarm-fürthöz](../container-service-connect.md)
- [Hozzon létre egy Azure-tárolót beállításjegyzék](../../container-registry/container-registry-get-started-portal.md)
- [Egy Visual Studio Team Services fiók és a team projekt létrehozása](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [Elágazás hello GitHub tárház tooyour GitHub-fiók](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Egy Ubuntu (14.04 vagy 16.04) gépet is telepítve Docker kell. Ezen a számítógépen használt Visual Studio Team Services során hello létrehozni, és felszabadíthatja a folyamatokat. Egyirányú toocreate ezen a számítógépen toouse hello kép hello elérhető [Azure piactér](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/). 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>1. lépés: A Visual Studio Team Services-fiók konfigurálása 

Ebben a szakaszban konfigurálhatja a Visual Studio Team Services-fiók.

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a>A Visual Studio Team Services Linux build ügynök konfigurálása

toocreate Docker képek és ezeket a lemezképeket egy Azure-tárolót beállításjegyzékbe, a Visual Studio Team Services a build leküldéses, kell tooregister egy Linux-ügynök. Ezen telepítési lehetőség közül választhat:

* [Egy Linux-ügynök telepítése](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Használja a Docker toorun hello VSTS-ügynökök](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-hello-docker-integration-vsts-extension"></a>Hello Docker integrációs VSTS-kiterjesztés telepítése

A Microsoft a VSTS bővítmény toowork, a Docker építés biztosít, és felszabadíthatja a folyamatokat. A bővítmény érhető el hello [VSTS piactér](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Kattintson a **telepítése** tooadd a bővítmény tooyour VSTS-fiókhoz:

![Hello Docker-integráció telepítése](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

Meg kell adnia tooconnect tooyour VSTS fiók hitelesítő adataival. 

### <a name="connect-visual-studio-team-services-and-github"></a>Csatlakoztassa a Visual Studio Team Services, GitHub

Állítsa be a kapcsolatot a VSTS-projektet és a GitHub-fiók között.

1. A Visual Studio Team Services projektben kattintson hello **beállítások** ikonra hello eszköztár, és válassza ki a **szolgáltatások**.

    ![A Visual Studio Team Services - külső kapcsolatot](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. Hello bal oldalon kattintson **új szolgáltatási végpont** > **GitHub**.

    ![A Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. a GitHub-fiókjában, tooauthorize VSTS toowork kattintson **engedélyezés** és hello eljárás a hello ablakban.

    ![A Visual Studio Team Services - engedélyezik a Githubon](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-tooyour-azure-container-registry-and-azure-container-service-cluster"></a>Csatlakozás a VSTS tooyour Azure tároló beállításjegyzék és az Azure Tárolószolgáltatás-fürt

hello utolsó be hello CI/CD adatcsatorna elérése előtt lépésekre tooconfigure külső kapcsolatok tooyour tároló beállításjegyzék és a Docker Swarm-fürt az Azure-ban. 

1. A hello **szolgáltatások** a Visual Studio Team Services projekt beállítások hozzáadása típusú szolgáltatásvégpont **Docker beállításjegyzék**. 

2. A hello felugró ablakban nyílik meg írja be a hello URL-cím és az Azure-tárolót beállításjegyzék hello hitelesítő adatait.

    ![A Visual Studio Team Services - Docker beállításjegyzék](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. Hello Docker Swarm-fürt, a típusú végpont hozzáadása **SSH**. Majd adja meg a hello SSH csatlakozási adatait a Swarm-fürt.

    ![A Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Minden hello konfigurálást most. A következő lépések hello hello CI/CD-feldolgozási folyamat létrehozza és telepíti hello alkalmazás toohello Docker Swarm-fürt létrehozása. 

## <a name="step-2-create-hello-build-definition"></a>2. lépés: Hello build definíció létrehozása

Ebben a lépésben a VSTS-projekt egy build definitionfor beállításában és hello létrehozási munkafolyamat megadása a tároló lemezképek

### <a name="initial-definition-setup"></a>Kezdeti beállításának

1. toocreate build definícióját, tooyour Visual Studio Team Services projektre, és kattintson a csatlakozás **Build & kiadási**. 

2. A hello **definíciók Build** kattintson **+ új**. Jelölje be hello **üres** sablont.

    ![A Visual Studio Team Services - új definíció létrehozása](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. Új build hello konfigurálása forrással GitHub tárház, ellenőrizze **folyamatos integrációt**, és jelölje be hello ügynök várólista ahol regisztrálták-e a Linux-ügynök. Kattintson a **létrehozása** toocreate hello build definíciója.

    ![A Visual Studio Team Services - Build-definíció létrehozása](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. A hello **Build definíciók** lapon, először megnyitja az hello **tárház** lapra, és hello build toouse hello elágazás hello MyShop projekt a hello Előfeltételek konfigurálása. Győződjön meg arról, hogy kiválassza *acs-dokumentumok* , hello **alapértelmezett ágat**.

    ![Visual Studio Team Services - Build tárház konfigurációja](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. A hello **eseményindítók** lapján minden véglegesítés után elindított hello build toobe konfigurálása. Válassza ki **folyamatos integrációt** és **módosítások kötegelt**.

    ![Visual Studio Team Services - Build eseményindító konfigurációja](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-hello-build-workflow"></a>Adja meg a hello létrehozási munkafolyamat
hello lépések hello létrehozási munkafolyamat határozza meg. Nincsenek hello öt tároló képek toobuild *MyShop* alkalmazás. Minden egyes lemezképének összeállítása a hello projekt mappákban lévő Dockerfile hello használata:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* ShopFront

Tooadd két Docker lépéseket az egyes lemezképek, egy toobuild hello lemezképét és egy toopush hello kép hello Azure tároló beállításjegyzék van szüksége. 

1. tooadd hello létrehozási munkafolyamat, egy lépésben kattintson **+ Hozzáadás összeállítása lépés** válassza **Docker**.

    ![A Visual Studio Team Services - Build lépéseket adhat hozzá.](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. Az egyes lemezképek konfigurálása egy lépésben hello használó `docker build` parancsot.

    ![A Visual Studio Team Services - Docker-Build](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Hello művelet létrehozni, jelölje be az Azure-tárolót beállításjegyzék hello **lemezképet készít** művelet, és egyes lemezképek definiáló Dockerfile hello. Set hello **összeállítási környezet** hello Dockerfile, gyökérkönyvtár, és adja meg a hello **Lemezképnév**. 
    
    Amint a képernyő megelőző hello, hello kép neve kezdődhet hello URI-azonosítója az Azure-tárolót beállításjegyzék. (A build változó tooparameterize hello címke hello kép, például a hello build azonosítója ebben a példában is használhatja.)

3. Az egyes lemezképek beállítása egy második lépésben hello használó `docker push` parancsot.

    ![A Visual Studio Team Services - Docker leküldéses](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Hello leküldéses művelethez, válassza ki az Azure-tárolót beállításjegyzék hello **lemezkép leküldéses** művelet, és írja be a hello **Lemezképnév** hello előző lépésben épül, amely.

4. Hello build konfigurálását követően, és leküldéses lépéseket az egyes hello öt képek, két további lépéseket a hello build munkafolyamat hozzáadása.

    a. A parancssori feladatot, amely használja a bash parancsfájlok tooreplace hello *BuildNumber* hello docker-compose.yml fájlt az aktuális hello számainak létrehozása azonosítóját. Tekintse meg a következő képernyő Részletek hello.

    ![A Visual Studio Team Services - frissítés Compose fájl](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Egy feladatot, amely hello esik Compose fájl frissíteni, mivel a egy összeállítási összetevő így hello verzióban is használható. Tekintse meg a következő képernyő Részletek hello.

    ![A Visual Studio Team Services - Compose közzététele fájl](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. Kattintson a **mentése** és a build definition neve.

## <a name="step-3-create-hello-release-definition"></a>3. lépés: Hello kiadás definíció létrehozása

A Visual Studio Team Services lehetővé teszi túl[kiadásokban kezelése környezetek között](https://www.visualstudio.com/team-services/release-management/). Folyamatos üzembe helyezés toomake meg arról, hogy az alkalmazás telepítve van a különböző környezetekben (például a fejlesztői, tesztelési, éles üzem előtti és éles) zökkenőmentes úgy is engedélyezheti. Létrehozhat egy új környezetben, amely az Azure tároló szolgáltatás Docker Swarm-fürt jelöli.

![A Visual Studio Team Services - kiadás tooACS](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Eredeti kiadásának beállítása

1. toocreate kiadás definícióját, kattintson a **kiadásokban** > **+ kiadás**

2. tooconfigure hello összetevő forrás, kattintson **összetevők** > **hivatkozás egy összetevő forrás**. Itt csatolja az új kiadási definition toohello build hello előző lépésben meghatározott. Ezzel az eljárással hello docker-compose.yml fájlt érhető hello kiadás során.

    ![A Visual Studio Team Services - kiadás összetevők](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. tooconfigure hello kiadás eseményindító, kattintson a **eseményindítók** válassza **folyamatos üzembe helyezés**. Hello beállítása hello eseményindítón ugyanazon összetevő-forrás. Ez a beállítás biztosítja, hogy az új kiadási elindul, amint hello build sikeresen befejeződik.

    ![A Visual Studio Team Services - kiadás eseményindítók](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-hello-release-workflow"></a>Adja meg a hello kiadás munkafolyamat

hello kiadás munkafolyamat két feladatot hozzáadott tevődik össze.

1. Egy feladat toosecurely másolási hello konfigurálása állítható össze a fájl tooa *telepítése* mappa a hello Docker Swarm fő csomóponton korábban konfigurált hello SSH-kapcsolat használatával. Tekintse meg a következő képernyő Részletek hello.

    ![Visual Studio Team Services - kiadás SCP-je](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. Egy második feladat tooexecute a bash parancs toorun konfigurálása `docker` és `docker-compose` parancsok hello fő csomóponton. Tekintse meg a következő képernyő Részletek hello.

    ![A Visual Studio Team Services - kiadás Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    hello parancs hello fő használata hello Docker parancssori felületén és hello Docker Compose CLI toodo hello követő feladatok végrehajtása:

    - Bejelentkezési toohello Azure tároló beállításjegyzék (három build variab'les hello definiált használ **változók** lapon)
    - Adja meg a hello **DOCKER_HOST** hello Swarm végponthoz változó toowork (: 2375)
    - Keresse meg a toohello *telepítése* , amelyek biztonságos másolási feladat megelőző hello hozta létre, és hello docker-compose.yml fájlt tartalmazó mappa 
    - Végrehajtás `docker-compose` parancsokat, amelyek hello új képek, lekéréses hello szolgáltatásainak leállítása, hello szolgáltatások eltávolítása és hello tárolók létrehozása.

    >[!IMPORTANT]
    > Amint a képernyő megelőző hello, hagyja hello **stderr-en sikertelen** jelölőnégyzet nincs bejelölve. Ez az egy fontos beállítása, mert `docker-compose` több diagnosztikai üzeneteket, jelenít meg, például a tárolók leállítása vagy törlése, a standard hiba kimeneti hello. Ha hello jelölőnégyzetet a Visual Studio Team Services jelenti, hogy hello kiadás során hibák jelentkeztek akkor is, ha minden megfelelően működik.
    >
3. Mentse az új kiadási definíciója.


>[!NOTE]
>A központi telepítés tartalmazza a némi állásidővel, mivel azt hello régi szolgáltatások leállítása és hello újat futtatja. Azt az lehetséges tooavoid Ez egy kék-zöld telepítési végrehajtásával.
>

## <a name="step-4-test-hello-cicd-pipeline"></a>4. lépés Hello CI/CD folyamat tesztelése

Most, hogy hello konfiguráció befejezése után a rendszer idő tootest Ez új CI/CD folyamat. a GitHub-tárház hello tooupdate hello forrás kódot, és aztán hello legegyszerűbb módja tootest változik. Néhány másodperccel azután leküldéses hello kód jelenik meg a Visual Studio Team Services rendszert futtató új buildverziót. Sikeres befejezést követően az új kiadási indul, és hello hello Azure Tárolószolgáltatás-fürt hello alkalmazás új verzióját telepíti.

## <a name="next-steps"></a>Következő lépések

* Visual Studio Team Services CI/CD kapcsolatos további információkért lásd: hello [VSTS összeállítása – áttekintés](https://www.visualstudio.com/docs/build/overview).
