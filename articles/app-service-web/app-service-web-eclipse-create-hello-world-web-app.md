---
title: "Alapszintű Azure web app használatával aaaCreate Holdas |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan toouse hello Azure eszközkészlet Eclipse toocreate egy Hello World az Azure-webalkalmazást."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: b2f42e0e7a5b98760ec02fab2fc38f9f07b1156b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a><span data-ttu-id="299e7-103">Alapszintű Azure-webalkalmazás létrehozása használata az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="299e7-103">Create a basic Azure web app using Eclipse</span></span>
<span data-ttu-id="299e7-104">Ez az oktatóanyag bemutatja, hogyan toocreate és központi telepítése egy alapszintű Hello World alkalmazás tooAzure webalkalmazásként hello segítségével [Eclipse Azure eszköztára].</span><span class="sxs-lookup"><span data-stu-id="299e7-104">This tutorial shows how toocreate and deploy a basic Hello World application tooAzure as a Web App by using hello [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="299e7-105">Egy alapszintű JSP példa az egyszerűség, de hasonló lépésekkel akkor lehet megfelelő, egy Java servlet, az Azure-telepítés illetően.</span><span class="sxs-lookup"><span data-stu-id="299e7-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="299e7-106">Ez az oktatóanyag befejezése után, az alkalmazás a következő ábrán egy webböngészőben megtekintésekor hasonló toohello fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="299e7-106">When you have completed this tutorial, your application will look similar toohello following illustration when you view it in a web browser:</span></span>

![Előzetes verziójú Hello World alkalmazás][01]

## <a name="prerequisites"></a><span data-ttu-id="299e7-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="299e7-108">Prerequisites</span></span>
* <span data-ttu-id="299e7-109">A Java fejlesztői készlet (JDK), v 1,8 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="299e7-109">A Java Developer Kit (JDK), v 1.8 or later.</span></span>
* <span data-ttu-id="299e7-110">Java EE-fejlesztőknek, Luna IDE Holdas vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="299e7-110">Eclipse IDE for Java EE Developers, Luna or later.</span></span> <span data-ttu-id="299e7-111">Ez letölthető <http://www.eclipse.org/downloads/>.</span><span class="sxs-lookup"><span data-stu-id="299e7-111">This can be downloaded from <http://www.eclipse.org/downloads/>.</span></span>
* <span data-ttu-id="299e7-112">A Java-alapú webkiszolgáló vagy a kiszolgáló, terjesztési például [Apache Tomcat] vagy [Jetty].</span><span class="sxs-lookup"><span data-stu-id="299e7-112">A distribution of a Java-based web server or application server, such as [Apache Tomcat] or [Jetty].</span></span>
* <span data-ttu-id="299e7-113">Azure-előfizetéssel, amely szerezhető: a <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>.</span><span class="sxs-lookup"><span data-stu-id="299e7-113">An Azure subscription, which can be acquired from <https://azure.microsoft.com/free/> or <http://azure.microsoft.com/pricing/purchase-options/>.</span></span>
* <span data-ttu-id="299e7-114">Hello [Eclipse Azure eszköztára].</span><span class="sxs-lookup"><span data-stu-id="299e7-114">hello [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="299e7-115">Hello Azure eszközkészlet telepítésével kapcsolatos információkért lásd: [telepítése hello Azure eszköztára Eclipse].</span><span class="sxs-lookup"><span data-stu-id="299e7-115">For information about installing hello Azure Toolkit, see [Installing hello Azure Toolkit for Eclipse].</span></span>

## <a name="toocreate-a-hello-world-application"></a><span data-ttu-id="299e7-116">toocreate egy Hello World alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="299e7-116">toocreate a Hello World application</span></span>
<span data-ttu-id="299e7-117">Először először foglalkozunk létrehoz egy Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="299e7-117">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="299e7-118">Indítsa el az eclipse-ben, és hello menüben kattintson **fájl**, kattintson a **új**, és kattintson a **dinamikus webes projekt**.</span><span class="sxs-lookup"><span data-stu-id="299e7-118">Start Eclipse, and at hello menu click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="299e7-119">(Ha nem lát **dinamikus webes projekt** kattintás után az elérhető projektek tulajdonosaként **fájl** és **új**, majd hello a következő: kattintson a **fájl**, kattintson a **új**, kattintson a **projekt...** , bontsa ki a **webes**, kattintson a **dinamikus webes projekt**, és kattintson a **következő**.)</span><span class="sxs-lookup"><span data-stu-id="299e7-119">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do hello following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>
2. <span data-ttu-id="299e7-120">Ez az oktatóanyag céljából, nevet hello projektnek **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="299e7-120">For purposes of this tutorial, name hello project **MyWebApp**.</span></span> <span data-ttu-id="299e7-121">A képernyő jelenik meg hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="299e7-121">Your screen will appear similar toohello following:</span></span>
   
    ![Új dinamikus webes projekt létrehozása][02]
3. <span data-ttu-id="299e7-123">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="299e7-123">Click **Finish**.</span></span>
4. <span data-ttu-id="299e7-124">A Eclipse Project Explorer nézet, bontsa ki a **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="299e7-124">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="299e7-125">Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.</span><span class="sxs-lookup"><span data-stu-id="299e7-125">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
5. <span data-ttu-id="299e7-126">A hello **új JSP-fájl** párbeszédpanelen nevű hello fájl **index.jsp**, hello fölérendelt mappája, tartsa **MyWebApp/WebContent**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="299e7-126">In hello **New JSP File** dialog box, name hello file **index.jsp**, keep hello parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>
6. <span data-ttu-id="299e7-127">A hello **JSP-sablon kiválasztása** párbeszédpanel, az oktatóanyag válasszon alkalmazásában **új JSP-fájl (html)**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="299e7-127">In hello **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
7. <span data-ttu-id="299e7-128">Megnyitásakor az index.jsp fájl az eclipse-ben, adja hozzá a szöveg toodynamically megjelenítési **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="299e7-128">When your index.jsp file opens in Eclipse, add in text toodynamically display **Hello World!**</span></span> <span data-ttu-id="299e7-129">hello meglévő belül `<body>` elemet.</span><span class="sxs-lookup"><span data-stu-id="299e7-129">within hello existing `<body>` element.</span></span> <span data-ttu-id="299e7-130">A frissített `<body>` tartalom a következő példa hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="299e7-130">Your updated `<body>` content should resemble hello following example:</span></span>
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. <span data-ttu-id="299e7-131">Mentse az index.jsp.</span><span class="sxs-lookup"><span data-stu-id="299e7-131">Save index.jsp.</span></span>

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a><span data-ttu-id="299e7-132">toodeploy az alkalmazás tooan Azure Web App tároló</span><span class="sxs-lookup"><span data-stu-id="299e7-132">toodeploy your application tooan Azure Web App Container</span></span>
<span data-ttu-id="299e7-133">Számos módon egy Java webes alkalmazás tooAzure-telepíthet.</span><span class="sxs-lookup"><span data-stu-id="299e7-133">There are several ways by which you can deploy a Java web application tooAzure.</span></span> <span data-ttu-id="299e7-134">Ez az oktatóanyag leírja az egyik legegyszerűbb hello: az alkalmazás lesz telepített tooan Azure Web App tároló - semmilyen különleges projekttípus sem további eszközök van szükség.</span><span class="sxs-lookup"><span data-stu-id="299e7-134">This tutorial describes one of hello simplest: your application will be deployed tooan Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="299e7-135">hello JDK és hello webes tároló szoftver biztosítja az Ön Azure-ra, így nincs szükség tooupload a saját; szüksége a Java-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="299e7-135">hello JDK and hello web container software will be provided for you by Azure, so there is no need tooupload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="299e7-136">Ennek eredményeképpen hello közzétételi alkalmazás vesz igénybe másodperc, nem perc.</span><span class="sxs-lookup"><span data-stu-id="299e7-136">As a result, hello publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="299e7-137">A Project Explorer Eclipse meg, kattintson a jobb gombbal **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="299e7-137">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>
2. <span data-ttu-id="299e7-138">Hello helyi menüben válasszon ki **Azure**, majd kattintson a **Azure Web Apps közzététel...**</span><span class="sxs-lookup"><span data-stu-id="299e7-138">In hello context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
    ![Az Azure webes alkalmazás közzététele][03]
   
    <span data-ttu-id="299e7-140">Azt is megteheti, amíg a webes projektet a Project Explorer hello van jelölve, kattintson hello **közzététel** a hello eszköztár, és válassza a legördülő gomb **Azure Web Apps közzététel** innen:</span><span class="sxs-lookup"><span data-stu-id="299e7-140">Alternatively, while your web application project is selected in hello Project Explorer, you can click hello **Publish** dropdown button on hello toolbar and select **Publish as Azure Web App** from there:</span></span>
   
    ![Az Azure webes alkalmazás közzététele][14]
3. <span data-ttu-id="299e7-142">Ha már nem regisztrált az Azure az eclipse-ben, a Azure fiókjába fogja felszólító toosign:</span><span class="sxs-lookup"><span data-stu-id="299e7-142">If you have not already signed into Azure from Eclipse, you will be prompted toosign into your Azure account:</span></span>
   
    ![Az Azure bejelentkezési párbeszédpanel][04]
   
    <span data-ttu-id="299e7-144">Több Azure fiók, van néhány hello kér során hello jelentkezzen be a folyamat előfordulhat, hogy megjelenik-e egynél többször, még akkor is, ha úgy tűnik, mintha toobe hello azonos.</span><span class="sxs-lookup"><span data-stu-id="299e7-144">If you have multiple Azure accounts, some of hello prompts during hello sign in process may be shown more than once, even if they appear toobe hello same.</span></span> <span data-ttu-id="299e7-145">Ha ez történik, továbbra is a következő utasításokat hello bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="299e7-145">When this happens, continue following hello sign in instructions.</span></span>
4. <span data-ttu-id="299e7-146">Miután sikeresen bejelentkezett az Azure-fiókjával a, hello **előfizetések kezelése oldalt** párbeszédpanelen jelenik meg a hitelesítő adatok társított előfizetések listáját.</span><span class="sxs-lookup"><span data-stu-id="299e7-146">After you have successfully signed into your Azure account, hello **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="299e7-147">Ha több felsorolt előfizetések tartoznak, és azt szeretné, hogy az egyik csak egy adott részének toowork, a hello meg a kívánt toouse szükség lehet, hogy jelet.</span><span class="sxs-lookup"><span data-stu-id="299e7-147">If there are multiple subscriptions listed and you want toowork with only a specific subset of them, you may optionally uncheck hello ones you do want toouse.</span></span> <span data-ttu-id="299e7-148">Miután kiválasztotta az előfizetések, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="299e7-148">When you have selected your subscriptions, click **Close**.</span></span>
   
    ![Kezelése előfizetések párbeszédpanel][05]
5. <span data-ttu-id="299e7-150">Ha hello **központi telepítése a webes alkalmazás tároló tooAzure** párbeszédpanel jelenik meg, jeleníti meg, hogy a korábban létrehozott webalkalmazás tárolókkal; Ha nem hozott létre tárolót, hello listája üres lesz.</span><span class="sxs-lookup"><span data-stu-id="299e7-150">When hello **Deploy tooAzure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, hello list will be empty.</span></span>
   
    ![Webes alkalmazás tároló párbeszédpanel tooAzure telepítése][06]
6. <span data-ttu-id="299e7-152">Ha még nem hozott egy Azure webes alkalmazás tároló előtt, vagy ha azt szeretné, hogy toopublish az alkalmazás tooa új tároló, használja a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="299e7-152">If you have not created an Azure Web App Container before, or if you would like toopublish your application tooa new container, use hello following steps.</span></span> <span data-ttu-id="299e7-153">Ellenkező esetben válassza ki a meglévő Web App tároló, és hagyja ki az alábbi 7 toostep.</span><span class="sxs-lookup"><span data-stu-id="299e7-153">Otherwise, select an existing Web App Container and skip toostep 7 below.</span></span>
   
   1. <span data-ttu-id="299e7-154">Kattintson a **új...**</span><span class="sxs-lookup"><span data-stu-id="299e7-154">Click **New...**</span></span>
      
       ![Webes alkalmazás tároló párbeszédpanel tooAzure telepítése][15]
   2. <span data-ttu-id="299e7-156">Hello **új webes alkalmazás tároló** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="299e7-156">hello **New Web App Container** dialog box will be displayed:</span></span>
      
       ![Új webes alkalmazás tároló párbeszédpanel][07a]
   3. <span data-ttu-id="299e7-158">Adjon meg egy **DNS-címke** a webes alkalmazás tároló; ez alkotó hello levél DNS-címke hello gazdagépre URL-címet a webalkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="299e7-158">Enter a **DNS Label** for your Web App Container; this will form hello leaf DNS label of hello host URL for your web application in Azure.</span></span> <span data-ttu-id="299e7-159">(Vegye figyelembe hello névvel kell érhető el, és tooAzure webalkalmazás-kiosztási követelményeinek felel meg.)</span><span class="sxs-lookup"><span data-stu-id="299e7-159">(Note that hello name must be available and conform tooAzure Web App naming requirements.)</span></span>
   4. <span data-ttu-id="299e7-160">A hello **webes tároló** legördülő menüre, válassza hello megfelelő szoftvert az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="299e7-160">In hello **Web Container** drop-down menu, select hello appropriate software for your application.</span></span>
      
       <span data-ttu-id="299e7-161">Jelenleg Tomcat 8 Tomcat 7 vagy Jetty 9 közül választhat.</span><span class="sxs-lookup"><span data-stu-id="299e7-161">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="299e7-162">Kijelölt hello szoftver legújabb terjesztési Azure biztosítja, és akkor fog futni, a legutóbbi terjesztésipont JDK 8 Oracle által létrehozott és az Azure által biztosított.</span><span class="sxs-lookup"><span data-stu-id="299e7-162">A recent distribution of hello selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>
   5. <span data-ttu-id="299e7-163">A hello **előfizetés** legördülő menüre, válassza hello előfizetés toouse ehhez a központi telepítéshez használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="299e7-163">In hello **Subscription** drop-down menu, select hello subscription you want toouse for this deployment.</span></span>
   6. <span data-ttu-id="299e7-164">A hello **erőforráscsoport** legördülő menüben válassza hello erőforráscsoportot, amelyhez tooassociate a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="299e7-164">In hello **Resource Group** drop-down menu, select hello Resource Group with which you want tooassociate your Web App.</span></span> <span data-ttu-id="299e7-165">(Az azure erőforráscsoport-sablonok engedélyezi, akkor toogroup kapcsolódó erőforrások együtt, így például, hogy törölheti együtt.)</span><span class="sxs-lookup"><span data-stu-id="299e7-165">(Azure Resource Groups allow you toogroup related resources together so that, for example, they can be deleted together.)</span></span>
      
       <span data-ttu-id="299e7-166">(Ha vannak ilyenek) egy meglévő erőforráscsoportot kiválaszthatja, és skip toostep g az alábbi, vagy használjon hello ezeket a következő lépéseket egy új erőforráscsoportot toocreate:</span><span class="sxs-lookup"><span data-stu-id="299e7-166">You can select an existing Resource Group (if you have any) and skip toostep g below, or use hello following these steps toocreate a new Resource Group:</span></span>
      
      * <span data-ttu-id="299e7-167">Kattintson a **új...**</span><span class="sxs-lookup"><span data-stu-id="299e7-167">Click **New...**</span></span>
      * <span data-ttu-id="299e7-168">Hello **új erőforráscsoport** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="299e7-168">hello **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Új erőforráscsoport párbeszédpanel][08]
      * <span data-ttu-id="299e7-170">A hello hello **neve** szövegmező, adja meg az új erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="299e7-170">In hello hello **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="299e7-171">A hello hello **régió** legördülő menüben válassza hello megfelelő Azure adatközpont-erőforrásnak a helyét.</span><span class="sxs-lookup"><span data-stu-id="299e7-171">In hello hello **Region** drop-down menu, select hello appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="299e7-172">Választható lehetőség: Alapértelmezés szerint Java 8 legutóbbi eloszlásáról telepíti az Azure automatikusan tooyour web app tárolóban a JVM-et.</span><span class="sxs-lookup"><span data-stu-id="299e7-172">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically tooyour web app container as your JVM.</span></span> <span data-ttu-id="299e7-173">Azonban is megadhat egy másik verziót és a terjesztési hello JVM-et, ha a webalkalmazás számára szükséges.</span><span class="sxs-lookup"><span data-stu-id="299e7-173">However, you can specify a different version and distribution of hello JVM if your Web App requires it.</span></span> <span data-ttu-id="299e7-174">a webalkalmazás JDK toospecify hello kattintson hello **JDK** fülre, és válassza ki a hello alábbi beállítások egyikét:</span><span class="sxs-lookup"><span data-stu-id="299e7-174">toospecify hello JDK for your Web App, click hello **JDK** tab, and select one of hello following options:</span></span>
        
        * <span data-ttu-id="299e7-175">**Hello alapértelmezett Azure Web Apps szolgáltatás által kínált JDK telepítése**: Ez a beállítás telepíti a Java 8 legutóbbi eloszlásáról.</span><span class="sxs-lookup"><span data-stu-id="299e7-175">**Deploy hello default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java 8.</span></span>
        * <span data-ttu-id="299e7-176">**A 3. fél elérhető Azure JDK telepítése**: Ez a beállítás lehetővé teszi toochoose JDKs, amely a Microsoft Azure által biztosított hello listája.</span><span class="sxs-lookup"><span data-stu-id="299e7-176">**Deploy a 3rd party JDK available on Azure**: This option allows you toochoose from hello list of JDKs which are provided by Microsoft Azure.</span></span>
        * <span data-ttu-id="299e7-177">**A letöltési helyről saját JDK telepítése**: Ez a beállítás lehetővé teszi toospecify saját JDK terjesztési, amely egy ZIP-fájlba kell csomagolni, és a nyilvánosan elérhető letöltési hely vagy egy Azure storage-fiók, amelynek tooeither feltöltött, rendelkezik hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="299e7-177">**Deploy my own JDK from this download location**: This option allows you toospecify your own JDK distribution, which must be packaged as a ZIP file and uploaded tooeither a publicly available download location or an Azure storage account for which you have access.</span></span>
          
          ![Új webes alkalmazás tároló párbeszédpanel][07b]
   7. <span data-ttu-id="299e7-179">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="299e7-179">Click **OK**.</span></span>
   8. <span data-ttu-id="299e7-180">Hello **App Service-csomag** legördülő menü hello app service-csomagokról hello erőforráscsoport kiválasztott társított sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="299e7-180">hello **App Service Plan** drop-down menu lists hello app service plans that are associated with hello Resource Group that you selected.</span></span> <span data-ttu-id="299e7-181">(App Service-csomagokról adja meg például a Web App alkalmazásban tarifacsomag és hello számítási példányméretének hello hello helyét.</span><span class="sxs-lookup"><span data-stu-id="299e7-181">(App Service Plans specify information such as hello location of your Web App, hello pricing tier and hello compute instance size.</span></span> <span data-ttu-id="299e7-182">Egy egyetlen App Service-csomag használható több Web Apps, ezért azt egy adott webalkalmazás telepítésből külön megmarad.)</span><span class="sxs-lookup"><span data-stu-id="299e7-182">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       <span data-ttu-id="299e7-183">Kiválaszthat egy meglévő App Service-csomag (ha vannak ilyenek), és hagyja ki az alábbi toostep h, vagy ezek lépések toocreate egy új App Service-csomag a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="299e7-183">You can select an existing App Service Plan (if you have any) and skip toostep h below, or use hello following these steps toocreate a new App Service Plan:</span></span>
      
      * <span data-ttu-id="299e7-184">Kattintson a **új...**</span><span class="sxs-lookup"><span data-stu-id="299e7-184">Click **New...**</span></span>
      * <span data-ttu-id="299e7-185">Hello **új App Service-csomag** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="299e7-185">hello **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Új App Service-csomag párbeszédpanel][09]
      * <span data-ttu-id="299e7-187">A hello hello **neve** szövegmező, adjon meg egy nevet az új App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="299e7-187">In hello hello **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="299e7-188">A hello hello **hely** legördülő menüben válassza hello megfelelő Azure adatközpont hello terv helyét.</span><span class="sxs-lookup"><span data-stu-id="299e7-188">In hello hello **Location** drop-down menu, select hello appropriate Azure data center location for hello plan.</span></span>
      * <span data-ttu-id="299e7-189">A hello hello **Tarifacsomagot** legördülő menüben válassza hello megfelelő hello terv árképzési.</span><span class="sxs-lookup"><span data-stu-id="299e7-189">In hello hello **Pricing Tier** drop-down menu, select hello appropriate pricing for hello plan.</span></span> <span data-ttu-id="299e7-190">Tesztelési célokra választhat **szabad**.</span><span class="sxs-lookup"><span data-stu-id="299e7-190">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="299e7-191">A hello hello **Példányméretének** legördülő menüre, válassza hello megfelelő példányméretének hello terv.</span><span class="sxs-lookup"><span data-stu-id="299e7-191">In hello hello **Instance Size** drop-down menu, select hello appropriate instance size for hello plan.</span></span> <span data-ttu-id="299e7-192">Tesztelési célokra választhat **kis**.</span><span class="sxs-lookup"><span data-stu-id="299e7-192">For testing purposes you can choose **Small**.</span></span>
   9. <span data-ttu-id="299e7-193">Az összes fenti lépéseket hello befejeződött, hello új webes alkalmazás tároló párbeszédpanelen a következő ábra hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="299e7-193">Once you have completed all of hello above steps, hello New Web App Container dialog box should resemble hello following illustration:</span></span>
      
       ![Új webes alkalmazás tároló párbeszédpanel][10]
   10. <span data-ttu-id="299e7-195">Kattintson a **OK** toocomplete hello létrehozása az új webalkalmazáshoz tároló.</span><span class="sxs-lookup"><span data-stu-id="299e7-195">Click **OK** toocomplete hello creation of your new Web App container.</span></span>
       
        <span data-ttu-id="299e7-196">Várjon néhány másodpercet hello webalkalmazás tárolók toobe hello listája frissítve, és az újonnan létrehozott webes alkalmazás tároló kijelöli hello listában.</span><span class="sxs-lookup"><span data-stu-id="299e7-196">Wait a few seconds for hello list of hello Web App containers toobe refreshed, and your newly-created web app container should now be selected in hello list.</span></span>
7. <span data-ttu-id="299e7-197">Most már készen áll a toocomplete hello kezdeti telepítése a webes alkalmazás tooAzure áll:</span><span class="sxs-lookup"><span data-stu-id="299e7-197">You are now ready toocomplete hello initial deployment of your Web App tooAzure:</span></span>
   
    ![Webes alkalmazás tároló párbeszédpanel tooAzure telepítése][11]
   
    <span data-ttu-id="299e7-199">Kattintson a **OK** toodeploy a Java-alkalmazás toohello kijelölt webalkalmazás tároló.</span><span class="sxs-lookup"><span data-stu-id="299e7-199">Click **OK** toodeploy your Java application toohello selected Web App container.</span></span>
   
    <span data-ttu-id="299e7-200">Alapértelmezés szerint az alkalmazás telepítése, hello alkalmazáskiszolgáló alkönyvtára.</span><span class="sxs-lookup"><span data-stu-id="299e7-200">By default, your application will be deployed as a subdirectory of hello application server.</span></span> <span data-ttu-id="299e7-201">Ha azt szeretné, központilag telepített hello gyökéralkalmazás toobe, ellenőrizze a hello **tooroot telepítése** való kattintás előtt jelölőnégyzet **OK**.</span><span class="sxs-lookup"><span data-stu-id="299e7-201">If you want it toobe deployed as hello root application, check hello **Deploy tooroot** checkbox before clicking **OK**.</span></span>
8. <span data-ttu-id="299e7-202">Ezt követően megtekintheti az hello **Azure tevékenységnapló** nézetet, amely a webalkalmazás hello központi telepítésének állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="299e7-202">Next, you should see hello **Azure Activity Log** view, which will indicate hello deployment status of your Web App.</span></span>
   
    ![Azure tevékenységnapló][12]
   
    <span data-ttu-id="299e7-204">a webalkalmazás tooAzure telepítési folyamata hello gyorsabban csupán néhány másodpercet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="299e7-204">hello process of deploying your Web App tooAzure should take only a few seconds toocomplete.</span></span> <span data-ttu-id="299e7-205">Ha alkalmazás készen áll, megjelenik egy hivatkozás nevű **közzétett** a hello **állapot** oszlop.</span><span class="sxs-lookup"><span data-stu-id="299e7-205">When your application ready, you will see a link named **Published** in hello **Status** column.</span></span> <span data-ttu-id="299e7-206">Hello hivatkozásra tart, telepített tooyour webes alkalmazás kezdőlapja.</span><span class="sxs-lookup"><span data-stu-id="299e7-206">When you click hello link, it will take you tooyour deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="299e7-207">Webalkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="299e7-207">Updating your web app</span></span>
<span data-ttu-id="299e7-208">Frissítése egy meglévő Azure-webalkalmazás fut egy gyors és egyszerű eljárás, és frissítési lehetőségei vannak:</span><span class="sxs-lookup"><span data-stu-id="299e7-208">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="299e7-209">Java-webalkalmazás telepítése hello frissítheti.</span><span class="sxs-lookup"><span data-stu-id="299e7-209">You can update hello deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="299e7-210">Egy további Java-alkalmazás toohello közzététele Web App tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="299e7-210">You can publish an additional Java application toohello same Web App Container.</span></span>

<span data-ttu-id="299e7-211">Mindkét esetben hello folyamata azonos, és csupán néhány másodpercet vesz igénybe:</span><span class="sxs-lookup"><span data-stu-id="299e7-211">In either case, hello process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="299e7-212">Az Eclipse project explorer hello kattintson a jobb gombbal hello Java-alkalmazás hozzáadása a webes alkalmazás tároló meglévő tooan vagy tooupdate szeretné.</span><span class="sxs-lookup"><span data-stu-id="299e7-212">In hello Eclipse project explorer, right-click hello Java application you want tooupdate or add tooan existing Web App Container.</span></span>
2. <span data-ttu-id="299e7-213">Amikor hello helyi menü megjelenik, válassza ki a **Azure** , majd **Azure Web Apps közzététel...**</span><span class="sxs-lookup"><span data-stu-id="299e7-213">When hello context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="299e7-214">Már már bejelentkezett korábban, mivel látni fogja a meglévő Web App tárolók listája.</span><span class="sxs-lookup"><span data-stu-id="299e7-214">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="299e7-215">Válasszon egyet a Java-alkalmazás tooand kattintson újra közzé vagy toopublish kívánt hello **OK**.</span><span class="sxs-lookup"><span data-stu-id="299e7-215">Select hello one you want toopublish or re-publish your Java application tooand click **OK**.</span></span>

<span data-ttu-id="299e7-216">Néhány másodperc múlva, hello **Azure tevékenységnapló** nézetben megjelenik, a frissített telepítési **közzétett** , és a frissített alkalmazást egy webböngészőben lesz képes tooverify.</span><span class="sxs-lookup"><span data-stu-id="299e7-216">A few seconds later, hello **Azure Activity Log** view will show your updated deployment as **Published** and you will be able tooverify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="299e7-217">Indítása, leállítása vagy egy már meglévő webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="299e7-217">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="299e7-218">toostart vagy állítsa le a meglévő Azure Web Apps tároló, (beleértve az összes telepített hello Java-alkalmazások azt), használhatja a hello **Azure Explorer** nézet.</span><span class="sxs-lookup"><span data-stu-id="299e7-218">toostart or stop an existing Azure Web App container, (including all hello deployed Java applications in it), you can use hello **Azure Explorer** view.</span></span>

<span data-ttu-id="299e7-219">Ha hello **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **ablak** menü az eclipse-ben, majd kattintson a **nézet megjelenítése**, majd **más...** , majd **Azure**, és kattintson a **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="299e7-219">If hello **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="299e7-220">Ha korábban nem jelentkezett be, amely felszólítja toodo stb.</span><span class="sxs-lookup"><span data-stu-id="299e7-220">If you have not previously logged in, it will prompt you toodo so.</span></span>

<span data-ttu-id="299e7-221">Ha hello **Azure Explorer** nézet jelenik meg, kövesse az alábbi lépéseket toostart használatát, vagy a webalkalmazás leállítása:</span><span class="sxs-lookup"><span data-stu-id="299e7-221">When hello **Azure Explorer** view is displayed, use follow these steps toostart or stop your Web App:</span></span> 

1. <span data-ttu-id="299e7-222">Bontsa ki a hello **Azure** csomópont.</span><span class="sxs-lookup"><span data-stu-id="299e7-222">Expand hello **Azure** node.</span></span>
2. <span data-ttu-id="299e7-223">Bontsa ki a hello **webalkalmazások** csomópont.</span><span class="sxs-lookup"><span data-stu-id="299e7-223">Expand hello **Web Apps** node.</span></span> 
3. <span data-ttu-id="299e7-224">Kattintson a jobb gombbal hello kívánt webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="299e7-224">Right-click hello desired Web App.</span></span>
4. <span data-ttu-id="299e7-225">Amikor hello helyi menü megjelenik, kattintson a **Start**, **leállítása**, vagy **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="299e7-225">When hello context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="299e7-226">Ne feledje, hogy hello menü döntések környezet használatát, így csak a futó webalkalmazás leállítása vagy egy webalkalmazást, ami jelenleg nem fut. Indítsa el.</span><span class="sxs-lookup"><span data-stu-id="299e7-226">Note that hello menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
    ![Egy már meglévő webalkalmazás leállítása][13]

## <a name="next-steps"></a><span data-ttu-id="299e7-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="299e7-228">Next Steps</span></span>
<span data-ttu-id="299e7-229">A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="299e7-229">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="299e7-230">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="299e7-230">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="299e7-231">[telepítése hello Azure eszköztára Eclipse]</span><span class="sxs-lookup"><span data-stu-id="299e7-231">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="299e7-232">*Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben (Ez a cikk)*</span><span class="sxs-lookup"><span data-stu-id="299e7-232">*Create a Hello World Web App for Azure in Eclipse (This Article)*</span></span>
  * <span data-ttu-id="299e7-233">[Újdonságok az Eclipse Azure eszköztára hello]</span><span class="sxs-lookup"><span data-stu-id="299e7-233">[What's New in hello Azure Toolkit for Eclipse]</span></span>
* <span data-ttu-id="299e7-234">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="299e7-234">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="299e7-235">[Az IntelliJ hello Azure eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="299e7-235">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="299e7-236">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="299e7-236">[Create a Hello World Web App for Azure in IntelliJ]</span></span>
  * <span data-ttu-id="299e7-237">[Újdonságok az intellij-t Azure eszköztára hello]</span><span class="sxs-lookup"><span data-stu-id="299e7-237">[What's New in hello Azure Toolkit for IntelliJ]</span></span>

<span data-ttu-id="299e7-238">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].</span><span class="sxs-lookup"><span data-stu-id="299e7-238">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

<span data-ttu-id="299e7-239">Azure Web Apps létrehozásával kapcsolatos további információkért lásd: hello [webes alkalmazások – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="299e7-239">For additional information about creating Azure Web Apps, see hello [Web Apps Overview].</span></span>

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Eclipse Azure eszköztára]: ../azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web-intellij-create-hello-world-web-app.md
[telepítése hello Azure eszköztára Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ../azure-toolkit-for-intellij-installation.md
[Újdonságok az Eclipse Azure eszköztára hello]: ../azure-toolkit-for-eclipse-whats-new.md
[Újdonságok az intellij-t Azure eszköztára hello]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[webes alkalmazások – áttekintés]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
