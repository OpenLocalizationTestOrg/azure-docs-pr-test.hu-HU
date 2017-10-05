---
title: "Alapszintű Azure-webalkalmazás létrehozása Eclipse használatával |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan használható az Azure-eszközkészlet az eclipse-ben Hello World webalkalmazás létrehozása az Azure."
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
ms.openlocfilehash: 683d6828546995a4dc60d8cac0669f356501c723
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a><span data-ttu-id="9ad60-103">Alapszintű Azure-webalkalmazás létrehozása használata az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="9ad60-103">Create a basic Azure web app using Eclipse</span></span>
<span data-ttu-id="9ad60-104">Ez az oktatóanyag bemutatja, hogyan hozhat létre és telepíthet egy alapszintű Hello World alkalmazásról az Azure-ba, a webes alkalmazás segítségével a [Eclipse-hez készült Azure-eszközkészlet].</span><span class="sxs-lookup"><span data-stu-id="9ad60-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a Web App by using the [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="9ad60-105">Egy alapszintű JSP példa az egyszerűség, de hasonló lépésekkel akkor lehet megfelelő, egy Java servlet, az Azure-telepítés illetően.</span><span class="sxs-lookup"><span data-stu-id="9ad60-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="9ad60-106">Ez az oktatóanyag befejezése után, az alkalmazás majd meg az alábbi ábrához hasonlóan egy webböngészőben megtekintheti:</span><span class="sxs-lookup"><span data-stu-id="9ad60-106">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![Előzetes verziójú Hello World alkalmazás][01]

## <a name="prerequisites"></a><span data-ttu-id="9ad60-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9ad60-108">Prerequisites</span></span>
* <span data-ttu-id="9ad60-109">A Java fejlesztői készlet (JDK), v 1,8 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="9ad60-109">A Java Developer Kit (JDK), v 1.8 or later.</span></span>
* <span data-ttu-id="9ad60-110">Java EE-fejlesztőknek, Luna IDE Holdas vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="9ad60-110">Eclipse IDE for Java EE Developers, Luna or later.</span></span> <span data-ttu-id="9ad60-111">Ez letölthető <http://www.eclipse.org/downloads/>.</span><span class="sxs-lookup"><span data-stu-id="9ad60-111">This can be downloaded from <http://www.eclipse.org/downloads/>.</span></span>
* <span data-ttu-id="9ad60-112">A Java-alapú webkiszolgáló vagy a kiszolgáló, terjesztési például [Apache Tomcat] vagy [Jetty].</span><span class="sxs-lookup"><span data-stu-id="9ad60-112">A distribution of a Java-based web server or application server, such as [Apache Tomcat] or [Jetty].</span></span>
* <span data-ttu-id="9ad60-113">Azure-előfizetéssel, amely szerezhető: a <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>.</span><span class="sxs-lookup"><span data-stu-id="9ad60-113">An Azure subscription, which can be acquired from <https://azure.microsoft.com/free/> or <http://azure.microsoft.com/pricing/purchase-options/>.</span></span>
* <span data-ttu-id="9ad60-114">Az [Eclipse-hez készült Azure-eszközkészlet].</span><span class="sxs-lookup"><span data-stu-id="9ad60-114">The [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="9ad60-115">Az Azure-eszközkészlet telepítésével kapcsolatos információkért lásd: [az eclipse-ben az Azure eszközkészlet telepítésével].</span><span class="sxs-lookup"><span data-stu-id="9ad60-115">For information about installing the Azure Toolkit, see [Installing the Azure Toolkit for Eclipse].</span></span>

## <a name="to-create-a-hello-world-application"></a><span data-ttu-id="9ad60-116">Hello World alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ad60-116">To create a Hello World application</span></span>
<span data-ttu-id="9ad60-117">Először először foglalkozunk létrehoz egy Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="9ad60-117">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="9ad60-118">Indítsa el az Eclipse, és válassza a menü **fájl**, kattintson a **új**, és kattintson a **dinamikus webes projekt**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-118">Start Eclipse, and at the menu click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="9ad60-119">(Ha nem lát **dinamikus webes projekt** kattintás után az elérhető projektek tulajdonosaként **fájl** és **új**, majd tegye a következőket: kattintson a **fájl**, kattintson a **új**, kattintson a **projekt...** , bontsa ki a **webes**, kattintson a **dinamikus webes projekt**, és kattintson a **következő**.)</span><span class="sxs-lookup"><span data-stu-id="9ad60-119">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do the following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>
2. <span data-ttu-id="9ad60-120">Ebben az oktatóanyagban a nevet a projektnek **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-120">For purposes of this tutorial, name the project **MyWebApp**.</span></span> <span data-ttu-id="9ad60-121">A képernyő jelenik meg a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="9ad60-121">Your screen will appear similar to the following:</span></span>
   
    ![Új dinamikus webes projekt létrehozása][02]
3. <span data-ttu-id="9ad60-123">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ad60-123">Click **Finish**.</span></span>
4. <span data-ttu-id="9ad60-124">A Eclipse Project Explorer nézet, bontsa ki a **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-124">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="9ad60-125">Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.</span><span class="sxs-lookup"><span data-stu-id="9ad60-125">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
5. <span data-ttu-id="9ad60-126">A a **új JSP-fájl** párbeszédablakban nevezze el a fájl **index.jsp**, mint a szülőmappa **MyWebApp/WebContent**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-126">In the **New JSP File** dialog box, name the file **index.jsp**, keep the parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>
6. <span data-ttu-id="9ad60-127">Az a **JSP-sablon kiválasztása** párbeszédpanel, az oktatóanyag válasszon alkalmazásában **új JSP-fájl (html)**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-127">In the **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
7. <span data-ttu-id="9ad60-128">Megnyitásakor az index.jsp fájl az eclipse-ben, adja hozzá a dinamikusan megjelenő szöveg **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="9ad60-128">When your index.jsp file opens in Eclipse, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="9ad60-129">a már meglévő `<body>` elemhez.</span><span class="sxs-lookup"><span data-stu-id="9ad60-129">within the existing `<body>` element.</span></span> <span data-ttu-id="9ad60-130">A frissített `<body>` tartalma az alábbihoz kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="9ad60-130">Your updated `<body>` content should resemble the following example:</span></span>
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. <span data-ttu-id="9ad60-131">Mentse az index.jsp.</span><span class="sxs-lookup"><span data-stu-id="9ad60-131">Save index.jsp.</span></span>

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a><span data-ttu-id="9ad60-132">Egy Azure Web App tárolóhoz az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="9ad60-132">To deploy your application to an Azure Web App Container</span></span>
<span data-ttu-id="9ad60-133">Számos módon Azure Java-webalkalmazás-telepíthet.</span><span class="sxs-lookup"><span data-stu-id="9ad60-133">There are several ways by which you can deploy a Java web application to Azure.</span></span> <span data-ttu-id="9ad60-134">Ez az oktatóanyag leírja a legegyszerűbb egyikét: az alkalmazás telepítve lesz az Azure Web App tároló - semmilyen különleges projekttípus sem további eszközök van szükség.</span><span class="sxs-lookup"><span data-stu-id="9ad60-134">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="9ad60-135">A JDK és a webes tároló szoftver biztosítja az Ön Azure-ban, nincs szükség töltse fel a saját; szüksége a Java-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9ad60-135">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="9ad60-136">Ennek eredményeképpen a közzétételt az alkalmazás érvénybe másodperc, nem perc.</span><span class="sxs-lookup"><span data-stu-id="9ad60-136">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="9ad60-137">A Project Explorer Eclipse meg, kattintson a jobb gombbal **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-137">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>
2. <span data-ttu-id="9ad60-138">Válassza ki a helyi menü **Azure**, majd kattintson a **Publish Azure Web Apps...**</span><span class="sxs-lookup"><span data-stu-id="9ad60-138">In the context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
    ![Az Azure webes alkalmazás közzététele][03]
   
    <span data-ttu-id="9ad60-140">Azt is megteheti, amíg a webes projektet a Project Explorer van jelölve, kattintson a **közzététel** legördülő gomb az eszköztáron és a select **Publish Azure Web Apps,** innen:</span><span class="sxs-lookup"><span data-stu-id="9ad60-140">Alternatively, while your web application project is selected in the Project Explorer, you can click the **Publish** dropdown button on the toolbar and select **Publish as Azure Web App** from there:</span></span>
   
    ![Az Azure webes alkalmazás közzététele][14]
3. <span data-ttu-id="9ad60-142">Ha már nem regisztrált az Azure az eclipse-ben, a rendszer kéri, jelentkezzen be az Azure-fiókjába való:</span><span class="sxs-lookup"><span data-stu-id="9ad60-142">If you have not already signed into Azure from Eclipse, you will be prompted to sign into your Azure account:</span></span>
   
    ![Az Azure bejelentkezési párbeszédpanel][04]
   
    <span data-ttu-id="9ad60-144">Ha több Azure-fiókra, néhány, a program a bejelentkezési folyamat során a előfordulhat, hogy akkor is, ha úgy tűnik, mintha azonos egynél többször látható.</span><span class="sxs-lookup"><span data-stu-id="9ad60-144">If you have multiple Azure accounts, some of the prompts during the sign in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="9ad60-145">Ha ez történik, folytassa a bejelentkezési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9ad60-145">When this happens, continue following the sign in instructions.</span></span>
4. <span data-ttu-id="9ad60-146">Miután sikeresen bejelentkezett az Azure-fiókjával, a a **előfizetések kezelése oldalt** párbeszédpanelen jelenik meg a hitelesítő adatok társított előfizetések listáját.</span><span class="sxs-lookup"><span data-stu-id="9ad60-146">After you have successfully signed into your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="9ad60-147">Ha több előfizetéssel a listában, és ezek csak egy adott részének dolgozni szeretne, akkor előfordulhat, hogy nem kötelező, törölje a jelet meg a használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="9ad60-147">If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the ones you do want to use.</span></span> <span data-ttu-id="9ad60-148">Miután kiválasztotta az előfizetések, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-148">When you have selected your subscriptions, click **Close**.</span></span>
   
    ![Kezelése előfizetések párbeszédpanel][05]
5. <span data-ttu-id="9ad60-150">Ha a **központi telepítése az Azure Web App tárolóhoz** párbeszédpanel jelenik meg, jeleníti meg, hogy a korábban létrehozott webalkalmazás tárolókkal; Ha nem hozott létre tárolót, a lista üres lesz.</span><span class="sxs-lookup"><span data-stu-id="9ad60-150">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
    ![Azure Web App tároló párbeszédpanel telepítése][06]
6. <span data-ttu-id="9ad60-152">Ha nem hozott létre egy Azure webes alkalmazás tároló előtt, vagy ha azt szeretné, az alkalmazás egy új tároló közzétételére, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9ad60-152">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="9ad60-153">Egyéb esetben válassza ki a meglévő Web App tároló, és ugorjon a 7 alatt.</span><span class="sxs-lookup"><span data-stu-id="9ad60-153">Otherwise, select an existing Web App Container and skip to step 7 below.</span></span>
   
   1. <span data-ttu-id="9ad60-154">Kattintson a **új...**</span><span class="sxs-lookup"><span data-stu-id="9ad60-154">Click **New...**</span></span>
      
       ![Azure Web App tároló párbeszédpanel telepítése][15]
   2. <span data-ttu-id="9ad60-156">A **új webes alkalmazás tároló** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="9ad60-156">The **New Web App Container** dialog box will be displayed:</span></span>
      
       ![Új webes alkalmazás tároló párbeszédpanel][07a]
   3. <span data-ttu-id="9ad60-158">Adjon meg egy **DNS-címke** a webes alkalmazás tároló; ez alkotó a levél DNS-címke a gazdagépre URL-címének a webalkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="9ad60-158">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="9ad60-159">(Vegye figyelembe, hogy a név kell elérhetőnek lennie Azure Web Apps követelmények elnevezési felelnek meg.)</span><span class="sxs-lookup"><span data-stu-id="9ad60-159">(Note that the name must be available and conform to Azure Web App naming requirements.)</span></span>
   4. <span data-ttu-id="9ad60-160">Az a **webes tároló** legördülő menüben válassza ki a megfelelő szoftver az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="9ad60-160">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
       <span data-ttu-id="9ad60-161">Jelenleg Tomcat 8 Tomcat 7 vagy Jetty 9 közül választhat.</span><span class="sxs-lookup"><span data-stu-id="9ad60-161">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="9ad60-162">A kiválasztott szoftverfrissítés legutóbbi terjesztési Azure biztosítja, és akkor fog futni, a legutóbbi terjesztésipont JDK 8 Oracle által létrehozott és az Azure által biztosított.</span><span class="sxs-lookup"><span data-stu-id="9ad60-162">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>
   5. <span data-ttu-id="9ad60-163">Az a **előfizetés** legördülő menüben válassza ki az ehhez a központi telepítéshez használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="9ad60-163">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>
   6. <span data-ttu-id="9ad60-164">Az a **erőforráscsoport** legördülő menüben válasszon, amelyhez a webalkalmazás hozzárendelni kívánt erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9ad60-164">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="9ad60-165">(Az azure erőforráscsoportok lehetővé teszik kapcsolódó erőforrások egy csoportba, így például, hogy törölheti együtt.)</span><span class="sxs-lookup"><span data-stu-id="9ad60-165">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
       <span data-ttu-id="9ad60-166">Válasszon ki egy meglévő erőforráscsoportot (ha vannak ilyenek) és a skip. lépés: az alábbi g, vagy használja az alábbi lépéseket egy új erőforráscsoport létrehozásához a következő:</span><span class="sxs-lookup"><span data-stu-id="9ad60-166">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following these steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="9ad60-167">Kattintson a **új...**</span><span class="sxs-lookup"><span data-stu-id="9ad60-167">Click **New...**</span></span>
      * <span data-ttu-id="9ad60-168">A **új erőforráscsoport** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="9ad60-168">The **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Új erőforráscsoport párbeszédpanel][08]
      * <span data-ttu-id="9ad60-170">Az a a **neve** szövegmező, adja meg az új erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="9ad60-170">In the the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="9ad60-171">Az a a **régió** legördülő menüben válassza a megfelelő Azure adatközpont-erőforrásnak a helyét.</span><span class="sxs-lookup"><span data-stu-id="9ad60-171">In the the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="9ad60-172">Választható lehetőség: Alapértelmezés szerint Java 8 legutóbbi terjesztési telepítve lesz az Azure automatikusan a webes alkalmazás tároló, a JVM-et.</span><span class="sxs-lookup"><span data-stu-id="9ad60-172">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically to your web app container as your JVM.</span></span> <span data-ttu-id="9ad60-173">Azonban is megadhat egy másik verzió és elosztását illetően a JVM-et, ha a webalkalmazás írja elő.</span><span class="sxs-lookup"><span data-stu-id="9ad60-173">However, you can specify a different version and distribution of the JVM if your Web App requires it.</span></span> <span data-ttu-id="9ad60-174">A webalkalmazás a JDK megadásához kattintson a **JDK** lapot, és jelölje be az alábbi lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="9ad60-174">To specify the JDK for your Web App, click the **JDK** tab, and select one of the following options:</span></span>
        
        * <span data-ttu-id="9ad60-175">**Az alapértelmezett Azure Web Apps szolgáltatás által kínált JDK telepítése**: Ez a beállítás telepíti a Java 8 legutóbbi eloszlásáról.</span><span class="sxs-lookup"><span data-stu-id="9ad60-175">**Deploy the default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java 8.</span></span>
        * <span data-ttu-id="9ad60-176">**A 3. fél elérhető Azure JDK telepítése**: Ez a beállítás lehetővé teszi a Microsoft Azure által biztosított JDKs listájából válasszon ki.</span><span class="sxs-lookup"><span data-stu-id="9ad60-176">**Deploy a 3rd party JDK available on Azure**: This option allows you to choose from the list of JDKs which are provided by Microsoft Azure.</span></span>
        * <span data-ttu-id="9ad60-177">**A letöltési helyről saját JDK telepítése**: Ezzel a beállítással megadhatja a saját JDK terjesztési, amely kell csomagolni csomagot .zip fájlként, és feltölti a nyilvánosan elérhető letöltési hely vagy egy Azure storage-fiókot, amely rendelkezik a hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="9ad60-177">**Deploy my own JDK from this download location**: This option allows you to specify your own JDK distribution, which must be packaged as a ZIP file and uploaded to either a publicly available download location or an Azure storage account for which you have access.</span></span>
          
          ![Új webes alkalmazás tároló párbeszédpanel][07b]
   7. <span data-ttu-id="9ad60-179">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ad60-179">Click **OK**.</span></span>
   8. <span data-ttu-id="9ad60-180">A **App Service-csomag** legördülő menü mutatja az erőforráscsoport kiválasztott társított app service-csomagokról.</span><span class="sxs-lookup"><span data-stu-id="9ad60-180">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="9ad60-181">(App Service-csomagokról a webalkalmazás, az árképzési szint és a számítási példányméretének például a hely információkat adják meg.</span><span class="sxs-lookup"><span data-stu-id="9ad60-181">(App Service Plans specify information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="9ad60-182">Egy egyetlen App Service-csomag használható több Web Apps, ezért azt egy adott webalkalmazás telepítésből külön megmarad.)</span><span class="sxs-lookup"><span data-stu-id="9ad60-182">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       <span data-ttu-id="9ad60-183">Válasszon ki egy meglévő App Service-csomag (ha vannak ilyenek) és skip h az alábbi lépések, vagy használja az alábbi lépéseket egy új App Service-csomag létrehozása a következő:</span><span class="sxs-lookup"><span data-stu-id="9ad60-183">You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following these steps to create a new App Service Plan:</span></span>
      
      * <span data-ttu-id="9ad60-184">Kattintson a **új...**</span><span class="sxs-lookup"><span data-stu-id="9ad60-184">Click **New...**</span></span>
      * <span data-ttu-id="9ad60-185">A **új App Service-csomag** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="9ad60-185">The **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Új App Service-csomag párbeszédpanel][09]
      * <span data-ttu-id="9ad60-187">Az a a **neve** szövegmező, adjon meg egy nevet az új App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="9ad60-187">In the the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="9ad60-188">Az a a **hely** legördülő menüben válassza a megfelelő Azure adatközpont helye a terv.</span><span class="sxs-lookup"><span data-stu-id="9ad60-188">In the the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="9ad60-189">Az a a **Tarifacsomagot** legördülő menüben válassza ki a megfelelő árképzési séma.</span><span class="sxs-lookup"><span data-stu-id="9ad60-189">In the the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="9ad60-190">Tesztelési célokra választhat **szabad**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-190">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="9ad60-191">Az a a **Példányméretének** legördülő menüben válassza a terv méretezés a megfelelő példányt.</span><span class="sxs-lookup"><span data-stu-id="9ad60-191">In the the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="9ad60-192">Tesztelési célokra választhat **kis**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-192">For testing purposes you can choose **Small**.</span></span>
   9. <span data-ttu-id="9ad60-193">A fenti lépés befejeződött, a webes alkalmazás új tároló párbeszédpanelen a következő ábra kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="9ad60-193">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
       ![Új webes alkalmazás tároló párbeszédpanel][10]
   10. <span data-ttu-id="9ad60-195">Kattintson a **OK** az új webalkalmazáshoz tároló létrehozásának befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="9ad60-195">Click **OK** to complete the creation of your new Web App container.</span></span>
       
        <span data-ttu-id="9ad60-196">Várjon néhány másodpercet, frissíteni kell a webalkalmazás-tárolók listáját, és az újonnan létrehozott webes alkalmazás tároló most már ki a listában.</span><span class="sxs-lookup"><span data-stu-id="9ad60-196">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>
7. <span data-ttu-id="9ad60-197">Most már készen áll az első Azure webalkalmazás üzembe befejezéséhez:</span><span class="sxs-lookup"><span data-stu-id="9ad60-197">You are now ready to complete the initial deployment of your Web App to Azure:</span></span>
   
    ![Azure Web App tároló párbeszédpanel telepítése][11]
   
    <span data-ttu-id="9ad60-199">Kattintson a **OK** a kijelölt webalkalmazás tárolóhoz a Java-alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="9ad60-199">Click **OK** to deploy your Java application to the selected Web App container.</span></span>
   
    <span data-ttu-id="9ad60-200">Alapértelmezés szerint az alkalmazás telepítve, a kiszolgáló alkönyvtára lehet.</span><span class="sxs-lookup"><span data-stu-id="9ad60-200">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="9ad60-201">Ha azt szeretné, hogy a legfelső szintű alkalmazás telepíthető központilag, ellenőrizze a **legfelső szintű telepítés** való kattintás előtt jelölőnégyzet **OK**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-201">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>
8. <span data-ttu-id="9ad60-202">Ezt követően kell megjelennie a **Azure tevékenységnapló** nézet, ami a webalkalmazás központi telepítési állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="9ad60-202">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
    ![Azure tevékenységnapló][12]
   
    <span data-ttu-id="9ad60-204">A webalkalmazás az Azure-bA üzembe helyezésének folyamata gyorsabban csak néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="9ad60-204">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="9ad60-205">Ha alkalmazás készen áll, megjelenik egy hivatkozás nevű **közzétett** a a **állapot** oszlop.</span><span class="sxs-lookup"><span data-stu-id="9ad60-205">When your application ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="9ad60-206">Amikor a hivatkozásra kattint, a telepített webalkalmazás kezdőlapra eltarthat meg.</span><span class="sxs-lookup"><span data-stu-id="9ad60-206">When you click the link, it will take you to your deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="9ad60-207">Webalkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="9ad60-207">Updating your web app</span></span>
<span data-ttu-id="9ad60-208">Frissítése egy meglévő Azure-webalkalmazás fut egy gyors és egyszerű eljárás, és frissítési lehetőségei vannak:</span><span class="sxs-lookup"><span data-stu-id="9ad60-208">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="9ad60-209">Java-webalkalmazás telepítése frissítheti.</span><span class="sxs-lookup"><span data-stu-id="9ad60-209">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="9ad60-210">Egy webes alkalmazás tárolóhoz további Java-alkalmazást is közzétehet.</span><span class="sxs-lookup"><span data-stu-id="9ad60-210">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="9ad60-211">Mindkét esetben a folyamat megegyezik, és csupán néhány másodpercet vesz igénybe:</span><span class="sxs-lookup"><span data-stu-id="9ad60-211">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="9ad60-212">Az Eclipse project explorer kattintson a jobb gombbal a Java-alkalmazást szeretne frissíteni, vagy meglévő Web App tároló hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="9ad60-212">In the Eclipse project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>
2. <span data-ttu-id="9ad60-213">Ha a helyi menü megjelenik, válassza ki a **Azure** , majd **Azure Web Apps közzététel...**</span><span class="sxs-lookup"><span data-stu-id="9ad60-213">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="9ad60-214">Már már bejelentkezett korábban, mivel látni fogja a meglévő Web App tárolók listája.</span><span class="sxs-lookup"><span data-stu-id="9ad60-214">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="9ad60-215">Jelölje ki a közzétenni vagy újra közzé a Java-alkalmazást, és kattintson a kívánt **OK**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-215">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="9ad60-216">Néhány másodperc múlva, a **Azure tevékenységnapló** nézetben megjelenik, a frissített telepítési **közzétett** és nem fogja tudni ellenőrizni a frissített alkalmazást egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="9ad60-216">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="9ad60-217">Indítása, leállítása vagy egy már meglévő webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="9ad60-217">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="9ad60-218">Indítsa el, vagy állítsa le a meglévő Azure Web Apps tároló, (beleértve az összes telepített Java-alkalmazások azt), használja a **Azure Explorer** nézet.</span><span class="sxs-lookup"><span data-stu-id="9ad60-218">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="9ad60-219">Ha a **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **ablak** menü az eclipse-ben, majd kattintson a **nézet megjelenítése**, majd **más...** , majd **Azure**, és kattintson a **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-219">If the **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="9ad60-220">Ha korábban nem jelentkezett be, akkor arra fogja kérni teszi.</span><span class="sxs-lookup"><span data-stu-id="9ad60-220">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="9ad60-221">Ha a **Azure Explorer** nézet jelenik meg, használjon kövesse az alábbi lépéseket indítása vagy leállítása a webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="9ad60-221">When the **Azure Explorer** view is displayed, use follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="9ad60-222">Bontsa ki a **Azure** csomópont.</span><span class="sxs-lookup"><span data-stu-id="9ad60-222">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="9ad60-223">Bontsa ki a **webalkalmazások** csomópont.</span><span class="sxs-lookup"><span data-stu-id="9ad60-223">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="9ad60-224">Kattintson a jobb gombbal a kívánt webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="9ad60-224">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="9ad60-225">Amikor a helyi menü megjelenik, kattintson a **Start**, **leállítása**, vagy **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="9ad60-225">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="9ad60-226">Vegye figyelembe, hogy a menüjéből választások környezet használatát, így csak a futó webalkalmazás leállítása vagy egy webalkalmazást, ami jelenleg nem fut. Indítsa el.</span><span class="sxs-lookup"><span data-stu-id="9ad60-226">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
    ![Egy már meglévő webalkalmazás leállítása][13]

## <a name="next-steps"></a><span data-ttu-id="9ad60-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ad60-228">Next Steps</span></span>
<span data-ttu-id="9ad60-229">A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="9ad60-229">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="9ad60-230">[Eclipse-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="9ad60-230">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="9ad60-231">[az eclipse-ben az Azure eszközkészlet telepítésével]</span><span class="sxs-lookup"><span data-stu-id="9ad60-231">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="9ad60-232">*Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben (Ez a cikk)*</span><span class="sxs-lookup"><span data-stu-id="9ad60-232">*Create a Hello World Web App for Azure in Eclipse (This Article)*</span></span>
  * <span data-ttu-id="9ad60-233">[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="9ad60-233">[What's New in the Azure Toolkit for Eclipse]</span></span>
* <span data-ttu-id="9ad60-234">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="9ad60-234">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="9ad60-235">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="9ad60-235">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="9ad60-236">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="9ad60-236">[Create a Hello World Web App for Azure in IntelliJ]</span></span>
  * <span data-ttu-id="9ad60-237">[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="9ad60-237">[What's New in the Azure Toolkit for IntelliJ]</span></span>

<span data-ttu-id="9ad60-238">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="9ad60-238">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<span data-ttu-id="9ad60-239">Azure Web Apps létrehozásával kapcsolatos további információkért lásd: a [webes alkalmazások – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="9ad60-239">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

<span data-ttu-id="9ad60-240">[Eclipse-hez készült Azure-eszközkészlet]: ../azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="9ad60-240">[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="9ad60-241">[Az IntelliJ-hez készült Azure-eszközkészlet]: ../azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="9ad60-241">[Azure Toolkit for IntelliJ]: ../azure-toolkit-for-intellij.md</span></span>
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
<span data-ttu-id="9ad60-242">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="9ad60-242">[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="9ad60-243">[az eclipse-ben az Azure eszközkészlet telepítésével]: ../azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="9ad60-243">[Installing the Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="9ad60-244">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ../azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="9ad60-244">[Installing the Azure Toolkit for IntelliJ]: ../azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="9ad60-245">[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ../azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="9ad60-245">[What's New in the Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="9ad60-246">[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ../azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="9ad60-246">[What's New in the Azure Toolkit for IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="9ad60-247">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="9ad60-247">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="9ad60-248">[webes alkalmazások – áttekintés]: ./app-service-web-overview.md</span><span class="sxs-lookup"><span data-stu-id="9ad60-248">[Web Apps Overview]: ./app-service-web-overview.md</span></span>
<span data-ttu-id="9ad60-249">[Apache Tomcat]: http://tomcat.apache.org/</span><span class="sxs-lookup"><span data-stu-id="9ad60-249">[Apache Tomcat]: http://tomcat.apache.org/</span></span>
<span data-ttu-id="9ad60-250">[Jetty]: http://www.eclipse.org/jetty/</span><span class="sxs-lookup"><span data-stu-id="9ad60-250">[Jetty]: http://www.eclipse.org/jetty/</span></span>

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
