---
title: "a webes alkalmazás tűzfalat (WAF) App Service Environment-környezet aaaConfiguring"
description: "Ismerje meg, hogyan tűzfal-tooconfigure egy webalkalmazást az App Service-környezet elé."
services: app-service\web
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: naziml
ms.openlocfilehash: 0fcf62aea871751c9d4f294d2d24df2186fc0e7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a><span data-ttu-id="85078-103">Webalkalmazási tűzfal (WAF) App Service Environment-környezet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85078-103">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="85078-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="85078-104">Overview</span></span>
<span data-ttu-id="85078-105">Webalkalmazás-hello például tűzfalak [Barracuda WAF az Azure-](https://www.barracuda.com/programs/azure) elérhető a hello [Azure piactér](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) segítséget nyújt a webes alkalmazások biztonságos bejövő webes forgalom tooblock SQL vizsgálatával alkalommal, idegen, kártevő szoftverek feltöltések & DDoS alkalmazás és más támadásoknak.</span><span class="sxs-lookup"><span data-stu-id="85078-105">Web application firewalls like hello [Barracuda WAF for Azure](https://www.barracuda.com/programs/azure) that is available on hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) helps secure your web applications by inspecting inbound web traffic tooblock SQL injections, Cross-Site Scripting, malware uploads & application DDoS and other attacks.</span></span> <span data-ttu-id="85078-106">Azt is ellenőrzi hello háttér-webkiszolgálók hello válaszainak az adatok adatvesztés-megelőzési (DLP).</span><span class="sxs-lookup"><span data-stu-id="85078-106">It also inspects hello responses from hello back-end web servers for Data Loss Prevention (DLP).</span></span> <span data-ttu-id="85078-107">Hello elkülönítési és az App Service Environment-környezetek által biztosított további skálázás együtt, a környezetet biztosít az ideális toohost üzleti kritikus webalkalmazások toowithstand kártékony kérelmek és nagy mennyiségű forgalom van szükség.</span><span class="sxs-lookup"><span data-stu-id="85078-107">Combined with hello isolation and additional scaling provided by App Service Environments, this provides an ideal environment toohost business critical web applications that need toowithstand malicious requests and high volume traffic.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a><span data-ttu-id="85078-108">Beállítás</span><span class="sxs-lookup"><span data-stu-id="85078-108">Setup</span></span>
<span data-ttu-id="85078-109">A dokumentum azt konfigurálja az App Service-környezet több terhelés mögött, hogy csak hello WAF érkező forgalom hello App Service Environment-környezet képes elérni és nem lesz elérhető hello DMZ kiegyensúlyozott Barracuda WAF példányai.</span><span class="sxs-lookup"><span data-stu-id="85078-109">For this document we will configure our App Service Environment behind multiple load balanced instances of Barracuda WAF so that only traffic from hello WAF can reach hello App Service Environment and it will not be accessible from hello DMZ.</span></span> <span data-ttu-id="85078-110">Azt is Azure Traffic Manager elé a Barracuda WAF példányok tooload egyenleg Azure-adatközpont és régiók között.</span><span class="sxs-lookup"><span data-stu-id="85078-110">We will also have Azure Traffic Manager in front of our Barracuda WAF instances tooload balance across Azure data centers and regions.</span></span> <span data-ttu-id="85078-111">Magas szintű diagramját, illetve hello beállítása néz alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="85078-111">A high level diagram of hello setup would look like what is shown below.</span></span>

![Architektúra][Architecture] 

> <span data-ttu-id="85078-113">Megjegyzés: A hello bevezetése [ILB támogatja az App Service Environment-környezet](app-service-environment-with-internal-load-balancer.md), nem érhető el a hello DMZ hello ASE toobe konfigurálhatja, és csak akkor érhető el toohello magánhálózati.</span><span class="sxs-lookup"><span data-stu-id="85078-113">Note: With hello introduction of [ILB support for App Service Environment](app-service-environment-with-internal-load-balancer.md), you can configure hello ASE toobe inaccessible from hello DMZ and only be available toohello private network.</span></span> 
> 
> 

## <a name="configuring-your-app-service-environment"></a><span data-ttu-id="85078-114">Az App Service-környezet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85078-114">Configuring your App Service Environment</span></span>
<span data-ttu-id="85078-115">az App Service-környezetek tooconfigure tekintse meg a túl[dokumentációban](app-service-web-how-to-create-an-app-service-environment.md) hello témáról.</span><span class="sxs-lookup"><span data-stu-id="85078-115">tooconfigure an App Service Environment refer too[our documentation](app-service-web-how-to-create-an-app-service-environment.md) on hello subject.</span></span> <span data-ttu-id="85078-116">Miután egy App Service-környezet létrehozása, létrehozhat [webalkalmazások](app-service-web-overview.md), [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md) és [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) ebben a környezetben, amely az összes védi mögött hello WAF azt a következő szakaszban hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="85078-116">Once you have an App Service Environment created, you can create [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) and [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) in this environment that will all be protected behind hello WAF we configure in hello next section.</span></span>

## <a name="configuring-your-barracuda-waf-cloud-service"></a><span data-ttu-id="85078-117">A Barracuda WAF Felhőszolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85078-117">Configuring your Barracuda WAF Cloud Service</span></span>
<span data-ttu-id="85078-118">Barracuda rendelkezik egy [részletes cikk](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) üzembe helyezni a WAF az Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="85078-118">Barracuda has a [detailed article](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) on deploying its WAF on a virtual machine in Azure.</span></span> <span data-ttu-id="85078-119">De azt szeretné, hogy redundancia, és a hibaérzékeny pontok kialakulását vezet be, mert szükség van-e legalább 2 toodeploy WAF példányának virtuális gépek hello azonos felhőalapú szolgáltatás, ha az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="85078-119">But because we want redundancy and not introduce a single point of failure, you want toodeploy at least 2 WAF instance VMs into hello same Cloud Service when following these instructions.</span></span>

### <a name="adding-endpoints-toocloud-service"></a><span data-ttu-id="85078-120">Végpontok tooCloud szolgáltatás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="85078-120">Adding Endpoints tooCloud Service</span></span>
<span data-ttu-id="85078-121">Ha szükség van a 2 vagy több WAF Virtuálisgép-példányok a felhőszolgáltatásban hello azonnal használható [Azure-portálon](https://portal.azure.com/) tooadd HTTP és HTTPS végpontok az alábbi hello ábrán látható módon az alkalmazás által használt.</span><span class="sxs-lookup"><span data-stu-id="85078-121">Once you have 2 or more WAF VM instances in your Cloud Service you can use hello [Azure portal](https://portal.azure.com/) tooadd HTTP and HTTPS endpoints that are used by your application as shown in hello image below.</span></span>

![Konfigurálja a végpontot][ConfigureEndpoint]

<span data-ttu-id="85078-123">Az alkalmazások más végpontok használatát, győződjön meg arról, hogy tooadd e toothis listáját, valamint.</span><span class="sxs-lookup"><span data-stu-id="85078-123">If your applications use other endpoints, make sure tooadd those toothis list as well.</span></span> 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a><span data-ttu-id="85078-124">A felügyeleti portálon keresztül WAF Barracuda konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85078-124">Configuring Barracuda WAF through its Management Portal</span></span>
<span data-ttu-id="85078-125">Barracuda WAF TCP Port 8000 használja a konfiguráció a felügyeleti portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="85078-125">Barracuda WAF uses TCP Port 8000 for configuration through its management portal.</span></span> <span data-ttu-id="85078-126">Mivel hello WAF virtuális gépek több példánya van szüksége lesz toorepeat hello itt lépéseket minden egyes Virtuálisgép-példány.</span><span class="sxs-lookup"><span data-stu-id="85078-126">Since we have multiple instances of hello WAF VMs you will need toorepeat hello steps here for each VM instance.</span></span> 

> <span data-ttu-id="85078-127">Megjegyzés: Ha elkészült, WAF konfigurációjával, távolítsa el hello TCP/8000 végpont a WAF virtuális gépek tookeep a biztonságos WAF.</span><span class="sxs-lookup"><span data-stu-id="85078-127">Note: Once you are done with WAF configuration, remove hello TCP/8000 endpoint from all your WAF VMs tookeep your WAF secure.</span></span>
> 
> 

<span data-ttu-id="85078-128">Hello felügyeleti végpont hello ábrának megfelelően tooconfigure alatt a Barracuda WAF hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="85078-128">Add hello management endpoint as shown in hello image below tooconfigure your Barracuda WAF.</span></span>

![Felügyeleti végpont hozzáadása][AddManagementEndpoint]

<span data-ttu-id="85078-130">A böngésző toobrowse toohello felügyeleti végpont használatát a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="85078-130">Use a browser toobrowse toohello management endpoint on your Cloud Service.</span></span> <span data-ttu-id="85078-131">Ha a Felhőszolgáltatás neve test.cloudapp.net, ehhez a végponthoz toohttp://test.cloudapp.net:8000 tallózással elérésére.</span><span class="sxs-lookup"><span data-stu-id="85078-131">If your Cloud Service is called test.cloudapp.net, you would access this endpoint by browsing toohttp://test.cloudapp.net:8000.</span></span> <span data-ttu-id="85078-132">A bejelentkezési oldal megjelenik, például alatt is bejelentkezési hello WAF virtuális gép telepítési fázis a megadott hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="85078-132">You should see a login page like below that you can login using credentials you specified in hello WAF VM setup phase.</span></span>

![Management bejelentkezési oldal][ManagementLoginPage]

<span data-ttu-id="85078-134">Többször bejelentkezési, megjelenik egy irányítópultot, hello egy hello kép, amely alatt a jelent hello WAF védelmi alapvető statisztikája.</span><span class="sxs-lookup"><span data-stu-id="85078-134">Once you login you should see a dashboard as hello one in hello image below that will present basic statistics about hello WAF protection.</span></span>

![Kezelési irányítópult][ManagementDashboard]

<span data-ttu-id="85078-136">Hello szolgáltatások fülre kattintva így állíthatja be az általa védett szolgáltatásokhoz WAF.</span><span class="sxs-lookup"><span data-stu-id="85078-136">Clicking on hello Services tab will let you configure your WAF for services it is protecting.</span></span> <span data-ttu-id="85078-137">A Barracuda WAF konfigurálásával kapcsolatos további részletekért tanulmányozza [dokumentum](https://techlib.barracuda.com/waf/getstarted1).</span><span class="sxs-lookup"><span data-stu-id="85078-137">For more details on configuring your Barracuda WAF you can consult [their documentation](https://techlib.barracuda.com/waf/getstarted1).</span></span> <span data-ttu-id="85078-138">Hello példában alatt az Azure Web Apps HTTP és HTTPS-forgalmat szolgáltató nincs beállítva.</span><span class="sxs-lookup"><span data-stu-id="85078-138">In hello example below an Azure Web App serving traffic on HTTP and HTTPS has been configured.</span></span>

![Felügyeleti szolgáltatások hozzáadása][ManagementAddServices]

> <span data-ttu-id="85078-140">Megjegyzés: Attól függően, hogy az alkalmazások hogyan vannak konfigurálva, és milyen funkciók használnak az App Service-környezet, szüksége lesz tooforward forgalom 80-as és 443-as TCP-portok például ha egy webalkalmazás IP SSL-beállítása.</span><span class="sxs-lookup"><span data-stu-id="85078-140">Note: Depending on how your applications are configured and what features are being used in your App Service Environment, you will need tooforward traffic for TCP ports other than 80 and 443, e.g. if you have IP SSL setup for a Web App.</span></span> <span data-ttu-id="85078-141">Az App Service Environment-környezetek használt hálózati portok listája, tekintse meg túl[vezérlő bejövő forgalom dokumentáció](app-service-app-service-environment-control-inbound-traffic.md) hálózati portok szakasz.</span><span class="sxs-lookup"><span data-stu-id="85078-141">For a list of network ports used in App Service Environments, please refer too[Control Inbound Traffic documentation's](app-service-app-service-environment-control-inbound-traffic.md) Network Ports section.</span></span>
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a><span data-ttu-id="85078-142">Microsoft Azure Traffic Managerben (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="85078-142">Configuring Microsoft Azure Traffic Manager (OPTIONAL)</span></span>
<span data-ttu-id="85078-143">Ha az alkalmazás érhető el több régióba, akkor érdemes választani tooload egyenleg őket mögött [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="85078-143">If your application is available in multiple regions, then you would want tooload balance them behind [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span></span> <span data-ttu-id="85078-144">úgy adhat hozzá a végpont hello toodo [a klasszikus Azure portálon](https://manage.azure.com) hello felhőalapú szolgáltatás nevét a WAF használatát hello Traffic Manager-profilt, az alábbi hello ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="85078-144">toodo so you can add an endpoint in hello [Azure classic portal](https://manage.azure.com) using hello Cloud Service name for your WAF in hello Traffic Manager profile as shown in hello image below.</span></span> 

![Traffic Manager-végpontot][TrafficManagerEndpoint]

<span data-ttu-id="85078-146">Ha az alkalmazás hitelesítést igényel, ellenőrizze, hogy néhány erőforrás, amely nem igényel a hitelesítés a Traffic Manager tooping hello rendelkezésre álláshoz az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="85078-146">If your application requires authentication, ensure you have some resource that doesn't require any authentication for Traffic Manager tooping for hello availability of your application.</span></span> <span data-ttu-id="85078-147">Beállíthatja a konfigurálási csoportban hello hello URL hello [a klasszikus Azure portálon](https://manage.azure.com) alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="85078-147">You can configure hello URL under hello Configure section on hello [Azure classic portal](https://manage.azure.com) as shown below.</span></span>

![A Traffic Manager konfigurálása][ConfigureTrafficManager]

<span data-ttu-id="85078-149">tooforward hello Traffic Manager ping-üzenetek az WAF tooyour alkalmazásról, kell toosetup webhely fordítások az Barracuda WAF tooforward forgalom tooyour alkalmazásra az alábbi hello példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="85078-149">tooforward hello Traffic Manager pings from your WAF tooyour application, you need toosetup Website Translations on your Barracuda WAF tooforward traffic tooyour application as shown in hello example below.</span></span>

![Webhely fordítások][WebsiteTranslations]

## <a name="securing-traffic-tooapp-service-environment-using-network-security-groups-nsg"></a><span data-ttu-id="85078-151">Forgalom tooApp szolgáltatási környezet használata hálózati biztonsági csoportok (NSG) védelmének biztosítása</span><span class="sxs-lookup"><span data-stu-id="85078-151">Securing Traffic tooApp Service Environment Using Network Security Groups (NSG)</span></span>
<span data-ttu-id="85078-152">Hajtsa végre a hello [vezérlő bejövő forgalom dokumentáció](app-service-app-service-environment-control-inbound-traffic.md) vonatkozó részletek korlátozása tooyour App Service Environment-környezet hello WAF a forgalmat, csak a felhőalapú szolgáltatás hello virtuális IP-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="85078-152">Follow hello [Control Inbound Traffic documentation](app-service-app-service-environment-control-inbound-traffic.md) for details on restricting traffic tooyour App Service Environment from hello WAF only by using hello VIP address of your Cloud Service.</span></span> <span data-ttu-id="85078-153">Íme egy minta Powershell-parancsot a 80-as TCP-port a feladat végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="85078-153">Here's a sample Powershell command for performing this task for TCP port 80.</span></span>

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

<span data-ttu-id="85078-154">Cserélje le hello SourceAddressPrefix hello Virtual IP Address (VIP) a WAF felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="85078-154">Replace hello SourceAddressPrefix with hello Virtual IP Address (VIP) of your WAF's Cloud Service.</span></span>

> <span data-ttu-id="85078-155">Megjegyzés: a felhőszolgáltatás VIP hello változik, törölje és hozza létre a felhőalapú szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="85078-155">Note: hello VIP of your Cloud Service will change when you delete and re-create hello Cloud Service.</span></span> <span data-ttu-id="85078-156">Győződjön meg arról, hogy tooupdate hello IP-cím hello hálózati erőforráscsoporthoz tartozik, a telepítést követően.</span><span class="sxs-lookup"><span data-stu-id="85078-156">Make sure tooupdate hello IP address in hello Network Resource group once you do so.</span></span> 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
