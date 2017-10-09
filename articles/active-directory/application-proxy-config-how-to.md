---
title: "Az alkalmazásproxy alkalmazás aaaHow tooconfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy néhány egyszerű lépésben alkalmazásproxy alkalmazás konfigurálása"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c64019098fc124e4fe10b8288830bcd2b7239d3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application"></a><span data-ttu-id="cd174-103">Hogyan tooconfigure az alkalmazásproxy-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="cd174-103">How tooconfigure an Application Proxy application</span></span>

<span data-ttu-id="cd174-104">Ez a cikk segítséget toounderstand hogyan tooconfigure belül tooexpose az Azure AD alkalmazásproxy kérelmet a helyszíni alkalmazások toohello a felhő.</span><span class="sxs-lookup"><span data-stu-id="cd174-104">This article help you toounderstand how tooconfigure an Application Proxy application within Azure AD tooexpose your on-premises applications toohello cloud.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="cd174-105">Ajánlott dokumentumok</span><span class="sxs-lookup"><span data-stu-id="cd174-105">Recommended documents</span></span> 

<span data-ttu-id="cd174-106">hello kezdeti, valamint az alkalmazásproxy alkalmazások hello felügyeleti portálon keresztül létrehozása toolearn kövesse hello [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="cd174-106">toolearn about hello initial configurations and creation of an Application Proxy application through hello Admin Portal, follow hello [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="cd174-107">Az összekötők konfigurálása, lásd: [alkalmazásproxy engedélyezése az Azure portál hello](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="cd174-107">For details on configuring Connectors, see [Enable Application Proxy in hello Azure portal](active-directory-application-proxy-enable.md).</span></span>

<span data-ttu-id="cd174-108">Tanúsítványok feltöltése és egyéni tartomány használatával kapcsolatos tudnivalókat lásd: [egyéni tartományok az Azure AD alkalmazásproxy használata](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="cd174-108">For information on uploading certificates and using custom domains, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span>

## <a name="create-hello-applicationsetting-hello-urls"></a><span data-ttu-id="cd174-109">Hello alkalmazás-beállítás hello URL-címek létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd174-109">Create hello Application/Setting hello URLs</span></span>

<span data-ttu-id="cd174-110">Ha követi hello hello szükséges lépések [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) és a dokumentáció első hello alkalmazás létrehozásakor hiba lépett fel a hello hiba részletes információkat, és javaslatokat arról, hogyan lásd: toofix hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cd174-110">If you are following hello steps in hello [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) documentation and are getting an error creating hello application, see hello error details for information and suggestions for how toofix hello application.</span></span> <span data-ttu-id="cd174-111">A legtöbb hiba üzenetekben javasolt javítás.</span><span class="sxs-lookup"><span data-stu-id="cd174-111">Most error messages include a suggested fix.</span></span> <span data-ttu-id="cd174-112">tooavoid gyakori hibákat, ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="cd174-112">tooavoid common errors, verify:</span></span>

-   <span data-ttu-id="cd174-113">Az engedély toocreate proxyval alkalmazás rendszergazda</span><span class="sxs-lookup"><span data-stu-id="cd174-113">You are an administrator with permission toocreate an Application Proxy application</span></span>

-   <span data-ttu-id="cd174-114">hello belső URL-címe egyedi:</span><span class="sxs-lookup"><span data-stu-id="cd174-114">hello internal URL is unique</span></span>

-   <span data-ttu-id="cd174-115">hello külső URL-címe egyedi:</span><span class="sxs-lookup"><span data-stu-id="cd174-115">hello external URL is unique</span></span>

-   <span data-ttu-id="cd174-116">hello http vagy https URL-címek kezdődik, és a szöveg végén a "/"</span><span class="sxs-lookup"><span data-stu-id="cd174-116">hello URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="cd174-117">hello URL-cím a tartomány nevét, IP-cím nem kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cd174-117">hello URL should be a domain name, not an IP address</span></span>

<span data-ttu-id="cd174-118">hello hibaüzenet jelenik meg hello jobb felső sarokban található hello alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="cd174-118">hello error message should display in hello top right corner when you create hello application.</span></span> <span data-ttu-id="cd174-119">Ehelyett választhatja hello értesítési ikon toosee hello hibaüzenetek.</span><span class="sxs-lookup"><span data-stu-id="cd174-119">You can also select hello notification icon toosee hello error messages.</span></span>

   ![Értesítési üzenet](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a><span data-ttu-id="cd174-121">Az összekötők/összekötő csoportok beállítása</span><span class="sxs-lookup"><span data-stu-id="cd174-121">Configure connectors/connector groups</span></span>

<span data-ttu-id="cd174-122">Ha az alkalmazás beállítása miatt figyelmeztetést jelenít meg hello összekötők és összekötő csoportok probléma, lásd: utasításokat a alkalmazásproxy engedélyezése kapcsolatos részletes tudnivalókért toodownload összekötők.</span><span class="sxs-lookup"><span data-stu-id="cd174-122">If you are having difficulty configuring your application because of warning about hello connectors and connector groups, see instructions on enabling Application Proxy for details on how toodownload connectors.</span></span> <span data-ttu-id="cd174-123">Ha azt szeretné, hogy további információk az összekötők toolearn, tekintse meg a hello [összekötők dokumentációja](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span><span class="sxs-lookup"><span data-stu-id="cd174-123">If you want toolearn more about connectors, see hello [connectors documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span></span>

<span data-ttu-id="cd174-124">Ha az összekötők nem működnek, ez azt jelenti, hogy azok nem tooreach hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cd174-124">If your connectors are inactive, this means that they are unable tooreach hello service.</span></span> <span data-ttu-id="cd174-125">Ez gyakran azért van, mert minden szükséges hello port nincs nyitva.</span><span class="sxs-lookup"><span data-stu-id="cd174-125">This is often because all hello required ports are not open.</span></span> <span data-ttu-id="cd174-126">toosee szükséges portok listáját a alkalmazásproxy dokumentáció engedélyezése hello hello Előfeltételek című szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="cd174-126">toosee a list of required ports, see hello pre-requisites section of hello enabling Application Proxy documentation.</span></span>

## <a name="upload-certificates-for-custom-domains"></a><span data-ttu-id="cd174-127">Az egyéni tartományok tanúsítványok feltöltése</span><span class="sxs-lookup"><span data-stu-id="cd174-127">Upload certificates for custom domains</span></span>

<span data-ttu-id="cd174-128">Egyéni tartományok toospecify hello tartományának a külső URL-címek engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cd174-128">Custom Domains allow you toospecify hello domain of your external URLs.</span></span> <span data-ttu-id="cd174-129">egyéni tartományok toouse, tanúsítványra van szükség tooupload hello ehhez a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="cd174-129">toouse custom domains, you need tooupload hello certificate for that domain.</span></span> <span data-ttu-id="cd174-130">Egyéni tartományok és tanúsítványok használatával kapcsolatos információkért lásd: [egyéni tartományok az Azure AD alkalmazásproxy használata](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="cd174-130">For information on using custom domains and certificates, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span> 

<span data-ttu-id="cd174-131">Ha a tanúsítvány feltöltése problémák adódtak, keresse meg hello hibaüzenetek hello portálon kapcsolatos további információkért hello hello tanúsítványával kapcsolatos problémára.</span><span class="sxs-lookup"><span data-stu-id="cd174-131">If you are encountering issues uploading your certificate, look for hello error messages in hello portal for additional information on hello problem with hello certificate.</span></span> <span data-ttu-id="cd174-132">Közös tanúsítvánnyal kapcsolatos problémák a következők:</span><span class="sxs-lookup"><span data-stu-id="cd174-132">Common certificate problems include:</span></span>

-   <span data-ttu-id="cd174-133">A lejárt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="cd174-133">Expired certificate</span></span>

-   <span data-ttu-id="cd174-134">Önaláírt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="cd174-134">Certificate is self-signed</span></span>

-   <span data-ttu-id="cd174-135">Tanúsítvány hiányzik a hello titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="cd174-135">Certificate is missing hello private key</span></span>

<span data-ttu-id="cd174-136">hello hibaüzenet jelenik meg a hello jobb felső sarokban tooupload hello tanúsítványt próbálja ki.</span><span class="sxs-lookup"><span data-stu-id="cd174-136">hello error message display in hello top right corner as you try tooupload hello certificate.</span></span> <span data-ttu-id="cd174-137">Ehelyett választhatja hello értesítési ikon toosee hello hibaüzenetek.</span><span class="sxs-lookup"><span data-stu-id="cd174-137">You can also select hello notification icon toosee hello error messages.</span></span>

   ![Értesítési üzenet](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a><span data-ttu-id="cd174-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd174-139">Next steps</span></span>
[<span data-ttu-id="cd174-140">Az Azure AD-alkalmazásproxy használó alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="cd174-140">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)
