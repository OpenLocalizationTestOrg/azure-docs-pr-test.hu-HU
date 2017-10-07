---
title: "egy alapszintű Azure-webalkalmazásban az IntelliJ aaaCreate |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan toouse hello Azure eszközkészlet IntelliJ toocreate egy Hello World az Azure-webalkalmazást."
services: app-service\web
documentationcenter: java
author: selvasingh
manager: erikre
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4667497213cac3ddf754d164e614c809f338cce5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a><span data-ttu-id="df61f-103">Egy alapszintű Azure-webalkalmazás létrehozása az intellij-t</span><span class="sxs-lookup"><span data-stu-id="df61f-103">Create a basic Azure web app in IntelliJ</span></span>
<span data-ttu-id="df61f-104">Ez az oktatóanyag bemutatja, hogyan toocreate és központi telepítése egy alapszintű Hello World alkalmazás tooAzure webalkalmazásként hello segítségével [IntelliJ Azure eszköztára].</span><span class="sxs-lookup"><span data-stu-id="df61f-104">This tutorial shows how toocreate and deploy a basic Hello World application tooAzure as a Web App by using hello [Azure Toolkit for IntelliJ].</span></span> <span data-ttu-id="df61f-105">Egy alapszintű JSP példa az egyszerűség, de hasonló lépésekkel akkor lehet megfelelő, egy Java servlet, az Azure-telepítés illetően.</span><span class="sxs-lookup"><span data-stu-id="df61f-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="df61f-106">Ez az oktatóanyag befejezése után, az alkalmazás a következő ábrán egy webböngészőben megtekintésekor hasonló toohello fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="df61f-106">When you have completed this tutorial, your application will look similar toohello following illustration when you view it in a web browser:</span></span>

![A minta-weblap][01]

## <a name="prerequisites"></a><span data-ttu-id="df61f-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="df61f-108">Prerequisites</span></span>
* <span data-ttu-id="df61f-109">A Java fejlesztői készlet (JDK), v 1,8 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="df61f-109">A Java Developer Kit (JDK), v 1.8 or later.</span></span>
* <span data-ttu-id="df61f-110">Az IntelliJ IDEA Ultimate Edition.</span><span class="sxs-lookup"><span data-stu-id="df61f-110">IntelliJ IDEA Ultimate Edition.</span></span> <span data-ttu-id="df61f-111">Ez letölthető <https://www.jetbrains.com/idea/download/index.html>.</span><span class="sxs-lookup"><span data-stu-id="df61f-111">This can be downloaded from <https://www.jetbrains.com/idea/download/index.html>.</span></span>
* <span data-ttu-id="df61f-112">A Java-alapú webkiszolgáló vagy a kiszolgáló, terjesztési például [Apache Tomcat] vagy [Jetty].</span><span class="sxs-lookup"><span data-stu-id="df61f-112">A distribution of a Java-based web server or application server, such as [Apache Tomcat] or [Jetty].</span></span>
* <span data-ttu-id="df61f-113">Azure-előfizetéssel, amely szerezhető: a <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>.</span><span class="sxs-lookup"><span data-stu-id="df61f-113">An Azure subscription, which can be acquired from <https://azure.microsoft.com/free/> or <http://azure.microsoft.com/pricing/purchase-options/>.</span></span>
* <span data-ttu-id="df61f-114">Hello [IntelliJ Azure eszköztára].</span><span class="sxs-lookup"><span data-stu-id="df61f-114">hello [Azure Toolkit for IntelliJ].</span></span> <span data-ttu-id="df61f-115">Hello Azure eszközkészlet telepítésével kapcsolatos információkért lásd: [telepítése hello Azure eszköztára IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="df61f-115">For information about installing hello Azure Toolkit, see [Installing hello Azure Toolkit for IntelliJ].</span></span>

## <a name="toocreate-a-hello-world-application"></a><span data-ttu-id="df61f-116">toocreate egy Hello World alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="df61f-116">toocreate a Hello World application</span></span>
<span data-ttu-id="df61f-117">Először először foglalkozunk létrehoz egy Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="df61f-117">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="df61f-118">Indítsa el az intellij-t, majd kattintson a hello **fájl** menü, majd kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="df61f-118">Start IntelliJ and click hello **File** menu, then click **New**, and then click **Project**.</span></span>
   
    ![Fájl új projekt][02]
2. <span data-ttu-id="df61f-120">Hello új projekt párbeszédpanel, válassza ki a **Java**, majd **webalkalmazás**, és kattintson a **új** tooadd egy projekt SDK.</span><span class="sxs-lookup"><span data-stu-id="df61f-120">In hello New Project dialog box, select **Java**, then **Web Application**, and then click **New** tooadd a Project SDK.</span></span>
   
    ![Új projekt párbeszédpanel][03a]
   
3. <span data-ttu-id="df61f-122">A Kezdőkönyvtár kiválasztása hello JDK párbeszédpanel megnyitásához, válassza ki hello a JDK telepítési mappáját, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="df61f-122">In hello Select Home Directory for JDK dialog box, select hello folder where your JDK is installed, and then click **OK**.</span></span> <span data-ttu-id="df61f-123">Kattintson a **tovább** a hello új projekt párbeszédpanel bezárásához toocontinue.</span><span class="sxs-lookup"><span data-stu-id="df61f-123">Click **Next** in hello New Project dialog box toocontinue.</span></span>
   
    ![Adja meg a JDK kezdőkönyvtára][03b]
4. <span data-ttu-id="df61f-125">Ez az oktatóanyag céljából, nevet hello projektnek **Java-webalkalmazás-alkalmazás-a-Azure**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="df61f-125">For purposes of this tutorial, name hello project **Java-Web-App-On-Azure**, and then click **Finish**.</span></span>
   
    ![Új projekt párbeszédpanel][04]
5. <span data-ttu-id="df61f-127">Az IntelliJ a Project Explorer nézet, bontsa ki a **Java-webalkalmazás-alkalmazás-a-Azure**, majd bontsa ki a **webes**, majd kattintson duplán **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="df61f-127">Within IntelliJ's Project Explorer view, expand **Java-Web-App-On-Azure**, then expand **web**, and then double-click **index.jsp**.</span></span>
   
    ![Nyissa meg az Index lap][05c]
6. <span data-ttu-id="df61f-129">Az index.jsp fájl megnyitásakor az IntelliJ beépülő modul toodynamically megjelenített szöveg **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="df61f-129">When your index.jsp file opens in IntelliJ, add in text toodynamically display **Hello World!**</span></span> <span data-ttu-id="df61f-130">hello meglévő belül `<body>` elemet.</span><span class="sxs-lookup"><span data-stu-id="df61f-130">within hello existing `<body>` element.</span></span> <span data-ttu-id="df61f-131">A frissített `<body>` tartalom a következő példa hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="df61f-131">Your updated `<body>` content should resemble hello following example:</span></span>
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
7. <span data-ttu-id="df61f-132">Mentse az index.jsp.</span><span class="sxs-lookup"><span data-stu-id="df61f-132">Save index.jsp.</span></span>

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a><span data-ttu-id="df61f-133">toodeploy az alkalmazás tooan Azure Web App tároló</span><span class="sxs-lookup"><span data-stu-id="df61f-133">toodeploy your application tooan Azure Web App Container</span></span>
<span data-ttu-id="df61f-134">Számos módon egy Java webes alkalmazás tooAzure-telepíthet.</span><span class="sxs-lookup"><span data-stu-id="df61f-134">There are several ways by which you can deploy a Java web application tooAzure.</span></span> <span data-ttu-id="df61f-135">Ez az oktatóanyag leírja az egyik legegyszerűbb hello: az alkalmazás lesz telepített tooan Azure Web App tároló - semmilyen különleges projekttípus sem további eszközök van szükség.</span><span class="sxs-lookup"><span data-stu-id="df61f-135">This tutorial describes one of hello simplest: your application will be deployed tooan Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="df61f-136">hello JDK és hello webes tároló szoftver biztosítja az Ön Azure-ra, így nincs szükség tooupload a saját; szüksége a Java-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="df61f-136">hello JDK and hello web container software will be provided for you by Azure, so there is no need tooupload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="df61f-137">Ennek eredményeképpen hello közzétételi alkalmazás vesz igénybe másodperc, nem perc.</span><span class="sxs-lookup"><span data-stu-id="df61f-137">As a result, hello publishing process for your application will take seconds, not minutes.</span></span>

<span data-ttu-id="df61f-138">Az alkalmazás közzététele előtt először tooconfigure a modul beállításait.</span><span class="sxs-lookup"><span data-stu-id="df61f-138">Before you publish your application, you first need tooconfigure your module settings.</span></span> <span data-ttu-id="df61f-139">toodo Igen, használja a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="df61f-139">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="df61f-140">Az intellij-t a Project Explorer, kattintson a jobb gombbal hello **Java-webalkalmazás-alkalmazás-a-Azure** projekt.</span><span class="sxs-lookup"><span data-stu-id="df61f-140">In IntelliJ's Project Explorer, right-click hello **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="df61f-141">Amikor hello helyi menü megjelenik, kattintson a **beállítások modul megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="df61f-141">When hello context menu appears, click **Open Module Settings**.</span></span>

    ![Nyissa meg a modul beállításai][05a]
2. <span data-ttu-id="df61f-143">Hello szerkezetének párbeszédpanel megjelenésekor:</span><span class="sxs-lookup"><span data-stu-id="df61f-143">When hello Project Structure dialog box appears:</span></span>

   <span data-ttu-id="df61f-144">a.</span><span class="sxs-lookup"><span data-stu-id="df61f-144">a.</span></span> <span data-ttu-id="df61f-145">Kattintson a **összetevők** hello listájában **Projektbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="df61f-145">Click **Artifacts** in hello list of **Project Settings**.</span></span>
   <span data-ttu-id="df61f-146">b.</span><span class="sxs-lookup"><span data-stu-id="df61f-146">b.</span></span> <span data-ttu-id="df61f-147">Módosítás hello összetevő neve, a hello **neve** jelölőnégyzetet, hogy nem tartalmaz szóközt vagy speciális karaktereket; Ez azért szükséges, mivel az egységes erőforrás-azonosító (URI) hello hello neve lesz.</span><span class="sxs-lookup"><span data-stu-id="df61f-147">Change hello artifact name in hello **Name** box so that it doesn't contain whitespace or special characters; this is necessary since hello name will be used in hello Uniform Resource Identifier (URI).</span></span>
   <span data-ttu-id="df61f-148">c.</span><span class="sxs-lookup"><span data-stu-id="df61f-148">c.</span></span> <span data-ttu-id="df61f-149">Változás hello **típus** túl**webalkalmazás: archív**.</span><span class="sxs-lookup"><span data-stu-id="df61f-149">Change hello **Type** too**Web Application: Archive**.</span></span>
   <span data-ttu-id="df61f-150">d.</span><span class="sxs-lookup"><span data-stu-id="df61f-150">d.</span></span> <span data-ttu-id="df61f-151">Kattintson a **OK** tooclose hello szerkezetének párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="df61f-151">Click **OK** tooclose hello Project Structure dialog box.</span></span>

    ![Nyissa meg a modul beállításai][05b]

<span data-ttu-id="df61f-153">Ha konfigurálta a modul beállításait, közzéteheti a alkalmazás tooAzure hello lépések segítségével:</span><span class="sxs-lookup"><span data-stu-id="df61f-153">When you have configured your module settings, you can publish your application tooAzure by using hello following steps:</span></span>

1. <span data-ttu-id="df61f-154">Az intellij-t a Project Explorer, kattintson a jobb gombbal hello **Java-webalkalmazás-alkalmazás-a-Azure** projekt.</span><span class="sxs-lookup"><span data-stu-id="df61f-154">In IntelliJ's Project Explorer, right-click hello **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="df61f-155">Amikor hello helyi menü megjelenik, válassza ki a **Azure**, és kattintson a **Azure Web Apps közzététel...**</span><span class="sxs-lookup"><span data-stu-id="df61f-155">When hello context menu appears, select **Azure**, and then click **Publish as Azure Web App...**</span></span>
   
    ![Azure közzététel helyi menü][06]
2. <span data-ttu-id="df61f-157">Ha már nem regisztrált az Azure az intellij-t, rákérdezéses toosign fogja be Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="df61f-157">If you have not already signed into Azure from IntelliJ, you will be prompted toosign into your Azure account.</span></span> <span data-ttu-id="df61f-158">(Még több Azure-fiókra, néhány hello kér hello bejelentkezési folyamat során előfordulhat, hogy megjelenik-e egynél többször, még akkor is, ha úgy tűnik, mintha toobe hello azonos.</span><span class="sxs-lookup"><span data-stu-id="df61f-158">(If you have multiple Azure accounts, some of hello prompts during hello sign in process may be shown more than once, even if they appear toobe hello same.</span></span> <span data-ttu-id="df61f-159">Ha ez történik, továbbra is toofollow hello bejelentkezési utasításokban.)</span><span class="sxs-lookup"><span data-stu-id="df61f-159">When this happens, continue toofollow hello sign in instructions.)</span></span>
   
    ![Az Azure bejelentkezési párbeszédpanel][07]
3. <span data-ttu-id="df61f-161">Miután sikeresen bejelentkezett az Azure-fiókjával a, hello **előfizetések kezelése oldalt** párbeszédpanelen jelenik meg a hitelesítő adatok társított előfizetések listáját.</span><span class="sxs-lookup"><span data-stu-id="df61f-161">After you have successfully signed into your Azure account, hello **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="df61f-162">(Ha szerepel a listában több előfizetése van, és azt szeretné, hogy csak egy adott részével őket toowork, akkor előfordulhat, hogy nem kötelező, törölje a jelet hello előfizetések nem szeretné, hogy toouse.) Miután kiválasztotta az előfizetések, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="df61f-162">(If there are multiple subscriptions listed and you want toowork with only a specific subset of them, you may optionally uncheck hello subscriptions you don't want toouse.) When you have selected your subscriptions, click **Close**.</span></span>
   
    ![Előfizetések kezelése][08]
4. <span data-ttu-id="df61f-164">Ha hello **központi telepítése a webes alkalmazás tároló tooAzure** párbeszédpanel jelenik meg, jeleníti meg, hogy a korábban létrehozott webalkalmazás tárolókkal; Ha nem hozott létre tárolót, hello listája üres lesz.</span><span class="sxs-lookup"><span data-stu-id="df61f-164">When hello **Deploy tooAzure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, hello list will be empty.</span></span>
   
    ![Alkalmazás tárolók][09]
5. <span data-ttu-id="df61f-166">Ha még nem hozott egy Azure webes alkalmazás tároló előtt, vagy ha azt szeretné, hogy toopublish az alkalmazás tooa új tároló, használja a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="df61f-166">If you have not created an Azure Web App Container before, or if you would like toopublish your application tooa new container, use hello following steps.</span></span> <span data-ttu-id="df61f-167">Ellenkező esetben válassza ki a meglévő Web App tároló, és hagyja ki az alábbi 6 toostep.</span><span class="sxs-lookup"><span data-stu-id="df61f-167">Otherwise, select an existing Web App Container and skip toostep 6 below.</span></span>
   
   1. <span data-ttu-id="df61f-168">Kattintson a**+**</span><span class="sxs-lookup"><span data-stu-id="df61f-168">Click **+**</span></span>
      
       ![Alkalmazás-tároló hozzáadása][10]
   2. <span data-ttu-id="df61f-170">Hello **új webes alkalmazás tároló** párbeszédpanel jelenik meg, amely hello használt mellett számos lépést.</span><span class="sxs-lookup"><span data-stu-id="df61f-170">hello **New Web App Container** dialog box will be displayed, which will be used for hello next several steps.</span></span>
      
       ![Új alkalmazás-tároló][11a]
   3. <span data-ttu-id="df61f-172">Adjon meg egy **DNS-címke** a webes alkalmazás tároló; ez alkotó hello levél DNS-címke hello gazdagépre URL-címet a webalkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="df61f-172">Enter a **DNS Label** for your Web App Container; this will form hello leaf DNS label of hello host URL for your web application in Azure.</span></span> <span data-ttu-id="df61f-173">Megjegyzés: hello neve kell érhető el, és felel meg a tooAzure webalkalmazás-kiosztási követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="df61f-173">Note that hello name must be available and conform tooAzure Web App naming requirements.</span></span>
   4. <span data-ttu-id="df61f-174">A hello **webes tároló** legördülő menüre, válassza hello megfelelő szoftvert az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="df61f-174">In hello **Web Container** drop-down menu, select hello appropriate software for your application.</span></span>
      
       <span data-ttu-id="df61f-175">Jelenleg Tomcat 8 Tomcat 7 vagy Jetty 9 közül választhat.</span><span class="sxs-lookup"><span data-stu-id="df61f-175">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="df61f-176">Kijelölt hello szoftver legújabb terjesztési Azure biztosítja, és akkor fog futni, a legutóbbi terjesztésipont JDK 8 Oracle által létrehozott és az Azure által biztosított.</span><span class="sxs-lookup"><span data-stu-id="df61f-176">A recent distribution of hello selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>
   5. <span data-ttu-id="df61f-177">A hello **előfizetés** legördülő menüre, válassza hello előfizetés toouse ehhez a központi telepítéshez használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="df61f-177">In hello **Subscription** drop-down menu, select hello subscription you want toouse for this deployment.</span></span>
   6. <span data-ttu-id="df61f-178">A hello **erőforráscsoport** legördülő menüben válassza hello erőforráscsoportot, amelyhez tooassociate a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="df61f-178">In hello **Resource Group** drop-down menu, select hello Resource Group with which you want tooassociate your Web App.</span></span> <span data-ttu-id="df61f-179">(Az azure erőforráscsoport-sablonok engedélyezi, akkor toogroup kapcsolódó erőforrások együtt, így például, hogy törölheti együtt.)</span><span class="sxs-lookup"><span data-stu-id="df61f-179">(Azure Resource Groups allow you toogroup related resources together so that, for example, they can be deleted together.)</span></span>
      
       <span data-ttu-id="df61f-180">Egy meglévő erőforráscsoportot kiválaszthatja (ha vannak ilyenek), és a skip toostep g az alábbi, vagy használjon hello következő lépések toocreate új erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="df61f-180">You can select an existing Resource Group (if you have any) and skip toostep g below, or use hello following steps toocreate a new Resource Group:</span></span>
      
      * <span data-ttu-id="df61f-181">Válassza ki  **&lt; &lt; hozzon létre új erőforráscsoportot &gt; &gt;**  a hello **erőforráscsoport** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="df61f-181">Select **&lt;&lt; Create new Resource Group &gt;&gt;** in hello **Resource Group** drop-down menu.</span></span>
      * <span data-ttu-id="df61f-182">Hello **új erőforráscsoport** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="df61f-182">hello **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Új erőforráscsoport][12]
      * <span data-ttu-id="df61f-184">A hello hello **neve** szövegmező, adja meg az új erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="df61f-184">In hello hello **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="df61f-185">A hello hello **régió** legördülő menüben válassza hello megfelelő Azure adatközpont-erőforrásnak a helyét.</span><span class="sxs-lookup"><span data-stu-id="df61f-185">In hello hello **Region** drop-down menu, select hello appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="df61f-186">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="df61f-186">Click **OK**.</span></span>
   7. <span data-ttu-id="df61f-187">Hello **App Service-csomag** legördülő menü hello app service-csomagokról hello erőforráscsoport kiválasztott társított sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="df61f-187">hello **App Service Plan** drop-down menu lists hello app service plans that are associated with hello Resource Group that you selected.</span></span> <span data-ttu-id="df61f-188">(A következő információkat, például a Web App alkalmazásban tarifacsomag és hello számítási példányméretének hello hello helyét egy App Service-csomag határozza meg.</span><span class="sxs-lookup"><span data-stu-id="df61f-188">(An App Service Plan specifies information such as hello location of your Web App, hello pricing tier and hello compute instance size.</span></span> <span data-ttu-id="df61f-189">Egy egyetlen App Service-csomag használható több Web Apps, ezért azt egy adott webalkalmazás telepítésből külön megmarad.)</span><span class="sxs-lookup"><span data-stu-id="df61f-189">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       <span data-ttu-id="df61f-190">Kiválaszthat egy meglévő App Service-csomag (ha vannak ilyenek), és hagyja ki az alábbi toostep h, vagy használja a következő lépéseket toocreate egy új App Service-csomag hello:</span><span class="sxs-lookup"><span data-stu-id="df61f-190">You can select an existing App Service Plan (if you have any) and skip toostep h below, or use hello following steps toocreate a new App Service Plan:</span></span>
      
      * <span data-ttu-id="df61f-191">Válassza ki  **&lt; &lt; új App Service-csomag létrehozása &gt; &gt;**  a hello **App Service-csomag** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="df61f-191">Select **&lt;&lt; Create new App Service Plan &gt;&gt;** in hello **App Service Plan** drop-down menu.</span></span>
      * <span data-ttu-id="df61f-192">Hello **új App Service-csomag** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="df61f-192">hello **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Új App Service-csomag][13]
      * <span data-ttu-id="df61f-194">A hello hello **neve** szövegmező, adjon meg egy nevet az új App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="df61f-194">In hello hello **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="df61f-195">A hello hello **hely** legördülő menüben válassza hello megfelelő Azure adatközpont hello terv helyét.</span><span class="sxs-lookup"><span data-stu-id="df61f-195">In hello hello **Location** drop-down menu, select hello appropriate Azure data center location for hello plan.</span></span>
      * <span data-ttu-id="df61f-196">A hello hello **Tarifacsomagot** legördülő menüben válassza hello megfelelő hello terv árképzési.</span><span class="sxs-lookup"><span data-stu-id="df61f-196">In hello hello **Pricing Tier** drop-down menu, select hello appropriate pricing for hello plan.</span></span> <span data-ttu-id="df61f-197">Tesztelési célokra választhat **szabad**.</span><span class="sxs-lookup"><span data-stu-id="df61f-197">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="df61f-198">A hello hello **Példányméretének** legördülő menüre, válassza hello megfelelő példányméretének hello terv.</span><span class="sxs-lookup"><span data-stu-id="df61f-198">In hello hello **Instance Size** drop-down menu, select hello appropriate instance size for hello plan.</span></span> <span data-ttu-id="df61f-199">Tesztelési célokra választhat **kis**.</span><span class="sxs-lookup"><span data-stu-id="df61f-199">For testing purposes you can choose **Small**.</span></span>
      * <span data-ttu-id="df61f-200">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="df61f-200">Click **OK**.</span></span>
   8. <span data-ttu-id="df61f-201">(Választható) Alapértelmezés szerint 8 Java legutóbbi terjesztési automatikusan telepíti a JVM-et, az Azure tooyour webes alkalmazás tároló.</span><span class="sxs-lookup"><span data-stu-id="df61f-201">(Optional) By default, a recent distribution of Java 8 will be automatically deployed as your JVM by Azure tooyour web app container.</span></span> <span data-ttu-id="df61f-202">Választhatja azonban egy másik verzió és elosztását illetően hello JVM-et.</span><span class="sxs-lookup"><span data-stu-id="df61f-202">However, you can select a different version and distribution of hello JVM.</span></span> <span data-ttu-id="df61f-203">toodo Igen, használja a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="df61f-203">toodo so, use hello following steps:</span></span>
      
      * <span data-ttu-id="df61f-204">Kattintson a hello **JDK** hello lapján **új webes alkalmazás tároló** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="df61f-204">Click hello **JDK** tab in hello **New Web App Container** dialog box.</span></span>
      * <span data-ttu-id="df61f-205">Hello alábbi beállítások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="df61f-205">You can choose from one of hello following options:</span></span>
        
        * <span data-ttu-id="df61f-206">Hello alapértelmezett JDK, amely az Azure által kínált telepítése</span><span class="sxs-lookup"><span data-stu-id="df61f-206">Deploy hello default JDK which is offered by Azure</span></span>
        * <span data-ttu-id="df61f-207">3. fél JDK Azure rendelkezésre álló további JDKs legördülő listájából telepítése</span><span class="sxs-lookup"><span data-stu-id="df61f-207">Deploy a 3rd party JDK from a drop-down list of additional JDKs which are available on Azure</span></span>
        * <span data-ttu-id="df61f-208">Központi telepítése egy egyéni JDK, és be kell csomagolni, és vagy egy ZIP-fájl nyilvánosan elérhető vagy az Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="df61f-208">Deploy a custom JDK, which must be packaged as a ZIP file and either publicly available or in your Azure storage account</span></span>
        
        ![Új alkalmazás tároló JDK lapon][11b]
   9. <span data-ttu-id="df61f-210">Az összes fenti lépéseket hello befejeződött, hello új webes alkalmazás tároló párbeszédpanelen a következő ábra hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="df61f-210">Once you have completed all of hello above steps, hello New Web App Container dialog box should resemble hello following illustration:</span></span>
      
       ![Új alkalmazás-tároló][14]
   10. <span data-ttu-id="df61f-212">Kattintson a **OK** toocomplete hello létrehozása az új webalkalmazáshoz tároló.</span><span class="sxs-lookup"><span data-stu-id="df61f-212">Click **OK** toocomplete hello creation of your new Web App container.</span></span>
       
        <span data-ttu-id="df61f-213">Várjon néhány másodpercet hello webalkalmazás tárolók toobe hello listája frissítve, és az újonnan létrehozott webes alkalmazás tároló kijelöli hello listában.</span><span class="sxs-lookup"><span data-stu-id="df61f-213">Wait a few seconds for hello list of hello Web App containers toobe refreshed, and your newly-created web app container should now be selected in hello list.</span></span>
6. <span data-ttu-id="df61f-214">Most már áll készen toocomplete hello kezdeti telepítése a webes alkalmazás tooAzure; Kattintson a **OK** toodeploy a Java-alkalmazás toohello kijelölt webalkalmazás tároló.</span><span class="sxs-lookup"><span data-stu-id="df61f-214">You are now ready toocomplete hello initial deployment of your Web App tooAzure; click **OK** toodeploy your Java application toohello selected Web App container.</span></span> <span data-ttu-id="df61f-215">Alapértelmezés szerint az alkalmazás telepítése, hello alkalmazáskiszolgáló alkönyvtára.</span><span class="sxs-lookup"><span data-stu-id="df61f-215">By default, your application will be deployed as a subdirectory of hello application server.</span></span> <span data-ttu-id="df61f-216">Ha azt szeretné, központilag telepített hello gyökéralkalmazás toobe, ellenőrizze a hello **tooroot telepítése** való kattintás előtt jelölőnégyzet **OK**.</span><span class="sxs-lookup"><span data-stu-id="df61f-216">If you want it toobe deployed as hello root application, check hello **Deploy tooroot** checkbox before clicking **OK**.</span></span>
   
    ![TooAzure telepítése][15]
7. <span data-ttu-id="df61f-218">Ezt követően megtekintheti az hello **Azure tevékenységnapló** nézetet, amely a webalkalmazás hello központi telepítésének állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="df61f-218">Next, you should see hello **Azure Activity Log** view, which will indicate hello deployment status of your Web App.</span></span>
   
    ![Folyamatjelző][16]
   
    <span data-ttu-id="df61f-220">a webalkalmazás tooAzure telepítési folyamata hello gyorsabban csupán néhány másodpercet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="df61f-220">hello process of deploying your Web App tooAzure should take only a few seconds toocomplete.</span></span> <span data-ttu-id="df61f-221">Ha alkalmazás készen áll, megjelenik egy hivatkozás nevű **közzétett** a hello **állapot** oszlop.</span><span class="sxs-lookup"><span data-stu-id="df61f-221">When your application ready, you will see a link named **Published** in hello **Status** column.</span></span> <span data-ttu-id="df61f-222">Hello hivatkozásra telepített tooyour Web App kezdőlapján fog tartani, vagy a következő szakasz toobrowse tooyour webalkalmazás hello hello lépéseket használhatja.</span><span class="sxs-lookup"><span data-stu-id="df61f-222">When you click hello link, it will take you tooyour deployed Web App's home page, or you can use hello steps in hello following section toobrowse tooyour web app.</span></span>

## <a name="browsing-tooyour-web-app-on-azure"></a><span data-ttu-id="df61f-223">Az Azure Web App tooyour tallózása</span><span class="sxs-lookup"><span data-stu-id="df61f-223">Browsing tooyour Web App on Azure</span></span>
<span data-ttu-id="df61f-224">toobrowse tooyour Azure Web App, hello használható **Azure Explorer** nézet.</span><span class="sxs-lookup"><span data-stu-id="df61f-224">toobrowse tooyour Web App on Azure, you can use hello **Azure Explorer** view.</span></span>

<span data-ttu-id="df61f-225">Ha hello **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **nézet** menü az intellij-t, majd kattintson a **eszköz Windows**, és kattintson a  **Szolgáltatás Explorer**.</span><span class="sxs-lookup"><span data-stu-id="df61f-225">If hello **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="df61f-226">Ha korábban nem jelentkezett be, amely felszólítja toodo stb.</span><span class="sxs-lookup"><span data-stu-id="df61f-226">If you have not previously logged in, it will prompt you toodo so.</span></span>

<span data-ttu-id="df61f-227">Ha hello **Azure Explorer** nézet jelenik meg, használja, hajtsa végre a lépéseket toobrowse tooyour Web App:</span><span class="sxs-lookup"><span data-stu-id="df61f-227">When hello **Azure Explorer** view is displayed, use follow these steps toobrowse tooyour Web App:</span></span> 

1. <span data-ttu-id="df61f-228">Bontsa ki a hello **Azure** csomópont.</span><span class="sxs-lookup"><span data-stu-id="df61f-228">Expand hello **Azure** node.</span></span>
2. <span data-ttu-id="df61f-229">Bontsa ki a hello **webalkalmazások** csomópont.</span><span class="sxs-lookup"><span data-stu-id="df61f-229">Expand hello **Web Apps** node.</span></span> 
3. <span data-ttu-id="df61f-230">Kattintson a jobb gombbal hello kívánt webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="df61f-230">Right-click hello desired Web App.</span></span>
4. <span data-ttu-id="df61f-231">Amikor hello helyi menü megjelenik, kattintson a **nyissa meg böngészőben**.</span><span class="sxs-lookup"><span data-stu-id="df61f-231">When hello context menu appears, click **Open in Browser**.</span></span>
   
    ![Keresse meg a webes alkalmazás][17]

## <a name="updating-your-web-app"></a><span data-ttu-id="df61f-233">Webalkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="df61f-233">Updating your web app</span></span>
<span data-ttu-id="df61f-234">Frissítése egy meglévő Azure-webalkalmazás fut egy gyors és egyszerű eljárás, és frissítési lehetőségei vannak:</span><span class="sxs-lookup"><span data-stu-id="df61f-234">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="df61f-235">Java-webalkalmazás telepítése hello frissítheti.</span><span class="sxs-lookup"><span data-stu-id="df61f-235">You can update hello deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="df61f-236">Egy további Java-alkalmazás toohello közzététele Web App tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="df61f-236">You can publish an additional Java application toohello same Web App Container.</span></span>

<span data-ttu-id="df61f-237">Mindkét esetben hello folyamata azonos, és csupán néhány másodpercet vesz igénybe:</span><span class="sxs-lookup"><span data-stu-id="df61f-237">In either case, hello process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="df61f-238">Hello IntelliJ project Explorer kattintson a jobb gombbal hello Java-alkalmazás hozzáadása a webes alkalmazás tároló meglévő tooan vagy tooupdate szeretné.</span><span class="sxs-lookup"><span data-stu-id="df61f-238">In hello IntelliJ project explorer, right-click hello Java application you want tooupdate or add tooan existing Web App Container.</span></span>
2. <span data-ttu-id="df61f-239">Amikor hello helyi menü megjelenik, válassza ki a **Azure** , majd **Azure Web Apps közzététel...**</span><span class="sxs-lookup"><span data-stu-id="df61f-239">When hello context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="df61f-240">Már már bejelentkezett korábban, mivel látni fogja a meglévő Web App tárolók listája.</span><span class="sxs-lookup"><span data-stu-id="df61f-240">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="df61f-241">Válasszon egyet a Java-alkalmazás tooand kattintson újra közzé vagy toopublish kívánt hello **OK**.</span><span class="sxs-lookup"><span data-stu-id="df61f-241">Select hello one you want toopublish or re-publish your Java application tooand click **OK**.</span></span>

<span data-ttu-id="df61f-242">Néhány másodperc múlva, hello **Azure tevékenységnapló** nézetben megjelenik, a frissített telepítési **közzétett** , és a frissített alkalmazást egy webböngészőben lesz képes tooverify.</span><span class="sxs-lookup"><span data-stu-id="df61f-242">A few seconds later, hello **Azure Activity Log** view will show your updated deployment as **Published** and you will be able tooverify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="df61f-243">Indítása, leállítása vagy egy már meglévő webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="df61f-243">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="df61f-244">toostart vagy állítsa le a meglévő Azure Web Apps tároló, (beleértve az összes telepített hello Java-alkalmazások azt), használhatja a hello **Azure Explorer** nézet.</span><span class="sxs-lookup"><span data-stu-id="df61f-244">toostart or stop an existing Azure Web App container, (including all hello deployed Java applications in it), you can use hello **Azure Explorer** view.</span></span>

<span data-ttu-id="df61f-245">Ha hello **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **nézet** menü az intellij-t, majd kattintson a **eszköz Windows**, és kattintson a  **Szolgáltatás Explorer**.</span><span class="sxs-lookup"><span data-stu-id="df61f-245">If hello **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="df61f-246">Ha korábban nem jelentkezett be, amely felszólítja toodo stb.</span><span class="sxs-lookup"><span data-stu-id="df61f-246">If you have not previously logged in, it will prompt you toodo so.</span></span>

<span data-ttu-id="df61f-247">Ha hello **Azure Explorer** nézet jelenik meg, kövesse az alábbi lépéseket toostart használatát, vagy a webalkalmazás leállítása:</span><span class="sxs-lookup"><span data-stu-id="df61f-247">When hello **Azure Explorer** view is displayed, use follow these steps toostart or stop your Web App:</span></span> 

1. <span data-ttu-id="df61f-248">Bontsa ki a hello **Azure** csomópont.</span><span class="sxs-lookup"><span data-stu-id="df61f-248">Expand hello **Azure** node.</span></span>
2. <span data-ttu-id="df61f-249">Bontsa ki a hello **webalkalmazások** csomópont.</span><span class="sxs-lookup"><span data-stu-id="df61f-249">Expand hello **Web Apps** node.</span></span> 
3. <span data-ttu-id="df61f-250">Kattintson a jobb gombbal hello kívánt webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="df61f-250">Right-click hello desired Web App.</span></span>
4. <span data-ttu-id="df61f-251">Amikor hello helyi menü megjelenik, kattintson a **Start**, **leállítása**, vagy **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="df61f-251">When hello context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="df61f-252">Ne feledje, hogy hello menü döntések környezet használatát, így csak a futó webalkalmazás leállítása vagy egy webalkalmazást, ami jelenleg nem fut. Indítsa el.</span><span class="sxs-lookup"><span data-stu-id="df61f-252">Note that hello menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
    ![A webalkalmazás leállítása][18]

## <a name="next-steps"></a><span data-ttu-id="df61f-254">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df61f-254">Next Steps</span></span>
<span data-ttu-id="df61f-255">A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="df61f-255">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="df61f-256">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="df61f-256">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="df61f-257">[Hello Azure eszköztára Eclipse telepítése]</span><span class="sxs-lookup"><span data-stu-id="df61f-257">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="df61f-258">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="df61f-258">[Create a Hello World Web App for Azure in Eclipse]</span></span>
  * <span data-ttu-id="df61f-259">[Újdonságok az Eclipse Azure eszköztára hello]</span><span class="sxs-lookup"><span data-stu-id="df61f-259">[What's New in hello Azure Toolkit for Eclipse]</span></span>
* <span data-ttu-id="df61f-260">[IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="df61f-260">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="df61f-261">[telepítése hello Azure eszköztára IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="df61f-261">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="df61f-262">*Hello World webalkalmazás létrehozása az Azure-hoz az intellij-t (Ez a cikk)*</span><span class="sxs-lookup"><span data-stu-id="df61f-262">*Create a Hello World Web App for Azure in IntelliJ (This Article)*</span></span>
  * <span data-ttu-id="df61f-263">[Újdonságok az intellij-t Azure eszköztára hello]</span><span class="sxs-lookup"><span data-stu-id="df61f-263">[What's New in hello Azure Toolkit for IntelliJ]</span></span>

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="df61f-264">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="df61f-264">See Also</span></span>
<span data-ttu-id="df61f-265">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].</span><span class="sxs-lookup"><span data-stu-id="df61f-265">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

<span data-ttu-id="df61f-266">Azure Web Apps létrehozásával kapcsolatos további információkért lásd: hello [webes alkalmazások – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="df61f-266">For additional information about creating Azure Web Apps, see hello [Web Apps Overview].</span></span>

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Eclipse Azure eszköztára]: ../azure-toolkit-for-eclipse.md
[IntelliJ Azure eszköztára]: ../azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ../azure-toolkit-for-eclipse-installation.md
[telepítése hello Azure eszköztára IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Újdonságok az Eclipse Azure eszköztára hello]: ../azure-toolkit-for-eclipse-whats-new.md
[Újdonságok az intellij-t Azure eszköztára hello]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[webes alkalmazások – áttekintés]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
