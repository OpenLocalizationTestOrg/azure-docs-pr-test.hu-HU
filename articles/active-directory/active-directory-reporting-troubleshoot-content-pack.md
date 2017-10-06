---
title: "Hibaelhárítás az Azure Active Directory-tevékenység tartalomcsomag hibákat naplózza |} Microsoft Docs"
description: "A tartalom-es számú hello Azure Active Directory-tevékenység pack és lépéseket rendszerhez készült toofix hibaüzenetek listáját biztosít számukra."
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
ms.openlocfilehash: 325de65ff1572a2f8f8319c0a52350bda03af3de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a><span data-ttu-id="ab9ec-103">A tartalomcsomag hibákat naplózza hibaelhárítása az Azure Active Directory-tevékenység</span><span class="sxs-lookup"><span data-stu-id="ab9ec-103">Troubleshooting Azure Active Directory Activity logs content pack errors</span></span> 


<span data-ttu-id="ab9ec-104">A Power BI-tartalomcsomag hello használata az Azure Active Directory előzetes, esetén lehetséges, hogy futtassa a következő hibák hello:</span><span class="sxs-lookup"><span data-stu-id="ab9ec-104">When working with hello Power BI Content Pack for Azure Active Directory Preview, it is possible that you run into hello following errors:</span></span> 

- [<span data-ttu-id="ab9ec-105">A frissítés nem sikerült</span><span class="sxs-lookup"><span data-stu-id="ab9ec-105">Refresh failed</span></span>](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [<span data-ttu-id="ab9ec-106">Nem sikerült tooupdate adatforrás hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="ab9ec-106">Failed tooupdate data source credentials</span></span>](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [<span data-ttu-id="ab9ec-107">Adatok importálása túl sokáig tart</span><span class="sxs-lookup"><span data-stu-id="ab9ec-107">Importing of data is taking too long</span></span>](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
<span data-ttu-id="ab9ec-108">Ez a témakör információkat hello lehetséges okok és hogyan toofix ezeket a hibákat.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-108">This topic provides you with information about hello possible causes and how toofix these errors.</span></span>
 
## <a name="refresh-failed"></a><span data-ttu-id="ab9ec-109">A frissítés nem sikerült</span><span class="sxs-lookup"><span data-stu-id="ab9ec-109">Refresh failed</span></span> 
 
<span data-ttu-id="ab9ec-110">**Hogyan van illesztett hibára**: e-mailt a Power bi-ban vagy a hello frissítési előzmények "sikertelen" állapota.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-110">**How this error is surfaced**: Email from Power BI or failed status in hello refresh history.</span></span> 


| <span data-ttu-id="ab9ec-111">Ok</span><span class="sxs-lookup"><span data-stu-id="ab9ec-111">Cause</span></span> | <span data-ttu-id="ab9ec-112">Hogyan toofix</span><span class="sxs-lookup"><span data-stu-id="ab9ec-112">How toofix</span></span> |
| ---   | ---        |
| <span data-ttu-id="ab9ec-113">Frissítési hiba hibákat is okozhat, ha hello azon felhasználók hitelesítési adatait hello toohello tartalomcsomag kapcsolódás alaphelyzetbe állítása, de nem frissíthetők a hello tartalomcsomag hello hello kapcsolódási beállításait.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-113">Refresh failure errors can be caused when hello credentials of hello users connecting toohello content pack have been reset but not updated in hello connection settings of hello of hello content pack.</span></span> | <span data-ttu-id="ab9ec-114">A Power bi-ban keresse meg a megfelelő hello dataset toohello Azure Active Directory-tevékenység naplók irányítópult (Azure Active Directory tevékenységi naplóit), válassza ki a frissítésütemezés és írja be az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-114">In Power BI, locate hello dataset corresponding toohello Azure Active Directory Activity logs dashboard (Azure Active Directory Activity logs), choose schedule refresh, and then enter your Azure AD credentials.</span></span> |
| <span data-ttu-id="ab9ec-115">Frissítés az alapul szolgáló tartalomcsomag hello toodata problémák miatt sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-115">A refresh can fail due toodata issues in hello underlying content pack.</span></span> | <span data-ttu-id="ab9ec-116">A fájl egy támogatási jegy.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-116">File a support ticket.</span></span> <span data-ttu-id="ab9ec-117">További részletekért lásd: [hogyan támogatják a tooget az Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="ab9ec-117">For more details, see [How tooget support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 
 
## <a name="failed-tooupdate-data-source-credentials"></a><span data-ttu-id="ab9ec-118">Nem sikerült tooupdate adatforrás hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="ab9ec-118">Failed tooupdate data source credentials</span></span> 
 
<span data-ttu-id="ab9ec-119">**Hogyan van illesztett hibára**: A Power BI-ban toohello Azure Active Directory-tevékenység naplókat (preview) tartalomcsomag kapcsolódás esetén.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-119">**How this error is surfaced**: In Power BI, when you are connecting toohello Azure Active Directory Activity logs (preview) content pack.</span></span> 

| <span data-ttu-id="ab9ec-120">Ok</span><span class="sxs-lookup"><span data-stu-id="ab9ec-120">Cause</span></span> | <span data-ttu-id="ab9ec-121">Hogyan toofix</span><span class="sxs-lookup"><span data-stu-id="ab9ec-121">How toofix</span></span> |
| ---   | ---        |
| <span data-ttu-id="ab9ec-122">hello csatlakozó felhasználó, sem globális rendszergazda, és nem biztonsági olvasó vagy a biztonsági rendszergazdával.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-122">hello connecting user is neither a global admin nor a security reader or a security admin.</span></span> | <span data-ttu-id="ab9ec-123">Olyan fiókot használjon, amely globális rendszergazda vagy egy biztonsági olvasó, vagy a biztonsági rendszergazda tooaccess hello tartalomcsomagok.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-123">Use an account that is either a global admin or a security reader or a security admin tooaccess hello content packs.</span></span> |
| <span data-ttu-id="ab9ec-124">A bérlő nincs Premium-bérlő, vagy nem rendelkezik a fájl Premium licenccel rendelkező legalább egy felhasználót.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-124">Your tenant is not a Premium tenant or doesn't have at least one user with Premium license File.</span></span> | <span data-ttu-id="ab9ec-125">A fájl egy támogatási jegy.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-125">File a support ticket.</span></span> <span data-ttu-id="ab9ec-126">További részletekért lásd: [hogyan támogatják a tooget az Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="ab9ec-126">For more details, see [How tooget support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 

 

## <a name="importing-of-data-is-taking-too-long"></a><span data-ttu-id="ab9ec-127">Adatok importálása túl sokáig tart</span><span class="sxs-lookup"><span data-stu-id="ab9ec-127">Importing of data is taking too long</span></span> 
 
<span data-ttu-id="ab9ec-128">**Hogyan van illesztett hibára**: A Power bi-ban, ha csatlakozott a tartalomcsomag hello adatok importálási folyamat elindul tooprepare az Azure Active Directory tevékenységnapló irányítópult.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-128">**How this error is surfaced**: In Power BI, once you have connected your content pack, hello data import process starts tooprepare your dashboard for Azure Active Directory Activity log.</span></span> <span data-ttu-id="ab9ec-129">Hello üzenet jelenik meg: "*... adatok importálása* "</span><span class="sxs-lookup"><span data-stu-id="ab9ec-129">You see hello message: “*Importing data...*”</span></span>  

| <span data-ttu-id="ab9ec-130">Ok</span><span class="sxs-lookup"><span data-stu-id="ab9ec-130">Cause</span></span> | <span data-ttu-id="ab9ec-131">Hogyan toofix</span><span class="sxs-lookup"><span data-stu-id="ab9ec-131">How toofix</span></span> |
| ---   | ---        |
| <span data-ttu-id="ab9ec-132">Attól függően, hogy a bérlő hello méretét Ez a lépés sikerült igénybe vehet néhány perc too30 perc múlva.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-132">Depending on hello size of your tenant, this step could take anywhere from a few mins too30 minutes.</span></span> | <span data-ttu-id="ab9ec-133">Csak türelemmel.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-133">Just be patient.</span></span> <span data-ttu-id="ab9ec-134">Ha üdvözlőüzenetére nem változik tooshowing az Irányítópulton egy órán belül, adjon fájl egy támogatási jegy.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-134">If hello message does not change tooshowing your dashboard within an hour, please file a support ticket.</span></span> <span data-ttu-id="ab9ec-135">További részletekért lásd: [hogyan támogatják a tooget az Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="ab9ec-135">For more details, see [How tooget support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|

## <a name="next-steps"></a><span data-ttu-id="ab9ec-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab9ec-136">Next steps</span></span>

<span data-ttu-id="ab9ec-137">Kattintson a Power BI tartalomcsomag Azure Active Directory előzetes tooinstall hello [Itt](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span><span class="sxs-lookup"><span data-stu-id="ab9ec-137">tooinstall hello Power BI Content Pack for Azure Active Directory Preview, click [here](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span></span>


