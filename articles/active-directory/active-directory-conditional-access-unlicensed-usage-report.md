---
title: "Licenc nélküli használati jelentés |} Microsoft Docs"
description: "A nem licencelt használati jelentés segítségével azonosíthatja a licenc nélküli felhasználók által használt fizetős Azure AD-funkciókat."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: c0b4f455f067e825362bdecc02ea62d7984f0d31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="unlicensed-usage-report"></a><span data-ttu-id="1b397-103">Licenc nélküli használati jelentés</span><span class="sxs-lookup"><span data-stu-id="1b397-103">Unlicensed usage report</span></span>
<span data-ttu-id="1b397-104">A nem licencelt használati jelentés segítségével azonosíthatja a licenc nélküli felhasználók által használt fizetős Azure AD-funkciókat.</span><span class="sxs-lookup"><span data-stu-id="1b397-104">The unlicensed usage report helps you identify unlicensed users that are using paid Azure AD features.</span></span> <span data-ttu-id="1b397-105">Ez lehetővé teszi, hogy élvezetesebbé tegyük használja a megvásárolt licencek és azonosításához tudja, amikor szükség lehet további licenceket.</span><span class="sxs-lookup"><span data-stu-id="1b397-105">This allows you to make better use of licenses that you have purchased and to identify you know when you may need additional licenses.</span></span> 

<span data-ttu-id="1b397-106">Ebben a jelentésben aktív használati fizetős funkciója az elmúlt 30 napban.</span><span class="sxs-lookup"><span data-stu-id="1b397-106">The report shows active usage of the paid features in the last 30 days.</span></span> 

## <a name="report-structure"></a><span data-ttu-id="1b397-107">A jelentés struktúra</span><span class="sxs-lookup"><span data-stu-id="1b397-107">Report structure</span></span>
| <span data-ttu-id="1b397-108">Oszlop neve</span><span class="sxs-lookup"><span data-stu-id="1b397-108">Column name</span></span> | <span data-ttu-id="1b397-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b397-109">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1b397-110">Licenc nélküli felhasználók</span><span class="sxs-lookup"><span data-stu-id="1b397-110">Unlicensed User</span></span> |<span data-ttu-id="1b397-111">A felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="1b397-111">Name of the user</span></span> |
| <span data-ttu-id="1b397-112">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="1b397-112">Feature</span></span> |<span data-ttu-id="1b397-113">A szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="1b397-113">The feature name.</span></span> <span data-ttu-id="1b397-114">Például: a feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="1b397-114">For example: conditional access</span></span> |
| <span data-ttu-id="1b397-115">Alkalmazás érhető el</span><span class="sxs-lookup"><span data-stu-id="1b397-115">Application Accessed</span></span> |<span data-ttu-id="1b397-116">Az alkalmazást, amely használatban van a szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="1b397-116">The name of the application that is being accessed with the feature.</span></span> <span data-ttu-id="1b397-117">Például: Office 365 SharePoint online-hoz</span><span class="sxs-lookup"><span data-stu-id="1b397-117">For example: Office 365 SharePoint Online</span></span> |

> [!NOTE]
> <span data-ttu-id="1b397-118">Ha egy felhasználói fiók törölve lett a "Licenc nélküli felhasználók" oszlop tölti fel az ID, például a 1003000090D8B285</span><span class="sxs-lookup"><span data-stu-id="1b397-118">If a user account has been deleted the ‘Unlicensed User’ column will be populated with an ID, like 1003000090D8B285</span></span>
> 
> 

## <a name="conditional-access-feature"></a><span data-ttu-id="1b397-119">Feltételes hozzáférési funkciónak</span><span class="sxs-lookup"><span data-stu-id="1b397-119">Conditional access feature</span></span>
<span data-ttu-id="1b397-120">Licenc nélküli felhasználók lesz megjelölve, amikor hozzáférni a feltételes hozzáférési szabályzatot, akkor használható, ha nem rendelkeznek egy Azure AD Premium-licenc egy szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1b397-120">Unlicensed users will be flagged when they access a service that has conditional access policy applied if they do not have an Azure AD Premium license.</span></span> 

<span data-ttu-id="1b397-121">Ez vonatkozik az MFA / hely házirendeket, valamint az eszköz házirendek, amelyek az Intune-nal.</span><span class="sxs-lookup"><span data-stu-id="1b397-121">This applies to MFA / Location policies as well as device polices that use Intune.</span></span>

## <a name="see-also"></a><span data-ttu-id="1b397-122">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1b397-122">See also</span></span>
* [<span data-ttu-id="1b397-123">Feltételes hozzáférés az Office 365 és az egyéb Azure Active Directory használatával csatlakozó alkalmazások</span><span class="sxs-lookup"><span data-stu-id="1b397-123">Using Conditional Access with Office 365 and other Azure Active Directory connected apps</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="1b397-124">Az Azure AD feltételes hozzáférés – első lépések</span><span class="sxs-lookup"><span data-stu-id="1b397-124">Getting started with conditional access to Azure AD</span></span>](active-directory-conditional-access-azuread-connected-apps.md) 

