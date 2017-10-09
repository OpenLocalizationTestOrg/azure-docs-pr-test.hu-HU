---
title: "Azure AD Connect szinkronizálása: változó hello AD DS-fiókja jelszavát |} Microsoft Docs"
description: "Ez a témakör a dokumentum ismerteti, hogyan tooupdate az Azure AD Connect hello Tartományi fiók jelszavát hello után módosul."
services: active-directory
keywords: "AD DS-fiókjához, az Active Directory-fiókot, a jelszót"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2707c9246446612f8d083ecde876b3b4fb2435ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-ad-ds-account-password"></a><span data-ttu-id="fe9a9-104">Hello AD DS fiók jelszavának módosítása</span><span class="sxs-lookup"><span data-stu-id="fe9a9-104">Changing hello AD DS account password</span></span>
<span data-ttu-id="fe9a9-105">hello AD DS-fiókjához toohello felhasználói fiókot az Azure AD Connect toocommunicate a helyszíni Active Directory által használt hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-105">hello AD DS account refers toohello user account used by Azure AD Connect toocommunicate with on-premises Active Directory.</span></span> <span data-ttu-id="fe9a9-106">Ha módosítja az AD DS-fiókjához hello hello jelszó, frissítenie kell az Azure AD Connect szinkronizálási szolgáltatás hello új jelszót.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-106">If you change hello password of hello AD DS account, you must update Azure AD Connect Synchronization Service with hello new password.</span></span> <span data-ttu-id="fe9a9-107">Ellenkező esetben szinkronizálási már nem szinkronizálható megfelelően hello hello a helyszíni Active Directory és a következő hibák hello fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="fe9a9-107">Otherwise, hello Synchronization can no longer synchronize correctly with hello on-premises Active Directory and you will encounter hello following errors:</span></span>

* <span data-ttu-id="fe9a9-108">Hello a Synchronization Service Managert, bármely importálási vagy exportálási művelet a helyszíni AD meghiúsul **-start-hitelesítő adatok elhagyását** hiba.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-108">In hello Synchronization Service Manager, any import or export operation with on-premises AD fails with **no-start-credentials** error.</span></span>

* <span data-ttu-id="fe9a9-109">A Windows Eseménynapló hello alkalmazások eseménynaplójában keresse meg a hibát tartalmaz **Event ID 6000** és üzenet **"hello"contoso.com"frissíti a kezelőügynököt folytán toorun hello hitelesítő adatok érvénytelenek voltak"** .</span><span class="sxs-lookup"><span data-stu-id="fe9a9-109">Under Windows Event Viewer, hello application event log contains an error with **Event ID 6000** and message **'hello management agent "contoso.com" failed toorun because hello credentials were invalid'**.</span></span>


## <a name="how-tooupdate-hello-synchronization-service-with-new-password-for-ad-ds-account"></a><span data-ttu-id="fe9a9-110">Hogyan tooupdate hello-szinkronizálási szolgáltatás új jelszót az AD DS-fiókjához</span><span class="sxs-lookup"><span data-stu-id="fe9a9-110">How tooupdate hello Synchronization Service with new password for AD DS account</span></span>
<span data-ttu-id="fe9a9-111">tooupdate hello szinkronizálási szolgáltatás hello új jelszóval:</span><span class="sxs-lookup"><span data-stu-id="fe9a9-111">tooupdate hello Synchronization Service with hello new password:</span></span>

1. <span data-ttu-id="fe9a9-112">Indítsa el a Synchronization Service Managert (KEZDŐ → szinkronizálási szolgáltatás) hello.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-112">Start hello Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="fe9a9-113">![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="fe9a9-113">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  

2. <span data-ttu-id="fe9a9-114">Nyissa meg toohello **összekötők** fülre.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-114">Go toohello **Connectors** tab.</span></span>

3. <span data-ttu-id="fe9a9-115">Jelölje be hello **Címtárösszekötőben** , amely megfelel a toohello AD DS-fiókjához, amelynek a jelszava megváltozott.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-115">Select hello **AD Connector** that corresponds toohello AD DS account for which its password was changed.</span></span>

4. <span data-ttu-id="fe9a9-116">A **műveletek**, jelölje be **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-116">Under **Actions**, select **Properties**.</span></span>

5. <span data-ttu-id="fe9a9-117">Hello előugró párbeszédpanelen válassza ki a **tooActive Directory-erdő csatlakozás**:</span><span class="sxs-lookup"><span data-stu-id="fe9a9-117">In hello pop-up dialog, select **Connect tooActive Directory Forest**:</span></span>

6. <span data-ttu-id="fe9a9-118">Adjon meg új jelszót hello hello Tartományi fiók hello **jelszó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-118">Enter hello new password of hello AD DS account in hello **Password** textbox.</span></span>

7. <span data-ttu-id="fe9a9-119">Kattintson a **OK** toosave hello új jelszót és Bezárás hello előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-119">Click **OK** toosave hello new password and close hello pop-up dialog.</span></span>

8. <span data-ttu-id="fe9a9-120">Indítsa újra az Azure AD Connect szinkronizálási szolgáltatás a Windows szolgáltatásvezérlő hello.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-120">Restart hello Azure AD Connect Synchronization Service under Windows Service Control Manager.</span></span> <span data-ttu-id="fe9a9-121">Ez az tooensure eltávolított minden hivatkozás toohello régi jelszót hello memória-gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="fe9a9-121">This is tooensure that any reference toohello old password is removed from hello memory cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe9a9-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe9a9-122">Next steps</span></span>
<span data-ttu-id="fe9a9-123">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="fe9a9-123">**Overview topics**</span></span>

* [<span data-ttu-id="fe9a9-124">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="fe9a9-124">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="fe9a9-125">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="fe9a9-125">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
