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
ms.openlocfilehash: 947ea3c9d789ecf5a754001aafcda6f8bcd41047
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-password-synchronization-to-azure-active-directory-domain-services"></a><span data-ttu-id="627fb-103">Jelszavak szinkronizálásának engedélyezése az Azure Active Directory Domain Services tartományi szolgáltatásokra</span><span class="sxs-lookup"><span data-stu-id="627fb-103">Enable password synchronization to Azure Active Directory Domain Services</span></span>
<span data-ttu-id="627fb-104">Az előző feladatokban engedélyezte az Active Directory Domain Servicest az Azure Active Directory (Azure AD) bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="627fb-104">In preceding tasks, you enabled Azure Active Directory Domain Services for your Azure Active Directory (Azure AD) tenant.</span></span> <span data-ttu-id="627fb-105">A következő feladat az NT LAN Manager (NTLM) és Kerberos hitelesítésiadat-kivonatok Azure AD tartományi szolgáltatásokkal való szinkronizálásának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="627fb-105">The next task is to enable synchronization of credential hashes required for NT LAN Manager (NTLM) and Kerberos authentication to Azure AD Domain Services.</span></span> <span data-ttu-id="627fb-106">A bejelentkezési adatok szinkronizálásának beállítását követően a felhasználók a vállalati hitelesítői adataikkal jelentkezhetnek be a felügyelt tartományba.</span><span class="sxs-lookup"><span data-stu-id="627fb-106">After you've set up credential synchronization, users can sign in to the managed domain with their corporate credentials.</span></span>

<span data-ttu-id="627fb-107">A folyamat lépései eltérőek a csak felhőalapú felhasználói fiókok és a helyszíni könyvtárból az Azure AD Connect használatával szinkronizált felhasználói fiókok esetében.</span><span class="sxs-lookup"><span data-stu-id="627fb-107">The steps involved are different for cloud-only user accounts vs user accounts that are synchronized from your on-premises directory using Azure AD Connect.</span></span> <span data-ttu-id="627fb-108">Ha az Azure AD-bérlő csak felhőalapú és a helyszíni AD-ből származó felhasználókkal is rendelkezik, mindkét lépést végre kell hajtania.</span><span class="sxs-lookup"><span data-stu-id="627fb-108">If your Azure AD tenant has a combination of cloud only users and users from your on-premises AD, you need to perform both steps.</span></span>

<br>

> [!div class="op_single_selector"]
> * <span data-ttu-id="627fb-109">**Csak felhőalapú felhasználói fiókok**: [Jelszavak szinkronizálása a felügyelt tartományra csak felhőalapú felhasználói fiókok esetében](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="627fb-109">**Cloud-only user accounts**: [Synchronize passwords for cloud-only user accounts to your managed domain](active-directory-ds-getting-started-password-sync.md)</span></span>
> * <span data-ttu-id="627fb-110">**Helyszíni felhasználói fiókok**: [Jelszavak szinkronizálása a felügyelt tartományra a helyszíni AD-ből szinkronizált felhasználói fiókok esetében](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span><span class="sxs-lookup"><span data-stu-id="627fb-110">**On-premises user accounts**: [Synchronize passwords for user accounts synced from your on-premises AD to your managed domain](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span></span>
>
>

<br>

## <a name="task-5-enable-password-synchronization-to-your-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a><span data-ttu-id="627fb-111">5. feladat: jelszavak szinkronizálásának engedélyezése a felügyelt tartományra a helyszíni AD-vel szinkronizált felhasználói fiókok számára</span><span class="sxs-lookup"><span data-stu-id="627fb-111">Task 5: enable password synchronization to your managed domain for user accounts synced with your on-premises AD</span></span>
<span data-ttu-id="627fb-112">A szinkronizált Azure AD-bérlő a szervezet helyi címtárával való szinkronizálásra van beállítva az Azure AD Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="627fb-112">A synced Azure AD tenant is set to synchronize with your organization's on-premises directory using Azure AD Connect.</span></span> <span data-ttu-id="627fb-113">Az Azure AD Connect alapértelmezés szerint nem szinkronizálja az NTLM és Kerberos hitelesítő adatok kivonatait az Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="627fb-113">By default, Azure AD Connect does not synchronize NTLM and Kerberos credential hashes to Azure AD.</span></span> <span data-ttu-id="627fb-114">Az Azure AD tartományi szolgáltatások használatához az Azure AD Connectet úgy kell konfigurálni, hogy szinkronizálja az NTLM- és Kerberos-hitelesítéshez szükséges hitelesítőadat-kivonatokat.</span><span class="sxs-lookup"><span data-stu-id="627fb-114">To use Azure AD Domain Services, you need to configure Azure AD Connect to synchronize credential hashes required for NTLM and Kerberos authentication.</span></span> <span data-ttu-id="627fb-115">Az alábbi lépésekkel engedélyezheti a helyszíni címtárból származó kivonatok Azure AD-bérlővel való szinkronizálását.</span><span class="sxs-lookup"><span data-stu-id="627fb-115">The following steps enable synchronization of the required credential hashes from your on-premises directory to your Azure AD tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="627fb-116">Ha a szervezet rendelkezik a helyszíni címtárból szinkronizált felhasználói fiókokkal, akkor a felügyelt tartomány használatához engedélyeznie kell az NTLM- és Kerberos-kivonatok szinkronizálását.</span><span class="sxs-lookup"><span data-stu-id="627fb-116">If your organization has user accounts that are synchronized from your on-premises directory, your must enable synchronization of NTLM and Kerberos hashes in order to use the managed domain.</span></span> <span data-ttu-id="627fb-117">A szinkronizált felhasználói fiókok olyan fiókok, amelyek a helyszíni címtárban lettek létrehozva, és az Azure AD Connect használatával szinkronizálnak az Azure AD-bérlővel.</span><span class="sxs-lookup"><span data-stu-id="627fb-117">A synced user account is an account that was created in your on-premises directory and is synchronized to your Azure AD tenant using Azure AD Connect.</span></span>
>
>

### <a name="install-or-update-azure-ad-connect"></a><span data-ttu-id="627fb-118">Azure AD Connect telepítése vagy frissítése</span><span class="sxs-lookup"><span data-stu-id="627fb-118">Install or update Azure AD Connect</span></span>
<span data-ttu-id="627fb-119">Telepítse az Azure AD Connect legújabb ajánlott kiadását egy tartományhoz csatlakoztatott számítógépre.</span><span class="sxs-lookup"><span data-stu-id="627fb-119">Install the latest recommended release of Azure AD Connect on a domain joined computer.</span></span> <span data-ttu-id="627fb-120">Ha az Azure AD Connect egy példánya már telepítve van, frissítse az Azure AD Connect legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="627fb-120">If you have an existing instance of Azure AD Connect setup, you need to update it to use the latest version of Azure AD Connect.</span></span> <span data-ttu-id="627fb-121">A már ismert és esetleg már javított problémák/hibák elkerülése érdekében győződjön meg arról, hogy az Azure AD Connect legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="627fb-121">To avoid known issues/bugs that may have already been fixed, ensure you always use the latest version of Azure AD Connect.</span></span>

<span data-ttu-id="627fb-122">**[Az Azure AD Connect letöltése](http://www.microsoft.com/download/details.aspx?id=47594)**</span><span class="sxs-lookup"><span data-stu-id="627fb-122">**[Download Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**</span></span>

<span data-ttu-id="627fb-123">Ajánlott verzió: **1.1.553.0** – közzététel dátuma: 2017. június 27.</span><span class="sxs-lookup"><span data-stu-id="627fb-123">Recommended version: **1.1.553.0** - published on June 27, 2017.</span></span>

> [!WARNING]
> <span data-ttu-id="627fb-124">Az Azure AD Connect legújabb ajánlott verzióját FELTÉTLENÜL telepítenie kell ahhoz, hogy az örökölt (NTLM és Kerberos hitelesítéshez szükséges) jelszavas hitelesítő adatokat szinkronizálni lehessen az Azure AD bérlőhöz.</span><span class="sxs-lookup"><span data-stu-id="627fb-124">You MUST install the latest recommended release of Azure AD Connect to enable the legacy password credentials (required for NTLM and Kerberos authentication) to synchronize to your Azure AD tenant.</span></span> <span data-ttu-id="627fb-125">Ez a funkció nem érhető el az Azure AD Connect korábbi verzióiban vagy az örökölt DirSync eszközzel.</span><span class="sxs-lookup"><span data-stu-id="627fb-125">This functionality is not available in prior releases of Azure AD Connect or with the legacy DirSync tool.</span></span>
>
>

<span data-ttu-id="627fb-126">Az Azure AD Connect telepítési utasításai a következő cikkben érhetők el: [Ismerkedés az Azure AD Connecttel](../active-directory/active-directory-aadconnect.md)</span><span class="sxs-lookup"><span data-stu-id="627fb-126">Installation instructions for Azure AD Connect are available in the following article - [Getting started with Azure AD Connect](../active-directory/active-directory-aadconnect.md)</span></span>

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a><span data-ttu-id="627fb-127">NTLM és Kerberos hitelesítőadat-kivonatok Azure AD-val történő szinkronizálásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="627fb-127">Enable synchronization of NTLM and Kerberos credential hashes to Azure AD</span></span>
<span data-ttu-id="627fb-128">A teljes jelszó-szinkronizálás és a helyszíni felhasználók jelszókivonatainak Azure AD-bérlővel való szinkronizálásának engedélyezéséhez hajtsa végre a következő PowerShell-parancsfájlt minden AD-erdőben.</span><span class="sxs-lookup"><span data-stu-id="627fb-128">Execute the following PowerShell script on each AD forest, to force full password synchronization, and enable all on-premises users’ credential hashes to sync to your Azure AD tenant.</span></span> <span data-ttu-id="627fb-129">A parancsfájl engedélyezi az NTLM-/Kerberos-hitelesítéshez szükséges hitelesítő adatok szinkronizálását az Azure AD-bérlővel.</span><span class="sxs-lookup"><span data-stu-id="627fb-129">This script enables the credential hashes required for NTLM/Kerberos authentication to be synchronized to your Azure AD tenant.</span></span>

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

<span data-ttu-id="627fb-130">A címtár méretétől (felhasználók, csoportok stb. száma) függ, hogy a jelszókivonatok Azure AD-val történő szinkronizálása mennyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="627fb-130">Depending on the size of your directory (number of users, groups etc.), synchronization of credential hashes to Azure AD takes time.</span></span> <span data-ttu-id="627fb-131">A jelszavak a hitelesítő kivonatok Azure AD-hoz történő szinkronizálását követően rövid időn belül használhatóvá válnak az Azure AD tartományi szolgáltatások által kezelt tartományban.</span><span class="sxs-lookup"><span data-stu-id="627fb-131">The passwords will be usable on the Azure AD Domain Services managed domain shortly after the credential hashes have synchronized to Azure AD.</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="627fb-132">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="627fb-132">Related Content</span></span>
* [<span data-ttu-id="627fb-133">Jelszó-szinkronizálás engedélyezése AAD tartományi szolgáltatásokra csak felhőalapú Azure AD címtárhoz</span><span class="sxs-lookup"><span data-stu-id="627fb-133">Enable password synchronization to AAD Domain Services for a cloud-only Azure AD directory</span></span>](active-directory-ds-getting-started-password-sync.md)
* [<span data-ttu-id="627fb-134">Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="627fb-134">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="627fb-135">Windows virtuális gépek csatlakoztatása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="627fb-135">Join a Windows virtual machine to an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* [<span data-ttu-id="627fb-136">Red Hat Enterprise Linux virtuális gépek csatlakoztatása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="627fb-136">Join a Red Hat Enterprise Linux virtual machine to an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
