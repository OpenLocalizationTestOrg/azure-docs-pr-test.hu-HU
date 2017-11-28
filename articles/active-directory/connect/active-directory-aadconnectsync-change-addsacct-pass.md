---
title: "Azure AD Connect szinkronizálása: az Active Directory tartományi szolgáltatások fiók jelszavának módosítása |} Microsoft Docs"
description: "Ez a témakör a dokumentum ismerteti az Azure AD Connect frissítése után az Active Directory tartományi szolgáltatások fiók jelszava megváltozott."
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
ms.openlocfilehash: 14e16a238e60ecfeeb3cbf88c3922a79349dcc75
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="changing-the-ad-ds-account-password"></a><span data-ttu-id="379f9-104">Az Active Directory tartományi szolgáltatások fiók jelszavának módosítása</span><span class="sxs-lookup"><span data-stu-id="379f9-104">Changing the AD DS account password</span></span>
<span data-ttu-id="379f9-105">Az AD DS-fiókjához az Azure AD Connect a helyszíni Active Directory folytatott kommunikációhoz használt felhasználói fiókra hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="379f9-105">The AD DS account refers to the user account used by Azure AD Connect to communicate with on-premises Active Directory.</span></span> <span data-ttu-id="379f9-106">Ha módosítja a Tartományi fiók jelszavát, az új jelszóval frissítenie kell az Azure AD Connect szinkronizálási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="379f9-106">If you change the password of the AD DS account, you must update Azure AD Connect Synchronization Service with the new password.</span></span> <span data-ttu-id="379f9-107">Ellenkező esetben a szinkronizálás a helyszíni Active Directory a már nem szinkronizálhat megfelelően, és találkozhat hibák a következők:</span><span class="sxs-lookup"><span data-stu-id="379f9-107">Otherwise, the Synchronization can no longer synchronize correctly with the on-premises Active Directory and you will encounter the following errors:</span></span>

* <span data-ttu-id="379f9-108">A Synchronization Service Managert, bármely importálási vagy exportálási művelet a helyszíni AD meghiúsul, és **-start-hitelesítő adatok elhagyását** hiba.</span><span class="sxs-lookup"><span data-stu-id="379f9-108">In the Synchronization Service Manager, any import or export operation with on-premises AD fails with **no-start-credentials** error.</span></span>

* <span data-ttu-id="379f9-109">A Windows eseménynaplóban, az alkalmazások eseménynaplójában keresse meg a hibát tartalmaz **Event ID 6000** és üzenet **"a"contoso.com"kezelőügynök futtatása sikertelen, mert a hitelesítő adatok érvénytelenek voltak"**.</span><span class="sxs-lookup"><span data-stu-id="379f9-109">Under Windows Event Viewer, the application event log contains an error with **Event ID 6000** and message **'The management agent "contoso.com" failed to run because the credentials were invalid'**.</span></span>


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a><span data-ttu-id="379f9-110">Az AD DS-fiókjához új jelszót a Synchronization Service frissítése</span><span class="sxs-lookup"><span data-stu-id="379f9-110">How to update the Synchronization Service with new password for AD DS account</span></span>
<span data-ttu-id="379f9-111">A Synchronization Service frissítése az új jelszóval:</span><span class="sxs-lookup"><span data-stu-id="379f9-111">To update the Synchronization Service with the new password:</span></span>

1. <span data-ttu-id="379f9-112">Indítsa el a Synchronization Service Managert (KEZDŐ → szinkronizálási szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="379f9-112">Start the Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="379f9-113">![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="379f9-113">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  

2. <span data-ttu-id="379f9-114">Lépjen a **összekötők** fülre.</span><span class="sxs-lookup"><span data-stu-id="379f9-114">Go to the **Connectors** tab.</span></span>

3. <span data-ttu-id="379f9-115">Válassza ki a **Címtárösszekötőben** , amely megfelel a az AD DS-fiókjához, amelynek a jelszava megváltozott.</span><span class="sxs-lookup"><span data-stu-id="379f9-115">Select the **AD Connector** that corresponds to the AD DS account for which its password was changed.</span></span>

4. <span data-ttu-id="379f9-116">A **műveletek**, jelölje be **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="379f9-116">Under **Actions**, select **Properties**.</span></span>

5. <span data-ttu-id="379f9-117">Az előugró párbeszédpanelen válassza ki a **csatlakozás az Active Directory-erdő**:</span><span class="sxs-lookup"><span data-stu-id="379f9-117">In the pop-up dialog, select **Connect to Active Directory Forest**:</span></span>

6. <span data-ttu-id="379f9-118">Írja be az új Active Directory tartományi szolgáltatások fiókjának a **jelszó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="379f9-118">Enter the new password of the AD DS account in the **Password** textbox.</span></span>

7. <span data-ttu-id="379f9-119">Kattintson a **OK** az új jelszó mentse és zárja be az előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="379f9-119">Click **OK** to save the new password and close the pop-up dialog.</span></span>

8. <span data-ttu-id="379f9-120">Indítsa újra az Azure AD Connect szinkronizálási szolgáltatás a Windows szolgáltatásvezérlő.</span><span class="sxs-lookup"><span data-stu-id="379f9-120">Restart the Azure AD Connect Synchronization Service under Windows Service Control Manager.</span></span> <span data-ttu-id="379f9-121">Ez azért szükséges, hogy az összes hivatkozás a régi jelszó törlődik a memória-gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="379f9-121">This is to ensure that any reference to the old password is removed from the memory cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="379f9-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="379f9-122">Next steps</span></span>
<span data-ttu-id="379f9-123">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="379f9-123">**Overview topics**</span></span>

* [<span data-ttu-id="379f9-124">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="379f9-124">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="379f9-125">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="379f9-125">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
