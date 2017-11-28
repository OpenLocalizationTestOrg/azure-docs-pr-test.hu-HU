---
title: "aaaConfigure egy webalkalmazást az Azure App Service szolgáltatásban a Traffic Manager a(z) terheléselosztást használó egyéni tartomány nevét."
description: "Adjon nevet az egyéni tartomány egy egy webalkalmazást az Azure App Service szolgáltatásban, amely tartalmazza a Traffic Manager a(z) terheléselosztást."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: dfde5fc6b445b30b10e03dcb03e8d072130d9377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a><span data-ttu-id="f7fb6-103">A webalkalmazáshoz tartozó egyéni tartománynév konfigurálása az Azure App Service szolgáltatásban a Traffic Managerrel</span><span class="sxs-lookup"><span data-stu-id="f7fb6-103">Configuring a custom domain name for a web app in Azure App Service using Traffic Manager</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

<span data-ttu-id="f7fb6-104">Ez a cikk általános útmutatást nyújt az Azure App Service egy egyéni tartománynév használatával, amely a Traffic Manager használata terheléselosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-104">This article provides generic instructions for using a custom domain name with Azure App Service that use Traffic Manager for load balancing.</span></span>

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="f7fb6-105">DNS-rekordok ismertetése</span><span class="sxs-lookup"><span data-stu-id="f7fb6-105">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a><span data-ttu-id="f7fb6-106">A szabványos mód a webalkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f7fb6-106">Configure your web apps for standard mode</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="f7fb6-107">Az egyéni tartomány DNS-rekord hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f7fb6-107">Add a DNS record for your custom domain</span></span>
> [!NOTE]
> <span data-ttu-id="f7fb6-108">Ha tartományi Azure App Service Web Apps keresztül vásárolt, majd ugorjon a következő lépéseket és az utolsó lépésében toohello hivatkozik [tartomány vásárlása Web Apps](custom-dns-web-site-buydomains-web-app.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-108">If you have purchased domain through Azure App Service Web Apps then skip following steps and refer toohello final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md) article.</span></span>
> 
> 

<span data-ttu-id="f7fb6-109">tooassociate egy webalkalmazást az Azure App Service szolgáltatásban az egyéni tartományt, hozzá kell adnia egy új bejegyzést hello DNS-tábla az egyéni tartomány hello tartományregisztráló a tartománynév, a megvásárolt által biztosított eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-109">tooassociate your custom domain with a web app in Azure App Service, you must add a new entry in hello DNS table for your custom domain by using tools provided by hello domain registrar that you purchased your domain name from.</span></span> <span data-ttu-id="f7fb6-110">A következő lépéseket toolocate hello használja, és hello DNS-eszközök használata.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-110">Use hello following steps toolocate and use hello DNS tools.</span></span>

1. <span data-ttu-id="f7fb6-111">Jelentkezzen be a tartományregisztrálónál tooyour fiókot, és keresse meg a DNS-rekordok kezelése lapon.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-111">Sign in tooyour account at your domain registrar, and look for a page for managing DNS records.</span></span> <span data-ttu-id="f7fb6-112">Keressen hivatkozásokat vagy hello hely területek címkézve **tartománynév**, **DNS**, vagy **neve kezelő**.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-112">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="f7fb6-113">Gyakran toothis lapján található hivatkozás lehet a fiók adatainak megtekintésekor, majd egy hivatkozást, mint **a tartományok**.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-113">Often a link toothis page can be found be viewing your account information, and then looking for a link such as **My domains**.</span></span>
2. <span data-ttu-id="f7fb6-114">A tartománynév talált hello kezelés lapon, keresse meg a hivatkozást, amellyel lehetővé teszi a tooedit hello DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-114">Once you have found hello management page for your domain name, look for a link that allows you tooedit hello DNS records.</span></span> <span data-ttu-id="f7fb6-115">Ez lehet, hogy legyen felsorolva egy **zónafájl**, **DNS-rekordok**, vagy mint egy **speciális** konfigurációs hivatkozására.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-115">This might be listed as a **Zone file**, **DNS Records**, or as an **Advanced** configuration link.</span></span>
   
   * <span data-ttu-id="f7fb6-116">hello lap valószínűleg fog rendelkezni a már létrehozott, például egy bejegyzés társítása néhány rekord "**@**"vagy"\*" "tartomány várakozást" lapot.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-116">hello page will most likely have a few records already created, such as an entry associating '**@**' or '\*' with a 'domain parking' page.</span></span> <span data-ttu-id="f7fb6-117">Például közös altartományok rekordjait is tartalmazhat **www**.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-117">It may also contain records for common sub-domains such as **www**.</span></span>
   * <span data-ttu-id="f7fb6-118">hello lapon fog említse meg **CNAME rekordok**, vagy adjon meg egy legördülő tooselect az típusát.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-118">hello page will mention **CNAME records**, or provide a drop-down tooselect a record type.</span></span> <span data-ttu-id="f7fb6-119">Azt is tüntethető fel más bejegyzések például **A rekordok** és **MX-rekordok**.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-119">It may also mention other records such as **A records** and **MX records**.</span></span> <span data-ttu-id="f7fb6-120">Bizonyos esetekben a CNAME-rekordok fogja meghívni az egyéb, mint egy **aliasrekordot**.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-120">In some cases, CNAME records will be called by other names such as an **Alias Record**.</span></span>
   * <span data-ttu-id="f7fb6-121">hello lap is, amelyek lehetővé teszik túl mezők**térkép** a egy **állomásnév** vagy **tartománynév** tooanother tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-121">hello page will also have fields that allow you too**map** from a **Host name** or **Domain name** tooanother domain name.</span></span>
3. <span data-ttu-id="f7fb6-122">Minden tartományregisztráló hello mintaadatokról eltérők lehetnek, amíg általában leképez *a* az egyéni tartománynevet (például **contoso.com**,) *való* hello Traffic Manager szolgáltatásbeli tartománynevére (**contoso.trafficmanager.net**) használt a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-122">While hello specifics of each registrar vary, in general you map *from* your custom domain name (such as **contoso.com**,) *to* hello Traffic Manager domain name (**contoso.trafficmanager.net**) that is used for your web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f7fb6-123">Másik lehetőségként Ha a bejegyzést már használatban van, és toopreemptively kell kötni az alkalmazások tooit, létrehozhat egy további CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-123">Alternatively, if a record is already in use and you need toopreemptively bind your apps tooit, you can create an additional CNAME record.</span></span> <span data-ttu-id="f7fb6-124">Például toopreemptively bind **www.contoso.com** tooyour webes alkalmazás, hozzon létre egy CNAME rekordot a **awverify.www** túl**contoso.trafficmanager.net**.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-124">For example, toopreemptively bind **www.contoso.com** tooyour web app, create a CNAME record from **awverify.www** too**contoso.trafficmanager.net**.</span></span> <span data-ttu-id="f7fb6-125">Felveheti a "www.contoso.com" tooyour webalkalmazás hello "www" CNAME rekordot a módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-125">You can then add "www.contoso.com" tooyour Web App without changing hello "www" CNAME record.</span></span> <span data-ttu-id="f7fb6-126">További információkért lásd: [hozzon létre DNS-rekordok az egyéni tartomány webalkalmazáshoz][CREATEDNS].</span><span class="sxs-lookup"><span data-stu-id="f7fb6-126">For more information, see [Create DNS records for a web app in a custom domain][CREATEDNS].</span></span>
   > 
   > 
4. <span data-ttu-id="f7fb6-127">Miután befejezte a hozzáadása vagy módosítása a regisztráló DNS-rekordokat, hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="f7fb6-127">Once you have finished adding or modifying DNS records at your registrar, save hello changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a><span data-ttu-id="f7fb6-128">A Traffic Manager engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f7fb6-128">Enable Traffic Manager</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a><span data-ttu-id="f7fb6-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f7fb6-129">Next steps</span></span>
<span data-ttu-id="f7fb6-130">További információkért lásd: hello [Node.js fejlesztői központ](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="f7fb6-130">For more information, see hello [Node.js Developer Center](/develop/nodejs/).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
