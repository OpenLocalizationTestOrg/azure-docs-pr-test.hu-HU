---
title: "Az Azure Active Directory B2C: Egyéni attribútumok |} Microsoft Docs"
description: "Az egyéni attribútumok használata az Azure Active Directory B2C gyűjthet a felhasználókról"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 055ffb0a-197b-4716-8dad-1fd8a01e174f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 356aaeff3a78fc7b682d621e8e0de9312582b2fe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a><span data-ttu-id="e7eb0-103">Az Azure Active Directory B2C: Gyűjthet a felhasználókról egyéni attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="e7eb0-103">Azure Active Directory B2C: Use custom attributes to collect information about your consumers</span></span>
<span data-ttu-id="e7eb0-104">Azure Active Directory (Azure AD) B2C-címtárban tartalmaz egy beépített információk (attribútumok): az Utónév, Vezetéknév, város, irányítószámát és egyéb attribútumai.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-104">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of information (attributes): Given Name, Surname, City, Postal Code, and other attributes.</span></span> <span data-ttu-id="e7eb0-105">Azonban minden egyes felhasználók felé néző alkalmazás követelményei egyedi attribútumok, mi a fogyasztói gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-105">However, every consumer-facing application has unique requirements on what attributes to gather from consumers.</span></span> <span data-ttu-id="e7eb0-106">Az Azure AD B2C-ben kiterjesztheti az egyes felhasználói fiókjában tárolt attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-106">With Azure AD B2C, you can extend the set of attributes stored on each consumer account.</span></span> <span data-ttu-id="e7eb0-107">Létrehozhat egyéni attribútumok a [Azure-portálon](https://portal.azure.com/) és az előfizetési házirendek alább látható módon használhatja.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-107">You can create custom attributes on the [Azure portal](https://portal.azure.com/) and use it in your sign-up policies, as shown below.</span></span> <span data-ttu-id="e7eb0-108">Is olvasási és írási ezek az attribútumok használatával a [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e7eb0-108">You can also read and write these attributes by using the [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e7eb0-109">Egyéni attribútumok használatát [az Azure AD Graph API Directory-séma bővítményei](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span><span class="sxs-lookup"><span data-stu-id="e7eb0-109">Custom attributes use [Azure AD Graph API Directory Schema Extensions](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span></span>
> 
> 

## <a name="create-a-custom-attribute"></a><span data-ttu-id="e7eb0-110">Egyéni attribútum létrehozása</span><span class="sxs-lookup"><span data-stu-id="e7eb0-110">Create a custom attribute</span></span>
1. <span data-ttu-id="e7eb0-111">[Kövesse az alábbi lépéseket az Azure portálon a B2C funkciók panelje navigáljon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="e7eb0-111">[Follow these steps to navigate to the B2C features blade on the Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="e7eb0-112">Kattintson a **felhasználói attribútumok**.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-112">Click **User attributes**.</span></span>
3. <span data-ttu-id="e7eb0-113">A panel tetején kattintson a **+Add** (+Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-113">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="e7eb0-114">Adjon meg egy **neve** az egyéni attribútum (például "ShoeSize"), és szükség esetén egy **leírás**.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-114">Provide a **Name** for the custom attribute (for example, "ShoeSize") and optionally, a **Description**.</span></span> <span data-ttu-id="e7eb0-115">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-115">Click **Create**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e7eb0-116">Csak a "karakterlánc" **adattípus** jelenleg rendelkezésre áll.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-116">Only the "String" **Data Type** is currently available.</span></span>
   > 
   > 

<span data-ttu-id="e7eb0-117">Az egyéni attribútum már elérhető a közül **felhasználói attribútumok**, és az előfizetési házirendek használható.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-117">The custom attribute is now available in the list of **User attributes**, and for use in your sign-up policies.</span></span>

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a><span data-ttu-id="e7eb0-118">Egyéni attribútumot használja a regisztrációs szabályzatban</span><span class="sxs-lookup"><span data-stu-id="e7eb0-118">Use a custom attribute in your sign-up policy</span></span>
1. <span data-ttu-id="e7eb0-119">[Kövesse az alábbi lépéseket az Azure portálon a B2C funkciók panelje navigáljon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="e7eb0-119">[Follow these steps to navigate to the B2C features blade on the Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="e7eb0-120">Kattintson a **Regisztrálási szabályzatok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-120">Click **Sign-up policies**.</span></span>
3. <span data-ttu-id="e7eb0-121">Kattintson a regisztrációs szabályzatban (például "B2C_1_SiUp") való megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-121">Click your sign-up policy (for example, "B2C_1_SiUp") to open it.</span></span> <span data-ttu-id="e7eb0-122">Kattintson a **szerkesztése** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-122">Click **Edit** at the top of the blade.</span></span>
4. <span data-ttu-id="e7eb0-123">Kattintson a **regisztrációs attribútumokat** válassza ki az egyéni attribútum (például "ShoeSize").</span><span class="sxs-lookup"><span data-stu-id="e7eb0-123">Click **Sign-up attributes** and select the custom attribute (for example, "ShoeSize").</span></span> <span data-ttu-id="e7eb0-124">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-124">Click **OK**.</span></span>
5. <span data-ttu-id="e7eb0-125">Kattintson a **alkalmazási jogcímet** , és válassza ki az egyéni attribútumot.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-125">Click **Application claims** and select the custom attribute.</span></span> <span data-ttu-id="e7eb0-126">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-126">Click **OK**.</span></span>
6. <span data-ttu-id="e7eb0-127">Kattintson a **mentése** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-127">Click **Save** at the top of the blade.</span></span>

<span data-ttu-id="e7eb0-128">A házirend a "Futtatás most" szolgáltatás használatával ellenőrizze a felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-128">You can use the "Run now" feature on the policy to verify the consumer experience.</span></span> <span data-ttu-id="e7eb0-129">Inkább most "ShoeSize" című felhasználói regisztráció során gyűjtött attribútumok listáját, és látja, a jogkivonat vissza az alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-129">You should now see "ShoeSize" in the list of attributes collected during consumer sign-up, and see it in the token sent back to your application.</span></span>

## <a name="notes"></a><span data-ttu-id="e7eb0-130">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e7eb0-130">Notes</span></span>
* <span data-ttu-id="e7eb0-131">Előfizetési házirendek, valamint az egyéni attribútumok használhatók a regisztráció vagy bejelentkezés házirendek és Profilszerkesztési házirendek.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-131">Along with sign-up policies, custom attributes can also be used in sign-up or sign-in policies and profile editing policies.</span></span>
* <span data-ttu-id="e7eb0-132">Egy ismert korlátozás egyéni attribútum van.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-132">There is a known limitation of custom attributes.</span></span> <span data-ttu-id="e7eb0-133">Csak jön létre, bármely házirend használatban van, és nem, amikor hozzáadja a listája **felhasználói attribútumok**.</span><span class="sxs-lookup"><span data-stu-id="e7eb0-133">It is only created the first time it is used in any policy, and not when you add it to the list of **User attributes**.</span></span>

