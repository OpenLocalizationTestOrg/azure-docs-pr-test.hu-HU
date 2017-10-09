---
title: "Windows 10-eszköz az Azure AD a beállítások mentése aaaSet |} Microsoft Docs"
description: "Ismerteti, hogyan felhasználók csatlakozhatnak tooAzure AD keresztül hello-beállítások menüjében."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: b844e1d9-ad43-4e4a-a398-5c4a43bf2f5c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: d6485228338db571cc01f913c99fbba49e0bb74a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a><span data-ttu-id="a581f-103">A beállítások Windows 10-es eszköz az Azure AD beállítása</span><span class="sxs-lookup"><span data-stu-id="a581f-103">Set up a Windows 10 device with Azure AD from Settings</span></span>
<span data-ttu-id="a581f-104">Ha már használja a Windows 7 vagy Windows 8 és a számítógép vagy eszköz frissített tooWindows 10, Active Directory (Azure AD) tooAzure hello beállítások menü keresztül is csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="a581f-104">If you are already using Windows 7 or Windows 8, and your computer or device has been upgraded tooWindows 10, you can join tooAzure Active Directory (Azure AD) through hello Settings menu.</span></span>

## <a name="toojoin-tooazure-ad-from-hello-settings-menu"></a><span data-ttu-id="a581f-105">toojoin tooAzure AD hello-beállítások menüjében</span><span class="sxs-lookup"><span data-stu-id="a581f-105">toojoin tooAzure AD from hello Settings menu</span></span>
1. <span data-ttu-id="a581f-106">A hello **Start** menü hello kattintson **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="a581f-106">From hello **Start** menu, click hello **Settings** charm.</span></span>
2. <span data-ttu-id="a581f-107">A **beállítások**, jelölje be **rendszer**->**kapcsolatos**->**az Azure AD Join**.</span><span class="sxs-lookup"><span data-stu-id="a581f-107">From **Settings**, select     **System**->**About**->**Join Azure AD**.</span></span>
   
   <span data-ttu-id="a581f-108"><center>
   ![Csatlakozás az Azure AD hello-beállítások menüjében](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center></span><span class="sxs-lookup"><span data-stu-id="a581f-108"><center>
![Join Azure AD from hello Settings menu](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png) </center></span></span>
3. <span data-ttu-id="a581f-109">Kattintson a **Folytatás** hello Azure AD Join üzenetablakban.</span><span class="sxs-lookup"><span data-stu-id="a581f-109">Click **Continue** in hello Azure AD Join message window.</span></span>
   
   <span data-ttu-id="a581f-110"><center>
   ![Az Azure AD JOIN üzenetablakban](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center></span><span class="sxs-lookup"><span data-stu-id="a581f-110"><center>
![Join Azure AD message window](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png) </center></span></span>
4. <span data-ttu-id="a581f-111">Adja meg a bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="a581f-111">Provide your sign-in credentials.</span></span> <span data-ttu-id="a581f-112">A bejelentkezés során tapasztal élmény tartalmazza, amelyek a szükséges toocomplete hitelesítési hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a581f-112">This sign-in experience will include all hello steps that are required toocomplete authentication.</span></span> <span data-ttu-id="a581f-113">Ha egy összevont bérlői részét képezik, a rendszergazda biztosít a szervezet által futtatott hello összevonási élmény.</span><span class="sxs-lookup"><span data-stu-id="a581f-113">If you are part of a federated tenant, your administrator will provide you with hello federation experience that's hosted by your organization.</span></span>
   <span data-ttu-id="a581f-114"><center>
   ![Adja meg a bejelentkezési hitelesítő adatok](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center></span><span class="sxs-lookup"><span data-stu-id="a581f-114"><center>
![Provide sign-in credentials](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center></span></span>
5. <span data-ttu-id="a581f-115">Ha a szervezet tooAzure AD csatlakoztatása Azure multi-factor Authentication hitelesítés van beállítva, adja meg a hello második tényezőként a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="a581f-115">If your organization has configured Azure Multi-Factor Authentication for joining tooAzure AD, provide hello second factor before proceeding.</span></span>
6. <span data-ttu-id="a581f-116">Kattintson a **elfogadás** a hello **engedélyezése a felügyelt eszközök toobe** képernyő.</span><span class="sxs-lookup"><span data-stu-id="a581f-116">Click **Accept** on hello **Allow this device toobe managed** screen.</span></span>
7. <span data-ttu-id="a581f-117">"Az eszköz már csatlakoztatott tooyour szervezet az Azure AD" üdvözlőüzenetére kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="a581f-117">You should see hello message "Your device is now joined tooyour organization in Azure AD".</span></span>

## <a name="additional-information"></a><span data-ttu-id="a581f-118">További információ</span><span class="sxs-lookup"><span data-stu-id="a581f-118">Additional information</span></span>
* [<span data-ttu-id="a581f-119">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="a581f-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="a581f-120">Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="a581f-120">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="a581f-121">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="a581f-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)
* [<span data-ttu-id="a581f-122">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="a581f-122">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)

