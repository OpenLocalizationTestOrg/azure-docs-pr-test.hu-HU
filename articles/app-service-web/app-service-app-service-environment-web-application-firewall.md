---
title: "Webalkalmazási tűzfal (WAF) App Service Environment-környezet konfigurálása"
description: "Ismerje meg az App Service-környezet elé webalkalmazási tűzfal konfigurálásáról."
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
ms.openlocfilehash: 3e9e9fa4ddab60a467e8aa793ec0ca269b0bc4e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a><span data-ttu-id="ebd05-103">Webalkalmazási tűzfal (WAF) App Service Environment-környezet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebd05-103">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="ebd05-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ebd05-104">Overview</span></span>
<span data-ttu-id="ebd05-105">Webalkalmazási tűzfalak, például a [Barracuda WAF az Azure-](https://www.barracuda.com/programs/azure) , amely megtalálható a [Azure piactér](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) segítségével biztonságos a webalkalmazások, ellenőrizze a bejövő webes forgalomnak SQL alkalommal többhelyes parancsfájlok, kártevő szoftverek feltöltések letiltása & DDoS alkalmazás és más támadásoknak.</span><span class="sxs-lookup"><span data-stu-id="ebd05-105">Web application firewalls like the [Barracuda WAF for Azure](https://www.barracuda.com/programs/azure) that is available on the [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) helps secure your web applications by inspecting inbound web traffic to block SQL injections, Cross-Site Scripting, malware uploads & application DDoS and other attacks.</span></span> <span data-ttu-id="ebd05-106">Azt is ellenőrzi a válaszokat a háttérben futó webes-kiszolgálókról az adatok adatvesztés-megelőzési (DLP).</span><span class="sxs-lookup"><span data-stu-id="ebd05-106">It also inspects the responses from the back-end web servers for Data Loss Prevention (DLP).</span></span> <span data-ttu-id="ebd05-107">Az elkülönítési és az App Service Environment-környezetek által biztosított további skálázás együtt, a környezetet biztosít az ideális üzleti kritikus webes alkalmazások kell állnia, kártékony kérelmek és nagy mennyiségű forgalom.</span><span class="sxs-lookup"><span data-stu-id="ebd05-107">Combined with the isolation and additional scaling provided by App Service Environments, this provides an ideal environment to host business critical web applications that need to withstand malicious requests and high volume traffic.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a><span data-ttu-id="ebd05-108">Beállítás</span><span class="sxs-lookup"><span data-stu-id="ebd05-108">Setup</span></span>
<span data-ttu-id="ebd05-109">A dokumentum azt konfigurálja az App Service-környezet több terhelés mögött, hogy csak a WAF forgalmát képes elérni az App Service Environment-környezet, és nem lesz elérhető a Szegélyhálózaton kiegyensúlyozott Barracuda WAF példányai.</span><span class="sxs-lookup"><span data-stu-id="ebd05-109">For this document we will configure our App Service Environment behind multiple load balanced instances of Barracuda WAF so that only traffic from the WAF can reach the App Service Environment and it will not be accessible from the DMZ.</span></span> <span data-ttu-id="ebd05-110">Azt is Azure Traffic Manager elé a Barracuda WAF példányok Azure-adatközpont és régiók terheléselosztása érdekében.</span><span class="sxs-lookup"><span data-stu-id="ebd05-110">We will also have Azure Traffic Manager in front of our Barracuda WAF instances to load balance across Azure data centers and regions.</span></span> <span data-ttu-id="ebd05-111">A telepítés magas szintű diagramját néz alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="ebd05-111">A high level diagram of the setup would look like what is shown below.</span></span>

![Architektúra][Architecture] 

> <span data-ttu-id="ebd05-113">Megjegyzés: A bevezetése [ILB támogatja az App Service Environment-környezet](app-service-environment-with-internal-load-balancer.md), konfigurálhatja a ASE a Szegélyhálózaton a nem érhető el, és csak a magánhálózaton elérhetővé tenni.</span><span class="sxs-lookup"><span data-stu-id="ebd05-113">Note: With the introduction of [ILB support for App Service Environment](app-service-environment-with-internal-load-balancer.md), you can configure the ASE to be inaccessible from the DMZ and only be available to the private network.</span></span> 
> 
> 

## <a name="configuring-your-app-service-environment"></a><span data-ttu-id="ebd05-114">Az App Service-környezet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebd05-114">Configuring your App Service Environment</span></span>
<span data-ttu-id="ebd05-115">Konfigurálhatja az App Service-környezetek hivatkoznak [dokumentációban](app-service-web-how-to-create-an-app-service-environment.md) a témáról.</span><span class="sxs-lookup"><span data-stu-id="ebd05-115">To configure an App Service Environment refer to [our documentation](app-service-web-how-to-create-an-app-service-environment.md) on the subject.</span></span> <span data-ttu-id="ebd05-116">Miután egy App Service-környezet létrehozása, létrehozhat [webalkalmazások](app-service-web-overview.md), [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md) és [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) ebben a környezetben, amely az összes védi a konfigurálását a következő szakaszban végezzük WAF mögött.</span><span class="sxs-lookup"><span data-stu-id="ebd05-116">Once you have an App Service Environment created, you can create [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) and [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) in this environment that will all be protected behind the WAF we configure in the next section.</span></span>

## <a name="configuring-your-barracuda-waf-cloud-service"></a><span data-ttu-id="ebd05-117">A Barracuda WAF Felhőszolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebd05-117">Configuring your Barracuda WAF Cloud Service</span></span>
<span data-ttu-id="ebd05-118">Barracuda rendelkezik egy [részletes cikk](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) üzembe helyezni a WAF az Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="ebd05-118">Barracuda has a [detailed article](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) on deploying its WAF on a virtual machine in Azure.</span></span> <span data-ttu-id="ebd05-119">De azt szeretné, hogy redundancia, és a hibaérzékeny pontok kialakulását vezet be, mert szeretné-e legalább 2 WAF példány virtuális gép üzembe helyezés az ugyanazon a felhőalapú szolgáltatás, ha az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="ebd05-119">But because we want redundancy and not introduce a single point of failure, you want to deploy at least 2 WAF instance VMs into the same Cloud Service when following these instructions.</span></span>

### <a name="adding-endpoints-to-cloud-service"></a><span data-ttu-id="ebd05-120">A felhőalapú szolgáltatás végpontok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ebd05-120">Adding Endpoints to Cloud Service</span></span>
<span data-ttu-id="ebd05-121">Ha szükség van a 2 vagy több WAF Virtuálisgép-példányok a felhőszolgáltatásban a [Azure-portálon](https://portal.azure.com/) HTTP és HTTPS-végpontok az alábbi ábrán látható módon az alkalmazás által használt hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="ebd05-121">Once you have 2 or more WAF VM instances in your Cloud Service you can use the [Azure portal](https://portal.azure.com/) to add HTTP and HTTPS endpoints that are used by your application as shown in the image below.</span></span>

![Konfigurálja a végpontot][ConfigureEndpoint]

<span data-ttu-id="ebd05-123">Ha az alkalmazások végpontja, ne felejtse el hozzáadni azokat ebben a listában, megfelelő-e.</span><span class="sxs-lookup"><span data-stu-id="ebd05-123">If your applications use other endpoints, make sure to add those to this list as well.</span></span> 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a><span data-ttu-id="ebd05-124">A felügyeleti portálon keresztül WAF Barracuda konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebd05-124">Configuring Barracuda WAF through its Management Portal</span></span>
<span data-ttu-id="ebd05-125">Barracuda WAF TCP Port 8000 használja a konfiguráció a felügyeleti portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="ebd05-125">Barracuda WAF uses TCP Port 8000 for configuration through its management portal.</span></span> <span data-ttu-id="ebd05-126">Mivel a WAF virtuális gépek több példánya van szüksége lesz a ismételje meg a Itt minden egyes Virtuálisgép-példány.</span><span class="sxs-lookup"><span data-stu-id="ebd05-126">Since we have multiple instances of the WAF VMs you will need to repeat the steps here for each VM instance.</span></span> 

> <span data-ttu-id="ebd05-127">Megjegyzés: Ha elkészült, WAF konfigurációjával, távolítsa el a TCP/8000 végpont a WAF virtuális gépeinek a WAF biztonsága.</span><span class="sxs-lookup"><span data-stu-id="ebd05-127">Note: Once you are done with WAF configuration, remove the TCP/8000 endpoint from all your WAF VMs to keep your WAF secure.</span></span>
> 
> 

<span data-ttu-id="ebd05-128">Adja hozzá a felügyeleti végpont, a Barracuda WAF konfigurálása az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="ebd05-128">Add the management endpoint as shown in the image below to configure your Barracuda WAF.</span></span>

![Felügyeleti végpont hozzáadása][AddManagementEndpoint]

<span data-ttu-id="ebd05-130">A böngésző segítségével keresse meg a felügyeleti végpont a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ebd05-130">Use a browser to browse to the management endpoint on your Cloud Service.</span></span> <span data-ttu-id="ebd05-131">Ha a Felhőszolgáltatás neve test.cloudapp.net, ehhez a végponthoz http://test.cloudapp.net:8000 tallózással elérésére.</span><span class="sxs-lookup"><span data-stu-id="ebd05-131">If your Cloud Service is called test.cloudapp.net, you would access this endpoint by browsing to http://test.cloudapp.net:8000.</span></span> <span data-ttu-id="ebd05-132">A bejelentkezési oldal megjelenik, például alatt is bejelentkezési WAF VM telepítő szakaszban megadott hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="ebd05-132">You should see a login page like below that you can login using credentials you specified in the WAF VM setup phase.</span></span>

![Management bejelentkezési oldal][ManagementLoginPage]

<span data-ttu-id="ebd05-134">Egyszer meg bejelentkezési meg kell jelennie egy irányítópultot megegyezik a lenti képen, hogy a WAF védelmi alapvető statisztikája.</span><span class="sxs-lookup"><span data-stu-id="ebd05-134">Once you login you should see a dashboard as the one in the image below that will present basic statistics about the WAF protection.</span></span>

![Kezelési irányítópult][ManagementDashboard]

<span data-ttu-id="ebd05-136">Kattintson a szolgáltatások lapon így állíthatja be az általa védett szolgáltatásokhoz WAF.</span><span class="sxs-lookup"><span data-stu-id="ebd05-136">Clicking on the Services tab will let you configure your WAF for services it is protecting.</span></span> <span data-ttu-id="ebd05-137">A Barracuda WAF konfigurálásával kapcsolatos további részletekért tanulmányozza [dokumentum](https://techlib.barracuda.com/waf/getstarted1).</span><span class="sxs-lookup"><span data-stu-id="ebd05-137">For more details on configuring your Barracuda WAF you can consult [their documentation](https://techlib.barracuda.com/waf/getstarted1).</span></span> <span data-ttu-id="ebd05-138">Az Azure Web Apps az alábbi példában a HTTP és HTTPS-forgalmat kiszolgáló van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="ebd05-138">In the example below an Azure Web App serving traffic on HTTP and HTTPS has been configured.</span></span>

![Felügyeleti szolgáltatások hozzáadása][ManagementAddServices]

> <span data-ttu-id="ebd05-140">Megjegyzés: Attól függően, hogy az alkalmazások hogyan vannak konfigurálva, és milyen funkciók használnak az App Service-környezet, akkor továbbítja a forgalmat a TCP eltérő 80-as és 443-as port például ha egy webalkalmazás IP SSL-beállítása.</span><span class="sxs-lookup"><span data-stu-id="ebd05-140">Note: Depending on how your applications are configured and what features are being used in your App Service Environment, you will need to forward traffic for TCP ports other than 80 and 443, e.g. if you have IP SSL setup for a Web App.</span></span> <span data-ttu-id="ebd05-141">Az App Service Environment-környezetek használt hálózati portok listája, olvassa el [vezérlő bejövő forgalom dokumentáció](app-service-app-service-environment-control-inbound-traffic.md) hálózati portok szakasz.</span><span class="sxs-lookup"><span data-stu-id="ebd05-141">For a list of network ports used in App Service Environments, please refer to [Control Inbound Traffic documentation's](app-service-app-service-environment-control-inbound-traffic.md) Network Ports section.</span></span>
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a><span data-ttu-id="ebd05-142">Microsoft Azure Traffic Managerben (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="ebd05-142">Configuring Microsoft Azure Traffic Manager (OPTIONAL)</span></span>
<span data-ttu-id="ebd05-143">Az alkalmazás érhető el több régióba, akkor kell betölteni egyenleg azok mögötti [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ebd05-143">If your application is available in multiple regions, then you would want to load balance them behind [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span></span> <span data-ttu-id="ebd05-144">A végpont adhat hozzá ehhez a [a klasszikus Azure portálon](https://manage.azure.com) használatával a felhőalapú szolgáltatás nevét a WAF a Traffic Manager-profilt, az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="ebd05-144">To do so you can add an endpoint in the [Azure classic portal](https://manage.azure.com) using the Cloud Service name for your WAF in the Traffic Manager profile as shown in the image below.</span></span> 

![Traffic Manager-végpontot][TrafficManagerEndpoint]

<span data-ttu-id="ebd05-146">Ha az alkalmazás hitelesítést igényel, ellenőrizze, hogy néhány, nincs szükség a hitelesítés a Traffic Manager Pingelje meg az alkalmazás rendelkezésre állásának az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ebd05-146">If your application requires authentication, ensure you have some resource that doesn't require any authentication for Traffic Manager to ping for the availability of your application.</span></span> <span data-ttu-id="ebd05-147">Az URL-cím konfigurálása szakaszban konfigurálhatók a [a klasszikus Azure portálon](https://manage.azure.com) alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="ebd05-147">You can configure the URL under the Configure section on the [Azure classic portal](https://manage.azure.com) as shown below.</span></span>

![A Traffic Manager konfigurálása][ConfigureTrafficManager]

<span data-ttu-id="ebd05-149">A Traffic Manager ping-üzenetek továbbítása a WAF az alkalmazáshoz, hogy végre kell telepítő webhely fordítások a Barracuda WAF továbbítsa a forgalmat az alkalmazáshoz az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="ebd05-149">To forward the Traffic Manager pings from your WAF to your application, you need to setup Website Translations on your Barracuda WAF to forward traffic to your application as shown in the example below.</span></span>

![Webhely fordítások][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a><span data-ttu-id="ebd05-151">Az App Service Environment-környezet hálózati biztonsági csoportokkal (NSG) adatforgalom biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="ebd05-151">Securing Traffic to App Service Environment Using Network Security Groups (NSG)</span></span>
<span data-ttu-id="ebd05-152">Kövesse a [vezérlő bejövő forgalom dokumentáció](app-service-app-service-environment-control-inbound-traffic.md) forgalmának korlátozása az App Service-környezet a WAF a csak a VIP-cím a felhőalapú szolgáltatás használatával kapcsolatos részletekért.</span><span class="sxs-lookup"><span data-stu-id="ebd05-152">Follow the [Control Inbound Traffic documentation](app-service-app-service-environment-control-inbound-traffic.md) for details on restricting traffic to your App Service Environment from the WAF only by using the VIP address of your Cloud Service.</span></span> <span data-ttu-id="ebd05-153">Íme egy minta Powershell-parancsot a 80-as TCP-port a feladat végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="ebd05-153">Here's a sample Powershell command for performing this task for TCP port 80.</span></span>

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

<span data-ttu-id="ebd05-154">A SourceAddressPrefix cserélje le a virtuális IP-cím (VIP) a WAF felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ebd05-154">Replace the SourceAddressPrefix with the Virtual IP Address (VIP) of your WAF's Cloud Service.</span></span>

> <span data-ttu-id="ebd05-155">Megjegyzés: A felhőszolgáltatás VIP változik, törölje és hozza létre a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="ebd05-155">Note: The VIP of your Cloud Service will change when you delete and re-create the Cloud Service.</span></span> <span data-ttu-id="ebd05-156">Ügyeljen arra, hogy a hálózati csoport az IP-címének frissítése, a telepítést követően.</span><span class="sxs-lookup"><span data-stu-id="ebd05-156">Make sure to update the IP address in the Network Resource group once you do so.</span></span> 
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
