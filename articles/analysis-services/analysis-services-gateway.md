---
title: "aaaOn helyszíni adatátjáró |} Microsoft Docs"
description: "Egy helyszíni átjárót szükség, ha az Analysis Services-kiszolgálóhoz, az Azure-ban tooon helyi adatforrások fog csatlakozni."
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
ms.openlocfilehash: fc7b9c69e6f81b41deb7a5d6d963225593845d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-tooon-premises-data-sources-with-azure-on-premises-data-gateway"></a><span data-ttu-id="77de1-103">Helyszíni Azure Data Gateway tooon helyi adatforrások összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="77de1-103">Connecting tooon-premises data sources with Azure On-premises Data Gateway</span></span>
<span data-ttu-id="77de1-104">hello helyszíni adatátjáró hídként, a helyszíni adatforrások és a hello felhőben Azure Analysis Services-kiszolgálók közötti biztonságos adatátvitel biztosító működik.</span><span class="sxs-lookup"><span data-stu-id="77de1-104">hello on-premises data gateway acts as a bridge, providing secure data transfer between on-premises data sources and your Azure Analysis Services servers in hello cloud.</span></span> <span data-ttu-id="77de1-105">Továbbá tooworking hello a több Azure Analysis Services-kiszolgáló ugyanabban a régióban, hello hello átjáró legújabb verziója is működik az Azure Logic Apps, a Power bi-ban, a kiemelt alkalmazások és a Microsoft Flow.</span><span class="sxs-lookup"><span data-stu-id="77de1-105">In addition tooworking with multiple Azure Analysis Services servers in hello same region, hello latest version of hello gateway also works with Azure Logic Apps, Power BI, Power Apps, and Microsoft Flow.</span></span> <span data-ttu-id="77de1-106">A hello több szolgáltatáshoz is társíthat ugyanabban a régióban, ha csak egyetlen átjáró.</span><span class="sxs-lookup"><span data-stu-id="77de1-106">You can associate multiple services in hello same region with a single gateway.</span></span> 

 <span data-ttu-id="77de1-107">Az Azure Analysis Services egy átjáró erőforrás hello szükséges ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="77de1-107">Azure Analysis Services requires a gateway resource in hello same region.</span></span> <span data-ttu-id="77de1-108">Például ha Azure Analysis Services-kiszolgálók hello USA keleti régiója 2 régióban, kell egy átjáró-erőforráshoz régióban hello USA keleti régiója 2.</span><span class="sxs-lookup"><span data-stu-id="77de1-108">For example, if you have Azure Analysis Services servers in hello East US 2 region, you need a gateway resource in hello East US 2 region.</span></span> <span data-ttu-id="77de1-109">USA keleti régiója 2 több kiszolgáló használható hello ugyanahhoz az átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="77de1-109">Multiple servers in East US 2 can use hello same gateway.</span></span>

<span data-ttu-id="77de1-110">Első hello átjáró hello telepítése először az egy négyrészes folyamat:</span><span class="sxs-lookup"><span data-stu-id="77de1-110">Getting setup with hello gateway hello first time is a four-part process:</span></span>

- <span data-ttu-id="77de1-111">**Töltse le és futtassa a telepítőt** – Ez a lépés telepíti egy átjáró szolgáltatás a szervezet egyik számítógépén.</span><span class="sxs-lookup"><span data-stu-id="77de1-111">**Download and run setup** - This step installs a gateway service on a computer in your organization.</span></span>

- <span data-ttu-id="77de1-112">**Regisztrálnia kell az átjárót** – ebben a lépésben adjon nevet és helyreállítási az átjáró kulcsát, és válasszon ki egy régiót, az átjáró regisztrálása hello átjáró Felhőszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="77de1-112">**Register your gateway** - In this step, you specify a name and recovery key for your gateway, and select a region, registering your gateway with hello Gateway Cloud Service.</span></span>

- <span data-ttu-id="77de1-113">**Hozzon létre egy átjáró erőforrást az Azure-ban** -ebben a lépésben az Azure-előfizetése létrehozhat egy átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="77de1-113">**Create a gateway resource in Azure** - In this step, you create a gateway resource in your Azure subscription.</span></span>

- <span data-ttu-id="77de1-114">**Csatlakozás a kiszolgálók tooyour átjáró erőforrás** -előfizetése van egy átjáró-erőforráshoz, amennyiben a kiszolgálók tooit kapcsolódás megkezdheti.</span><span class="sxs-lookup"><span data-stu-id="77de1-114">**Connect your servers tooyour gateway resource** - Once you have a gateway resource in your subscription, you can begin connecting your servers tooit.</span></span>

<span data-ttu-id="77de1-115">Ha már van egy átjáró-erőforráshoz az előfizetéséhez beállított, több kiszolgáló, és egyéb szolgáltatások tooit is elérheti.</span><span class="sxs-lookup"><span data-stu-id="77de1-115">Once you have a gateway resource configured for your subscription, you can connect multiple servers, and other services tooit.</span></span> <span data-ttu-id="77de1-116">Csak egy másik átjáró tooinstall kell, és további átjáró erőforrások létrehozása, ha más szolgáltatások vagy kiszolgálók egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="77de1-116">You only need tooinstall a different gateway and create additional gateway resources if you have servers or other services in a different region.</span></span>

<span data-ttu-id="77de1-117">azonnal, indítható tooget lásd [telepítése és konfigurálása a helyszíni adatátjáró](analysis-services-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="77de1-117">tooget started right away, see [Install and configure on-premises data gateway](analysis-services-gateway-install.md).</span></span>

## <span data-ttu-id="77de1-118"><a name="how-it-works"></a>Annak működéséről</span><span class="sxs-lookup"><span data-stu-id="77de1-118"><a name="how-it-works"> </a>How it works</span></span>
<span data-ttu-id="77de1-119">a Windows szolgáltatásként fut egy számítógépre a szervezet telepít hello átjáró **helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="77de1-119">hello gateway you install on a computer in your organization runs as a Windows service, **On-premises data gateway**.</span></span> <span data-ttu-id="77de1-120">A helyi szolgáltatás hello átjáró Felhőszolgáltatáshoz Azure Service Buson keresztül van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="77de1-120">This local service is registered with hello Gateway Cloud Service through Azure Service Bus.</span></span> <span data-ttu-id="77de1-121">Ezután hozzon létre egy átjáró-erőforráshoz átjáró Felhőszolgáltatáshoz Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="77de1-121">You then create a gateway resource Gateway Cloud Service for your Azure subscription.</span></span> <span data-ttu-id="77de1-122">Az Azure Analysis Services-kiszolgálók esetén, majd csatlakoztatott tooyour átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="77de1-122">Your Azure Analysis Services servers are then connected tooyour gateway resource.</span></span> <span data-ttu-id="77de1-123">Ha a kiszolgáló kell tooconnect tooyour modell a helyszíni adatforrások lekérdezések vagy feldolgozásra, a lekérdezés- és folyamata traverses hello átjáró erőforrások Azure Service Bus hello helyi helyszíni átjáró szolgáltatás, és az adatforrások.</span><span class="sxs-lookup"><span data-stu-id="77de1-123">When models on your server need tooconnect tooyour on-premises data sources for queries or processing, a query and data flow traverses hello gateway resource, Azure Service Bus, hello local on-premises data gateway service, and your data sources.</span></span> 

![Működés](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

<span data-ttu-id="77de1-125">Lekérdezések és adatfolyam:</span><span class="sxs-lookup"><span data-stu-id="77de1-125">Queries and data flow:</span></span>

1. <span data-ttu-id="77de1-126">A lekérdezés hello felhőszolgáltatás hello titkosított hitelesítő adatokkal a helyszíni adatforrás hello hozta létre.</span><span class="sxs-lookup"><span data-stu-id="77de1-126">A query is created by hello cloud service with hello encrypted credentials for hello on-premises data source.</span></span> <span data-ttu-id="77de1-127">Majd továbbított hello átjáró tooprocess tooa várakozási sorban.</span><span class="sxs-lookup"><span data-stu-id="77de1-127">It's then sent tooa queue for hello gateway tooprocess.</span></span>
2. <span data-ttu-id="77de1-128">hello átjáró felhőszolgáltatáshoz hello lekérdezés elemzi, és leküldéses értesítések hello kérelem toohello [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="77de1-128">hello gateway cloud service analyzes hello query and pushes hello request toohello [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span></span>
3. <span data-ttu-id="77de1-129">hello helyszíni adatátjáró hello Azure Service Bus a függőben lévő kérelmek kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="77de1-129">hello on-premises data gateway polls hello Azure Service Bus for pending requests.</span></span>
4. <span data-ttu-id="77de1-130">hello átjáró hello lekérdezés lekérdezi, visszafejti hello hitelesítő adatokat, és toohello adatforrásokhoz csatlakoznak az ezeket a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="77de1-130">hello gateway gets hello query, decrypts hello credentials, and connects toohello data sources with those credentials.</span></span>
5. <span data-ttu-id="77de1-131">hello átjáró küldi hello lekérdezési toohello adatforrás végrehajtásra.</span><span class="sxs-lookup"><span data-stu-id="77de1-131">hello gateway sends hello query toohello data source for execution.</span></span>
6. <span data-ttu-id="77de1-132">hello eredmények küldött hello adatforrások, hátsó toohello átjáró, és hello felhőalapú szolgáltatás, és a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="77de1-132">hello results are sent from hello data source, back toohello gateway, and then onto hello cloud service and your server.</span></span>

## <span data-ttu-id="77de1-133"><a name="windows-service-account"></a>Windows-szolgáltatás fiókja</span><span class="sxs-lookup"><span data-stu-id="77de1-133"><a name="windows-service-account"> </a>Windows Service account</span></span>
<span data-ttu-id="77de1-134">hello helyszíni az adatátjáró konfigurált toouse *NT SERVICE\PBIEgwService* a hello Windows szolgáltatás bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="77de1-134">hello on-premises data gateway is configured toouse *NT SERVICE\PBIEgwService* for hello Windows service logon credential.</span></span> <span data-ttu-id="77de1-135">Alapértelmezés szerint rendelkezik hello jobb szélén bejelentkezés szolgáltatásként; hello gép, amely telepíti az hello átjáró hello környezetében.</span><span class="sxs-lookup"><span data-stu-id="77de1-135">By default, it has hello right of Logon as a service; in hello context of hello machine that you are installing hello gateway on.</span></span> <span data-ttu-id="77de1-136">Ezeket a hitelesítő adatokat nem hello, azonos használt fiók tooconnect tooon helyi adatforrások vagy az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="77de1-136">This credential is not hello same account used tooconnect tooon-premises data sources or your Azure account.</span></span>  

<span data-ttu-id="77de1-137">Ha problémák miatt a proxykiszolgáló tooauthentication, érdemes lehet toochange hello Windows szolgáltatás tooa tartományfelhasználói fiókot, vagy felügyelt szolgáltatásfiók.</span><span class="sxs-lookup"><span data-stu-id="77de1-137">If you encounter issues with your proxy server due tooauthentication, you may want toochange hello Windows service account tooa domain user or managed service account.</span></span>

## <span data-ttu-id="77de1-138"><a name="ports"></a>Portok</span><span class="sxs-lookup"><span data-stu-id="77de1-138"><a name="ports"> </a>Ports</span></span>
<span data-ttu-id="77de1-139">hello átjáró létrehoz egy kimenő kapcsolat tooAzure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="77de1-139">hello gateway creates an outbound connection tooAzure Service Bus.</span></span> <span data-ttu-id="77de1-140">A kimenő portokon kommunikál: TCP 443-as (alapértelmezett), 5671, 5672, 9350 – 9354-es.</span><span class="sxs-lookup"><span data-stu-id="77de1-140">It communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span>  <span data-ttu-id="77de1-141">hello átjáró nincs szükség a bejövő portra.</span><span class="sxs-lookup"><span data-stu-id="77de1-141">hello gateway does not require inbound ports.</span></span>

<span data-ttu-id="77de1-142">Javasoljuk, hogy engedélyezett hello IP-címet az adatterületen, a tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="77de1-142">We recommend you whitelist hello IP addresses for your data region in your firewall.</span></span> <span data-ttu-id="77de1-143">Letöltheti a hello [Microsoft Azure Datacenter IP-lista](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="77de1-143">You can download hello [Microsoft Azure Datacenter IP list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="77de1-144">A lista a heti frissül.</span><span class="sxs-lookup"><span data-stu-id="77de1-144">This list is updated weekly.</span></span>

> [!NOTE]
> <span data-ttu-id="77de1-145">hello IP-címek szerepel-e hello Azure Datacenter IP CIDR-formátumban vannak.</span><span class="sxs-lookup"><span data-stu-id="77de1-145">hello IP Addresses listed in hello Azure Datacenter IP list are in CIDR notation.</span></span> <span data-ttu-id="77de1-146">Például: 10.0.0.0/24 nem jelenti azt 10.0.0.0 10.0.0.24 keresztül.</span><span class="sxs-lookup"><span data-stu-id="77de1-146">For example, 10.0.0.0/24 does not mean 10.0.0.0 through 10.0.0.24.</span></span> <span data-ttu-id="77de1-147">További tudnivalók hello [CIDR-jelöléssel](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="77de1-147">Learn more about hello [CIDR notation](http://whatismyipaddress.com/cidr).</span></span>
>
>

<span data-ttu-id="77de1-148">Az alábbiakban hello hello átjáró által használt hello teljesen minősített tartománynév.</span><span class="sxs-lookup"><span data-stu-id="77de1-148">hello following are hello fully qualified domain names used by hello gateway.</span></span>

| <span data-ttu-id="77de1-149">Tartománynevek</span><span class="sxs-lookup"><span data-stu-id="77de1-149">Domain names</span></span> | <span data-ttu-id="77de1-150">Kimenő portok</span><span class="sxs-lookup"><span data-stu-id="77de1-150">Outbound ports</span></span> | <span data-ttu-id="77de1-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="77de1-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="77de1-152">*. powerbi.com webhelyre</span><span class="sxs-lookup"><span data-stu-id="77de1-152">*.powerbi.com</span></span> |<span data-ttu-id="77de1-153">80</span><span class="sxs-lookup"><span data-stu-id="77de1-153">80</span></span> |<span data-ttu-id="77de1-154">HTTP toodownload hello telepítő használt.</span><span class="sxs-lookup"><span data-stu-id="77de1-154">HTTP used toodownload hello installer.</span></span> |
| <span data-ttu-id="77de1-155">*. powerbi.com webhelyre</span><span class="sxs-lookup"><span data-stu-id="77de1-155">*.powerbi.com</span></span> |<span data-ttu-id="77de1-156">443</span><span class="sxs-lookup"><span data-stu-id="77de1-156">443</span></span> |<span data-ttu-id="77de1-157">HTTPS</span><span class="sxs-lookup"><span data-stu-id="77de1-157">HTTPS</span></span> |
| <span data-ttu-id="77de1-158">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="77de1-158">*.analysis.windows.net</span></span> |<span data-ttu-id="77de1-159">443</span><span class="sxs-lookup"><span data-stu-id="77de1-159">443</span></span> |<span data-ttu-id="77de1-160">HTTPS</span><span class="sxs-lookup"><span data-stu-id="77de1-160">HTTPS</span></span> |
| <span data-ttu-id="77de1-161">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="77de1-161">*.login.windows.net</span></span> |<span data-ttu-id="77de1-162">443</span><span class="sxs-lookup"><span data-stu-id="77de1-162">443</span></span> |<span data-ttu-id="77de1-163">HTTPS</span><span class="sxs-lookup"><span data-stu-id="77de1-163">HTTPS</span></span> |
| <span data-ttu-id="77de1-164">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="77de1-164">*.servicebus.windows.net</span></span> |<span data-ttu-id="77de1-165">5671-5672</span><span class="sxs-lookup"><span data-stu-id="77de1-165">5671-5672</span></span> |<span data-ttu-id="77de1-166">Speciális üzenetsor-kezelési protokoll (AMQP)</span><span class="sxs-lookup"><span data-stu-id="77de1-166">Advanced Message Queuing Protocol (AMQP)</span></span> |
| <span data-ttu-id="77de1-167">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="77de1-167">*.servicebus.windows.net</span></span> |<span data-ttu-id="77de1-168">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="77de1-168">443, 9350-9354</span></span> |<span data-ttu-id="77de1-169">A Service Bus Relay (a 443-as kér a hozzáférés-vezérlés jogkivonat beszerzése) TCP-n keresztül figyelői</span><span class="sxs-lookup"><span data-stu-id="77de1-169">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> |
| <span data-ttu-id="77de1-170">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="77de1-170">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="77de1-171">443</span><span class="sxs-lookup"><span data-stu-id="77de1-171">443</span></span> |<span data-ttu-id="77de1-172">HTTPS</span><span class="sxs-lookup"><span data-stu-id="77de1-172">HTTPS</span></span> |
| <span data-ttu-id="77de1-173">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="77de1-173">*.core.windows.net</span></span> |<span data-ttu-id="77de1-174">443</span><span class="sxs-lookup"><span data-stu-id="77de1-174">443</span></span> |<span data-ttu-id="77de1-175">HTTPS</span><span class="sxs-lookup"><span data-stu-id="77de1-175">HTTPS</span></span> |
| <span data-ttu-id="77de1-176">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="77de1-176">login.microsoftonline.com</span></span> |<span data-ttu-id="77de1-177">443</span><span class="sxs-lookup"><span data-stu-id="77de1-177">443</span></span> |<span data-ttu-id="77de1-178">HTTPS</span><span class="sxs-lookup"><span data-stu-id="77de1-178">HTTPS</span></span> |
| <span data-ttu-id="77de1-179">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="77de1-179">*.msftncsi.com</span></span> |<span data-ttu-id="77de1-180">443</span><span class="sxs-lookup"><span data-stu-id="77de1-180">443</span></span> |<span data-ttu-id="77de1-181">Használja a tootest internetkapcsolattal, ha hello átjáró által hello Power BI szolgáltatás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="77de1-181">Used tootest internet connectivity if hello gateway is unreachable by hello Power BI service.</span></span> |
| <span data-ttu-id="77de1-182">*.microsoftonline-p.com</span><span class="sxs-lookup"><span data-stu-id="77de1-182">*.microsoftonline-p.com</span></span> |<span data-ttu-id="77de1-183">443</span><span class="sxs-lookup"><span data-stu-id="77de1-183">443</span></span> |<span data-ttu-id="77de1-184">Konfigurációjától függően a hitelesítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="77de1-184">Used for authentication depending on configuration.</span></span> |

### <span data-ttu-id="77de1-185"><a name="force-https"></a>Az Azure Service Bus HTTPS-kommunikációt kényszerítése</span><span class="sxs-lookup"><span data-stu-id="77de1-185"><a name="force-https"></a>Forcing HTTPS communication with Azure Service Bus</span></span>
<span data-ttu-id="77de1-186">Az Azure Service Bus hello átjáró toocommunicate kényszerítheti a közvetlen TCP; helyett HTTPS használatával azonban ez úgy is jelentősen csökkenti a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="77de1-186">You can force hello gateway toocommunicate with Azure Service Bus by using HTTPS instead of direct TCP; however, doing so can greatly reduce performance.</span></span> <span data-ttu-id="77de1-187">Módosíthatja a hello *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* hello érték módosításával fájl `AutoDetect` túl`Https`.</span><span class="sxs-lookup"><span data-stu-id="77de1-187">You can modify hello *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* file by changing hello value from `AutoDetect` too`Https`.</span></span> <span data-ttu-id="77de1-188">Ez a fájl általában *C:\Program Files\On helyszíni adatátjáró*.</span><span class="sxs-lookup"><span data-stu-id="77de1-188">This file is typically located at *C:\Program Files\On-premises data gateway*.</span></span>

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <span data-ttu-id="77de1-189"><a name="faq"></a>Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="77de1-189"><a name="faq"></a>Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="77de1-190">Általános kérdések</span><span class="sxs-lookup"><span data-stu-id="77de1-190">General</span></span>

<span data-ttu-id="77de1-191">**A Q**: van egy átjáró adatforrások hello felhőben, például az Azure SQL Database?</span><span class="sxs-lookup"><span data-stu-id="77de1-191">**Q**: Do I need a gateway for data sources in hello cloud, such as Azure SQL Database?</span></span> <br/><span data-ttu-id="77de1-192">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="77de1-192">
**A**: No.</span></span> <span data-ttu-id="77de1-193">Egy átjáró tooon helyi adatforrások csak csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="77de1-193">A gateway connects tooon-premises data sources only.</span></span>

<span data-ttu-id="77de1-194">**A Q**: rendelkezik hello átjáró hello hello adatforrásként azonos számítógépre telepített toobe?</span><span class="sxs-lookup"><span data-stu-id="77de1-194">**Q**: Does hello gateway have toobe installed on hello same machine as hello data source?</span></span> <br/><span data-ttu-id="77de1-195">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="77de1-195">
**A**: No.</span></span> <span data-ttu-id="77de1-196">hello átjáró toohello az adatforráshoz megadott hello kapcsolat adatainak felhasználásával csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="77de1-196">hello gateway connects toohello data source using hello connection information that was provided.</span></span> <span data-ttu-id="77de1-197">Vegye figyelembe a hello átjáró abban az értelemben, ügyfél-alkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="77de1-197">Consider hello gateway as a client application in this sense.</span></span> <span data-ttu-id="77de1-198">hello csak regisztrálnia kell az hello funkció tooconnect toohello kiszolgáló nevét a megadott, általában a hello azonos hálózati.</span><span class="sxs-lookup"><span data-stu-id="77de1-198">hello gateway just needs hello capability tooconnect toohello server name that was provided, typically on hello same network.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="77de1-199">**A Q**: Miért ne I kell toouse munkahelyi vagy iskolai fiók toosign a?</span><span class="sxs-lookup"><span data-stu-id="77de1-199">**Q**: Why do I need toouse a work or school account toosign in?</span></span> <br/><span data-ttu-id="77de1-200">
**A**: csak használja az Azure munkahelyi vagy iskolai fiókkal, hello helyszíni az átjáró telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="77de1-200">
**A**: You can only use an Azure work or school account when you install hello on-premises data gateway.</span></span> <span data-ttu-id="77de1-201">A bejelentkezési fiók egy Azure Active Directory (Azure AD) által felügyelt bérlői tárolja.</span><span class="sxs-lookup"><span data-stu-id="77de1-201">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="77de1-202">Az Azure AD-fiókot egyszerű felhasználónév (UPN) általában hello e-mail cím megfelelő.</span><span class="sxs-lookup"><span data-stu-id="77de1-202">Usually, your Azure AD account's user principal name (UPN) matches hello email address.</span></span>

<span data-ttu-id="77de1-203">**A Q**: a hitelesítő adataimat tároló?</span><span class="sxs-lookup"><span data-stu-id="77de1-203">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="77de1-204">
**A**: hello adatforráshoz beírt hitelesítő adatok titkosítottak és a tárolt hello átjáró Felhőszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="77de1-204">
**A**: hello credentials that you enter for a data source are encrypted and stored in hello Gateway Cloud Service.</span></span> <span data-ttu-id="77de1-205">hello hitelesítő adatok lesznek visszafejtve hello a helyszíni adatok átjáróra.</span><span class="sxs-lookup"><span data-stu-id="77de1-205">hello credentials are decrypted at hello on-premises data gateway.</span></span>

<span data-ttu-id="77de1-206">**A Q**: vannak-e hálózati sávszélesség vonatkozó követelmények?</span><span class="sxs-lookup"><span data-stu-id="77de1-206">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="77de1-207">
**A**: azt javasoljuk rendelkezik, a hálózati kapcsolat van a jó teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="77de1-207">
**A**: It's recommend your network connection has good throughput.</span></span> <span data-ttu-id="77de1-208">Minden környezet más, és elküldött adatok mennyisége hello hello eredményekre van hatással.</span><span class="sxs-lookup"><span data-stu-id="77de1-208">Every environment is different, and hello amount of data being sent affects hello results.</span></span> <span data-ttu-id="77de1-209">ExpressRoute használata segíthet a helyszíni és az Azure adatközpontjaiban hello közötti átviteli szintű tooguarantee.</span><span class="sxs-lookup"><span data-stu-id="77de1-209">Using ExpressRoute could help tooguarantee a level of throughput between on-premises and hello Azure datacenters.</span></span>
<span data-ttu-id="77de1-210">Hello külső eszköz Azure sebesség teszt app toohelp mérőműszer használhatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="77de1-210">You can use hello third-party tool Azure Speed Test app toohelp gauge your throughput.</span></span>

<span data-ttu-id="77de1-211">**A Q**: Mi az az hello késés futó lekérdezések tooa adatforrás hello átjáró?</span><span class="sxs-lookup"><span data-stu-id="77de1-211">**Q**: What is hello latency for running queries tooa data source from hello gateway?</span></span> <span data-ttu-id="77de1-212">Mi az a legjobb architektúra hello?</span><span class="sxs-lookup"><span data-stu-id="77de1-212">What is hello best architecture?</span></span> <br/><span data-ttu-id="77de1-213">
**A**: tooreduce hálózati késés, telepítés hello átjáró lehető Bezárás toohello adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="77de1-213">
**A**: tooreduce network latency, install hello gateway as close toohello data source as possible.</span></span> <span data-ttu-id="77de1-214">Hello tényleges adatforrás hello átjáró telepíthető, ha a közelségi kapcsolat minimálisra hello késés rendszerben jelent meg.</span><span class="sxs-lookup"><span data-stu-id="77de1-214">If you can install hello gateway on hello actual data source, this proximity minimizes hello latency introduced.</span></span> <span data-ttu-id="77de1-215">Érdemes lehet túl hello adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="77de1-215">Consider hello datacenters too.</span></span> <span data-ttu-id="77de1-216">Például ha a szolgáltatás hello USA nyugati régiója datacenter használ, és egy Azure virtuális Gépen található SQL-kiszolgálóval rendelkezik, az Azure virtuális gép kell hello USA nyugati régiója túl.</span><span class="sxs-lookup"><span data-stu-id="77de1-216">For example, if your service uses hello West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in hello West US too.</span></span> <span data-ttu-id="77de1-217">A közelségi kapcsolat minimálisra csökkenti a késést, és ezzel elkerülheti a kimenő forgalom költségek hello Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="77de1-217">This proximity minimizes latency and avoids egress charges on hello Azure VM.</span></span>

<span data-ttu-id="77de1-218">**A Q**: hogyan történik az eredmények küldött vissza toohello felhő?</span><span class="sxs-lookup"><span data-stu-id="77de1-218">**Q**: How are results sent back toohello cloud?</span></span> <br/><span data-ttu-id="77de1-219">
**A**: eredmények hello Azure Service Bus keresztül zajlik.</span><span class="sxs-lookup"><span data-stu-id="77de1-219">
**A**: Results are sent through hello Azure Service Bus.</span></span>

<span data-ttu-id="77de1-220">**A Q**: vannak-e a bejövő kapcsolatok toohello átjáró hello felhőből?</span><span class="sxs-lookup"><span data-stu-id="77de1-220">**Q**: Are there any inbound connections toohello gateway from hello cloud?</span></span> <br/><span data-ttu-id="77de1-221">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="77de1-221">
**A**: No.</span></span> <span data-ttu-id="77de1-222">hello átjáró kimenő kapcsolatok tooAzure Service Bus használja.</span><span class="sxs-lookup"><span data-stu-id="77de1-222">hello gateway uses outbound connections tooAzure Service Bus.</span></span>

<span data-ttu-id="77de1-223">**A Q**: Mi történik, ha az általam letiltottak kimenő kapcsolatokat?</span><span class="sxs-lookup"><span data-stu-id="77de1-223">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="77de1-224">Mire van szükségem az tooopen?</span><span class="sxs-lookup"><span data-stu-id="77de1-224">What do I need tooopen?</span></span> <br/><span data-ttu-id="77de1-225">
**A**: hello portok és a gazdagépekhez, amelyek hello átjárót használja.</span><span class="sxs-lookup"><span data-stu-id="77de1-225">
**A**: See hello ports and hosts that hello gateway uses.</span></span>

<span data-ttu-id="77de1-226">**A Q**: hello tényleges Windows-szolgáltatás úgynevezett?</span><span class="sxs-lookup"><span data-stu-id="77de1-226">**Q**: What is hello actual Windows service called?</span></span><br/><span data-ttu-id="77de1-227">
**A**: A szolgáltatások hello átjárót a helyszíni átjáró szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="77de1-227">
**A**: In Services, hello gateway is called On-premises data gateway service.</span></span>

<span data-ttu-id="77de1-228">**A Q**: is hello átjáró Windows-szolgáltatás Azure Active Directory-fiókkal való futtatása?</span><span class="sxs-lookup"><span data-stu-id="77de1-228">**Q**: Can hello gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="77de1-229">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="77de1-229">
**A**: No.</span></span> <span data-ttu-id="77de1-230">Windows-szolgáltatás hello rendelkeznie kell egy érvényes Windows-fiókot.</span><span class="sxs-lookup"><span data-stu-id="77de1-230">hello Windows service must have a valid Windows account.</span></span> <span data-ttu-id="77de1-231">Alapértelmezés szerint hello szolgáltatás SID NT SERVICE\PBIEgwService hello szolgáltatást futtatja.</span><span class="sxs-lookup"><span data-stu-id="77de1-231">By default, hello service runs with hello Service SID, NT SERVICE\PBIEgwService.</span></span>

### <span data-ttu-id="77de1-232"><a name="high-availability"></a>Magas rendelkezésre állás és a katasztrófa utáni helyreállítás</span><span class="sxs-lookup"><span data-stu-id="77de1-232"><a name="high-availability"></a>High availability and disaster recovery</span></span>

<span data-ttu-id="77de1-233">**A Q**: milyen beállítások érhetők el a vész-helyreállítási?</span><span class="sxs-lookup"><span data-stu-id="77de1-233">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="77de1-234">
**A**: hello helyreállítási kulcs toorestore használhatja, vagy helyezze át az átjárót.</span><span class="sxs-lookup"><span data-stu-id="77de1-234">
**A**: You can use hello recovery key toorestore or move a gateway.</span></span> <span data-ttu-id="77de1-235">Hello átjáró telepítésekor adja meg a hello helyreállítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="77de1-235">When you install hello gateway, specify hello recovery key.</span></span>

<span data-ttu-id="77de1-236">**A Q**: Miért érdemes hello hello helyreállítási kulcs?</span><span class="sxs-lookup"><span data-stu-id="77de1-236">**Q**: What is hello benefit of hello recovery key?</span></span> <br/><span data-ttu-id="77de1-237">
**A**: hello helyreállítási kulcs tartalmaz egy módja toomigrate, vagy az átjáró beállításainak katasztrófa utáni helyreállításhoz.</span><span class="sxs-lookup"><span data-stu-id="77de1-237">
**A**: hello recovery key provides a way toomigrate or recover your gateway settings after a disaster.</span></span>

## <span data-ttu-id="77de1-238"><a name="troubleshooting"></a>Hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="77de1-238"><a name="troubleshooting"> </a>Troubleshooting</span></span>

<span data-ttu-id="77de1-239">**A Q**: hogyan láthatja meg, hogy milyen lekérdezések küldési toohello helyszíni adatforrás?</span><span class="sxs-lookup"><span data-stu-id="77de1-239">**Q**: How can I see what queries are being sent toohello on-premises data source?</span></span> <br/><span data-ttu-id="77de1-240">
**A**: engedélyezheti a lekérdezés nyomon követése, beleértve a küldött hello lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="77de1-240">
**A**: You can enable query tracing, which includes hello queries that are sent.</span></span> <span data-ttu-id="77de1-241">Ne felejtse el vissza toohello eredeti érték Miután befejezte a hibakeresési nyomkövetés toochange lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="77de1-241">Remember toochange query tracing back toohello original value when done troubleshooting.</span></span> <span data-ttu-id="77de1-242">A lekérdezés nyomon követése engedélyezve van a Kilépés hoz létre a nagyobb naplókat.</span><span class="sxs-lookup"><span data-stu-id="77de1-242">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="77de1-243">Is megtekintheti, hogy az adatforrás rendelkezik a nyomkövetési lekérdezések eszközök.</span><span class="sxs-lookup"><span data-stu-id="77de1-243">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="77de1-244">Például használhatja bővített eseményektől vagy SQL Profiler az SQL Server és az Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="77de1-244">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="77de1-245">**A Q**: Ha az hello átjáró naplói nem?</span><span class="sxs-lookup"><span data-stu-id="77de1-245">**Q**: Where are hello gateway logs?</span></span> <br/><span data-ttu-id="77de1-246">
**A**: naplók lásd a témakör későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="77de1-246">
**A**: See Logs later in this topic.</span></span>

### <span data-ttu-id="77de1-247"><a name="update"></a>Frissítés toohello legújabb verziója</span><span class="sxs-lookup"><span data-stu-id="77de1-247"><a name="update"></a>Update toohello latest version</span></span>

<span data-ttu-id="77de1-248">Számos problémájának is surface, amikor hello átjáró verziója elavult.</span><span class="sxs-lookup"><span data-stu-id="77de1-248">Many issues can surface when hello gateway version becomes outdated.</span></span> <span data-ttu-id="77de1-249">Jó általános gyakorlat győződjön meg arról, hogy hello legújabb verzióját használja-e.</span><span class="sxs-lookup"><span data-stu-id="77de1-249">As good general practice, make sure that you use hello latest version.</span></span> <span data-ttu-id="77de1-250">Ha hello átjáró havonta vagy hosszabb ideig nem frissítette, akkor előfordulhat, hogy hello hello átjáró legújabb verzióját telepítse, és ha Reprodukálja hello probléma.</span><span class="sxs-lookup"><span data-stu-id="77de1-250">If you haven't updated hello gateway for a month or longer, you might consider installing hello latest version of hello gateway, and see if you can reproduce hello issue.</span></span>

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="77de1-251">Hiba: Nem sikerült tooadd felhasználói toogroup.</span><span class="sxs-lookup"><span data-stu-id="77de1-251">Error: Failed tooadd user toogroup.</span></span> <span data-ttu-id="77de1-252">(Felhasználói-2147463168 PBIEgwService teljesítmény)</span><span class="sxs-lookup"><span data-stu-id="77de1-252">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="77de1-253">Ez a hiba kaphat, ha tooinstall hello átjáró tartományvezérlőn, amely nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="77de1-253">You might get this error if you try tooinstall hello gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="77de1-254">Győződjön meg arról, hogy egy számítógép, amely a tartományvezérlő nem hello-átjáró üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="77de1-254">Make sure that you deploy hello gateway on a machine that isn't a domain controller.</span></span>

## <span data-ttu-id="77de1-255"><a name="logs"></a>Naplók</span><span class="sxs-lookup"><span data-stu-id="77de1-255"><a name="logs"></a>Logs</span></span>

<span data-ttu-id="77de1-256">Naplófájlok egy fontos erőforrás hibaelhárítása során.</span><span class="sxs-lookup"><span data-stu-id="77de1-256">Log files are an important resource when troubleshooting.</span></span>

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="77de1-257">Vállalati átjáró service naplóit</span><span class="sxs-lookup"><span data-stu-id="77de1-257">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a><span data-ttu-id="77de1-258">Konfigurációs naplók</span><span class="sxs-lookup"><span data-stu-id="77de1-258">Configuration logs</span></span>

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a><span data-ttu-id="77de1-259">Eseménynaplók</span><span class="sxs-lookup"><span data-stu-id="77de1-259">Event logs</span></span>

<span data-ttu-id="77de1-260">Hello az adatkezelési átjáró és PowerBIGateway naplók alapján is megtalálhatja **alkalmazás- és szolgáltatásnaplók**.</span><span class="sxs-lookup"><span data-stu-id="77de1-260">You can find hello Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>


## <span data-ttu-id="77de1-261"><a name="telemetry"></a>Telemetria</span><span class="sxs-lookup"><span data-stu-id="77de1-261"><a name="telemetry"></a>Telemetry</span></span>
<span data-ttu-id="77de1-262">Telemetriai adatainak figyelés és hibaelhárítás céljából is használható.</span><span class="sxs-lookup"><span data-stu-id="77de1-262">Telemetry can be used for monitoring and troubleshooting.</span></span> <span data-ttu-id="77de1-263">Alapértelmezés szerint</span><span class="sxs-lookup"><span data-stu-id="77de1-263">By default</span></span>

<span data-ttu-id="77de1-264">**a telemetria tooturn**</span><span class="sxs-lookup"><span data-stu-id="77de1-264">**tooturn on telemetry**</span></span>

1.  <span data-ttu-id="77de1-265">Ellenőrizze a hello helyszíni átjáró ügyfél adatkönyvtára hello számítógépen.</span><span class="sxs-lookup"><span data-stu-id="77de1-265">Check hello On-premises data gateway client directory on hello computer.</span></span> <span data-ttu-id="77de1-266">Általában **%systemdrive%\Program Files\On helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="77de1-266">Typically, it is **%systemdrive%\Program Files\On-premises data gateway**.</span></span> <span data-ttu-id="77de1-267">Nyissa meg a szolgáltatások konzolt, és ellenőrizze az elérési út tooexecutable hello: hello helyszíni átjáró szolgáltatás tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="77de1-267">Or, you can open a Services console and check hello Path tooexecutable: A property of hello On-premises data gateway service.</span></span>
2.  <span data-ttu-id="77de1-268">Az ügyfél directory hello Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="77de1-268">In hello Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config file from client directory.</span></span> <span data-ttu-id="77de1-269">Hello SendTelemetry beállítás tootrue módosítása.</span><span class="sxs-lookup"><span data-stu-id="77de1-269">Change hello SendTelemetry setting tootrue.</span></span>
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  <span data-ttu-id="77de1-270">A módosítások mentéséhez, és indítsa újra a Windows hello-szolgáltatás: A helyszíni átjáró szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="77de1-270">Save your changes and restart hello Windows service: On-premises data gateway service.</span></span>




## <a name="next-steps"></a><span data-ttu-id="77de1-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="77de1-271">Next steps</span></span>
* [<span data-ttu-id="77de1-272">Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="77de1-272">Manage Analysis Services</span></span>](analysis-services-manage.md)
* [<span data-ttu-id="77de1-273">Adatok beolvasása az Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="77de1-273">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)
