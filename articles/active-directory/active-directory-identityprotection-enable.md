---
title: "Az Azure Active Directory-identitás védelem engedélyezése |} Microsoft Docs"
description: "Megtudhatja, hogyan engedélyezze az Azure Active Directory azonosító adatok védelmét."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f7a7ffaf-76bf-4cc7-96a1-86c944275c82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: e0683e837086ca08f4b4b3f49695bdd2b4063ea4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enabling-azure-active-directory-identity-protection"></a><span data-ttu-id="8b37f-104">Az Azure Active Directory-identitás védelem engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8b37f-104">Enabling Azure Active Directory Identity Protection</span></span>
<span data-ttu-id="8b37f-105">Az Azure Active Directory azonosító adatok védelmét, mely tartalmazza a gyanús bejelentkezési tevékenységek és a potenciális biztonsági réseket egyesített nézetét, és értesítéseket, a javítási javaslatokat és a kockázati-alapú házirendek segítségével megóvhatja új képessége az üzleti.</span><span class="sxs-lookup"><span data-stu-id="8b37f-105">Azure Active Directory Identity Protection is a new capability that provides a consolidated view into suspicious sign-in activities and potential vulnerabilities and with notifications, remediation recommendations and risk-based policies helps you protect your business.</span></span> 

<span data-ttu-id="8b37f-106">A szolgáltatás észleli a végfelhasználók és a kiemelt (rendszergazda) identitások gyanús tevékenységek alapján jelek, például a találgatásos támadásokkal szemben, elszivárgott a hitelesítő adatokat, bejelentkezések ismeretlen helyekről fertőzött eszközök, ezek a tevékenységek valós idejű elleni védelem érdekében.</span><span class="sxs-lookup"><span data-stu-id="8b37f-106">The service detects suspicious activities for end user and privileged (admin) identities based on signals like brute force attacks, leaked credentials, sign ins from unfamiliar locations, infected devices, to protect against these activities in real-time.</span></span> <span data-ttu-id="8b37f-107">Ennél is fontosabb ezek a gyanús tevékenységek alapján, a felhasználó kockázati besorolású számított és kockázati-alapú házirendek konfigurálhatók, és automatikus védelme a szervezet identitását.</span><span class="sxs-lookup"><span data-stu-id="8b37f-107">More importantly, based on these suspicious activities, a user risk severity is computed and risk-based policies can be configured and automatically protect the identities of your organization.</span></span> <span data-ttu-id="8b37f-108">További részletekért lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="8b37f-108">For more details, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

<span data-ttu-id="8b37f-109">A témakörök azt ismerteti, hogyan engedélyezze az Azure Active Directory azonosító adatok védelmét.</span><span class="sxs-lookup"><span data-stu-id="8b37f-109">This topics shows how to enable Azure Active Directory Identity Protection.</span></span>

## <a name="steps-to-enable-azure-active-directory-identity-protection"></a><span data-ttu-id="8b37f-110">Azure Active Directory Identity Protection engedélyezésének lépései</span><span class="sxs-lookup"><span data-stu-id="8b37f-110">Steps to enable Azure Active Directory Identity Protection</span></span>
1. <span data-ttu-id="8b37f-111">[Bejelentkezés](https://ms.portal.azure.com/) globális rendszergazdaként az Azure-portálra.</span><span class="sxs-lookup"><span data-stu-id="8b37f-111">[Sign-on](https://ms.portal.azure.com/) to your Azure portal as global administrator.</span></span> 
2. <span data-ttu-id="8b37f-112">Az Azure portálon kattintson **piactér**.</span><span class="sxs-lookup"><span data-stu-id="8b37f-112">In the Azure portal, click **Marketplace**.</span></span>
   
    <span data-ttu-id="8b37f-113">![Hozzon létre](./media/active-directory-identityprotection-enable/01.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8b37f-113">![Create](./media/active-directory-identityprotection-enable/01.png "Create")</span></span>
3. <span data-ttu-id="8b37f-114">Alkalmazások, kattintson a **biztonság + identitás szakaszában**.</span><span class="sxs-lookup"><span data-stu-id="8b37f-114">In the applications list, click **Security + Identity**.</span></span>
   
    <span data-ttu-id="8b37f-115">![Hozzon létre](./media/active-directory-identityprotection-enable/02.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8b37f-115">![Create](./media/active-directory-identityprotection-enable/02.png "Create")</span></span>
4. <span data-ttu-id="8b37f-116">Kattintson a **Azure AD Identity Protection**.</span><span class="sxs-lookup"><span data-stu-id="8b37f-116">Click **Azure AD Identity Protection**.</span></span>
   
    <span data-ttu-id="8b37f-117">![Hozzon létre](./media/active-directory-identityprotection-enable/03.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8b37f-117">![Create](./media/active-directory-identityprotection-enable/03.png "Create")</span></span>
5. <span data-ttu-id="8b37f-118">Az a **Azure AD Identity Protection** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8b37f-118">On the **Azure AD Identity Protection** blade, click **Create**.</span></span>
   
    <span data-ttu-id="8b37f-119">![Hozzon létre](./media/active-directory-identityprotection-enable/04.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8b37f-119">![Create](./media/active-directory-identityprotection-enable/04.png "Create")</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b37f-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8b37f-120">Next Steps</span></span>
* [<span data-ttu-id="8b37f-121">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="8b37f-121">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

