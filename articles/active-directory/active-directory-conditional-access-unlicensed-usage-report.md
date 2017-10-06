---
title: "Használati jelentés aaaUnlicensed |} Microsoft Docs"
description: "hello licenc nélküli használati jelentés licenc nélküli felhasználók által használt azonosíthatja, hogy a fizetős Azure AD-funkciókat."
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
ms.openlocfilehash: c44d1756b4641d7ca88434017eedb6c5e2567cb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="unlicensed-usage-report"></a><span data-ttu-id="a339b-103">Licenc nélküli használati jelentés</span><span class="sxs-lookup"><span data-stu-id="a339b-103">Unlicensed usage report</span></span>
<span data-ttu-id="a339b-104">hello licenc nélküli használati jelentés licenc nélküli felhasználók által használt azonosíthatja, hogy a fizetős Azure AD-funkciókat.</span><span class="sxs-lookup"><span data-stu-id="a339b-104">hello unlicensed usage report helps you identify unlicensed users that are using paid Azure AD features.</span></span> <span data-ttu-id="a339b-105">Ez lehetővé teszi toomake jobb megvásárolt licencek és tudja, amikor szükség lehet további licencek tooidentify használatát.</span><span class="sxs-lookup"><span data-stu-id="a339b-105">This allows you toomake better use of licenses that you have purchased and tooidentify you know when you may need additional licenses.</span></span> 

<span data-ttu-id="a339b-106">a jelentésben hello hello szolgáltatások fizetős utolsó 30 nap hello aktív használatát.</span><span class="sxs-lookup"><span data-stu-id="a339b-106">hello report shows active usage of hello paid features in hello last 30 days.</span></span> 

## <a name="report-structure"></a><span data-ttu-id="a339b-107">A jelentés struktúra</span><span class="sxs-lookup"><span data-stu-id="a339b-107">Report structure</span></span>
| <span data-ttu-id="a339b-108">Oszlop neve</span><span class="sxs-lookup"><span data-stu-id="a339b-108">Column name</span></span> | <span data-ttu-id="a339b-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="a339b-109">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a339b-110">Licenc nélküli felhasználók</span><span class="sxs-lookup"><span data-stu-id="a339b-110">Unlicensed User</span></span> |<span data-ttu-id="a339b-111">Hello felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="a339b-111">Name of hello user</span></span> |
| <span data-ttu-id="a339b-112">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a339b-112">Feature</span></span> |<span data-ttu-id="a339b-113">hello szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="a339b-113">hello feature name.</span></span> <span data-ttu-id="a339b-114">Például: a feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="a339b-114">For example: conditional access</span></span> |
| <span data-ttu-id="a339b-115">Alkalmazás érhető el</span><span class="sxs-lookup"><span data-stu-id="a339b-115">Application Accessed</span></span> |<span data-ttu-id="a339b-116">hello hello funkcióval is hozzáférnek hello alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="a339b-116">hello name of hello application that is being accessed with hello feature.</span></span> <span data-ttu-id="a339b-117">Például: Office 365 SharePoint online-hoz</span><span class="sxs-lookup"><span data-stu-id="a339b-117">For example: Office 365 SharePoint Online</span></span> |

> [!NOTE]
> <span data-ttu-id="a339b-118">Ha egy felhasználói fiókot törölték hello "Licenc nélküli felhasználó" oszlop tölti fel az ID, például a 1003000090D8B285</span><span class="sxs-lookup"><span data-stu-id="a339b-118">If a user account has been deleted hello ‘Unlicensed User’ column will be populated with an ID, like 1003000090D8B285</span></span>
> 
> 

## <a name="conditional-access-feature"></a><span data-ttu-id="a339b-119">Feltételes hozzáférési funkciónak</span><span class="sxs-lookup"><span data-stu-id="a339b-119">Conditional access feature</span></span>
<span data-ttu-id="a339b-120">Licenc nélküli felhasználók lesz megjelölve, amikor hozzáférni a feltételes hozzáférési szabályzatot, akkor használható, ha nem rendelkeznek egy Azure AD Premium-licenc egy szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a339b-120">Unlicensed users will be flagged when they access a service that has conditional access policy applied if they do not have an Azure AD Premium license.</span></span> 

<span data-ttu-id="a339b-121">Ez vonatkozik tooMFA, / hely házirendeket, valamint az eszköz házirendek, amelyek az Intune-nal.</span><span class="sxs-lookup"><span data-stu-id="a339b-121">This applies tooMFA / Location policies as well as device polices that use Intune.</span></span>

## <a name="see-also"></a><span data-ttu-id="a339b-122">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a339b-122">See also</span></span>
* [<span data-ttu-id="a339b-123">Feltételes hozzáférés az Office 365 és az egyéb Azure Active Directory használatával csatlakozó alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a339b-123">Using Conditional Access with Office 365 and other Azure Active Directory connected apps</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="a339b-124">Feltételes hozzáférés tooAzure AD első lépések</span><span class="sxs-lookup"><span data-stu-id="a339b-124">Getting started with conditional access tooAzure AD</span></span>](active-directory-conditional-access-azuread-connected-apps.md) 

