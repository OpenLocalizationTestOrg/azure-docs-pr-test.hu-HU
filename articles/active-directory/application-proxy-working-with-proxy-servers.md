---
title: "Munka a meglévő helyszíni proxykiszolgálókra és az Azure AD |} Microsoft Docs"
description: "Bemutatja, hogyan adhat az meglévő helyszíni proxy kiszolgálóhoz."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: bdca442755507c4ffe8d43692c5b7f2aa3a746f3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a><span data-ttu-id="4f5a2-103">A meglévő helyszíni proxykiszolgálókkal működik</span><span class="sxs-lookup"><span data-stu-id="4f5a2-103">Work with existing on-premises proxy servers</span></span>

<span data-ttu-id="4f5a2-104">Ez a cikk ismerteti, hogyan konfigurálja az Azure Active Directory (Azure AD) alkalmazásproxy összekötőket az kimenőproxy-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-104">This article explains how to configure Azure Active Directory (Azure AD) Application Proxy connectors to work with outbound proxy servers.</span></span> <span data-ttu-id="4f5a2-105">Az ügyfelek számára hálózati környezetekben, ahol a meglévő proxyk szolgál.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-105">It is intended for customers with network environments that have existing proxies.</span></span>

<span data-ttu-id="4f5a2-106">Először fő központi telepítési forgatókönyvek alapján:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-106">We start by looking at these main deployment scenarios:</span></span>
* <span data-ttu-id="4f5a2-107">Konfigurálja az összekötőket a helyszíni kimenő proxyk kihagyásához.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-107">Configure connectors to bypass your on-premises outbound proxies.</span></span>
* <span data-ttu-id="4f5a2-108">Konfigurálja az összekötőket egy kimenő proxy használatát az Azure AD-alkalmazásproxy eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-108">Configure connectors to use an outbound proxy to access Azure AD Application Proxy.</span></span>

<span data-ttu-id="4f5a2-109">Összekötők működésével kapcsolatos további információkért lásd: [megértéséhez Azure AD-alkalmazásproxy összekötők](application-proxy-understand-connectors.md).</span><span class="sxs-lookup"><span data-stu-id="4f5a2-109">For more information about how connectors work, see [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md).</span></span>

## <a name="configure-the-outbound-proxy"></a><span data-ttu-id="4f5a2-110">A kimenő proxy konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4f5a2-110">Configure the outbound proxy</span></span>

<span data-ttu-id="4f5a2-111">Ha egy kimenő proxy a környezetben, fiók használatával a megfelelő engedélyekkel a kimenő proxy konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-111">If you have an outbound proxy in your environment, use an account with appropriate permissions to configure the outbound proxy.</span></span> <span data-ttu-id="4f5a2-112">A telepítő a telepítés tesz a felhasználó környezetében fut, mert a konfiguráció ellenőrizheti a Microsoft Edge vagy egy másik böngészőben.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-112">Because the installer runs in the context of the user who's doing the installation, you can check the configuration by using Microsoft Edge or another Internet browser.</span></span>

<span data-ttu-id="4f5a2-113">A Proxybeállítások konfigurálása a Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-113">To configure the proxy settings in Microsoft Edge:</span></span>

1. <span data-ttu-id="4f5a2-114">Ugrás **beállítások** > **nézet speciális beállítások** > **nyissa meg a proxybeállítások** > **manuálisProxytelepítése**.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-114">Go to **Settings** > **View Advanced Settings** > **Open Proxy Settings** > **Manual Proxy Setup**.</span></span>
2. <span data-ttu-id="4f5a2-115">Állítsa be **proxykiszolgálót használni** való **a**, jelölje be a **nem használni a proxykiszolgálót a helyi (intranetes) címek** jelölőnégyzetet, majd módosítsa a címet és portot a helyi megfelelően proxykiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-115">Set **Use a proxy server** to **On**, select the **Don’t use the proxy server for local (intranet) addresses** check box, and then change the address and port to reflect your local proxy server.</span></span>
3. <span data-ttu-id="4f5a2-116">Töltse ki a szükséges proxybeállításokat.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-116">Fill in the necessary proxy settings.</span></span>

   ![A proxykiszolgáló beállításai párbeszédpanel](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a><span data-ttu-id="4f5a2-118">Kimenő proxyk figyelmen kívül hagyása</span><span class="sxs-lookup"><span data-stu-id="4f5a2-118">Bypass outbound proxies</span></span>

<span data-ttu-id="4f5a2-119">Összekötők rendelkezik az operációs rendszer alapösszetevőt kimenő kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-119">Connectors have underlying OS components that make outbound requests.</span></span> <span data-ttu-id="4f5a2-120">Ezeket az összetevőket automatikusan megpróbálja megkeresni a proxykiszolgálót a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-120">These components automatically attempt to locate a proxy server on the network.</span></span> <span data-ttu-id="4f5a2-121">Webes Proxy automatikus felderítését a lekérdezés (WPA), akkor használja, ha engedélyezve van a környezetben.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-121">They use Web Proxy Auto-Discovery (WPAD), if it's enabled in the environment.</span></span>

<span data-ttu-id="4f5a2-122">Az operációs rendszer összetevőit megpróbálja megkeresni a proxykiszolgáló wpad.domainsuffix a DNS-címkeresést elvégzésével.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-122">The OS components attempt to locate a proxy server by carrying out a DNS lookup for wpad.domainsuffix.</span></span> <span data-ttu-id="4f5a2-123">Ha így megoldódik, a DNS-ben, HTTP-kérelem majd történik wpad.dat IP-címét.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-123">If this resolves in DNS, an HTTP request is then made to the IP address for wpad.dat.</span></span> <span data-ttu-id="4f5a2-124">A kérelem válik a proxy konfigurációs parancsprogramja a környezetben.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-124">This request becomes the proxy configuration script in your environment.</span></span> <span data-ttu-id="4f5a2-125">Az összekötő jelöljön ki egy kimenő proxy használja ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-125">The connector uses this script to select an outbound proxy server.</span></span> <span data-ttu-id="4f5a2-126">Azonban összekötő forgalom előfordulhat, hogy még nem halad át, a proxy szükség további konfigurációs beállítások miatt.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-126">However, connector traffic might still not go through, because of additional configuration settings needed on the proxy.</span></span>

<span data-ttu-id="4f5a2-127">Beállíthatja, hogy az összekötő a helyszíni proxy győződjön meg arról, hogy használ-e közvetlen kapcsolat az Azure-szolgáltatásokhoz való kihagyásához.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-127">You can configure the connector to bypass your on-premises proxy to ensure that it uses direct connectivity to the Azure services.</span></span> <span data-ttu-id="4f5a2-128">Ezt a módszert javasoljuk (Ha a hálózati házirend lehetővé teszi, hogy az), mert az azt jelenti, hogy rendelkezik-e egy kisebb konfigurációs fenntartásához.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-128">We recommend this approach (if your network policy allows for it), because it means that you have one less configuration to maintain.</span></span>

<span data-ttu-id="4f5a2-129">Tiltsa le az összekötő kimenő proxy használatát, a C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config fájl szerkesztésével, és adja hozzá a *System.NET névtérbeli* látható a fenti szakaszban:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-129">To disable outbound proxy usage for the connector, edit the C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add the *system.net* section shown in this code sample:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>
    <defaultProxy enabled="false"></defaultProxy>
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```
<span data-ttu-id="4f5a2-130">Győződjön meg arról, hogy az összekötő frissítési szolgáltatást is megkerüli a proxy, egy hasonló módosítást a ApplicationProxyConnectorUpdaterService.exe.config fájl helye: C:\Program Files\Microsoft AAD App Proxy Connector Frissítőjének.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-130">To ensure that the Connector Updater service also bypasses the proxy, make a similar change to the ApplicationProxyConnectorUpdaterService.exe.config file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater.</span></span>

<span data-ttu-id="4f5a2-131">Ügyeljen arra, hogy készítsen másolatot az eredeti fájlok abban az esetben meg kell visszaállítania az alapértelmezett .config kiterjesztésű fájlokat.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-131">Be sure to make copies of the original files, in case you need to revert to the default .config files.</span></span>

## <a name="use-the-outbound-proxy-server"></a><span data-ttu-id="4f5a2-132">A kimenő proxykiszolgáló használata</span><span class="sxs-lookup"><span data-stu-id="4f5a2-132">Use the outbound proxy server</span></span>

<span data-ttu-id="4f5a2-133">Egyes környezetekben megkövetelése minden kimenő forgalom haladhat végig az kimenő proxy, kivételhiba jelentkezése nélkül.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-133">Some environments require all outbound traffic to go through an outbound proxy, without exception.</span></span> <span data-ttu-id="4f5a2-134">Ennek eredményeképpen a proxy megkerülése lehetőség nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-134">As a result, bypassing the proxy is not an option.</span></span>

<span data-ttu-id="4f5a2-135">Nyissa meg a kimenő proxy connector forgalmat is konfigurálhatja, az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-135">You can configure the connector traffic to go through the outbound proxy, as shown in the following diagram:</span></span>

 ![Egy kimenő proxyn keresztül történő Ugrás az Azure AD-alkalmazásproxy-összekötő forgalom konfigurálása](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

<span data-ttu-id="4f5a2-137">Miatt, amelyek csak a kimenő forgalom, nincs szükség a tűzfalon keresztüli bejövő hozzáférés konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-137">As a result of having only outbound traffic, there's no need to configure inbound access through your firewalls.</span></span>

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a><span data-ttu-id="4f5a2-138">1. lépés: Az összekötő és a kapcsolódó szolgáltatások haladhat végig a kimenő proxy konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4f5a2-138">Step 1: Configure the connector and related services to go through the outbound proxy</span></span>

<span data-ttu-id="4f5a2-139">Korábban vonatkozik, ha WPAD engedélyezve a környezetben, és megfelelően konfigurálva, az összekötő automatikusan a kimenő proxykiszolgáló felderítése, és megkísérli használni azt.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-139">As covered earlier, if WPAD is enabled in the environment and configured appropriately, the connector will automatically discover the outbound proxy server and attempt to use it.</span></span> <span data-ttu-id="4f5a2-140">Azonban explicit módon konfigurálhatja az összekötő egy kimenő proxy végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-140">However, you can explicitly configure the connector to go through an outbound proxy.</span></span>

<span data-ttu-id="4f5a2-141">Ehhez az szükséges, módosítsa a C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config fájlt, és adja hozzá a *System.NET névtérbeli* látható a fenti szakaszban.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-141">To do so, edit the C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add the *system.net* section shown in this code sample.</span></span> <span data-ttu-id="4f5a2-142">Változás *proxyserver:8080* a helyi proxykiszolgáló nevét vagy IP-cím és a portot, amelyet figyeli az megfelelően.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-142">Change *proxyserver:8080* to reflect your local proxy server name or IP address, and the port that it's listening on.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

<span data-ttu-id="4f5a2-143">Ezután konfigurálja a Connector frissítési szolgáltatást által hasonló módosítást végez a fájl helye: C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config proxy használatára.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-143">Next, configure the Connector Updater service to use the proxy by making a similar change to the file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span></span>

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a><span data-ttu-id="4f5a2-144">2. lépés: Az összekötő és a kapcsolódó szolgáltatások keresztül érkező forgalmat engedélyezi a proxy konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4f5a2-144">Step 2: Configure the proxy to allow traffic from the connector and related services to flow through</span></span>

<span data-ttu-id="4f5a2-145">Négy szempontot kell figyelembe venni a kimenő proxynál:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-145">There are four aspects to consider at the outbound proxy:</span></span>
* <span data-ttu-id="4f5a2-146">Proxy kimenő szabályok</span><span class="sxs-lookup"><span data-stu-id="4f5a2-146">Proxy outbound rules</span></span>
* <span data-ttu-id="4f5a2-147">A proxy hitelesítése</span><span class="sxs-lookup"><span data-stu-id="4f5a2-147">Proxy authentication</span></span>
* <span data-ttu-id="4f5a2-148">Proxy portok</span><span class="sxs-lookup"><span data-stu-id="4f5a2-148">Proxy ports</span></span>
* <span data-ttu-id="4f5a2-149">SSL-ellenőrzést</span><span class="sxs-lookup"><span data-stu-id="4f5a2-149">SSL inspection</span></span>

#### <a name="proxy-outbound-rules"></a><span data-ttu-id="4f5a2-150">Proxy kimenő szabályok</span><span class="sxs-lookup"><span data-stu-id="4f5a2-150">Proxy outbound rules</span></span>
<span data-ttu-id="4f5a2-151">Az összekötő szolgáltatás hozzáférés a következő végpontok hozzáférés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-151">Allow access to the following endpoints for connector service access:</span></span>

* <span data-ttu-id="4f5a2-152">*. msappproxy.net</span><span class="sxs-lookup"><span data-stu-id="4f5a2-152">*.msappproxy.net</span></span>
* <span data-ttu-id="4f5a2-153">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="4f5a2-153">*.servicebus.windows.net</span></span>

<span data-ttu-id="4f5a2-154">A kezdeti regisztráció a következő végpontok hozzáférés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-154">For initial registration, allow access to the following endpoints:</span></span>

* <span data-ttu-id="4f5a2-155">login.windows.net</span><span class="sxs-lookup"><span data-stu-id="4f5a2-155">login.windows.net</span></span>
* <span data-ttu-id="4f5a2-156">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="4f5a2-156">login.microsoftonline.com</span></span>

<span data-ttu-id="4f5a2-157">Ha nem engedélyezi a csatlakozást a teljes tartománynév alapján és IP-címtartományok Ehelyett meg kell adni, használja ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-157">If you can't allow connectivity by FQDN and need to specify IP ranges instead, use these options:</span></span>

* <span data-ttu-id="4f5a2-158">Az összes cél összekötő kimenő hozzáférés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-158">Allow the connector outbound access to all destinations.</span></span>
* <span data-ttu-id="4f5a2-159">Az összekötő kimenő hozzáférést nyújtson [Azure datacenter IP-címtartományok](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="4f5a2-159">Allow the connector outbound access to [Azure datacenter IP ranges](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span></span> <span data-ttu-id="4f5a2-160">Használatával a Azure datacenter IP-címtartománylista ellenőrző, hogy hetente kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-160">The challenge with using the list of Azure datacenter IP ranges is that it's updated weekly.</span></span> <span data-ttu-id="4f5a2-161">Győződjön meg arról, hogy a hozzáférési szabályok megfelelően vannak-e frissíti a folyamat bevezetni kell.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-161">You need to put a process in place to ensure that your access rules are updated accordingly.</span></span>

#### <a name="proxy-authentication"></a><span data-ttu-id="4f5a2-162">A proxy hitelesítése</span><span class="sxs-lookup"><span data-stu-id="4f5a2-162">Proxy authentication</span></span>

<span data-ttu-id="4f5a2-163">A proxy hitelesítése jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-163">Proxy authentication is not currently supported.</span></span> <span data-ttu-id="4f5a2-164">Az aktuális javasoljuk, hogy az összekötő a Internet célhelyre névtelen hozzáférés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-164">Our current recommendation is to allow the connector anonymous access to the Internet destinations.</span></span>

#### <a name="proxy-ports"></a><span data-ttu-id="4f5a2-165">Proxy portok</span><span class="sxs-lookup"><span data-stu-id="4f5a2-165">Proxy ports</span></span>

<span data-ttu-id="4f5a2-166">Az összekötő lehetővé teszi a kimenő SSL-alapú kapcsolatok KAPCSOLATI módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-166">The connector makes outbound SSL-based connections by using the CONNECT method.</span></span> <span data-ttu-id="4f5a2-167">Ez a módszer lényegében állít be egy alagúton, a kimenő proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-167">This method essentially sets up a tunnel through the outbound proxy.</span></span> <span data-ttu-id="4f5a2-168">Engedélyezi a 443-as és a 80-as portra tunneling a proxykiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-168">Configure the proxy server to allow tunneling to ports 443 and 80.</span></span>

>[!NOTE]
><span data-ttu-id="4f5a2-169">A Service Bus a HTTPS-KAPCSOLATON keresztül futtatása 443-as portot használja.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-169">When Service Bus runs over HTTPS, it uses port 443.</span></span> <span data-ttu-id="4f5a2-170">Azonban az alapértelmezés szerint a Service Bus kísérletet tett a közvetlen TCP-kapcsolatok, és csak akkor, ha közvetlen kapcsolatot sikertelen visszaáll HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-170">However, by default, Service Bus attempts direct TCP connections and falls back to HTTPS only if direct connectivity fails.</span></span>

<span data-ttu-id="4f5a2-171">Győződjön meg arról, hogy a Service Bus-forgalom is zajlik a kimenő proxykiszolgálón keresztül, győződjön meg arról, hogy az összekötő nem közvetlenül csatlakozni az Azure-szolgáltatások 9350 9352 és 5671 portok.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-171">To ensure that the Service Bus traffic is also sent through the outbound proxy server, ensure that the connector cannot directly connect to the Azure services for ports 9350, 9352, and 5671.</span></span>

#### <a name="ssl-inspection"></a><span data-ttu-id="4f5a2-172">SSL-ellenőrzést</span><span class="sxs-lookup"><span data-stu-id="4f5a2-172">SSL inspection</span></span>
<span data-ttu-id="4f5a2-173">Ne használjon SSL-ellenőrzést az összekötő-forgalom esetén, mert az összekötő forgalom problémákat okoz.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-173">Do not use SSL inspection for the connector traffic, because it causes problems for the connector traffic.</span></span>

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a><span data-ttu-id="4f5a2-174">Összekötő proxy problémák és a szolgáltatás kapcsolódási problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="4f5a2-174">Troubleshoot connector proxy problems and service connectivity issues</span></span>
<span data-ttu-id="4f5a2-175">Most már megtekintheti az összes forgalom a proxyn keresztül történő továbbítására.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-175">Now you should see all traffic flowing through the proxy.</span></span> <span data-ttu-id="4f5a2-176">Ha problémába ütközik, a következő hibaelhárítási információk segítségül szolgálhat.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-176">If you have problems, the following troubleshooting information should help.</span></span>

<span data-ttu-id="4f5a2-177">A legjobb azonosítása és összekötő csatlakozási problémák módja lehet az összekötő szolgáltatás hálózati rögzítőeszközt végrehajtani az összekötő-szolgáltatás indításakor.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-177">The best way to identify and troubleshoot connector connectivity issues is to take a network capture on the connector service while starting the connector service.</span></span> <span data-ttu-id="4f5a2-178">Ez nagyobb terhet jelenthetnek, gyors tippek az rögzítésével és hálózati nyomkövetés szűréshez tehát vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-178">This can be a daunting task, so let’s look at quick tips on capturing and filtering network traces.</span></span>

<span data-ttu-id="4f5a2-179">A felügyeleti eszköz az Ön által választott használható.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-179">You can use the monitoring tool of your choice.</span></span> <span data-ttu-id="4f5a2-180">Ez a cikk alkalmazásában Microsoft Network Monitor 3.4 használtuk.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-180">For the purposes of this article, we used Microsoft Network Monitor 3.4.</span></span> <span data-ttu-id="4f5a2-181">Is [töltse le a Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span><span class="sxs-lookup"><span data-stu-id="4f5a2-181">You can [download it from Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span></span>

<span data-ttu-id="4f5a2-182">A példák és szűrőket, amelyek a következő szakaszokban használjuk csak az adott hálózati figyelő, de bármely eszköz alkalmazhatja ezeket az alapelveket.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-182">The examples and filters that we use in the following sections are specific to Network Monitor, but the principles can be applied to any analysis tool.</span></span>

### <a name="take-a-capture-by-using-network-monitor"></a><span data-ttu-id="4f5a2-183">Tegye meg a rögzítés a Hálózatfigyelővel</span><span class="sxs-lookup"><span data-stu-id="4f5a2-183">Take a capture by using Network Monitor</span></span>

<span data-ttu-id="4f5a2-184">A Rögzítés indítása:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-184">To start a capture:</span></span>

1. <span data-ttu-id="4f5a2-185">Nyissa meg a hálózati figyelő, és kattintson a **új rögzítése**.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-185">Open Network Monitor and click **New Capture**.</span></span>
2. <span data-ttu-id="4f5a2-186">Kattintson a **Start** gombra.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-186">Click the **Start** button.</span></span>

   ![Hálózati figyelő ablak](./media/application-proxy-working-with-proxy-servers/network-capture.png)

<span data-ttu-id="4f5a2-188">Miután elkészült egy rögzítési, kattintson a **leállítása** gombra kell végződnie.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-188">After you complete a capture, click the **Stop** button to end it.</span></span>

### <a name="take-a-capture-of-connector-traffic"></a><span data-ttu-id="4f5a2-189">Az összekötő forgalom rögzítés igénybe</span><span class="sxs-lookup"><span data-stu-id="4f5a2-189">Take a capture of connector traffic</span></span>

<span data-ttu-id="4f5a2-190">A kezdeti hibaelhárítási, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-190">For initial troubleshooting, perform the following steps:</span></span>

1. <span data-ttu-id="4f5a2-191">A "Services.msc" parancsot állítsa le az Azure AD alkalmazásproxy-összekötő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-191">From services.msc, stop the Azure AD Application Proxy Connector service.</span></span>
2. <span data-ttu-id="4f5a2-192">Indítsa el a hálózati rögzítési.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-192">Start the network capture.</span></span>
3. <span data-ttu-id="4f5a2-193">Indítsa el az Azure AD alkalmazásproxy-összekötő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-193">Start the Azure AD Application Proxy Connector service.</span></span>
4. <span data-ttu-id="4f5a2-194">Állítsa le a hálózati rögzítést.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-194">Stop the network capture.</span></span>

   ![A "Services.msc" parancsot az Azure AD alkalmazásproxy-összekötő szolgáltatás](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-the-requests-from-the-connector-to-the-proxy-server"></a><span data-ttu-id="4f5a2-196">Tekintse meg a kérelmeket az összekötőről érkező a proxykiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="4f5a2-196">Look at the requests from the connector to the proxy server</span></span>

<span data-ttu-id="4f5a2-197">Most, hogy van hálózati rögzítőeszközt, készen áll az szűrése.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-197">Now that you’ve got a network capture, you're ready to filter it.</span></span> <span data-ttu-id="4f5a2-198">A kulcsot, a nyomkövetés megnézi a rögzítés szűrése van ismertetése.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-198">The key to looking at the trace is understanding how to filter the capture.</span></span>

<span data-ttu-id="4f5a2-199">Egy szűrő értéke (ahol a 8080-as azt a szolgáltatás proxyport):</span><span class="sxs-lookup"><span data-stu-id="4f5a2-199">One filter is as follows (where 8080 is the proxy service port):</span></span>

<span data-ttu-id="4f5a2-200">**(http-alapú. - Vagy HTTP-kérelem. Válasz) és tcp.port==8080**</span><span class="sxs-lookup"><span data-stu-id="4f5a2-200">**(http.Request or http.Response) and tcp.port==8080**</span></span>

<span data-ttu-id="4f5a2-201">Ha a szűrőt a adja meg a **szűrő megjelenítése** ablakot, és válassza ki **alkalmaz**, a rögzített forgalom megadott szűrő alapján szűri.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-201">If you enter this filter in the **Display Filter** window and select **Apply**, it filters the captured traffic based on the filter.</span></span>

<span data-ttu-id="4f5a2-202">Az előző szűrő csak a HTTP-kérések és válaszok jeleníti meg a proxykiszolgáló port és a.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-202">The preceding filter shows just the HTTP requests and responses to/from the proxy port.</span></span> <span data-ttu-id="4f5a2-203">Az összekötő indítás, ha az összekötő egy proxykiszolgáló használatára van konfigurálva a szűrő megjelenítése volna például ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-203">For a connector startup where the connector is configured to use a proxy server, the filter would show something like this:</span></span>

 ![Szűrt HTTP-kérések és válaszok példa listája](./media/application-proxy-working-with-proxy-servers/http-requests.png)

<span data-ttu-id="4f5a2-205">Mostantól kifejezetten keres a CONNECT-kérelmeket, amelyek megjelenítik a proxy-kiszolgálóval folytatott kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-205">You're now specifically looking for the CONNECT requests that show communication with the proxy server.</span></span> <span data-ttu-id="4f5a2-206">Sikeres, akkor egy HTTP-OK (200-as) választ kapott.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-206">Upon success, you get an HTTP OK (200) response.</span></span>

<span data-ttu-id="4f5a2-207">Ha megjelenik a többi válaszkódnál 407 vagy 502-es, például a proxy hitelesítést igénylő vagy nem engedélyezi a forgalom valamilyen más okból.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-207">If you see other response codes, such as 407 or 502, the proxy is requiring authentication or not allowing the traffic for some other reason.</span></span> <span data-ttu-id="4f5a2-208">Ezen a ponton bevonása, akik a proxy server támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-208">At this point, you engage your proxy server support team.</span></span>

### <a name="identify-failed-tcp-connection-attempts"></a><span data-ttu-id="4f5a2-209">Sikertelen TCP-kapcsolati kísérletek azonosítása</span><span class="sxs-lookup"><span data-stu-id="4f5a2-209">Identify failed TCP connection attempts</span></span>

<span data-ttu-id="4f5a2-210">A többi gyakori forgatókönyv, előfordulhat, hogy érdeklődik akkor, ha az összekötő közvetlenül csatlakozni próbál, de ez nem működik.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-210">The other common scenario that you may be interested in is when the connector is trying to connect directly, but it's failing.</span></span>

<span data-ttu-id="4f5a2-211">Egy másik hálózati figyelő szűrő, amellyel könnyedén azonosíthatja a probléma a következő:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-211">Another Network Monitor filter that helps you to easily identify this problem is:</span></span>

<span data-ttu-id="4f5a2-212">**tulajdonság. TCPSynRetransmit**</span><span class="sxs-lookup"><span data-stu-id="4f5a2-212">**property.TCPSynRetransmit**</span></span>

<span data-ttu-id="4f5a2-213">SZIN csomagot küldött egy TCP-kapcsolatot létesíteni az első csomag.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-213">A SYN packet is the first packet sent to establish a TCP connection.</span></span> <span data-ttu-id="4f5a2-214">Ez a csomag a válasz nem ad vissza, ha a szinkronizálás a mi reattempted van.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-214">If this packet doesn’t return a response, the SYN is reattempted.</span></span> <span data-ttu-id="4f5a2-215">Az előző szűrő segítségével bármely újraküldött SYNs láthatja.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-215">You can use the preceding filter to see any retransmitted SYNs.</span></span> <span data-ttu-id="4f5a2-216">Ezután ellenőrizheti, hogy ezek SYNs megfelelnek-e a connector kapcsolatos forgalommal.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-216">Then, you can check whether these SYNs correspond to any connector-related traffic.</span></span>

<span data-ttu-id="4f5a2-217">A következő példa bemutatja a Service Bus-portra 9352 sikertelen kapcsolódási kísérlet:</span><span class="sxs-lookup"><span data-stu-id="4f5a2-217">The following example shows a failed connection attempt to Service Bus port 9352:</span></span>

 ![Példa egy válasz sikertelen a rendszer](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

<span data-ttu-id="4f5a2-219">Ha például az előző választ, az összekötő megpróbál közvetlenül az Azure Service Bus szolgáltatással való kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-219">If you see something like the preceding response, the connector is trying to communicate directly with the Azure Service Bus service.</span></span> <span data-ttu-id="4f5a2-220">Ha várhatóan az Azure-szolgáltatásokhoz való közvetlen kapcsolat az összekötőt, ezt a választ, hogy rendelkezik-e a hálózati vagy probléma egyértelmű jelzése.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-220">If you expect the connector to make direct connections to the Azure services, this response is a clear indication that you have a network or firewall problem.</span></span>

>[!NOTE]
><span data-ttu-id="4f5a2-221">A proxykiszolgáló használatára van konfigurálva, a válasz jelentheti, hogy a Service Bus kísérel meg a közvetlen TCP-kapcsolat HTTPS-KAPCSOLATON keresztüli kapcsolatot próbált átváltás előtt.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-221">If you are configured to use a proxy server, this response might mean that Service Bus is attempting a direct TCP connection before switching to attempting a connection over HTTPS.</span></span>
>

<span data-ttu-id="4f5a2-222">A hálózati nyomkövetés elemzésének nincs mindenki számára.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-222">Network trace analysis is not for everyone.</span></span> <span data-ttu-id="4f5a2-223">De mi a helyzet a hálózattal kapcsolatos gyors adatok hasznos eszköze lehet.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-223">But it can be a valuable tool to get quick information about what's going on with your network.</span></span>

<span data-ttu-id="4f5a2-224">Ha továbbra is az összekötő csatlakozási problémák kihívást jelent, hozzon létre a jegyet a támogatási csapat.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-224">If you continue to struggle with connector connectivity issues, create a ticket with our support team.</span></span> <span data-ttu-id="4f5a2-225">A csapatunk segítségére a további hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="4f5a2-225">The team can assist you with further troubleshooting.</span></span>

<span data-ttu-id="4f5a2-226">Az alkalmazásproxy-összekötő hibák feloldására vonatkozó információkért lásd: [alkalmazásproxy hibaelhárítása](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="4f5a2-226">For information about resolving errors with Application Proxy Connector, see [Troubleshoot Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f5a2-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f5a2-227">Next steps</span></span>

[<span data-ttu-id="4f5a2-228">Az Azure AD-alkalmazásproxy összekötők ismertetése</span><span class="sxs-lookup"><span data-stu-id="4f5a2-228">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)<br><span data-ttu-id="4f5a2-229">
[Csendes telepítése az Azure AD alkalmazásproxy-összekötő](active-directory-application-proxy-silent-installation.md)</span><span class="sxs-lookup"><span data-stu-id="4f5a2-229">
[How to silently install the Azure AD Application Proxy Connector ](active-directory-application-proxy-silent-installation.md)</span></span>
