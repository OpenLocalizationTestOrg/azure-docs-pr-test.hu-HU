---
title: "Azure AD Connect szinkronizálása: címtárbővítmények |} Microsoft Docs"
description: "Ez a témakör ismerteti a hello directory böngészőbővítmények funkciója az Azure AD Connectben."
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
ms.openlocfilehash: 31525ae914aa4d9e047ea1515b460a8311d5c815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a><span data-ttu-id="e64d3-103">Azure AD Connect szinkronizálása: címtárbővítmények</span><span class="sxs-lookup"><span data-stu-id="e64d3-103">Azure AD Connect sync: Directory extensions</span></span>
<span data-ttu-id="e64d3-104">Címtárbővítmények lehetővé teszi tooextend hello séma saját attribútumait a helyszíni Active Directoryból az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e64d3-104">Directory extensions allows you tooextend hello schema in Azure AD with your own attributes from on-premises Active Directory.</span></span> <span data-ttu-id="e64d3-105">Ez a funkció lehetővé teszi toobuild LOB-alkalmazások felhasználása attribútumok helyszíni toomanage folytatja a műveletet.</span><span class="sxs-lookup"><span data-stu-id="e64d3-105">This feature allows you toobuild LOB apps consuming attributes you continue toomanage on-premises.</span></span> <span data-ttu-id="e64d3-106">Ezek az attribútumok felhasználhatók, keresztül [Azure AD Graph címtárbővítmények](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) vagy [Microsoft Graph](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="e64d3-106">These attributes can be consumed through [Azure AD Graph directory extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) or [Microsoft Graph](https://graph.microsoft.io/).</span></span> <span data-ttu-id="e64d3-107">Megtekintheti a hello attribútumok elérhető használatával [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) és [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="e64d3-107">You can see hello attributes available using [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) and [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) respectively.</span></span>

<span data-ttu-id="e64d3-108">Jelenleg nincsenek Office 365 munkaterhelés igényel, ezek az attribútumok.</span><span class="sxs-lookup"><span data-stu-id="e64d3-108">At present no Office 365 workload consumes these attributes.</span></span>

<span data-ttu-id="e64d3-109">Mely további attribútumokat beállíthat kívánt toosynchronize hello egyéni beállítások útvonalán hello telepítővarázslóját.</span><span class="sxs-lookup"><span data-stu-id="e64d3-109">You configure which additional attributes you want toosynchronize in hello custom settings path in hello installation wizard.</span></span>
<span data-ttu-id="e64d3-110">![Séma kiterjesztése varázsló](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span><span class="sxs-lookup"><span data-stu-id="e64d3-110">![Schema Extension Wizard](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)</span></span>  
<span data-ttu-id="e64d3-111">hello telepítési hello attribútumok, amelyek érvényes erre a következő látható:</span><span class="sxs-lookup"><span data-stu-id="e64d3-111">hello installation shows hello following attributes, which are valid candidates:</span></span>

* <span data-ttu-id="e64d3-112">Felhasználók és csoportok objektumtípusok</span><span class="sxs-lookup"><span data-stu-id="e64d3-112">User and Group object types</span></span>
* <span data-ttu-id="e64d3-113">Egyértékű attribútumok: String, Boolean, egész, bináris</span><span class="sxs-lookup"><span data-stu-id="e64d3-113">Single-valued attributes: String, Boolean, Integer, Binary</span></span>
* <span data-ttu-id="e64d3-114">Többértékű attribútumok: karakterlánc, a bináris</span><span class="sxs-lookup"><span data-stu-id="e64d3-114">Multi-valued attributes: String, Binary</span></span>

<span data-ttu-id="e64d3-115">az attribútumok listájában hello az Azure AD Connect telepítése során létrehozott hello sémagyorsítótár olvasható.</span><span class="sxs-lookup"><span data-stu-id="e64d3-115">hello list of attributes is read from hello schema cache created during installation of Azure AD Connect.</span></span> <span data-ttu-id="e64d3-116">Ha további attribútumokkal hello Active Directory-séma már ki van bővítve, majd hello [sémát frissíteni kell](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) előtt ezeket az új attribútumokat láthatók.</span><span class="sxs-lookup"><span data-stu-id="e64d3-116">If you have extended hello Active Directory schema with additional attributes, then hello [schema must be refreshed](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) before these new attributes are visible.</span></span>

<span data-ttu-id="e64d3-117">Az Azure AD-objektum too100 bővítmények attribútumok legfeljebb tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="e64d3-117">An object in Azure AD can have up too100 directory extensions attributes.</span></span> <span data-ttu-id="e64d3-118">maximális hossz hello 250 karakterből áll.</span><span class="sxs-lookup"><span data-stu-id="e64d3-118">hello max length is 250 characters.</span></span> <span data-ttu-id="e64d3-119">Ha egy attribútum-érték hosszabb, majd a függvény egésszé csonkítja hello szinkronizálási motor.</span><span class="sxs-lookup"><span data-stu-id="e64d3-119">If an attribute value is longer, then it is truncated by hello sync engine.</span></span>

<span data-ttu-id="e64d3-120">Az Azure AD Connect telepítése során az alkalmazás regisztrálva van, ahol elérhetők ezek az attribútumok.</span><span class="sxs-lookup"><span data-stu-id="e64d3-120">During installation of Azure AD Connect, an application is registered where these attributes are available.</span></span> <span data-ttu-id="e64d3-121">Ez az alkalmazás hello Azure-portálon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="e64d3-121">You can see this application in hello Azure portal.</span></span>  
![Séma kiterjesztése alkalmazás](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

<span data-ttu-id="e64d3-123">Ezek az attribútumok elérhetők Graph keresztül:</span><span class="sxs-lookup"><span data-stu-id="e64d3-123">These attributes are now available through Graph:</span></span>  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

<span data-ttu-id="e64d3-125">hello attribútumok fűzve előtagként a bővítmény\_{AppClientId}\_.</span><span class="sxs-lookup"><span data-stu-id="e64d3-125">hello attributes are prefixed with extension\_{AppClientId}\_.</span></span> <span data-ttu-id="e64d3-126">hello AppClientId hello értékeként az összes attribútum az az Azure AD-bérlő esetében azonos értéket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e64d3-126">hello AppClientId has hello same value for all attributes in your Azure AD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e64d3-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e64d3-127">Next steps</span></span>
<span data-ttu-id="e64d3-128">További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="e64d3-128">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="e64d3-129">További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="e64d3-129">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
