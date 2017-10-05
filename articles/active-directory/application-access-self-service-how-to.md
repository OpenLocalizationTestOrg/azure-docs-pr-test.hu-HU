---
title: "Önkiszolgáló alkalmazás-hozzárendelés konfigurálása |} Microsoft Docs"
description: "A felhasználók a saját alkalmazások keresése az önkiszolgáló alkalmazás hozzáférésének engedélyezése"
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
ms.openlocfilehash: 7991dc19d41c5eb8e149c3ee08069e1a162929cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-self-service-application-assignment"></a><span data-ttu-id="6c0c5-103">Önkiszolgáló alkalmazás-hozzárendelés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c0c5-103">How to configure self-service application assignment</span></span>

<span data-ttu-id="6c0c5-104">Mielőtt a felhasználók saját maguk felderíthetők az alkalmazások a hozzáférési panelen, engedélyeznie kell **önkiszolgáló alkalmazás-hozzáférés** olyan alkalmazásokat, amelyek önálló felderítését, és kérjen engedélyezni szeretné a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-104">Before your users can self-discover applications from their access panel, you need to enable **Self-service application access** to any applications that you wish to allow users to self-discover and request access to.</span></span>

<span data-ttu-id="6c0c5-105">Ez a szolgáltatás ahhoz, hogy időt és pénzt elmentse az IT-részleg remek mód, és erősen ajánlott az Azure Active Directoryban a modern alkalmazások központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-105">This feature is a great way for you to save time and money as an IT group, and is highly recommended as part of a modern applications deployment with Azure Active Directory.</span></span>

<span data-ttu-id="6c0c5-106">Ez a szolgáltatás használatával:</span><span class="sxs-lookup"><span data-stu-id="6c0c5-106">Using this feature, you can:</span></span>

-   <span data-ttu-id="6c0c5-107">Önálló felderítése az alkalmazások a felhasználók a [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) az IT-részleg bothering nélkül.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-107">Let users self-discover applications from the [Application Access Panel](https://myapps.microsoft.com/) without bothering the IT group.</span></span>

-   <span data-ttu-id="6c0c5-108">Ezek a felhasználók hozzáadása az előre konfigurált csoporthoz, tekintse meg, akik hozzáférést kér, távolítsa el a hozzáférést, és a hozzájuk rendelt szerepkörök kezelése.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-108">Add those users to a pre-configured group so you can see who has requested access, remove access, and manage the roles assigned to them.</span></span>

-   <span data-ttu-id="6c0c5-109">Alkalmazás-hozzáférési kérelmek jóváhagyása, így nem kell az IT-részleg a vállalati jóváhagyó opcionálisan engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-109">Optionally allow a business approver to approve application access requests so the IT group doesn’t have to.</span></span>

-   <span data-ttu-id="6c0c5-110">Választható lehetőségként konfigurálhatja legfeljebb 10 egyéni felhasználók számára, akik jóváhagyhatja az alkalmazáshoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-110">Optionally configure up to 10 individuals who may approve access to this application.</span></span>

-   <span data-ttu-id="6c0c5-111">Opcionálisan engedélyezése a vállalati jóváhagyó beállítása a jelszavak azoknak a felhasználóknak használatával jelentkezzen be az alkalmazás közvetlenül a vállalati jóváhagyó a [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="6c0c5-111">Optionally allow a business approver to set the passwords those users can use to sign in to the application, right from the business approver’s [Application Access Panel](https://myapps.microsoft.com/).</span></span>

-   <span data-ttu-id="6c0c5-112">Nem kötelező automatikus hozzárendelése közvetlenül egy alkalmazás szerepkörhöz hozzárendelt felhasználók önkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-112">Optionally automatically assign self-service assigned users to an application role directly.</span></span>

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a><span data-ttu-id="6c0c5-113">A felhasználók a saját alkalmazások keresése az önkiszolgáló alkalmazás hozzáférésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6c0c5-113">Enable self-service application access to allow users to find their own applications</span></span>

<span data-ttu-id="6c0c5-114">Önkiszolgáló alkalmazás-hozzáférés kiváló módja annak, hogy önálló felderítéséhez az alkalmazások, felhasználók igény szerint engedélyezett az üzleti csoport ezeket az alkalmazásokat a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-114">Self-service application access is a great way to allow users to self-discover applications, optionally allow the business group to approve access to those applications.</span></span> <span data-ttu-id="6c0c5-115">Engedélyezze az üzleti csoport társítva a hozzáférési panel azoknak a felhasználóknak jelszót egyszeri bejelentkezést az alkalmazások jobb a hitelesítő adatok kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-115">You can allow the business group to manage the credentials assigned to those users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="6c0c5-116">Ahhoz, hogy az alkalmazás önkiszolgáló hozzáférést egy alkalmazáshoz, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6c0c5-116">To enable self-service application access to an application, follow the steps below:</span></span>

1.  <span data-ttu-id="6c0c5-117">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="6c0c5-117">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6c0c5-118">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-118">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6c0c5-119">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-119">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6c0c5-120">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-120">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6c0c5-121">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-121">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="6c0c5-122">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="6c0c5-122">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6c0c5-123">Válassza ki az önkiszolgáló engedélyezni szeretné a hozzáférést a listából.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-123">Select the application you want to enable Self-service access to from the list.</span></span>

7.  <span data-ttu-id="6c0c5-124">Ha az alkalmazás betölt, kattintson **önkiszolgáló** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-124">Once the application loads, click **Self-service** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6c0c5-125">Ahhoz, hogy az önkiszolgáló alkalmazás hozzáférése az alkalmazáshoz, kapcsolja be a **az alkalmazáshoz való hozzáférés kérését?** kapcsolót **igen.**</span><span class="sxs-lookup"><span data-stu-id="6c0c5-125">To enable Self-service application access for this application, turn the **Allow users to request access to this application?** toggle to **Yes.**</span></span>

9.  <span data-ttu-id="6c0c5-126">A következő, amelyekhez a felhasználók, akik kérnek az alkalmazáshoz való hozzáférés hozzá kell adni a csoport kijelöléséhez kattintson a felirat melletti választó **mely csoporthoz rendelt felhasználók vehet fel?** válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-126">Next, to select the group to which users who request access to this application should be added, click the selector next to the label **To which group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="6c0c5-127">**Választható lehetőség:** előtt egy üzleti jóváhagyás megkövetelése, ha a felhasználók jogosultak-e a hozzáférést, és állítsa a **az alkalmazáshoz való hozzáférés megadása előtt jóvá kell hagyni?** kapcsolót **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-127">**Optional:** If you wish to require a business approval before users are allowed access, set the **Require approval before granting access to this application?** toggle to **Yes**.</span></span>

11. <span data-ttu-id="6c0c5-128">**Választható lehetőség: az alkalmazások csak a jelszó egyszeri bejelentkezés használatával** adott üzleti jóváhagyóknak adhatja meg a jelszavakat, a jóváhagyott felhasználók számára az alkalmazás küldött engedélyezni szeretné, ha a **engedélyezése az alkalmazás felhasználói jelszavak beállítása jóváhagyóknak?** kapcsolót **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-128">**Optional: For applications using password single-sign on only,** if you wish to allow those business approvers to specify the passwords that are sent to this application for approved users, set the **Allow approvers to set user’s passwords for this application?** toggle to **Yes**.</span></span>

12. <span data-ttu-id="6c0c5-129">**Választható lehetőség:** az üzleti jóváhagyóknak, akik jogosultak a hagyja jóvá az alkalmazáshoz való hozzáférés megadásához kattintson a felirat melletti választó **ki jogosult az alkalmazáshoz való hozzáférés jóváhagyásához?** legfeljebb 10 egyéni üzleti jóváhagyóknak kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-129">**Optional:** To specify the business approvers who are allowed to approve access to this application, click the selector next to the label **Who is allowed to approve access to this application?** to select up to 10 individual business approvers.</span></span>

   >[!NOTE]
   ><span data-ttu-id="6c0c5-130">Csoportok használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-130">Groups are not supported.</span></span>
   >
   >

13. <span data-ttu-id="6c0c5-131">**Választható lehetőség:** **az alkalmazások, amelyeknél a szerepkörök**, ha önkiszolgáló jóváhagyott felhasználók hozzárendelése egy szerepkörhöz, kattintson a Tovább gombra a választó a **mely szerepkörhöz felhasználók hozzárendeli az alkalmazásban?** válassza ki a szerepkört, amelyhez hozzá kell rendelni a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-131">**Optional:** **For applications which expose roles**, if you wish to assign self-service approved users to a role, click the selector next to the **To which role should users be assigned in this application?** to select the role to which these users should be assigned.</span></span>

14. <span data-ttu-id="6c0c5-132">Kattintson a **mentése** gombra a befejezéshez panel tetején.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-132">Click the **Save** button at the top of the blade to finish.</span></span>

<span data-ttu-id="6c0c5-133">Ha befejezte az önkiszolgáló Alkalmazáskonfiguráció, felhasználók lépjen a [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) , és kattintson a **+ Hozzáadás** gombra kattintva keresse meg az alkalmazások, amelyhez engedélyezte a hozzáférést az önkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-133">Once you complete Self-service application configuration, users can navigate to their [Application Access Panel](https://myapps.microsoft.com/) and click the **+Add** button to find the apps to which you have enabled Self-service access.</span></span> <span data-ttu-id="6c0c5-134">Üzleti jóváhagyóknak is megjelenik egy értesítés a saját [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="6c0c5-134">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="6c0c5-135">Egy e-mailt, amely értesíti őket, amikor a felhasználó által kért a jóváhagyást igénylő alkalmazáshoz való hozzáférés engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-135">You can enable an email notifying them when a user has requested access to an application that requires their approval.</span></span> 

<span data-ttu-id="6c0c5-136">Ezek a jóváhagyások támogatja egyetlen jóváhagyási munkafolyamatok csak, ami azt jelenti, hogy több jóváhagyó ad meg, ha bármely egyetlen jóváhagyó előfordulhat, hogy jóváhagyó hozzáférni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-136">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access to the application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c0c5-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c0c5-137">Next steps</span></span>
[<span data-ttu-id="6c0c5-138">Azure Active Directory beállítása önkiszolgáló csoportkezelés</span><span class="sxs-lookup"><span data-stu-id="6c0c5-138">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
