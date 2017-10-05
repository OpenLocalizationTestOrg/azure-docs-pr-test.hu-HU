---
title: "A webalkalmazás egyéni tartománynév beállítása az Azure App Service által használt Traffic Manager terheléselosztás."
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
ms.openlocfilehash: 5f099201d9018a6f8577cb3daf127d09560fb94b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a><span data-ttu-id="46ccc-103">A webalkalmazáshoz tartozó egyéni tartománynév konfigurálása az Azure App Service szolgáltatásban a Traffic Managerrel</span><span class="sxs-lookup"><span data-stu-id="46ccc-103">Configuring a custom domain name for a web app in Azure App Service using Traffic Manager</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

<span data-ttu-id="46ccc-104">Ez a cikk általános útmutatást nyújt az Azure App Service egy egyéni tartománynév használatával, amely a Traffic Manager használata terheléselosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="46ccc-104">This article provides generic instructions for using a custom domain name with Azure App Service that use Traffic Manager for load balancing.</span></span>

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="46ccc-105">DNS-rekordok ismertetése</span><span class="sxs-lookup"><span data-stu-id="46ccc-105">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a><span data-ttu-id="46ccc-106">A szabványos mód a webalkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="46ccc-106">Configure your web apps for standard mode</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="46ccc-107">Az egyéni tartomány DNS-rekord hozzáadása</span><span class="sxs-lookup"><span data-stu-id="46ccc-107">Add a DNS record for your custom domain</span></span>
> [!NOTE]
> <span data-ttu-id="46ccc-108">Ha tartományi Azure App Service Web Apps keresztül vásárolt, majd ugorjon a következő lépéseket és az utolsó lépésében hivatkozik [tartomány vásárlása Web Apps](custom-dns-web-site-buydomains-web-app.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="46ccc-108">If you have purchased domain through Azure App Service Web Apps then skip following steps and refer to the final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md) article.</span></span>
> 
> 

<span data-ttu-id="46ccc-109">Az egyéni tartomány társítandó egy webalkalmazást az Azure App Service-ben, hozzá kell adnia egy új bejegyzést a DNS-tábla az egyéni tartomány a tartományregisztráló a tartománynév, a megvásárolt által biztosított eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="46ccc-109">To associate your custom domain with a web app in Azure App Service, you must add a new entry in the DNS table for your custom domain by using tools provided by the domain registrar that you purchased your domain name from.</span></span> <span data-ttu-id="46ccc-110">Az alábbi lépések segítségével keresse meg és a DNS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="46ccc-110">Use the following steps to locate and use the DNS tools.</span></span>

1. <span data-ttu-id="46ccc-111">Jelentkezzen be fiókjába a tartományregisztrálónál, és keresse meg a DNS-rekordok kezelése lapján.</span><span class="sxs-lookup"><span data-stu-id="46ccc-111">Sign in to your account at your domain registrar, and look for a page for managing DNS records.</span></span> <span data-ttu-id="46ccc-112">Keressen hivatkozásokat vagy a hely címkézve területéhez **tartománynév**, **DNS**, vagy **neve kezelő**.</span><span class="sxs-lookup"><span data-stu-id="46ccc-112">Look for links or areas of the site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="46ccc-113">Ezen a lapon egy hivatkozást található gyakran kell a fiók adatainak megtekintésekor, majd egy hivatkozást, mint **a tartományok**.</span><span class="sxs-lookup"><span data-stu-id="46ccc-113">Often a link to this page can be found be viewing your account information, and then looking for a link such as **My domains**.</span></span>
2. <span data-ttu-id="46ccc-114">Miután megtalálta a felügyelet lapon a tartománynév, keresse meg a hivatkozást, amellyel lehetővé teszi a DNS-rekordjainak szerkesztését.</span><span class="sxs-lookup"><span data-stu-id="46ccc-114">Once you have found the management page for your domain name, look for a link that allows you to edit the DNS records.</span></span> <span data-ttu-id="46ccc-115">Ez lehet, hogy legyen felsorolva egy **zónafájl**, **DNS-rekordok**, vagy mint egy **speciális** konfigurációs hivatkozására.</span><span class="sxs-lookup"><span data-stu-id="46ccc-115">This might be listed as a **Zone file**, **DNS Records**, or as an **Advanced** configuration link.</span></span>
   
   * <span data-ttu-id="46ccc-116">Az oldalon nagy valószínűséggel kell néhány rekord már létrehozott, például egy bejegyzés társítása "**@**"vagy"\*" "tartomány várakozást" lapot.</span><span class="sxs-lookup"><span data-stu-id="46ccc-116">The page will most likely have a few records already created, such as an entry associating '**@**' or '\*' with a 'domain parking' page.</span></span> <span data-ttu-id="46ccc-117">Például közös altartományok rekordjait is tartalmazhat **www**.</span><span class="sxs-lookup"><span data-stu-id="46ccc-117">It may also contain records for common sub-domains such as **www**.</span></span>
   * <span data-ttu-id="46ccc-118">A lap fog említse meg **CNAME rekordok**, vagy adjon meg egy legördülő listán válasszon egy rekordtípust.</span><span class="sxs-lookup"><span data-stu-id="46ccc-118">The page will mention **CNAME records**, or provide a drop-down to select a record type.</span></span> <span data-ttu-id="46ccc-119">Azt is tüntethető fel más bejegyzések például **A rekordok** és **MX-rekordok**.</span><span class="sxs-lookup"><span data-stu-id="46ccc-119">It may also mention other records such as **A records** and **MX records**.</span></span> <span data-ttu-id="46ccc-120">Bizonyos esetekben a CNAME-rekordok fogja meghívni az egyéb, mint egy **aliasrekordot**.</span><span class="sxs-lookup"><span data-stu-id="46ccc-120">In some cases, CNAME records will be called by other names such as an **Alias Record**.</span></span>
   * <span data-ttu-id="46ccc-121">A lap is, amely lehetővé teszi mezők **térkép** a egy **állomásnév** vagy **tartománynév** egy másik tartománynévvel.</span><span class="sxs-lookup"><span data-stu-id="46ccc-121">The page will also have fields that allow you to **map** from a **Host name** or **Domain name** to another domain name.</span></span>
3. <span data-ttu-id="46ccc-122">Minden tartományregisztráló a mintaadatokról eltérők lehetnek, amíg általában leképez *a* az egyéni tartománynevet (például **contoso.com**,) *való* a Traffic Manager szolgáltatásbeli tartománynevére (**contoso.trafficmanager.net**) használt a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="46ccc-122">While the specifics of each registrar vary, in general you map *from* your custom domain name (such as **contoso.com**,) *to* the Traffic Manager domain name (**contoso.trafficmanager.net**) that is used for your web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="46ccc-123">Azt is megteheti Ha egy olyan rekordot már használatban van, és megelőző jelleggel kötni az alkalmazások azt szeretné, létrehozhat egy további CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="46ccc-123">Alternatively, if a record is already in use and you need to preemptively bind your apps to it, you can create an additional CNAME record.</span></span> <span data-ttu-id="46ccc-124">Ahhoz például, hogy megelőző jelleggel kötési **www.contoso.com** a webes alkalmazás, hozzon létre egy CNAME rekordot a **awverify.www** való **contoso.trafficmanager.net**.</span><span class="sxs-lookup"><span data-stu-id="46ccc-124">For example, to preemptively bind **www.contoso.com** to your web app, create a CNAME record from **awverify.www** to **contoso.trafficmanager.net**.</span></span> <span data-ttu-id="46ccc-125">A webalkalmazás a "www" CNAME rekord módosítása nélkül Ezután felvehet "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="46ccc-125">You can then add "www.contoso.com" to your Web App without changing the "www" CNAME record.</span></span> <span data-ttu-id="46ccc-126">További információkért lásd: [hozzon létre DNS-rekordok az egyéni tartomány webalkalmazáshoz][CREATEDNS].</span><span class="sxs-lookup"><span data-stu-id="46ccc-126">For more information, see [Create DNS records for a web app in a custom domain][CREATEDNS].</span></span>
   > 
   > 
4. <span data-ttu-id="46ccc-127">Miután befejezte a hozzáadása vagy módosítása a regisztráló DNS-rekordokat, a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="46ccc-127">Once you have finished adding or modifying DNS records at your registrar, save the changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a><span data-ttu-id="46ccc-128">A Traffic Manager engedélyezése</span><span class="sxs-lookup"><span data-stu-id="46ccc-128">Enable Traffic Manager</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a><span data-ttu-id="46ccc-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46ccc-129">Next steps</span></span>
<span data-ttu-id="46ccc-130">További információk: [Node.js fejlesztői központ](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="46ccc-130">For more information, see the [Node.js Developer Center](/develop/nodejs/).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
