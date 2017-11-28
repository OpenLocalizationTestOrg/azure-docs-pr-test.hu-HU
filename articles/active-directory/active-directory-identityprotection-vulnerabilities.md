---
title: "Azure Active Directory Identity Protection által észlelt aaaVulnerabilities |} Microsoft Docs"
description: "Azure Active Directory Identity Protection által észlelt hello biztonsági rések áttekintése."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 5e1cb401f8b566a180eb46e3420a090bcfc66767
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a><span data-ttu-id="8170a-104">Azure Active Directory Identity Protection által észlelt biztonsági rések</span><span class="sxs-lookup"><span data-stu-id="8170a-104">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>
<span data-ttu-id="8170a-105">Biztonsági rések a környezetben, amely is kihasználható egy támadó gyengeségei miatt.</span><span class="sxs-lookup"><span data-stu-id="8170a-105">Vulnerabilities are weaknesses in your environment that can be exploited by an attacker.</span></span> <span data-ttu-id="8170a-106">Azt javasoljuk, hogy a biztonsági rések tooimprove hello biztonságot, ha a szervezet cím, és hogy a támadók kihasználva őket.</span><span class="sxs-lookup"><span data-stu-id="8170a-106">We recommend that you address these vulnerabilities tooimprove hello security posture of your organization, and prevent attackers from exploiting them.</span></span>


<span data-ttu-id="8170a-107">![biztonsági rések](./media/active-directory-identityprotection-vulnerabilities/101.png "biztonsági réseket")</span><span class="sxs-lookup"><span data-stu-id="8170a-107">![vulnerabilities](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")</span></span>



<span data-ttu-id="8170a-108">hello alább láthatja hello biztonsági rések Identity Protection által jelentett áttekintést.</span><span class="sxs-lookup"><span data-stu-id="8170a-108">hello following sections provide you with an overview of hello vulnerabilities reported by Identity Protection.</span></span>

## <a name="multi-factor-authentication-registration-not-configured"></a><span data-ttu-id="8170a-109">A multi-factor authentication regisztráció nincs konfigurálva</span><span class="sxs-lookup"><span data-stu-id="8170a-109">Multi-factor authentication registration not configured</span></span>
<span data-ttu-id="8170a-110">A biztonsági rés segítségével szabályozhatja a szervezet Azure multi-factor Authentication hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="8170a-110">This vulnerability helps you control hello deployment of Azure Multi-Factor Authentication in your organization.</span></span> 

<span data-ttu-id="8170a-111">Azure multi-factor Authentication hitelesítési biztonsági toouser hitelesítési egy második réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="8170a-111">Azure multi-factor authentication provides a second layer of security toouser authentication.</span></span> <span data-ttu-id="8170a-112">Segít a biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint.</span><span class="sxs-lookup"><span data-stu-id="8170a-112">It helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="8170a-113">Erős hitelesítés, könnyen ellenőrzési lehetőségek széles keresztül biztosítja – a telefonhívás, szöveges üzenet vagy mobilalkalmazás értesítés vagy ellenőrző kód és a 3. fél OATH-tokeneket.</span><span class="sxs-lookup"><span data-stu-id="8170a-113">It delivers strong authentication via a range of easy verification options—phone call, text message, or mobile app notification or verification code and 3rd party OATH tokens.</span></span>

<span data-ttu-id="8170a-114">Azt javasoljuk, hogy a felhasználói bejelentkezések Azure multi-factor Authentication hitelesítés szükséges. A multi-factor authentication kulcsfontosságú szerepet játszik a kockázat-alapú feltételes hozzáférési házirendek Identity Protection keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="8170a-114">We recommend that you require Azure Multi-Factor Authentication for user sign-ins. Multi-factor authentication plays a key role in risk-based conditional access policies available through Identity Protection.</span></span>

<span data-ttu-id="8170a-115">További részletekért lásd: [Mi az Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="8170a-115">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

## <a name="unmanaged-cloud-apps"></a><span data-ttu-id="8170a-116">Nem felügyelt felhőalkalmazások</span><span class="sxs-lookup"><span data-stu-id="8170a-116">Unmanaged cloud apps</span></span>
<span data-ttu-id="8170a-117">A biztonsági rés segít azonosítani a szervezet nem felügyelt felhőalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8170a-117">This vulnerability helps you identify unmanaged cloud apps in your organization.</span></span>

<span data-ttu-id="8170a-118">A modern vállalatok számára az informatikai részlegeket gyakran nem tudnak a összes hello felhőalapú alkalmazásokhoz, hogy a szervezetek használó felhasználókat toodo a munkahelyi.</span><span class="sxs-lookup"><span data-stu-id="8170a-118">In modern enterprises, IT departments are often unaware of all hello cloud applications that users in their organization are using toodo their work.</span></span> <span data-ttu-id="8170a-119">Miért a rendszergazdák jogosulatlan hozzáférés toocorporate adatokat, a lehetséges adatszivárgás és az egyéb biztonsági kockázatok kétségei vannak könnyen toosee.</span><span class="sxs-lookup"><span data-stu-id="8170a-119">It is easy toosee why administrators would have concerns about unauthorized access toocorporate data, possible data leakage and other security risks.</span></span> 

<span data-ttu-id="8170a-120">Azt javasoljuk, hogy a szervezet és központi telepítése a Cloud App Discovery nem felügyelt toodiscover felhőalkalmazásokhoz toomanage ezeket az alkalmazásokat az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="8170a-120">We recommend that your organization deploy Cloud App Discovery toodiscover unmanaged cloud applications, and toomanage these applications using Azure Active Directory.</span></span>

<span data-ttu-id="8170a-121">További részletekért lásd: [keresése a nem felügyelt felhőalapú alkalmazásokhoz, a Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8170a-121">For more details, see [Finding unmanaged cloud applications with Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>

## <a name="security-alerts-from-privileged-identity-management"></a><span data-ttu-id="8170a-122">Privileged Identity Management biztonsági riasztásai</span><span class="sxs-lookup"><span data-stu-id="8170a-122">Security Alerts from Privileged Identity Management</span></span>
<span data-ttu-id="8170a-123">A biztonsági rés segít felderíteni, és oldja meg a szervezet a kiemelt jogosultságú identitások kapcsolatos riasztások.</span><span class="sxs-lookup"><span data-stu-id="8170a-123">This vulnerability helps you discover and resolve alerts about privileged identities in your organization.</span></span>  

<span data-ttu-id="8170a-124">tooenable felhasználók toocarry jogosultságokhoz kötött műveletek ki, a szervezeteknek kell toogrant felhasználók ideiglenes és állandó kiemelt férhetnek hozzá az Azure AD-erőforrások Azure vagy az Office 365 vagy más Szolgáltatottszoftver-alkalmazásoknál.</span><span class="sxs-lookup"><span data-stu-id="8170a-124">tooenable users toocarry out privileged operations, organizations need toogrant users temporary or permanent privileged access in Azure AD, Azure or Office 365 resources, or other SaaS apps.</span></span> <span data-ttu-id="8170a-125">Minden egyes kiemelt felhasználók növekszik hello támadási felületét a szervezet.</span><span class="sxs-lookup"><span data-stu-id="8170a-125">Each of these privileged users increases hello attack surface of your organization.</span></span> <span data-ttu-id="8170a-126">A biztonsági rés segítséget nyújt a felhasználók azonosítása a szükségtelen rendszerjogosultságú hozzáférés és a megfelelő lépéseket tooreduce végrehajtása vagy a kiküszöbölése hello kockázatot jelentenek.</span><span class="sxs-lookup"><span data-stu-id="8170a-126">This vulnerability helps you identify users with unnecessary privileged access, and take appropriate action tooreduce or eliminate hello risk they pose.</span></span> 

<span data-ttu-id="8170a-127">Azt javasoljuk, hogy a szervezet Azure AD Privileged Identity Management toomanage, vezérlő, és a figyelő emelt szintű identitások és azok az Azure AD hozzáférési tooresources, valamint más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune használja.</span><span class="sxs-lookup"><span data-stu-id="8170a-127">We recommend that your organization uses Azure AD Privileged Identity Management toomanage, control, and monitor privileged identities and their access tooresources in Azure AD as well as other Microsoft online services like Office 365 or Microsoft Intune.</span></span>

<span data-ttu-id="8170a-128">További részletekért lásd: [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="8170a-128">For more details, see [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="see-also"></a><span data-ttu-id="8170a-129">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8170a-129">See also</span></span>
* [<span data-ttu-id="8170a-130">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="8170a-130">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

