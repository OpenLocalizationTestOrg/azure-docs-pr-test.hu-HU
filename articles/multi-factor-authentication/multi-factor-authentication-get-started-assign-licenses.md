---
title: az Azure MFA aaaAssign icenses |} Microsoft Docs
description: "Ismerje meg, hogyan tooassign felhasználói licencek, a Microsoft Azure multi-factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 514ef423-8ee6-4987-8a4e-80d5dc394cf9
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/13/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ROBOTS: NOINDEX
ms.openlocfilehash: ca324eb4d6622fdad8bd3d74b7e1595919e36535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assigning-an-azure-mfa-azure-ad-premium-or-enterprise-mobility-license-toousers"></a><span data-ttu-id="9a252-103">Az Azure MFA-, prémium szintű Azure AD, vagy vállalati mobilitási licenc toousers hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9a252-103">Assigning an Azure MFA, Azure AD Premium, or Enterprise Mobility license toousers</span></span>
<span data-ttu-id="9a252-104">Ha az Azure MFA, prémium szintű Azure AD vagy a nagyvállalati mobilitási csomag licencet vásárolt, nem kell a multi-factor Auth provider toocreate.</span><span class="sxs-lookup"><span data-stu-id="9a252-104">If you have purchased Azure MFA, Azure AD Premium, or Enterprise Mobility Suite licenses, you do not need toocreate a Multi-Factor Auth provider.</span></span> <span data-ttu-id="9a252-105">Miután hello licencek hozzárendelése tooyour felhasználók, megkezdheti a multi-factor Authentication engedélyezésük.</span><span class="sxs-lookup"><span data-stu-id="9a252-105">Once you assign hello licenses tooyour users, you can begin enabling them for MFA.</span></span>

## <a name="tooassign-a-license"></a><span data-ttu-id="9a252-106">a licenc tooassign</span><span class="sxs-lookup"><span data-stu-id="9a252-106">tooassign a license</span></span>
1. <span data-ttu-id="9a252-107">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9a252-107">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) as an administrator.</span></span>
2. <span data-ttu-id="9a252-108">Hello bal oldalon válassza ki a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9a252-108">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="9a252-109">Hello Active Directory lapján kattintson duplán a megegyező tooenable kívánja hello felhasználók hello könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="9a252-109">On hello Active Directory page, double-click hello directory that has hello users you wish tooenable.</span></span>
4. <span data-ttu-id="9a252-110">Hello hello directory oldal tetején, válassza ki a **licencek**.</span><span class="sxs-lookup"><span data-stu-id="9a252-110">At hello top of hello directory page, select **Licenses**.</span></span>
   <span data-ttu-id="9a252-111">![Licencek hozzárendelése](./media/multi-factor-authentication-get-started-assign-licenses/assign1.png)</span><span class="sxs-lookup"><span data-stu-id="9a252-111">![Assign Licenses](./media/multi-factor-authentication-get-started-assign-licenses/assign1.png)</span></span>
5. <span data-ttu-id="9a252-112">Hello licencek lapon jelölje be **Azure multi-factor Authentication**, **Active Directory Premium**, vagy **nagyvállalati mobilitási csomag**.</span><span class="sxs-lookup"><span data-stu-id="9a252-112">On hello Licenses page, select **Azure Multi-Factor Authentication**, **Active Directory Premium**, or **Enterprise Mobility Suite**.</span></span>  <span data-ttu-id="9a252-113">Ha csak eggyel rendelkezik, akkor azt a rendszer automatikusan bejelöli.</span><span class="sxs-lookup"><span data-stu-id="9a252-113">If you only have one, then it should be selected automatically.</span></span>
6. <span data-ttu-id="9a252-114">Hello a hello lap alján, kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="9a252-114">At hello bottom of hello page, click **Assign**.</span></span>
   <span data-ttu-id="9a252-115">![Licencek hozzárendelése](./media/multi-factor-authentication-get-started-assign-licenses/assign3.png)</span><span class="sxs-lookup"><span data-stu-id="9a252-115">![Assign Licenses](./media/multi-factor-authentication-get-started-assign-licenses/assign3.png)</span></span>
7. <span data-ttu-id="9a252-116">A felmerül hello párbeszédpanelen kattintson a Tovább toohello felhasználók vagy csoportok tooassign licenceket szeretne.</span><span class="sxs-lookup"><span data-stu-id="9a252-116">In hello box that comes up, click next toohello users or groups you want tooassign licenses to.</span></span>  <span data-ttu-id="9a252-117">Egy zöld pipának kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="9a252-117">You should see a green check mark appear.</span></span>
8. <span data-ttu-id="9a252-118">Kattintson a hello pipa ikon toosave hello módosítások.</span><span class="sxs-lookup"><span data-stu-id="9a252-118">Click hello check mark icon toosave hello changes.</span></span>
   <span data-ttu-id="9a252-119">![Licencek hozzárendelése](./media/multi-factor-authentication-get-started-assign-licenses/assign4.png)</span><span class="sxs-lookup"><span data-stu-id="9a252-119">![Assign Licenses](./media/multi-factor-authentication-get-started-assign-licenses/assign4.png)</span></span>
9. <span data-ttu-id="9a252-120">Ekkor egy üzenet jelenik meg amely tartalmazza, hány licenc lett hozzárendelve, és hány hozzárendelése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="9a252-120">You should see a message saying how many licenses were assigned and how many may have failed.</span></span>  <span data-ttu-id="9a252-121">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9a252-121">Click **Ok**.</span></span>
   <span data-ttu-id="9a252-122">![Licencek hozzárendelése](./media/multi-factor-authentication-get-started-assign-licenses/assign5.png)</span><span class="sxs-lookup"><span data-stu-id="9a252-122">![Assign Licenses](./media/multi-factor-authentication-get-started-assign-licenses/assign5.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a252-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a252-123">Next steps</span></span>

- <span data-ttu-id="9a252-124">További információkért lásd [a Microsoft Azure Active Directory-licencelést](../active-directory/active-directory-licensing-what-is.md) ismertető témakört.</span><span class="sxs-lookup"><span data-stu-id="9a252-124">For more information, see [What is Microsoft Azure Active Directory licensing?](../active-directory/active-directory-licensing-what-is.md)</span></span>
