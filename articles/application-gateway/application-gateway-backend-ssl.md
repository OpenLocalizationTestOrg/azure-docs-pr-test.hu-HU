---
title: aaaEnabling end tooend Azure Application Gateway SSL |} Microsoft Docs
description: "Ezen a lapon hello Alkalmazásátjáró end tooend támogatja az SSL áttekintést nyújt."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: amsriva
ms.openlocfilehash: c5cb398a1e7d9a08662a3120baad98edb5575917
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-end-tooend-ssl-with-application-gateway"></a><span data-ttu-id="f1298-103">Záró tooend SSL-Titkosítást az Alkalmazásátjáró áttekintése</span><span class="sxs-lookup"><span data-stu-id="f1298-103">Overview of end tooend SSL with Application Gateway</span></span>

<span data-ttu-id="f1298-104">Alkalmazásátjáró támogatja az SSL-lezárást hello átjáróra, után toohello háttérkiszolgálók mely általában a forgalom titkosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="f1298-104">Application gateway supports SSL termination at hello gateway, after which traffic typically flows unencrypted toohello backend servers.</span></span> <span data-ttu-id="f1298-105">Ez a funkció lehetővé teszi, hogy a webes kiszolgálók toobe unburdened a költséges titkosítási és visszafejtési terhelést.</span><span class="sxs-lookup"><span data-stu-id="f1298-105">This feature allows web servers toobe unburdened from costly encryption and decryption overhead.</span></span> <span data-ttu-id="f1298-106">Azonban az egyes ügyfelek titkosítatlan kommunikáció toohello háttérkiszolgálók lehetőség nem elfogadható.</span><span class="sxs-lookup"><span data-stu-id="f1298-106">However for some customers unencrypted communication toohello backend servers is not an acceptable option.</span></span> <span data-ttu-id="f1298-107">Titkosítatlan kommunikáció toosecurity követelmények, a megfelelőségi követelmények oka lehet, vagy hello alkalmazás csak biztonságos kapcsolatot fogad el.</span><span class="sxs-lookup"><span data-stu-id="f1298-107">This unencrypted communication could be due toosecurity requirements, compliance requirements, or hello application may only accept a secure connection.</span></span> <span data-ttu-id="f1298-108">Az ilyen alkalmazások alkalmazás-átjáró támogatja a záró tooend SSL titkosítást.</span><span class="sxs-lookup"><span data-stu-id="f1298-108">For such applications, application gateway supports end tooend SSL encryption.</span></span>

## <a name="overview"></a><span data-ttu-id="f1298-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f1298-109">Overview</span></span>

<span data-ttu-id="f1298-110">Záró tooend SSL lehetővé teszi a toosecurely továbbítja a bizalmas adatok toohello háttér továbbra is kihasználja a réteg 7 terheléselosztási funkciók előnyeit hello mely Alkalmazásátjáró biztosít titkosítja.</span><span class="sxs-lookup"><span data-stu-id="f1298-110">End tooend SSL allows you toosecurely transmit sensitive data toohello backend encrypted while still taking advantage of hello benefits of Layer 7 load balancing features which application gateway provides.</span></span> <span data-ttu-id="f1298-111">Ezek a funkciók némelyike munkamenet cookie-alapú kapcsolat, az URL-alapú útválasztási, a támogatási helyek vagy képességét tooinject X - továbbított - alapú útválasztási * fejléceket.</span><span class="sxs-lookup"><span data-stu-id="f1298-111">Some of these features are cookie-based session affinity, URL-based routing, support for routing based on sites, or ability tooinject X-Forwarded-* headers.</span></span>

<span data-ttu-id="f1298-112">Vége tooend SSL kommunikáció üzemmódban konfigurálásakor Alkalmazásátjáró hello SSL-munkameneteket hello átjáróra leáll, és a felhasználói forgalom visszafejti.</span><span class="sxs-lookup"><span data-stu-id="f1298-112">When configured with end tooend SSL communication mode, application gateway terminates hello SSL sessions at hello gateway and decrypts user traffic.</span></span> <span data-ttu-id="f1298-113">Ezt követően végrehajtja hello konfigurált szabályok tooselect egy megfelelő háttér címkészletet példány tooroute forgalmat.</span><span class="sxs-lookup"><span data-stu-id="f1298-113">It then applies hello configured rules tooselect an appropriate backend pool instance tooroute traffic to.</span></span> <span data-ttu-id="f1298-114">Alkalmazásátjáró majd indít el egy új SSL kapcsolat toohello háttérkiszolgáló, és újból titkosítja az adatokat, mielőtt továbbítanák hello kérelem toohello háttér hello háttér kiszolgáló nyilvános kulcsú tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="f1298-114">Application gateway then initiates a new SSL connection toohello backend server and re-encrypts data using hello backend server's public key certificate before transmitting hello request toohello backend.</span></span> <span data-ttu-id="f1298-115">SSL engedélyezve van, úgy, hogy protokollbeállítás BackendHTTPSetting tooHTTPS, majd pedig a végfelhasználók tooend tooa háttérkészlet alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="f1298-115">End tooend SSL is enabled by setting protocol setting in BackendHTTPSetting tooHTTPS, which is then applied tooa backend pool.</span></span> <span data-ttu-id="f1298-116">Tanúsítvány tooallow biztonságos kommunikációhoz hello háttérkészlet end tooend SSL engedélyezve van a háttér-kiszolgálónként kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="f1298-116">Each backend server in hello backend pool with end tooend SSL enabled must be configured with a certificate tooallow secure communication.</span></span>

![tooend ssl forgatókönyvek][1]

<span data-ttu-id="f1298-118">Ebben a példában a kérelmek TLS1.2 történő olyan irányított toobackend kiszolgálók Pool1 a záró tooend SSL használatával.</span><span class="sxs-lookup"><span data-stu-id="f1298-118">In this example, requests using TLS1.2 are routed toobackend servers in Pool1 using end tooend SSL.</span></span>

## <a name="end-tooend-ssl-and-whitelisting-of-certificates"></a><span data-ttu-id="f1298-119">Befejezési tooend SSL és a tanúsítványok engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f1298-119">End tooend SSL and whitelisting of certificates</span></span>

<span data-ttu-id="f1298-120">Alkalmazásátjáró szerepel az engedélyezési listán hello Alkalmazásátjáró a tanúsítvány rendelkezik ismert háttér-példányok csak kommunikál.</span><span class="sxs-lookup"><span data-stu-id="f1298-120">Application gateway only communicates with known backend instances that have whitelisted their certificate with hello application gateway.</span></span> <span data-ttu-id="f1298-121">tooenable az engedélyezett tanúsítványok, kell feltöltenie hello háttér kiszolgálói tanúsítványok toohello Alkalmazásátjáró (nem hello főtanúsítvány) nyilvános kulcsa.</span><span class="sxs-lookup"><span data-stu-id="f1298-121">tooenable whitelisting of certificates, you must upload hello public key of backend server certificates toohello application gateway (not hello root certificate).</span></span> <span data-ttu-id="f1298-122">Csak kapcsolatok tooknown és engedélyezési háttérkiszolgálókon majd engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="f1298-122">Only connections tooknown and whitelisted backends are then allowed.</span></span> <span data-ttu-id="f1298-123">fennmaradó háttérkiszolgálókon hello egy átjáró hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="f1298-123">hello remaining backends results in a gateway error.</span></span> <span data-ttu-id="f1298-124">Az önaláírt tanúsítványok csupán tesztelési célokat szolgálnak, és nem ajánlottak éles számítási feladatokra.</span><span class="sxs-lookup"><span data-stu-id="f1298-124">Self-signed certificates are for test purposes only and not recommended for production workloads.</span></span> <span data-ttu-id="f1298-125">Ilyen tanúsítványának rendelkeznie kell a toobe engedélyezési hello alkalmazás átjáróval előző lépésekben előtt a felhasználásuk hello leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="f1298-125">Such certificates have toobe whitelisted with hello application gateway as described in hello preceding steps before they can be used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1298-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1298-126">Next steps</span></span>

<span data-ttu-id="f1298-127">Záró tooend SSL megismerését követően nyissa meg túl[end tooend SSL az Alkalmazásátjáró engedélyezése](application-gateway-end-to-end-ssl-powershell.md) egy alkalmazást átjáró, amely toocreate tooend SSL végén.</span><span class="sxs-lookup"><span data-stu-id="f1298-127">After learning about end tooend SSL, go too[enable end tooend SSL on application gateway](application-gateway-end-to-end-ssl-powershell.md) toocreate an application gateway using end tooend SSL.</span></span>

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png
