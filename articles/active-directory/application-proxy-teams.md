---
title: "Az Azure AD alkalmazás Proxy alkalmazások csoportban eléréséhez |} Microsoft Docs"
description: "Azure AD alkalmazásproxy segítségével Microsoft Teams keresztül férnek hozzá a helyszíni alkalmazások."
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
ms.openlocfilehash: 24e22d7314de536714a825cd7035d2cec2112278
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a><span data-ttu-id="a6b36-103">Microsoft Teams keresztül férnek hozzá a helyszíni alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a6b36-103">Access your on-premises applications through Microsoft Teams</span></span>

<span data-ttu-id="a6b36-104">Az Azure Active Directory Alkalmazásproxyjával lehetővé teszi az egyszeri bejelentkezést a helyszíni alkalmazások függetlenül attól, hogy hol áll, és a Microsoft Teams leegyszerűsíti a együttműködési próbálkozások egy helyen.</span><span class="sxs-lookup"><span data-stu-id="a6b36-104">Azure Active Directory Application Proxy gives you single sign-on to on-premises applications no matter where you are, and Microsoft Teams streamlines your collaborative efforts in one place.</span></span> <span data-ttu-id="a6b36-105">A kettő együtt integrálása, az azt jelenti, hogy a felhasználók hatékony legyen a teammates minden helyzetben lehetnek.</span><span class="sxs-lookup"><span data-stu-id="a6b36-105">Integrating the two together means that your users can be productive with their teammates in any situation.</span></span> 

<span data-ttu-id="a6b36-106">A felhasználók felhőalapú alkalmazások hozzáadása a saját csoportjai csatornák [lapok használatával](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), de mi történik, ha a SharePoint-webhely és a tervezési eszköz, azok összes helyben tárolt?</span><span class="sxs-lookup"><span data-stu-id="a6b36-106">Your users can add cloud apps to their Teams channels [using tabs](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), but what happens if that SharePoint site or planning tool they all use is hosted on-premises?</span></span> <span data-ttu-id="a6b36-107">Alkalmazásproxy használata a megoldás.</span><span class="sxs-lookup"><span data-stu-id="a6b36-107">Application Proxy is the solution.</span></span> <span data-ttu-id="a6b36-108">Azokat hozzáadják a proxyn keresztül történő a csatornák a azonos külső URL-címekkel mindig használata alkalmazások távoli eléréséhez a közzétett alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a6b36-108">They can add apps published through Application Proxy to their channels using the same external URLs they always use to access their apps remotely.</span></span> <span data-ttu-id="a6b36-109">És mivel alkalmazásproxy Azure Active Directory használatával hitelesíti, egyszeri bejelentkezéses felhasználói élmény ugyanaz hordoz magában, ha.</span><span class="sxs-lookup"><span data-stu-id="a6b36-109">And because Application Proxy authenticates through Azure Active Directory, the same single sign-on experience carries through.</span></span>


## <a name="install-the-application-proxy-connector-and-publish-your-app"></a><span data-ttu-id="a6b36-110">Az alkalmazásproxy-összekötő telepítése és az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="a6b36-110">Install the Application Proxy connector and publish your app</span></span>

<span data-ttu-id="a6b36-111">Ha még nem tette, [-Proxy konfigurálása a bérlő számára, és az összekötő telepítéséhez](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="a6b36-111">If you haven't already, [configure Application Proxy for your tenant and install the connector](active-directory-application-proxy-enable.md).</span></span> <span data-ttu-id="a6b36-112">Ezt követően [a helyszíni alkalmazások közzététele](application-proxy-publish-azure-portal.md) táveléréshez.</span><span class="sxs-lookup"><span data-stu-id="a6b36-112">Then, [publish your on-premises application](application-proxy-publish-azure-portal.md) for remote access.</span></span> <span data-ttu-id="a6b36-113">Az alkalmazás közzétételekor most jegyezze meg a külső URL-cím, mert a végfelhasználók számára, hogy információra van szükség, amikor azok az alkalmazás hozzáadása csoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="a6b36-113">When you're publishing the app, make note of the external URL because your end users need that information when they add the app to Teams.</span></span>

<span data-ttu-id="a6b36-114">Ha már rendelkezik a közzétett alkalmazásokhoz, de nem emlékszik a külső URL-címeit, keresse meg azokat a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a6b36-114">If you already have your apps published but don't remember their external URLs, look them up in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="a6b36-115">Jelentkezzen be, majd navigáljon a **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás** > Válassza ki az alkalmazást > **alkalmazásproxy**.</span><span class="sxs-lookup"><span data-stu-id="a6b36-115">Sign in, then navigate to **Azure Active Directory** > **Enterprise applications** > **All applications** > select your app > **Application proxy**.</span></span>

## <a name="add-your-app-to-teams"></a><span data-ttu-id="a6b36-116">Az alkalmazás hozzáadása csoportokhoz</span><span class="sxs-lookup"><span data-stu-id="a6b36-116">Add your app to Teams</span></span>

<span data-ttu-id="a6b36-117">Ha az alkalmazás-proxyn keresztül történő közzétételéhez tudassa a felhasználókkal, tudja, hogy azok is megadhatja azt a lapján közvetlenül a csapatok csatornák.</span><span class="sxs-lookup"><span data-stu-id="a6b36-117">Once you publish the app through Application Proxy, let your users know that they can add it as a tab directly in their Teams channels.</span></span> <span data-ttu-id="a6b36-118">Kövesse a fenti három lépést rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="a6b36-118">Have them follow these three steps:</span></span>

1. <span data-ttu-id="a6b36-119">Keresse meg a csoportok csatorna, vegye fel ezt az alkalmazást, illetve megadhatja, ahová  **+**  hozzáadása egy lapon.</span><span class="sxs-lookup"><span data-stu-id="a6b36-119">Navigate to the Teams channel where you want to add this app and select **+** to add a tab.</span></span>

   ![Válassza a lap](./media/application-proxy-teams/add-tab.png)

2. <span data-ttu-id="a6b36-121">Válassza ki **webhely** az a beállítások lapon.</span><span class="sxs-lookup"><span data-stu-id="a6b36-121">Select **Website** from the tab options.</span></span>

   ![Webhely hozzáadása](./media/application-proxy-teams/website.png)

3. <span data-ttu-id="a6b36-123">Nevezze el a lapot, és állítsa be az URL-címet az alkalmazásproxy külső URL-címet.</span><span class="sxs-lookup"><span data-stu-id="a6b36-123">Give the tab a name and set the URL to the Application Proxy external URL.</span></span> 

   ![Lap neve és az URL-cím konfigurálása](./media/application-proxy-teams/tab-name-url.png)

<span data-ttu-id="a6b36-125">A csoport egyik tagjának hozzáadja a lapon, ha azt jeleníti meg a csatorna mindenki számára.</span><span class="sxs-lookup"><span data-stu-id="a6b36-125">Once one member of a team adds the tab, it shows up for everyone in the channel.</span></span> <span data-ttu-id="a6b36-126">Az alkalmazáshoz hozzáféréssel rendelkező felhasználók egyszeri bejelentkezéses hozzáférést a Microsoft Teams használt hitelesítő adatokkal beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a6b36-126">Any users who have access to the app get single sign-on access with the credentials they use for Microsoft Teams.</span></span> <span data-ttu-id="a6b36-127">Minden hozzáférése az alkalmazáshoz nem rendelkező felhasználók csoportban lapon megjelenik, de le vannak tiltva, amíg nem adja meg őket engedélyeket a helyszíni alkalmazásra és az alkalmazás az Azure portálon közzétett verziója.</span><span class="sxs-lookup"><span data-stu-id="a6b36-127">Any users who don't have access to the app will see the tab in Teams, but are blocked until you give them permissions to the on-premises app and the Azure portal published version of the app.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a6b36-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6b36-128">Next steps</span></span>

- <span data-ttu-id="a6b36-129">Megtudhatja, hogyan [tegye közzé a helyszíni SharePoint helyeket](application-proxy-enable-remote-access-sharepoint.md) az alkalmazásproxy.</span><span class="sxs-lookup"><span data-stu-id="a6b36-129">Learn how to [publish on-premises SharePoint sites](application-proxy-enable-remote-access-sharepoint.md) with Application Proxy.</span></span>
- <span data-ttu-id="a6b36-130">Az alkalmazások használatára konfigurálja [egyéni tartományok](active-directory-application-proxy-custom-domains.md) a külső URL-címhez.</span><span class="sxs-lookup"><span data-stu-id="a6b36-130">Configure your apps to use [custom domains](active-directory-application-proxy-custom-domains.md) for their external URL.</span></span> 
