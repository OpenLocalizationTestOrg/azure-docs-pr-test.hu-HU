---
title: "Az Azure Active Directory B2C: Egyéni attribútumok |} Microsoft Docs"
description: "Hogyan toouse egyéni attribútumok a toocollect Azure Active Directory B2C a felhasználókról"
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
ms.openlocfilehash: fb1bff77ad54c246c6d2f69f39c03eafb76fe6bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-toocollect-information-about-your-consumers"></a><span data-ttu-id="8ce04-103">Az Azure Active Directory B2C: Toocollect egyéni attribútumok a felhasználókról használata</span><span class="sxs-lookup"><span data-stu-id="8ce04-103">Azure Active Directory B2C: Use custom attributes toocollect information about your consumers</span></span>
<span data-ttu-id="8ce04-104">Azure Active Directory (Azure AD) B2C-címtárban tartalmaz egy beépített információk (attribútumok): az Utónév, Vezetéknév, város, irányítószámát és egyéb attribútumai.</span><span class="sxs-lookup"><span data-stu-id="8ce04-104">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of information (attributes): Given Name, Surname, City, Postal Code, and other attributes.</span></span> <span data-ttu-id="8ce04-105">Minden egyes felhasználók felé néző alkalmazás azonban egyedi követelményeket támaszt a fogyasztói milyen attribútumok toogather.</span><span class="sxs-lookup"><span data-stu-id="8ce04-105">However, every consumer-facing application has unique requirements on what attributes toogather from consumers.</span></span> <span data-ttu-id="8ce04-106">Az Azure AD B2C-ben minden felhasználói fiókhoz tárolt attribútumok készletét hello bővítheti.</span><span class="sxs-lookup"><span data-stu-id="8ce04-106">With Azure AD B2C, you can extend hello set of attributes stored on each consumer account.</span></span> <span data-ttu-id="8ce04-107">Létrehozhat egyéni attribútumok hello [Azure-portálon](https://portal.azure.com/) és az előfizetési házirendek alább látható módon használhatja.</span><span class="sxs-lookup"><span data-stu-id="8ce04-107">You can create custom attributes on hello [Azure portal](https://portal.azure.com/) and use it in your sign-up policies, as shown below.</span></span> <span data-ttu-id="8ce04-108">Is olvasási és írási ezek az attribútumok hello segítségével [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="8ce04-108">You can also read and write these attributes by using hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

> [!NOTE]
> <span data-ttu-id="8ce04-109">Egyéni attribútumok használatát [az Azure AD Graph API Directory-séma bővítményei](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ce04-109">Custom attributes use [Azure AD Graph API Directory Schema Extensions](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span></span>
> 
> 

## <a name="create-a-custom-attribute"></a><span data-ttu-id="8ce04-110">Egyéni attribútum létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ce04-110">Create a custom attribute</span></span>
1. <span data-ttu-id="8ce04-111">[Kövesse a lépéseket toonavigate toohello B2C funkciók panelje hello Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="8ce04-111">[Follow these steps toonavigate toohello B2C features blade on hello Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="8ce04-112">Kattintson a **felhasználói attribútumok**.</span><span class="sxs-lookup"><span data-stu-id="8ce04-112">Click **User attributes**.</span></span>
3. <span data-ttu-id="8ce04-113">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="8ce04-113">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="8ce04-114">Adjon meg egy **neve** hello egyéni attribútum (például "ShoeSize"), és szükség esetén egy **leírás**.</span><span class="sxs-lookup"><span data-stu-id="8ce04-114">Provide a **Name** for hello custom attribute (for example, "ShoeSize") and optionally, a **Description**.</span></span> <span data-ttu-id="8ce04-115">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8ce04-115">Click **Create**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8ce04-116">Csak a "String" hello **adattípus** jelenleg rendelkezésre áll.</span><span class="sxs-lookup"><span data-stu-id="8ce04-116">Only hello "String" **Data Type** is currently available.</span></span>
   > 
   > 

<span data-ttu-id="8ce04-117">hello egyéni attribútum már elérhető a hello listája **felhasználói attribútumok**, és az előfizetési házirendek használható.</span><span class="sxs-lookup"><span data-stu-id="8ce04-117">hello custom attribute is now available in hello list of **User attributes**, and for use in your sign-up policies.</span></span>

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a><span data-ttu-id="8ce04-118">Egyéni attribútumot használja a regisztrációs szabályzatban</span><span class="sxs-lookup"><span data-stu-id="8ce04-118">Use a custom attribute in your sign-up policy</span></span>
1. <span data-ttu-id="8ce04-119">[Kövesse a lépéseket toonavigate toohello B2C funkciók panelje hello Azure-portálon](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="8ce04-119">[Follow these steps toonavigate toohello B2C features blade on hello Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="8ce04-120">Kattintson a **Regisztrálási szabályzatok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="8ce04-120">Click **Sign-up policies**.</span></span>
3. <span data-ttu-id="8ce04-121">Kattintson a regisztrációs szabályzatban (például "B2C_1_SiUp") tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="8ce04-121">Click your sign-up policy (for example, "B2C_1_SiUp") tooopen it.</span></span> <span data-ttu-id="8ce04-122">Kattintson a **szerkesztése** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="8ce04-122">Click **Edit** at hello top of hello blade.</span></span>
4. <span data-ttu-id="8ce04-123">Kattintson a **regisztrációs attribútumokat** és select hello egyedi attribútum (például "ShoeSize").</span><span class="sxs-lookup"><span data-stu-id="8ce04-123">Click **Sign-up attributes** and select hello custom attribute (for example, "ShoeSize").</span></span> <span data-ttu-id="8ce04-124">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8ce04-124">Click **OK**.</span></span>
5. <span data-ttu-id="8ce04-125">Kattintson a **alkalmazási jogcímet** és select hello egyéni attribútum.</span><span class="sxs-lookup"><span data-stu-id="8ce04-125">Click **Application claims** and select hello custom attribute.</span></span> <span data-ttu-id="8ce04-126">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8ce04-126">Click **OK**.</span></span>
6. <span data-ttu-id="8ce04-127">Kattintson a **mentése** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="8ce04-127">Click **Save** at hello top of hello blade.</span></span>

<span data-ttu-id="8ce04-128">Hello házirend tooverify hello végfelhasználói élmény hello "Futtatás most" funkciót is használja.</span><span class="sxs-lookup"><span data-stu-id="8ce04-128">You can use hello "Run now" feature on hello policy tooverify hello consumer experience.</span></span> <span data-ttu-id="8ce04-129">Kell most "ShoeSize" című felhasználói regisztráció során gyűjtött attribútumok hello listáját, és látja, hello token küldött vissza tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8ce04-129">You should now see "ShoeSize" in hello list of attributes collected during consumer sign-up, and see it in hello token sent back tooyour application.</span></span>

## <a name="notes"></a><span data-ttu-id="8ce04-130">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8ce04-130">Notes</span></span>
* <span data-ttu-id="8ce04-131">Előfizetési házirendek, valamint az egyéni attribútumok használhatók a regisztráció vagy bejelentkezés házirendek és Profilszerkesztési házirendek.</span><span class="sxs-lookup"><span data-stu-id="8ce04-131">Along with sign-up policies, custom attributes can also be used in sign-up or sign-in policies and profile editing policies.</span></span>
* <span data-ttu-id="8ce04-132">Egy ismert korlátozás egyéni attribútum van.</span><span class="sxs-lookup"><span data-stu-id="8ce04-132">There is a known limitation of custom attributes.</span></span> <span data-ttu-id="8ce04-133">Csak a létrehozott hello használják első alkalommal a házirendet, és nem Ha hozzáadja toohello listája **felhasználói attribútumok**.</span><span class="sxs-lookup"><span data-stu-id="8ce04-133">It is only created hello first time it is used in any policy, and not when you add it toohello list of **User attributes**.</span></span>

