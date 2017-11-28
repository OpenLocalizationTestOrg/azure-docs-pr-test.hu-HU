---
title: "az Azure Active Directoryban aaaIntroduction toodevice felügyeleti |} Microsoft Docs"
description: "Ismerje meg, hogyan Eszközkezelés segíthetnek erőforrásoknak a környezetben elérő eszközök hello tooget vezérelheti."
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
ms.openlocfilehash: e2fc0a3e8d00dc69cf01db9074e34427e396cfcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toodevice-management-in-azure-active-directory"></a><span data-ttu-id="cd579-103">Bevezetés toodevice kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="cd579-103">Introduction toodevice management in Azure Active Directory</span></span>

<span data-ttu-id="cd579-104">Mobileszköz-first, a felhő-első világában Azure Active Directory (Azure AD) lehetővé teszi, hogy egyszeri bejelentkezés toodevices, alkalmazásokhoz és szolgáltatásokhoz bárhonnan.</span><span class="sxs-lookup"><span data-stu-id="cd579-104">In a mobile-first, cloud-first world, Azure Active Directory (Azure AD) enables single sign-on toodevices, apps, and services from anywhere.</span></span> <span data-ttu-id="cd579-105">Hello elterjedése eszközök – beleértve a saját eszközök használata (Bring BYOD), az informatikai szakemberek számára két másik célt szemben:</span><span class="sxs-lookup"><span data-stu-id="cd579-105">With hello proliferation of devices - including Bring Your Own Device (BYOD), IT professionals are faced with two opposing goals:</span></span>

- <span data-ttu-id="cd579-106">Hello végfelhasználók toobe hatékony építve, amikor és ahol csak</span><span class="sxs-lookup"><span data-stu-id="cd579-106">Empower hello end users toobe productive wherever and whenever</span></span>
- <span data-ttu-id="cd579-107">Bármikor hello vállalati eszközök védelme</span><span class="sxs-lookup"><span data-stu-id="cd579-107">Protect hello corporate assets at any time</span></span>

<span data-ttu-id="cd579-108">Eszközön a felhasználók vannak fog hozzáférni tooyour vállalati eszközöket.</span><span class="sxs-lookup"><span data-stu-id="cd579-108">Through devices, your users are getting access tooyour corporate assets.</span></span> <span data-ttu-id="cd579-109">tooprotect a vállalati eszközök informatikai rendszergazdaként szeretné toohave felügyelheti.</span><span class="sxs-lookup"><span data-stu-id="cd579-109">tooprotect your corporate assets, as an IT administrator, you want toohave control over these devices.</span></span> <span data-ttu-id="cd579-110">Ez lehetővé teszi toomake meg arról, hogy a felhasználók a biztonsági és megfelelőségi szabványoknak megfelelő eszközökről érnek el az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cd579-110">This enables you toomake sure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> 

<span data-ttu-id="cd579-111">Eszközkezelés egyben a hello alapját [eszközalapú feltételes hozzáférési](active-directory-conditional-access-policy-connected-applications.md).</span><span class="sxs-lookup"><span data-stu-id="cd579-111">Device management is also hello foundation for [device-based conditional access](active-directory-conditional-access-policy-connected-applications.md).</span></span> <span data-ttu-id="cd579-112">Az eszközalapú feltételes hozzáférés biztosítható, hogy hozzáférési tooresources a környezetben csak az lehetséges megbízható eszköz.</span><span class="sxs-lookup"><span data-stu-id="cd579-112">With device-based conditional access, you can ensure that access tooresources in your environment is only possible with trusted devices.</span></span>   

<span data-ttu-id="cd579-113">Ez a témakör ismerteti, hogyan működik a kezelés az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="cd579-113">This topic explains how device management in Azure Active Directory works.</span></span>

## <a name="getting-devices-under-hello-control-of-azure-ad"></a><span data-ttu-id="cd579-114">Az Azure AD hello vezérlése alatt eszközök</span><span class="sxs-lookup"><span data-stu-id="cd579-114">Getting devices under hello control of Azure AD</span></span>

<span data-ttu-id="cd579-115">tooget egy eszközt az Azure AD hello ellenőrzése alatt, két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="cd579-115">tooget a device under hello control of Azure AD, you have two options:</span></span>

- <span data-ttu-id="cd579-116">Regisztrálása</span><span class="sxs-lookup"><span data-stu-id="cd579-116">Registering</span></span> 
- <span data-ttu-id="cd579-117">Csatlakozás</span><span class="sxs-lookup"><span data-stu-id="cd579-117">Joining</span></span>

<span data-ttu-id="cd579-118">**Regisztrálás** egy eszköz tooAzure AD lehetővé teszi, hogy Ön toomanage egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="cd579-118">**Registering** a device tooAzure AD enables you toomanage a device’s identity.</span></span> <span data-ttu-id="cd579-119">Amikor regisztrál egy eszközt, az Azure AD eszközregisztrációval biztosít hello eszköz, amely használt tooauthenticate hello eszközre, ha egy felhasználó bejelentkezik tooAzure AD identitással.</span><span class="sxs-lookup"><span data-stu-id="cd579-119">When a device is registered, Azure AD device registration provides hello device with an identity that is used tooauthenticate hello device when a user signs-in tooAzure AD.</span></span> <span data-ttu-id="cd579-120">Hello identitás tooenable használja, vagy az eszköz letiltása.</span><span class="sxs-lookup"><span data-stu-id="cd579-120">You can use hello identity tooenable or disable a device.</span></span>

<span data-ttu-id="cd579-121">Például a Microsoft Intune mobileszköz-lévő eszközattribútumok megoldás együtt, hello eszközattribútumokon az Azure AD frissítődik hello eszközzel kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="cd579-121">When combined with a mobile device management(MDM) solution such as Microsoft Intune, hello device attributes in Azure AD are updated with additional information about hello device.</span></span> <span data-ttu-id="cd579-122">Ez lehetővé teszi toocreate feltételes hozzáférési szabályok, amelyeket eszközök toomeet való hozzáférést a biztonsági és megfelelőségi szabványoknak.</span><span class="sxs-lookup"><span data-stu-id="cd579-122">This allows you toocreate conditional access rules that enforce access from devices toomeet your standards for security and compliance.</span></span> <span data-ttu-id="cd579-123">A Microsoft Intune-beli regisztrálásának eszközökön további információkért lásd: eszközök regisztrálása felügyeletre a Intune-ban.</span><span class="sxs-lookup"><span data-stu-id="cd579-123">For more information on enrolling devices in Microsoft Intune, see Enroll devices for management in Intune .</span></span>

<span data-ttu-id="cd579-124">**Csatlakozás** egy eszköz egy bővítmény tooregistering eszköz.</span><span class="sxs-lookup"><span data-stu-id="cd579-124">**Joining** a device is an extension tooregistering a device.</span></span> <span data-ttu-id="cd579-125">Ez azt jelenti, az összes hello előnyt biztosít az eszköz regisztrációja és hozzáadását toothis, hello helyi állapota az eszköz is változik.</span><span class="sxs-lookup"><span data-stu-id="cd579-125">This means, it provides you with all hello benefits of registering a device and in addition toothis, it also changes hello local state of a device.</span></span> <span data-ttu-id="cd579-126">Hello helyi állapotváltás lehetővé teszi, hogy a felhasználók tooa toosign az eszközt egy szervezeti munkahelyi vagy iskolai fiókot egy személyes fiók helyett.</span><span class="sxs-lookup"><span data-stu-id="cd579-126">Changing hello local state enables your users toosign-in tooa device using an organizational work or school account instead of a personal account.</span></span>

## <a name="azure-ad-registered-devices"></a><span data-ttu-id="cd579-127">Az Azure AD regisztrált eszközök</span><span class="sxs-lookup"><span data-stu-id="cd579-127">Azure AD registered devices</span></span>   

<span data-ttu-id="cd579-128">hello Azure AD regisztrált eszközök célja való támogatása hello tooprovide **kapcsolja a saját eszközök használata (BYOD)** forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="cd579-128">hello goal of Azure AD registered devices is tooprovide you with support for hello **Bring Your Own Device (BYOD)** scenario.</span></span> <span data-ttu-id="cd579-129">Ebben a forgatókönyvben egy felhasználó hozzáférhessen a szervezet Azure Active Directory szabályozott erőforrások egy személyes eszközt használ.</span><span class="sxs-lookup"><span data-stu-id="cd579-129">In this scenario, a user can access your organization’s Azure Active Directory controlled resources using a personal device.</span></span>  

![Az Azure AD regisztrált eszközök](./media/device-management-introduction/03.png)

<span data-ttu-id="cd579-131">hello hozzáférést a munkahelyi vagy iskolai fiókkal, hello eszközön beírt alapul.</span><span class="sxs-lookup"><span data-stu-id="cd579-131">hello access is based on a work or school account that has been entered on hello device.</span></span>  
<span data-ttu-id="cd579-132">Például a Windows 10 lehetővé teszi a felhasználók tooadd egy munkahelyi vagy iskolai fiók tooa személyi számítógép, táblagép vagy telefon.</span><span class="sxs-lookup"><span data-stu-id="cd579-132">For example, Windows 10 enables users tooadd a work or school account tooa personal computer, tablet, or phone.</span></span>  
<span data-ttu-id="cd579-133">Amikor a felhasználó hozzáadta a munkahelyi vagy iskolai fiókkal, hello eszköz regisztrálva az Azure AD és opcionálisan regisztrált rendszerben hello mobileszköz-felügyelet (MDM), amely a szervezet van-e beállítva.</span><span class="sxs-lookup"><span data-stu-id="cd579-133">When a user has added a work or school account, hello device is registered with Azure AD and optionally enrolled in hello mobile device management (MDM) system that your organization has configured.</span></span> <span data-ttu-id="cd579-134">A szervezet felhasználók adja hozzá a munkahelyi vagy iskolai kényelmesen fiók tooa személyes eszköz:</span><span class="sxs-lookup"><span data-stu-id="cd579-134">Your organization’s users can add a work or school account tooa personal device conveniently:</span></span>

- <span data-ttu-id="cd579-135">A munkahelyi alkalmazás hello első alkalommal való hozzáféréskor</span><span class="sxs-lookup"><span data-stu-id="cd579-135">When accessing a work application for hello first time</span></span>
- <span data-ttu-id="cd579-136">Manuálisan keresztül hello **beállítások** hello esetben a Windows 10 menü</span><span class="sxs-lookup"><span data-stu-id="cd579-136">Manually via hello **Settings** menu in hello case of Windows 10</span></span> 

<span data-ttu-id="cd579-137">Az Azure AD regisztrált eszközöket a Windows 10, iOS, Android és macOS konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="cd579-137">You can configure Azure AD registered devices for Windows 10, iOS, Android and macOS.</span></span>

## <a name="azure-ad-joined-devices"></a><span data-ttu-id="cd579-138">Az Azure AD csatlakoztatott eszközök</span><span class="sxs-lookup"><span data-stu-id="cd579-138">Azure AD joined devices</span></span>

<span data-ttu-id="cd579-139">hello az Azure AD csatlakoztatott eszközök célja toosimplify:</span><span class="sxs-lookup"><span data-stu-id="cd579-139">hello goal of Azure AD joined devices is toosimplify:</span></span>

- <span data-ttu-id="cd579-140">A munkahelyi eszközök a Windows központi telepítése</span><span class="sxs-lookup"><span data-stu-id="cd579-140">Windows deployments of work-owned devices</span></span> 
- <span data-ttu-id="cd579-141">Hozzáférés tooorganizational alkalmazásokat és erőforrásokat bármilyen Windows eszközről</span><span class="sxs-lookup"><span data-stu-id="cd579-141">Access tooorganizational apps and resources from any Windows device</span></span>

![Az Azure AD regisztrált eszközök](./media/device-management-introduction/02.png)


<span data-ttu-id="cd579-143">A felhasználók egy önkiszolgáló felhasználói élmény biztosítása számára az Azure AD hello vezérlése alatt munkahelyi által birtokolt eszközök ezen célok lehet elvégezni.</span><span class="sxs-lookup"><span data-stu-id="cd579-143">These goals are accomplished by providing your users with a self-service experience for getting work-owned devices under hello control of Azure AD.</span></span>  
<span data-ttu-id="cd579-144">**Az Azure AD Join** , amelyek a felhő-első / csak felhőalapú szervezetek számára készült.</span><span class="sxs-lookup"><span data-stu-id="cd579-144">**Azure AD Join** is intended for organizations that are cloud-first / cloud-only.</span></span> <span data-ttu-id="cd579-145">Ezek rendszerint azok a helyszíni Windows Server Active Directory infrastruktúrával nem rendelkező kis - és közepes méretű vállalkozások.</span><span class="sxs-lookup"><span data-stu-id="cd579-145">These are typically small- and medium-sized businesses that do not have an on-premises Windows Server Active Directory infrastructure.</span></span> 

<span data-ttu-id="cd579-146">Az Azure AD csatlakoztatott eszközök végrehajtási tesz lehetővé a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="cd579-146">Implementing Azure AD joined devices provides you with hello following benefits:</span></span>

- <span data-ttu-id="cd579-147">**Egyszeri bejelentkezéses (SSO)** tooyour Azure felügyelt SaaS-alkalmazásokhoz és szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="cd579-147">**Single-Sign-On (SSO)** tooyour Azure managed SaaS apps and services.</span></span> <span data-ttu-id="cd579-148">Nem jelennek meg a felhasználók további hitelesítési kérések munkahelyi erőforrásokhoz való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="cd579-148">Your users don’t see additional authentication prompts when accessing work resources.</span></span> <span data-ttu-id="cd579-149">hello SSO funkció még akkor is, ha azok nem csatlakoztatott toohello tartomány elérhető hálózat.</span><span class="sxs-lookup"><span data-stu-id="cd579-149">hello SSO functionality is even when they are not connected toohello domain network available.</span></span>

- <span data-ttu-id="cd579-150">**Vállalati kompatibilis központi** a felhasználói beállítások csatlakoztatott eszközön.</span><span class="sxs-lookup"><span data-stu-id="cd579-150">**Enterprise compliant roaming** of user settings across joined devices.</span></span> <span data-ttu-id="cd579-151">A felhasználóknak nem kell tooconnect egy Microsoft-fiók (például Hotmail) toosee beállításait az eszközön.</span><span class="sxs-lookup"><span data-stu-id="cd579-151">Users don’t need tooconnect a Microsoft account (for example, Hotmail) toosee settings across devices.</span></span>

- <span data-ttu-id="cd579-152">**A vállalati hozzáférés tooWindows áruház** AD fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="cd579-152">**Access tooWindows Store for Business** using AD account.</span></span> <span data-ttu-id="cd579-153">A felhasználók előre kiválasztott hello szervezeti alkalmazások leltárt közül választhatnak.</span><span class="sxs-lookup"><span data-stu-id="cd579-153">Your users can choose from an inventory of applications pre-selected by hello organization.</span></span>

- <span data-ttu-id="cd579-154">**A Windows Hello** biztonságosabb és kényelmesebb hozzáférési toowork erőforrások támogatása.</span><span class="sxs-lookup"><span data-stu-id="cd579-154">**Windows Hello** support for secure and convenient access toowork resources.</span></span>

- <span data-ttu-id="cd579-155">**Hozzáférés korlátozása** tooapps csak olyan eszközökön, amelyek megfelelnek a megfelelőségi szabályzatnak.</span><span class="sxs-lookup"><span data-stu-id="cd579-155">**Restriction of access** tooapps from only devices that meet compliance policy.</span></span>

<span data-ttu-id="cd579-156">Az Azure AD join elsődlegesen a helyi Windows Server Active Directory infrastruktúrával nem rendelkező szervezeteknek, amíg is alapértékekkel is helyzetekben használhatja, ahol:</span><span class="sxs-lookup"><span data-stu-id="cd579-156">While Azure AD join is primarily intended for organizations that do not have an on-premises Windows Server Active Directory infrastructure, you can certainly also use it in scenarios where:</span></span>

- <span data-ttu-id="cd579-157">Nem használhat egy helyszíni tartományhoz való csatlakozást, például ha tooget mobil eszközök, például táblagépekről és mobiltelefonokról vezérlése alatt van szüksége.</span><span class="sxs-lookup"><span data-stu-id="cd579-157">You can’t use an on-premises domain join, for example, if you need tooget mobile devices such as tablets and phones under control.</span></span>

- <span data-ttu-id="cd579-158">A felhasználók elsősorban tooaccess Office 365 vagy más SaaS-alkalmazásokhoz az Azure ad-vel integrált van szükség.</span><span class="sxs-lookup"><span data-stu-id="cd579-158">Your users primarily need tooaccess Office 365 or other SaaS apps integrated with Azure AD.</span></span>

- <span data-ttu-id="cd579-159">Az Azure AD ahelyett, hogy az Active Directory kívánt toomanage a felhasználók egy csoportjánál.</span><span class="sxs-lookup"><span data-stu-id="cd579-159">You want toomanage a group of users in Azure AD instead of in Active Directory.</span></span> <span data-ttu-id="cd579-160">Ez is vonatkozik, például tooseasonal munkavállalók, alvállalkozói és a diákok.</span><span class="sxs-lookup"><span data-stu-id="cd579-160">This can apply, for example, tooseasonal workers, contractors, or students.</span></span>

- <span data-ttu-id="cd579-161">Azt szeretné, hogy tooprovide csatlakozó képességek tooworkers távoli fiókirodák korlátozott a helyszíni infrastruktúrával.</span><span class="sxs-lookup"><span data-stu-id="cd579-161">You want tooprovide joining capabilities tooworkers in remote branch offices with limited on-premises infrastructure.</span></span>

<span data-ttu-id="cd579-162">Beállíthatja, hogy csatlakozott az Azure AD-eszközök Windows 10 rendszerű eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="cd579-162">You can configure Azure AD joined devices for Windows 10 devices.</span></span>


## <a name="hybrid-azure-ad-joined-devices"></a><span data-ttu-id="cd579-163">Az Azure AD hibrid csatlakoztatott eszközök</span><span class="sxs-lookup"><span data-stu-id="cd579-163">Hybrid Azure AD joined devices</span></span>

<span data-ttu-id="cd579-164">A több mint egy évtizedben számos szervezet hello tartományhoz csatlakoztatás tootheir a helyszíni Active Directory tooenable használja:</span><span class="sxs-lookup"><span data-stu-id="cd579-164">For more than a decade, many organizations have used hello domain join tootheir on-premises Active Directory tooenable:</span></span>

- <span data-ttu-id="cd579-165">INFORMATIKAI részlegek toomanage munkahelyi tulajdonban lévő eszközök egy központi helyről.</span><span class="sxs-lookup"><span data-stu-id="cd579-165">IT departments toomanage work-owned devices from a central location.</span></span>

- <span data-ttu-id="cd579-166">Az Active Directory tootheir eszközöket a felhasználók toosign munkahelyi vagy iskolai fiókok.</span><span class="sxs-lookup"><span data-stu-id="cd579-166">Users toosign in tootheir devices with their Active Directory work or school accounts.</span></span> 

<span data-ttu-id="cd579-167">Általában egy helyszíni tárhely szervezetek támaszkodnak imaging módszerek tooprovision eszközöket, és gyakran használják **System Center Configuration Manager (SCCM)** vagy **csoportházirend (GP)** toomanage őket.</span><span class="sxs-lookup"><span data-stu-id="cd579-167">Typically, organizations with an on-premises footprint rely on imaging methods tooprovision devices, and they often use **System Center Configuration Manager (SCCM)** or **group policy (GP)** toomanage them.</span></span>

<span data-ttu-id="cd579-168">Ha a környezetben egy helyszíni AD kezdjen, és azt is szeretné előny az Azure Active Directory által biztosított hello képességek, hibrid csatlakozott az Azure AD-eszközöket is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="cd579-168">If your environment has an on-premises AD footprint and you also want benefit from hello capabilities provided by Azure Active Directory, you can implement hybrid Azure AD joined devices.</span></span> <span data-ttu-id="cd579-169">Ezek a nyomtatók is, a csatlakoztatott tooyour a helyszíni Active Directory és az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="cd579-169">These are devices that are both, joined tooyour on-premises Active Directory and your Azure Active Directory.</span></span>

![Az Azure AD regisztrált eszközök](./media/device-management-introduction/01.png)


<span data-ttu-id="cd579-171">Az Azure AD hibrid csatlakoztatott eszközöket kell használnia, ha:</span><span class="sxs-lookup"><span data-stu-id="cd579-171">You should use Azure AD hybrid joined devices if:</span></span>

- <span data-ttu-id="cd579-172">Win32 alkalmazásokat telepített toothese eszközt használó NTLM / Kerberos.</span><span class="sxs-lookup"><span data-stu-id="cd579-172">You have Win32 apps deployed toothese devices that use NTLM / Kerberos.</span></span>

- <span data-ttu-id="cd579-173">Szüksége van a csoportházirend vagy SCCM / DCM toomanage eszközök.</span><span class="sxs-lookup"><span data-stu-id="cd579-173">You require GP or SCCM / DCM toomanage devices.</span></span>

- <span data-ttu-id="cd579-174">Az alkalmazottak toocontinue toouse lemezkép-készítési megoldások tooconfigure eszközök használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="cd579-174">You want toocontinue toouse imaging solutions tooconfigure devices for your employees.</span></span>

<span data-ttu-id="cd579-175">Konfigurálhatja a Hybrid Azure AD csatlakoztatott eszközök a Windows 10-es és régebbi eszközök, például a Windows 8 és Windows 7.</span><span class="sxs-lookup"><span data-stu-id="cd579-175">You can configure Hybrid Azure AD joined devices for Windows 10 and down-level devices such as Windows 8 and Windows 7.</span></span>

## <a name="summary"></a><span data-ttu-id="cd579-176">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="cd579-176">Summary</span></span>

<span data-ttu-id="cd579-177">Az eszköz kezelése az Azure ad-ben a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="cd579-177">With device management in Azure AD, you can:</span></span> 

- <span data-ttu-id="cd579-178">Az Azure AD hello vezérlése alatt eszközükkel hello folyamat leegyszerűsítése érdekében</span><span class="sxs-lookup"><span data-stu-id="cd579-178">Simplify hello process of bringing devices under hello control of Azure AD</span></span>

- <span data-ttu-id="cd579-179">Felhőalapú erőforrások tooyour szervezetek könnyen toouse hozzáférést biztosítania a felhasználóknak</span><span class="sxs-lookup"><span data-stu-id="cd579-179">Provide your users with an easy toouse access tooyour organization’s cloud-based resources</span></span>

<span data-ttu-id="cd579-180">A görgetőgomb szabályként kell használnia:</span><span class="sxs-lookup"><span data-stu-id="cd579-180">As a rule of a thumb, you should use:</span></span>

- <span data-ttu-id="cd579-181">Az Azure AD regisztrált eszközök személyes eszközök</span><span class="sxs-lookup"><span data-stu-id="cd579-181">Azure AD registered devices for personal devices</span></span>

- <span data-ttu-id="cd579-182">Az Azure AD csatlakoztatott eszközök, az eszközök, amelyek nem csatlakoztatott tooan a helyszíni AD</span><span class="sxs-lookup"><span data-stu-id="cd579-182">Azure AD joined devices for devices that are not joined tooan on-premises AD</span></span> 

- <span data-ttu-id="cd579-183">Az Azure AD hibrid csatlakoztatott eszközök, az eszközök, amelyek csatlakoztatott tooan a helyszíni AD</span><span class="sxs-lookup"><span data-stu-id="cd579-183">Hybrid Azure AD joined devices for devices that are joined tooan on-premises AD</span></span>     




## <a name="next-steps"></a><span data-ttu-id="cd579-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd579-184">Next steps</span></span>

- <span data-ttu-id="cd579-185">Hogyan toomanage eszköz hello Azure portál, lásd: áttekintést tooget [hello Azure-portál használatával eszközök kezelése](device-management-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="cd579-185">tooget an overview of how toomanage device in hello Azure portal, see [managing devices using hello Azure portal](device-management-azure-portal.md)</span></span>

- <span data-ttu-id="cd579-186">További információ az eszközalapú feltételes hozzáférési toolearn lásd [Azure Active Directory eszközalapú feltételes hozzáférési házirendek konfigurálása](active-directory-conditional-access-policy-connected-applications.md).</span><span class="sxs-lookup"><span data-stu-id="cd579-186">toolearn more about device-based conditional access, see [configure Azure Active Directory device-based conditional access policies](active-directory-conditional-access-policy-connected-applications.md).</span></span>

- <span data-ttu-id="cd579-187">toosetup hibrid csatlakozott az Azure AD-eszközök esetén lásd: [hogyan tooconfigure hibrid Azure Active Directoryhoz csatlakoztatott eszközök](device-management-hybrid-azuread-joined-devices-setup.md).</span><span class="sxs-lookup"><span data-stu-id="cd579-187">toosetup hybrid Azure AD joined devices, see [how tooconfigure hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md).</span></span>


