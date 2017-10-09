---
title: "a meglévő aaaWork helyi proxykiszolgálót és az Azure AD |} Microsoft Docs"
description: "Ismerteti, hogyan toowork a meglévő helyszíni proxykiszolgálókat."
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
ms.openlocfilehash: 7f8cec4f676f99bead5211bcbcf23056bd7f211f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a><span data-ttu-id="e1064-103">A meglévő helyszíni proxykiszolgálókkal működik</span><span class="sxs-lookup"><span data-stu-id="e1064-103">Work with existing on-premises proxy servers</span></span>

<span data-ttu-id="e1064-104">Ez a cikk azt ismerteti, hogyan tooconfigure Azure Active Directory (Azure AD) alkalmazásproxy összekötők toowork kimenőproxy-kiszolgálókkal.</span><span class="sxs-lookup"><span data-stu-id="e1064-104">This article explains how tooconfigure Azure Active Directory (Azure AD) Application Proxy connectors toowork with outbound proxy servers.</span></span> <span data-ttu-id="e1064-105">Az ügyfelek számára hálózati környezetekben, ahol a meglévő proxyk szolgál.</span><span class="sxs-lookup"><span data-stu-id="e1064-105">It is intended for customers with network environments that have existing proxies.</span></span>

<span data-ttu-id="e1064-106">Először fő központi telepítési forgatókönyvek alapján:</span><span class="sxs-lookup"><span data-stu-id="e1064-106">We start by looking at these main deployment scenarios:</span></span>
* <span data-ttu-id="e1064-107">Összekötők toobypass konfigurálása a helyszíni kimenő proxyk.</span><span class="sxs-lookup"><span data-stu-id="e1064-107">Configure connectors toobypass your on-premises outbound proxies.</span></span>
* <span data-ttu-id="e1064-108">Egy kimenő proxy tooaccess Azure AD alkalmazásproxy összekötők toouse konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e1064-108">Configure connectors toouse an outbound proxy tooaccess Azure AD Application Proxy.</span></span>

<span data-ttu-id="e1064-109">Összekötők működésével kapcsolatos további információkért lásd: [megértéséhez Azure AD-alkalmazásproxy összekötők](application-proxy-understand-connectors.md).</span><span class="sxs-lookup"><span data-stu-id="e1064-109">For more information about how connectors work, see [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md).</span></span>

## <a name="configure-hello-outbound-proxy"></a><span data-ttu-id="e1064-110">Hello kimenő proxy konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e1064-110">Configure hello outbound proxy</span></span>

<span data-ttu-id="e1064-111">Ha egy kimenő proxy a környezetben, a megfelelő engedélyek tooconfigure hello kimenő proxy olyan fiókot használjon.</span><span class="sxs-lookup"><span data-stu-id="e1064-111">If you have an outbound proxy in your environment, use an account with appropriate permissions tooconfigure hello outbound proxy.</span></span> <span data-ttu-id="e1064-112">Hello telepítő hello hello tesz hello telepítés a felhasználó környezetében fut, mert a Microsoft Edge vagy egy másik böngészőben ellenőrizheti hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="e1064-112">Because hello installer runs in hello context of hello user who's doing hello installation, you can check hello configuration by using Microsoft Edge or another Internet browser.</span></span>

<span data-ttu-id="e1064-113">tooconfigure hello proxybeállításai Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="e1064-113">tooconfigure hello proxy settings in Microsoft Edge:</span></span>

1. <span data-ttu-id="e1064-114">Nyissa meg túl**beállítások** > **speciális beállítások megjelenítése** > **nyissa meg a proxybeállítások** > **manuális Proxy telepítése** .</span><span class="sxs-lookup"><span data-stu-id="e1064-114">Go too**Settings** > **View Advanced Settings** > **Open Proxy Settings** > **Manual Proxy Setup**.</span></span>
2. <span data-ttu-id="e1064-115">Állítsa be **proxykiszolgálót használni** túl**a**, jelölje be hello **ne használjon hello proxykiszolgáló helyi (intranetes) címek** jelölőnégyzetet, majd a módosítás hello cím és port tooreflect a helyi proxykiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="e1064-115">Set **Use a proxy server** too**On**, select hello **Don’t use hello proxy server for local (intranet) addresses** check box, and then change hello address and port tooreflect your local proxy server.</span></span>
3. <span data-ttu-id="e1064-116">Töltse ki hello szükséges proxybeállításokat.</span><span class="sxs-lookup"><span data-stu-id="e1064-116">Fill in hello necessary proxy settings.</span></span>

   ![A proxykiszolgáló beállításai párbeszédpanel](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a><span data-ttu-id="e1064-118">Kimenő proxyk figyelmen kívül hagyása</span><span class="sxs-lookup"><span data-stu-id="e1064-118">Bypass outbound proxies</span></span>

<span data-ttu-id="e1064-119">Összekötők rendelkezik az operációs rendszer alapösszetevőt kimenő kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="e1064-119">Connectors have underlying OS components that make outbound requests.</span></span> <span data-ttu-id="e1064-120">Ezeket az összetevőket automatikusan megpróbál toolocate hello hálózat proxykiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="e1064-120">These components automatically attempt toolocate a proxy server on hello network.</span></span> <span data-ttu-id="e1064-121">Webes Proxy automatikus felderítését a lekérdezés (WPA), ha engedélyezve van a hello környezet használata.</span><span class="sxs-lookup"><span data-stu-id="e1064-121">They use Web Proxy Auto-Discovery (WPAD), if it's enabled in hello environment.</span></span>

<span data-ttu-id="e1064-122">hello operációs rendszer összetevőit toolocate proxykiszolgáló wpad.domainsuffix a DNS-címkeresést elvégzésével történt kísérlet.</span><span class="sxs-lookup"><span data-stu-id="e1064-122">hello OS components attempt toolocate a proxy server by carrying out a DNS lookup for wpad.domainsuffix.</span></span> <span data-ttu-id="e1064-123">Ha így megoldódik, a DNS-ben, egy HTTP majd kérelem toohello IP-címe wpad.dat.</span><span class="sxs-lookup"><span data-stu-id="e1064-123">If this resolves in DNS, an HTTP request is then made toohello IP address for wpad.dat.</span></span> <span data-ttu-id="e1064-124">A kérelem lesz hello proxy konfigurációs parancsprogramja a környezetben.</span><span class="sxs-lookup"><span data-stu-id="e1064-124">This request becomes hello proxy configuration script in your environment.</span></span> <span data-ttu-id="e1064-125">hello összekötő a parancsfájl tooselect egy kimenő proxykiszolgálót használ.</span><span class="sxs-lookup"><span data-stu-id="e1064-125">hello connector uses this script tooselect an outbound proxy server.</span></span> <span data-ttu-id="e1064-126">Azonban összekötő forgalom előfordulhat, hogy még nem halad át, hello proxy szükség további konfigurációs beállítások miatt.</span><span class="sxs-lookup"><span data-stu-id="e1064-126">However, connector traffic might still not go through, because of additional configuration settings needed on hello proxy.</span></span>

<span data-ttu-id="e1064-127">Konfigurálhatja a helyszíni proxy tooensure használt közvetlen kapcsolat toohello Azure hello összekötő toobypass szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e1064-127">You can configure hello connector toobypass your on-premises proxy tooensure that it uses direct connectivity toohello Azure services.</span></span> <span data-ttu-id="e1064-128">Ezt a módszert javasoljuk (Ha a hálózati házirend lehetővé teszi, hogy az), mert az azt jelenti, hogy rendelkezik-e egy kisebb konfigurációs toomaintain.</span><span class="sxs-lookup"><span data-stu-id="e1064-128">We recommend this approach (if your network policy allows for it), because it means that you have one less configuration toomaintain.</span></span>

<span data-ttu-id="e1064-129">toodisable kimenőproxy használati hello összekötő hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config fájl szerkesztésével, és adja hozzá a hello *System.NET névtérbeli* látható a fenti szakasz :</span><span class="sxs-lookup"><span data-stu-id="e1064-129">toodisable outbound proxy usage for hello connector, edit hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add hello *system.net* section shown in this code sample:</span></span>

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
<span data-ttu-id="e1064-130">tooensure, hogy hello összekötő frissítési szolgáltatást is megkerüli hello proxy, hogy hasonló módosítása toohello ApplicationProxyConnectorUpdaterService.exe.config fájl helye: C:\Program Files\Microsoft AAD App Proxy Connector Frissítőjének.</span><span class="sxs-lookup"><span data-stu-id="e1064-130">tooensure that hello Connector Updater service also bypasses hello proxy, make a similar change toohello ApplicationProxyConnectorUpdaterService.exe.config file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater.</span></span>

<span data-ttu-id="e1064-131">Hello eredeti fájlok másolatait meg arról, hogy toomake lennie, ha toorevert toohello alapértelmezett .config kiterjesztésű fájlokban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e1064-131">Be sure toomake copies of hello original files, in case you need toorevert toohello default .config files.</span></span>

## <a name="use-hello-outbound-proxy-server"></a><span data-ttu-id="e1064-132">Hello kimenő proxykiszolgáló használata</span><span class="sxs-lookup"><span data-stu-id="e1064-132">Use hello outbound proxy server</span></span>

<span data-ttu-id="e1064-133">Egyes környezetekben megkövetelése minden kimenő forgalom toogo keresztül kimenő, kivételhiba jelentkezése nélkül.</span><span class="sxs-lookup"><span data-stu-id="e1064-133">Some environments require all outbound traffic toogo through an outbound proxy, without exception.</span></span> <span data-ttu-id="e1064-134">Ennek eredményeképpen hello proxy megkerülése lehetőség nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="e1064-134">As a result, bypassing hello proxy is not an option.</span></span>

<span data-ttu-id="e1064-135">Hello összekötő forgalom toogo hello kimenő proxyn keresztül is konfigurálhatja, ahogy az ábra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="e1064-135">You can configure hello connector traffic toogo through hello outbound proxy, as shown in hello following diagram:</span></span>

 ![Konfigurálása az összekötő forgalom toogo keresztül egy kimenő proxy tooAzure AD alkalmazásproxy](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

<span data-ttu-id="e1064-137">Ennek eredményeképpen, hogy csak a kimenő forgalom, nincs nincs szükség tooconfigure hozzáférés a tűzfalon keresztüli bejövő.</span><span class="sxs-lookup"><span data-stu-id="e1064-137">As a result of having only outbound traffic, there's no need tooconfigure inbound access through your firewalls.</span></span>

### <a name="step-1-configure-hello-connector-and-related-services-toogo-through-hello-outbound-proxy"></a><span data-ttu-id="e1064-138">1. lépés: Hello-összekötő konfigurálása, és a kapcsolódó szolgáltatások toogo hello kimenő proxyn keresztül</span><span class="sxs-lookup"><span data-stu-id="e1064-138">Step 1: Configure hello connector and related services toogo through hello outbound proxy</span></span>

<span data-ttu-id="e1064-139">Vonatkozik korábban, ha a WPAD hello környezetben engedélyezve van, és megfelelő konfigurálása, mint a hello connector automatikusan észleli az hello kimenőproxy kiszolgáló, és megpróbál toouse azt.</span><span class="sxs-lookup"><span data-stu-id="e1064-139">As covered earlier, if WPAD is enabled in hello environment and configured appropriately, hello connector will automatically discover hello outbound proxy server and attempt toouse it.</span></span> <span data-ttu-id="e1064-140">Azonban explicit módon konfigurálhatja hello összekötő toogo egy kimenő proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="e1064-140">However, you can explicitly configure hello connector toogo through an outbound proxy.</span></span>

<span data-ttu-id="e1064-141">toodo tehát hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config fájl szerkesztésével, és adja hozzá a hello *System.NET névtérbeli* látható a fenti szakaszban.</span><span class="sxs-lookup"><span data-stu-id="e1064-141">toodo so, edit hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config file and add hello *system.net* section shown in this code sample.</span></span> <span data-ttu-id="e1064-142">Változás *proxyserver:8080* a helyi proxykiszolgáló nevét vagy IP-cím és hello porton figyeli a tooreflect.</span><span class="sxs-lookup"><span data-stu-id="e1064-142">Change *proxyserver:8080* tooreflect your local proxy server name or IP address, and hello port that it's listening on.</span></span>

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

<span data-ttu-id="e1064-143">Ezután konfigurálja a hello összekötő Frissítőjének toouse hello szolgáltatásproxy azáltal, hogy a hasonló módosítása toohello fájl helye: C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span><span class="sxs-lookup"><span data-stu-id="e1064-143">Next, configure hello Connector Updater service toouse hello proxy by making a similar change toohello file located at C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.</span></span>

### <a name="step-2-configure-hello-proxy-tooallow-traffic-from-hello-connector-and-related-services-tooflow-through"></a><span data-ttu-id="e1064-144">2. lépés: Hello proxy tooallow forgalom hello összekötő és a kapcsolódó szolgáltatások tooflow keresztül konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e1064-144">Step 2: Configure hello proxy tooallow traffic from hello connector and related services tooflow through</span></span>

<span data-ttu-id="e1064-145">Jelenleg hello kimenőproxy négy szempontok tooconsider:</span><span class="sxs-lookup"><span data-stu-id="e1064-145">There are four aspects tooconsider at hello outbound proxy:</span></span>
* <span data-ttu-id="e1064-146">Proxy kimenő szabályok</span><span class="sxs-lookup"><span data-stu-id="e1064-146">Proxy outbound rules</span></span>
* <span data-ttu-id="e1064-147">A proxy hitelesítése</span><span class="sxs-lookup"><span data-stu-id="e1064-147">Proxy authentication</span></span>
* <span data-ttu-id="e1064-148">Proxy portok</span><span class="sxs-lookup"><span data-stu-id="e1064-148">Proxy ports</span></span>
* <span data-ttu-id="e1064-149">SSL-ellenőrzést</span><span class="sxs-lookup"><span data-stu-id="e1064-149">SSL inspection</span></span>

#### <a name="proxy-outbound-rules"></a><span data-ttu-id="e1064-150">Proxy kimenő szabályok</span><span class="sxs-lookup"><span data-stu-id="e1064-150">Proxy outbound rules</span></span>
<span data-ttu-id="e1064-151">A következő hozzáférést összekötő-szolgáltatás végpontjainak hozzáférés toohello engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="e1064-151">Allow access toohello following endpoints for connector service access:</span></span>

* <span data-ttu-id="e1064-152">*. msappproxy.net</span><span class="sxs-lookup"><span data-stu-id="e1064-152">*.msappproxy.net</span></span>
* <span data-ttu-id="e1064-153">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="e1064-153">*.servicebus.windows.net</span></span>

<span data-ttu-id="e1064-154">A kezdeti regisztráció a következő végpontok hozzáférés toohello engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="e1064-154">For initial registration, allow access toohello following endpoints:</span></span>

* <span data-ttu-id="e1064-155">login.windows.net</span><span class="sxs-lookup"><span data-stu-id="e1064-155">login.windows.net</span></span>
* <span data-ttu-id="e1064-156">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="e1064-156">login.microsoftonline.com</span></span>

<span data-ttu-id="e1064-157">Ha nem engedélyezi a kapcsolatot a teljes tartománynév alapján, és kell toospecify IP-címtartományok Ehelyett használja az alábbi beállításokat:</span><span class="sxs-lookup"><span data-stu-id="e1064-157">If you can't allow connectivity by FQDN and need toospecify IP ranges instead, use these options:</span></span>

* <span data-ttu-id="e1064-158">Hello összekötő kimenő hozzá lehessen férni tooall célhelyre.</span><span class="sxs-lookup"><span data-stu-id="e1064-158">Allow hello connector outbound access tooall destinations.</span></span>
* <span data-ttu-id="e1064-159">Hozzáférést hello összekötő kimenő túl[Azure datacenter IP-címtartományok](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="e1064-159">Allow hello connector outbound access too[Azure datacenter IP ranges](https://www.microsoft.com/en-gb/download/details.aspx?id=41653).</span></span> <span data-ttu-id="e1064-160">hello challenge hello Azure datacenter IP-címtartománylista használatával, hogy hetente kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="e1064-160">hello challenge with using hello list of Azure datacenter IP ranges is that it's updated weekly.</span></span> <span data-ttu-id="e1064-161">Egy folyamatot, hogy a hozzáférési szabályok megfelelően vannak-e frissíti hely tooensure tooput van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e1064-161">You need tooput a process in place tooensure that your access rules are updated accordingly.</span></span>

#### <a name="proxy-authentication"></a><span data-ttu-id="e1064-162">A proxy hitelesítése</span><span class="sxs-lookup"><span data-stu-id="e1064-162">Proxy authentication</span></span>

<span data-ttu-id="e1064-163">A proxy hitelesítése jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e1064-163">Proxy authentication is not currently supported.</span></span> <span data-ttu-id="e1064-164">Az aktuális ajánlott megoldás tooallow hello összekötő névtelen hozzáférés toohello Internet célok.</span><span class="sxs-lookup"><span data-stu-id="e1064-164">Our current recommendation is tooallow hello connector anonymous access toohello Internet destinations.</span></span>

#### <a name="proxy-ports"></a><span data-ttu-id="e1064-165">Proxy portok</span><span class="sxs-lookup"><span data-stu-id="e1064-165">Proxy ports</span></span>

<span data-ttu-id="e1064-166">hello összekötő lehetővé teszi a kimenő SSL-alapú kapcsolatok hello KAPCSOLATI módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="e1064-166">hello connector makes outbound SSL-based connections by using hello CONNECT method.</span></span> <span data-ttu-id="e1064-167">Ez a módszer lényegében állít be egy alagúton hello kimenő proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="e1064-167">This method essentially sets up a tunnel through hello outbound proxy.</span></span> <span data-ttu-id="e1064-168">Hello proxy server tooallow tooports 443-as és a 80-as bújtatás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e1064-168">Configure hello proxy server tooallow tunneling tooports 443 and 80.</span></span>

>[!NOTE]
><span data-ttu-id="e1064-169">A Service Bus a HTTPS-KAPCSOLATON keresztül futtatása 443-as portot használja.</span><span class="sxs-lookup"><span data-stu-id="e1064-169">When Service Bus runs over HTTPS, it uses port 443.</span></span> <span data-ttu-id="e1064-170">Azonban az alapértelmezés szerint a Service Bus kísérletet tett a közvetlen TCP-kapcsolatok, és visszaáll tooHTTPS csak akkor, ha közvetlen kapcsolatot sikertelen.</span><span class="sxs-lookup"><span data-stu-id="e1064-170">However, by default, Service Bus attempts direct TCP connections and falls back tooHTTPS only if direct connectivity fails.</span></span>

<span data-ttu-id="e1064-171">Service Bus forgalom is zajlik hello kimenő proxykiszolgálón keresztül hello, győződjön meg arról, hogy hello összekötő közvetlenül nem lehet kapcsolódni a toohello tooensure Azure szolgáltatások 9350 9352 és 5671 portok.</span><span class="sxs-lookup"><span data-stu-id="e1064-171">tooensure that hello Service Bus traffic is also sent through hello outbound proxy server, ensure that hello connector cannot directly connect toohello Azure services for ports 9350, 9352, and 5671.</span></span>

#### <a name="ssl-inspection"></a><span data-ttu-id="e1064-172">SSL-ellenőrzést</span><span class="sxs-lookup"><span data-stu-id="e1064-172">SSL inspection</span></span>
<span data-ttu-id="e1064-173">Ne használjon SSL-ellenőrzést hello összekötő forgalmat, mert hello összekötő forgalom problémákat okoz.</span><span class="sxs-lookup"><span data-stu-id="e1064-173">Do not use SSL inspection for hello connector traffic, because it causes problems for hello connector traffic.</span></span>

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a><span data-ttu-id="e1064-174">Összekötő proxy problémák és a szolgáltatás kapcsolódási problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="e1064-174">Troubleshoot connector proxy problems and service connectivity issues</span></span>
<span data-ttu-id="e1064-175">Most már megtekintheti az összes forgalom hello proxyn keresztül történő továbbítására.</span><span class="sxs-lookup"><span data-stu-id="e1064-175">Now you should see all traffic flowing through hello proxy.</span></span> <span data-ttu-id="e1064-176">Ha problémába ütközik, a következő hibaelhárítási információk hello segítségül szolgálhat.</span><span class="sxs-lookup"><span data-stu-id="e1064-176">If you have problems, hello following troubleshooting information should help.</span></span>

<span data-ttu-id="e1064-177">legjobb módja tooidentify hello és összekötő kapcsolatok problémák tootake hello összekötő szolgáltatás indítása közben hello összekötő szolgáltatás egy hálózati adatváltozások rögzítése.</span><span class="sxs-lookup"><span data-stu-id="e1064-177">hello best way tooidentify and troubleshoot connector connectivity issues is tootake a network capture on hello connector service while starting hello connector service.</span></span> <span data-ttu-id="e1064-178">Ez nagyobb terhet jelenthetnek, gyors tippek az rögzítésével és hálózati nyomkövetés szűréshez tehát vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="e1064-178">This can be a daunting task, so let’s look at quick tips on capturing and filtering network traces.</span></span>

<span data-ttu-id="e1064-179">A kiválasztott eszköz figyelési hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e1064-179">You can use hello monitoring tool of your choice.</span></span> <span data-ttu-id="e1064-180">Ez a cikk hello célokra Microsoft Network Monitor 3.4 használtuk.</span><span class="sxs-lookup"><span data-stu-id="e1064-180">For hello purposes of this article, we used Microsoft Network Monitor 3.4.</span></span> <span data-ttu-id="e1064-181">Is [töltse le a Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span><span class="sxs-lookup"><span data-stu-id="e1064-181">You can [download it from Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).</span></span>

<span data-ttu-id="e1064-182">hello példák és szűrőket, amelyek a következő részekben hello használjuk adott tooNetwork figyelő, de hello alapelvek lehet alkalmazott tooany elemző eszközt.</span><span class="sxs-lookup"><span data-stu-id="e1064-182">hello examples and filters that we use in hello following sections are specific tooNetwork Monitor, but hello principles can be applied tooany analysis tool.</span></span>

### <a name="take-a-capture-by-using-network-monitor"></a><span data-ttu-id="e1064-183">Tegye meg a rögzítés a Hálózatfigyelővel</span><span class="sxs-lookup"><span data-stu-id="e1064-183">Take a capture by using Network Monitor</span></span>

<span data-ttu-id="e1064-184">a rögzítési toostart:</span><span class="sxs-lookup"><span data-stu-id="e1064-184">toostart a capture:</span></span>

1. <span data-ttu-id="e1064-185">Nyissa meg a hálózati figyelő, és kattintson a **új rögzítése**.</span><span class="sxs-lookup"><span data-stu-id="e1064-185">Open Network Monitor and click **New Capture**.</span></span>
2. <span data-ttu-id="e1064-186">Kattintson a hello **Start** gombra.</span><span class="sxs-lookup"><span data-stu-id="e1064-186">Click hello **Start** button.</span></span>

   ![Hálózati figyelő ablak](./media/application-proxy-working-with-proxy-servers/network-capture.png)

<span data-ttu-id="e1064-188">A rögzítési befejezése után kattintson a hello **leállítása** gomb tooend azt.</span><span class="sxs-lookup"><span data-stu-id="e1064-188">After you complete a capture, click hello **Stop** button tooend it.</span></span>

### <a name="take-a-capture-of-connector-traffic"></a><span data-ttu-id="e1064-189">Az összekötő forgalom rögzítés igénybe</span><span class="sxs-lookup"><span data-stu-id="e1064-189">Take a capture of connector traffic</span></span>

<span data-ttu-id="e1064-190">A kezdeti hibaelhárítási, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="e1064-190">For initial troubleshooting, perform hello following steps:</span></span>

1. <span data-ttu-id="e1064-191">A "Services.msc" parancsot hello Azure AD alkalmazásproxy-összekötő szolgáltatás leállítása.</span><span class="sxs-lookup"><span data-stu-id="e1064-191">From services.msc, stop hello Azure AD Application Proxy Connector service.</span></span>
2. <span data-ttu-id="e1064-192">Indítsa el a hello hálózati rögzítési.</span><span class="sxs-lookup"><span data-stu-id="e1064-192">Start hello network capture.</span></span>
3. <span data-ttu-id="e1064-193">Hello Azure AD alkalmazásproxy-összekötő szolgáltatás elindítása.</span><span class="sxs-lookup"><span data-stu-id="e1064-193">Start hello Azure AD Application Proxy Connector service.</span></span>
4. <span data-ttu-id="e1064-194">Állítsa le a hello hálózati rögzítést.</span><span class="sxs-lookup"><span data-stu-id="e1064-194">Stop hello network capture.</span></span>

   ![A "Services.msc" parancsot az Azure AD alkalmazásproxy-összekötő szolgáltatás](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-hello-requests-from-hello-connector-toohello-proxy-server"></a><span data-ttu-id="e1064-196">Nézze meg hello kérelmek hello összekötő toohello proxykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="e1064-196">Look at hello requests from hello connector toohello proxy server</span></span>

<span data-ttu-id="e1064-197">Most, hogy van hálózati rögzítőeszközt, most készen áll a toofilter azt.</span><span class="sxs-lookup"><span data-stu-id="e1064-197">Now that you’ve got a network capture, you're ready toofilter it.</span></span> <span data-ttu-id="e1064-198">hello kulcs toolooking: hello nyomkövetési van megértése, hogyan toofilter hello rögzítése.</span><span class="sxs-lookup"><span data-stu-id="e1064-198">hello key toolooking at hello trace is understanding how toofilter hello capture.</span></span>

<span data-ttu-id="e1064-199">Egy szűrő értéke (ahol a 8080-as azt hello szolgáltatás proxyport):</span><span class="sxs-lookup"><span data-stu-id="e1064-199">One filter is as follows (where 8080 is hello proxy service port):</span></span>

<span data-ttu-id="e1064-200">**(http-alapú. - Vagy HTTP-kérelem. Válasz) és tcp.port==8080**</span><span class="sxs-lookup"><span data-stu-id="e1064-200">**(http.Request or http.Response) and tcp.port==8080**</span></span>

<span data-ttu-id="e1064-201">Ha ez a szűrő ír hello **szűrő megjelenítése** ablakot, és válassza ki **alkalmaz**, rögzített hello forgalom hello szűrő alapján szűri.</span><span class="sxs-lookup"><span data-stu-id="e1064-201">If you enter this filter in hello **Display Filter** window and select **Apply**, it filters hello captured traffic based on hello filter.</span></span>

<span data-ttu-id="e1064-202">hello előző szűrő látható az ebben az esetben hello HTTP-kérések és válaszok hello proxy portja és a.</span><span class="sxs-lookup"><span data-stu-id="e1064-202">hello preceding filter shows just hello HTTP requests and responses to/from hello proxy port.</span></span> <span data-ttu-id="e1064-203">Hello összekötő esetén konfigurált toouse proxykiszolgálón connector indítási, az alábbihoz hasonló hello szűrő volna megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="e1064-203">For a connector startup where hello connector is configured toouse a proxy server, hello filter would show something like this:</span></span>

 ![Szűrt HTTP-kérések és válaszok példa listája](./media/application-proxy-working-with-proxy-servers/http-requests.png)

<span data-ttu-id="e1064-205">Mostantól kifejezetten keresett hello csatlakozás igényel, amely megjelenítése hello proxy-kiszolgálóval folytatott kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="e1064-205">You're now specifically looking for hello CONNECT requests that show communication with hello proxy server.</span></span> <span data-ttu-id="e1064-206">Sikeres, akkor egy HTTP-OK (200-as) választ kapott.</span><span class="sxs-lookup"><span data-stu-id="e1064-206">Upon success, you get an HTTP OK (200) response.</span></span>

<span data-ttu-id="e1064-207">Ha megjelenik a többi válaszkódnál, mint a 407 vagy 502-es, hello proxy hitelesítést igénylő vagy nem így bármilyen más okból hello forgalmat.</span><span class="sxs-lookup"><span data-stu-id="e1064-207">If you see other response codes, such as 407 or 502, hello proxy is requiring authentication or not allowing hello traffic for some other reason.</span></span> <span data-ttu-id="e1064-208">Ezen a ponton bevonása, akik a proxy server támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="e1064-208">At this point, you engage your proxy server support team.</span></span>

### <a name="identify-failed-tcp-connection-attempts"></a><span data-ttu-id="e1064-209">Sikertelen TCP-kapcsolati kísérletek azonosítása</span><span class="sxs-lookup"><span data-stu-id="e1064-209">Identify failed TCP connection attempts</span></span>

<span data-ttu-id="e1064-210">hello egyéb gyakori forgatókönyv, előfordulhat, hogy érdeklődik esetén hello összekötő próbál tooconnect közvetlenül, de ez nem működik.</span><span class="sxs-lookup"><span data-stu-id="e1064-210">hello other common scenario that you may be interested in is when hello connector is trying tooconnect directly, but it's failing.</span></span>

<span data-ttu-id="e1064-211">Van egy másik hálózati figyelő szűrő, amely segít tooeasily, ez a probléma meghatározásához:</span><span class="sxs-lookup"><span data-stu-id="e1064-211">Another Network Monitor filter that helps you tooeasily identify this problem is:</span></span>

<span data-ttu-id="e1064-212">**tulajdonság. TCPSynRetransmit**</span><span class="sxs-lookup"><span data-stu-id="e1064-212">**property.TCPSynRetransmit**</span></span>

<span data-ttu-id="e1064-213">A szinkronizálás a mi csomag érték hello első csomag tooestablish TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="e1064-213">A SYN packet is hello first packet sent tooestablish a TCP connection.</span></span> <span data-ttu-id="e1064-214">Ha a csomag nem ad vissza a választ, hello SZIN reattempted van.</span><span class="sxs-lookup"><span data-stu-id="e1064-214">If this packet doesn’t return a response, hello SYN is reattempted.</span></span> <span data-ttu-id="e1064-215">Szűrő toosee megelőző hello bármely újraküldött SYNs használható.</span><span class="sxs-lookup"><span data-stu-id="e1064-215">You can use hello preceding filter toosee any retransmitted SYNs.</span></span> <span data-ttu-id="e1064-216">Ezután ellenőrizheti, hogy ezek SYNs megfelelnek-e tooany összekötő kapcsolatos forgalom.</span><span class="sxs-lookup"><span data-stu-id="e1064-216">Then, you can check whether these SYNs correspond tooany connector-related traffic.</span></span>

<span data-ttu-id="e1064-217">hello alábbi példa bemutatja a sikertelen kapcsolódás tooService Bus port 9352:</span><span class="sxs-lookup"><span data-stu-id="e1064-217">hello following example shows a failed connection attempt tooService Bus port 9352:</span></span>

 ![Példa egy válasz sikertelen a rendszer](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

<span data-ttu-id="e1064-219">Ha például a válasz megelőző hello, hello összekötő megpróbál közvetlenül az Azure Service Bus szolgáltatás hello toocommunicate.</span><span class="sxs-lookup"><span data-stu-id="e1064-219">If you see something like hello preceding response, hello connector is trying toocommunicate directly with hello Azure Service Bus service.</span></span> <span data-ttu-id="e1064-220">Ha a várt hello összekötő toomake közvetlen kapcsolatokon keresztül toohello Azure szolgáltatások, ezt a választ az, hogy rendelkezik-e a hálózati vagy probléma egyértelmű jelzése.</span><span class="sxs-lookup"><span data-stu-id="e1064-220">If you expect hello connector toomake direct connections toohello Azure services, this response is a clear indication that you have a network or firewall problem.</span></span>

>[!NOTE]
><span data-ttu-id="e1064-221">Ha konfigurált toouse proxykiszolgáló, ez a válasz jelentheti, hogy a Service Bus kísérel meg előtt váltás tooattempting a kapcsolat HTTPS-KAPCSOLATON keresztül a közvetlen TCP-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="e1064-221">If you are configured toouse a proxy server, this response might mean that Service Bus is attempting a direct TCP connection before switching tooattempting a connection over HTTPS.</span></span>
>

<span data-ttu-id="e1064-222">A hálózati nyomkövetés elemzésének nincs mindenki számára.</span><span class="sxs-lookup"><span data-stu-id="e1064-222">Network trace analysis is not for everyone.</span></span> <span data-ttu-id="e1064-223">Azonban a legértékesebb eszköz tooget gyors információ a hálózati jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="e1064-223">But it can be a valuable tool tooget quick information about what's going on with your network.</span></span>

<span data-ttu-id="e1064-224">Ha Ön most folytatja, az összekötő csatlakozási problémák toostruggle, hozzon létre egy jegyet a támogatási csapat.</span><span class="sxs-lookup"><span data-stu-id="e1064-224">If you continue toostruggle with connector connectivity issues, create a ticket with our support team.</span></span> <span data-ttu-id="e1064-225">hello csapatunk segítségére a további hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="e1064-225">hello team can assist you with further troubleshooting.</span></span>

<span data-ttu-id="e1064-226">Az alkalmazásproxy-összekötő hibák feloldására vonatkozó információkért lásd: [alkalmazásproxy hibaelhárítása](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e1064-226">For information about resolving errors with Application Proxy Connector, see [Troubleshoot Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1064-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1064-227">Next steps</span></span>

[<span data-ttu-id="e1064-228">Az Azure AD-alkalmazásproxy összekötők ismertetése</span><span class="sxs-lookup"><span data-stu-id="e1064-228">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)<br><span data-ttu-id="e1064-229">
[Hogyan a toosilently hello Azure AD alkalmazásproxy-összekötő telepítése](active-directory-application-proxy-silent-installation.md)</span><span class="sxs-lookup"><span data-stu-id="e1064-229">
[How toosilently install hello Azure AD Application Proxy Connector ](active-directory-application-proxy-silent-installation.md)</span></span>
