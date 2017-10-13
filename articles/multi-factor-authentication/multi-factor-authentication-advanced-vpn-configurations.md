---
title: "az Azure MFA és a külső VPN-EK aaaAdvanced forgatókönyvek"
description: "Cisco, Citrix vagy Juniper az Azure MFA toointegrate részletes konfigurációs útmutatói."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1f94a214-d6f6-48a8-8a12-006b5896ae45
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/13/2017
ms.author: kgremban
ms.openlocfilehash: e23960ca4977cc01271f99fa2bec70449e9acfff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-third-party-vpn-solutions"></a><span data-ttu-id="d5f44-103">Speciális forgatókönyvek az Azure multi-factor Authentication és a külső VPN-megoldások</span><span class="sxs-lookup"><span data-stu-id="d5f44-103">Advanced scenarios with Azure Multi-Factor Authentication and third-party VPN solutions</span></span>
<span data-ttu-id="d5f44-104">Az Azure multi-factor Authentication használható tooseamlessly csatlakozzon különböző külső VPN-megoldások.</span><span class="sxs-lookup"><span data-stu-id="d5f44-104">Azure Multi-Factor Authentication can be used tooseamlessly connect with various third-party VPN solutions.</span></span> <span data-ttu-id="d5f44-105">Ez a cikk a Cisco® ASA VPN-készülék, Citrix NetScaler SSL VPN-készülék és hello Juniper hálózatok biztonságos hozzáférést/Pulse Secure Csatlakozás biztonságos SSL VPN-készülék összpontosít.</span><span class="sxs-lookup"><span data-stu-id="d5f44-105">This article focuses on Cisco® ASA VPN appliance, Citrix NetScaler SSL VPN appliance, and hello Juniper Networks Secure Access/Pulse Secure Connect Secure SSL VPN appliance.</span></span> <span data-ttu-id="d5f44-106">Három közös készülék létrehozott konfigurációs útmutatók tooaddress, de a multi-factor Authentication kiszolgáló RADIUS, LDAP, az IIS vagy FS szolgáltatáson alapuló Jogcímalapú hitelesítés tooAD használó rendszerek többsége integrálható.</span><span class="sxs-lookup"><span data-stu-id="d5f44-106">We created configuration guides tooaddress these three common appliances, but Multi-Factor Authentication Server can integrate with most systems that use RADIUS, LDAP, IIS, or claims-based authentication tooAD FS.</span></span> <span data-ttu-id="d5f44-107">További részletek találhatók [MFA kiszolgáló konfigurációk](multi-factor-authentication-get-started-server.md#next-steps).</span><span class="sxs-lookup"><span data-stu-id="d5f44-107">You can find more details in [MFA Server configurations](multi-factor-authentication-get-started-server.md#next-steps).</span></span>

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a><span data-ttu-id="d5f44-108">Cisco ASA VPN-készülék és az Azure multi-factor Authentication</span><span class="sxs-lookup"><span data-stu-id="d5f44-108">Cisco ASA VPN appliance and Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="d5f44-109">Az Azure multi-factor Authentication integrálható a Cisco® ASA VPN készülék tooprovide további biztonsági Cisco AnyConnect® VPN bejelentkezéseket és a portál hozzáféréséhez.</span><span class="sxs-lookup"><span data-stu-id="d5f44-109">Azure Multi-Factor Authentication integrates with your Cisco® ASA VPN appliance tooprovide additional security for Cisco AnyConnect® VPN logins and portal access.</span></span>  <span data-ttu-id="d5f44-110">Ezt megteheti vagy hello LDAP és a RADIUS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="d5f44-110">This can be done using either hello LDAP or RADIUS protocol.</span></span>  <span data-ttu-id="d5f44-111">Válasszon egyet az alábbi toodownload hello részletes részletes konfigurációs hello útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="d5f44-111">Select one of hello following toodownload hello detailed step-by-step configuration guides.</span></span>

| <span data-ttu-id="d5f44-112">Konfigurációs útmutató</span><span class="sxs-lookup"><span data-stu-id="d5f44-112">Configuration Guide</span></span> | <span data-ttu-id="d5f44-113">Leírás</span><span class="sxs-lookup"><span data-stu-id="d5f44-113">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="d5f44-114">Cisco ASA LDAP Anyconnect VPN- és az Azure MFA-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d5f44-114">Cisco ASA with Anyconnect VPN and Azure MFA Configuration for LDAP</span></span>](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | <span data-ttu-id="d5f44-115">A Cisco ASA VPN-készülék integrálása az Azure MFA Használatát az LDAP segítségével</span><span class="sxs-lookup"><span data-stu-id="d5f44-115">Integrate your Cisco ASA VPN appliance with Azure MFA using LDAP</span></span> |
| [<span data-ttu-id="d5f44-116">Cisco ASA RADIUS Anyconnect VPN- és az Azure MFA-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d5f44-116">Cisco ASA with Anyconnect VPN and Azure MFA Configuration for RADIUS</span></span>](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | <span data-ttu-id="d5f44-117">A Cisco ASA VPN-készülék integrálása az Azure MFA RADIUS használata</span><span class="sxs-lookup"><span data-stu-id="d5f44-117">Integrate your Cisco ASA VPN appliance with Azure MFA using RADIUS</span></span> |

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a><span data-ttu-id="d5f44-118">Citrix NetScaler SSL VPN- és az Azure többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="d5f44-118">Citrix NetScaler SSL VPN and Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="d5f44-119">Az Azure multi-factor Authentication integrálható a Citrix NetScaler SSL VPN készülék tooprovide további biztonsági Citrix NetScaler SSL VPN bejelentkezéseket és a portál hozzáféréséhez.</span><span class="sxs-lookup"><span data-stu-id="d5f44-119">Azure Multi-Factor Authentication integrates with your Citrix NetScaler SSL VPN appliance tooprovide additional security for Citrix NetScaler SSL VPN logins and portal access.</span></span>  <span data-ttu-id="d5f44-120">Ezt megteheti vagy hello LDAP és a RADIUS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="d5f44-120">This can be done using either hello LDAP or RADIUS protocol.</span></span>  <span data-ttu-id="d5f44-121">Válasszon egyet az alábbi toodownload hello részletes részletes konfigurációs hello útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="d5f44-121">Select one of hello following toodownload hello detailed step-by-step configuration guides.</span></span>

| <span data-ttu-id="d5f44-122">Konfigurációs útmutató</span><span class="sxs-lookup"><span data-stu-id="d5f44-122">Configuration Guide</span></span> | <span data-ttu-id="d5f44-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="d5f44-123">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="d5f44-124">LDAP Citrix NetScaler SSL VPN- és az Azure MFA-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d5f44-124">Citrix NetScaler SSL VPN and Azure MFA Configuration for LDAP</span></span>](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | <span data-ttu-id="d5f44-125">A Citrix NetScaler SSL VPN integrálása az Azure MFA készülék LDAP segítségével</span><span class="sxs-lookup"><span data-stu-id="d5f44-125">Integrate your Citrix NetScaler SSL VPN with Azure MFA appliance using LDAP</span></span> |
| [<span data-ttu-id="d5f44-126">A RADIUS-Citrix NetScaler SSL VPN- és az Azure MFA-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d5f44-126">Citrix NetScaler SSL VPN and Azure MFA Configuration for RADIUS</span></span>](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | <span data-ttu-id="d5f44-127">A Citrix NetScaler SSL VPN-készülék integrálása az Azure MFA RADIUS használata</span><span class="sxs-lookup"><span data-stu-id="d5f44-127">Integrate your Citrix NetScaler SSL VPN appliance with Azure MFA using RADIUS</span></span> |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a><span data-ttu-id="d5f44-128">Juniper/Pulse Secure SSL VPN-készülék és az Azure multi-factor Authentication</span><span class="sxs-lookup"><span data-stu-id="d5f44-128">Juniper/Pulse Secure SSL VPN appliance and Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="d5f44-129">Az Azure multi-factor Authentication integrálható a Juniper/Pulse Secure SSL VPN készülék tooprovide további biztonsági Juniper/Pulse Secure SSL VPN bejelentkezéseket és a portál hozzáféréséhez.</span><span class="sxs-lookup"><span data-stu-id="d5f44-129">Azure Multi-Factor Authentication integrates with your Juniper/Pulse Secure SSL VPN appliance tooprovide additional security for Juniper/Pulse Secure SSL VPN logins and portal access.</span></span>  <span data-ttu-id="d5f44-130">Ezt megteheti vagy hello LDAP és a RADIUS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="d5f44-130">This can be done using either hello LDAP or RADIUS protocol.</span></span>  <span data-ttu-id="d5f44-131">Válasszon egyet az alábbi toodownload hello részletes részletes konfigurációs hello útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="d5f44-131">Select one of hello following toodownload hello detailed step-by-step configuration guides.</span></span>

| <span data-ttu-id="d5f44-132">Konfigurációs útmutató</span><span class="sxs-lookup"><span data-stu-id="d5f44-132">Configuration Guide</span></span> | <span data-ttu-id="d5f44-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="d5f44-133">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="d5f44-134">LDAP Juniper/Pulse biztonságos SSL VPN és az Azure MFA konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d5f44-134">Juniper/Pulse Secure SSL VPN and Azure MFA Configuration for LDAP</span></span>](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | <span data-ttu-id="d5f44-135">A Juniper/Pulse Secure SSL VPN integrálása az Azure MFA készülék LDAP segítségével</span><span class="sxs-lookup"><span data-stu-id="d5f44-135">Integrate your Juniper/Pulse Secure SSL VPN with Azure MFA appliance using LDAP</span></span> |
| [<span data-ttu-id="d5f44-136">A RADIUS-Juniper/Pulse biztonságos SSL VPN- és Azure MFA-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d5f44-136">Juniper/Pulse Secure SSL VPN and Azure MFA Configuration for RADIUS</span></span>](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | <span data-ttu-id="d5f44-137">A Juniper/Pulse Secure SSL VPN-készülék integrálása az Azure MFA RADIUS használata</span><span class="sxs-lookup"><span data-stu-id="d5f44-137">Integrate your Juniper/Pulse Secure SSL VPN appliance with Azure MFA using RADIUS</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d5f44-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5f44-138">Next steps</span></span>

- [<span data-ttu-id="d5f44-139">A meglévő hitelesítési infrastruktúrát a hálózati házirend-kiszolgáló bővítmény hello kiegészítheti az Azure multi-factor Authentication</span><span class="sxs-lookup"><span data-stu-id="d5f44-139">Augment your existing authentication infrastructure with hello NPS extension for Azure Multi-Factor Authentication</span></span>](multi-factor-authentication-nps-extension.md)

- [<span data-ttu-id="d5f44-140">Az Azure Multi-Factor Authentication beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d5f44-140">Configure Azure Multi-Factor Authentication settings</span></span>](multi-factor-authentication-whats-next.md)