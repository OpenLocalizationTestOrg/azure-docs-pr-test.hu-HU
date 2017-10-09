---
title: "Azure AD alkalmazás Proxy alkalmazások csoportban aaaAccess |} Microsoft Docs"
description: "Használhatja az Azure AD-alkalmazásproxy tooaccess a helyszíni Microsoft Teams keresztül."
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
ms.date: 07/12/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 13c36e43ae6349df09272e308ad4f40451cbbeb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a><span data-ttu-id="0f95a-103">Microsoft Teams keresztül férnek hozzá a helyszíni alkalmazások</span><span class="sxs-lookup"><span data-stu-id="0f95a-103">Access your on-premises applications through Microsoft Teams</span></span>

<span data-ttu-id="0f95a-104">Az Azure Active Directory Alkalmazásproxyjával lehetővé teszi az egyszeri bejelentkezés tooon helyszíni alkalmazások függetlenül attól, hogy hol áll, és a Microsoft Teams leegyszerűsíti a együttműködési próbálkozások egy helyen.</span><span class="sxs-lookup"><span data-stu-id="0f95a-104">Azure Active Directory Application Proxy gives you single sign-on tooon-premises applications no matter where you are, and Microsoft Teams streamlines your collaborative efforts in one place.</span></span> <span data-ttu-id="0f95a-105">Hello integrálása két együtt azt jelenti, hogy a felhasználók hatékony legyen a teammates minden helyzetben lehetnek.</span><span class="sxs-lookup"><span data-stu-id="0f95a-105">Integrating hello two together means that your users can be productive with their teammates in any situation.</span></span> 

<span data-ttu-id="0f95a-106">A felhasználók is hozzáadhat a felhőalapú alkalmazások tootheir csapatok csatornák [lapok használatával](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), de mi történik, ha a SharePoint-webhely és a tervezési eszköz, azok összes helyben tárolt?</span><span class="sxs-lookup"><span data-stu-id="0f95a-106">Your users can add cloud apps tootheir Teams channels [using tabs](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), but what happens if that SharePoint site or planning tool they all use is hosted on-premises?</span></span> <span data-ttu-id="0f95a-107">Alkalmazásproxy egy hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="0f95a-107">Application Proxy is hello solution.</span></span> <span data-ttu-id="0f95a-108">Akkor is hozzáadhat alkalmazásokat alkalmazásproxy tootheir közzétett csatornák használatával hello azonos külső URL-címek mindig használata tooaccess alkalmazások távolról.</span><span class="sxs-lookup"><span data-stu-id="0f95a-108">They can add apps published through Application Proxy tootheir channels using hello same external URLs they always use tooaccess their apps remotely.</span></span> <span data-ttu-id="0f95a-109">És mivel alkalmazásproxy hitelesíti az Azure Active Directoryban, egyszeri bejelentkezéses felhasználói élmény ugyanaz hordoz magában, keresztül hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="0f95a-109">And because Application Proxy authenticates through Azure Active Directory, hello same single sign-on experience carries through.</span></span>


## <a name="install-hello-application-proxy-connector-and-publish-your-app"></a><span data-ttu-id="0f95a-110">Hello alkalmazásproxy-összekötő telepítése és az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="0f95a-110">Install hello Application Proxy connector and publish your app</span></span>

<span data-ttu-id="0f95a-111">Ha még nem tette, [-Proxy konfigurálása a bérlő számára, és hello összekötő telepítéséhez](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="0f95a-111">If you haven't already, [configure Application Proxy for your tenant and install hello connector](active-directory-application-proxy-enable.md).</span></span> <span data-ttu-id="0f95a-112">Ezt követően [a helyszíni alkalmazások közzététele](application-proxy-publish-azure-portal.md) táveléréshez.</span><span class="sxs-lookup"><span data-stu-id="0f95a-112">Then, [publish your on-premises application](application-proxy-publish-azure-portal.md) for remote access.</span></span> <span data-ttu-id="0f95a-113">Hello app közzétételekor most jegyezze fel a hello külső URL-cím, mert a végfelhasználók számára, hogy információra van szükség, amikor hello app tooTeams.</span><span class="sxs-lookup"><span data-stu-id="0f95a-113">When you're publishing hello app, make note of hello external URL because your end users need that information when they add hello app tooTeams.</span></span>

<span data-ttu-id="0f95a-114">Ha már rendelkezik a közzétett alkalmazásokhoz, de nem emlékszik a külső URL-címeit, keresse meg őket hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0f95a-114">If you already have your apps published but don't remember their external URLs, look them up in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="0f95a-115">Jelentkezzen be, majd keresse meg a túl**Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás** > Válassza ki az alkalmazást > **Alkalmazásproxy**.</span><span class="sxs-lookup"><span data-stu-id="0f95a-115">Sign in, then navigate too**Azure Active Directory** > **Enterprise applications** > **All applications** > select your app > **Application proxy**.</span></span>

## <a name="add-your-app-tooteams"></a><span data-ttu-id="0f95a-116">Az alkalmazás tooTeams hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0f95a-116">Add your app tooTeams</span></span>

<span data-ttu-id="0f95a-117">Ha közzéteszi a proxyn keresztül történő hello app, tudassa a felhasználókkal, tudja, hogy azok is megadhatja azt a lapján közvetlenül a csapatok csatornák.</span><span class="sxs-lookup"><span data-stu-id="0f95a-117">Once you publish hello app through Application Proxy, let your users know that they can add it as a tab directly in their Teams channels.</span></span> <span data-ttu-id="0f95a-118">Kövesse a fenti három lépést rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="0f95a-118">Have them follow these three steps:</span></span>

1. <span data-ttu-id="0f95a-119">Keresse meg a toohello csoportok csatorna tooadd, ahová ezt az alkalmazást, és válassza ki  **+**  tooadd egy lapon.</span><span class="sxs-lookup"><span data-stu-id="0f95a-119">Navigate toohello Teams channel where you want tooadd this app and select **+** tooadd a tab.</span></span>

   ![Válassza a lap](./media/application-proxy-teams/add-tab.png)

2. <span data-ttu-id="0f95a-121">Válassza ki **webhely** hello lapon lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="0f95a-121">Select **Website** from hello tab options.</span></span>

   ![Webhely hozzáadása](./media/application-proxy-teams/website.png)

3. <span data-ttu-id="0f95a-123">Hello lapon adjon egy nevet, és hello URL-cím toohello proxyval külső URL-Címének beállítása.</span><span class="sxs-lookup"><span data-stu-id="0f95a-123">Give hello tab a name and set hello URL toohello Application Proxy external URL.</span></span> 

   ![Lap neve és az URL-cím konfigurálása](./media/application-proxy-teams/tab-name-url.png)

<span data-ttu-id="0f95a-125">Után a csoport egyik tagjának hello lapon ad hozzá, mutatja a hello csatorna mindenki számára.</span><span class="sxs-lookup"><span data-stu-id="0f95a-125">Once one member of a team adds hello tab, it shows up for everyone in hello channel.</span></span> <span data-ttu-id="0f95a-126">Bármely access toohello alkalmazásnak rendelkező felhasználónál egyszeri bejelentkezéses hozzáférést Microsoft Teams használata hello hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="0f95a-126">Any users who have access toohello app get single sign-on access with hello credentials they use for Microsoft Teams.</span></span> <span data-ttu-id="0f95a-127">Hozzáférés toohello app nem rendelkező felhasználók csoportban hello lapon megjelenik, de le vannak tiltva, és az engedélyek toohello helyszíni app számukra hello hello alkalmazás az Azure portálon közzétett verziója.</span><span class="sxs-lookup"><span data-stu-id="0f95a-127">Any users who don't have access toohello app will see hello tab in Teams, but are blocked until you give them permissions toohello on-premises app and hello Azure portal published version of hello app.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0f95a-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0f95a-128">Next steps</span></span>

- <span data-ttu-id="0f95a-129">Ismerje meg, hogyan túl[tegye közzé a helyszíni SharePoint helyeket](application-proxy-enable-remote-access-sharepoint.md) az alkalmazásproxy.</span><span class="sxs-lookup"><span data-stu-id="0f95a-129">Learn how too[publish on-premises SharePoint sites](application-proxy-enable-remote-access-sharepoint.md) with Application Proxy.</span></span>
- <span data-ttu-id="0f95a-130">Konfigurálja az alkalmazások toouse [egyéni tartományok](active-directory-application-proxy-custom-domains.md) a külső URL-címhez.</span><span class="sxs-lookup"><span data-stu-id="0f95a-130">Configure your apps toouse [custom domains](active-directory-application-proxy-custom-domains.md) for their external URL.</span></span> 
