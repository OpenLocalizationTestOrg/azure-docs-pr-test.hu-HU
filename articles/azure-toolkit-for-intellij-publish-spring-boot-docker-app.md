---
title: "a rugó rendszerindító alkalmazást egy Docker-tároló segítségével aaaPublish hello Azure eszközkészlet IntelliJ |} Microsoft Docs"
description: "Megtudhatja, hogyan toopublish egy webes alkalmazás tooMicrosoft Azure-bA egy Docker-tároló segítségével hello Azure eszközkészlet intellij-t."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/21/2017
ms.author: robmcm
ms.openlocfilehash: 8964cb33fd8f61a39f091633ae9074d9658232fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a><span data-ttu-id="bc52e-103">A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, IntelliJ hello Azure eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="bc52e-103">Publish a Spring Boot app as a Docker container by using hello Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="bc52e-104">Hello [rugó keretrendszer] egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="bc52e-104">hello [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="bc52e-105">Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bc52e-105">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="bc52e-106">[Docker] egy nyílt forráskódú megoldás, amely a fejlesztőket automatizálásához hello telepítési méretezés és a tárolók a futó alkalmazások kezelését.</span><span class="sxs-lookup"><span data-stu-id="bc52e-106">[Docker] is an open-source solution that helps developers automate hello deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="bc52e-107">Ez az oktatóanyag bemutatja, hogyan hello lépéseket toodeploy egy rugó rendszerindító alkalmazásra, amely egy Docker-tároló tooMicrosoft Azure IntelliJ hello Azure eszközkészlet használatával.</span><span class="sxs-lookup"><span data-stu-id="bc52e-107">This tutorial walks you through hello steps toodeploy a Spring Boot application as a Docker container tooMicrosoft Azure by using hello Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repo"></a><span data-ttu-id="bc52e-108">Hello alapértelmezett rugó rendszerindító Docker tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="bc52e-108">Clone hello default Spring Boot Docker repo</span></span>

<span data-ttu-id="bc52e-109">hello következő lépések végigvezetik hello rugó rendszerindító Docker tárház klónozása IntelliJ használatával.</span><span class="sxs-lookup"><span data-stu-id="bc52e-109">hello following steps walk you through cloning hello Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="bc52e-110">Ha azt szeretné, hogy a parancssor toouse, [rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Linux központilag][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="bc52e-110">If you want toouse a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="bc52e-111">Nyissa meg az intellij-t.</span><span class="sxs-lookup"><span data-stu-id="bc52e-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="bc52e-112">Hello üdvözlőképernyőn jelölje ki a hello **GitHub** hello beállítása **tekintse meg a verziókövetés** listája.</span><span class="sxs-lookup"><span data-stu-id="bc52e-112">On hello welcome screen, select hello **GitHub** option in hello **Check out from Version Control** list.</span></span>

   ![GitHub Verziókövetés beállítása][CL01]

1. <span data-ttu-id="bc52e-114">Adja meg a hitelesítő adatokat, ha a kért toolog.</span><span class="sxs-lookup"><span data-stu-id="bc52e-114">Enter your credentials if you are prompted toolog in.</span></span>

   * <span data-ttu-id="bc52e-115">Ha a felhasználónév/jelszó toolog tooGitHub használ:</span><span class="sxs-lookup"><span data-stu-id="bc52e-115">If you are using a username/password toolog in tooGitHub:</span></span>

      ![A GitHub-felhasználónevét és a jelszó megadása párbeszédpanel][CL02a]

   * <span data-ttu-id="bc52e-117">Ha egy token toolog tooGitHub használ:</span><span class="sxs-lookup"><span data-stu-id="bc52e-117">If you are using a token toolog in tooGitHub:</span></span>

      ![A GitHub-token megadása párbeszédpanel][CL02b]

1. <span data-ttu-id="bc52e-119">Adja meg **https://github.com/spring-guides/gs-spring-boot-docker.git** hello tárház URL-címhez, adja meg a helyi elérési út és a mappa adatait, és kattintson **Klónozás**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for hello repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![Klónozott tárház párbeszédpanel][CL03]

1. <span data-ttu-id="bc52e-121">Amikor felszólítja az IntelliJ projektre, válassza toocreate **nem**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-121">When you're prompted toocreate an IntelliJ project, select **No**.</span></span>

   ![Az IntelliJ-projektek toocreate elutasítása][CL04]

1. <span data-ttu-id="bc52e-123">A hello kezdőlapján kattintson **projekt importálása**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-123">On hello welcome page, click **Import Project**.</span></span>

   ![Importálja a projekt kiválasztása][CL05]

1. <span data-ttu-id="bc52e-125">Keresse meg a hello elérési utat, ahol hello rugó rendszerindító tárház klónozott, válassza ki a hello **teljes** mappa hello gyökérszintű, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-125">Locate hello path where you cloned hello Spring Boot repo, select hello **complete** folder under hello root, and then click **OK**.</span></span>

   ![Válasszon egy mappát az importálás][CL06]

1. <span data-ttu-id="bc52e-127">Amikor a rendszer kéri, válassza ki a **Create project meglévő forrásokból származó**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![A beállítás toocreate meglévő forrásokból projekt][CL07]

1. <span data-ttu-id="bc52e-129">Írja be a projekt nevét vagy hello alapértelmezés elfogadásához, ellenőrizze a helyes elérési utat toohello hello **teljes** mappa, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-129">Specify your project name or accept hello default, verify hello correct path toohello **complete** folder, and then click **Next**.</span></span>

   ![Adja meg a hello projekt neve][CL08]

1. <span data-ttu-id="bc52e-131">Az importálás könyvtárak testreszabása, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![Válassza ki a könyvtárak][CL09]

1. <span data-ttu-id="bc52e-133">Tekintse át a hello szalagtárak tooimport, majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-133">Review hello libraries tooimport, and then click **Next**.</span></span>

   ![Tekintse át a projekt függvénytárak][CL10]

1. <span data-ttu-id="bc52e-135">Tekintse át a hello modul szerkezete, majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-135">Review hello module structure, and then click **Next**.</span></span>

   ![Tekintse át a modul szerkezete][CL11]

1. <span data-ttu-id="bc52e-137">Adja meg a JDK, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-137">Specify your JDK, and then click **Next**.</span></span>

   ![Adja meg a JDK][CL12]

1. <span data-ttu-id="bc52e-139">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc52e-139">Click **Finish**.</span></span>

   ![A Befejezés gombra.][CL13]

<span data-ttu-id="bc52e-141">Az IntelliJ hello rugó rendszerindító alkalmazás projektként importálja, és hello struktúra megjelenít, amikor hello befejeződött.</span><span class="sxs-lookup"><span data-stu-id="bc52e-141">IntelliJ imports hello Spring Boot app as a project and displays hello structure when hello import has finished.</span></span>

![Az IntelliJ rugó rendszerindító alkalmazás][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="bc52e-143">A rugó rendszerindító build alkalmazás</span><span class="sxs-lookup"><span data-stu-id="bc52e-143">Build your Spring Boot app</span></span>

### <a name="build-hello-app-by-using-hello-maven-pom"></a><span data-ttu-id="bc52e-144">Hozza létre a hello alkalmazását hello Maven POM használatával</span><span class="sxs-lookup"><span data-stu-id="bc52e-144">Build hello app by using hello Maven POM</span></span>

1. <span data-ttu-id="bc52e-145">Hello Maven eszköz ablak nyílik meg, ha már nincs megnyitva.</span><span class="sxs-lookup"><span data-stu-id="bc52e-145">Open hello Maven tool window if it is not already opened.</span></span> <span data-ttu-id="bc52e-146">Kattintson a **nézet** > **Windows eszköz** > **Maven-projektek**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![Eszköz Windows és a Maven-projektek parancsok][BU01]

1. <span data-ttu-id="bc52e-148">Hello Maven-eszköz ablakában kattintson a jobb gombbal **csomag** válassza **Maven Build futtatása**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-148">In hello Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="bc52e-149">(Ha a Maven project nem jelenik meg automatikusan, kattintson a hello **újbóli importálása** hello Maven eszköztár ikonjára.)</span><span class="sxs-lookup"><span data-stu-id="bc52e-149">(If your Maven project does not show up automatically, click hello **Reimport** icon on hello Maven toolbar.)</span></span>

   ![Maven Build parancs futtatása][BU02]

1. <span data-ttu-id="bc52e-151">Az IntelliJ megjelenjen-e egy **létrehozása sikeres** jelenik meg, ha a rugó rendszerindító alkalmazás sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="bc52e-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![BUILD üzenetet][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="bc52e-153">A telepítés kész eltérés létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc52e-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="bc52e-154">toopublish a rugó rendszerindító alkalmazást, a telepítés kész összetevő toocreate van szüksége.</span><span class="sxs-lookup"><span data-stu-id="bc52e-154">toopublish your Spring Boot app, you need toocreate a deployment-ready artifact.</span></span> <span data-ttu-id="bc52e-155">A lépéseket követve hello használata:</span><span class="sxs-lookup"><span data-stu-id="bc52e-155">Use hello following steps:</span></span>

1. <span data-ttu-id="bc52e-156">Nyissa meg a webalkalmazás-projektet az intellij-t.</span><span class="sxs-lookup"><span data-stu-id="bc52e-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="bc52e-157">Kattintson a **fájl**, és kattintson a **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-157">Click **File**, and then click **Project Structure**.</span></span>

   ![Projekt struktúra parancs][ART01]

1. <span data-ttu-id="bc52e-159">Kattintson a zöld plusz hello (**+**) tooadd összetevő szimbólumának, kattintson a **JAR**, és kattintson a **üres**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-159">Click hello green plus (**+**) symbol tooadd an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![Összetevő felvétele][ART02]

1. <span data-ttu-id="bc52e-161">Az összetevő neve eközben nem tooadd kiterjesztés ".jar" hello, és adja meg az Maven kimeneti hello hello célmappa.</span><span class="sxs-lookup"><span data-stu-id="bc52e-161">Name your artifact while making sure not tooadd hello ".jar" extension, and then specify hello target folder for hello Maven output.</span></span>

   ![Adja meg az összetevő tulajdonságai][ART03]

1. <span data-ttu-id="bc52e-163">Az összetevő a jegyzékfájl létrehozása (nem kötelező):</span><span class="sxs-lookup"><span data-stu-id="bc52e-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="bc52e-164">a.</span><span class="sxs-lookup"><span data-stu-id="bc52e-164">a.</span></span> <span data-ttu-id="bc52e-165">Kattintson a **jegyzékfájl létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-165">Click **Create Manifest**.</span></span>

      ![Hello jegyzékfájl létrehozása gombra][ART04a]

   <span data-ttu-id="bc52e-167">b.</span><span class="sxs-lookup"><span data-stu-id="bc52e-167">b.</span></span> <span data-ttu-id="bc52e-168">Válassza ki a hello alapértelmezett elérési hello összetevő, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-168">Choose hello default path for hello artifact, and then click **OK**.</span></span>

      ![Adja meg az összetevő elérési útja][ART04b]

   <span data-ttu-id="bc52e-170">c.</span><span class="sxs-lookup"><span data-stu-id="bc52e-170">c.</span></span> <span data-ttu-id="bc52e-171">Hello három pont gombra (**...** ) toolocate hello fő osztályban.</span><span class="sxs-lookup"><span data-stu-id="bc52e-171">Click hello ellipsis (**...**) toolocate hello main class.</span></span>

      ![Keresse meg a fő osztály][ART04c]

   <span data-ttu-id="bc52e-173">d.</span><span class="sxs-lookup"><span data-stu-id="bc52e-173">d.</span></span> <span data-ttu-id="bc52e-174">Válassza ki a fő osztályban, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-174">Choose your main class, and then click **OK**.</span></span>

      ![Adja meg a fő osztály][ART04d]

1. <span data-ttu-id="bc52e-176">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc52e-176">Click **OK**.</span></span>

   ![Zárja be a hello szerkezetének párbeszédpanel][ART05]

> [!NOTE]
> <span data-ttu-id="bc52e-178">Az IntelliJ összetevők létrehozásával kapcsolatos további információkért lásd: [konfigurálása összetevők] hello JetBrains webhelyen.</span><span class="sxs-lookup"><span data-stu-id="bc52e-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on hello JetBrains website.</span></span>
>

### <a name="build-hello-artifact-for-deployment"></a><span data-ttu-id="bc52e-179">Központi telepítés hello összetevő létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc52e-179">Build hello artifact for deployment</span></span>

1. <span data-ttu-id="bc52e-180">Kattintson a **Build**, és kattintson a **összetevők**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![Az összetevők parancs létrehozása][BU04]

1. <span data-ttu-id="bc52e-182">Ha hello **Build összetevő** helyi menü megjelenik, kattintson a **Build**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-182">When hello **Build Artifact** context menu appears, click **Build**.</span></span>

   ![Build összetevő a helyi menü][BU05]

<span data-ttu-id="bc52e-184">A rugó rendszerindító alkalmazás befejeződött hello összetevő IntelliJ megjelenjen-e hello projekt eszköz ablakban.</span><span class="sxs-lookup"><span data-stu-id="bc52e-184">IntelliJ should display hello completed artifact for your Spring Boot app in hello project tool window.</span></span>

   ![Létrehozott összetevő][BU06]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="bc52e-186">A webes alkalmazás tooAzure közzététele egy Docker-tároló használatával</span><span class="sxs-lookup"><span data-stu-id="bc52e-186">Publish your web app tooAzure by using a Docker container</span></span>

1. <span data-ttu-id="bc52e-187">Ha nincs bejelentkezve tooyour Azure-fiókra, kövesse hello [bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ][Azure Sign In for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="bc52e-187">If you have not signed in tooyour Azure account, follow hello steps in [Sign-in instructions for hello Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="bc52e-188">Hello Project Explorer eszköz ablakában kattintson a jobb gombbal a hello projektet, és válassza **Azure** > **Docker-tároló közzététel**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-188">In hello Project Explorer tool window, right-click hello project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![Docker-tároló parancsként közzététele][PU01]

1. <span data-ttu-id="bc52e-190">Ha hello **Docker-tároló központi telepítése az Azure-on** párbeszédpanel jelenik meg, a meglévő Docker-gazdagépekből jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="bc52e-190">When hello **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="bc52e-191">Ha úgy dönt, hogy toodeploy tooan meglévő állomás, kihagyhatja toostep 4.</span><span class="sxs-lookup"><span data-stu-id="bc52e-191">If you choose toodeploy tooan existing host, you can skip toostep 4.</span></span> <span data-ttu-id="bc52e-192">Ellenkező esetben használja a következő lépéseket toocreate állomás hello:</span><span class="sxs-lookup"><span data-stu-id="bc52e-192">Otherwise, use hello following steps toocreate a host:</span></span>

   <span data-ttu-id="bc52e-193">a.</span><span class="sxs-lookup"><span data-stu-id="bc52e-193">a.</span></span> <span data-ttu-id="bc52e-194">Kattintson a zöld plusz hello (**+**) szimbólum.</span><span class="sxs-lookup"><span data-stu-id="bc52e-194">Click hello green plus (**+**) symbol.</span></span>

      ![Új Docker-gazdagép hozzáadása][PU02]

   <span data-ttu-id="bc52e-196">b.</span><span class="sxs-lookup"><span data-stu-id="bc52e-196">b.</span></span> <span data-ttu-id="bc52e-197">Ha hello **Docker állomás létrehozása** párbeszédpanel jelenik meg, dönthet, hogy tooaccept hello alapértelmezett értékeket, vagy megadhatja az egyéni beállításait az új Docker-állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="bc52e-197">When hello **Create Docker Host** dialog box appears, you can choose tooaccept hello defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="bc52e-198">(A részletes leírását hello különböző beállításairól [a webes alkalmazás is egy Docker-tároló közzé a IntelliJ hello Azure eszközkészlet használatával][Publish Container with Azure Toolkit].) Kattintson a **következő** esetén mely beállítások toouse megadott.</span><span class="sxs-lookup"><span data-stu-id="bc52e-198">(For detailed descriptions of hello various settings, see [Publish a web app as a Docker container by using hello Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings toouse.</span></span>

      ![Adja meg a Docker állomás beállításai][PU03a]

   <span data-ttu-id="bc52e-200">c.</span><span class="sxs-lookup"><span data-stu-id="bc52e-200">c.</span></span> <span data-ttu-id="bc52e-201">Az az Azure key vault közül választhat a meglévő toouse bejelentkezési hitelesítő adatok, vagy dönthet úgy tooenter új Docker bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="bc52e-201">You can choose toouse existing login credentials from an Azure key vault, or you can choose tooenter new Docker login credentials.</span></span> <span data-ttu-id="bc52e-202">Kattintson a **Befejezés** Ha megadta a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="bc52e-202">Click **Finish** when you have specified your options.</span></span>

      ![Docker állomás hitelesítő adatok megadása][PU03b]

1. <span data-ttu-id="bc52e-204">Válassza ki a Docker-állomás, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-204">Select your Docker host, and then click **Next**.</span></span>

   ![Válassza ki a hello Docker állomás toouse][PU04]

1. <span data-ttu-id="bc52e-206">Hello utolsó oldalán hello **Docker-tároló központi telepítése az Azure-on** párbeszédpanelen adja meg az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="bc52e-206">On hello last page of hello **Deploy Docker Container on Azure** dialog box, specify hello following options:</span></span>

   <span data-ttu-id="bc52e-207">a.</span><span class="sxs-lookup"><span data-stu-id="bc52e-207">a.</span></span> <span data-ttu-id="bc52e-208">Dönthet toospecify hello tárolót a Docker-tároló futtató egyéni nevet, vagy elfogadhatja az alapértelmezett hello.</span><span class="sxs-lookup"><span data-stu-id="bc52e-208">You can choose toospecify a custom name for hello container that will host your Docker container, or you can accept hello default.</span></span>

   <span data-ttu-id="bc52e-209">b.</span><span class="sxs-lookup"><span data-stu-id="bc52e-209">b.</span></span> <span data-ttu-id="bc52e-210">Adja meg a TCP-portok hello a docker állomás hello a következő szintaxis használatával: *[külső port]*:*[belső port]*.</span><span class="sxs-lookup"><span data-stu-id="bc52e-210">Enter hello TCP ports for your docker host by using hello following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="bc52e-211">Például **80:8080** egy külső port a 80-as és hello alapértelmezett belső rugó rendszerindító port a 8080-as határozza meg.</span><span class="sxs-lookup"><span data-stu-id="bc52e-211">For example, **80:8080** specifies an external port of 80 and hello default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="bc52e-212">Ha a belső port testreszabott (például a hello application.yml fájl szerkesztésével), szüksége toospecify hello portszám hello megfelelő útválasztási toooccur az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="bc52e-212">If you have customized your internal port (for example, by editing hello application.yml file), you need toospecify hello port number for hello correct routing toooccur in Azure.</span></span>

   <span data-ttu-id="bc52e-213">c.</span><span class="sxs-lookup"><span data-stu-id="bc52e-213">c.</span></span> <span data-ttu-id="bc52e-214">Ezek a beállítások konfigurálása után kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="bc52e-214">After you have configured these options, click **Finish**.</span></span>

   ![Egy Docker-tároló az Azure telepítéséhez][PU05]

1. <span data-ttu-id="bc52e-216">Amikor hello Azure eszközkészlet közzététele befejeződött, hello Azure tevékenységnapló megjeleníti **közzétett** hello állapot.</span><span class="sxs-lookup"><span data-stu-id="bc52e-216">When hello Azure Toolkit has finished publishing, hello Azure Activity Log displays **Published** for hello status.</span></span>

   ![Docker-gazdagép telepítése sikeres volt][PU06]

## <a name="next-steps"></a><span data-ttu-id="bc52e-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc52e-218">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="bc52e-219">toolearn további módszereket biztosít a rugó rendszerindító alkalmazások IntelliJ, használatával kapcsolatban lásd: [rugó rendszerindító projektek létrehozása](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) hello JetBrains webhelyen.</span><span class="sxs-lookup"><span data-stu-id="bc52e-219">toolearn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on hello JetBrains website.</span></span>

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[konfigurálása összetevők]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó keretrendszer]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
