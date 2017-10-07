---
title: "az Azure-portálon hello Azure BizTalk szolgáltatások aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan tooprovision, vagy hozzon létre Azure-portálon; hello Azure BizTalk szolgáltatások MABS, WABS"
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
ms.openlocfilehash: 6781cadada8ac9c84e1fe045d2b0f995811f75b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-biztalk-services-using-hello-azure-portal"></a><span data-ttu-id="c0b87-103">Hello Azure-portál használatával BizTalk szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0b87-103">Create BizTalk Services using hello Azure portal</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]


> [!TIP]
> <span data-ttu-id="c0b87-104">az Azure-portálon toohello toosign, van szüksége egy Azure-fiókot és az Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="c0b87-104">toosign in toohello Azure portal, you need an Azure account and Azure subscription.</span></span> <span data-ttu-id="c0b87-105">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="c0b87-105">If you don't have an account, you can create a free trial account within a few minutes.</span></span> <span data-ttu-id="c0b87-106">Lásd: [Ingyenes Azure-próbafiók](http://go.microsoft.com/fwlink/p/?LinkID=239738).</span><span class="sxs-lookup"><span data-stu-id="c0b87-106">See [Azure Free Trial](http://go.microsoft.com/fwlink/p/?LinkID=239738).</span></span>


## <span data-ttu-id="c0b87-107"><a name="CreateService"></a>BizTalk-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0b87-107"><a name="CreateService"></a>Create a BizTalk Service</span></span>
<span data-ttu-id="c0b87-108">Attól függően, hogy hello kiadását választja nem minden BizTalk szolgáltatás beállítások érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c0b87-108">Depending on hello Edition you choose, not all BizTalk Service settings may be available.</span></span>

1. <span data-ttu-id="c0b87-109">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="c0b87-109">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="c0b87-110">Hello alsó navigációs ablaktáblán válassza ki **új**:</span><span class="sxs-lookup"><span data-stu-id="c0b87-110">In hello bottom navigation pane, select **NEW**:</span></span>  
   <span data-ttu-id="c0b87-111">![Hello új gomb kiválasztása][NEWButton]</span><span class="sxs-lookup"><span data-stu-id="c0b87-111">![Select hello New button][NEWButton]</span></span>
3. <span data-ttu-id="c0b87-112">Válassza az **APP SERVICES** (ALKALMAZÁSSZOLGÁLTATÁSOK) > **BIZTALK SERVICE** > **CUSTOM CREATE** (EGYÉNI LÉTREHOZÁS) elemet:</span><span class="sxs-lookup"><span data-stu-id="c0b87-112">Select **APP SERVICES** > **BIZTALK SERVICE** > **CUSTOM CREATE**:</span></span>  
   <span data-ttu-id="c0b87-113">![Válassza a BizTalk Service elemet, majd a Custom Create (Egyéni létrehozás) elemet][NewBizTalkService]</span><span class="sxs-lookup"><span data-stu-id="c0b87-113">![Select BizTalk Service and select Custom Create][NewBizTalkService]</span></span>
4. <span data-ttu-id="c0b87-114">Adja meg a hello BizTalk szolgáltatás beállításai:</span><span class="sxs-lookup"><span data-stu-id="c0b87-114">Enter hello BizTalk Service settings:</span></span>
   
    <table border="1">
    <tr>
    <td><span data-ttu-id="c0b87-115"><strong>BizTalk-szolgáltatás neve</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-115"><strong>BizTalk service name</strong></span></span></td>
    <td><span data-ttu-id="c0b87-116">Bármilyen nevet megadhat, de használjon konkrét nevet.</span><span class="sxs-lookup"><span data-stu-id="c0b87-116">You can enter any name but be specific.</span></span> <span data-ttu-id="c0b87-117">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="c0b87-117">Some examples include:</span></span><br/><br/><span data-ttu-id="c0b87-118">
    <em>mycompany</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="c0b87-118">
    <em>mycompany</em>.biztalk.windows.net</span></span><br/><span data-ttu-id="c0b87-119">
    <em>mycompanymyapplication</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="c0b87-119">
    <em>mycompanymyapplication</em>.biztalk.windows.net</span></span><br/><span data-ttu-id="c0b87-120">
    <em>myapplication</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="c0b87-120">
    <em>myapplication</em>.biztalk.windows.net</span></span><br/><br/><span data-ttu-id="c0b87-121">". biztalk.windows.net" automatikusan hozzáadott toohello neve meg.</span><span class="sxs-lookup"><span data-stu-id="c0b87-121">".biztalk.windows.net" is automatically added toohello name you enter.</span></span> <span data-ttu-id="c0b87-122">Ezzel létrehoz egy URL-címet, amely a BizTalk szolgáltatás, például használt tooaccess <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.</span><span class="sxs-lookup"><span data-stu-id="c0b87-122">This creates a URL that is used tooaccess your BizTalk Service, like <strong>https://<em>myapplication</em>.biztalk.windows.net</strong>.</span></span>
    </td>
    </tr>
    <tr>
    <td><span data-ttu-id="c0b87-123"><strong>Kiadás</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-123"><strong>Edition</strong></span></span></td>
    <td><span data-ttu-id="c0b87-124">Ha hello fejlesztési/tesztelési fázisban, kattintson az <strong>fejlesztői</strong>.</span><span class="sxs-lookup"><span data-stu-id="c0b87-124">If you are in hello testing/development phase, choose <strong>Developer</strong>.</span></span> <span data-ttu-id="c0b87-125">Ha hello gyártás szakaszban, akkor hello <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk szolgáltatások: kiadások diagram</a> toodetermine Ha <strong>prémium</strong>, <strong>szabványos</strong>, vagy <strong>Basic</strong>hello megfelelő választás az üzleti forgatókönyv szerint.</span><span class="sxs-lookup"><span data-stu-id="c0b87-125">If you are in hello production phase, use hello <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: Editions Chart</a> toodetermine if <strong>Premium</strong>, <strong>Standard</strong>, or <strong>Basic</strong> is hello correct choice for your business scenario.</span></span>
    </td>
    </tr>
    <tr>
    <td><span data-ttu-id="c0b87-126"><strong>Régió</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-126"><strong>Region</strong></span></span></td>
    <td><span data-ttu-id="c0b87-127">Válassza ki a földrajzi régióban toohost hello a BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c0b87-127">Select hello geographic region toohost your BizTalk Service.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c0b87-128"><strong>Tartomány URL-je</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-128"><strong>Domain URL</strong></span></span></td>
    <td><span data-ttu-id="c0b87-129"><strong>Választható</strong>.</span><span class="sxs-lookup"><span data-stu-id="c0b87-129"><strong>Optional</strong>.</span></span> <span data-ttu-id="c0b87-130">Alapértelmezés szerint hello tartomány URL-címe az <em>YourBizTalkServiceName</em>. biztalk.windows.net.</span><span class="sxs-lookup"><span data-stu-id="c0b87-130">By default, hello domain URL is <em>YourBizTalkServiceName</em>.biztalk.windows.net.</span></span> <span data-ttu-id="c0b87-131">Egyéni tartomány is beírható.</span><span class="sxs-lookup"><span data-stu-id="c0b87-131">A custom domain can also be entered.</span></span> <span data-ttu-id="c0b87-132">Ha a tartománya például a <em>contoso</em>, beírhatja a következőt:</span><span class="sxs-lookup"><span data-stu-id="c0b87-132">For example, if your domain is <em>contoso</em>, you can enter:</span></span> <br/><br/><span data-ttu-id="c0b87-133">
    <em>Sajátvállalat</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c0b87-133">
    <em>MyCompany</em>.contoso.com</span></span><br/><span data-ttu-id="c0b87-134">
    <em>SajátVállalatSajátAlkalmazás</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c0b87-134">
    <em>MyCompanyMyApplication</em>.contoso.com</span></span><br/><span data-ttu-id="c0b87-135">
    <em>SajátAlkalmazás</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c0b87-135">
    <em>MyApplication</em>.contoso.com</span></span><br/><span data-ttu-id="c0b87-136">
    <em>BizTalkSzolgáltatásNeve</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c0b87-136">
    <em>YourBizTalkServiceName</em>.contoso.com</span></span><br/>
    </td>
    </tr>
    </table>
<span data-ttu-id="c0b87-137">Válassza ki a hello tovább nyílra.</span><span class="sxs-lookup"><span data-stu-id="c0b87-137">Select hello NEXT arrow.</span></span>
<span data-ttu-id="c0b87-138">5.</span><span class="sxs-lookup"><span data-stu-id="c0b87-138">5.</span></span> <span data-ttu-id="c0b87-139">Adja meg a tárolási hello és adatbázis-beállítások:</span><span class="sxs-lookup"><span data-stu-id="c0b87-139">Enter hello Storage and Database Settings:</span></span>  <table border="1">
    <tr>
    <td><span data-ttu-id="c0b87-140"><strong>Megfigyelő/archiváló tárfiók</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-140"><strong>Monitoring/Archiving storage account</strong></span></span></td>
    <td><span data-ttu-id="c0b87-141">Válasszon meglévő Storage-fiókot, vagy hozzon létre egy új Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="c0b87-141">Select an existing storage account or create a new storage account.</span></span> <br/><br/><span data-ttu-id="c0b87-142">Ha létrehoz egy új tárfiókot, adja meg a hello <strong>Tárfióknév</strong>.</span><span class="sxs-lookup"><span data-stu-id="c0b87-142">If you create a new Storage account, enter hello <strong>Storage Account Name</strong>.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c0b87-143"><strong>Nyomkövető adatbázis</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-143"><strong>Tracking database</strong></span></span></td>
    <td><span data-ttu-id="c0b87-144">Ha meglévő Azure SQL Database-adatbázist használ, azt nem használhatja másik BizTalk-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c0b87-144">If you use an existing Azure SQL Database, it cannot be used by another BizTalk Service.</span></span> <span data-ttu-id="c0b87-145">Hello bejelentkezési nevet és jelszót adott Azure SQL adatbázis-kiszolgáló létrehozásakor megadott van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c0b87-145">You need hello login name and password entered when that Azure SQL Database Server was created.</span></span><br/><br/><span data-ttu-id="c0b87-146"><strong>Tipp</strong> hello követési adatbázis létrehozása és a figyelés/archiválás tárfiók hello ugyanabban a régióban, hello BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c0b87-146"><strong>TIP</strong> Create hello Tracking database and Monitoring/Archiving storage account in hello same region as hello BizTalk Service.</span></span></td>
    </tr>
    </table>
<span data-ttu-id="c0b87-147">Válassza ki a hello tovább nyílra.</span><span class="sxs-lookup"><span data-stu-id="c0b87-147">Select hello NEXT arrow.</span></span>
<span data-ttu-id="c0b87-148">6.</span><span class="sxs-lookup"><span data-stu-id="c0b87-148">6.</span></span> <span data-ttu-id="c0b87-149">Adja meg a hello adatbázis-beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c0b87-149">Enter hello Database settings:</span></span>  <table border="1">
    <tr>
    <td><span data-ttu-id="c0b87-150"><strong>Name (Név)</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-150"><strong>Name</strong></span></span></td>
    <td><span data-ttu-id="c0b87-151">Érhető el, ha <strong>hozzon létre egy új SQL-adatbázispéldány</strong> van megadva a hello előző képernyőre.</span><span class="sxs-lookup"><span data-stu-id="c0b87-151">Available when <strong>Create a new SQL Database instance</strong> is selected in hello previous screen.</span></span>
    <br/><br/>
<span data-ttu-id="c0b87-152">Adjon meg egy SQL-adatbázis neve toobe a BizTalk szolgáltatás által használt.</span><span class="sxs-lookup"><span data-stu-id="c0b87-152">Enter a SQL Database name toobe used by your BizTalk Service.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c0b87-153"><strong>Kiszolgáló</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-153"><strong>Server</strong></span></span></td>
    <td><span data-ttu-id="c0b87-154">Érhető el, ha <strong>hozzon létre egy új SQL-adatbázispéldány</strong> van megadva a hello előző képernyőre.</span><span class="sxs-lookup"><span data-stu-id="c0b87-154">Available when <strong>Create a new SQL Database instance</strong> is selected in hello previous screen.</span></span>
    <br/><br/>
<span data-ttu-id="c0b87-155">Válasszon egy meglévő SQL Database-kiszolgálót vagy hozzon létre új SQL Database-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="c0b87-155">Select an existing SQL Database Server or create a new SQL Database server.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c0b87-156"><strong>Kiszolgáló bejelentkezési neve</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-156"><strong>Server login name</strong></span></span></td>
    <td><span data-ttu-id="c0b87-157">Adjon meg hello bejelentkezési nevet.</span><span class="sxs-lookup"><span data-stu-id="c0b87-157">Enter hello login user name.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c0b87-158"><strong>Kiszolgáló bejelentkezési jelszava</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-158"><strong>Server login password</strong></span></span></td>
    <td><span data-ttu-id="c0b87-159">Adja meg a hello bejelentkezési jelszót.</span><span class="sxs-lookup"><span data-stu-id="c0b87-159">Enter hello login password.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c0b87-160"><strong>Régió</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-160"><strong>Region</strong></span></span></td>
    <td><span data-ttu-id="c0b87-161">Akkor érhető el, ha az <strong>Új SQL Database-példány létrehozását</strong> választotta.</span><span class="sxs-lookup"><span data-stu-id="c0b87-161">Available when <strong>Create a new SQL Database instance</strong> is selected.</span></span> <span data-ttu-id="c0b87-162">Válassza ki a földrajzi régióban toohost hello az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c0b87-162">Select hello geographic region toohost your SQL Database.</span></span></td>
    </tr>
    </table>

<span data-ttu-id="c0b87-163">Válassza ki a hello pipa toocomplete hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="c0b87-163">Select hello check mark toocomplete hello wizard.</span></span> <span data-ttu-id="c0b87-164">hello folyamatban ikon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c0b87-164">hello progress icon appears:</span></span>  
![Az állapotjelző ikon megjelenik, ha befejeződött][ProgressComplete]

<span data-ttu-id="c0b87-166">Amikor végzett, a hello Azure BizTalk szolgáltatás létrehozott és az alkalmazások készen.</span><span class="sxs-lookup"><span data-stu-id="c0b87-166">When complete, hello Azure BizTalk Service is created and ready for your applications.</span></span> <span data-ttu-id="c0b87-167">hello alapértelmezett beállítások elegendőek.</span><span class="sxs-lookup"><span data-stu-id="c0b87-167">hello default settings are sufficient.</span></span> <span data-ttu-id="c0b87-168">Ha toochange hello alapértelmezett beállításokat, válassza ki a **BIZTALK szolgáltatások** a hello bal oldali navigációs panelen, majd válassza ki a BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c0b87-168">If you want toochange hello default settings, select **BIZTALK SERVICES** in hello left navigation pane, and then select your BizTalk Service.</span></span> <span data-ttu-id="c0b87-169">További beállítások is megjelennek a hello [irányítópult, a figyelő és a skála lapon](biztalk-dashboard-monitor-scale-tabs.md) hello tetején.</span><span class="sxs-lookup"><span data-stu-id="c0b87-169">Additional settings are displayed in hello [Dashboard, Monitor, and Scale tabs](biztalk-dashboard-monitor-scale-tabs.md) at hello top.</span></span>

<span data-ttu-id="c0b87-170">Attól függően, hogy hello hello BizTalk szolgáltatás állapotát vannak bizonyos műveletek, amelyek nem fejezhető be.</span><span class="sxs-lookup"><span data-stu-id="c0b87-170">Depending on hello state of hello BizTalk Service, there are some operations that cannot be completed.</span></span> <span data-ttu-id="c0b87-171">Ezek a műveletek listájának megtekintéséhez nyissa meg túl[BizTalk szolgáltatások állapota diagram](biztalk-service-state-chart.md).</span><span class="sxs-lookup"><span data-stu-id="c0b87-171">For a list of these operations, go too[BizTalk Services State Chart](biztalk-service-state-chart.md).</span></span>

## <a name="post-provisioning-steps"></a><span data-ttu-id="c0b87-172">Kiépítés utáni lépések</span><span class="sxs-lookup"><span data-stu-id="c0b87-172">Post-provisioning steps</span></span>
* [<span data-ttu-id="c0b87-173">A helyi számítógép hello tanúsítvány telepítése</span><span class="sxs-lookup"><span data-stu-id="c0b87-173">Install hello certificate on a local computer</span></span>](#InstallCert)
* [<span data-ttu-id="c0b87-174">Termelésre kész tanúsítvány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c0b87-174">Add a production-ready certificate</span></span>](#AddCert)
* [<span data-ttu-id="c0b87-175">Hello hozzáférés-vezérlés névtér beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0b87-175">Get hello Access Control namespace</span></span>](#ACS)

#### <span data-ttu-id="c0b87-176"><a name="InstallCert"></a>A helyi számítógép hello tanúsítvány telepítése</span><span class="sxs-lookup"><span data-stu-id="c0b87-176"><a name="InstallCert"></a>Install hello certificate on a local computer</span></span>
<span data-ttu-id="c0b87-177">A BizTalk-szolgáltatás kiépítésének részeként létrejön egy önaláírt tanúsítvány, amely társítva van a BizTalk-szolgáltatás előfizetésével.</span><span class="sxs-lookup"><span data-stu-id="c0b87-177">As part of BizTalk Service provisioning, a self-signed certificate is created and associated with your BizTalk Service subscription.</span></span> <span data-ttu-id="c0b87-178">Töltse le a tanúsítványt kell, és ahol Ön BizTalk szolgáltatás alkalmazások központi telepítése, illetve üzenetek tooa BizTalk szolgáltatás végpontjának küldése számítógépekre telepíthető.</span><span class="sxs-lookup"><span data-stu-id="c0b87-178">You must download this certificate and install it on computers from where you either deploy BizTalk Service applications or send messages tooa BizTalk Service endpoint.</span></span>

1. <span data-ttu-id="c0b87-179">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="c0b87-179">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="c0b87-180">Válassza ki **BIZTALK szolgáltatások** a hello bal oldali navigációs panelen, majd válassza ki a BizTalk szolgáltatás előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="c0b87-180">Select **BIZTALK SERVICES** in hello left navigation pane, and then select your BizTalk Service subscription.</span></span>
3. <span data-ttu-id="c0b87-181">Jelölje be hello **irányítópult** fülre.</span><span class="sxs-lookup"><span data-stu-id="c0b87-181">Select hello **Dashboard** tab.</span></span>
4. <span data-ttu-id="c0b87-182">Válassza az **SSL-tanúsítvány letöltése** elemet:</span><span class="sxs-lookup"><span data-stu-id="c0b87-182">Select **Download SSL Certificate**:</span></span>  
   <span data-ttu-id="c0b87-183">![SSL-tanúsítvány módosítása][QuickGlance]</span><span class="sxs-lookup"><span data-stu-id="c0b87-183">![Modify SSL Certificate][QuickGlance]</span></span>
5. <span data-ttu-id="c0b87-184">Kattintson duplán a hello tanúsítványt, és futtasson hello varázsló tooinstall hello tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="c0b87-184">Double-click hello certificate and run through hello wizard tooinstall hello certificate.</span></span> <span data-ttu-id="c0b87-185">Ellenőrizze, hogy a hello hello tanúsítványát telepítenie **megbízható legfelső szintű hitelesítésszolgáltatók** tárolja.</span><span class="sxs-lookup"><span data-stu-id="c0b87-185">Make sure you install hello certificate under hello **Trusted Root Certificate Authorities** store.</span></span>

#### <span data-ttu-id="c0b87-186"><a name="AddCert"></a>Termelésre kész tanúsítvány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c0b87-186"><a name="AddCert"></a>Add a production-ready certificate</span></span>
<span data-ttu-id="c0b87-187">hello önaláírt tanúsítványt, amely automatikusan jön létre, amikor csak a fejlesztési környezetben használatra szánt BizTalk szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c0b87-187">hello self-signed certificate that is automatically created when creating BizTalk Services is intended for use in development environments only.</span></span> <span data-ttu-id="c0b87-188">A termelési forgatókönyvekhez cserélje le egy termelésre kész tanúsítványra.</span><span class="sxs-lookup"><span data-stu-id="c0b87-188">For production scenarios, replace it with a production-ready certificate.</span></span>

1. <span data-ttu-id="c0b87-189">A hello **irányítópult** lapon jelölje be **frissítés SSL-tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="c0b87-189">On hello **Dashboard** tab, select **Update SSL Certificate**.</span></span>
2. <span data-ttu-id="c0b87-190">Keresse meg a saját SSL-tanúsítvány tooyour (*CertificateName*.pfx), amely tartalmazza a BizTalk szolgáltatás neve, hello jelszót adjon meg, és kattintson a pipa hello.</span><span class="sxs-lookup"><span data-stu-id="c0b87-190">Browse tooyour private SSL certificate (*CertificateName*.pfx) that includes your BizTalk Service name, enter hello password, and then click hello check mark.</span></span>

#### <span data-ttu-id="c0b87-191"><a name="ACS"></a>Hello hozzáférés-vezérlés névtér beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0b87-191"><a name="ACS"></a>Get hello Access Control namespace</span></span>
1. <span data-ttu-id="c0b87-192">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="c0b87-192">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="c0b87-193">Válassza ki **BIZTALK szolgáltatások** a hello bal oldali navigációs panelen, majd válassza ki a BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c0b87-193">Select **BIZTALK SERVICES** in hello left navigation pane, and then select your BizTalk Service.</span></span>
3. <span data-ttu-id="c0b87-194">Hello tálcán, válassza ki a **kapcsolatadatok**:</span><span class="sxs-lookup"><span data-stu-id="c0b87-194">In hello task bar, select **Connection Information**:</span></span>  
   <span data-ttu-id="c0b87-195">![Kattintson a Connection Information (Kapcsolatadatok) elemre.][ACSConnectInfo]</span><span class="sxs-lookup"><span data-stu-id="c0b87-195">![Select Connection Information][ACSConnectInfo]</span></span>
4. <span data-ttu-id="c0b87-196">Hello hozzáférés-vezérlés értékek másolása.</span><span class="sxs-lookup"><span data-stu-id="c0b87-196">Copy hello Access Control values.</span></span>

<span data-ttu-id="c0b87-197">Amikor BizTalk Service-projektet helyez üzembe a Visual Studióból, ezt a hozzáférés-vezérlési névteret írja be.</span><span class="sxs-lookup"><span data-stu-id="c0b87-197">When you deploy a BizTalk Service project from Visual Studio, you enter this Access Control namespace.</span></span> <span data-ttu-id="c0b87-198">hello hozzáférés-vezérlés névtér automatikusan létrejön a BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c0b87-198">hello Access Control namespace is automatically created for your BizTalk Service.</span></span>

<span data-ttu-id="c0b87-199">hello hozzáférés-vezérlés értékek bármilyen alkalmazással használható.</span><span class="sxs-lookup"><span data-stu-id="c0b87-199">hello Access Control values can be used with any application.</span></span> <span data-ttu-id="c0b87-200">Azure BizTalk szolgáltatás létrehozása a hozzáférés-vezérlés névtér hello hitelesítés és a BizTalk szolgáltatás központi telepítése az szabályozza.</span><span class="sxs-lookup"><span data-stu-id="c0b87-200">When Azure BizTalk Services is created, this Access Control namespace controls hello authentication with your BizTalk Service deployment.</span></span> <span data-ttu-id="c0b87-201">Ha szeretné, hogy toochange hello előfizetés vagy hello névtér kezelésére, válassza ki a **ACTIVE DIRECTORY** a hello bal oldali navigációs panelen, és válassza ki a névteret.</span><span class="sxs-lookup"><span data-stu-id="c0b87-201">If you want toochange hello subscription or manage hello namespace, select **ACTIVE DIRECTORY** in hello left navigation pane and then select your namespace.</span></span> <span data-ttu-id="c0b87-202">hello tálcán felsorolja a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="c0b87-202">hello task bar lists your options.</span></span>

<span data-ttu-id="c0b87-203">Kattintson a **kezelése** megnyílik hello Access Control felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="c0b87-203">Clicking **Manage** opens hello Access Control Management Portal.</span></span> <span data-ttu-id="c0b87-204">Hello Access Control felügyeleti portálon, a BizTalk szolgáltatás által használt hello **identitások szolgáltatás**:</span><span class="sxs-lookup"><span data-stu-id="c0b87-204">In hello Access Control Management Portal, hello BizTalk Service uses **Service identities**:</span></span>  
<span data-ttu-id="c0b87-205">![Szolgáltatás-identitások ACS a hello Access Control felügyeleti portálon][ACSServiceIdentities]</span><span class="sxs-lookup"><span data-stu-id="c0b87-205">![ACS Service Identities in hello Access Control Management Portal][ACSServiceIdentities]</span></span>

<span data-ttu-id="c0b87-206">Hozzáférés-vezérlés szolgáltatásidentitás hello olyan hitelesítő adatokat, amelyek lehetővé teszik az alkalmazások és ügyfelek tooauthenticate közvetlenül a hozzáférés-vezérlés és fogadhatnak jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="c0b87-206">hello Access Control service identity is a set of credentials that allow applications or clients tooauthenticate directly with Access Control and receive a token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0b87-207">hello BizTalk szolgáltatás használja **tulajdonos** hello alapértelmezett szolgáltatásidentitás és hello **jelszó** érték.</span><span class="sxs-lookup"><span data-stu-id="c0b87-207">hello BizTalk Service uses **Owner** for hello default service identity and hello **Password** value.</span></span> <span data-ttu-id="c0b87-208">Hello szimmetrikus kulcs-érték helyett hello jelszó érték használatakor hello alábbi hiba történhet.</span><span class="sxs-lookup"><span data-stu-id="c0b87-208">If you use hello Symmetric Key value instead of hello Password value, hello following error may occur.</span></span><br/><br/><span data-ttu-id="c0b87-209">*Nem lehet kapcsolódni a megadott hello toohello Access Control Management Service fiók hitelesítő adatait*</span><span class="sxs-lookup"><span data-stu-id="c0b87-209">*Could not connect toohello Access Control Management Service account with hello specified credentials*</span></span>
> 
> 

<span data-ttu-id="c0b87-210">[Az ACS-névtér felügyeletét](https://msdn.microsoft.com/library/azure/hh674478.aspx) ismertető dokumentum tartalmaz néhány útmutatást és javaslatot.</span><span class="sxs-lookup"><span data-stu-id="c0b87-210">[Managing Your ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) lists some guidelines and recommendations.</span></span>

## <a name="requirements-explained"></a><span data-ttu-id="c0b87-211">A követelmények részletei</span><span class="sxs-lookup"><span data-stu-id="c0b87-211">Requirements explained</span></span>
<span data-ttu-id="c0b87-212">Ezeket a követelményeket csak akkor érvényesíthetők toohello ingyenes kiadásban.</span><span class="sxs-lookup"><span data-stu-id="c0b87-212">These requirements do not apply toohello Free Edition.</span></span>

<table border="1">
<tr bgcolor="FAF9F9">
        <td><span data-ttu-id="c0b87-213"><strong>Mi szükséges</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-213"><strong>What you need</strong></span></span></td>
        <td><span data-ttu-id="c0b87-214"><strong>Miért szükséges</strong></span><span class="sxs-lookup"><span data-stu-id="c0b87-214"><strong>Why you need it</strong></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c0b87-215">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="c0b87-215">Azure subscription</span></span></td>
<td><span data-ttu-id="c0b87-216">hello előfizetés határozza meg, akik toohello Azure-portálon bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="c0b87-216">hello subscription determines who can sign in toohello Azure portal.</span></span> <span data-ttu-id="c0b87-217">hello fióktulajdonos hoz létre hello előfizetést, <a HREF="https://account.windowsazure.com/Subscriptions"> Azure-előfizetések</a>.</span><span class="sxs-lookup"><span data-stu-id="c0b87-217">hello Account holder creates hello subscription at <a HREF="https://account.windowsazure.com/Subscriptions"> Azure Subscriptions</a>.</span></span>
<br/><br/>
<span data-ttu-id="c0b87-218">hello Azure-fiók több előfizetéssel is rendelkezhet, és kezelheti bárki, aki engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="c0b87-218">hello Azure account can have multiple subscriptions and can be managed by anyone who is permitted.</span></span> <span data-ttu-id="c0b87-219">Például az Azure-fiók jogosult egy nevű előfizetést hoz létre <em>BizTalkServiceSubscription</em> és által biztosított BizTalk rendszergazdák hello a vállalaton belül (például ContosoBTSAdmins@live.com) toothis előfizetés eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c0b87-219">For example, your Azure account holder creates a subscription named <em>BizTalkServiceSubscription</em> and gives hello BizTalk Administrators within your company (for example, ContosoBTSAdmins@live.com) access toothis subscription.</span></span> <span data-ttu-id="c0b87-220">Ebben a forgatókönyvben hello BizTalk rendszergazdák bejelentkezik toohello Azure-portálon, de rendelkezik teljes rendszergazdai jogosultságok tooall hello üzemeltetett szolgáltatások hello az előfizetést, beleértve az Azure BizTalk szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="c0b87-220">In this scenario, hello BizTalk Administrators sign in toohello Azure portal and have full Administrator rights tooall hello hosted services in hello subscription, including Azure BizTalk Services.</span></span> <span data-ttu-id="c0b87-221">hello BizTalk rendszergazdák nincsenek hello Azure-fiókhoz tulajdonosainak, és ezért nem rendelkezik hozzáféréssel tooany számlázási adatokat.</span><span class="sxs-lookup"><span data-stu-id="c0b87-221">hello BizTalk Administrators are not hello Azure account holders and therefore don't have access tooany billing information.</span></span>
<br/><br/><span data-ttu-id="c0b87-222">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Előfizetések és a Storage-fiókok kezelése az Azure-portálon hello</a> nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="c0b87-222">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577"> Manage Subscriptions and Storage Accounts in hello Azure portal</a> provides more information.</span></span>
</td>
</tr>
<tr>
<td><span data-ttu-id="c0b87-223">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c0b87-223">Azure SQL Database</span></span></td>
<td><span data-ttu-id="c0b87-224">Hello táblákat, nézeteket és tárolt eljárások hello BizTalk szolgáltatás, így az hello nyomon követési adatok által használt tárolja.</span><span class="sxs-lookup"><span data-stu-id="c0b87-224">Stores hello tables, views, and stored procedures used by hello BizTalk Service, including hello Tracking data.</span></span>
<br/><br/>
<span data-ttu-id="c0b87-225">BizTalk-szolgáltatások létrehozásakor használhat meglévő Azure SQL Server-kiszolgálót vagy Azure SQL Database-adatbázist, vagy automatikusan létrehozhat egy új kiszolgálót vagy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="c0b87-225">When you create a BizTalk Service, you can use an existing Azure SQL Server, Azure SQL Database, or automatically create a new Server or Database.</span></span>
<br/><br/>
<span data-ttu-id="c0b87-226">SQL adatbázis-méretezési hello automatikusan úgy van beállítva.</span><span class="sxs-lookup"><span data-stu-id="c0b87-226">hello SQL Database scale is automatically configured.</span></span> <span data-ttu-id="c0b87-227">Hello alapértelmezett méretezési általában elegendő a BizTalk szolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="c0b87-227">Typically, hello default scale is sufficient for a BizTalk Service.</span></span> <span data-ttu-id="c0b87-228">Változó hello méretezési hatással van a díjszabás.</span><span class="sxs-lookup"><span data-stu-id="c0b87-228">Changing hello scale impacts pricing.</span></span> <span data-ttu-id="c0b87-229">Lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">Fiókok és számlázás az Azure SQL Database-ben</a>
</span><span class="sxs-lookup"><span data-stu-id="c0b87-229">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930"> Accounts and Billing in Azure SQL Database</a>
</span></span><br/><br/><span data-ttu-id="c0b87-230">
<strong>Megjegyzések</strong>
</span><span class="sxs-lookup"><span data-stu-id="c0b87-230">
<strong>Notes</strong>
</span></span><br/>
<ul>
<li> <span data-ttu-id="c0b87-231">Új Azure SQL Server és adatbázis létrehozásakor az Azure-szolgáltatások automatikusan engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="c0b87-231">When you create a new Azure SQL Server and Database, Azure Services is automatically enabled.</span></span> <span data-ttu-id="c0b87-232">hello BizTalk szolgáltatás Azure-szolgáltatások szükségesek engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="c0b87-232">hello BizTalk Service requires Azure Services be enabled.</span></span></li>
<li><span data-ttu-id="c0b87-233">Ha egy meglévő Azure SQL Server hoz létre egy új Azure SQL Database, hello tűzfalszabályt hello kiszolgáló, nem módosulnak.</span><span class="sxs-lookup"><span data-stu-id="c0b87-233">If you create a new Azure SQL Database on an existing Azure SQL Server, hello firewall rules of hello Server are not changed.</span></span> <span data-ttu-id="c0b87-234">Ennek eredményeképpen lehetőség más Azure-szolgáltatások használata nem engedélyezett a hozzáférés toohello Server-adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="c0b87-234">As a result, it's possible other Azure Services are not allowed access toohello Server's databases.</span></span></li>
</ul>
</td>
</tr>
<tr>
<td><span data-ttu-id="c0b87-235">Azure hozzáférés-vezérlési névtér</span><span class="sxs-lookup"><span data-stu-id="c0b87-235">Azure Access Control namespace</span></span></td>
<td><span data-ttu-id="c0b87-236">Az Azure BizTalk Services modullal végez hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="c0b87-236">Authenticates with Azure BizTalk Services.</span></span> <span data-ttu-id="c0b87-237">Amikor BizTalk Service-projektet helyez üzembe a Visual Studióból, ezt a hozzáférés-vezérlési névteret írja be.</span><span class="sxs-lookup"><span data-stu-id="c0b87-237">When you deploy a BizTalk Service project from Visual Studio, you enter this Access Control namespace.</span></span> <span data-ttu-id="c0b87-238">BizTalk szolgáltatás létrehozása, amikor a rendszer automatikusan létrehoz hello hozzáférés-vezérlés névtér.</span><span class="sxs-lookup"><span data-stu-id="c0b87-238">When you create a BizTalk Service, hello Access Control namespace is automatically created.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="c0b87-239">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="c0b87-239">Azure Storage account</span></span></td>
<td><span data-ttu-id="c0b87-240">Által biztosított hozzáférés tootables, blobok és a BizTalk szolgáltatás toosave hello következő által használt:</span><span class="sxs-lookup"><span data-stu-id="c0b87-240">Gives access tootables, blobs, and queues used by your BizTalk Service toosave hello following:</span></span>

<ul>
<li><span data-ttu-id="c0b87-241">A naplófájlok, hogy a figyelő hello BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c0b87-241">Log files that monitor hello BizTalk Service.</span></span> <span data-ttu-id="c0b87-242">kimeneti figyelési hello megjelenik a hello **figyelés** hello Azure-portálon lapján.</span><span class="sxs-lookup"><span data-stu-id="c0b87-242">hello monitoring output is also displayed in hello **Monitoring** tab in hello Azure portal.</span></span></li>
<li><span data-ttu-id="c0b87-243">Egy X12 vagy AS2 megállapodást a partnerek között létrehozásakor hello Archiválás szolgáltatás toostore üzenettulajdonságok engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="c0b87-243">When creating an X12 or AS2 agreement between partners, you can enable hello Archiving feature toostore message properties.</span></span> <span data-ttu-id="c0b87-244">Ezek az adatok hello Storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="c0b87-244">This data is saved in hello Storage account.</span></span></li>
</ul>
<br/>
<span data-ttu-id="c0b87-245">BizTalk Services-szolgáltatások létrehozásakor használhat meglévő Storage-fiókot, vagy automatikusan létrehozhat egy új Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="c0b87-245">When you create a BizTalk Service, you can use an existing Storage account or automatically create a new Storage account.</span></span>
<br/><br/>
<span data-ttu-id="c0b87-246">a BizTalk szolgáltatás hello alapértelmezett tárolási beállítások elegendőek.</span><span class="sxs-lookup"><span data-stu-id="c0b87-246">hello default Storage settings are sufficient for a BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="c0b87-247">Storage-fiók létrehozásakor automatikusan létrejön egy elsődleges kulcs és egy másodlagos kulcs.</span><span class="sxs-lookup"><span data-stu-id="c0b87-247">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="c0b87-248">Ezeket a kulcsokat a tárfiók hozzáférési tooyour szabályozza.</span><span class="sxs-lookup"><span data-stu-id="c0b87-248">These Keys control access tooyour Storage account.</span></span> <span data-ttu-id="c0b87-249">hello BizTalk szolgáltatás automatikusan hello elsődleges kulcsot használja.</span><span class="sxs-lookup"><span data-stu-id="c0b87-249">hello BizTalk Service automatically uses hello Primary Key.</span></span>
<br/><br/>
<span data-ttu-id="c0b87-250">További információért lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">Storage</a>.</span><span class="sxs-lookup"><span data-stu-id="c0b87-250">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671"> Storage</a> for more information.</span></span>
</td>
</tr>

<tr>
<td><span data-ttu-id="c0b87-251">SSL személyes tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="c0b87-251">SSL private certificate</span></span></td>
<td>
<span data-ttu-id="c0b87-252">Azure BizTalk Services-szolgáltatások létrehozásakor létrejön a BizTalk Services-szolgáltatás nevét tartalmazó HTTPS URL.</span><span class="sxs-lookup"><span data-stu-id="c0b87-252">When an Azure BizTalk Service is created, an HTTPS URL that includes your BizTalk Service name is also created.</span></span> <span data-ttu-id="c0b87-253">Az URL-cím, automatikusan konfigurált toouse csak fejlesztési önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c0b87-253">This URL is automatically configured toouse a self-signed development-only certificate.</span></span> <span data-ttu-id="c0b87-254">A termeléshez személyes SSL-tanúsítvány szükséges.</span><span class="sxs-lookup"><span data-stu-id="c0b87-254">For production, you need a private SSL certificate.</span></span>
<br/><br/><span data-ttu-id="c0b87-255">
<strong>SSL-tanúsítvánnyal kapcsolatos fontos információk</strong>

</span><span class="sxs-lookup"><span data-stu-id="c0b87-255">
<strong>Important SSL Certificate Information</strong>

</span></span><ul>
<li><span data-ttu-id="c0b87-256">hello tanúsítvány lejárati dátumnak kell lennie a kisebb, mint 5 év.</span><span class="sxs-lookup"><span data-stu-id="c0b87-256">hello certificate expiration date must be less than 5 years.</span></span></li>
<li><span data-ttu-id="c0b87-257">Minden személyes tanúsítványhoz jelszó szükséges.</span><span class="sxs-lookup"><span data-stu-id="c0b87-257">All private certificates require a password.</span></span> <span data-ttu-id="c0b87-258">Jegyezze meg ezt a jelszót, és ajánlott eljárásként ossza meg a rendszergazdáival.</span><span class="sxs-lookup"><span data-stu-id="c0b87-258">Know this password and as a best practice, share this password with your administrators.</span></span></li>
<li><span data-ttu-id="c0b87-259">A tesztelési/fejlesztési környezetekben önaláírt tanúsítványokat használnak.</span><span class="sxs-lookup"><span data-stu-id="c0b87-259">Self-signed certificates are used in a test/development environment.</span></span> <span data-ttu-id="c0b87-260">Ha önaláírt tanúsítványokat használ, hello tanúsítvány tooyour személyes tanúsítványtárolójába importálni és hello megbízható legfelső szintű hitelesítésszolgáltatók tanúsítványtárolójába.</span><span class="sxs-lookup"><span data-stu-id="c0b87-260">When using self-signed certificates, import hello certificate tooyour Personal certificate store and hello Trusted Root Certification Authorities certificate store.</span></span></li>
</ul>
<br/><span data-ttu-id="c0b87-261">Hello éles tanúsítvány kérelem tooyour hitelesítésszolgáltató küldésekor adja meg a következő tanúsítvány tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="c0b87-261">When sending hello production certificate request tooyour certification authority, give hello following certificate properties:</span></span>
<br/>

<ul>
<li><span data-ttu-id="c0b87-262"><strong>Kibővített kulcshasználat</strong>: Az Azure BizTalk Services legalább kiszolgálói hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="c0b87-262"><strong>Enhanced Key Usage</strong>: At a minimum, Azure BizTalk Services requires Server Authentication.</span></span></li>
<li><span data-ttu-id="c0b87-263"><strong>Köznapi név</strong>: hello teljesen minősített tartományneve (FQDN) az Azure BizTalk szolgáltatás URL-CÍMÉT adja meg.</span><span class="sxs-lookup"><span data-stu-id="c0b87-263"><strong>Common Name</strong>: Enter hello fully qualified domain name (FQDN) of your Azure BizTalk Service URL.</span></span> <span data-ttu-id="c0b87-264">Lásd ezen cikk <a HREF="#CreateService">BizTalk Services-szolgáltatás létrehozása</a> szakaszát.</span><span class="sxs-lookup"><span data-stu-id="c0b87-264">See <a HREF="#CreateService">Create a BizTalk Service</a> in this article.</span></span></li>
</ul>
<br/>
<span data-ttu-id="c0b87-265">Egy új vagy eltérő tanúsítvány hello BizTalk szolgáltatás létrehozása után adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="c0b87-265">A new or different certificate can be added after hello BizTalk Service is created.</span></span>
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting tooany location.--->



## <a name="hybrid-connections"></a><span data-ttu-id="c0b87-266">Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c0b87-266">Hybrid Connections</span></span>
<span data-ttu-id="c0b87-267">Egy Azure BizTalk szolgáltatás létrehozásakor hello **hibrid kapcsolatok** lap jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c0b87-267">When you create an Azure BizTalk Service, hello **Hybrid Connections** tab is available:</span></span>

![Hibrid kapcsolatok lap][HybridConnectionTab]

<span data-ttu-id="c0b87-269">Hibrid kapcsolatok használt tooconnect egy Azure webhely vagy az Azure-mobilszolgáltatás tooany helyszíni erőforrása, amely a statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k, a Mobile Services és a legtöbb egyéni webszolgáltatások használja.</span><span class="sxs-lookup"><span data-stu-id="c0b87-269">Hybrid Connections are used tooconnect an Azure website or Azure mobile service tooany on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, Mobile Services, and most custom Web Services.</span></span>  <span data-ttu-id="c0b87-270">Hibrid kapcsolatok és a BizTalk szolgáltatás hello eltérőek.</span><span class="sxs-lookup"><span data-stu-id="c0b87-270">Hybrid Connections and hello BizTalk Adapter Service are different.</span></span> <span data-ttu-id="c0b87-271">BizTalk szolgáltatás hello rendszer használt tooconnect Azure BizTalk szolgáltatások tooan helyszíni sor az üzleti (LOB).</span><span class="sxs-lookup"><span data-stu-id="c0b87-271">hello BizTalk Adapter Service is used tooconnect Azure BizTalk Services tooan on-premises Line of Business (LOB) system.</span></span>

 <span data-ttu-id="c0b87-272">Lásd: [hibrid kapcsolatok](integration-hybrid-connection-overview.md) toolearn további, beleértve a létrehozása és a hibrid kapcsolatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="c0b87-272">See [Hybrid Connections](integration-hybrid-connection-overview.md) toolearn more, including creating and managing Hybrid Connections.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0b87-273">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0b87-273">Next steps</span></span>
<span data-ttu-id="c0b87-274">Most, hogy a BizTalk szolgáltatás létrehozása, ismerkedjen meg különböző hello [BizTalk szolgáltatások: irányítópult, a figyelő és a skála lapon](biztalk-dashboard-monitor-scale-tabs.md).</span><span class="sxs-lookup"><span data-stu-id="c0b87-274">Now that a BizTalk Service is created, familiarize yourself with hello different [BizTalk Services: Dashboard, Monitor and Scale tabs](biztalk-dashboard-monitor-scale-tabs.md).</span></span> <span data-ttu-id="c0b87-275">Az Azure BizTalk-szolgáltatás készen áll az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c0b87-275">Your BizTalk Service is ready for your applications.</span></span> <span data-ttu-id="c0b87-276">alkalmazások, lépjen túl létrehozásának toostart[Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span><span class="sxs-lookup"><span data-stu-id="c0b87-276">toostart creating applications, go too[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="c0b87-277">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c0b87-277">See also</span></span>
* [<span data-ttu-id="c0b87-278">BizTalk Services: Kiadások diagramja</span><span class="sxs-lookup"><span data-stu-id="c0b87-278">BizTalk Services: Editions Chart</span></span>](biztalk-editions-feature-chart.md)<br/>
* [<span data-ttu-id="c0b87-279">BizTalk Services: Állapottáblázat</span><span class="sxs-lookup"><span data-stu-id="c0b87-279">BizTalk Services: State Chart</span></span>](biztalk-service-state-chart.md)<br/>
* [<span data-ttu-id="c0b87-280">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="c0b87-280">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)<br/>
* [<span data-ttu-id="c0b87-281">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="c0b87-281">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)<br/>
* [<span data-ttu-id="c0b87-282">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="c0b87-282">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)<br/>
* [<span data-ttu-id="c0b87-283">Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t</span><span class="sxs-lookup"><span data-stu-id="c0b87-283">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="c0b87-284">Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="c0b87-284">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
