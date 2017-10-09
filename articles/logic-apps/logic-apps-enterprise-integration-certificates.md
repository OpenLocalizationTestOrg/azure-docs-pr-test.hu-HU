---
title: "Vállalati integrációs csomag aaaUsing tanúsítványok |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse tanúsítványokat a vállalati integrációs csomag hello |} Az Azure Logic Apps alkalmazások"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 7ba9f597a03a852a9ba05d2af08fe4bc2d98aef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a><span data-ttu-id="4059e-103">A tanúsítványok és az Enterprise Integration Pack ismertetése</span><span class="sxs-lookup"><span data-stu-id="4059e-103">Learn about certificates and Enterprise Integration Pack</span></span>
## <a name="overview"></a><span data-ttu-id="4059e-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4059e-104">Overview</span></span>
<span data-ttu-id="4059e-105">Vállalati integrációs tanúsítványok toosecure B2B kommunikációhoz használ.</span><span class="sxs-lookup"><span data-stu-id="4059e-105">Enterprise integration uses certificates toosecure B2B communications.</span></span> <span data-ttu-id="4059e-106">A vállalati integrációs alkalmazások két típusú tanúsítványt használhatja:</span><span class="sxs-lookup"><span data-stu-id="4059e-106">You can use two types of certificates in your enterprise integration apps:</span></span>

* <span data-ttu-id="4059e-107">Nyilvános tanúsítványokat, amelyek egy hitelesítésszolgáltatótól (CA) kell megvásárolni.</span><span class="sxs-lookup"><span data-stu-id="4059e-107">Public certificates, which must be purchased from a certification authority (CA).</span></span>
* <span data-ttu-id="4059e-108">Személyes tanúsítványok, amelyek saját kezűleg adhat ki.</span><span class="sxs-lookup"><span data-stu-id="4059e-108">Private certificates, which you can issue yourself.</span></span> <span data-ttu-id="4059e-109">Ezek a tanúsítványok néha hivatkozott tooas önaláírt tanúsítványokat is.</span><span class="sxs-lookup"><span data-stu-id="4059e-109">These certificates are sometimes referred tooas self-signed certificates.</span></span>

## <a name="what-are-certificates"></a><span data-ttu-id="4059e-110">Mik azok a tanúsítványok?</span><span class="sxs-lookup"><span data-stu-id="4059e-110">What are certificates?</span></span>
<span data-ttu-id="4059e-111">Tanúsítványok olyan digitális dokumentumok, ellenőrizze, hogy hello azonosságát hello résztvevők elektronikus kommunikáció, majd, amely is biztonságos elektronikus kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="4059e-111">Certificates are digital documents that verify hello identity of hello participants in electronic communications and that also secure electronic communications.</span></span>

## <a name="why-use-certificates"></a><span data-ttu-id="4059e-112">Tanúsítványok miért érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="4059e-112">Why use certificates?</span></span>
<span data-ttu-id="4059e-113">Egyes esetekben B2B kommunikációs kell tartani bizalmas.</span><span class="sxs-lookup"><span data-stu-id="4059e-113">Sometimes B2B communications must be kept confidential.</span></span> <span data-ttu-id="4059e-114">Vállalati integrációs használ a tanúsítványok toosecure ezek a kommunikációk két módon:</span><span class="sxs-lookup"><span data-stu-id="4059e-114">Enterprise integration uses certificates toosecure these communications in two ways:</span></span>

* <span data-ttu-id="4059e-115">Az üzenetek hello tartalma titkosításával</span><span class="sxs-lookup"><span data-stu-id="4059e-115">By encrypting hello contents of messages</span></span>
* <span data-ttu-id="4059e-116">Üzenetek digitális aláírásával</span><span class="sxs-lookup"><span data-stu-id="4059e-116">By digitally signing messages</span></span>  

## <a name="upload-a-public-certificate"></a><span data-ttu-id="4059e-117">Nyilvános tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="4059e-117">Upload a public certificate</span></span>

<span data-ttu-id="4059e-118">toouse egy *nyilvános tanúsítvány* a logic apps B2B képességeket, először kell tooupload hello tanúsítvány integrációs fiókjába.</span><span class="sxs-lookup"><span data-stu-id="4059e-118">toouse a *public certificate* in your logic apps with B2B capabilities, you first need tooupload hello certificate into your integration account.</span></span>  

<span data-ttu-id="4059e-119">A tanúsítvány feltöltése után-e a B2B üzenetek biztonságos, hello tulajdonságaik definiálásakor elérhető toohelp [megállapodások](logic-apps-enterprise-integration-agreements.md) az Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4059e-119">After you upload a certificate, it's available toohelp you secure your B2B messages when you define their properties in hello [agreements](logic-apps-enterprise-integration-agreements.md) that you create.</span></span>  

<span data-ttu-id="4059e-120">Az alábbiakban a hello feltöltése a nyilvános tanúsítványokat integrációs fiókjába, Azure-portálon toohello bejelentkezés után részletes lépései:</span><span class="sxs-lookup"><span data-stu-id="4059e-120">Here are hello detailed steps for uploading your public certificates into your integration account after you sign in toohello Azure portal:</span></span>

1. <span data-ttu-id="4059e-121">Válassza ki **további szolgáltatások** , és írja be **integrációs** hello szűrő Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="4059e-121">Select **More services** and enter **integration** in hello filter search box.</span></span> <span data-ttu-id="4059e-122">Válassza ki **integrációs fiókok** a hello eredményeinek listája</span><span class="sxs-lookup"><span data-stu-id="4059e-122">Select **Integration Accounts** from hello results list</span></span>     
<span data-ttu-id="4059e-123">![Válassza ki a Tallózás gombra](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span><span class="sxs-lookup"><span data-stu-id="4059e-123">![Select Browse](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span></span>  
2. <span data-ttu-id="4059e-124">Válassza ki a hello integrációs fiók toowhich szeretne tooadd hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="4059e-124">Select hello integration account toowhich you want tooadd hello certificate.</span></span>  
![Válassza ki a hello integrációs fiók toowhich szeretne tooadd hello tanúsítványt](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. <span data-ttu-id="4059e-126">Jelölje be hello **tanúsítványok** csempére.</span><span class="sxs-lookup"><span data-stu-id="4059e-126">Select hello **Certificates** tile.</span></span>  
<span data-ttu-id="4059e-127">![Jelölje be hello tanúsítványok csempe](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="4059e-127">![Select hello Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>
4. <span data-ttu-id="4059e-128">A hello **tanúsítványok** panelt megnyitó, válassza hello **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4059e-128">In hello **Certificates** blade that opens, select hello **Add** button.</span></span>   
<span data-ttu-id="4059e-129">![Válassza ki a hello Hozzáadás gomb](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="4059e-129">![Select hello Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
5. <span data-ttu-id="4059e-130">Adjon meg egy **neve** a tanúsítványt, és jelölje ki hello tanúsítvány típusú, mint **nyilvános** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="4059e-130">Enter a **Name** for your certificate, and then select hello certificate type as **public** from hello dropdown.</span></span>  
6. <span data-ttu-id="4059e-131">Hello hello jobb oldalán válassza hello ikonja **tanúsítvány** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="4059e-131">Select hello folder icon on hello right side of hello **Certificate** text box.</span></span> <span data-ttu-id="4059e-132">Hello fájlválasztó megnyitása, keresse meg és jelölje ki a megjeleníteni kívánt tooupload tooyour integrációs fiók hello tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="4059e-132">When hello file picker opens, find and select hello certificate file that you want tooupload tooyour integration account.</span></span>
7. <span data-ttu-id="4059e-133">Hello tanúsítványt, majd válassza ki és **OK** a hello fájlválasztó.</span><span class="sxs-lookup"><span data-stu-id="4059e-133">Select hello certificate, and then select **OK** in hello file picker.</span></span> <span data-ttu-id="4059e-134">Ez érvényesíti, és feltölti a hello tanúsítvány tooyour integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="4059e-134">This validates and uploads hello certificate tooyour integration account.</span></span>
8. <span data-ttu-id="4059e-135">Végezetül biztonsági hello **Hozzáadás tanúsítvány** panelen, jelölje be hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4059e-135">Finally, back on hello **Add certificate** blade, select hello **OK** button.</span></span>  
<span data-ttu-id="4059e-136">![Válassza ki a hello OK gomb](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span><span class="sxs-lookup"><span data-stu-id="4059e-136">![Select hello OK button](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span></span>  
9. <span data-ttu-id="4059e-137">Jelölje be hello **tanúsítványok** csempére.</span><span class="sxs-lookup"><span data-stu-id="4059e-137">Select hello **Certificates** tile.</span></span> <span data-ttu-id="4059e-138">Meg kell jelennie a hello hozzáadta az új tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="4059e-138">You should see hello newly added certificate.</span></span>  
<span data-ttu-id="4059e-139">![Tekintse meg az új tanúsítvány hello](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span><span class="sxs-lookup"><span data-stu-id="4059e-139">![See hello new certificate](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span></span>  

## <a name="upload-a-private-certificate"></a><span data-ttu-id="4059e-140">Személyes tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="4059e-140">Upload a private certificate</span></span>

<span data-ttu-id="4059e-141">toouse egy *személyes tanúsítvány* a logic apps B2B képességeket, a feltölthet egy személyes tanúsítvány tooyour integrációs fiók véve hello lépések</span><span class="sxs-lookup"><span data-stu-id="4059e-141">toouse a *private certificate* in your logic apps with B2B capabilities, You can upload a private certificate tooyour integration account by taking hello following steps</span></span>

1. <span data-ttu-id="4059e-142">[Töltse fel a titkos kulcs tooKey tároló](../key-vault/key-vault-get-started.md "további információ a Key Vault") , és adjon meg egy **kulcs neve**</span><span class="sxs-lookup"><span data-stu-id="4059e-142">[Upload your private key tooKey Vault](../key-vault/key-vault-get-started.md "Learn about Key Vault") and provide a **Key Name**</span></span> 
   
   > [!TIP]
   > <span data-ttu-id="4059e-143">Engedélyeznie kell a Key Vault Logic Apps tooperform műveletek.</span><span class="sxs-lookup"><span data-stu-id="4059e-143">You must authorize Logic Apps tooperform operations on Key Vault.</span></span> <span data-ttu-id="4059e-144">Hozzáférés toohello Logic Apps szolgáltatás egyszerű hello a következő PowerShell-parancs használatával adja meg:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span><span class="sxs-lookup"><span data-stu-id="4059e-144">You can grant access toohello Logic Apps service principal by using hello following PowerShell command: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span></span>  
   > 
   > 

<span data-ttu-id="4059e-145">Miután hello előző lépést, a személyes tanúsítvány toointegration fiók hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="4059e-145">After you've taken hello previous step, add a private certificate toointegration account.</span></span>

<span data-ttu-id="4059e-146">Következő vannak hello részletes, lépésenkénti leírását a személyes tanúsítványok feltöltése a integrációs fiókjába, a bejelentkezést toohello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="4059e-146">Following are hello detailed steps for uploading your private certificates into your integration account after you sign in toohello Azure portal:</span></span>  
 
1. <span data-ttu-id="4059e-147">Válassza ki a hello integrációs fiók toowhich szeretné, hogy tooadd hello tanúsítványt, és válassza ki a hello **tanúsítványok** csempére.</span><span class="sxs-lookup"><span data-stu-id="4059e-147">Select hello integration account toowhich you want tooadd hello certificate and select hello **Certificates** tile.</span></span>  
<span data-ttu-id="4059e-148">![Jelölje be hello tanúsítványok csempe](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="4059e-148">![Select hello Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>  
2. <span data-ttu-id="4059e-149">A hello **tanúsítványok** panelt megnyitó, válassza hello **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4059e-149">In hello **Certificates** blade that opens, select hello **Add** button.</span></span>   
<span data-ttu-id="4059e-150">![Válassza ki a hello Hozzáadás gomb](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="4059e-150">![Select hello Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
3. <span data-ttu-id="4059e-151">Adjon meg egy **neve** a tanúsítványt, és jelölje be hello tanúsítvány típusú, mint **titkos** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="4059e-151">Enter a **Name** for your certificate, and select hello certificate type as **private** from hello dropdown.</span></span>   
4. <span data-ttu-id="4059e-152">Válassza ki a hello ikonja hello jobb oldalán hello **tanúsítvány** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="4059e-152">select hello folder icon on hello right side of hello **Certificate** text box.</span></span> <span data-ttu-id="4059e-153">Hello fájlválasztó megnyitása után hello megfelelő nyilvános tanúsítvány, amelyet az tooupload tooyour integrációs fiók található.</span><span class="sxs-lookup"><span data-stu-id="4059e-153">When hello file picker opens, find hello corresponding public certificate that you want tooupload tooyour integration account.</span></span>   
   
   > [!Note]
   > <span data-ttu-id="4059e-154">Személyes tanúsítvány hozzáadása során fontos, megfelelő nyilvános tooadd tanúsítvány a tooshow [AS2 megállapodás](logic-apps-enterprise-integration-as2.md) beállítások aláírásához és titkosításához üdvözlő üzenetek küldésére és fogadására.</span><span class="sxs-lookup"><span data-stu-id="4059e-154">While adding a private certificate it is important tooadd corresponding public certificate tooshow in [AS2 agreement](logic-apps-enterprise-integration-as2.md) receive and send settings for signing and encrypting hello messages.</span></span>
   > 
   >   

5. <span data-ttu-id="4059e-155">Jelölje be hello **erőforráscsoport**, **Key Vault**, **kulcsnév** és select hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4059e-155">Select hello **Resource Group**, **Key Vault**, **Key Name** and select hello **OK** button.</span></span>  
<span data-ttu-id="4059e-156">![Tanúsítvány hozzáadása](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="4059e-156">![Add certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span></span>  
6. <span data-ttu-id="4059e-157">Jelölje be hello **tanúsítványok** csempére.</span><span class="sxs-lookup"><span data-stu-id="4059e-157">Select hello **Certificates** tile.</span></span> <span data-ttu-id="4059e-158">Meg kell jelennie a hello hozzáadta az új tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="4059e-158">You should see hello newly added certificate.</span></span>
<span data-ttu-id="4059e-159">![Tekintse meg az új tanúsítvány hello](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="4059e-159">![See hello new certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span></span>  



* [<span data-ttu-id="4059e-160">B2B-egyezmény létrehozása</span><span class="sxs-lookup"><span data-stu-id="4059e-160">Create a B2B agreement</span></span>](logic-apps-enterprise-integration-agreements.md)  
* [<span data-ttu-id="4059e-161">További információ a Key Vault</span><span class="sxs-lookup"><span data-stu-id="4059e-161">Learn more about Key Vault</span></span>](../key-vault/key-vault-get-started.md "Key Vault megismerése")  

