---
title: "Hibaelhárítás az Azure Active Directory-tevékenység tartalomcsomag hibákat naplózza |} Microsoft Docs"
description: "Biztosít javítja őket az Azure Active Directory-tevékenység tartalomcsomag és lépéseket hibaüzenetek listáját."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c880e9eb6d48bd1e38075fbd867d3906ec67b547
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a><span data-ttu-id="65027-103">A tartalomcsomag hibákat naplózza hibaelhárítása az Azure Active Directory-tevékenység</span><span class="sxs-lookup"><span data-stu-id="65027-103">Troubleshooting Azure Active Directory Activity logs content pack errors</span></span> 


<span data-ttu-id="65027-104">A Power BI tartalomcsomag használata az Azure Active Directory előzetes, esetén lehetséges, hogy futtatja-e be a következő hibákat:</span><span class="sxs-lookup"><span data-stu-id="65027-104">When working with the Power BI Content Pack for Azure Active Directory Preview, it is possible that you run into the following errors:</span></span> 

- [<span data-ttu-id="65027-105">A frissítés nem sikerült</span><span class="sxs-lookup"><span data-stu-id="65027-105">Refresh failed</span></span>](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [<span data-ttu-id="65027-106">Az adatforrás hitelesítő adatainak frissítése sikertelen</span><span class="sxs-lookup"><span data-stu-id="65027-106">Failed to update data source credentials</span></span>](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [<span data-ttu-id="65027-107">Adatok importálása túl sokáig tart</span><span class="sxs-lookup"><span data-stu-id="65027-107">Importing of data is taking too long</span></span>](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
<span data-ttu-id="65027-108">Ez a témakör a lehetséges okok és ezek a hibák megoldásával kapcsolatos információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="65027-108">This topic provides you with information about the possible causes and how to fix these errors.</span></span>
 
## <a name="refresh-failed"></a><span data-ttu-id="65027-109">A frissítés nem sikerült</span><span class="sxs-lookup"><span data-stu-id="65027-109">Refresh failed</span></span> 
 
<span data-ttu-id="65027-110">**Hogyan van illesztett hibára**: e-mailt a Power bi-ban vagy a frissítési előzmények "sikertelen" állapota.</span><span class="sxs-lookup"><span data-stu-id="65027-110">**How this error is surfaced**: Email from Power BI or failed status in the refresh history.</span></span> 


| <span data-ttu-id="65027-111">Ok</span><span class="sxs-lookup"><span data-stu-id="65027-111">Cause</span></span> | <span data-ttu-id="65027-112">Megoldásával kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="65027-112">How to fix</span></span> |
| ---   | ---        |
| <span data-ttu-id="65027-113">Frissítési hiba hibákat is okozhat, ha a tartalomcsomag csatlakozik a felhasználói hitelesítő adatok alaphelyzetbe állítása, de a kapcsolat beállításai nem frissíthetők a tartalomcsomag.</span><span class="sxs-lookup"><span data-stu-id="65027-113">Refresh failure errors can be caused when the credentials of the users connecting to the content pack have been reset but not updated in the connection settings of the of the content pack.</span></span> | <span data-ttu-id="65027-114">A Power bi-ban keresse meg az adatkészletnek az Azure Active Directory-tevékenység naplók irányítópult (Azure Active Directory tevékenységi naplóit) megfelelő, válassza ki a frissítésütemezés, és adja meg Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="65027-114">In Power BI, locate the dataset corresponding to the Azure Active Directory Activity logs dashboard (Azure Active Directory Activity logs), choose schedule refresh, and then enter your Azure AD credentials.</span></span> |
| <span data-ttu-id="65027-115">Egy frissítés sikertelen lehet az alapul szolgáló tartalomcsomag adatok hibái miatt.</span><span class="sxs-lookup"><span data-stu-id="65027-115">A refresh can fail due to data issues in the underlying content pack.</span></span> | <span data-ttu-id="65027-116">A fájl egy támogatási jegy.</span><span class="sxs-lookup"><span data-stu-id="65027-116">File a support ticket.</span></span> <span data-ttu-id="65027-117">További részletekért lásd: [hogyan kérhet támogatást az Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="65027-117">For more details, see [How to get support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 
 
## <a name="failed-to-update-data-source-credentials"></a><span data-ttu-id="65027-118">Az adatforrás hitelesítő adatainak frissítése sikertelen</span><span class="sxs-lookup"><span data-stu-id="65027-118">Failed to update data source credentials</span></span> 
 
<span data-ttu-id="65027-119">**Hogyan van illesztett hibára**: A Power BI-ban az Azure Active Directory-tevékenység naplókat (preview) tartalomcsomag való kapcsolódás esetén.</span><span class="sxs-lookup"><span data-stu-id="65027-119">**How this error is surfaced**: In Power BI, when you are connecting to the Azure Active Directory Activity logs (preview) content pack.</span></span> 

| <span data-ttu-id="65027-120">Ok</span><span class="sxs-lookup"><span data-stu-id="65027-120">Cause</span></span> | <span data-ttu-id="65027-121">Megoldásával kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="65027-121">How to fix</span></span> |
| ---   | ---        |
| <span data-ttu-id="65027-122">A csatlakozó felhasználó, sem globális rendszergazda, és nem biztonsági olvasó vagy a biztonsági rendszergazdával.</span><span class="sxs-lookup"><span data-stu-id="65027-122">The connecting user is neither a global admin nor a security reader or a security admin.</span></span> | <span data-ttu-id="65027-123">Olyan fiókot, amely globális rendszergazda vagy egy biztonsági olvasó vagy egy biztonsági rendszergazdai a tartalomcsomagok eléréséhez használjon.</span><span class="sxs-lookup"><span data-stu-id="65027-123">Use an account that is either a global admin or a security reader or a security admin to access the content packs.</span></span> |
| <span data-ttu-id="65027-124">A bérlő nincs Premium-bérlő, vagy nem rendelkezik a fájl Premium licenccel rendelkező legalább egy felhasználót.</span><span class="sxs-lookup"><span data-stu-id="65027-124">Your tenant is not a Premium tenant or doesn't have at least one user with Premium license File.</span></span> | <span data-ttu-id="65027-125">A fájl egy támogatási jegy.</span><span class="sxs-lookup"><span data-stu-id="65027-125">File a support ticket.</span></span> <span data-ttu-id="65027-126">További részletekért lásd: [hogyan kérhet támogatást az Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="65027-126">For more details, see [How to get support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 

 

## <a name="importing-of-data-is-taking-too-long"></a><span data-ttu-id="65027-127">Adatok importálása túl sokáig tart</span><span class="sxs-lookup"><span data-stu-id="65027-127">Importing of data is taking too long</span></span> 
 
<span data-ttu-id="65027-128">**Hogyan van illesztett hibára**: A Power BI-ban a tartalomcsomaghoz csatlakozás után az importálási folyamat elindítja az irányítópulton előkészítése az Azure Active Directory tevékenységnapló.</span><span class="sxs-lookup"><span data-stu-id="65027-128">**How this error is surfaced**: In Power BI, once you have connected your content pack, the data import process starts to prepare your dashboard for Azure Active Directory Activity log.</span></span> <span data-ttu-id="65027-129">Az üzenet jelenik meg: "*... adatok importálása* "</span><span class="sxs-lookup"><span data-stu-id="65027-129">You see the message: “*Importing data...*”</span></span>  

| <span data-ttu-id="65027-130">Ok</span><span class="sxs-lookup"><span data-stu-id="65027-130">Cause</span></span> | <span data-ttu-id="65027-131">Megoldásával kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="65027-131">How to fix</span></span> |
| ---   | ---        |
| <span data-ttu-id="65027-132">A bérlő méretétől függően ez a lépés sikerült igénybe vehet néhány perc és 30 perc.</span><span class="sxs-lookup"><span data-stu-id="65027-132">Depending on the size of your tenant, this step could take anywhere from a few mins to 30 minutes.</span></span> | <span data-ttu-id="65027-133">Csak türelemmel.</span><span class="sxs-lookup"><span data-stu-id="65027-133">Just be patient.</span></span> <span data-ttu-id="65027-134">Ha az üzenet nem változtatja meg az irányítópult megjelenítése egy órán belül, adjon fájl egy támogatási jegy.</span><span class="sxs-lookup"><span data-stu-id="65027-134">If the message does not change to showing your dashboard within an hour, please file a support ticket.</span></span> <span data-ttu-id="65027-135">További részletekért lásd: [hogyan kérhet támogatást az Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="65027-135">For more details, see [How to get support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|

## <a name="next-steps"></a><span data-ttu-id="65027-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65027-136">Next steps</span></span>

<span data-ttu-id="65027-137">A Power BI tartalomcsomag az Azure Active Directory előzetes telepítéséhez kattintson [Itt](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span><span class="sxs-lookup"><span data-stu-id="65027-137">To install the Power BI Content Pack for Azure Active Directory Preview, click [here](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span></span>


