---
title: "az Azure MFA aaaAccess és használati jelentések |} Microsoft Docs"
description: "Ez ismerteti, hogyan toouse hello Azure multi-factor Authentication szolgáltatás - jelentéseket."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: ae7ccceca4968d7ec7cf0cb1cf9e041d9997c840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a><span data-ttu-id="17e91-103">Azure multi-factor Authentication jelentései</span><span class="sxs-lookup"><span data-stu-id="17e91-103">Reports in Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="17e91-104">Az Azure multi-factor Authentication segítségével, és a szervezet több jelentéseket biztosít.</span><span class="sxs-lookup"><span data-stu-id="17e91-104">Azure Multi-Factor Authentication provides several reports that can be used by you and your organization.</span></span> <span data-ttu-id="17e91-105">Ezek a jelentések hello multi-factor Authentication kezelési portál is elérhetőek.</span><span class="sxs-lookup"><span data-stu-id="17e91-105">These reports can be accessed through hello Multi-Factor Authentication Management Portal.</span></span> <span data-ttu-id="17e91-106">hello az alábbiakban látható hello elérhető jelentések listája:</span><span class="sxs-lookup"><span data-stu-id="17e91-106">hello following is a list of hello available reports:</span></span>

| <span data-ttu-id="17e91-107">Jelentés</span><span class="sxs-lookup"><span data-stu-id="17e91-107">Report</span></span> | <span data-ttu-id="17e91-108">Leírás</span><span class="sxs-lookup"><span data-stu-id="17e91-108">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="17e91-109">Használat</span><span class="sxs-lookup"><span data-stu-id="17e91-109">Usage</span></span> |<span data-ttu-id="17e91-110">hello használati jelentések megjelenítési adatok összesített használatát, a – felhasználói összefoglalás és a felhasználó adatait.</span><span class="sxs-lookup"><span data-stu-id="17e91-110">hello usage reports display information on overall usage, user summary, and user details.</span></span> |
| <span data-ttu-id="17e91-111">Kiszolgáló állapota</span><span class="sxs-lookup"><span data-stu-id="17e91-111">Server Status</span></span> |<span data-ttu-id="17e91-112">Ez a jelentés a multi-factor Authentication kiszolgáló a fiókjához társított hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="17e91-112">This report displays hello status of Multi-Factor Authentication Servers associated with your account.</span></span> |
| <span data-ttu-id="17e91-113">Letiltott felhasználók előzményei</span><span class="sxs-lookup"><span data-stu-id="17e91-113">Blocked User History</span></span> |<span data-ttu-id="17e91-114">Ezek a jelentések kérelmek tooblock hello előzmények megjelenítése, és a felhasználók feloldása.</span><span class="sxs-lookup"><span data-stu-id="17e91-114">These reports show hello history of requests tooblock or unblock users.</span></span> |
| <span data-ttu-id="17e91-115">Kihagyott felhasználók előzményei</span><span class="sxs-lookup"><span data-stu-id="17e91-115">Bypassed User History</span></span> |<span data-ttu-id="17e91-116">Kérelmek toobypass többtényezős hitelesítés a felhasználó telefonszámának hello előzményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="17e91-116">Shows hello history of requests toobypass Multi-Factor Authentication for a user's phone number.</span></span> |
| <span data-ttu-id="17e91-117">Csalási riasztás</span><span class="sxs-lookup"><span data-stu-id="17e91-117">Fraud Alert</span></span> |<span data-ttu-id="17e91-118">Során megadott hello dátumtartományban küldött visszaélési riasztások előzményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="17e91-118">Shows a history of fraud alerts submitted during hello date range you specified.</span></span> |
| <span data-ttu-id="17e91-119">A várólistára</span><span class="sxs-lookup"><span data-stu-id="17e91-119">Queued</span></span> |<span data-ttu-id="17e91-120">Listák sorba állított jelentések feldolgozási és azok állapotát.</span><span class="sxs-lookup"><span data-stu-id="17e91-120">Lists reports queued for processing and their status.</span></span> <span data-ttu-id="17e91-121">Hivatkozás toodownload vagy nézet hello jelentés készül róla hello jelentés befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="17e91-121">A link toodownload or view hello report is provided when hello report is complete.</span></span> |

## <a name="view-reports"></a><span data-ttu-id="17e91-122">Jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="17e91-122">View reports</span></span>
1. <span data-ttu-id="17e91-123">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="17e91-123">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="17e91-124">Hello bal oldalon válassza ki az Active Directory.</span><span class="sxs-lookup"><span data-stu-id="17e91-124">On hello left, select Active Directory.</span></span>
3. <span data-ttu-id="17e91-125">Kövesse az e két beállítás megadása, attól függően, hogy használ-e hitelesítésszolgáltatók egyikét:</span><span class="sxs-lookup"><span data-stu-id="17e91-125">Follow one of these two options, depending on whether you use Auth Providers:</span></span>
   * <span data-ttu-id="17e91-126">**1. lehetőség**: hello többtényezős hitelesítésszolgáltatók lapon. Válassza ki az MFA-szolgáltatóra, majd kattintson a hello **kezelése** hello alsó gombra.</span><span class="sxs-lookup"><span data-stu-id="17e91-126">**Option 1**: Click hello Multi-Factor Auth Providers tab. Select your MFA provider and click hello **Manage** button at hello bottom.</span></span>
   * <span data-ttu-id="17e91-127">**2. lehetőség**: válassza ki a könyvtárhoz, és lépjen toohello **konfigurálása** fülre. Hello a multi-factor authentication szakaszban válassza ki a **szolgáltatás beállításainak kezelése**.</span><span class="sxs-lookup"><span data-stu-id="17e91-127">**Option 2**: Select your directory and go toohello **Configure** tab. Under hello multi-factor authentication section, select **Manage service settings**.</span></span> <span data-ttu-id="17e91-128">Hello a hello multi-factor Authentication szolgáltatás beállításainak lap alján kattintson hello Ugrás toohello portal-hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="17e91-128">At hello bottom of hello MFA Service Settings page, click hello Go toohello portal link.</span></span>
4. <span data-ttu-id="17e91-129">Hello Azure multi-factor Authentication kezelési portál, jelölje ki hello típusú kívánt jelentést hello **megtekint egy jelentést** bal oldali navigációs hello című szakasza.</span><span class="sxs-lookup"><span data-stu-id="17e91-129">In hello Azure Multi-Factor Authentication Management Portal, select hello type of report you want from hello **View a Report** section in hello left navigation.</span></span>

<span data-ttu-id="17e91-130"><center>![Felhő](./media/multi-factor-authentication-manage-reports/report.png)</center></span><span class="sxs-lookup"><span data-stu-id="17e91-130"><center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center></span></span>


<span data-ttu-id="17e91-131">**További források**</span><span class="sxs-lookup"><span data-stu-id="17e91-131">**Additional Resources**</span></span>

* [<span data-ttu-id="17e91-132">A felhasználók számára</span><span class="sxs-lookup"><span data-stu-id="17e91-132">For Users</span></span>](end-user/multi-factor-authentication-end-user.md)
* [<span data-ttu-id="17e91-133">Az Azure többtényezős hitelesítés az MSDN webhelyen</span><span class="sxs-lookup"><span data-stu-id="17e91-133">Azure Multi-Factor Authentication on MSDN</span></span>](https://msdn.microsoft.com/library/azure/dn249471.aspx)
