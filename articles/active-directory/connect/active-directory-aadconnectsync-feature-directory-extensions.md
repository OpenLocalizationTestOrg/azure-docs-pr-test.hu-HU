---
title: "Azure AD Connect szinkronizálása: címtárbővítmények |} Microsoft Docs"
description: "Ez a témakör ismerteti az Azure AD Connectben a directory böngészőbővítmények funkciója."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 8ab9944437443a6d796345782f4f798aec96a0f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a><span data-ttu-id="fcdd3-103">Azure AD Connect szinkronizálása: címtárbővítmények</span><span class="sxs-lookup"><span data-stu-id="fcdd3-103">Azure AD Connect sync: Directory extensions</span></span>
<span data-ttu-id="fcdd3-104">Címtárbővítmények lehetővé teszi a séma kiterjesztése a saját attribútumokkal rendelkező Azure AD-ben a helyszíni Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-104">Directory extensions allows you to extend the schema in Azure AD with your own attributes from on-premises Active Directory.</span></span> <span data-ttu-id="fcdd3-105">Ez a funkció lehetővé teszi ÜZLETÁGI alkalmazások továbbra is kezelheti a helyszíni attribútumok fel.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-105">This feature allows you to build LOB apps consuming attributes you continue to manage on-premises.</span></span> <span data-ttu-id="fcdd3-106">Ezek az attribútumok felhasználhatók, keresztül [Azure AD Graph címtárbővítmények](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) vagy [Microsoft Graph](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="fcdd3-106">These attributes can be consumed through [Azure AD Graph directory extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) or [Microsoft Graph](https://graph.microsoft.io/).</span></span> <span data-ttu-id="fcdd3-107">Megjelenik az attribútumok elérhető használatával [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) és [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-107">You can see the attributes available using [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) and [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respectively.</span></span>

<span data-ttu-id="fcdd3-108">Jelenleg nincsenek Office 365 munkaterhelés igényel, ezek az attribútumok.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-108">At present no Office 365 workload consumes these attributes.</span></span>

<span data-ttu-id="fcdd3-109">Szeretné szinkronizálni a telepítési varázsló egyéni beállítások útvonalán mely további attribútumok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fcdd3-109">You configure which additional attributes you want to synchronize in the custom settings path in the installation wizard.</span></span>
<span data-ttu-id="fcdd3-110">![Séma kiterjesztése varázsló](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span><span class="sxs-lookup"><span data-stu-id="fcdd3-110">![Schema Extension Wizard](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span></span>  
<span data-ttu-id="fcdd3-111">A telepítés a következő attribútumok, amelyek a deduplikációra kijelölt érvényes jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="fcdd3-111">The installation shows the following attributes, which are valid candidates:</span></span>

* <span data-ttu-id="fcdd3-112">Felhasználók és csoportok objektumtípusok</span><span class="sxs-lookup"><span data-stu-id="fcdd3-112">User and Group object types</span></span>
* <span data-ttu-id="fcdd3-113">Egyértékű attribútumok: String, Boolean, egész, bináris</span><span class="sxs-lookup"><span data-stu-id="fcdd3-113">Single-valued attributes: String, Boolean, Integer, Binary</span></span>
* <span data-ttu-id="fcdd3-114">Többértékű attribútumok: karakterlánc, a bináris</span><span class="sxs-lookup"><span data-stu-id="fcdd3-114">Multi-valued attributes: String, Binary</span></span>

<span data-ttu-id="fcdd3-115">Az Azure AD Connect telepítése során létrehozott séma gyorsítótárból attribútumok listája olvasható.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-115">The list of attributes is read from the schema cache created during installation of Azure AD Connect.</span></span> <span data-ttu-id="fcdd3-116">Ha további attribútumokkal, az Active Directory-séma ki van bővítve a [sémát frissíteni kell](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) előtt ezeket az új attribútumokat láthatók.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-116">If you have extended the Active Directory schema with additional attributes, then the [schema must be refreshed](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) before these new attributes are visible.</span></span>

<span data-ttu-id="fcdd3-117">Az Azure AD-objektum lehet akár 100 directory bővítmények attribútuma.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-117">An object in Azure AD can have up to 100 directory extensions attributes.</span></span> <span data-ttu-id="fcdd3-118">A maximális hossza 250 karakterből áll.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-118">The max length is 250 characters.</span></span> <span data-ttu-id="fcdd3-119">Ha egy attribútum-érték hosszabb, majd a függvény egésszé csonkítja a szinkronizálási motor.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-119">If an attribute value is longer, then it is truncated by the sync engine.</span></span>

<span data-ttu-id="fcdd3-120">Az Azure AD Connect telepítése során az alkalmazás regisztrálva van, ahol elérhetők ezek az attribútumok.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-120">During installation of Azure AD Connect, an application is registered where these attributes are available.</span></span> <span data-ttu-id="fcdd3-121">Ez az alkalmazás az Azure-portálon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-121">You can see this application in the Azure portal.</span></span>  
![Séma kiterjesztése alkalmazás](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

<span data-ttu-id="fcdd3-123">Ezek az attribútumok elérhetők Graph keresztül:</span><span class="sxs-lookup"><span data-stu-id="fcdd3-123">These attributes are now available through Graph:</span></span>  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

<span data-ttu-id="fcdd3-125">Az attribútumok fűzve előtagként a bővítmény\_{AppClientId}\_.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-125">The attributes are prefixed with extension\_{AppClientId}\_.</span></span> <span data-ttu-id="fcdd3-126">A AppClientId értéke az összes attribútum esetében az Azure AD-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-126">The AppClientId has the same value for all attributes in your Azure AD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fcdd3-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fcdd3-127">Next steps</span></span>
<span data-ttu-id="fcdd3-128">További információ a [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-128">Learn more about the [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="fcdd3-129">További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="fcdd3-129">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
