---
title: "aaaHow tooAssign felhasználók tooapplications |} Microsoft Docs"
description: "Hogyan legyenek hozzárendelve felhasználók tooan alkalmazás az Ön bérelt szolgáltatásának ismertetése"
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
ms.openlocfilehash: 4df60c7d723140d0d1bbd6ba8b34aa4e762d1138
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-tooapplications"></a><span data-ttu-id="47146-103">Hogyan tooassign felhasználók tooapplications</span><span class="sxs-lookup"><span data-stu-id="47146-103">How tooassign users tooapplications</span></span>

<span data-ttu-id="47146-104">Ez a cikk segítséget toounderstand hogyan legyenek hozzárendelve felhasználók tooan alkalmazás az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="47146-104">This article help you toounderstand how users get assigned tooan application in your tenant.</span></span>

## <a name="how-do-users-get-assigned-tooan-application-in-azure-ad"></a><span data-ttu-id="47146-105">Hogyan tegye felhasználók legyenek hozzárendelve tooan alkalmazás az Azure AD?</span><span class="sxs-lookup"><span data-stu-id="47146-105">How do users get assigned tooan application in Azure AD?</span></span>

<span data-ttu-id="47146-106">A felhasználó tooaccess egy alkalmazást akkor először meg kell adni tooit valamilyen módon.</span><span class="sxs-lookup"><span data-stu-id="47146-106">For a user tooaccess an application, they must first be assigned tooit in some way.</span></span> <span data-ttu-id="47146-107">Hozzárendelés végzi el a rendszergazda, egy üzleti delegált, vagy bizonyos esetekben hello felhasználói magukat.</span><span class="sxs-lookup"><span data-stu-id="47146-107">Assignment can be performed by an administrator, a business delegate, or sometimes, hello user themselves.</span></span> <span data-ttu-id="47146-108">Alább ismerteti azokat a felhasználókat is legyenek hozzárendelve tooapplications hello módszereket:</span><span class="sxs-lookup"><span data-stu-id="47146-108">Below describes hello ways users can get assigned tooapplications:</span></span>

1.  <span data-ttu-id="47146-109">A rendszergazda [rendel egy felhasználói](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) közvetlenül toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="47146-109">An administrator [assigns a user](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) toohello application directly</span></span>

2.  <span data-ttu-id="47146-110">A rendszergazda [hozzárendel egy csoport](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) hello felhasználó tagja toohello alkalmazás, beleértve:</span><span class="sxs-lookup"><span data-stu-id="47146-110">An administrator [assigns a group](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) that hello user is a member of toohello application, including:</span></span>

  * <span data-ttu-id="47146-111">Egy csoport, szinkronizált helyszíni</span><span class="sxs-lookup"><span data-stu-id="47146-111">A group that was synchronized from on-premises</span></span>

  * <span data-ttu-id="47146-112">A létrehozott hello felhőben statikus biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="47146-112">A static security group created in hello cloud</span></span>

  * <span data-ttu-id="47146-113">A [dinamikus biztonsági csoport](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) hello felhő létrehozása</span><span class="sxs-lookup"><span data-stu-id="47146-113">A [dynamic security group](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) created in hello cloud</span></span>

  * <span data-ttu-id="47146-114">Az Office 365 hello felhő létrehozott csoport</span><span class="sxs-lookup"><span data-stu-id="47146-114">An Office 365 group created in hello cloud</span></span>

  * <span data-ttu-id="47146-115">Hello [minden felhasználó](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) csoport</span><span class="sxs-lookup"><span data-stu-id="47146-115">hello [All Users](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) group</span></span>

3.  <span data-ttu-id="47146-116">Lehetővé teszi a rendszergazda [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) egy felhasználó tooadd egy alkalmazást, amely hello tooallow [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **alkalmazás hozzáadása** szolgáltatás**üzleti jóváhagyása nélkül**</span><span class="sxs-lookup"><span data-stu-id="47146-116">An administrator enables [Self-service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow a user tooadd an application using hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Add App** feature **without business approval**</span></span>

4.  <span data-ttu-id="47146-117">Lehetővé teszi a rendszergazda [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) egy felhasználó tooadd egy alkalmazást, amely hello tooallow [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **alkalmazás hozzáadása** szolgáltatást, de csak w **edik előzetes jóváhagyási vállalati jóváhagyó egy kijelölt készletből**</span><span class="sxs-lookup"><span data-stu-id="47146-117">An administrator enables [Self-service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow a user tooadd an application using hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Add App** feature, but only w**ith prior approval from a selected set of business approvers**</span></span>

5.  <span data-ttu-id="47146-118">Lehetővé teszi a rendszergazda [önkiszolgáló csoportkezelési](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow egy felhasználó toojoin egy csoportot, amely az alkalmazás hozzá van rendelve túl**üzleti jóváhagyása nélkül**</span><span class="sxs-lookup"><span data-stu-id="47146-118">An administrator enables [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow a user toojoin a group that an application is assigned too**without business approval**</span></span>

6.  <span data-ttu-id="47146-119">Lehetővé teszi a rendszergazda [önkiszolgáló csoportkezelési](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow egy felhasználó toojoin egy csoportot, amely az alkalmazás hozzá van rendelve, a, de csak **vállalati jóváhagyó egy kijelölt készletből előzetes jóváhagyással**</span><span class="sxs-lookup"><span data-stu-id="47146-119">An administrator enables [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow a user toojoin a group that an application is assigned to, but only **with prior approval from a selected set of business approvers**</span></span>

7.  <span data-ttu-id="47146-120">A rendszergazda hozzárendeli a licenc tooa felhasználó közvetlenül az első gyártótól származó alkalmazás, például [Microsoft Office 365](http://products.office.com/)</span><span class="sxs-lookup"><span data-stu-id="47146-120">An administrator assigns a license tooa user directly for a first party application, like [Microsoft Office 365](http://products.office.com/)</span></span>

8.  <span data-ttu-id="47146-121">Egy rendszergazda rendel, amely a felhasználó hello tooa licenccsoport tagja tooa első gyártótól származó alkalmazás, például a [Microsoft Office 365](http://products.office.com/)</span><span class="sxs-lookup"><span data-stu-id="47146-121">An administrator assigns a license tooa group that hello user is a member of tooa first party application, like [Microsoft Office 365](http://products.office.com/)</span></span>

9.  <span data-ttu-id="47146-122">Egy [rendszergazda beleegyezik tooan alkalmazás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toobe használt minden felhasználó és a felhasználó bejelentkezik majd toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="47146-122">An [administrator consents tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toobe used by all users and then a user signs in toohello application</span></span>

10. <span data-ttu-id="47146-123">A felhasználó [tooan alkalmazás beleegyezik](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) magukat a bejelentkezéssel toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="47146-123">A user [consents tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) themselves by signing in toohello application</span></span>

## <a name="next-steps"></a><span data-ttu-id="47146-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47146-124">Next steps</span></span>
[<span data-ttu-id="47146-125">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="47146-125">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
