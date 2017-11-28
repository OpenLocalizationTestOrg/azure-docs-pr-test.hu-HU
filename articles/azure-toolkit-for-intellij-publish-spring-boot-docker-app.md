---
title: "A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, az intellij-t az Azure-eszközkészlet használatával |} Microsoft Docs"
description: "Útmutató a webes alkalmazás a Microsoft Azure is közzé egy Docker-tároló az intellij-t az Azure-eszközkészlet használatával."
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
ms.openlocfilehash: b771238934183c953615ac33c42a275d80657556
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="3d61c-103">A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, az intellij-t az Azure-eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="3d61c-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="3d61c-104">A [rugó keretrendszer] egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="3d61c-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="3d61c-105">A beépített több népszerű projektek a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d61c-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="3d61c-106">[Docker] egy nyílt forráskódú megoldás, amely a fejlesztőket automatizálja a központi telepítési méretezés és a tárolók a futó alkalmazások kezelését.</span><span class="sxs-lookup"><span data-stu-id="3d61c-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="3d61c-107">Ez az oktatóanyag végigvezeti a központi telepítése egy rugó rendszerindító alkalmazásra, amely egy Docker-tároló Microsoft Azure az intellij-t az Azure-eszközkészlet használatával.</span><span class="sxs-lookup"><span data-stu-id="3d61c-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a><span data-ttu-id="3d61c-108">A alapértelmezett rugó rendszerindító Docker tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="3d61c-108">Clone the default Spring Boot Docker repo</span></span>

<span data-ttu-id="3d61c-109">A következő lépések végigvezetik a rugó rendszerindító Docker-tárház klónozása IntelliJ használatával.</span><span class="sxs-lookup"><span data-stu-id="3d61c-109">The following steps walk you through cloning the Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="3d61c-110">Ha azt szeretné, a parancssor használatára, lásd: [rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Linux központilag][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="3d61c-110">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="3d61c-111">Nyissa meg az intellij-t.</span><span class="sxs-lookup"><span data-stu-id="3d61c-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="3d61c-112">Az üdvözlőképernyőn jelölje ki a **GitHub** beállítást a **tekintse meg a verziókövetés** listája.</span><span class="sxs-lookup"><span data-stu-id="3d61c-112">On the welcome screen, select the **GitHub** option in the **Check out from Version Control** list.</span></span>

   ![GitHub Verziókövetés beállítása][CL01]

1. <span data-ttu-id="3d61c-114">Adja meg a hitelesítő adatok, ha a rendszer kéri-e jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="3d61c-114">Enter your credentials if you are prompted to log in.</span></span>

   * <span data-ttu-id="3d61c-115">Ha a felhasználónév/jelszó segítségével bejelentkezni a Githubon:</span><span class="sxs-lookup"><span data-stu-id="3d61c-115">If you are using a username/password to log in to GitHub:</span></span>

      ![A GitHub-felhasználónevét és a jelszó megadása párbeszédpanel][CL02a]

   * <span data-ttu-id="3d61c-117">Ha egy jogkivonatot használ próbál bejelentkezni a Githubon:</span><span class="sxs-lookup"><span data-stu-id="3d61c-117">If you are using a token to log in to GitHub:</span></span>

      ![A GitHub-token megadása párbeszédpanel][CL02b]

1. <span data-ttu-id="3d61c-119">Adja meg **https://github.com/spring-guides/gs-spring-boot-docker.git** a tárház URL-címet, adja meg a helyi elérési út és a mappa adatait, és kattintson **Klónozás**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for the repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![Klónozott tárház párbeszédpanel][CL03]

1. <span data-ttu-id="3d61c-121">Amikor a rendszer kéri az IntelliJ-projekt létrehozása, válassza ki a **nem**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-121">When you're prompted to create an IntelliJ project, select **No**.</span></span>

   ![Az IntelliJ-projekt létrehozása elutasítása][CL04]

1. <span data-ttu-id="3d61c-123">Az üdvözlőoldalon kattintson **projekt importálása**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-123">On the welcome page, click **Import Project**.</span></span>

   ![Importálja a projekt kiválasztása][CL05]

1. <span data-ttu-id="3d61c-125">Keresse meg az elérési utat, ahol a rugó rendszerindító tárház klónozott, válassza ki a **teljes** mappa a gyökérszintű, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-125">Locate the path where you cloned the Spring Boot repo, select the **complete** folder under the root, and then click **OK**.</span></span>

   ![Válasszon egy mappát az importálás][CL06]

1. <span data-ttu-id="3d61c-127">Amikor a rendszer kéri, válassza ki a **Create project meglévő forrásokból származó**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![Lehetőség meglévő forrásokból projekt létrehozása][CL07]

1. <span data-ttu-id="3d61c-129">Írja be a projekt nevét, vagy fogadja el az alapértelmezett, ellenőrizze a helyes elérési útját a **teljes** mappa, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-129">Specify your project name or accept the default, verify the correct path to the **complete** folder, and then click **Next**.</span></span>

   ![Adja meg a projekt neve][CL08]

1. <span data-ttu-id="3d61c-131">Az importálás könyvtárak testreszabása, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![Válassza ki a könyvtárak][CL09]

1. <span data-ttu-id="3d61c-133">Tekintse át az importáláshoz, majd kattintson a könyvtárak **következő**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-133">Review the libraries to import, and then click **Next**.</span></span>

   ![Tekintse át a projekt függvénytárak][CL10]

1. <span data-ttu-id="3d61c-135">Tekintse át a modul szerkezete, majd a **következő**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-135">Review the module structure, and then click **Next**.</span></span>

   ![Tekintse át a modul szerkezete][CL11]

1. <span data-ttu-id="3d61c-137">Adja meg a JDK, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-137">Specify your JDK, and then click **Next**.</span></span>

   ![Adja meg a JDK][CL12]

1. <span data-ttu-id="3d61c-139">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d61c-139">Click **Finish**.</span></span>

   ![A Befejezés gombra.][CL13]

<span data-ttu-id="3d61c-141">IntelliJ importálja a rugó rendszerindító alkalmazás projektként, és a struktúra jeleníti meg, ha az importálás befejeződik.</span><span class="sxs-lookup"><span data-stu-id="3d61c-141">IntelliJ imports the Spring Boot app as a project and displays the structure when the import has finished.</span></span>

![Az IntelliJ rugó rendszerindító alkalmazás][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="3d61c-143">A rugó rendszerindító build alkalmazás</span><span class="sxs-lookup"><span data-stu-id="3d61c-143">Build your Spring Boot app</span></span>

### <a name="build-the-app-by-using-the-maven-pom"></a><span data-ttu-id="3d61c-144">Az alkalmazás összeállítása a Maven POM használatával</span><span class="sxs-lookup"><span data-stu-id="3d61c-144">Build the app by using the Maven POM</span></span>

1. <span data-ttu-id="3d61c-145">Nyissa meg a Maven eszköz ablakot, ha már nincs megnyitva.</span><span class="sxs-lookup"><span data-stu-id="3d61c-145">Open the Maven tool window if it is not already opened.</span></span> <span data-ttu-id="3d61c-146">Kattintson a **nézet** > **Windows eszköz** > **Maven-projektek**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![Eszköz Windows és a Maven-projektek parancsok][BU01]

1. <span data-ttu-id="3d61c-148">A Maven eszköz ablakban kattintson a jobb gombbal **csomag** válassza **Maven Build futtatása**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-148">In the Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="3d61c-149">(Ha a Maven project nem jelenik meg automatikusan, kattintson a **újbóli importálása** a Maven eszköztár ikonjára.)</span><span class="sxs-lookup"><span data-stu-id="3d61c-149">(If your Maven project does not show up automatically, click the **Reimport** icon on the Maven toolbar.)</span></span>

   ![Maven Build parancs futtatása][BU02]

1. <span data-ttu-id="3d61c-151">Az IntelliJ megjelenjen-e egy **létrehozása sikeres** jelenik meg, ha a rugó rendszerindító alkalmazás sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="3d61c-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![BUILD üzenetet][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="3d61c-153">A telepítés kész eltérés létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d61c-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="3d61c-154">A rugó rendszerindító alkalmazás közzétételéhez létrehozásához szükséges a telepítés kész összetevő.</span><span class="sxs-lookup"><span data-stu-id="3d61c-154">To publish your Spring Boot app, you need to create a deployment-ready artifact.</span></span> <span data-ttu-id="3d61c-155">Ehhez a következő lépések szükségesek:</span><span class="sxs-lookup"><span data-stu-id="3d61c-155">Use the following steps:</span></span>

1. <span data-ttu-id="3d61c-156">Nyissa meg a webalkalmazás-projektet az intellij-t.</span><span class="sxs-lookup"><span data-stu-id="3d61c-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="3d61c-157">Kattintson a **fájl**, és kattintson a **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-157">Click **File**, and then click **Project Structure**.</span></span>

   ![Projekt struktúra parancs][ART01]

1. <span data-ttu-id="3d61c-159">Kattintson a zöld plusz (**+**) az összetevő hozzáadásához kattintson a szimbólum **JAR**, és kattintson a **üres**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-159">Click the green plus (**+**) symbol to add an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![Összetevő felvétele][ART02]

1. <span data-ttu-id="3d61c-161">Annak biztosítása, hogy nem adja hozzá a ".jar" kiterjesztést közben az összetevő neve, és adja meg a célmappában Maven kimenetének.</span><span class="sxs-lookup"><span data-stu-id="3d61c-161">Name your artifact while making sure not to add the ".jar" extension, and then specify the target folder for the Maven output.</span></span>

   ![Adja meg az összetevő tulajdonságai][ART03]

1. <span data-ttu-id="3d61c-163">Az összetevő a jegyzékfájl létrehozása (nem kötelező):</span><span class="sxs-lookup"><span data-stu-id="3d61c-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="3d61c-164">a.</span><span class="sxs-lookup"><span data-stu-id="3d61c-164">a.</span></span> <span data-ttu-id="3d61c-165">Kattintson a **jegyzékfájl létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-165">Click **Create Manifest**.</span></span>

      ![A jegyzékfájl létrehozása gombra][ART04a]

   <span data-ttu-id="3d61c-167">b.</span><span class="sxs-lookup"><span data-stu-id="3d61c-167">b.</span></span> <span data-ttu-id="3d61c-168">Válassza ki az alapértelmezett elérési összetevő, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-168">Choose the default path for the artifact, and then click **OK**.</span></span>

      ![Adja meg az összetevő elérési útja][ART04b]

   <span data-ttu-id="3d61c-170">c.</span><span class="sxs-lookup"><span data-stu-id="3d61c-170">c.</span></span> <span data-ttu-id="3d61c-171">Kattintson a három pont (**...** ) a fő osztályban található.</span><span class="sxs-lookup"><span data-stu-id="3d61c-171">Click the ellipsis (**...**) to locate the main class.</span></span>

      ![Keresse meg a fő osztály][ART04c]

   <span data-ttu-id="3d61c-173">d.</span><span class="sxs-lookup"><span data-stu-id="3d61c-173">d.</span></span> <span data-ttu-id="3d61c-174">Válassza ki a fő osztályban, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-174">Choose your main class, and then click **OK**.</span></span>

      ![Adja meg a fő osztály][ART04d]

1. <span data-ttu-id="3d61c-176">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d61c-176">Click **OK**.</span></span>

   ![A projekt struktúra párbeszédpanel bezárásához.][ART05]

> [!NOTE]
> <span data-ttu-id="3d61c-178">Az IntelliJ összetevők létrehozásával kapcsolatos további információkért lásd: [konfigurálása összetevők] a JetBrains webhelyen.</span><span class="sxs-lookup"><span data-stu-id="3d61c-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on the JetBrains website.</span></span>
>

### <a name="build-the-artifact-for-deployment"></a><span data-ttu-id="3d61c-179">A központi telepítés összetevő létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d61c-179">Build the artifact for deployment</span></span>

1. <span data-ttu-id="3d61c-180">Kattintson a **Build**, és kattintson a **összetevők**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![Az összetevők parancs létrehozása][BU04]

1. <span data-ttu-id="3d61c-182">Ha a **Build összetevő** helyi menü megjelenik, kattintson a **Build**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-182">When the **Build Artifact** context menu appears, click **Build**.</span></span>

   ![Build összetevő a helyi menü][BU05]

<span data-ttu-id="3d61c-184">A rugó rendszerindító alkalmazás befejezett összetevő IntelliJ megjelenjen-e a projekt eszköz ablakban.</span><span class="sxs-lookup"><span data-stu-id="3d61c-184">IntelliJ should display the completed artifact for your Spring Boot app in the project tool window.</span></span>

   ![Létrehozott összetevő][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="3d61c-186">A webalkalmazás közzététele az Azure-bA egy Docker-tároló használatával</span><span class="sxs-lookup"><span data-stu-id="3d61c-186">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="3d61c-187">Ha nem bejelentkezett az Azure-fiókjába, kövesse a lépéseket [bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ][Azure Sign In for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="3d61c-187">If you have not signed in to your Azure account, follow the steps in [Sign-in instructions for the Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="3d61c-188">A Project Explorer eszköz ablakban kattintson a jobb gombbal a projektre, majd válassza ki **Azure** > **Docker-tároló közzététel**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-188">In the Project Explorer tool window, right-click the project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![Docker-tároló parancsként közzététele][PU01]

1. <span data-ttu-id="3d61c-190">Ha a **Docker-tároló központi telepítése az Azure-on** párbeszédpanel jelenik meg, a meglévő Docker-gazdagépekből jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="3d61c-190">When the **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="3d61c-191">Ha egy meglévő gazdagépre telepíti, a 4. lépés kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="3d61c-191">If you choose to deploy to an existing host, you can skip to step 4.</span></span> <span data-ttu-id="3d61c-192">Ellenkező esetben a következő lépések segítségével gazdagép létrehozása:</span><span class="sxs-lookup"><span data-stu-id="3d61c-192">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="3d61c-193">a.</span><span class="sxs-lookup"><span data-stu-id="3d61c-193">a.</span></span> <span data-ttu-id="3d61c-194">Kattintson a zöld plusz (**+**) szimbólum.</span><span class="sxs-lookup"><span data-stu-id="3d61c-194">Click the green plus (**+**) symbol.</span></span>

      ![Új Docker-gazdagép hozzáadása][PU02]

   <span data-ttu-id="3d61c-196">b.</span><span class="sxs-lookup"><span data-stu-id="3d61c-196">b.</span></span> <span data-ttu-id="3d61c-197">Ha a **Docker állomás létrehozása** párbeszédpanel jelenik meg, dönthet úgy, hogy fogadja el az alapértelmezett vagy az egyéni beállításait az új Docker-állomáshoz is megadhat.</span><span class="sxs-lookup"><span data-stu-id="3d61c-197">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="3d61c-198">(Különböző beállításának részletes leírását lásd: [egy Docker-tároló egy webalkalmazást az intellij-t az Azure-eszközkészlet használatával közzététel][Publish Container with Azure Toolkit].) Kattintson a **következő** amikor megadott mely beállításokat kívánják használni.</span><span class="sxs-lookup"><span data-stu-id="3d61c-198">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Adja meg a Docker állomás beállításai][PU03a]

   <span data-ttu-id="3d61c-200">c.</span><span class="sxs-lookup"><span data-stu-id="3d61c-200">c.</span></span> <span data-ttu-id="3d61c-201">Választhat egy az Azure key vault a meglévő bejelentkezési hitelesítő adatok használatát, vagy dönthet úgy, új Docker bejelentkezési hitelesítő adatok megadásához.</span><span class="sxs-lookup"><span data-stu-id="3d61c-201">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="3d61c-202">Kattintson a **Befejezés** Ha megadta a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="3d61c-202">Click **Finish** when you have specified your options.</span></span>

      ![Docker állomás hitelesítő adatok megadása][PU03b]

1. <span data-ttu-id="3d61c-204">Válassza ki a Docker-állomás, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-204">Select your Docker host, and then click **Next**.</span></span>

   ![Válassza ki a Docker állomás használata][PU04]

1. <span data-ttu-id="3d61c-206">Az utolsó oldalán a **Docker-tároló központi telepítése az Azure-on** párbeszédpanelen adja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="3d61c-206">On the last page of the **Deploy Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="3d61c-207">a.</span><span class="sxs-lookup"><span data-stu-id="3d61c-207">a.</span></span> <span data-ttu-id="3d61c-208">Adjon meg egy egyéni nevet, a tároló, amely üzemelteti a Docker-tároló választhat, vagy elfogadhatja az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="3d61c-208">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="3d61c-209">b.</span><span class="sxs-lookup"><span data-stu-id="3d61c-209">b.</span></span> <span data-ttu-id="3d61c-210">Adja meg a TCP-portok a docker-állomás a következő szintaxissal: *[külső port]*:*[belső port]*.</span><span class="sxs-lookup"><span data-stu-id="3d61c-210">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="3d61c-211">Például **80:8080** megadja egy külső port a 80-as és az alapértelmezett belső rugó rendszerindító port a 8080-as.</span><span class="sxs-lookup"><span data-stu-id="3d61c-211">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="3d61c-212">Ha testreszabta a belső port (például a a application.yml fájl szerkesztésével), meg kell adnia a portszámot a megfelelő irányításához megtörténik az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3d61c-212">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="3d61c-213">c.</span><span class="sxs-lookup"><span data-stu-id="3d61c-213">c.</span></span> <span data-ttu-id="3d61c-214">Ezek a beállítások konfigurálása után kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="3d61c-214">After you have configured these options, click **Finish**.</span></span>

   ![Egy Docker-tároló az Azure telepítéséhez][PU05]

1. <span data-ttu-id="3d61c-216">Ha az Azure-eszközkészlet közzététele befejeződött, az Azure tevékenységnapló megjeleníti **közzétett** az állapot.</span><span class="sxs-lookup"><span data-stu-id="3d61c-216">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![Docker-gazdagép telepítése sikeres volt][PU06]

## <a name="next-steps"></a><span data-ttu-id="3d61c-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d61c-218">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="3d61c-219">A rugó rendszerindító alkalmazások létrehozása az IntelliJ segítségével további módszerekkel kapcsolatos további tudnivalókért lásd: [rugó rendszerindító projektek létrehozása](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) a JetBrains webhelyen.</span><span class="sxs-lookup"><span data-stu-id="3d61c-219">To learn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on the JetBrains website.</span></span>

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
<span data-ttu-id="3d61c-220">[konfigurálása összetevők]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span><span class="sxs-lookup"><span data-stu-id="3d61c-220">[Configuring Artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span></span>
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
<span data-ttu-id="3d61c-221">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="3d61c-221">[Docker]: https://www.docker.com/</span></span>
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
<span data-ttu-id="3d61c-222">[rugó rendszerindító]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="3d61c-222">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="3d61c-223">[rugó keretrendszer]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="3d61c-223">[Spring Framework]: https://spring.io/</span></span>

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
