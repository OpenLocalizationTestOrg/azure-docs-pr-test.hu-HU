---
title: Az Azure App Service hibrid kapcsolatok |} Microsoft Docs
description: "Hozzon létre és különálló hálózaton lévő erőforrások eléréséhez használhatnak hibrid kapcsolatokat"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: fef9e7b280387934cb093f51b4c8aa134a3b87e7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-hybrid-connections"></a><span data-ttu-id="10f41-103">Az Azure App Service hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="10f41-103">Azure App Service Hybrid Connections</span></span> #

## <a name="overview"></a><span data-ttu-id="10f41-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="10f41-104">Overview</span></span> ##

<span data-ttu-id="10f41-105">Hibrid kapcsolatok, mind az Azure-szolgáltatás, valamint az Azure App Service szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="10f41-105">Hybrid Connections is both a service in Azure as well as a feature in the Azure App Service.</span></span>  <span data-ttu-id="10f41-106">Szolgáltatásként van használatban és képességekkel ruházzák, amely az Azure App Service használja.</span><span class="sxs-lookup"><span data-stu-id="10f41-106">As a service it has use and capabilities beyond those that are leveraged in the Azure App Service.</span></span>  <span data-ttu-id="10f41-107">További információt a hibrid kapcsolatok és az Azure App Service itt kezdheti kívül használatát [Azure hibrid kapcsolatok][HCService]</span><span class="sxs-lookup"><span data-stu-id="10f41-107">To learn more about Hybrid Connections and their usage outside of the Azure App Service you can start here, [Azure Relay Hybrid Connections][HCService]</span></span>

<span data-ttu-id="10f41-108">Az Azure App Service-ben belül hibrid kapcsolatok használható más hálózatokkal alkalmazás erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="10f41-108">Within the Azure App Service, hybrid connections can be used to access application resources in other networks.</span></span> <span data-ttu-id="10f41-109">Egy alkalmazás végpontjának a alkalmazás hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="10f41-109">It provides access FROM your app TO an application endpoint.</span></span>  <span data-ttu-id="10f41-110">Nem engedélyezi az alternatív magasabb az alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="10f41-110">It does not enable an alternative capability to access your application.</span></span>  <span data-ttu-id="10f41-111">Az App Service az alkalmazott, minden egyes hibrid kapcsolat felel meg egyetlen TCP-állomás és port kombinációját.</span><span class="sxs-lookup"><span data-stu-id="10f41-111">As used in the App Service, each hybrid connection correlates to a single TCP host and port combination.</span></span>  <span data-ttu-id="10f41-112">Ez azt jelenti, hogy a hibrid kapcsolat végpont lehet operációs rendszert, és bármely alkalmazás feltéve, hogy elérte a TCP-figyelési port-e, hogy.</span><span class="sxs-lookup"><span data-stu-id="10f41-112">This means that the hybrid connection endpoint can be on any operating system and any application provided you are hitting a TCP listening port.</span></span> <span data-ttu-id="10f41-113">Hibrid kapcsolatok nem tudja, vagy ügyeljen arra, a protokoll van, vagy mi elérésére.</span><span class="sxs-lookup"><span data-stu-id="10f41-113">Hybrid connections does not know or care what the application protocol is or what you are accessing.</span></span>  <span data-ttu-id="10f41-114">Hálózati hozzáférés egyszerűen is ellát.</span><span class="sxs-lookup"><span data-stu-id="10f41-114">It is simply providing network access.</span></span>  


## <a name="how-it-works"></a><span data-ttu-id="10f41-115">Működés</span><span class="sxs-lookup"><span data-stu-id="10f41-115">How it works</span></span> ##
<span data-ttu-id="10f41-116">A hibrid kapcsolatok szolgáltatás Service Bus Relay két kimenő hívásainak foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="10f41-116">The hybrid connections feature consists of two outbound calls to Service Bus Relay.</span></span>  <span data-ttu-id="10f41-117">Van-e az állomáson, ahol az az app Service szolgáltatásban működik a webalkalmazás és majd van egy kapcsolat és a Service Bus relay a hibrid kapcsolat Manager(HCM) tárból kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="10f41-117">There is a connection from a library on the host where your app is running in the app service and then there is a connection from the Hybrid Connection Manager(HCM) to Service Bus relay.</span></span>  <span data-ttu-id="10f41-118">A HCM a továbbítási szolgáltatás, amely telepít, a hálózati helyet adó belül</span><span class="sxs-lookup"><span data-stu-id="10f41-118">The HCM is a relay service that you deploy within the network hosting</span></span> 

<span data-ttu-id="10f41-119">A két illesztett kapcsolatokon keresztül az alkalmazás rendelkezik egy rögzített gazdagépet: port kombinációján TCP alagút a HCM másik oldalán.</span><span class="sxs-lookup"><span data-stu-id="10f41-119">Through the two joined connections your app has a TCP tunnel to a fixed host:port combination on the other side of the HCM.</span></span>  <span data-ttu-id="10f41-120">A kapcsolat biztonsági és a SAS-kulcs a hitelesítési/engedélyezési használja a TLS 1.2-es.</span><span class="sxs-lookup"><span data-stu-id="10f41-120">The connection uses TLS 1.2 for security and SAS keys for authentication/authorization.</span></span>    

![][1]

<span data-ttu-id="10f41-121">Ha az alkalmazás, amely megfelel a konfigurálása a hibrid kapcsolat végpont DNS kérést, majd a kimenő TCP-forgalmat a rendszer átirányítja a hibrid kapcsolat le.</span><span class="sxs-lookup"><span data-stu-id="10f41-121">When your app makes a DNS request that matches a configure Hybrid Connection endpoint, then the outbound TCP traffic will be redirected down the hybrid connection.</span></span>  

> [!NOTE]
> <span data-ttu-id="10f41-122">Ez azt jelenti, hogy egy DNS-nevet a hibrid kapcsolat mindig használandó használja.</span><span class="sxs-lookup"><span data-stu-id="10f41-122">This means that you should try to always use a DNS name for your Hybrid Connection.</span></span>  <span data-ttu-id="10f41-123">Néhány ügyfélszoftver nem teendő DNS-címkeresést, ha a végpont egy IP-címet használja helyette.</span><span class="sxs-lookup"><span data-stu-id="10f41-123">Some client software does not do a DNS lookup if the endpoint uses an IP address instead.</span></span>
>
>

<span data-ttu-id="10f41-124">A hibrid kapcsolatok, az új hibrid kapcsolatok az Azure-továbbító szolgáltatás felajánlott és a régebbi BizTalk hibrid kapcsolatok két típusa van.</span><span class="sxs-lookup"><span data-stu-id="10f41-124">There are two types of hybrid connections, the new hybrid connections that are offered as a service under Azure Relay and the older BizTalk Hybrid Connections.</span></span>  <span data-ttu-id="10f41-125">A régebbi BizTalk hibrid kapcsolatok nevezzük klasszikus hibrid kapcsolatok a portálon.</span><span class="sxs-lookup"><span data-stu-id="10f41-125">The older BizTalk Hybrid Connections are referred to as Classic Hybrid Connections in the portal.</span></span>  <span data-ttu-id="10f41-126">Nincs további információ a jelen dokumentum azokat.</span><span class="sxs-lookup"><span data-stu-id="10f41-126">There is more information later in this document about them.</span></span>

### <a name="app-service-hybrid-connection-benefits"></a><span data-ttu-id="10f41-127">App Service hibrid kapcsolat előnyei</span><span class="sxs-lookup"><span data-stu-id="10f41-127">App Service hybrid connection benefits</span></span> ###

<span data-ttu-id="10f41-128">A hibrid kapcsolatok funkció köztük előnyt több</span><span class="sxs-lookup"><span data-stu-id="10f41-128">There are a number of benefits to the hybrid connections capability including</span></span>

- <span data-ttu-id="10f41-129">Alkalmazások biztonságosan férjenek hozzá a helyi rendszeren és a szolgáltatások biztonságosan</span><span class="sxs-lookup"><span data-stu-id="10f41-129">Apps can securely access on premises systems and services securely</span></span>
- <span data-ttu-id="10f41-130">a szolgáltatás nem szükséges az interneten elérhető végponton</span><span class="sxs-lookup"><span data-stu-id="10f41-130">the feature does not require an internet accessible endpoint</span></span>
- <span data-ttu-id="10f41-131">gyorsan és egyszerűen állíthat be</span><span class="sxs-lookup"><span data-stu-id="10f41-131">it is quick and easy to set up</span></span>  
- <span data-ttu-id="10f41-132">kiváló biztonsági eleme, amely egyetlen állomás: port kombinációjához megfelel minden a hibrid kapcsolat</span><span class="sxs-lookup"><span data-stu-id="10f41-132">each hybrid connection matches to a single host:port combination which is an excellent security aspect</span></span>
- <span data-ttu-id="10f41-133">általában nem szükséges tűzfal lyuk a kapcsolatok vannak az összes kimenő szokásos webes portokon keresztül</span><span class="sxs-lookup"><span data-stu-id="10f41-133">it normally does not require firewall holes as the connections are all outbound over standard web ports</span></span>
- <span data-ttu-id="10f41-134">mivel a szolgáltatás, amely azt is jelenti, hogy a hálózati szint is független az alkalmazás által használt nyelv és a végpont által használt technológiára</span><span class="sxs-lookup"><span data-stu-id="10f41-134">because the feature is network level that also means that it is agnostic to the language used by your app and the technology used by the endpoint</span></span>
- <span data-ttu-id="10f41-135">több hálózat egyetlen alkalmazásból hozzáférést biztosíthat használat</span><span class="sxs-lookup"><span data-stu-id="10f41-135">it can be used to provide access in multiple networks from a single app</span></span> 

### <a name="things-you-cannot-do-with-hybrid-connections"></a><span data-ttu-id="10f41-136">Művelet nem hajtható végre a hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="10f41-136">Things you cannot do with Hybrid Connections</span></span> ###

<span data-ttu-id="10f41-137">Néhány dolgot hibrid kapcsolatokkal nem hajtható végre, és tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="10f41-137">There are a few things you cannot do with hybrid connections and they include:</span></span>

- <span data-ttu-id="10f41-138">a meghajtó csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="10f41-138">mounting a drive</span></span>
- <span data-ttu-id="10f41-139">UDP segítségével</span><span class="sxs-lookup"><span data-stu-id="10f41-139">using UDP</span></span>
- <span data-ttu-id="10f41-140">fér hozzá a TCP-alapú szolgáltatásokat, például a passzív FTP-módban, vagy passzív módban bővített dinamikus portok használatára</span><span class="sxs-lookup"><span data-stu-id="10f41-140">accessing TCP based services that use dynamic ports such as FTP Passive Mode or Extended Passive Mode</span></span>
- <span data-ttu-id="10f41-141">LDAP-támogatás, ahogy néha szükséges UDP</span><span class="sxs-lookup"><span data-stu-id="10f41-141">LDAP support, as it sometimes requires UDP</span></span>
- <span data-ttu-id="10f41-142">Active Directory-támogatás</span><span class="sxs-lookup"><span data-stu-id="10f41-142">Active Directory support</span></span>

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a><span data-ttu-id="10f41-143">Hozzáadása és az alkalmazás egy hibrid kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="10f41-143">Adding and Creating a Hybrid Connection in your App</span></span> ##

<span data-ttu-id="10f41-144">Hibrid kapcsolatok is létrehozható, az alkalmazás portálon keresztül vagy a hibrid kapcsolat szolgáltatás portálján.</span><span class="sxs-lookup"><span data-stu-id="10f41-144">Hybrid Connections can be created through the app portal or from the Hybrid Connection service portal.</span></span>  <span data-ttu-id="10f41-145">Erősen ajánlott, hogy az alkalmazás-portál használatával a hibrid kapcsolatok szeretné használni az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="10f41-145">It is highly recommended that you use the app portal to create the Hybrid Connections you wish to use with your app.</span></span>  <span data-ttu-id="10f41-146">A hibrid kapcsolat hozhatja létre a [Azure-portálon] [ portal] és használja fel az alkalmazás felhasználói felülete.</span><span class="sxs-lookup"><span data-stu-id="10f41-146">To create a Hybrid Connection go to the [Azure portal][portal] and go into the UI for your app.</span></span>  <span data-ttu-id="10f41-147">Válassza ki **hálózati > hibrid kapcsolati végpontok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="10f41-147">Select **Networking > Configure your hybrid connection endpoints**.</span></span>  <span data-ttu-id="10f41-148">Itt látható az alkalmazás használatára konfigurált hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="10f41-148">From here you can see the Hybrid Connections that are configured with your app</span></span>  

![][2]

<span data-ttu-id="10f41-149">Új hibrid kapcsolat hozzáadásához kattintson a hibrid kapcsolat hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="10f41-149">To add a new Hybrid Connection, click Add Hybrid Connection.</span></span>  <span data-ttu-id="10f41-150">A felhasználói felület, a megnyitott a hibrid kapcsolatok már létrehozott sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="10f41-150">The UI that opens up lists the Hybrid Connections that you have already created.</span></span>  <span data-ttu-id="10f41-151">Az alkalmazás egy vagy több hozzáadásához kattintson azokra szeretné, majd nyomja le **hozzáadása a hibrid kapcsolat kijelölt**.</span><span class="sxs-lookup"><span data-stu-id="10f41-151">To add one or more of them to your app, click on the ones you want and hit **Add selected hybrid connection**.</span></span>  

![][3]

<span data-ttu-id="10f41-152">Új hibrid kapcsolat létrehozásához, kattintson a **új hibrid kapcsolat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="10f41-152">If you want to create a new Hybrid Connection, click **Create new hybrid connection**.</span></span>  <span data-ttu-id="10f41-153">Itt adja meg a:</span><span class="sxs-lookup"><span data-stu-id="10f41-153">From here you specify the:</span></span> 

- <span data-ttu-id="10f41-154">a végpont neve</span><span class="sxs-lookup"><span data-stu-id="10f41-154">endpoint name</span></span>
- <span data-ttu-id="10f41-155">végpont állomásneve</span><span class="sxs-lookup"><span data-stu-id="10f41-155">endpoint hostname</span></span>
- <span data-ttu-id="10f41-156">Végpont portja</span><span class="sxs-lookup"><span data-stu-id="10f41-156">endpoint port</span></span>
- <span data-ttu-id="10f41-157">a szolgáltatásbusz-névtér szeretné használni</span><span class="sxs-lookup"><span data-stu-id="10f41-157">servicebus namespace you wish to use</span></span>

![][4]

<span data-ttu-id="10f41-158">Minden hibrid kapcsolat egy service bus-névtér van kötve, és minden egyes service bus-névtér az Azure-régióban van.</span><span class="sxs-lookup"><span data-stu-id="10f41-158">Every hybrid connection is tied to a service bus namespace and each service bus namespace is in an Azure region.</span></span>  <span data-ttu-id="10f41-159">Fontos, próbálja meg, és a service bus-névtér használata ugyanabban a régióban, az alkalmazás előidézett hálózati késleltetés elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="10f41-159">It is important to try and use a service bus namespace in the same region as your app so as to avoid network induced latency.</span></span>

<span data-ttu-id="10f41-160">Ha szeretné a hibrid kapcsolat eltávolítása az alkalmazásból, a jobb gombbal kattintson rá, és válassza ki **Disconnect**.</span><span class="sxs-lookup"><span data-stu-id="10f41-160">If you want to remove your hybrid connection from your app, right click on it and select **Disconnect**.</span></span>  

<span data-ttu-id="10f41-161">A hibrid kapcsolat hozzáadása a webalkalmazáshoz után részletes információkat tekinthet meg egyszerűen kattintással.</span><span class="sxs-lookup"><span data-stu-id="10f41-161">Once a hybrid connection is added to your web app, you can see details on it by simply clicking on it.</span></span>  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a><span data-ttu-id="10f41-162">Hibrid kapcsolatok és az App Service-csomagokról</span><span class="sxs-lookup"><span data-stu-id="10f41-162">Hybrid Connections and App Service Plans</span></span> ##

<span data-ttu-id="10f41-163">A csak hibrid kapcsolatok hajtsa végre az új hibrid kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="10f41-163">The only hybrid connections you can now make are the new hybrid connections.</span></span>  <span data-ttu-id="10f41-164">Csak azok a Basic, Standard, Premium és termékváltozatok árképzési elszigetelt érhető el.</span><span class="sxs-lookup"><span data-stu-id="10f41-164">They are only available in Basic, Standard, Premium and Isolated pricing SKUs.</span></span>  <span data-ttu-id="10f41-165">Nincsenek korlátai, a tarifacsomag kötődik.</span><span class="sxs-lookup"><span data-stu-id="10f41-165">There are limits tied to the pricing plan.</span></span>  

| <span data-ttu-id="10f41-166">Terv díjszabása</span><span class="sxs-lookup"><span data-stu-id="10f41-166">Pricing Plan</span></span> | <span data-ttu-id="10f41-167">A terv felhasználható hibrid kapcsolatok száma</span><span class="sxs-lookup"><span data-stu-id="10f41-167">Number of hybrid connections usable in the plan</span></span> |
|----|----|
| <span data-ttu-id="10f41-168">Basic</span><span class="sxs-lookup"><span data-stu-id="10f41-168">Basic</span></span> | <span data-ttu-id="10f41-169">5</span><span class="sxs-lookup"><span data-stu-id="10f41-169">5</span></span> |
| <span data-ttu-id="10f41-170">Standard</span><span class="sxs-lookup"><span data-stu-id="10f41-170">Standard</span></span> | <span data-ttu-id="10f41-171">25</span><span class="sxs-lookup"><span data-stu-id="10f41-171">25</span></span> |
| <span data-ttu-id="10f41-172">Prémium</span><span class="sxs-lookup"><span data-stu-id="10f41-172">Premium</span></span> | <span data-ttu-id="10f41-173">200</span><span class="sxs-lookup"><span data-stu-id="10f41-173">200</span></span> |
| <span data-ttu-id="10f41-174">Izolált</span><span class="sxs-lookup"><span data-stu-id="10f41-174">Isolated</span></span> | <span data-ttu-id="10f41-175">200</span><span class="sxs-lookup"><span data-stu-id="10f41-175">200</span></span> |

<span data-ttu-id="10f41-176">Mivel az App Service-csomag korlátozások is van az App Service-csomag, amely bemutatja, hogy hány hibrid kapcsolatok használatban vannak, és milyen alkalmazások által használt felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="10f41-176">Since there are App Service Plan restrictions there is also UI in the App Service Plan that shows you how many hybrid connections are being used and by what apps.</span></span>  

![][6]

<span data-ttu-id="10f41-177">Ahogy a alkalmazás nézet, részletes információkat tekinthet a hibrid kapcsolat azt.</span><span class="sxs-lookup"><span data-stu-id="10f41-177">Just as with the app view, you can see details on your hybrid connection by clicking on it.</span></span>  <span data-ttu-id="10f41-178">Az itt látható tulajdonságok is az összes információt, az alkalmazás megtekintése volt, de is láthatja, hogy hány más alkalmazások az azonos App Service-csomag használ, hogy a hibrid kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="10f41-178">In the properties shown here you can see all the information you had at the app view but can also see how many other apps in the same App Service Plan are using that hybrid connection.</span></span>

![][7]

<span data-ttu-id="10f41-179">Hibrid kapcsolati végpontok, amely egy App Service-csomag használható száma korlátozva van, amíg minden egyes használt hibrid kapcsolat az adott App Service-csomag alkalmazások tetszőleges számú segítségével.</span><span class="sxs-lookup"><span data-stu-id="10f41-179">While there is a limit on the number of hybrid connection endpoints that can be used in an App Service Plan, each hybrid connection used can be used across any number of apps in that App Service Plan.</span></span>  <span data-ttu-id="10f41-180">Tegyük fel például, hogy ha 1 hibrid kapcsolat az App Service-csomag az 5 külön alkalmazásokban használt, ez továbbra is csak 1 hibrid kapcsolat is.</span><span class="sxs-lookup"><span data-stu-id="10f41-180">That is to say that if I had 1 hybrid connection that I used in 5 separate apps in my App Service Plan, that is still just 1 hybrid connection.</span></span>

<span data-ttu-id="10f41-181">A hibrid kapcsolatok túl alatt csak a Basic, Standard, Premium vagy elkülönített SKU felhasználható további költségek van.</span><span class="sxs-lookup"><span data-stu-id="10f41-181">There is an additional cost to hybrid connections beyond being only usable in a Basic, Standard, Premium or Isolated SKU.</span></span>  <span data-ttu-id="10f41-182">A hibrid kapcsolat árakkal kapcsolatos részletekért nyissa meg itt: [Service Bus árképzési][sbpricing].</span><span class="sxs-lookup"><span data-stu-id="10f41-182">For details on hybrid connection pricing please go here: [Service Bus pricing][sbpricing].</span></span>

## <a name="hybrid-connection-manager"></a><span data-ttu-id="10f41-183">Hibrid Csatlakozáskezelő</span><span class="sxs-lookup"><span data-stu-id="10f41-183">Hybrid Connection Manager</span></span> ##

<span data-ttu-id="10f41-184">Ahhoz, hogy hibrid kapcsolatok működéséhez van szüksége a hálózat, amelyen a hibrid kapcsolat végpont továbbító ügynök.</span><span class="sxs-lookup"><span data-stu-id="10f41-184">In order for hybrid connections to work you need a relay agent in the network that hosts your hybrid connection endpoint.</span></span>  <span data-ttu-id="10f41-185">A továbbító ügynök neve a hibrid kapcsolat Manager (HCM).</span><span class="sxs-lookup"><span data-stu-id="10f41-185">That relay agent is called the Hybrid Connection Manager (HCM).</span></span>  <span data-ttu-id="10f41-186">Ez az eszköz letölthető a **hálózati > hibrid kapcsolati végpontok konfigurálása** érhető el az alkalmazást a felhasználói felület a [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="10f41-186">This tool can be downloaded from the **Networking > Configure your hybrid connection endpoints** UI available from your app in the [Azure portal][portal].</span></span>  

<span data-ttu-id="10f41-187">Ez az eszköz fut, a Windows server 2008 R2 és a Windows.</span><span class="sxs-lookup"><span data-stu-id="10f41-187">This tool runs on Windows server 2008 R2 and later versions of Windows.</span></span>  <span data-ttu-id="10f41-188">Egyszer az HCM fut a szolgáltatásként telepített.</span><span class="sxs-lookup"><span data-stu-id="10f41-188">Once installed the HCM runs as a service.</span></span>  <span data-ttu-id="10f41-189">Ez a szolgáltatás a beállított végpontjaikra alapján Azure szolgáltatásbusz-továbbító csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="10f41-189">This service connects to Azure servicebus relay based on the configured endpoints.</span></span>  <span data-ttu-id="10f41-190">A HCM közötti kapcsolatok kimenő 80-as és 443-as portra.</span><span class="sxs-lookup"><span data-stu-id="10f41-190">The connections from the HCM are outbound to ports 80 and 443.</span></span>    

<span data-ttu-id="10f41-191">A HCM rendelkezik konfigurálásának felhasználói Felületet.</span><span class="sxs-lookup"><span data-stu-id="10f41-191">The HCM has a UI to configure it.</span></span>  <span data-ttu-id="10f41-192">Miután telepítette a HCM van lehetősége a felhasználói felület a HybridConnectionManagerUi.exe, amely futtatja a Hybrid Connection Manager telepítő könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="10f41-192">After the HCM is installed you can bring up the UI by running the HybridConnectionManagerUi.exe that sits in the Hybrid Connection Manager installation directory.</span></span>  <span data-ttu-id="10f41-193">Azt is könnyen elérésekor a Windows 10 beírásával *Hybrid Connection Manager felhasználói felületén* be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="10f41-193">It is also easily reached on Windows 10 by typing *Hybrid Connection Manager UI* in your search box.</span></span>  

<span data-ttu-id="10f41-194">A HCM felhasználói felületének indításakor az első nevezzük a hibrid kapcsolatok ezen példánya számára a HCM használatára konfigurált összes tábla.</span><span class="sxs-lookup"><span data-stu-id="10f41-194">When the HCM UI is started, the first thing you see is a table that lists all of the hybrid connections that are configured with this instance of the HCM.</span></span>  <span data-ttu-id="10f41-195">Ha módosítani kívánja szüksége lesz Azure szolgáltatással való hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="10f41-195">If you wish to make any changes you will need to authenticate with Azure.</span></span> 

<span data-ttu-id="10f41-196">Egy vagy több hibrid kapcsolat hozzáadása a HCM:</span><span class="sxs-lookup"><span data-stu-id="10f41-196">To add one or more Hybrid Connections to your HCM:</span></span>

1. <span data-ttu-id="10f41-197">Indítsa el a HCM felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="10f41-197">Start the HCM UI</span></span>
1. <span data-ttu-id="10f41-198">Kattintson egy másik hibrid kapcsolat konfigurálása![][8]</span><span class="sxs-lookup"><span data-stu-id="10f41-198">Click Configure another hybrid connection ![][8]</span></span>

1. <span data-ttu-id="10f41-199">Jelentkezzen be az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="10f41-199">Sign in with your Azure account</span></span>
1. <span data-ttu-id="10f41-200">Előfizetés kiválasztása</span><span class="sxs-lookup"><span data-stu-id="10f41-200">Choose a subscription</span></span>
1. <span data-ttu-id="10f41-201">Kattintson a HCM továbbítani szeretné a hibrid kapcsolatok használata esetén![][9]</span><span class="sxs-lookup"><span data-stu-id="10f41-201">Click on the hybrid connections you want this HCM to relay ![][9]</span></span>

1. <span data-ttu-id="10f41-202">Kattintson a Save (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="10f41-202">Click Save</span></span>

<span data-ttu-id="10f41-203">Ekkor megjelenik a hibrid kapcsolatok hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="10f41-203">At this point you will see the hybrid connections you added.</span></span>  <span data-ttu-id="10f41-204">Kattintson a konfigurált hibrid kapcsolaton is, és a kapcsolatra vonatkozó részletes.</span><span class="sxs-lookup"><span data-stu-id="10f41-204">You can also click on the configured hybrid connection and see details about the connection.</span></span>

![][10]

<span data-ttu-id="10f41-205">A HCM tudjanak támogatják a hibrid kapcsolatok portadatokkal lesz konfigurálva, a szükséges:</span><span class="sxs-lookup"><span data-stu-id="10f41-205">For your HCM to be able to support the hybrid connections it is configured with, it needs:</span></span>

- <span data-ttu-id="10f41-206">TCP 80-as és 443-as porton keresztül Azure eléréséhez</span><span class="sxs-lookup"><span data-stu-id="10f41-206">TCP access to Azure over ports 80 and 443</span></span>
- <span data-ttu-id="10f41-207">A hibrid kapcsolat végpont TCP elérésére</span><span class="sxs-lookup"><span data-stu-id="10f41-207">TCP access to the hybrid connection endpoint</span></span>
- <span data-ttu-id="10f41-208">Ehhez a DNS képes a végpont-állomás és az azure szolgáltatásbusz-névtér keresések</span><span class="sxs-lookup"><span data-stu-id="10f41-208">ability to do DNS look ups on the endpoint host and the azure servicebus namespace</span></span>

<span data-ttu-id="10f41-209">A HCM támogatja az új hibrid kapcsolatok, valamint a régebbi BizTalk hibrid kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="10f41-209">The HCM supports both new hybrid connections as well as the older BizTalk hybrid connections.</span></span>

### <a name="redundancy"></a><span data-ttu-id="10f41-210">Redundancia</span><span class="sxs-lookup"><span data-stu-id="10f41-210">Redundancy</span></span> ###

<span data-ttu-id="10f41-211">Minden egyes HCM támogathatja több hibrid kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="10f41-211">Each HCM can support multiple hybrid connections.</span></span>  <span data-ttu-id="10f41-212">Ezenkívül egy adott hibrid kapcsolathoz több HCMs használható.</span><span class="sxs-lookup"><span data-stu-id="10f41-212">Also, any given hybrid connection can be supported by multiple HCMs.</span></span>  <span data-ttu-id="10f41-213">A megadott végpont konfigurált HCMs keresztül ciklikus időszeletelési forgalom az alapértelmezés lesz.</span><span class="sxs-lookup"><span data-stu-id="10f41-213">The default behavior is to round robin traffic across the configured HCMs for any given endpoint.</span></span>  <span data-ttu-id="10f41-214">Ha magas rendelkezésre állás a hibrid kapcsolatok használata esetén a hálózatról, egyszerűen hozható létre több HCMs külön gépeken.</span><span class="sxs-lookup"><span data-stu-id="10f41-214">If you want high availability on your hybrid connections from your network, simply instantiate multiple HCMs on separate machines.</span></span> 

### <a name="manually-adding-a-hybrid-connection"></a><span data-ttu-id="10f41-215">Hibrid kapcsolat manuális hozzáadása</span><span class="sxs-lookup"><span data-stu-id="10f41-215">Manually adding a hybrid connection</span></span> ###

<span data-ttu-id="10f41-216">Ha valaki kívül az előfizetés egy adott a hibrid kapcsolat HCM példány futtatására, megoszthatja velük a hibrid kapcsolat átjáró kapcsolati karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="10f41-216">If you wish somebody outside of your subscription to host an HCM instance for a given hybrid connection, you can share with them the gateway connection string for the hybrid connection.</span></span>  <span data-ttu-id="10f41-217">Ez a hibrid kapcsolat tulajdonságaiban megtekintheti a [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="10f41-217">You can see this in the properties for a hybrid connection in the [Azure portal][portal].</span></span> <span data-ttu-id="10f41-218">A karakterlánc használatához kattintson a **kézzel konfigurálásához** a HCM gombra, és illessze be az átjáró kapcsolati karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="10f41-218">To use that string, click the **Configure manually** button in the HCM and paste in the gateway connection string.</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="10f41-219">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="10f41-219">Troubleshooting</span></span> ##

<span data-ttu-id="10f41-220">Ha egy futó alkalmazáshoz be van állítva a a hibrid kapcsolat legalább egy HCM, amely rendelkezik a hibrid kapcsolat konfigurálva van, majd megtudhatja, hogy az állapot **csatlakoztatva** a portálon.</span><span class="sxs-lookup"><span data-stu-id="10f41-220">When your hybrid connection is set up with a running application and there is at least one HCM that has that hybrid connection configured, then the status will say **Connected** in the portal.</span></span>  <span data-ttu-id="10f41-221">Ha ezt nem kéri a **csatlakoztatva** az azt jelenti, hogy vagy az alkalmazás nem működik, vagy az Azure-ba, a 80-as vagy 443-as port az a HCM nem lehet kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="10f41-221">If it does not say **Connected** then it means that either your app is down or your HCM cannot connect out to Azure on ports 80 or 443.</span></span>  

<span data-ttu-id="10f41-222">A legfőbb oka, hogy az ügyfelek nem tudnak csatlakozni a végpont az oka, hogy a végpont IP-cím helyett egy DNS-név lett megadva.</span><span class="sxs-lookup"><span data-stu-id="10f41-222">The primary reason that clients cannot connect to their endpoint is because the endpoint was specified using an IP address instead of a DNS name.</span></span>  <span data-ttu-id="10f41-223">Ha az alkalmazás nem tudja elérni a kívánt végpont, és egy IP-címet, váltson a HCM futtató egy a gazdagépen érvényes DNS-név használatával.</span><span class="sxs-lookup"><span data-stu-id="10f41-223">If your app cannot reach the desired endpoint and you used an IP address, switch to using a DNS name that is valid on the host where the HCM is running.</span></span>  <span data-ttu-id="10f41-224">Más ellenőrizze az alábbiakat, amelyek, hogy a DNS-név megfelelően feloldja a gazdagépen, amelyen fut a HCM, és hogy van-e kapcsolat a gazdagép, amelyen fut a HCM a hibrid kapcsolat végpontjához.</span><span class="sxs-lookup"><span data-stu-id="10f41-224">Other things to check are that the DNS name resolves properly on the host where the HCM is running and that there is connectivity from the host where the HCM is running to the hybrid connection endpoint.</span></span>  

<span data-ttu-id="10f41-225">Egy eszköz nincs az App Service-tcpping nevű konzolról is elindítható.</span><span class="sxs-lookup"><span data-stu-id="10f41-225">There is a tool in the App Service that can be invoked from the console called tcpping.</span></span>  <span data-ttu-id="10f41-226">Ez az eszköz segítségével megállapíthatja, hogy ha egy TCP-végpont elérésére rendelkezik, de nem közli, ha hozzáfér a hibrid kapcsolat végponthoz.</span><span class="sxs-lookup"><span data-stu-id="10f41-226">This tool can tell you if you have access to a TCP endpoint but does not tell you if you have access to a hybrid connection endpoint.</span></span>  <span data-ttu-id="10f41-227">Ha használja a konzol a hibrid kapcsolat végpontjának ellen, a sikeres ping csak jelzi, hogy rendelkezik-e konfigurálva az alkalmazáshoz, a gazdagép és a portszám kombináció használó hibrid kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="10f41-227">When used in the console against a hybrid connection endpoint, a successful ping will only tell you that you have a hybrid connection configured for your app that uses that host:port combination.</span></span>  

## <a name="biztalk-hybrid-connections"></a><span data-ttu-id="10f41-228">Hibrid kapcsolatok a BizTalk szolgáltatásokban</span><span class="sxs-lookup"><span data-stu-id="10f41-228">BizTalk Hybrid Connections</span></span> ##

<span data-ttu-id="10f41-229">A régebbi BizTalk hibrid kapcsolatok funkció le van zárva ki a további BizTalk a hibrid kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="10f41-229">The older BizTalk Hybrid Connections capability has been closed off to further BizTalk Hybrid Connection creations.</span></span>  <span data-ttu-id="10f41-230">A már meglévő BizTalk hibrid kapcsolatok használata az alkalmazások továbbra is, de át kell telepíteni az új szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="10f41-230">You can continue using your preexisting BizTalk Hybrid Connections with your apps but should migrate to the new service.</span></span>  <span data-ttu-id="10f41-231">A BizTalk verzióra az új szolgáltatás előnye között a következők:</span><span class="sxs-lookup"><span data-stu-id="10f41-231">Among the benefits in the new service over the BizTalk version are:</span></span>

- <span data-ttu-id="10f41-232">Nincsenek további BizTalk fiókot kell megadni</span><span class="sxs-lookup"><span data-stu-id="10f41-232">no additional BizTalk account is required</span></span>
- <span data-ttu-id="10f41-233">A TLS 1.2-es 1.0 hasonlóan BizTalk hibrid kapcsolatok helyett</span><span class="sxs-lookup"><span data-stu-id="10f41-233">TLS is 1.2 instead of 1.0 as in BizTalk Hybrid Connections</span></span>
- <span data-ttu-id="10f41-234">Kommunikációs port a 80-as és 443-as, a DNS-név használatával Azure helyett IP-címek és a további számos más eléréséhez portokon keresztül van.</span><span class="sxs-lookup"><span data-stu-id="10f41-234">Communication is over ports 80 and 443 using a DNS name to reach Azure instead of IP addresses and a range of additional other ports.</span></span>  

<span data-ttu-id="10f41-235">A BizTalk a hibrid kapcsolat hozzáadása az alkalmazáshoz, keresse fel az alkalmazást a a [Azure-portálon] [ portal] kattintson **hálózati > hibrid kapcsolati végpontok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="10f41-235">To add a BizTalk hybrid connection to your app, go to your app in the [Azure portal][portal] and click **Networking > Configure your hybrid connection endpoints**.</span></span>  <span data-ttu-id="10f41-236">Kattintson a klasszikus hibrid kapcsolatok tábla **klasszikus hibrid kapcsolat hozzáadása a**.</span><span class="sxs-lookup"><span data-stu-id="10f41-236">In the Classic hybrid connections table click **Add classic hybrid connection**.</span></span>  <span data-ttu-id="10f41-237">Itt látni fogja a BizTalk hibrid kapcsolatok listáját.</span><span class="sxs-lookup"><span data-stu-id="10f41-237">From here you will see a list of your BizTalk hybrid connections.</span></span>  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/

