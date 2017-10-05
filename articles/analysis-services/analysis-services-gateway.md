---
title: "A helyszíni adatátjáró |} Microsoft Docs"
description: "Egy helyszíni átjárót szükség, ha az Azure-ban az Analysis Services-kiszolgálóhoz csatlakoznak a helyszíni adatforrások."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: 514b5404e8cbfa0baa657eb41736e20cad502638
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connecting-to-on-premises-data-sources-with-azure-on-premises-data-gateway"></a><span data-ttu-id="774ff-103">Csatlakozás az adatforrásokhoz helyszíni Azure a helyszíni adatok átjáróval</span><span class="sxs-lookup"><span data-stu-id="774ff-103">Connecting to on-premises data sources with Azure On-premises Data Gateway</span></span>
<span data-ttu-id="774ff-104">Az a helyszíni átjáró működik hídként, a helyszíni adatforrások és a felhőben az Azure Analysis Services-kiszolgálók közötti biztonságos adatátvitel biztosítása.</span><span class="sxs-lookup"><span data-stu-id="774ff-104">The on-premises data gateway acts as a bridge, providing secure data transfer between on-premises data sources and your Azure Analysis Services servers in the cloud.</span></span> <span data-ttu-id="774ff-105">Mellett több Azure Analysis Services-kiszolgáló ugyanabban a régióban dolgozik, az átjáró legújabb verzióját is működik az Azure Logic Apps, a Power bi-ban, a kiemelt alkalmazások és a Microsoft Flow.</span><span class="sxs-lookup"><span data-stu-id="774ff-105">In addition to working with multiple Azure Analysis Services servers in the same region, the latest version of the gateway also works with Azure Logic Apps, Power BI, Power Apps, and Microsoft Flow.</span></span> <span data-ttu-id="774ff-106">Csak egyetlen átjáró ugyanabban a régióban több szolgáltatáshoz is társíthat.</span><span class="sxs-lookup"><span data-stu-id="774ff-106">You can associate multiple services in the same region with a single gateway.</span></span> 

 <span data-ttu-id="774ff-107">Az Azure Analysis Services egy átjáró-erőforráshoz ugyanabban a régióban van szükség.</span><span class="sxs-lookup"><span data-stu-id="774ff-107">Azure Analysis Services requires a gateway resource in the same region.</span></span> <span data-ttu-id="774ff-108">Például ha Azure Analysis Services-kiszolgálók az USA keleti régiója 2 régióban, kell egy átjáró-erőforráshoz régióban USA keleti régiója 2.</span><span class="sxs-lookup"><span data-stu-id="774ff-108">For example, if you have Azure Analysis Services servers in the East US 2 region, you need a gateway resource in the East US 2 region.</span></span> <span data-ttu-id="774ff-109">USA keleti régiója 2 több kiszolgáló használható ugyanahhoz az átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="774ff-109">Multiple servers in East US 2 can use the same gateway.</span></span>

<span data-ttu-id="774ff-110">Az átjáró telepítése az első alkalommal első az egy négyrészes folyamat:</span><span class="sxs-lookup"><span data-stu-id="774ff-110">Getting setup with the gateway the first time is a four-part process:</span></span>

- <span data-ttu-id="774ff-111">**Töltse le és futtassa a telepítőt** – Ez a lépés telepíti egy átjáró szolgáltatás a szervezet egyik számítógépén.</span><span class="sxs-lookup"><span data-stu-id="774ff-111">**Download and run setup** - This step installs a gateway service on a computer in your organization.</span></span>

- <span data-ttu-id="774ff-112">**Regisztrálnia kell az átjárót** – ebben a lépésben adjon nevet és helyreállítási az átjáró kulcsát, és válasszon ki egy régiót, az átjáró regisztrálása az átjáró Felhőszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="774ff-112">**Register your gateway** - In this step, you specify a name and recovery key for your gateway, and select a region, registering your gateway with the Gateway Cloud Service.</span></span>

- <span data-ttu-id="774ff-113">**Hozzon létre egy átjáró erőforrást az Azure-ban** -ebben a lépésben az Azure-előfizetése létrehozhat egy átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="774ff-113">**Create a gateway resource in Azure** - In this step, you create a gateway resource in your Azure subscription.</span></span>

- <span data-ttu-id="774ff-114">**Csatlakoztassa a kiszolgálókat az átjáró erőforrás** -előfizetése van egy átjáró-erőforráshoz, amennyiben a kiszolgálók csatlakoztatása megkezdheti.</span><span class="sxs-lookup"><span data-stu-id="774ff-114">**Connect your servers to your gateway resource** - Once you have a gateway resource in your subscription, you can begin connecting your servers to it.</span></span>

<span data-ttu-id="774ff-115">Ha már van egy átjáró-erőforráshoz az előfizetéséhez beállított, csatlakozhat több kiszolgálót, és egyéb szolgáltatások azt.</span><span class="sxs-lookup"><span data-stu-id="774ff-115">Once you have a gateway resource configured for your subscription, you can connect multiple servers, and other services to it.</span></span> <span data-ttu-id="774ff-116">Csak kell telepíteni egy másik átjárót, és további átjáró erőforrások létrehozása, ha más szolgáltatások vagy kiszolgálók egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="774ff-116">You only need to install a different gateway and create additional gateway resources if you have servers or other services in a different region.</span></span>

<span data-ttu-id="774ff-117">Rögtön használatba, lásd: [telepítse és konfigurálja a helyszíni adatátjáró](analysis-services-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="774ff-117">To get started right away, see [Install and configure on-premises data gateway](analysis-services-gateway-install.md).</span></span>

## <span data-ttu-id="774ff-118"><a name="how-it-works"></a>Annak működéséről</span><span class="sxs-lookup"><span data-stu-id="774ff-118"><a name="how-it-works"> </a>How it works</span></span>
<span data-ttu-id="774ff-119">A szervezet egy számítógépre telepítse az átjáró fut a Windows, **helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="774ff-119">The gateway you install on a computer in your organization runs as a Windows service, **On-premises data gateway**.</span></span> <span data-ttu-id="774ff-120">A helyi szolgáltatás az átjáró Felhőszolgáltatáshoz Azure Service Buson keresztül van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="774ff-120">This local service is registered with the Gateway Cloud Service through Azure Service Bus.</span></span> <span data-ttu-id="774ff-121">Ezután hozzon létre egy átjáró-erőforráshoz átjáró Felhőszolgáltatáshoz Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="774ff-121">You then create a gateway resource Gateway Cloud Service for your Azure subscription.</span></span> <span data-ttu-id="774ff-122">Az Azure Analysis Services-kiszolgáló majd csatlakoznak az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="774ff-122">Your Azure Analysis Services servers are then connected to your gateway resource.</span></span> <span data-ttu-id="774ff-123">Ha a kiszolgálón lévő modellek az adatokhoz történő kapcsolódáshoz a helyszíni adatforrások lekérdezések és feldolgozásra vonatkozó, a lekérdezés és az adatok áramlását az átjáró-erőforráshoz, Azure Service Bus, a helyi helyszíni átjáró szolgáltatás és az adatforrások halad át.</span><span class="sxs-lookup"><span data-stu-id="774ff-123">When models on your server need to connect to your on-premises data sources for queries or processing, a query and data flow traverses the gateway resource, Azure Service Bus, the local on-premises data gateway service, and your data sources.</span></span> 

![Működés](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

<span data-ttu-id="774ff-125">Lekérdezések és adatfolyam:</span><span class="sxs-lookup"><span data-stu-id="774ff-125">Queries and data flow:</span></span>

1. <span data-ttu-id="774ff-126">A lekérdezés a felhőalapú szolgáltatás, a titkosított hitelesítő adatokkal a helyszíni adatforrás hozta létre.</span><span class="sxs-lookup"><span data-stu-id="774ff-126">A query is created by the cloud service with the encrypted credentials for the on-premises data source.</span></span> <span data-ttu-id="774ff-127">A várólista feldolgozása az átjáró majd érkezik.</span><span class="sxs-lookup"><span data-stu-id="774ff-127">It's then sent to a queue for the gateway to process.</span></span>
2. <span data-ttu-id="774ff-128">Az átjáró felhőszolgáltatáshoz elemzi a lekérdezést, és leküldéses értesítések a kérést a [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="774ff-128">The gateway cloud service analyzes the query and pushes the request to the [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span></span>
3. <span data-ttu-id="774ff-129">Az a helyszíni átjáró kérdezze le az Azure Service Bus, a függőben lévő kérelmek.</span><span class="sxs-lookup"><span data-stu-id="774ff-129">The on-premises data gateway polls the Azure Service Bus for pending requests.</span></span>
4. <span data-ttu-id="774ff-130">Az átjáró a lekérdezés lekérdezi, visszafejti a hitelesítő adatokat, és ezeket a hitelesítő adatokat az adatforrások kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="774ff-130">The gateway gets the query, decrypts the credentials, and connects to the data sources with those credentials.</span></span>
5. <span data-ttu-id="774ff-131">Az átjáró a lekérdezést küld az adatforrás-végrehajtásra.</span><span class="sxs-lookup"><span data-stu-id="774ff-131">The gateway sends the query to the data source for execution.</span></span>
6. <span data-ttu-id="774ff-132">Az eredményeket az adatforrásból kerülnek vissza az átjáró, majd a felhőszolgáltatás és a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="774ff-132">The results are sent from the data source, back to the gateway, and then onto the cloud service and your server.</span></span>

## <span data-ttu-id="774ff-133"><a name="windows-service-account"></a>Windows-szolgáltatás fiókja</span><span class="sxs-lookup"><span data-stu-id="774ff-133"><a name="windows-service-account"> </a>Windows Service account</span></span>
<span data-ttu-id="774ff-134">Az a helyszíni átjáró használatára van konfigurálva *NT SERVICE\PBIEgwService* a Windows szolgáltatás bejelentkezési hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="774ff-134">The on-premises data gateway is configured to use *NT SERVICE\PBIEgwService* for the Windows service logon credential.</span></span> <span data-ttu-id="774ff-135">Alapértelmezés szerint rendelkezik bejelentkezési jobb szolgáltatásként; a gépet, telepíti az átjárót a környezetében.</span><span class="sxs-lookup"><span data-stu-id="774ff-135">By default, it has the right of Logon as a service; in the context of the machine that you are installing the gateway on.</span></span> <span data-ttu-id="774ff-136">Ezeket a hitelesítő adatokat, de nem ugyanazt a fiókot a helyszíni adatforrások eléréséhez használt az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="774ff-136">This credential is not the same account used to connect to on-premises data sources or your Azure account.</span></span>  

<span data-ttu-id="774ff-137">Ha problémák lépnek fel a proxykiszolgáló hitelesítést miatt, előfordulhat, hogy módosítani szeretné a Windows-fiók egy tartományi felhasználó vagy a felügyelt szolgáltatásfiók.</span><span class="sxs-lookup"><span data-stu-id="774ff-137">If you encounter issues with your proxy server due to authentication, you may want to change the Windows service account to a domain user or managed service account.</span></span>

## <span data-ttu-id="774ff-138"><a name="ports"></a>Portok</span><span class="sxs-lookup"><span data-stu-id="774ff-138"><a name="ports"> </a>Ports</span></span>
<span data-ttu-id="774ff-139">Az átjáró Azure Service Bus egy kimenő kapcsolatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="774ff-139">The gateway creates an outbound connection to Azure Service Bus.</span></span> <span data-ttu-id="774ff-140">A kimenő portokon kommunikál: TCP 443-as (alapértelmezett), 5671, 5672, 9350 – 9354-es.</span><span class="sxs-lookup"><span data-stu-id="774ff-140">It communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span>  <span data-ttu-id="774ff-141">Az átjáró nincs szükség a bejövő portra.</span><span class="sxs-lookup"><span data-stu-id="774ff-141">The gateway does not require inbound ports.</span></span>

<span data-ttu-id="774ff-142">Javasoljuk, hogy a tűzfal az adatterület az IP-címek engedélyezési listája.</span><span class="sxs-lookup"><span data-stu-id="774ff-142">We recommend you whitelist the IP addresses for your data region in your firewall.</span></span> <span data-ttu-id="774ff-143">Letöltheti a [Microsoft Azure Datacenter IP-lista](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="774ff-143">You can download the [Microsoft Azure Datacenter IP list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="774ff-144">A lista a heti frissül.</span><span class="sxs-lookup"><span data-stu-id="774ff-144">This list is updated weekly.</span></span>

> [!NOTE]
> <span data-ttu-id="774ff-145">Az IP-címek szerepel-e az Azure Datacenter IP CIDR-formátumban vannak.</span><span class="sxs-lookup"><span data-stu-id="774ff-145">The IP Addresses listed in the Azure Datacenter IP list are in CIDR notation.</span></span> <span data-ttu-id="774ff-146">Például: 10.0.0.0/24 nem jelenti azt 10.0.0.0 10.0.0.24 keresztül.</span><span class="sxs-lookup"><span data-stu-id="774ff-146">For example, 10.0.0.0/24 does not mean 10.0.0.0 through 10.0.0.24.</span></span> <span data-ttu-id="774ff-147">További információ a [CIDR-jelöléssel](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="774ff-147">Learn more about the [CIDR notation](http://whatismyipaddress.com/cidr).</span></span>
>
>

<span data-ttu-id="774ff-148">Az alábbiakban a teljes tartománynevek az átjáró által használt.</span><span class="sxs-lookup"><span data-stu-id="774ff-148">The following are the fully qualified domain names used by the gateway.</span></span>

| <span data-ttu-id="774ff-149">Tartománynevek</span><span class="sxs-lookup"><span data-stu-id="774ff-149">Domain names</span></span> | <span data-ttu-id="774ff-150">Kimenő portok</span><span class="sxs-lookup"><span data-stu-id="774ff-150">Outbound ports</span></span> | <span data-ttu-id="774ff-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="774ff-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="774ff-152">*. powerbi.com webhelyre</span><span class="sxs-lookup"><span data-stu-id="774ff-152">*.powerbi.com</span></span> |<span data-ttu-id="774ff-153">80</span><span class="sxs-lookup"><span data-stu-id="774ff-153">80</span></span> |<span data-ttu-id="774ff-154">A telepítő letöltéséhez használt HTTP.</span><span class="sxs-lookup"><span data-stu-id="774ff-154">HTTP used to download the installer.</span></span> |
| <span data-ttu-id="774ff-155">*. powerbi.com webhelyre</span><span class="sxs-lookup"><span data-stu-id="774ff-155">*.powerbi.com</span></span> |<span data-ttu-id="774ff-156">443</span><span class="sxs-lookup"><span data-stu-id="774ff-156">443</span></span> |<span data-ttu-id="774ff-157">HTTPS</span><span class="sxs-lookup"><span data-stu-id="774ff-157">HTTPS</span></span> |
| <span data-ttu-id="774ff-158">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="774ff-158">*.analysis.windows.net</span></span> |<span data-ttu-id="774ff-159">443</span><span class="sxs-lookup"><span data-stu-id="774ff-159">443</span></span> |<span data-ttu-id="774ff-160">HTTPS</span><span class="sxs-lookup"><span data-stu-id="774ff-160">HTTPS</span></span> |
| <span data-ttu-id="774ff-161">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="774ff-161">*.login.windows.net</span></span> |<span data-ttu-id="774ff-162">443</span><span class="sxs-lookup"><span data-stu-id="774ff-162">443</span></span> |<span data-ttu-id="774ff-163">HTTPS</span><span class="sxs-lookup"><span data-stu-id="774ff-163">HTTPS</span></span> |
| <span data-ttu-id="774ff-164">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="774ff-164">*.servicebus.windows.net</span></span> |<span data-ttu-id="774ff-165">5671-5672</span><span class="sxs-lookup"><span data-stu-id="774ff-165">5671-5672</span></span> |<span data-ttu-id="774ff-166">Speciális üzenetsor-kezelési protokoll (AMQP)</span><span class="sxs-lookup"><span data-stu-id="774ff-166">Advanced Message Queuing Protocol (AMQP)</span></span> |
| <span data-ttu-id="774ff-167">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="774ff-167">*.servicebus.windows.net</span></span> |<span data-ttu-id="774ff-168">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="774ff-168">443, 9350-9354</span></span> |<span data-ttu-id="774ff-169">A Service Bus Relay (a 443-as kér a hozzáférés-vezérlés jogkivonat beszerzése) TCP-n keresztül figyelői</span><span class="sxs-lookup"><span data-stu-id="774ff-169">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> |
| <span data-ttu-id="774ff-170">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="774ff-170">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="774ff-171">443</span><span class="sxs-lookup"><span data-stu-id="774ff-171">443</span></span> |<span data-ttu-id="774ff-172">HTTPS</span><span class="sxs-lookup"><span data-stu-id="774ff-172">HTTPS</span></span> |
| <span data-ttu-id="774ff-173">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="774ff-173">*.core.windows.net</span></span> |<span data-ttu-id="774ff-174">443</span><span class="sxs-lookup"><span data-stu-id="774ff-174">443</span></span> |<span data-ttu-id="774ff-175">HTTPS</span><span class="sxs-lookup"><span data-stu-id="774ff-175">HTTPS</span></span> |
| <span data-ttu-id="774ff-176">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="774ff-176">login.microsoftonline.com</span></span> |<span data-ttu-id="774ff-177">443</span><span class="sxs-lookup"><span data-stu-id="774ff-177">443</span></span> |<span data-ttu-id="774ff-178">HTTPS</span><span class="sxs-lookup"><span data-stu-id="774ff-178">HTTPS</span></span> |
| <span data-ttu-id="774ff-179">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="774ff-179">*.msftncsi.com</span></span> |<span data-ttu-id="774ff-180">443</span><span class="sxs-lookup"><span data-stu-id="774ff-180">443</span></span> |<span data-ttu-id="774ff-181">Használja az internet kapcsolat tesztelése, ha az átjáró a Power BI szolgáltatás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="774ff-181">Used to test internet connectivity if the gateway is unreachable by the Power BI service.</span></span> |
| <span data-ttu-id="774ff-182">*.microsoftonline-p.com</span><span class="sxs-lookup"><span data-stu-id="774ff-182">*.microsoftonline-p.com</span></span> |<span data-ttu-id="774ff-183">443</span><span class="sxs-lookup"><span data-stu-id="774ff-183">443</span></span> |<span data-ttu-id="774ff-184">Konfigurációjától függően a hitelesítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="774ff-184">Used for authentication depending on configuration.</span></span> |

### <span data-ttu-id="774ff-185"><a name="force-https"></a>Az Azure Service Bus HTTPS-kommunikációt kényszerítése</span><span class="sxs-lookup"><span data-stu-id="774ff-185"><a name="force-https"></a>Forcing HTTPS communication with Azure Service Bus</span></span>
<span data-ttu-id="774ff-186">Beállíthatja, hogy az közvetlen TCP; helyett HTTPS használatával kommunikálnak Azure Service Bus-átjáró azonban ez úgy is jelentősen csökkenti a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="774ff-186">You can force the gateway to communicate with Azure Service Bus by using HTTPS instead of direct TCP; however, doing so can greatly reduce performance.</span></span> <span data-ttu-id="774ff-187">Módosíthatja a *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* megváltoztatja az értéket a fájl `AutoDetect` való `Https`.</span><span class="sxs-lookup"><span data-stu-id="774ff-187">You can modify the *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* file by changing the value from `AutoDetect` to `Https`.</span></span> <span data-ttu-id="774ff-188">Ez a fájl általában *C:\Program Files\On helyszíni adatátjáró*.</span><span class="sxs-lookup"><span data-stu-id="774ff-188">This file is typically located at *C:\Program Files\On-premises data gateway*.</span></span>

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <span data-ttu-id="774ff-189"><a name="faq"></a>Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="774ff-189"><a name="faq"></a>Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="774ff-190">Általános kérdések</span><span class="sxs-lookup"><span data-stu-id="774ff-190">General</span></span>

<span data-ttu-id="774ff-191">**A Q**: van egy átjáró adatforrások, például az Azure SQL-adatbázis a felhőben?</span><span class="sxs-lookup"><span data-stu-id="774ff-191">**Q**: Do I need a gateway for data sources in the cloud, such as Azure SQL Database?</span></span> <br/><span data-ttu-id="774ff-192">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="774ff-192">
**A**: No.</span></span> <span data-ttu-id="774ff-193">Átjáró csatlakozik a helyi adatforrások csak.</span><span class="sxs-lookup"><span data-stu-id="774ff-193">A gateway connects to on-premises data sources only.</span></span>

<span data-ttu-id="774ff-194">**A Q**: az adatforrással azonos gépen kell telepíteni az átjárót rendelkezik?</span><span class="sxs-lookup"><span data-stu-id="774ff-194">**Q**: Does the gateway have to be installed on the same machine as the data source?</span></span> <br/><span data-ttu-id="774ff-195">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="774ff-195">
**A**: No.</span></span> <span data-ttu-id="774ff-196">Az átjáró kapcsolódik az adatforráshoz, a megadott kapcsolati információk használatával.</span><span class="sxs-lookup"><span data-stu-id="774ff-196">The gateway connects to the data source using the connection information that was provided.</span></span> <span data-ttu-id="774ff-197">Egy ügyfélalkalmazás abban az értelemben, fontolja meg az átjáró.</span><span class="sxs-lookup"><span data-stu-id="774ff-197">Consider the gateway as a client application in this sense.</span></span> <span data-ttu-id="774ff-198">Az átjáró éppen kell képes csatlakozni a kiszolgáló neve, a megadott, általában ugyanazon a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="774ff-198">The gateway just needs the capability to connect to the server name that was provided, typically on the same network.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="774ff-199">**A Q**: Miért szükséges a munkahelyi vagy iskolai fiókkal való bejelentkezéshez használt?</span><span class="sxs-lookup"><span data-stu-id="774ff-199">**Q**: Why do I need to use a work or school account to sign in?</span></span> <br/><span data-ttu-id="774ff-200">
**A**: csak használja az Azure munkahelyi vagy iskolai fiókkal, az a helyszíni átjáró telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="774ff-200">
**A**: You can only use an Azure work or school account when you install the on-premises data gateway.</span></span> <span data-ttu-id="774ff-201">A bejelentkezési fiók egy Azure Active Directory (Azure AD) által felügyelt bérlői tárolja.</span><span class="sxs-lookup"><span data-stu-id="774ff-201">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="774ff-202">Általában az Azure AD-fiókot egyszerű felhasználónév (UPN) felel meg az e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="774ff-202">Usually, your Azure AD account's user principal name (UPN) matches the email address.</span></span>

<span data-ttu-id="774ff-203">**A Q**: a hitelesítő adataimat tároló?</span><span class="sxs-lookup"><span data-stu-id="774ff-203">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="774ff-204">
**A**: adatforráshoz megadott hitelesítő adatok titkosítva, és az átjáró felhőszolgáltatásban tárolt.</span><span class="sxs-lookup"><span data-stu-id="774ff-204">
**A**: The credentials that you enter for a data source are encrypted and stored in the Gateway Cloud Service.</span></span> <span data-ttu-id="774ff-205">A hitelesítő adatokat a helyszíni adatok átjáróra lesznek visszafejtve.</span><span class="sxs-lookup"><span data-stu-id="774ff-205">The credentials are decrypted at the on-premises data gateway.</span></span>

<span data-ttu-id="774ff-206">**A Q**: vannak-e hálózati sávszélesség vonatkozó követelmények?</span><span class="sxs-lookup"><span data-stu-id="774ff-206">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="774ff-207">
**A**: azt javasoljuk rendelkezik, a hálózati kapcsolat van a jó teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="774ff-207">
**A**: It's recommend your network connection has good throughput.</span></span> <span data-ttu-id="774ff-208">Minden környezet más, és a küldött adatok mennyisége hatással van az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="774ff-208">Every environment is different, and the amount of data being sent affects the results.</span></span> <span data-ttu-id="774ff-209">A helyszíni és az Azure adatközpontjaiban közötti átviteli szintű segíthet az ExpressRoute segítségével.</span><span class="sxs-lookup"><span data-stu-id="774ff-209">Using ExpressRoute could help to guarantee a level of throughput between on-premises and the Azure datacenters.</span></span>
<span data-ttu-id="774ff-210">A külső eszköz Azure sebesség teszt alkalmazás segítségével fel tudja mérni a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="774ff-210">You can use the third-party tool Azure Speed Test app to help gauge your throughput.</span></span>

<span data-ttu-id="774ff-211">**A Q**: Mi az a várakozási lekérdezések futtatását adatforráshoz az átjáró?</span><span class="sxs-lookup"><span data-stu-id="774ff-211">**Q**: What is the latency for running queries to a data source from the gateway?</span></span> <span data-ttu-id="774ff-212">Mi az a legjobb architektúra?</span><span class="sxs-lookup"><span data-stu-id="774ff-212">What is the best architecture?</span></span> <br/><span data-ttu-id="774ff-213">
**A**: hálózati késés csökkentésére, telepítse az átjáró lehető az adatforrás a lehető legjobban hasonlítson.</span><span class="sxs-lookup"><span data-stu-id="774ff-213">
**A**: To reduce network latency, install the gateway as close to the data source as possible.</span></span> <span data-ttu-id="774ff-214">Az átjáró telepíthető a konkrét adatforrásokhoz, ha a közelségi kapcsolat minimalizálja a várakozási bevezetett.</span><span class="sxs-lookup"><span data-stu-id="774ff-214">If you can install the gateway on the actual data source, this proximity minimizes the latency introduced.</span></span> <span data-ttu-id="774ff-215">Túl vegye figyelembe az adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="774ff-215">Consider the datacenters too.</span></span> <span data-ttu-id="774ff-216">Például ha a szolgáltatás használja, az USA nyugati régiója adatközpontban, és egy Azure virtuális Gépen található SQL-kiszolgálóval rendelkezik, az Azure virtuális gép kell lennie az USA nyugati régiója túl.</span><span class="sxs-lookup"><span data-stu-id="774ff-216">For example, if your service uses the West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in the West US too.</span></span> <span data-ttu-id="774ff-217">A közelségi kapcsolat minimálisra csökkenti a késést, és ezzel elkerülheti a kimenő forgalom díjak, az Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="774ff-217">This proximity minimizes latency and avoids egress charges on the Azure VM.</span></span>

<span data-ttu-id="774ff-218">**A Q**: hogyan eredmények küldött vissza a felhőben?</span><span class="sxs-lookup"><span data-stu-id="774ff-218">**Q**: How are results sent back to the cloud?</span></span> <br/><span data-ttu-id="774ff-219">
**A**: eredmények az Azure Service Bus keresztül zajlik.</span><span class="sxs-lookup"><span data-stu-id="774ff-219">
**A**: Results are sent through the Azure Service Bus.</span></span>

<span data-ttu-id="774ff-220">**A Q**: vannak-e a bejövő kapcsolatokat az átjáróra a felhőből?</span><span class="sxs-lookup"><span data-stu-id="774ff-220">**Q**: Are there any inbound connections to the gateway from the cloud?</span></span> <br/><span data-ttu-id="774ff-221">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="774ff-221">
**A**: No.</span></span> <span data-ttu-id="774ff-222">Az átjáró Azure Service Bus kimenő kapcsolatokat használja.</span><span class="sxs-lookup"><span data-stu-id="774ff-222">The gateway uses outbound connections to Azure Service Bus.</span></span>

<span data-ttu-id="774ff-223">**A Q**: Mi történik, ha az általam letiltottak kimenő kapcsolatokat?</span><span class="sxs-lookup"><span data-stu-id="774ff-223">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="774ff-224">Mit kell megnyitni?</span><span class="sxs-lookup"><span data-stu-id="774ff-224">What do I need to open?</span></span> <br/><span data-ttu-id="774ff-225">
**A**: a portok és a gazdagépekhez, amelyek az átjárót használja.</span><span class="sxs-lookup"><span data-stu-id="774ff-225">
**A**: See the ports and hosts that the gateway uses.</span></span>

<span data-ttu-id="774ff-226">**A Q**: a tényleges Windows-szolgáltatás úgynevezett?</span><span class="sxs-lookup"><span data-stu-id="774ff-226">**Q**: What is the actual Windows service called?</span></span><br/><span data-ttu-id="774ff-227">
**A**: A szolgáltatások, az átjárót a helyszíni átjáró szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="774ff-227">
**A**: In Services, the gateway is called On-premises data gateway service.</span></span>

<span data-ttu-id="774ff-228">**A Q**: Azure Active Directory-fiókkal az átjáró Windows-szolgáltatás futtatható?</span><span class="sxs-lookup"><span data-stu-id="774ff-228">**Q**: Can the gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="774ff-229">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="774ff-229">
**A**: No.</span></span> <span data-ttu-id="774ff-230">A Windows-szolgáltatás egy érvényes Windows-fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="774ff-230">The Windows service must have a valid Windows account.</span></span> <span data-ttu-id="774ff-231">Alapértelmezés szerint a szolgáltatás fut a szolgáltatás SID NT SERVICE\PBIEgwService.</span><span class="sxs-lookup"><span data-stu-id="774ff-231">By default, the service runs with the Service SID, NT SERVICE\PBIEgwService.</span></span>

### <span data-ttu-id="774ff-232"><a name="high-availability"></a>Magas rendelkezésre állás és a katasztrófa utáni helyreállítás</span><span class="sxs-lookup"><span data-stu-id="774ff-232"><a name="high-availability"></a>High availability and disaster recovery</span></span>

<span data-ttu-id="774ff-233">**A Q**: milyen beállítások érhetők el a vész-helyreállítási?</span><span class="sxs-lookup"><span data-stu-id="774ff-233">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="774ff-234">
**A**: a helyreállítási kulcs használatával állítsa vissza, vagy helyezze át az átjárót.</span><span class="sxs-lookup"><span data-stu-id="774ff-234">
**A**: You can use the recovery key to restore or move a gateway.</span></span> <span data-ttu-id="774ff-235">Az átjáró telepítésekor adja meg a helyreállítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="774ff-235">When you install the gateway, specify the recovery key.</span></span>

<span data-ttu-id="774ff-236">**A Q**: Mi az az előnye, hogy a helyreállítási kulcs?</span><span class="sxs-lookup"><span data-stu-id="774ff-236">**Q**: What is the benefit of the recovery key?</span></span> <br/><span data-ttu-id="774ff-237">
**A**: A helyreállítási kulcs biztosítja az áttelepítéshez, vagy az átjáró beállításainak katasztrófa utáni helyreállításhoz.</span><span class="sxs-lookup"><span data-stu-id="774ff-237">
**A**: The recovery key provides a way to migrate or recover your gateway settings after a disaster.</span></span>

## <span data-ttu-id="774ff-238"><a name="troubleshooting"></a>Hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="774ff-238"><a name="troubleshooting"> </a>Troubleshooting</span></span>

<span data-ttu-id="774ff-239">**A Q**: Hogyan tekinthető meg mi lekérdezések folyamatban van a helyszíni adatforrás küldött?</span><span class="sxs-lookup"><span data-stu-id="774ff-239">**Q**: How can I see what queries are being sent to the on-premises data source?</span></span> <br/><span data-ttu-id="774ff-240">
**A**: engedélyezheti a lekérdezés nyomon követése, beleértve a küldött lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="774ff-240">
**A**: You can enable query tracing, which includes the queries that are sent.</span></span> <span data-ttu-id="774ff-241">Ne felejtse el módosítani a lekérdezés vissza az eredeti értéket, miután befejezte a hibakeresési nyomkövetés.</span><span class="sxs-lookup"><span data-stu-id="774ff-241">Remember to change query tracing back to the original value when done troubleshooting.</span></span> <span data-ttu-id="774ff-242">A lekérdezés nyomon követése engedélyezve van a Kilépés hoz létre a nagyobb naplókat.</span><span class="sxs-lookup"><span data-stu-id="774ff-242">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="774ff-243">Is megtekintheti, hogy az adatforrás rendelkezik a nyomkövetési lekérdezések eszközök.</span><span class="sxs-lookup"><span data-stu-id="774ff-243">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="774ff-244">Például használhatja bővített eseményektől vagy SQL Profiler az SQL Server és az Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="774ff-244">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="774ff-245">**A Q**: hol találhatók az átjáró naplói?</span><span class="sxs-lookup"><span data-stu-id="774ff-245">**Q**: Where are the gateway logs?</span></span> <br/><span data-ttu-id="774ff-246">
**A**: naplók lásd a témakör későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="774ff-246">
**A**: See Logs later in this topic.</span></span>

### <span data-ttu-id="774ff-247"><a name="update"></a>Frissítse a legújabb verzióra</span><span class="sxs-lookup"><span data-stu-id="774ff-247"><a name="update"></a>Update to the latest version</span></span>

<span data-ttu-id="774ff-248">Számos problémájának is surface, ha az átjáró verziója elavult.</span><span class="sxs-lookup"><span data-stu-id="774ff-248">Many issues can surface when the gateway version becomes outdated.</span></span> <span data-ttu-id="774ff-249">Jó általános gyakorlat győződjön meg arról, hogy a legújabb verzióját használja-e.</span><span class="sxs-lookup"><span data-stu-id="774ff-249">As good general practice, make sure that you use the latest version.</span></span> <span data-ttu-id="774ff-250">Ha az átjáró nem frissültek, havonta vagy hosszabb ideig, akkor előfordulhat, hogy javasoljuk, hogy az átjáró legújabb verzióját telepítse, és ha Reprodukálja a hibát.</span><span class="sxs-lookup"><span data-stu-id="774ff-250">If you haven't updated the gateway for a month or longer, you might consider installing the latest version of the gateway, and see if you can reproduce the issue.</span></span>

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="774ff-251">Hiba: Nem sikerült hozzáadni a felhasználót a csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="774ff-251">Error: Failed to add user to group.</span></span> <span data-ttu-id="774ff-252">(Felhasználói-2147463168 PBIEgwService teljesítmény)</span><span class="sxs-lookup"><span data-stu-id="774ff-252">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="774ff-253">Ez a hiba kaphat, ha az átjáró telepítésekor a tartományvezérlőn, amely nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="774ff-253">You might get this error if you try to install the gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="774ff-254">Győződjön meg arról, hogy telepít-e az átjáró gépen, amely nem tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="774ff-254">Make sure that you deploy the gateway on a machine that isn't a domain controller.</span></span>

## <span data-ttu-id="774ff-255"><a name="logs"></a>Naplók</span><span class="sxs-lookup"><span data-stu-id="774ff-255"><a name="logs"></a>Logs</span></span>

<span data-ttu-id="774ff-256">Naplófájlok egy fontos erőforrás hibaelhárítása során.</span><span class="sxs-lookup"><span data-stu-id="774ff-256">Log files are an important resource when troubleshooting.</span></span>

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="774ff-257">Vállalati átjáró service naplóit</span><span class="sxs-lookup"><span data-stu-id="774ff-257">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a><span data-ttu-id="774ff-258">Konfigurációs naplók</span><span class="sxs-lookup"><span data-stu-id="774ff-258">Configuration logs</span></span>

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a><span data-ttu-id="774ff-259">Eseménynaplók</span><span class="sxs-lookup"><span data-stu-id="774ff-259">Event logs</span></span>

<span data-ttu-id="774ff-260">Az adatkezelési átjáró, a PowerBIGateway naplókat alatt található **alkalmazás- és szolgáltatásnaplók**.</span><span class="sxs-lookup"><span data-stu-id="774ff-260">You can find the Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>


## <span data-ttu-id="774ff-261"><a name="telemetry"></a>Telemetria</span><span class="sxs-lookup"><span data-stu-id="774ff-261"><a name="telemetry"></a>Telemetry</span></span>
<span data-ttu-id="774ff-262">Telemetriai adatainak figyelés és hibaelhárítás céljából is használható.</span><span class="sxs-lookup"><span data-stu-id="774ff-262">Telemetry can be used for monitoring and troubleshooting.</span></span> <span data-ttu-id="774ff-263">Alapértelmezés szerint</span><span class="sxs-lookup"><span data-stu-id="774ff-263">By default</span></span>

<span data-ttu-id="774ff-264">**Telemetria bekapcsolása**</span><span class="sxs-lookup"><span data-stu-id="774ff-264">**To turn on telemetry**</span></span>

1.  <span data-ttu-id="774ff-265">Ellenőrizze a helyszíni adatok átjáró ügyfél könyvtárat a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="774ff-265">Check the On-premises data gateway client directory on the computer.</span></span> <span data-ttu-id="774ff-266">Általában **%systemdrive%\Program Files\On helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="774ff-266">Typically, it is **%systemdrive%\Program Files\On-premises data gateway**.</span></span> <span data-ttu-id="774ff-267">Nyissa meg a szolgáltatások konzolt, és ellenőrizze az elérési utat a végrehajtható fájl: A tulajdonság a helyszíni adatok átjáró szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="774ff-267">Or, you can open a Services console and check the Path to executable: A property of the On-premises data gateway service.</span></span>
2.  <span data-ttu-id="774ff-268">Az ügyfél könyvtárból Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="774ff-268">In the Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config file from client directory.</span></span> <span data-ttu-id="774ff-269">Módosítsa a SendTelemetry beállítást igaz értékre.</span><span class="sxs-lookup"><span data-stu-id="774ff-269">Change the SendTelemetry setting to true.</span></span>
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  <span data-ttu-id="774ff-270">A módosítások mentéséhez, és indítsa újra a Windows-szolgáltatás: A helyszíni átjáró szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="774ff-270">Save your changes and restart the Windows service: On-premises data gateway service.</span></span>




## <a name="next-steps"></a><span data-ttu-id="774ff-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="774ff-271">Next steps</span></span>
* [<span data-ttu-id="774ff-272">Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="774ff-272">Manage Analysis Services</span></span>](analysis-services-manage.md)
* [<span data-ttu-id="774ff-273">Adatok beolvasása az Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="774ff-273">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)
