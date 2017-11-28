---
title: "aaaManaging eszközök hello Azure portál – előzetes verzió |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure portál toomanage eszközök."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: a39d14e4ce8bb79f0223a9de40d5f1259a869927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-devices-using-hello-azure-portal---preview"></a><span data-ttu-id="1194e-103">Eszközök kezelése hello Azure portál – előzetes</span><span class="sxs-lookup"><span data-stu-id="1194e-103">Managing devices using hello Azure portal - preview</span></span>

>[!NOTE]
><span data-ttu-id="1194e-104">Ez a funkció jelenleg nyilvános előzetes verziójához.</span><span class="sxs-lookup"><span data-stu-id="1194e-104">This capability currently is in public preview.</span></span> <span data-ttu-id="1194e-105">Készüljön toorevert, vagy távolítsa el a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="1194e-105">Be prepared toorevert or remove any changes.</span></span> <span data-ttu-id="1194e-106">bármely Azure Active Directory (Azure AD) előfizetés nyilvános előzetes hello szolgáltatás érhető el.</span><span class="sxs-lookup"><span data-stu-id="1194e-106">hello feature is available in any Azure Active Directory (Azure AD) subscription during public preview.</span></span> <span data-ttu-id="1194e-107">Amikor hello szolgáltatás általánosan elérhetővé válik, azonban bizonyos aspektusainak hello szolgáltatást szükség lehet az Azure Active Directory premium előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="1194e-107">However, when hello feature becomes generally available, some aspects of hello feature might require an Azure Active Directory premium subscription.</span></span>


<span data-ttu-id="1194e-108">Az eszköz kezelése az Azure Active Directory (Azure AD) biztosíthatja, hogy a felhasználók a biztonsági és megfelelőségi szabványoknak megfelelő eszközökről érnek el az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1194e-108">With device management in Azure Active Directory (Azure AD), you can ensure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> 

<span data-ttu-id="1194e-109">Ez a témakör:</span><span class="sxs-lookup"><span data-stu-id="1194e-109">This topic:</span></span>

- <span data-ttu-id="1194e-110">Feltételezi, hogy Ön ismeri a hello [bemutatása toodevice kezelése az Azure Active Directoryban](device-management-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="1194e-110">Assumes that you are familiar with hello [introduction toodevice management in Azure Active Directory](device-management-introduction.md)</span></span>

- <span data-ttu-id="1194e-111">Biztosít, akkor használja az eszközök felügyeletével kapcsolatos információkkal hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1194e-111">Provides you with information about managing your devices using hello Azure portal</span></span>


<span data-ttu-id="1194e-112">toomanage eszközök hello Azure-portálon, kell tooclick **eszközök** a hello **kezelése** hello hello szakasza **Azure Active Directory** panelen.</span><span class="sxs-lookup"><span data-stu-id="1194e-112">toomanage devices in hello Azure portal, you need tooclick **Devices** in hello **Manage** section of hello hello **Azure Active Directory** blade.</span></span>

![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a><span data-ttu-id="1194e-114">Eszközök beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1194e-114">Configure device settings</span></span>

<span data-ttu-id="1194e-115">toomanage toobe szükségük az eszközök hello Azure-portál használatával, vagy regisztrálva, vagy csatlakoztatott tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1194e-115">toomanage your devices using hello Azure portal, they need toobe either registered or joined tooAzure AD.</span></span> <span data-ttu-id="1194e-116">A rendszergazdák úgy finomhangolhatja, regisztrálása, illetve az eszközök csatlakoztatása hello eszközbeállítások konfigurálásával hello folyamatán.</span><span class="sxs-lookup"><span data-stu-id="1194e-116">As an administrator, you can fine-tune hello process of registering and joining devices by configuring hello device settings.</span></span>

![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/22.png)


<span data-ttu-id="1194e-118">hello eszköz beállítások panel tooconfigure lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="1194e-118">hello device settings blade enables you tooconfigure:</span></span>

- <span data-ttu-id="1194e-119">**Felhasználók csatlakozhat egy eszközök tooAzure AD** – a beállítások lehetővé teszi tooselect hello felhasználók, akik csatlakozhatnak eszközök tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1194e-119">**Users may join devices tooAzure AD** - This settings enables you tooselect hello users who can join devices tooAzure AD.</span></span> <span data-ttu-id="1194e-120">hello alapértelmezett érték a **összes**.</span><span class="sxs-lookup"><span data-stu-id="1194e-120">hello default is **All**.</span></span>

- <span data-ttu-id="1194e-121">**További Azure AD a helyi rendszergazdák csatlakoztatott eszközök** -hello adott eszközön helyi rendszergazdai jogosultságokkal rendelkező felhasználók is kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="1194e-121">**Additional local administrators on Azure AD joined devices** - You can select hello users that are granted local administrator rights on a device.</span></span> <span data-ttu-id="1194e-122">Hozzá felhasználót adnak hozzá toohello *Eszközadminisztrátorok* szerepkört az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="1194e-122">Users added here are added toohello *Device Administrators* role in Azure AD.</span></span> <span data-ttu-id="1194e-123">Az Azure AD globális rendszergazdák és eszköztulajdonosok alapértelmezés szerint kapnak helyi rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="1194e-123">Global administrators in Azure AD and device owners are granted local administrator rights by default.</span></span> <span data-ttu-id="1194e-124">Ez a beállítás akkor premium edition-funkció termékek, például az Azure AD Premium vagy hello nagyvállalati mobilitási csomag (EMS) keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="1194e-124">This option is a premium edition capability available through products such as Azure AD Premium or hello Enterprise Mobility Suite (EMS).</span></span> 

- <span data-ttu-id="1194e-125">**Felhasználók előfordulhat, hogy regisztrálják az eszközeiket az Azure AD** – Ez a beállítás tooallow eszközök toobe regisztrálva az Azure ad-val tooconfigure van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1194e-125">**Users may register their devices with Azure AD** - You need tooconfigure this setting tooallow devices toobe registered with Azure AD.</span></span> <span data-ttu-id="1194e-126">Ha **nincs**, eszközök tooregister, amelyek nincsenek tartományhoz az Azure AD vagy az Azure AD-tartományhoz hibrid nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="1194e-126">If you select **None**, devices are not allowed tooregister when they are not Azure AD joined or hybrid Azure AD joined.</span></span> <span data-ttu-id="1194e-127">Az Office 365 mobileszköz-felügyeleti (MDM) vagy a Microsoft Intune beléptetési regisztráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="1194e-127">Enrollment with Microsoft Intune or Mobile Device Management (MDM) for Office 365 requires registration.</span></span> <span data-ttu-id="1194e-128">Ha konfigurálta a szolgáltatások valamelyikét **minden** van kiválasztva és **NONE** nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="1194e-128">If you have configured either of these services, **ALL** is selected and **NONE** is not available..</span></span>

- <span data-ttu-id="1194e-129">**A többtényezős hitelesítés toojoin eszközök megkövetelése** -dönthet úgy, hogy a felhasználók a szükséges tooprovide egy második hitelesítési tényező toojoin az eszköz tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1194e-129">**Require Multi-Factor Auth toojoin devices** - You can choose whether users are required tooprovide a second authentication factor toojoin their device tooAzure AD.</span></span> <span data-ttu-id="1194e-130">hello alapértelmezett érték a **nem**.</span><span class="sxs-lookup"><span data-stu-id="1194e-130">hello default is **No**.</span></span> <span data-ttu-id="1194e-131">Azt javasoljuk, hogy többtényezős hitelesítés megkövetelése eszközök regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="1194e-131">We recommend requiring multi-factor authentication when registering a device.</span></span> <span data-ttu-id="1194e-132">Mielőtt engedélyezné ezt a szolgáltatást a többtényezős hitelesítést, győződjön meg róla, hogy a többtényezős hitelesítés beállítása hello felhasználókat, hogy regisztrálják az eszközeiket.</span><span class="sxs-lookup"><span data-stu-id="1194e-132">Before you enable multi-factor authentication for this service, you must ensure that multi-factor authentication is configured for hello users that register their devices.</span></span> <span data-ttu-id="1194e-133">A különböző Azure multi-factor authentication szolgáltatások további információkért lásd: [Ismerkedés az Azure multi-factor Authentication hitelesítés](../multi-factor-authentication/multi-factor-authentication-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1194e-133">For more information on different Azure multi-factor authentication services, see [getting started with Azure multi-factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md).</span></span> 

- <span data-ttu-id="1194e-134">**Eszközök maximális számát** – Ez a beállítás lehetővé teszi tooselect hello eszközök maximális számát, amelyeken a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1194e-134">**Maximum number of devices** - This setting enables you tooselect hello maximum number of devices that a user can have in Azure AD.</span></span> <span data-ttu-id="1194e-135">Ha a felhasználó eléri ezt a kvótát, azok vannak nem tud tooadd további eszközöket amíg egy vagy több olyan hello meglévő eszközt a rendszer eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="1194e-135">If a user reaches this quota, they are not be able tooadd additional devices until one or more of hello existing devices are removed.</span></span> <span data-ttu-id="1194e-136">hello eszköz ajánlat csatlakozott az Azure AD vagy az Azure AD ma regisztrált összes eszköz akkor számít.</span><span class="sxs-lookup"><span data-stu-id="1194e-136">hello device quote is counted for all devices that are either Azure AD joined or Azure AD registered today.</span></span> <span data-ttu-id="1194e-137">hello alapértelmezett értéke **20**.</span><span class="sxs-lookup"><span data-stu-id="1194e-137">hello default value is **20**.</span></span>

- <span data-ttu-id="1194e-138">**Felhasználók is szinkronizálásának beállítások és alkalmazások adatainak eszközök** -alapértelmezés szerint ez a beállítás túl**NONE**.</span><span class="sxs-lookup"><span data-stu-id="1194e-138">**Users may sync settings and app data across devices** - By default, this setting is set too**NONE**.</span></span> <span data-ttu-id="1194e-139">Egyes felhasználók vagy csoportok, vagy az összes kijelölésével hello felhasználói beállítások és adatok toosync alkalmazás a Windows 10-eszközön.</span><span class="sxs-lookup"><span data-stu-id="1194e-139">Selecting specific users or groups or ALL allows hello user’s settings and app data toosync across their Windows 10 devices.</span></span> <span data-ttu-id="1194e-140">További tudnivalókért a Windows 10-es sync működéséről.</span><span class="sxs-lookup"><span data-stu-id="1194e-140">Learn more on how sync works in Windows 10.</span></span>
<span data-ttu-id="1194e-141">Ez a beállítás akkor a termékek, például az Azure AD Premium vagy hello nagyvállalati mobilitási csomag (EMS) keresztül érhető el prémium funkció.</span><span class="sxs-lookup"><span data-stu-id="1194e-141">This option is a premium capability available through products such as Azure AD Premium or hello Enterprise Mobility Suite (EMS).</span></span>
 
    ![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a><span data-ttu-id="1194e-143">Keresse meg az eszközök</span><span class="sxs-lookup"><span data-stu-id="1194e-143">Locate devices</span></span>

<span data-ttu-id="1194e-144">A rendszergazdák az Azure-portálon hello két lehetősége van toolocate regisztrálva, és a csatlakoztatott eszközök:</span><span class="sxs-lookup"><span data-stu-id="1194e-144">As an administrator, in hello Azure portal, you have two options toolocate registered and joined devices:</span></span>

- <span data-ttu-id="1194e-145">**Minden eszköz** a hello **kezelése** hello szakasza **eszközök** panel</span><span class="sxs-lookup"><span data-stu-id="1194e-145">**All devices** in hello **Manage** section of hello **Devices** blade</span></span>  

    ![Minden eszköz](./media/device-management-azure-portal/41.png)


- <span data-ttu-id="1194e-147">**Eszközök** a hello **kezelése** szakasza egy **felhasználói** panel</span><span class="sxs-lookup"><span data-stu-id="1194e-147">**Devices** in hello **Manage** section of a **User** blade</span></span>
 
    ![Minden eszköz](./media/device-management-azure-portal/43.png)



<span data-ttu-id="1194e-149">Mindkét lehetőség érhet tooa megtekintése, amelyek:</span><span class="sxs-lookup"><span data-stu-id="1194e-149">With both options, you can get tooa view that:</span></span>


- <span data-ttu-id="1194e-150">Lehetővé teszi a toosearch eszközök szűrő hello megjelenített név használatával.</span><span class="sxs-lookup"><span data-stu-id="1194e-150">Enables you toosearch for devices using hello display name as filter.</span></span>

- <span data-ttu-id="1194e-151">Lehetővé teszi a regisztrált és a csatlakoztatott eszközök részletes áttekintése</span><span class="sxs-lookup"><span data-stu-id="1194e-151">Provides you with detailed overview of registered and joined devices</span></span>

- <span data-ttu-id="1194e-152">Lehetővé teszi a tooperform gyakori feladatok</span><span class="sxs-lookup"><span data-stu-id="1194e-152">Enables you tooperform common device management tasks</span></span>
   

![Minden eszköz](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a><span data-ttu-id="1194e-154">Eszközfelügyeleti feladatokat</span><span class="sxs-lookup"><span data-stu-id="1194e-154">Device management tasks</span></span>

<span data-ttu-id="1194e-155">A rendszergazdák kezelése hello regisztrálva, vagy csatlakoztatott eszközök.</span><span class="sxs-lookup"><span data-stu-id="1194e-155">As an administrator, you can manage hello registered or joined devices.</span></span> <span data-ttu-id="1194e-156">Ez a szakasz a közös eszköz felügyeleti feladatokkal kapcsolatos információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="1194e-156">This section provides you with information about common device management tasks.</span></span>


<span data-ttu-id="1194e-157">**Az Intune eszközök kezelésére** – Ha egy Intune-rendszergazda, jelölésű eszközöket kezelheti **a Microsoft Intune**.</span><span class="sxs-lookup"><span data-stu-id="1194e-157">**Manage an Intune device** - If you are an Intune administrator, you can manage devices marked as **Microsoft Intune**.</span></span> <span data-ttu-id="1194e-158">A rendszergazda további eszköz látható</span><span class="sxs-lookup"><span data-stu-id="1194e-158">An administrator can see additional device</span></span> 

![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/31.png)


<span data-ttu-id="1194e-160">**Engedélyezi / letiltja az Azure AD-eszközök**</span><span class="sxs-lookup"><span data-stu-id="1194e-160">**Enable / disable an Azure AD device**</span></span>

<span data-ttu-id="1194e-161">tooenable vagy letilthatja az eszköz, szüksége toobe egy globális rendszergazda Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1194e-161">tooenable or disable a device, you need toobe a global administrator in Azure  AD.</span></span> <span data-ttu-id="1194e-162">Ha letilt egy eszközt megakadályozza, hogy az eszköz az Azure AD-erőforrások hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="1194e-162">Disabling a device prevents a device from accessing your Azure AD resources.</span></span>  <span data-ttu-id="1194e-163">toodisable hello eszköz, vagy kattintson is *...*</span><span class="sxs-lookup"><span data-stu-id="1194e-163">toodisable hello device, you can either click *…*</span></span> <span data-ttu-id="1194e-164">Kattintson a további részletekért hello eszközre.</span><span class="sxs-lookup"><span data-stu-id="1194e-164">click hello device for additional details.</span></span>

 
![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/33.png)

<span data-ttu-id="1194e-166">Ha letilt egy eszközt állapota hello a hello **engedélyezve** oszlop túl**nem**.</span><span class="sxs-lookup"><span data-stu-id="1194e-166">Disabling a device changes hello state in hello **ENABLED** column too**No**.</span></span>

![Az eszköz letiltása](./media/device-management-azure-portal/32.png)


<span data-ttu-id="1194e-168">**Egy Azure AD-eszköz törlése** -toodelete egy eszközt, toobe egy globális rendszergazda Azure AD-ben van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1194e-168">**Delete an Azure AD device** - toodelete a device, you need toobe a global administrator in Azure AD.</span></span>  
<span data-ttu-id="1194e-169">Egy eszköz törlése:</span><span class="sxs-lookup"><span data-stu-id="1194e-169">Deleting a device:</span></span>
 
- <span data-ttu-id="1194e-170">Megakadályozza, hogy az eszköz az Azure AD-erőforrások elérése</span><span class="sxs-lookup"><span data-stu-id="1194e-170">Prevents a device from accessing your Azure AD resources</span></span> 

- <span data-ttu-id="1194e-171">Eltávolítja az összes csatolt toohello eszköz adatok például, a BitLocker-kulcsok Windows-eszközök</span><span class="sxs-lookup"><span data-stu-id="1194e-171">Removes all details that are attached toohello device, for example, BitLocker keys for Windows devices</span></span>  

- <span data-ttu-id="1194e-172">Nem állíthatók helyre tevékenységet jelent, és nem ajánlott, kivéve, ha szükséges</span><span class="sxs-lookup"><span data-stu-id="1194e-172">Represents a non-recoverable activity and is not recommended unless it is required</span></span>

<span data-ttu-id="1194e-173">Ha egy eszköz felügyelete egy másik felügyeleti hatóság (pl. Microsoft Intune), ellenőrizze, hogy hello eszközön tárolt adatok törléséig / már nincs az Azure AD hello eszköz törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="1194e-173">If a device is managed by another management authority (e.g. Microsoft Intune), please make sure that hello device has been wiped / retired before deleting hello device in Azure AD.</span></span>

<span data-ttu-id="1194e-174">Kiválaszthatja "..."</span><span class="sxs-lookup"><span data-stu-id="1194e-174">You can either select “…”</span></span> <span data-ttu-id="1194e-175">toodelete eszköz hello, vagy kattintson a további részletekért hello eszközön</span><span class="sxs-lookup"><span data-stu-id="1194e-175">toodelete hello device or click on hello device for additional details</span></span>
 
![Eszköz törlése](./media/device-management-azure-portal/34.png)


<span data-ttu-id="1194e-177">**Megtekintése vagy másolása Eszközazonosító** -használhatja egy eszköz azonosítója tooverify hello azonosító eszközadatok hello eszközön vagy a PowerShell használatával a hibaelhárítás során.</span><span class="sxs-lookup"><span data-stu-id="1194e-177">**View or copy device ID** - You can use a device ID tooverify hello device ID details on hello device or using PowerShell during troubleshooting.</span></span> <span data-ttu-id="1194e-178">tooaccess hello másolása, majd hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="1194e-178">tooaccess hello copy option, click hello device.</span></span>

![Eszközazonosítót megtekintése](./media/device-management-azure-portal/35.png)
  

<span data-ttu-id="1194e-180">**Megtekintése vagy a BitLocker-kulcsok másolása** -rendszergazdák, megtekintheti, és másolási hello BitLocker-kulcsok toohelp felhasználók toorecover a titkosított meghajtón.</span><span class="sxs-lookup"><span data-stu-id="1194e-180">**View or copy BitLocker keys** - If you are an administrator, you can view and copy hello BitLocker keys toohelp users toorecover their encrypted drive.</span></span> <span data-ttu-id="1194e-181">Ezek a kulcsok csak titkosított Windows-eszközökhöz elérhető, és a kulcsaikat az Azure ad-ben tárolt.</span><span class="sxs-lookup"><span data-stu-id="1194e-181">These keys are only available for Windows devices that are encrypted and have their keys stored in Azure AD.</span></span> <span data-ttu-id="1194e-182">Ezek a kulcsok másolhatja hello eszköz részleteit való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="1194e-182">You can copy these keys when accessing details of hello device.</span></span>
 
![A BitLocker-kulcsok megtekintése](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a><span data-ttu-id="1194e-184">Naplók</span><span class="sxs-lookup"><span data-stu-id="1194e-184">Audit logs</span></span>


<span data-ttu-id="1194e-185">hello eszköz tevékenységek hello tevékenységi naplóit keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="1194e-185">hello device activities are available through hello activity logs.</span></span> <span data-ttu-id="1194e-186">Ez magában foglalja az eszközregisztrációs szolgáltatás hello vagy hello felhasználó által indított tevékenységek:</span><span class="sxs-lookup"><span data-stu-id="1194e-186">This includes activities triggered by hello device registration service or by hello user:</span></span>

- <span data-ttu-id="1194e-187">Eszköz létrehozása és hozzáadása tulajdonosai és felhasználói számára hello eszközön</span><span class="sxs-lookup"><span data-stu-id="1194e-187">Device creation and adding owners/users on hello device</span></span>

- <span data-ttu-id="1194e-188">Toodevice beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="1194e-188">Changes toodevice settings</span></span>

- <span data-ttu-id="1194e-189">Eszköz műveletek, például a törlés vagy egy eszköz frissítése</span><span class="sxs-lookup"><span data-stu-id="1194e-189">Device operations such as deleting or updating a device</span></span>
 
<span data-ttu-id="1194e-190">A belépési pont toohello adatok naplózása van **naplók** a hello **tevékenység** hello szakasza **eszközök* panelen.</span><span class="sxs-lookup"><span data-stu-id="1194e-190">Your entry point toohello auditing data is **Audit logs** in hello **Activity** section of hello **Devices* blade.</span></span>

![Naplók](./media/device-management-azure-portal/61.png)


<span data-ttu-id="1194e-192">Az auditnapló alapértelmezett listanézete az alábbi adatokat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="1194e-192">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="1194e-193">hello dátum és idő hello előfordulás</span><span class="sxs-lookup"><span data-stu-id="1194e-193">hello date and time of hello occurrence</span></span>

- <span data-ttu-id="1194e-194">hello célok</span><span class="sxs-lookup"><span data-stu-id="1194e-194">hello targets</span></span>

- <span data-ttu-id="1194e-195">hello kezdeményező / szereplő (ki) tevékenység</span><span class="sxs-lookup"><span data-stu-id="1194e-195">hello initiator / actor (who) of an activity</span></span>

- <span data-ttu-id="1194e-196">hello tevékenység (Mit)</span><span class="sxs-lookup"><span data-stu-id="1194e-196">hello activity (what)</span></span>

![Naplók](./media/device-management-azure-portal/63.png)

<span data-ttu-id="1194e-198">Testre szabhatja hello listanézet kattintva **oszlopok** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="1194e-198">You can customize hello list view by clicking **Columns** in hello toolbar.</span></span>
 
![Naplók](./media/device-management-azure-portal/64.png)


<span data-ttu-id="1194e-200">lefelé hello toonarrow jelentette, hogy az Ön működik, végezhet hello naplózási adatok használatával a következő mezők hello tooa szintnek:</span><span class="sxs-lookup"><span data-stu-id="1194e-200">toonarrow down hello reported data tooa level that works for you, you can filter hello audit data using hello following fields:</span></span>

- <span data-ttu-id="1194e-201">Catergory</span><span class="sxs-lookup"><span data-stu-id="1194e-201">Catergory</span></span>
- <span data-ttu-id="1194e-202">Tevékenység erőforrástípusa</span><span class="sxs-lookup"><span data-stu-id="1194e-202">Activity resource type</span></span>
- <span data-ttu-id="1194e-203">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="1194e-203">Activity</span></span>
- <span data-ttu-id="1194e-204">Dátumtartomány</span><span class="sxs-lookup"><span data-stu-id="1194e-204">Date range</span></span>
- <span data-ttu-id="1194e-205">cél</span><span class="sxs-lookup"><span data-stu-id="1194e-205">Target</span></span>
- <span data-ttu-id="1194e-206">(Aktor) által kezdeményezett</span><span class="sxs-lookup"><span data-stu-id="1194e-206">Initiated By (Actor)</span></span>

<span data-ttu-id="1194e-207">Ezenkívül toohello szűrők kereshet adott bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="1194e-207">In addition toohello filters, you can search for specific entries.</span></span>

![Naplók](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a><span data-ttu-id="1194e-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1194e-209">Next steps</span></span>

* [<span data-ttu-id="1194e-210">Bevezetés toodevice kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="1194e-210">Introduction toodevice management in Azure Active Directory</span></span>](device-management-introduction.md)



