---
title: "Az alkalmazásproxy alkalmazás konfigurálása |} Microsoft Docs"
description: "Ismerje meg, hogyan hozhat létre egy konfigurálása az alkalmazásproxy alkalmazást néhány egyszerű lépésben"
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
ms.openlocfilehash: c8f98536048a85ebb3f061d840457130579196d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-an-application-proxy-application"></a><span data-ttu-id="a3a8c-103">Az alkalmazásproxy alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3a8c-103">How to configure an Application Proxy application</span></span>

<span data-ttu-id="a3a8c-104">Ez a cikk segít megérteni, hogyan teszi közzé a felhőbe a helyszíni alkalmazások belül az Azure AD alkalmazásproxy alkalmazást konfigurálhatja úgy nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-104">This article help you to understand how to configure an Application Proxy application within Azure AD to expose your on-premises applications to the cloud.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="a3a8c-105">Ajánlott dokumentumok</span><span class="sxs-lookup"><span data-stu-id="a3a8c-105">Recommended documents</span></span> 

<span data-ttu-id="a3a8c-106">További tudnivalók a kezdeti konfigurációt és a felügyeleti portálon keresztül alkalmazásproxy alkalmazás létrehozását, hajtsa végre a [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="a3a8c-106">To learn about the initial configurations and creation of an Application Proxy application through the Admin Portal, follow the [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="a3a8c-107">Az összekötők konfigurálása, lásd: [alkalmazásproxy engedélyezése az Azure-portálon](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="a3a8c-107">For details on configuring Connectors, see [Enable Application Proxy in the Azure portal](active-directory-application-proxy-enable.md).</span></span>

<span data-ttu-id="a3a8c-108">Tanúsítványok feltöltése és egyéni tartomány használatával kapcsolatos tudnivalókat lásd: [egyéni tartományok az Azure AD alkalmazásproxy használata](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="a3a8c-108">For information on uploading certificates and using custom domains, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span>

## <a name="create-the-applicationsetting-the-urls"></a><span data-ttu-id="a3a8c-109">Az Alkalmazásbeállítás URL-címek létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3a8c-109">Create the Application/Setting the URLs</span></span>

<span data-ttu-id="a3a8c-110">Ha a következő lépéseket a [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) és a dokumentáció első hiba történt az alkalmazás létrehozásakor tekintse meg a hiba részletes adatait információk és javaslatok megoldásával a az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-110">If you are following the steps in the [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) documentation and are getting an error creating the application, see the error details for information and suggestions for how to fix the application.</span></span> <span data-ttu-id="a3a8c-111">A legtöbb hiba üzenetekben javasolt javítás.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-111">Most error messages include a suggested fix.</span></span> <span data-ttu-id="a3a8c-112">Gyakori hibák elkerülése érdekében győződjön meg arról:</span><span class="sxs-lookup"><span data-stu-id="a3a8c-112">To avoid common errors, verify:</span></span>

-   <span data-ttu-id="a3a8c-113">Alkalmazásproxy alkalmazás létrehozása engedéllyel rendelkező rendszergazdáknak</span><span class="sxs-lookup"><span data-stu-id="a3a8c-113">You are an administrator with permission to create an Application Proxy application</span></span>

-   <span data-ttu-id="a3a8c-114">A belső URL-címe egyedi:</span><span class="sxs-lookup"><span data-stu-id="a3a8c-114">The internal URL is unique</span></span>

-   <span data-ttu-id="a3a8c-115">A külső URL-címe egyedi:</span><span class="sxs-lookup"><span data-stu-id="a3a8c-115">The external URL is unique</span></span>

-   <span data-ttu-id="a3a8c-116">Az URL-címének http vagy https kezdődnie, és végén a "/"</span><span class="sxs-lookup"><span data-stu-id="a3a8c-116">The URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="a3a8c-117">Az URL-cím a tartomány nevét, IP-cím nem kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-117">The URL should be a domain name, not an IP address</span></span>

<span data-ttu-id="a3a8c-118">A jobb felső sarokban a hibaüzenet a következő megjelenjen-e az alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-118">The error message should display in the top right corner when you create the application.</span></span> <span data-ttu-id="a3a8c-119">Igény szerint kiválaszthatja az értesítés ikonra a hibaüzenetekben talál.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-119">You can also select the notification icon to see the error messages.</span></span>

   ![Értesítési üzenet](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a><span data-ttu-id="a3a8c-121">Az összekötők/összekötő csoportok beállítása</span><span class="sxs-lookup"><span data-stu-id="a3a8c-121">Configure connectors/connector groups</span></span>

<span data-ttu-id="a3a8c-122">Ha a rendelkezésre álló összekötők és összekötő csoportok figyelmeztetés miatt az alkalmazás konfigurálása, lásd: utasításokat a letöltési összekötők részletekért alkalmazásproxy engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-122">If you are having difficulty configuring your application because of warning about the connectors and connector groups, see instructions on enabling Application Proxy for details on how to download connectors.</span></span> <span data-ttu-id="a3a8c-123">Ha azt szeretné, további információt az összekötők, tekintse meg a [összekötők dokumentációja](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span><span class="sxs-lookup"><span data-stu-id="a3a8c-123">If you want to learn more about connectors, see the [connectors documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span></span>

<span data-ttu-id="a3a8c-124">Ha az összekötők nem működnek, ez azt jelenti, hogy azok nem érhetők el a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-124">If your connectors are inactive, this means that they are unable to reach the service.</span></span> <span data-ttu-id="a3a8c-125">Ez gyakran azért van, mivel nincsenek nyissa meg a szükséges portok.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-125">This is often because all the required ports are not open.</span></span> <span data-ttu-id="a3a8c-126">Szükséges portok listájának megtekintéséhez lásd: a szükséges Előfeltételek című részben engedélyezése alkalmazásproxy.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-126">To see a list of required ports, see the pre-requisites section of the enabling Application Proxy documentation.</span></span>

## <a name="upload-certificates-for-custom-domains"></a><span data-ttu-id="a3a8c-127">Az egyéni tartományok tanúsítványok feltöltése</span><span class="sxs-lookup"><span data-stu-id="a3a8c-127">Upload certificates for custom domains</span></span>

<span data-ttu-id="a3a8c-128">Egyéni tartományok engedélyezi, hogy a külső URL-címek tartományát adja meg.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-128">Custom Domains allow you to specify the domain of your external URLs.</span></span> <span data-ttu-id="a3a8c-129">Egyéni tartományok használatához szüksége töltse fel az ehhez a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-129">To use custom domains, you need to upload the certificate for that domain.</span></span> <span data-ttu-id="a3a8c-130">Egyéni tartományok és tanúsítványok használatával kapcsolatos információkért lásd: [egyéni tartományok az Azure AD alkalmazásproxy használata](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="a3a8c-130">For information on using custom domains and certificates, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span> 

<span data-ttu-id="a3a8c-131">Ha a tanúsítvány feltöltése problémák adódtak, keresse meg a hibaüzeneteket, a portálon található további információ a tanúsítvánnyal kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-131">If you are encountering issues uploading your certificate, look for the error messages in the portal for additional information on the problem with the certificate.</span></span> <span data-ttu-id="a3a8c-132">Közös tanúsítvánnyal kapcsolatos problémák a következők:</span><span class="sxs-lookup"><span data-stu-id="a3a8c-132">Common certificate problems include:</span></span>

-   <span data-ttu-id="a3a8c-133">A lejárt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="a3a8c-133">Expired certificate</span></span>

-   <span data-ttu-id="a3a8c-134">Önaláírt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="a3a8c-134">Certificate is self-signed</span></span>

-   <span data-ttu-id="a3a8c-135">Tanúsítvány hiányzik a titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="a3a8c-135">Certificate is missing the private key</span></span>

<span data-ttu-id="a3a8c-136">A hibaüzenetet jeleníti meg a jobb felső sarokban található megpróbálja feltölteni a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-136">The error message display in the top right corner as you try to upload the certificate.</span></span> <span data-ttu-id="a3a8c-137">Igény szerint kiválaszthatja az értesítés ikonra a hibaüzenetekben talál.</span><span class="sxs-lookup"><span data-stu-id="a3a8c-137">You can also select the notification icon to see the error messages.</span></span>

   ![Értesítési üzenet](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a><span data-ttu-id="a3a8c-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3a8c-139">Next steps</span></span>
[<span data-ttu-id="a3a8c-140">Az Azure AD-alkalmazásproxy használó alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="a3a8c-140">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)
