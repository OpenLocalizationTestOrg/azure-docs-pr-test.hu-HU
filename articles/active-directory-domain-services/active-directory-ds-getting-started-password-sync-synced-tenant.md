---
title: "Azure AD tartományi szolgáltatások: Jelszavak szinkronizálásának engedélyezése | Microsoft Docs"
description: "Első lépések az Azure Active Directory tartományi szolgáltatások használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: a7a6ee0f83d3d9bdaf236717efb39155a26934e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a><span data-ttu-id="e3aed-103">Jelszó-szinkronizálás tooAzure Active Directory tartományi szolgáltatások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e3aed-103">Enable password synchronization tooAzure Active Directory Domain Services</span></span>
<span data-ttu-id="e3aed-104">Az előző feladatokban engedélyezte az Active Directory Domain Servicest az Azure Active Directory (Azure AD) bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="e3aed-104">In preceding tasks, you enabled Azure Active Directory Domain Services for your Azure Active Directory (Azure AD) tenant.</span></span> <span data-ttu-id="e3aed-105">hello következő feladata NT LAN Manager-(NTLM-) és a Kerberos-hitelesítés tooAzure AD tartományi szolgáltatások a hitelesítő kivonatok tooenable szinkronizálás szükséges.</span><span class="sxs-lookup"><span data-stu-id="e3aed-105">hello next task is tooenable synchronization of credential hashes required for NT LAN Manager (NTLM) and Kerberos authentication tooAzure AD Domain Services.</span></span> <span data-ttu-id="e3aed-106">Miután beállította a hitelesítő adatok szinkronizálása, felhasználók bejelentkezhetnek toohello által kezelt tartomány a vállalati hitelesítő adataikkal.</span><span class="sxs-lookup"><span data-stu-id="e3aed-106">After you've set up credential synchronization, users can sign in toohello managed domain with their corporate credentials.</span></span>

<span data-ttu-id="e3aed-107">hello lépései eltérőek csak felhőalapú felhasználói fiókok és felhasználói fiókok, amelyek alapján az Azure AD Connect használatával a helyszíni címtárral szinkronizálja.</span><span class="sxs-lookup"><span data-stu-id="e3aed-107">hello steps involved are different for cloud-only user accounts vs user accounts that are synchronized from your on-premises directory using Azure AD Connect.</span></span> <span data-ttu-id="e3aed-108">Ha az Azure AD-bérlő rendelkezik a felhő csak a felhasználók és a felhasználó számára a helyszíni Active Directory, tooperform két lépést kell.</span><span class="sxs-lookup"><span data-stu-id="e3aed-108">If your Azure AD tenant has a combination of cloud only users and users from your on-premises AD, you need tooperform both steps.</span></span>

<br>

> [!div class="op_single_selector"]
> * <span data-ttu-id="e3aed-109">**Csak felhőalapú felhasználói fiókokhoz**: [szinkronizálja a jelszavakat, csak felhőalapú felhasználói fiókok tooyour felügyelt tartomány számára](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="e3aed-109">**Cloud-only user accounts**: [Synchronize passwords for cloud-only user accounts tooyour managed domain](active-directory-ds-getting-started-password-sync.md)</span></span>
> * <span data-ttu-id="e3aed-110">**A helyszíni felhasználói fiókok**: [szinkronizálja a jelszavakat a felhasználói fiókok szinkronizálása a helyszíni AD tooyour felügyelt tartományhoz](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span><span class="sxs-lookup"><span data-stu-id="e3aed-110">**On-premises user accounts**: [Synchronize passwords for user accounts synced from your on-premises AD tooyour managed domain](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span></span>
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a><span data-ttu-id="e3aed-111">5. feladat: jelszó szinkronizálási tooyour kezelt tartományi felhasználói fiókok szinkronizálása megtörtént-e a helyszíni engedélyezése AD</span><span class="sxs-lookup"><span data-stu-id="e3aed-111">Task 5: enable password synchronization tooyour managed domain for user accounts synced with your on-premises AD</span></span>
<span data-ttu-id="e3aed-112">A szinkronizált Azure AD-bérlő toosynchronize van beállítva a szervezete helyszíni címtár Azure AD Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="e3aed-112">A synced Azure AD tenant is set toosynchronize with your organization's on-premises directory using Azure AD Connect.</span></span> <span data-ttu-id="e3aed-113">Alapértelmezés szerint az Azure AD Connect nem szinkronizálja az NTLM és Kerberos hitelesítő kivonatok tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="e3aed-113">By default, Azure AD Connect does not synchronize NTLM and Kerberos credential hashes tooAzure AD.</span></span> <span data-ttu-id="e3aed-114">toouse Azure AD tartományi szolgáltatásokat, tooconfigure az Azure AD Connect toosynchronize szükséges hitelesítő kivonatokat NTLM és Kerberos hitelesítéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="e3aed-114">toouse Azure AD Domain Services, you need tooconfigure Azure AD Connect toosynchronize credential hashes required for NTLM and Kerberos authentication.</span></span> <span data-ttu-id="e3aed-115">a lépéseket követve hello hello szükséges hitelesítő kivonatokat a helyszíni címtár tooyour Azure AD-bérlővel való szinkronizálásának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e3aed-115">hello following steps enable synchronization of hello required credential hashes from your on-premises directory tooyour Azure AD tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="e3aed-116">Ha a szervezete helyszíni címtárat, a futnia kell a szinkronizált felhasználói fiókok a NTLM és Kerberos-kivonatok rendelés toouse hello felügyelt tartomány szinkronizálásának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e3aed-116">If your organization has user accounts that are synchronized from your on-premises directory, your must enable synchronization of NTLM and Kerberos hashes in order toouse hello managed domain.</span></span> <span data-ttu-id="e3aed-117">A szinkronizált felhasználói fiók pedig egy fiókot, amelyet a helyszíni címtár jött létre, és szinkronizálva az Azure AD Connect használatával tooyour Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="e3aed-117">A synced user account is an account that was created in your on-premises directory and is synchronized tooyour Azure AD tenant using Azure AD Connect.</span></span>
>
>

### <a name="install-or-update-azure-ad-connect"></a><span data-ttu-id="e3aed-118">Azure AD Connect telepítése vagy frissítése</span><span class="sxs-lookup"><span data-stu-id="e3aed-118">Install or update Azure AD Connect</span></span>
<span data-ttu-id="e3aed-119">Telepítse a legújabb ajánlott hello kiadásban az Azure AD Connect olyan tartományhoz csatlakoztatott számítógép.</span><span class="sxs-lookup"><span data-stu-id="e3aed-119">Install hello latest recommended release of Azure AD Connect on a domain joined computer.</span></span> <span data-ttu-id="e3aed-120">Ha az Azure AD Connect a telepítő egy meglévő példányát, tooupdate kell azt az Azure AD Connect toouse hello letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="e3aed-120">If you have an existing instance of Azure AD Connect setup, you need tooupdate it toouse hello latest version of Azure AD Connect.</span></span> <span data-ttu-id="e3aed-121">ismert problémák/hibák, lehetséges, hogy már rögzített, tooavoid biztosítja, hogy mindig hello Azure AD Connect legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="e3aed-121">tooavoid known issues/bugs that may have already been fixed, ensure you always use hello latest version of Azure AD Connect.</span></span>

<span data-ttu-id="e3aed-122">**[Az Azure AD Connect letöltése](http://www.microsoft.com/download/details.aspx?id=47594)**</span><span class="sxs-lookup"><span data-stu-id="e3aed-122">**[Download Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**</span></span>

<span data-ttu-id="e3aed-123">Ajánlott verzió: **1.1.553.0** – közzététel dátuma: 2017. június 27.</span><span class="sxs-lookup"><span data-stu-id="e3aed-123">Recommended version: **1.1.553.0** - published on June 27, 2017.</span></span>

> [!WARNING]
> <span data-ttu-id="e3aed-124">Telepítenie kell a hello legújabb ajánlott az Azure AD Connect tooenable hello örökölt jelszó (NTLM és Kerberos hitelesítéshez szükséges) hitelesítő toosynchronize tooyour az Azure AD bérlő kiadásában.</span><span class="sxs-lookup"><span data-stu-id="e3aed-124">You MUST install hello latest recommended release of Azure AD Connect tooenable hello legacy password credentials (required for NTLM and Kerberos authentication) toosynchronize tooyour Azure AD tenant.</span></span> <span data-ttu-id="e3aed-125">Ez a funkció nem érhető el a korábbi kiadásokban az Azure AD Connect vagy hello örökölt DirSync eszközzel.</span><span class="sxs-lookup"><span data-stu-id="e3aed-125">This functionality is not available in prior releases of Azure AD Connect or with hello legacy DirSync tool.</span></span>
>
>

<span data-ttu-id="e3aed-126">Az Azure AD Connect telepítési útmutatás a hello következő érhető el – a cikk [Ismerkedés az Azure AD Connect](../active-directory/active-directory-aadconnect.md)</span><span class="sxs-lookup"><span data-stu-id="e3aed-126">Installation instructions for Azure AD Connect are available in hello following article - [Getting started with Azure AD Connect](../active-directory/active-directory-aadconnect.md)</span></span>

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-tooazure-ad"></a><span data-ttu-id="e3aed-127">Az NTLM és Kerberos hitelesítő kivonatok tooAzure AD szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e3aed-127">Enable synchronization of NTLM and Kerberos credential hashes tooAzure AD</span></span>
<span data-ttu-id="e3aed-128">Hajtsa végre a következő PowerShell-parancsfájlt minden AD-erdőben, tooforce teljes jelszó-szinkronizálás, a hello, és lehetővé teszi minden helyi felhasználói hitelesítő kivonatok toosync tooyour az Azure AD bérlői.</span><span class="sxs-lookup"><span data-stu-id="e3aed-128">Execute hello following PowerShell script on each AD forest, tooforce full password synchronization, and enable all on-premises users’ credential hashes toosync tooyour Azure AD tenant.</span></span> <span data-ttu-id="e3aed-129">Ez a parancsfájl lehetővé teszi, hogy az NTLM/Kerberos hitelesítési toobe szinkronizált tooyour az Azure AD-bérlő szükséges hello hitelesítő kivonatokat.</span><span class="sxs-lookup"><span data-stu-id="e3aed-129">This script enables hello credential hashes required for NTLM/Kerberos authentication toobe synchronized tooyour Azure AD tenant.</span></span>

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

<span data-ttu-id="e3aed-130">A címtár hello méretétől függően (felhasználók, száma, csoportok stb), a hitelesítő adatok szinkronizálását csak tooAzure AD időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e3aed-130">Depending on hello size of your directory (number of users, groups etc.), synchronization of credential hashes tooAzure AD takes time.</span></span> <span data-ttu-id="e3aed-131">hello jelszavak használhatóvá válnak az Azure AD tartományi szolgáltatások által kezelt tartomány hello hello hitelesítő kivonatokat szinkronizálta tooAzure AD követően rövid időn belül.</span><span class="sxs-lookup"><span data-stu-id="e3aed-131">hello passwords will be usable on hello Azure AD Domain Services managed domain shortly after hello credential hashes have synchronized tooAzure AD.</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="e3aed-132">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="e3aed-132">Related Content</span></span>
* [<span data-ttu-id="e3aed-133">Engedélyezze a jelszó-szinkronizálás tooAAD tartományi szolgáltatásokra kizárólag felhőalapú Azure AD-címtár</span><span class="sxs-lookup"><span data-stu-id="e3aed-133">Enable password synchronization tooAAD Domain Services for a cloud-only Azure AD directory</span></span>](active-directory-ds-getting-started-password-sync.md)
* [<span data-ttu-id="e3aed-134">Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="e3aed-134">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="e3aed-135">Csatlakozás egy Windows virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="e3aed-135">Join a Windows virtual machine tooan Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* [<span data-ttu-id="e3aed-136">Csatlakozás a Red Hat Enterprise Linux virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="e3aed-136">Join a Red Hat Enterprise Linux virtual machine tooan Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
