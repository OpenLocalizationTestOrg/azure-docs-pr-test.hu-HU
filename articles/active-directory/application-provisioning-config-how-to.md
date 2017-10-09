---
title: "aaaHow tooconfigure felhasználók átadásához tooan az Azure AD-gyűjtemény alkalmazás |} Microsoft Docs"
description: "Gazdag felhasználói fiók kiépítésének és megszüntetésének biztosítása már szerepel az Azure AD Application Gallery hello tooapplications gyors konfigurálásához"
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
ms.openlocfilehash: 2c28e59a3ac8f221ed93b2f6b0b1221f7604af23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-user-provisioning-tooan-azure-ad-gallery-application"></a><span data-ttu-id="b32c9-103">Hogyan tooconfigure felhasználói tooan az Azure AD-gyűjtemény alkalmazás kiépítése</span><span class="sxs-lookup"><span data-stu-id="b32c9-103">How tooconfigure user provisioning tooan Azure AD Gallery application</span></span>

<span data-ttu-id="b32c9-104">*Felhasználói fiók kiépítése* hello act létrehozása, frissítése, illetve letiltása a felhasználói fiókot rögzíti az alkalmazás helyi felhasználói profil tárolójában van.</span><span class="sxs-lookup"><span data-stu-id="b32c9-104">*User account provisioning* is hello act of creating, updating, and/or disabling user account records in an application’s local user profile store.</span></span> <span data-ttu-id="b32c9-105">A legtöbb felhő- és SaaS-alkalmazásokhoz tárolása hello felhasználók szerepköröket és engedélyeket a saját helyi felhasználói profil tároló, valamint ilyen felhasználói rekordban a helyi tárolóban levő jelenléte *szükséges* az egyszeri bejelentkezés és a hozzáférés toowork.</span><span class="sxs-lookup"><span data-stu-id="b32c9-105">Most cloud and SaaS applications store hello users role and permissions in their own local user profile store, and presence of such a user record in their local store is *required* for single sign-on and access toowork.</span></span>

<span data-ttu-id="b32c9-106">Hello Azure-portálon, a hello **kiépítési** egy vállalati alkalmazást jeleníti meg, milyen üzembe helyezési mód támogatja az alkalmazás hello bal oldali navigációs panelen lapján.</span><span class="sxs-lookup"><span data-stu-id="b32c9-106">In hello Azure portal, hello **Provisioning** tab in hello left navigation pane for an Enterprise App displays what provisioning modes are supported for that app.</span></span> <span data-ttu-id="b32c9-107">Ez a két értékek egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="b32c9-107">This can be one of two values:</span></span>

## <a name="configuring-an-application-for-manual-provisioning"></a><span data-ttu-id="b32c9-108">Alkalmazások konfigurálása a manuális üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="b32c9-108">Configuring an application for Manual Provisioning</span></span>

<span data-ttu-id="b32c9-109">*Manuális* kiépítés azt jelenti, hogy felhasználói fiókokat kell létrehozni az alkalmazás által biztosított hello módszerek segítségével manuálisan.</span><span class="sxs-lookup"><span data-stu-id="b32c9-109">*Manual* provisioning means that user accounts must be created manually using hello methods provided by that app.</span></span> <span data-ttu-id="b32c9-110">Ez azt jelentheti, hogy az alkalmazáshoz egy felügyeleti portálra való bejelentkezéskor, és a webes felhasználói felülete segítségével a felhasználók hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="b32c9-110">This could mean logging into an administrative portal for that app and adding users using a web-based user interface.</span></span> <span data-ttu-id="b32c9-111">Vagy az sikerült feltölteni egy táblázatot a felhasználói fiók részletes, egy adott alkalmazás által biztosított mechanizmus használatával.</span><span class="sxs-lookup"><span data-stu-id="b32c9-111">Or it could be uploading a spreadsheet with user account detail, using a mechanism provided by that application.</span></span> <span data-ttu-id="b32c9-112">Dokumentációjában talál hello megadott szerint hello alkalmazást, illetve kapcsolattartási hello alkalmazást fejlesztői toodetermine Nyugat-afrikai mechanizmusok érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b32c9-112">Consult hello documentation provided by hello app, or contact hello app developer toodetermine wat mechanisms are available.</span></span>

<span data-ttu-id="b32c9-113">Ha manuális hello egyetlen mód jelenik meg egy adott alkalmazáshoz, az azt jelenti, hogy nincs automatikus az Azure AD összekötő kiépítése még létrehozva hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b32c9-113">If Manual is hello only mode shown for a given application, it means that no automatic Azure AD provisioning connector has been created for hello app yet.</span></span> <span data-ttu-id="b32c9-114">Vagy az azt jelenti, hogy hello alkalmazás nem nem támogatási hello működéséhez szükséges felhasználói felügyeleti API mely toobuild követően az automatikus létesítési csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="b32c9-114">Or it means hello app does not support hello pre-requisite user management API upon which toobuild an automated provisioning connector.</span></span>

<span data-ttu-id="b32c9-115">Ha szeretné, hogy egy adott alkalmazás automatikus kiépítés toorequest támogatása, a kérelem kitöltheti <http://aka.ms/aadapprequest>.</span><span class="sxs-lookup"><span data-stu-id="b32c9-115">If you would like toorequest support for automatic provisioning for a given app, you can fill out a request at <http://aka.ms/aadapprequest>.</span></span>

## <a name="configuring-an-application-for-automatic-provisioning"></a><span data-ttu-id="b32c9-116">Alkalmazások konfigurálása az Automatikus kiépítés</span><span class="sxs-lookup"><span data-stu-id="b32c9-116">Configuring an application for Automatic Provisioning</span></span>

<span data-ttu-id="b32c9-117">*Automatikus* azt jelenti, hogy egy összekötő kiépítése az Azure AD identitáskezelési ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b32c9-117">*Automatic* means that an Azure AD provisioning connector has been developed for this application.</span></span> <span data-ttu-id="b32c9-118">További információ a hello kiépítése szolgáltatáshoz, és hogyan működik, az Azure AD: [Felhasználókiépítés és -megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span><span class="sxs-lookup"><span data-stu-id="b32c9-118">For more information on hello Azure AD provisioning service and how it works, see [Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span></span>

<span data-ttu-id="b32c9-119">További információ a hogyan tooprovision bizonyos felhasználók és csoportok tooan alkalmazás: [kezelése a felhasználói fiók kiépítése vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span><span class="sxs-lookup"><span data-stu-id="b32c9-119">For more information on how tooprovision specific users and groups tooan application, see [Managing user account provisioning for enterprise apps](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span></span>

<span data-ttu-id="b32c9-120">hello szükséges tooenable tényleges lépéseket, és állítsa be automatikus kiépítés hello alkalmazástól függően változhat.</span><span class="sxs-lookup"><span data-stu-id="b32c9-120">hello actual steps required tooenable and configure automatic provisioning vary depending on hello application.</span></span>

>[!NOTE]
><span data-ttu-id="b32c9-121">Először meg kell válaszolnia hello beállítása adott oktatóanyag toosetting be az alkalmazás telepítése, és ezeket a lépéseket tooconfigure következő hello alkalmazás, mind az Azure AD toocreate hello létesítési kapcsolat keresése.</span><span class="sxs-lookup"><span data-stu-id="b32c9-121">You should start by finding hello setup tutorial specific toosetting up provisioning for your application, and following those steps tooconfigure both hello app and Azure AD toocreate hello provisioning connection.</span></span> 
>
>

<span data-ttu-id="b32c9-122">Alkalmazás oktatóprogramok találhatók [oktatóanyagok listáját hogyan tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span><span class="sxs-lookup"><span data-stu-id="b32c9-122">App tutorials can be found at [List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span></span>

<span data-ttu-id="b32c9-123">Ha kiépítés beállítása tooreview egy fontos tooconsider hello attribútum-leképezésekhez és munkafolyamatok, amelyek meghatározzák, milyen felhasználói (vagy a csoport) tulajdonságok folyamata az Azure AD toohello alkalmazásból és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b32c9-123">An important thing tooconsider when setting up provisioning be tooreview and configure hello attribute mappings and workflows that define which user (or group) properties flow from Azure AD toohello application.</span></span> <span data-ttu-id="b32c9-124">Ez magában foglalja, hello "egyező tulajdonság" beállítása használt toouniquely kell azonosítani és felel meg a felhasználók/csoportok hello két rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="b32c9-124">This includes setting hello “matching property” that be used toouniquely identify and match users/groups between hello two systems.</span></span> <span data-ttu-id="b32c9-125">További információ a fontos folyamatban.</span><span class="sxs-lookup"><span data-stu-id="b32c9-125">For more information on this important process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b32c9-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b32c9-126">Next steps</span></span>
[<span data-ttu-id="b32c9-127">Attribútum-leképezésekhez kiépítés az SaaS-alkalmazásokhoz az Azure Active Directory felhasználói testreszabása</span><span class="sxs-lookup"><span data-stu-id="b32c9-127">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

