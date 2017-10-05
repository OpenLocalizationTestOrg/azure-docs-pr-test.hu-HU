---
title: "A felhasználók átadása egy Azure AD-katalógusában alkalmazás konfigurálása |} Microsoft Docs"
description: "Gazdag felhasználói fiók kiépítésének és megszüntetésének biztosítása már szerepel az Azure AD Application Gallery alkalmazások gyors konfigurálásához"
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
ms.openlocfilehash: 2e38fcb30ea7632339a3ba8815a536872cfcc69e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-user-provisioning-to-an-azure-ad-gallery-application"></a><span data-ttu-id="9bc1a-103">A felhasználók átadása egy Azure AD-katalógusában alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bc1a-103">How to configure user provisioning to an Azure AD Gallery application</span></span>

<span data-ttu-id="9bc1a-104">*Felhasználói fiók kiépítése* történő létrehozása, frissítése és/vagy letiltása a felhasználói fiókot rögzíti az alkalmazás helyi felhasználói profil tárolójában van.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-104">*User account provisioning* is the act of creating, updating, and/or disabling user account records in an application’s local user profile store.</span></span> <span data-ttu-id="9bc1a-105">Legtöbb felhő- és SaaS-alkalmazásokhoz tárolja a felhasználói szerepköröket és engedélyeket a saját helyi felhasználói profil tárolójában, és ilyen felhasználói rekordban a helyi tárolóban levő jelenléte *szükséges* egyszeri bejelentkezést és a hozzáférés működéséhez.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-105">Most cloud and SaaS applications store the users role and permissions in their own local user profile store, and presence of such a user record in their local store is *required* for single sign-on and access to work.</span></span>

<span data-ttu-id="9bc1a-106">Az Azure portálon a **kiépítési** egy vállalati alkalmazást jeleníti meg, milyen üzembe helyezési mód támogatja az alkalmazás a bal oldali navigációs panelen lapján.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-106">In the Azure portal, the **Provisioning** tab in the left navigation pane for an Enterprise App displays what provisioning modes are supported for that app.</span></span> <span data-ttu-id="9bc1a-107">Ez a két értékek egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="9bc1a-107">This can be one of two values:</span></span>

## <a name="configuring-an-application-for-manual-provisioning"></a><span data-ttu-id="9bc1a-108">Alkalmazások konfigurálása a manuális üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="9bc1a-108">Configuring an application for Manual Provisioning</span></span>

<span data-ttu-id="9bc1a-109">*Manuális* kiépítés azt jelenti, hogy felhasználói fiókokat kell létrehozni az alkalmazás által biztosított módszerek segítségével manuálisan.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-109">*Manual* provisioning means that user accounts must be created manually using the methods provided by that app.</span></span> <span data-ttu-id="9bc1a-110">Ez azt jelentheti, hogy az alkalmazáshoz egy felügyeleti portálra való bejelentkezéskor, és a webes felhasználói felülete segítségével a felhasználók hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-110">This could mean logging into an administrative portal for that app and adding users using a web-based user interface.</span></span> <span data-ttu-id="9bc1a-111">Vagy az sikerült feltölteni egy táblázatot a felhasználói fiók részletes, egy adott alkalmazás által biztosított mechanizmus használatával.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-111">Or it could be uploading a spreadsheet with user account detail, using a mechanism provided by that application.</span></span> <span data-ttu-id="9bc1a-112">Az alkalmazás által biztosított dokumentációt, vagy lépjen kapcsolatba az alkalmazás fejlesztőjének annak meghatározásához, Nyugat-afrikai mechanizmusok érhetők el.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-112">Consult the documentation provided by the app, or contact the app developer to determine wat mechanisms are available.</span></span>

<span data-ttu-id="9bc1a-113">Ha manuális az egyetlen mód egy adott alkalmazás látható, az azt jelenti, hogy nincs automatikus az Azure AD összekötő kiépítése még létrehozva az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-113">If Manual is the only mode shown for a given application, it means that no automatic Azure AD provisioning connector has been created for the app yet.</span></span> <span data-ttu-id="9bc1a-114">Vagy az azt jelenti, hogy az alkalmazás nem támogatja a működéséhez szükséges felhasználói API szerint az automatikus létesítési összekötő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-114">Or it means the app does not support the pre-requisite user management API upon which to build an automated provisioning connector.</span></span>

<span data-ttu-id="9bc1a-115">Ha azt szeretné, a megadott alkalmazások automatikus kiépítés támogatás kéréséhez, a kérelem kitöltheti <http://aka.ms/aadapprequest>.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-115">If you would like to request support for automatic provisioning for a given app, you can fill out a request at <http://aka.ms/aadapprequest>.</span></span>

## <a name="configuring-an-application-for-automatic-provisioning"></a><span data-ttu-id="9bc1a-116">Alkalmazások konfigurálása az Automatikus kiépítés</span><span class="sxs-lookup"><span data-stu-id="9bc1a-116">Configuring an application for Automatic Provisioning</span></span>

<span data-ttu-id="9bc1a-117">*Automatikus* azt jelenti, hogy egy összekötő kiépítése az Azure AD identitáskezelési ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-117">*Automatic* means that an Azure AD provisioning connector has been developed for this application.</span></span> <span data-ttu-id="9bc1a-118">Az Azure ad-val kiépítése szolgáltatáshoz, és annak működéséről további információkért lásd: [Felhasználókiépítés és -megszüntetés automatizálása a SaaS-alkalmazásokhoz az Azure Active Directoryval](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span><span class="sxs-lookup"><span data-stu-id="9bc1a-118">For more information on the Azure AD provisioning service and how it works, see [Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).</span></span>

<span data-ttu-id="9bc1a-119">A meghatározott felhasználókhoz és csoportokhoz alkalmazáshoz való kiépítése további információkért lásd: [kezelése a felhasználói fiók kiépítése vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span><span class="sxs-lookup"><span data-stu-id="9bc1a-119">For more information on how to provision specific users and groups to an application, see [Managing user account provisioning for enterprise apps](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).</span></span>

<span data-ttu-id="9bc1a-120">Engedélyezze és konfigurálja az Automatikus kiépítés tényleges lépéseit az alkalmazástól függően eltérőek.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-120">The actual steps required to enable and configure automatic provisioning vary depending on the application.</span></span>

>[!NOTE]
><span data-ttu-id="9bc1a-121">Először meg kell válaszolnia a telepítő az oktatóanyag az alkalmazás üzembe helyezési, és a következő azokat az alkalmazás és az üzembe helyezési kapcsolat létrehozása az Azure AD konfigurálása lépésekkel beállítására vonatkozó keresése.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-121">You should start by finding the setup tutorial specific to setting up provisioning for your application, and following those steps to configure both the app and Azure AD to create the provisioning connection.</span></span> 
>
>

<span data-ttu-id="9bc1a-122">Alkalmazás oktatóprogramok találhatók [integrálhatja SaaS-alkalmazásokhoz az Azure Active Directoryval kapcsolatos lista a bemutatók](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span><span class="sxs-lookup"><span data-stu-id="9bc1a-122">App tutorials can be found at [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span></span>

<span data-ttu-id="9bc1a-123">Kiépítés beállítása során figyelembe kell venni egy fontos dolog lehet áttekintése és konfigurálása a attribútum-leképezésekhez és a munkafolyamatok, amelyek meghatározzák, milyen felhasználói (vagy a csoport) tulajdonságok folyamata az Azure AD az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-123">An important thing to consider when setting up provisioning be to review and configure the attribute mappings and workflows that define which user (or group) properties flow from Azure AD to the application.</span></span> <span data-ttu-id="9bc1a-124">Ez magában foglalja a "egyező property" beállítás használható egyedileg azonosíthatja és felel meg a felhasználókat/csoportokat a két rendszer között.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-124">This includes setting the “matching property” that be used to uniquely identify and match users/groups between the two systems.</span></span> <span data-ttu-id="9bc1a-125">További információ a fontos folyamatban.</span><span class="sxs-lookup"><span data-stu-id="9bc1a-125">For more information on this important process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9bc1a-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9bc1a-126">Next steps</span></span>
[<span data-ttu-id="9bc1a-127">Attribútum-leképezésekhez kiépítés az SaaS-alkalmazásokhoz az Azure Active Directory felhasználói testreszabása</span><span class="sxs-lookup"><span data-stu-id="9bc1a-127">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

