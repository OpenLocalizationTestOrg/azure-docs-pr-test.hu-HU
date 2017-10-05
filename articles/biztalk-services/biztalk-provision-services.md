---
title: "Azure BizTalk Services létrehozása az Azure Portalon | Microsoft Docs"
description: "Megtudhatja, hogyan építheti ki vagy hozhatja létre az Azure BizTalk Servicest az Azure Portalon: MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: eca77b4a82eb67e1755717bb4429f8d450a64dc5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-biztalk-services-using-the-azure-portal"></a><span data-ttu-id="c2850-103">BizTalk Services létrehozása az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="c2850-103">Create BizTalk Services using the Azure portal</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]


> [!TIP]
> <span data-ttu-id="c2850-104">Az Azure Portalra való bejelentkezéshez Azure-fiókra és Azure-előfizetésre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c2850-104">To sign in to the Azure portal, you need an Azure account and Azure subscription.</span></span> <span data-ttu-id="c2850-105">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="c2850-105">If you don't have an account, you can create a free trial account within a few minutes.</span></span> <span data-ttu-id="c2850-106">Lásd: [Ingyenes Azure-próbafiók](http://go.microsoft.com/fwlink/p/?LinkID=239738).</span><span class="sxs-lookup"><span data-stu-id="c2850-106">See [Azure Free Trial](http://go.microsoft.com/fwlink/p/?LinkID=239738).</span></span>


## <span data-ttu-id="c2850-107"><a name="CreateService"></a>BizTalk-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2850-107"><a name="CreateService"></a>Create a BizTalk Service</span></span>
<span data-ttu-id="c2850-108">A kiválasztott kiadástól függően lehet, hogy nem mindegyik BizTalk-szolgáltatásbeállítás érhető el.</span><span class="sxs-lookup"><span data-stu-id="c2850-108">Depending on the Edition you choose, not all BizTalk Service settings may be available.</span></span>

1. <span data-ttu-id="c2850-109">Jelentkezzen be az [Azure portálra](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="c2850-109">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="c2850-110">A navigációs ablak alján kattintson a **NEW** (ÚJ) elemre:</span><span class="sxs-lookup"><span data-stu-id="c2850-110">In the bottom navigation pane, select **NEW**:</span></span>  
   <span data-ttu-id="c2850-111">![Válassza a New (Új) gombot][NEWButton]</span><span class="sxs-lookup"><span data-stu-id="c2850-111">![Select the New button][NEWButton]</span></span>
3. <span data-ttu-id="c2850-112">Válassza az **APP SERVICES** (ALKALMAZÁSSZOLGÁLTATÁSOK) > **BIZTALK SERVICE** > **CUSTOM CREATE** (EGYÉNI LÉTREHOZÁS) elemet:</span><span class="sxs-lookup"><span data-stu-id="c2850-112">Select **APP SERVICES** > **BIZTALK SERVICE** > **CUSTOM CREATE**:</span></span>  
   <span data-ttu-id="c2850-113">![Válassza a BizTalk Service elemet, majd a Custom Create (Egyéni létrehozás) elemet][NewBizTalkService]</span><span class="sxs-lookup"><span data-stu-id="c2850-113">![Select BizTalk Service and select Custom Create][NewBizTalkService]</span></span>
4. <span data-ttu-id="c2850-114">Adja meg a BizTalk-szolgáltatásbeállításokat:</span><span class="sxs-lookup"><span data-stu-id="c2850-114">Enter the BizTalk Service settings:</span></span>
   
    <table border="1">
    <tr>
    <td><span data-ttu-id="c2850-115"><strong>BizTalk-szolgáltatás neve</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-115"><strong>BizTalk service name</strong></span></span></td>
    <td><span data-ttu-id="c2850-116">Bármilyen nevet megadhat, de használjon konkrét nevet.</span><span class="sxs-lookup"><span data-stu-id="c2850-116">You can enter any name but be specific.</span></span> <span data-ttu-id="c2850-117">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="c2850-117">Some examples include:</span></span><br/><br/><span data-ttu-id="c2850-118">
    <em>mycompany</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="c2850-118">
    <em>mycompany</em>.biztalk.windows.net</span></span><br/><span data-ttu-id="c2850-119">
    <em>mycompanymyapplication</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="c2850-119">
    <em>mycompanymyapplication</em>.biztalk.windows.net</span></span><br/><span data-ttu-id="c2850-120">
    <em>myapplication</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="c2850-120">
    <em>myapplication</em>.biztalk.windows.net</span></span><br/><br/><span data-ttu-id="c2850-121">A „.biztalk.windows.net” automatikusan hozzá lesz adva megadott névhez.</span><span class="sxs-lookup"><span data-stu-id="c2850-121">".biztalk.windows.net" is automatically added to the name you enter.</span></span> <span data-ttu-id="c2850-122">Ez olyan URL-t hoz létre, amellyel elérheti a BizTalk-szolgáltatást (például <strong>https://<em>myapplication</em>.biztalk.windows.net</strong>).</span><span class="sxs-lookup"><span data-stu-id="c2850-122">This creates a URL that is used to access your BizTalk Service, like <strong>https://<em>myapplication</em>.biztalk.windows.net</strong>.</span></span>
    </td>
    </tr>
    <tr>
    <td><span data-ttu-id="c2850-123"><strong>Kiadás</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-123"><strong>Edition</strong></span></span></td>
    <td><span data-ttu-id="c2850-124">Ha a tesztelési/fejlesztési fázisban van, válassza a <strong>Developer</strong> elemet.</span><span class="sxs-lookup"><span data-stu-id="c2850-124">If you are in the testing/development phase, choose <strong>Developer</strong>.</span></span> <span data-ttu-id="c2850-125">Ha a termelési fázisban van, a <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: Kiadások diagramja</a> szakasz alapján határozza meg, hogy a <strong>Premium</strong>, <strong>Standard</strong> vagy <strong>Alapszintű</strong> a legmegfelelőbb választás az üzleti forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="c2850-125">If you are in the production phase, use the <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: Editions Chart</a> to determine if <strong>Premium</strong>, <strong>Standard</strong>, or <strong>Basic</strong> is the correct choice for your business scenario.</span></span>
    </td>
    </tr>
    <tr>
    <td><span data-ttu-id="c2850-126"><strong>Régió</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-126"><strong>Region</strong></span></span></td>
    <td><span data-ttu-id="c2850-127">Válassza ki a BizTalk-szolgáltatás földrajzi helyét.</span><span class="sxs-lookup"><span data-stu-id="c2850-127">Select the geographic region to host your BizTalk Service.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c2850-128"><strong>Tartomány URL-je</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-128"><strong>Domain URL</strong></span></span></td>
    <td><span data-ttu-id="c2850-129"><strong>Választható</strong>.</span><span class="sxs-lookup"><span data-stu-id="c2850-129"><strong>Optional</strong>.</span></span> <span data-ttu-id="c2850-130">Alapértelmezés szerint a tartomány URL-je a <em>SajázBizTalkSzolgáltatásNeve</em>.biztalk.windows.net.</span><span class="sxs-lookup"><span data-stu-id="c2850-130">By default, the domain URL is <em>YourBizTalkServiceName</em>.biztalk.windows.net.</span></span> <span data-ttu-id="c2850-131">Egyéni tartomány is beírható.</span><span class="sxs-lookup"><span data-stu-id="c2850-131">A custom domain can also be entered.</span></span> <span data-ttu-id="c2850-132">Ha a tartománya például a <em>contoso</em>, beírhatja a következőt:</span><span class="sxs-lookup"><span data-stu-id="c2850-132">For example, if your domain is <em>contoso</em>, you can enter:</span></span> <br/><br/><span data-ttu-id="c2850-133">
    <em>Sajátvállalat</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c2850-133">
    <em>MyCompany</em>.contoso.com</span></span><br/><span data-ttu-id="c2850-134">
    <em>SajátVállalatSajátAlkalmazás</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c2850-134">
    <em>MyCompanyMyApplication</em>.contoso.com</span></span><br/><span data-ttu-id="c2850-135">
    <em>SajátAlkalmazás</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c2850-135">
    <em>MyApplication</em>.contoso.com</span></span><br/><span data-ttu-id="c2850-136">
    <em>BizTalkSzolgáltatásNeve</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c2850-136">
    <em>YourBizTalkServiceName</em>.contoso.com</span></span><br/>
    </td>
    </tr>
    </table>
<span data-ttu-id="c2850-137">Válassza a TOVÁBB nyilat.</span><span class="sxs-lookup"><span data-stu-id="c2850-137">Select the NEXT arrow.</span></span>
<span data-ttu-id="c2850-138">5.</span><span class="sxs-lookup"><span data-stu-id="c2850-138">5.</span></span> <span data-ttu-id="c2850-139">Adja meg a tároló és az adatbázis beállításait:</span><span class="sxs-lookup"><span data-stu-id="c2850-139">Enter the Storage and Database Settings:</span></span>  <table border="1">
    <tr>
    <td><span data-ttu-id="c2850-140"><strong>Megfigyelő/archiváló tárfiók</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-140"><strong>Monitoring/Archiving storage account</strong></span></span></td>
    <td><span data-ttu-id="c2850-141">Válasszon meglévő Storage-fiókot, vagy hozzon létre egy új Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="c2850-141">Select an existing storage account or create a new storage account.</span></span> <br/><br/><span data-ttu-id="c2850-142">Ha új Storage-fiókot hoz létre, adja meg a <strong>Storage-fiók nevét</strong>.</span><span class="sxs-lookup"><span data-stu-id="c2850-142">If you create a new Storage account, enter the <strong>Storage Account Name</strong>.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c2850-143"><strong>Nyomkövető adatbázis</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-143"><strong>Tracking database</strong></span></span></td>
    <td><span data-ttu-id="c2850-144">Ha meglévő Azure SQL Database-adatbázist használ, azt nem használhatja másik BizTalk-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c2850-144">If you use an existing Azure SQL Database, it cannot be used by another BizTalk Service.</span></span> <span data-ttu-id="c2850-145">Meg kell adnia az Azure SQL Database Server létrehozásakor megadott bejelentkezési nevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="c2850-145">You need the login name and password entered when that Azure SQL Database Server was created.</span></span><br/><br/><span data-ttu-id="c2850-146"><strong>TIPP</strong> A nyomkövető adatbázist és a megfigyelő/archiváló tárfiókot ugyanabban a régióban hozza létre, mint a BizTalk-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c2850-146"><strong>TIP</strong> Create the Tracking database and Monitoring/Archiving storage account in the same region as the BizTalk Service.</span></span></td>
    </tr>
    </table>
<span data-ttu-id="c2850-147">Válassza a TOVÁBB nyilat.</span><span class="sxs-lookup"><span data-stu-id="c2850-147">Select the NEXT arrow.</span></span>
<span data-ttu-id="c2850-148">6.</span><span class="sxs-lookup"><span data-stu-id="c2850-148">6.</span></span> <span data-ttu-id="c2850-149">Adja meg az adatbázis beállításait:</span><span class="sxs-lookup"><span data-stu-id="c2850-149">Enter the Database settings:</span></span>  <table border="1">
    <tr>
    <td><span data-ttu-id="c2850-150"><strong>Name (Név)</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-150"><strong>Name</strong></span></span></td>
    <td><span data-ttu-id="c2850-151">Akkor érhető el, ha az előző képernyőn az <strong>Új SQL Database-példány létrehozását</strong> választotta.</span><span class="sxs-lookup"><span data-stu-id="c2850-151">Available when <strong>Create a new SQL Database instance</strong> is selected in the previous screen.</span></span>
    <br/><br/>
<span data-ttu-id="c2850-152">Írja be a BizTalk-szolgáltatás által használandó SQL Database-adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="c2850-152">Enter a SQL Database name to be used by your BizTalk Service.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c2850-153"><strong>Kiszolgáló</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-153"><strong>Server</strong></span></span></td>
    <td><span data-ttu-id="c2850-154">Akkor érhető el, ha az előző képernyőn az <strong>Új SQL Database-példány létrehozását</strong> választotta.</span><span class="sxs-lookup"><span data-stu-id="c2850-154">Available when <strong>Create a new SQL Database instance</strong> is selected in the previous screen.</span></span>
    <br/><br/>
<span data-ttu-id="c2850-155">Válasszon egy meglévő SQL Database-kiszolgálót vagy hozzon létre új SQL Database-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="c2850-155">Select an existing SQL Database Server or create a new SQL Database server.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c2850-156"><strong>Kiszolgáló bejelentkezési neve</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-156"><strong>Server login name</strong></span></span></td>
    <td><span data-ttu-id="c2850-157">Adja meg a bejelentkezési felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="c2850-157">Enter the login user name.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c2850-158"><strong>Kiszolgáló bejelentkezési jelszava</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-158"><strong>Server login password</strong></span></span></td>
    <td><span data-ttu-id="c2850-159">Adja meg a bejelentkezési jelszót.</span><span class="sxs-lookup"><span data-stu-id="c2850-159">Enter the login password.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c2850-160"><strong>Régió</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-160"><strong>Region</strong></span></span></td>
    <td><span data-ttu-id="c2850-161">Akkor érhető el, ha az <strong>Új SQL Database-példány létrehozását</strong> választotta.</span><span class="sxs-lookup"><span data-stu-id="c2850-161">Available when <strong>Create a new SQL Database instance</strong> is selected.</span></span> <span data-ttu-id="c2850-162">Válassza ki az SQL Database földrajzi helyét.</span><span class="sxs-lookup"><span data-stu-id="c2850-162">Select the geographic region to host your SQL Database.</span></span></td>
    </tr>
    </table>

<span data-ttu-id="c2850-163">Kattintson a pipára a varázsló befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="c2850-163">Select the check mark to complete the wizard.</span></span> <span data-ttu-id="c2850-164">Megjelenik az állapotjelző ikon:</span><span class="sxs-lookup"><span data-stu-id="c2850-164">The progress icon appears:</span></span>  
![Az állapotjelző ikon megjelenik, ha befejeződött][ProgressComplete]

<span data-ttu-id="c2850-166">Amikor elkészült, létrejön az Azure BizTalk-szolgáltatás, és készen áll az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c2850-166">When complete, the Azure BizTalk Service is created and ready for your applications.</span></span> <span data-ttu-id="c2850-167">Az alapértelmezett beállítások elegendőek.</span><span class="sxs-lookup"><span data-stu-id="c2850-167">The default settings are sufficient.</span></span> <span data-ttu-id="c2850-168">Ha módosítani kívánja az alapértelmezett beállításokat, válassza a bal oldali navigációs panelen a **BIZTALK SERVICES** elemet, majd válassza ki a BizTalk-szolgáltatását.</span><span class="sxs-lookup"><span data-stu-id="c2850-168">If you want to change the default settings, select **BIZTALK SERVICES** in the left navigation pane, and then select your BizTalk Service.</span></span> <span data-ttu-id="c2850-169">A fenti [Irányítópult, Figyelő és Méretezés lapokon](biztalk-dashboard-monitor-scale-tabs.md) további beállítások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="c2850-169">Additional settings are displayed in the [Dashboard, Monitor, and Scale tabs](biztalk-dashboard-monitor-scale-tabs.md) at the top.</span></span>

<span data-ttu-id="c2850-170">A BizTalk-szolgáltatás állapotától függően néhány művelet nem végezhető el.</span><span class="sxs-lookup"><span data-stu-id="c2850-170">Depending on the state of the BizTalk Service, there are some operations that cannot be completed.</span></span> <span data-ttu-id="c2850-171">Ezen műveletek listájáért tekintse meg a [BizTalk-szolgáltatások állapota diagramot](biztalk-service-state-chart.md).</span><span class="sxs-lookup"><span data-stu-id="c2850-171">For a list of these operations, go to [BizTalk Services State Chart](biztalk-service-state-chart.md).</span></span>

## <a name="post-provisioning-steps"></a><span data-ttu-id="c2850-172">Kiépítés utáni lépések</span><span class="sxs-lookup"><span data-stu-id="c2850-172">Post-provisioning steps</span></span>
* [<span data-ttu-id="c2850-173">A tanúsítvány telepítése helyi számítógépre</span><span class="sxs-lookup"><span data-stu-id="c2850-173">Install the certificate on a local computer</span></span>](#InstallCert)
* [<span data-ttu-id="c2850-174">Termelésre kész tanúsítvány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c2850-174">Add a production-ready certificate</span></span>](#AddCert)
* [<span data-ttu-id="c2850-175">A hozzáférés-vezérlési névtér beszerzése</span><span class="sxs-lookup"><span data-stu-id="c2850-175">Get the Access Control namespace</span></span>](#ACS)

#### <span data-ttu-id="c2850-176"><a name="InstallCert"></a>A tanúsítvány telepítése helyi számítógépre</span><span class="sxs-lookup"><span data-stu-id="c2850-176"><a name="InstallCert"></a>Install the certificate on a local computer</span></span>
<span data-ttu-id="c2850-177">A BizTalk-szolgáltatás kiépítésének részeként létrejön egy önaláírt tanúsítvány, amely társítva van a BizTalk-szolgáltatás előfizetésével.</span><span class="sxs-lookup"><span data-stu-id="c2850-177">As part of BizTalk Service provisioning, a self-signed certificate is created and associated with your BizTalk Service subscription.</span></span> <span data-ttu-id="c2850-178">Le kell töltenie és telepítenie kell ezt a tanúsítványt azokra a számítógépekre, amelyekről üzembe helyezi a BizTalk-szolgáltatás alkalmazásait, vagy amelyekről üzeneteket küld a BizTalk-szolgáltatás végpontjaira.</span><span class="sxs-lookup"><span data-stu-id="c2850-178">You must download this certificate and install it on computers from where you either deploy BizTalk Service applications or send messages to a BizTalk Service endpoint.</span></span>

1. <span data-ttu-id="c2850-179">Jelentkezzen be az [Azure portálra](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="c2850-179">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="c2850-180">Válassza a bal oldali navigációs panelen a **BIZTALK SERVICES** elemet, majd válassza ki a BizTalk-szolgáltatás előfizetését.</span><span class="sxs-lookup"><span data-stu-id="c2850-180">Select **BIZTALK SERVICES** in the left navigation pane, and then select your BizTalk Service subscription.</span></span>
3. <span data-ttu-id="c2850-181">Kattintson az **Irányítópult** fülre.</span><span class="sxs-lookup"><span data-stu-id="c2850-181">Select the **Dashboard** tab.</span></span>
4. <span data-ttu-id="c2850-182">Válassza az **SSL-tanúsítvány letöltése** elemet:</span><span class="sxs-lookup"><span data-stu-id="c2850-182">Select **Download SSL Certificate**:</span></span>  
   <span data-ttu-id="c2850-183">![SSL-tanúsítvány módosítása][QuickGlance]</span><span class="sxs-lookup"><span data-stu-id="c2850-183">![Modify SSL Certificate][QuickGlance]</span></span>
5. <span data-ttu-id="c2850-184">Kattintson duplán a tanúsítványra, és haladjon végig a varázslón a tanúsítvány telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c2850-184">Double-click the certificate and run through the wizard to install the certificate.</span></span> <span data-ttu-id="c2850-185">Győződjön meg arról, hogy a **Megbízható gyökérhitelesítő hatóságok** tárolóba telepítette a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c2850-185">Make sure you install the certificate under the **Trusted Root Certificate Authorities** store.</span></span>

#### <span data-ttu-id="c2850-186"><a name="AddCert"></a>Termelésre kész tanúsítvány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c2850-186"><a name="AddCert"></a>Add a production-ready certificate</span></span>
<span data-ttu-id="c2850-187">A BizTalk-szolgáltatások létrehozásakor automatikusan létrejött önaláírt tanúsítvány csak fejlesztési környezetekben használható.</span><span class="sxs-lookup"><span data-stu-id="c2850-187">The self-signed certificate that is automatically created when creating BizTalk Services is intended for use in development environments only.</span></span> <span data-ttu-id="c2850-188">A termelési forgatókönyvekhez cserélje le egy termelésre kész tanúsítványra.</span><span class="sxs-lookup"><span data-stu-id="c2850-188">For production scenarios, replace it with a production-ready certificate.</span></span>

1. <span data-ttu-id="c2850-189">Az **Irányítópult** lapon válassza az **SSL-tanúsítvány frissítése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c2850-189">On the **Dashboard** tab, select **Update SSL Certificate**.</span></span>
2. <span data-ttu-id="c2850-190">Keresse meg a BizTalk-szolgáltatás nevét tartalmazó személyes SSL-tanúsítványt (*CertificateName*.pfx), írja be a jelszót, majd kattintson a pipára.</span><span class="sxs-lookup"><span data-stu-id="c2850-190">Browse to your private SSL certificate (*CertificateName*.pfx) that includes your BizTalk Service name, enter the password, and then click the check mark.</span></span>

#### <span data-ttu-id="c2850-191"><a name="ACS"></a>A hozzáférés-vezérlési névtér beszerzése</span><span class="sxs-lookup"><span data-stu-id="c2850-191"><a name="ACS"></a>Get the Access Control namespace</span></span>
1. <span data-ttu-id="c2850-192">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="c2850-192">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="c2850-193">Válassza a bal oldali navigációs panelen a **BIZTALK SERVICES** elemet, majd válassza ki a BizTalk-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c2850-193">Select **BIZTALK SERVICES** in the left navigation pane, and then select your BizTalk Service.</span></span>
3. <span data-ttu-id="c2850-194">A tálcán válassza a **Connection Information** (Kapcsolódási adatok) lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="c2850-194">In the task bar, select **Connection Information**:</span></span>  
   <span data-ttu-id="c2850-195">![Kattintson a Connection Information (Kapcsolatadatok) elemre.][ACSConnectInfo]</span><span class="sxs-lookup"><span data-stu-id="c2850-195">![Select Connection Information][ACSConnectInfo]</span></span>
4. <span data-ttu-id="c2850-196">Másolja a hozzáférés-vezérlési értékeket.</span><span class="sxs-lookup"><span data-stu-id="c2850-196">Copy the Access Control values.</span></span>

<span data-ttu-id="c2850-197">Amikor BizTalk Service-projektet helyez üzembe a Visual Studióból, ezt a hozzáférés-vezérlési névteret írja be.</span><span class="sxs-lookup"><span data-stu-id="c2850-197">When you deploy a BizTalk Service project from Visual Studio, you enter this Access Control namespace.</span></span> <span data-ttu-id="c2850-198">A hozzáférés-vezérlési névtér automatikusan létrejön a BizTalk Services-szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c2850-198">The Access Control namespace is automatically created for your BizTalk Service.</span></span>

<span data-ttu-id="c2850-199">A hozzáférés-vezérlési értékeket bármilyen alkalmazással használhatja.</span><span class="sxs-lookup"><span data-stu-id="c2850-199">The Access Control values can be used with any application.</span></span> <span data-ttu-id="c2850-200">Az Azure BizTalk-szolgáltatások létrehozásakor ez a hozzáférés-vezérlési névtér vezérli a BizTalk Services-szolgáltatás üzembe helyezésekor végzett hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="c2850-200">When Azure BizTalk Services is created, this Access Control namespace controls the authentication with your BizTalk Service deployment.</span></span> <span data-ttu-id="c2850-201">Az előfizetés módosításához vagy a névtér felügyeletéhez válassza a bal oldali navigációs panelen az **ACTIVE DIRECTORY** elemet, majd válassza ki a névteret.</span><span class="sxs-lookup"><span data-stu-id="c2850-201">If you want to change the subscription or manage the namespace, select **ACTIVE DIRECTORY** in the left navigation pane and then select your namespace.</span></span> <span data-ttu-id="c2850-202">A tálca felsorolja a lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="c2850-202">The task bar lists your options.</span></span>

<span data-ttu-id="c2850-203">A **Manage** (Felügyelet) gombra kattintva megnyitja a hozzáférés-vezérlési felügyeleti portált.</span><span class="sxs-lookup"><span data-stu-id="c2850-203">Clicking **Manage** opens the Access Control Management Portal.</span></span> <span data-ttu-id="c2850-204">A hozzáférés-vezérlési felügyeleti portálon a BizTalk Services-szolgáltatás **szolgáltatásidentitásokat** használ:</span><span class="sxs-lookup"><span data-stu-id="c2850-204">In the Access Control Management Portal, the BizTalk Service uses **Service identities**:</span></span>  
<span data-ttu-id="c2850-205">![ACS-szolgáltatásidentitások a hozzáférés-vezérlési felügyeleti portálon][ACSServiceIdentities]</span><span class="sxs-lookup"><span data-stu-id="c2850-205">![ACS Service Identities in the Access Control Management Portal][ACSServiceIdentities]</span></span>

<span data-ttu-id="c2850-206">A hozzáférés-vezérlési szolgáltatásidentitás olyan hitelesítő adatok készlete, amelyekkel az alkalmazások vagy ügyfelek közvetlenül hitelesíthetnek a hozzáférés-vezérléssel, és tokent kaphatnak.</span><span class="sxs-lookup"><span data-stu-id="c2850-206">The Access Control service identity is a set of credentials that allow applications or clients to authenticate directly with Access Control and receive a token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2850-207">A BizTalk Services-szolgáltatás a **Tulajdonost** használja az alapértelmezett szolgáltatásidentitáshoz és a **Jelszó** értékéhez.</span><span class="sxs-lookup"><span data-stu-id="c2850-207">The BizTalk Service uses **Owner** for the default service identity and the **Password** value.</span></span> <span data-ttu-id="c2850-208">Ha a Szimmetrikus kulcs értéket használja a Jelszó érték helyett, a következő hiba fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="c2850-208">If you use the Symmetric Key value instead of the Password value, the following error may occur.</span></span><br/><br/><span data-ttu-id="c2850-209">*Nem lehet kapcsolódni a hozzáférés-vezérlési felügyeleti szolgáltatásfiókhoz a megadott hitelesítő adatokkal*</span><span class="sxs-lookup"><span data-stu-id="c2850-209">*Could not connect to the Access Control Management Service account with the specified credentials*</span></span>
> 
> 

<span data-ttu-id="c2850-210">[Az ACS-névtér felügyeletét](https://msdn.microsoft.com/library/azure/hh674478.aspx) ismertető dokumentum tartalmaz néhány útmutatást és javaslatot.</span><span class="sxs-lookup"><span data-stu-id="c2850-210">[Managing Your ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) lists some guidelines and recommendations.</span></span>

## <a name="requirements-explained"></a><span data-ttu-id="c2850-211">A követelmények részletei</span><span class="sxs-lookup"><span data-stu-id="c2850-211">Requirements explained</span></span>
<span data-ttu-id="c2850-212">Ezek a követelmények nem érvényesek az ingyenes kiadásra.</span><span class="sxs-lookup"><span data-stu-id="c2850-212">These requirements do not apply to the Free Edition.</span></span>

<table border="1">
<tr bgcolor="FAF9F9">
        <td><span data-ttu-id="c2850-213"><strong>Mi szükséges</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-213"><strong>What you need</strong></span></span></td>
        <td><span data-ttu-id="c2850-214"><strong>Miért szükséges</strong></span><span class="sxs-lookup"><span data-stu-id="c2850-214"><strong>Why you need it</strong></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c2850-215">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="c2850-215">Azure subscription</span></span></td>
<td><span data-ttu-id="c2850-216">Az előfizetés határozza meg, hogy ki jelentkezhet be az Azure Portalra.</span><span class="sxs-lookup"><span data-stu-id="c2850-216">The subscription determines who can sign in to the Azure portal.</span></span> <span data-ttu-id="c2850-217">A fióktulajdonos hozza létre az előfizetést az <a HREF="https://account.windowsazure.com/Subscriptions"> Azure-előfizetésekben</a>.</span><span class="sxs-lookup"><span data-stu-id="c2850-217">The Account holder creates the subscription at <a HREF="https://account.windowsazure.com/Subscriptions"> Azure Subscriptions</a>.</span></span>
<br/><br/>
<span data-ttu-id="c2850-218">Az Azure-fiók több előfizetéssel rendelkezhet, és bárki felügyelheti, aki erre jogosult.</span><span class="sxs-lookup"><span data-stu-id="c2850-218">The Azure account can have multiple subscriptions and can be managed by anyone who is permitted.</span></span> <span data-ttu-id="c2850-219">Az Azure-fióktulajdonos például létrehozhat egy <em>BizTalkServiceSubscription</em> nevű előfizetést, és hozzáférést adhat a vállalaton (például ContosoBTSAdmins@live.com) belüli BizTalk-rendszergazdáknak az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c2850-219">For example, your Azure account holder creates a subscription named <em>BizTalkServiceSubscription</em> and gives the BizTalk Administrators within your company (for example, ContosoBTSAdmins@live.com) access to this subscription.</span></span> <span data-ttu-id="c2850-220">Ebben a forgatókönyvben a BizTalk-rendszergazdák bejelentkeznek az Azure Portalra, és teljes rendszergazdai jogosultságokkal rendelkeznek az előfizetés összes üzemeltetett szolgáltatásához, beleértve az Azure BizTalk-szolgáltatásokat is.</span><span class="sxs-lookup"><span data-stu-id="c2850-220">In this scenario, the BizTalk Administrators sign in to the Azure portal and have full Administrator rights to all the hosted services in the subscription, including Azure BizTalk Services.</span></span> <span data-ttu-id="c2850-221">A BizTalk-rendszergazdák nem az Azure-fiók tulajdonosai, és ezért nem érik el a számlázási információkat.</span><span class="sxs-lookup"><span data-stu-id="c2850-221">The BizTalk Administrators are not the Azure account holders and therefore don't have access to any billing information.</span></span>
<br/><br/><span data-ttu-id="c2850-222">További információt az 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">előfizetések és tárfiókok az Azure Portalon való kezelését</a> ismertető szakaszban talál.</span><span class="sxs-lookup"><span data-stu-id="c2850-222">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577"> Manage Subscriptions and Storage Accounts in the Azure portal</a> provides more information.</span></span>
</td>
</tr>
<tr>
<td><span data-ttu-id="c2850-223">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c2850-223">Azure SQL Database</span></span></td>
<td><span data-ttu-id="c2850-224">A BizTalk-szolgáltatás által használt táblázatokat, nézeteket és tárolt eljárásokat tárolja, beleértve a nyomkövető adatokat.</span><span class="sxs-lookup"><span data-stu-id="c2850-224">Stores the tables, views, and stored procedures used by the BizTalk Service, including the Tracking data.</span></span>
<br/><br/>
<span data-ttu-id="c2850-225">BizTalk-szolgáltatások létrehozásakor használhat meglévő Azure SQL Server-kiszolgálót vagy Azure SQL Database-adatbázist, vagy automatikusan létrehozhat egy új kiszolgálót vagy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="c2850-225">When you create a BizTalk Service, you can use an existing Azure SQL Server, Azure SQL Database, or automatically create a new Server or Database.</span></span>
<br/><br/>
<span data-ttu-id="c2850-226">Az SQL Database méretének konfigurálása automatikusan történik.</span><span class="sxs-lookup"><span data-stu-id="c2850-226">The SQL Database scale is automatically configured.</span></span> <span data-ttu-id="c2850-227">Az alapértelmezett méret általában elég a BizTalk-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c2850-227">Typically, the default scale is sufficient for a BizTalk Service.</span></span> <span data-ttu-id="c2850-228">A méret módosítása hatással van az árra.</span><span class="sxs-lookup"><span data-stu-id="c2850-228">Changing the scale impacts pricing.</span></span> <span data-ttu-id="c2850-229">Lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">Fiókok és számlázás az Azure SQL Database-ben</a>
</span><span class="sxs-lookup"><span data-stu-id="c2850-229">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930"> Accounts and Billing in Azure SQL Database</a>
</span></span><br/><br/><span data-ttu-id="c2850-230">
<strong>Megjegyzések</strong>
</span><span class="sxs-lookup"><span data-stu-id="c2850-230">
<strong>Notes</strong>
</span></span><br/>
<ul>
<li> <span data-ttu-id="c2850-231">Új Azure SQL Server és adatbázis létrehozásakor az Azure-szolgáltatások automatikusan engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="c2850-231">When you create a new Azure SQL Server and Database, Azure Services is automatically enabled.</span></span> <span data-ttu-id="c2850-232">A BizTalk-szolgáltatáshoz az Azure-szolgáltatásoknak engedélyezve kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="c2850-232">The BizTalk Service requires Azure Services be enabled.</span></span></li>
<li><span data-ttu-id="c2850-233">Ha új Azure SQL Database-adatbázist hoz létre meglévő Azure SQL Server -kiszolgálón, a kiszolgáló tűzfalszabályai nem változnak.</span><span class="sxs-lookup"><span data-stu-id="c2850-233">If you create a new Azure SQL Database on an existing Azure SQL Server, the firewall rules of the Server are not changed.</span></span> <span data-ttu-id="c2850-234">Ennek eredményeképpen lehet, hogy más Azure-szolgáltatások nem tudják elérni a kiszolgáló adatbázisait.</span><span class="sxs-lookup"><span data-stu-id="c2850-234">As a result, it's possible other Azure Services are not allowed access to the Server's databases.</span></span></li>
</ul>
</td>
</tr>
<tr>
<td><span data-ttu-id="c2850-235">Azure hozzáférés-vezérlési névtér</span><span class="sxs-lookup"><span data-stu-id="c2850-235">Azure Access Control namespace</span></span></td>
<td><span data-ttu-id="c2850-236">Az Azure BizTalk Services modullal végez hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="c2850-236">Authenticates with Azure BizTalk Services.</span></span> <span data-ttu-id="c2850-237">Amikor BizTalk Service-projektet helyez üzembe a Visual Studióból, ezt a hozzáférés-vezérlési névteret írja be.</span><span class="sxs-lookup"><span data-stu-id="c2850-237">When you deploy a BizTalk Service project from Visual Studio, you enter this Access Control namespace.</span></span> <span data-ttu-id="c2850-238">BizTalk Services-szolgáltatás létrehozásakor a hozzáférés-vezérlési névtér automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="c2850-238">When you create a BizTalk Service, the Access Control namespace is automatically created.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="c2850-239">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="c2850-239">Azure Storage account</span></span></td>
<td><span data-ttu-id="c2850-240">Hozzáférést nyújt a BizTalk Services-szolgáltatás által használt táblázatokhoz, blobokhoz és sorokhoz a következők mentése érdekében:</span><span class="sxs-lookup"><span data-stu-id="c2850-240">Gives access to tables, blobs, and queues used by your BizTalk Service to save the following:</span></span>

<ul>
<li><span data-ttu-id="c2850-241">A BizTalk Services-szolgáltatást figyelő naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="c2850-241">Log files that monitor the BizTalk Service.</span></span> <span data-ttu-id="c2850-242">A megfigyelés kimenete az Azure Portal **Figyelés** lapján is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="c2850-242">The monitoring output is also displayed in the **Monitoring** tab in the Azure portal.</span></span></li>
<li><span data-ttu-id="c2850-243">Amikor X12- vagy AS2-egyezményt hoz létre partnerek között, engedélyezheti az Archiválás funkciót az üzenettulajdonságok tárolása érdekében.</span><span class="sxs-lookup"><span data-stu-id="c2850-243">When creating an X12 or AS2 agreement between partners, you can enable the Archiving feature to store message properties.</span></span> <span data-ttu-id="c2850-244">Ezeket az adatokat a tárfiókba menti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="c2850-244">This data is saved in the Storage account.</span></span></li>
</ul>
<br/>
<span data-ttu-id="c2850-245">BizTalk Services-szolgáltatások létrehozásakor használhat meglévő Storage-fiókot, vagy automatikusan létrehozhat egy új Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="c2850-245">When you create a BizTalk Service, you can use an existing Storage account or automatically create a new Storage account.</span></span>
<br/><br/>
<span data-ttu-id="c2850-246">Az alapértelmezett tárolóbeállítások elegendőek a BizTalk-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c2850-246">The default Storage settings are sufficient for a BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="c2850-247">Storage-fiók létrehozásakor automatikusan létrejön egy elsődleges kulcs és egy másodlagos kulcs.</span><span class="sxs-lookup"><span data-stu-id="c2850-247">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="c2850-248">Ezek a kulcsok vezérlik a tárfiók elérését.</span><span class="sxs-lookup"><span data-stu-id="c2850-248">These Keys control access to your Storage account.</span></span> <span data-ttu-id="c2850-249">A BizTalk Services-szolgáltatás automatikusan az elsődleges kulcsot használja.</span><span class="sxs-lookup"><span data-stu-id="c2850-249">The BizTalk Service automatically uses the Primary Key.</span></span>
<br/><br/>
<span data-ttu-id="c2850-250">További információért lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">Storage</a>.</span><span class="sxs-lookup"><span data-stu-id="c2850-250">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671"> Storage</a> for more information.</span></span>
</td>
</tr>

<tr>
<td><span data-ttu-id="c2850-251">SSL személyes tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="c2850-251">SSL private certificate</span></span></td>
<td>
<span data-ttu-id="c2850-252">Azure BizTalk Services-szolgáltatások létrehozásakor létrejön a BizTalk Services-szolgáltatás nevét tartalmazó HTTPS URL.</span><span class="sxs-lookup"><span data-stu-id="c2850-252">When an Azure BizTalk Service is created, an HTTPS URL that includes your BizTalk Service name is also created.</span></span> <span data-ttu-id="c2850-253">Ez az URL automatikusan úgy van konfigurálva, hogy önaláírt, csak fejlesztési tanúsítványt használjon.</span><span class="sxs-lookup"><span data-stu-id="c2850-253">This URL is automatically configured to use a self-signed development-only certificate.</span></span> <span data-ttu-id="c2850-254">A termeléshez személyes SSL-tanúsítvány szükséges.</span><span class="sxs-lookup"><span data-stu-id="c2850-254">For production, you need a private SSL certificate.</span></span>
<br/><br/><span data-ttu-id="c2850-255">
<strong>SSL-tanúsítvánnyal kapcsolatos fontos információk</strong>

</span><span class="sxs-lookup"><span data-stu-id="c2850-255">
<strong>Important SSL Certificate Information</strong>

</span></span><ul>
<li><span data-ttu-id="c2850-256">A tanúsítvány lejárati dátumának 5 éven belül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c2850-256">The certificate expiration date must be less than 5 years.</span></span></li>
<li><span data-ttu-id="c2850-257">Minden személyes tanúsítványhoz jelszó szükséges.</span><span class="sxs-lookup"><span data-stu-id="c2850-257">All private certificates require a password.</span></span> <span data-ttu-id="c2850-258">Jegyezze meg ezt a jelszót, és ajánlott eljárásként ossza meg a rendszergazdáival.</span><span class="sxs-lookup"><span data-stu-id="c2850-258">Know this password and as a best practice, share this password with your administrators.</span></span></li>
<li><span data-ttu-id="c2850-259">A tesztelési/fejlesztési környezetekben önaláírt tanúsítványokat használnak.</span><span class="sxs-lookup"><span data-stu-id="c2850-259">Self-signed certificates are used in a test/development environment.</span></span> <span data-ttu-id="c2850-260">Önaláírt tanúsítvány használatakor importálja a tanúsítványt a személyes tanúsítványtárolójába és a megbízható legfelső szintű hitelesítésszolgáltatók tanúsítványtárolóba.</span><span class="sxs-lookup"><span data-stu-id="c2850-260">When using self-signed certificates, import the certificate to your Personal certificate store and the Trusted Root Certification Authorities certificate store.</span></span></li>
</ul>
<br/><span data-ttu-id="c2850-261">Amikor a termelési tanúsítvánnyal kapcsolatos kérést elküldi a hitelesítésszolgáltatójának, adja meg a tanúsítvány következő tulajdonságait:</span><span class="sxs-lookup"><span data-stu-id="c2850-261">When sending the production certificate request to your certification authority, give the following certificate properties:</span></span>
<br/>

<ul>
<li><span data-ttu-id="c2850-262"><strong>Kibővített kulcshasználat</strong>: Az Azure BizTalk Services legalább kiszolgálói hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="c2850-262"><strong>Enhanced Key Usage</strong>: At a minimum, Azure BizTalk Services requires Server Authentication.</span></span></li>
<li><span data-ttu-id="c2850-263"><strong>Köznapi név</strong>: Adja meg az Azure BizTalk Services-szolgáltatás URL-jének teljes tartománynevét (FQDN-jét).</span><span class="sxs-lookup"><span data-stu-id="c2850-263"><strong>Common Name</strong>: Enter the fully qualified domain name (FQDN) of your Azure BizTalk Service URL.</span></span> <span data-ttu-id="c2850-264">Lásd ezen cikk <a HREF="#CreateService">BizTalk Services-szolgáltatás létrehozása</a> szakaszát.</span><span class="sxs-lookup"><span data-stu-id="c2850-264">See <a HREF="#CreateService">Create a BizTalk Service</a> in this article.</span></span></li>
</ul>
<br/>
<span data-ttu-id="c2850-265">A BizTalk szolgáltatás létrehozása után hozzáadható egy új vagy másik tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="c2850-265">A new or different certificate can be added after the BizTalk Service is created.</span></span>
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting to any location.--->



## <a name="hybrid-connections"></a><span data-ttu-id="c2850-266">Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c2850-266">Hybrid Connections</span></span>
<span data-ttu-id="c2850-267">Azure BizTalk Servies-szolgáltatások létrehozásakor elérhető a **Hibrid kapcsolatok** lap:</span><span class="sxs-lookup"><span data-stu-id="c2850-267">When you create an Azure BizTalk Service, the **Hybrid Connections** tab is available:</span></span>

![Hibrid kapcsolatok lap][HybridConnectionTab]

<span data-ttu-id="c2850-269">A hibrid kapcsolatok az Azure-webhelyeket vagy Azure-mobilszolgáltatásokat bármely, statikus TCP-portot használó helyszíni erőforráshoz, például SQL Serverhez, MySQL-hez, HTTP Web API-khoz, mobilszolgáltatásokhoz és a legtöbb egyéni webszolgáltatáshoz csatlakoztatják.</span><span class="sxs-lookup"><span data-stu-id="c2850-269">Hybrid Connections are used to connect an Azure website or Azure mobile service to any on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, Mobile Services, and most custom Web Services.</span></span>  <span data-ttu-id="c2850-270">A hibrid kapcsolatok és a BizTalk Adapter Service nem ugyanaz.</span><span class="sxs-lookup"><span data-stu-id="c2850-270">Hybrid Connections and the BizTalk Adapter Service are different.</span></span> <span data-ttu-id="c2850-271">A BizTalk Adapter Service Azure BizTalk Services-szolgáltatásokat csatlakoztat helyszíni üzletági (LOB) rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="c2850-271">The BizTalk Adapter Service is used to connect Azure BizTalk Services to an on-premises Line of Business (LOB) system.</span></span>

 <span data-ttu-id="c2850-272">További információért lásd a [Hibrid kapcsolatok](integration-hybrid-connection-overview.md) című szakaszt, ahol a hibrid kapcsolatok létrehozásáról és felügyeletéről is olvashat.</span><span class="sxs-lookup"><span data-stu-id="c2850-272">See [Hybrid Connections](integration-hybrid-connection-overview.md) to learn more, including creating and managing Hybrid Connections.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c2850-273">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2850-273">Next steps</span></span>
<span data-ttu-id="c2850-274">Most, hogy létrejött a BizTalk Services-szolgáltatás, ismerje meg a különböző [BizTalk Services: Irányítópult, Figyelő és Méret lapokat](biztalk-dashboard-monitor-scale-tabs.md).</span><span class="sxs-lookup"><span data-stu-id="c2850-274">Now that a BizTalk Service is created, familiarize yourself with the different [BizTalk Services: Dashboard, Monitor and Scale tabs](biztalk-dashboard-monitor-scale-tabs.md).</span></span> <span data-ttu-id="c2850-275">Az Azure BizTalk-szolgáltatás készen áll az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c2850-275">Your BizTalk Service is ready for your applications.</span></span> <span data-ttu-id="c2850-276">Az alkalmazások létrehozásának megkezdéséhez ugorjon az [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197) című témakörre.</span><span class="sxs-lookup"><span data-stu-id="c2850-276">To start creating applications, go to [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="c2850-277">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c2850-277">See also</span></span>
* [<span data-ttu-id="c2850-278">BizTalk Services: Kiadások diagramja</span><span class="sxs-lookup"><span data-stu-id="c2850-278">BizTalk Services: Editions Chart</span></span>](biztalk-editions-feature-chart.md)<br/>
* [<span data-ttu-id="c2850-279">BizTalk Services: Állapottáblázat</span><span class="sxs-lookup"><span data-stu-id="c2850-279">BizTalk Services: State Chart</span></span>](biztalk-service-state-chart.md)<br/>
* [<span data-ttu-id="c2850-280">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="c2850-280">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)<br/>
* [<span data-ttu-id="c2850-281">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="c2850-281">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)<br/>
* [<span data-ttu-id="c2850-282">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="c2850-282">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)<br/>
* [<span data-ttu-id="c2850-283">Hogyan kezdhetem el az Azure BizTalk Services SDK használatát</span><span class="sxs-lookup"><span data-stu-id="c2850-283">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="c2850-284">Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c2850-284">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
