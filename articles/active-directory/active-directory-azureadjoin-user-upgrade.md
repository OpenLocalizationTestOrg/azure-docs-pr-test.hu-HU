---
title: "A beállítások Windows 10-es eszköz az Azure AD beállítása |} Microsoft Docs"
description: "Ismerteti, hogy a felhasználók hogyan csatlakozhatnak az Azure AD használatával a beállítások menü."
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
ms.openlocfilehash: 5a3963f16b471ad1ca8681b22a1a027935400465
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a><span data-ttu-id="55340-103">A beállítások Windows 10-es eszköz az Azure AD beállítása</span><span class="sxs-lookup"><span data-stu-id="55340-103">Set up a Windows 10 device with Azure AD from Settings</span></span>
<span data-ttu-id="55340-104">Ha már használja a Windows 7 vagy Windows 8, és a számítógép vagy eszköz frissítve lett a Windows 10-re, csatlakozhat az Azure Active Directory (Azure AD) révén a beállítások menüben.</span><span class="sxs-lookup"><span data-stu-id="55340-104">If you are already using Windows 7 or Windows 8, and your computer or device has been upgraded to Windows 10, you can join to Azure Active Directory (Azure AD) through the Settings menu.</span></span>

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a><span data-ttu-id="55340-105">Csatlakozás az Azure AD a beállítások menüből</span><span class="sxs-lookup"><span data-stu-id="55340-105">To join to Azure AD from the Settings menu</span></span>
1. <span data-ttu-id="55340-106">Az a **Start** menüben kattintson a **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="55340-106">From the **Start** menu, click the **Settings** charm.</span></span>
2. <span data-ttu-id="55340-107">A **beállítások**, jelölje be **rendszer**->**kapcsolatos**->**az Azure AD Join**.</span><span class="sxs-lookup"><span data-stu-id="55340-107">From **Settings**, select     **System**->**About**->**Join Azure AD**.</span></span>
   
   <span data-ttu-id="55340-108"><center>
   ![A beállítások menüből az Azure AD JOIN](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center></span><span class="sxs-lookup"><span data-stu-id="55340-108"><center>
![Join Azure AD from the Settings menu](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png) </center></span></span>
3. <span data-ttu-id="55340-109">Kattintson a **Folytatás** az Azure AD Join üzenetablakban.</span><span class="sxs-lookup"><span data-stu-id="55340-109">Click **Continue** in the Azure AD Join message window.</span></span>
   
   <span data-ttu-id="55340-110"><center>
   ![Az Azure AD JOIN üzenetablakban](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center></span><span class="sxs-lookup"><span data-stu-id="55340-110"><center>
![Join Azure AD message window](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png) </center></span></span>
4. <span data-ttu-id="55340-111">Adja meg a bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="55340-111">Provide your sign-in credentials.</span></span> <span data-ttu-id="55340-112">A bejelentkezés során tapasztal élmény tartalmazza azokat a lépéseket, amelyek szükségesek a teljes hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="55340-112">This sign-in experience will include all the steps that are required to complete authentication.</span></span> <span data-ttu-id="55340-113">Ha egy összevont bérlői részét képezik, a rendszergazda biztosít az Ön szervezete által működtetett összevonási élményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="55340-113">If you are part of a federated tenant, your administrator will provide you with the federation experience that's hosted by your organization.</span></span>
   <span data-ttu-id="55340-114"><center>
   ![Adja meg a bejelentkezési hitelesítő adatok](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center></span><span class="sxs-lookup"><span data-stu-id="55340-114"><center>
![Provide sign-in credentials](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center></span></span>
5. <span data-ttu-id="55340-115">Ha a szervezet az Azure AD-csatlakozás Azure multi-factor Authentication hitelesítés van beállítva, adja meg a második tényező a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="55340-115">If your organization has configured Azure Multi-Factor Authentication for joining to Azure AD, provide the second factor before proceeding.</span></span>
6. <span data-ttu-id="55340-116">Kattintson a **elfogadás** a a **felügyelhető eszköz** képernyő.</span><span class="sxs-lookup"><span data-stu-id="55340-116">Click **Accept** on the **Allow this device to be managed** screen.</span></span>
7. <span data-ttu-id="55340-117">Megjelenik az üzenet "az eszköz most már csatlakozott a szervezet Azure AD-ben".</span><span class="sxs-lookup"><span data-stu-id="55340-117">You should see the message "Your device is now joined to your organization in Azure AD".</span></span>

## <a name="additional-information"></a><span data-ttu-id="55340-118">További információ</span><span class="sxs-lookup"><span data-stu-id="55340-118">Additional information</span></span>
* [<span data-ttu-id="55340-119">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="55340-119">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="55340-120">Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="55340-120">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="55340-121">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="55340-121">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)
* [<span data-ttu-id="55340-122">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="55340-122">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)

