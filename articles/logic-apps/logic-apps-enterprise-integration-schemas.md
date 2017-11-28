---
title: "az XML-érvényesítés - Azure Logic Apps aaaSchemas |} Microsoft Docs"
description: "XML-sémák dokumentumok ellenőrzése az Azure Logic Apps és vállalati integrációs csomag"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 87cf92741e10ff7cccd260f27442909e34928903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-hello-enterprise-integration-pack"></a><span data-ttu-id="64c5c-103">Ellenőrizze az XML a sémák Azure Logic Apps és hello vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="64c5c-103">Validate XML with schemas for Azure Logic Apps and hello Enterprise Integration Pack</span></span>

<span data-ttu-id="64c5c-104">Sémák ellenőrizze, hogy hello XML-dokumentumok kap érvényes és hello várható rendelkező adatok előre meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="64c5c-104">Schemas confirm that hello XML documents you receive are valid and have hello expected data in a predefined format.</span></span> <span data-ttu-id="64c5c-105">Sémák is hozzájárulhat a B2B forgatókönyvek cserélt üzeneteket ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="64c5c-105">Schemas also help validate messages that are exchanged in a B2B scenario.</span></span>

## <a name="add-a-schema"></a><span data-ttu-id="64c5c-106">A séma hozzáadása</span><span class="sxs-lookup"><span data-stu-id="64c5c-106">Add a schema</span></span>

1. <span data-ttu-id="64c5c-107">Hello Azure-portálon, válassza ki **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-107">In hello Azure portal, select **More services**.</span></span>

    ![Azure-portálon "Szolgáltatás"](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. <span data-ttu-id="64c5c-109">Hello szűrő keresési mezőbe, írja be a **integrációs**, és válassza ki **integrációs fiókok** hello eredmények listából.</span><span class="sxs-lookup"><span data-stu-id="64c5c-109">In hello filter search box, enter **integration**, and select **Integration Accounts** from hello results list.</span></span>

    ![Szűrő keresőmezőbe](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. <span data-ttu-id="64c5c-111">Jelölje be hello **integrációs fiók** tooadd hello séma, ahová.</span><span class="sxs-lookup"><span data-stu-id="64c5c-111">Select hello **integration account** where you want tooadd hello schema.</span></span>

    ![Integrációs fiókok listája](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. <span data-ttu-id="64c5c-113">Válassza ki a hello **sémák** csempére.</span><span class="sxs-lookup"><span data-stu-id="64c5c-113">Choose hello **Schemas** tile.</span></span>

    ![Példa integrációs fiók "Sémák"](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a><span data-ttu-id="64c5c-115">Egy 2 MB-nál kisebb sémafájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="64c5c-115">Add a schema file smaller than 2 MB</span></span>

1. <span data-ttu-id="64c5c-116">A hello **sémák** panel (az előző lépésekben hello), megnyílik a választható **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-116">In hello **Schemas** blade that opens (from hello preceding steps), choose **Add**.</span></span>

    !["Add" sémák panelen](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. <span data-ttu-id="64c5c-118">Adjon meg egy nevet a sémát.</span><span class="sxs-lookup"><span data-stu-id="64c5c-118">Enter a name for your schema.</span></span> <span data-ttu-id="64c5c-119">Töltse fel a hello sémafájl hello mappa ikon következő toohello kiválasztásával **séma** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="64c5c-119">Upload hello schema file by selecting hello folder icon next toohello **Schema** box.</span></span> <span data-ttu-id="64c5c-120">Hello feltöltési folyamat befejezése után válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-120">After hello upload process completes, select **OK**.</span></span>

    ![Képernyőkép a "Séma hozzáadása", "Kis fájl" kiemelve](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-too8-mb-maximum"></a><span data-ttu-id="64c5c-122">A (felfelé too8 MB maximális) 2 MB-nál nagyobb sémafájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="64c5c-122">Add a schema file larger than 2 MB (up too8 MB maximum)</span></span>

<span data-ttu-id="64c5c-123">Ezeket a lépéseket attól függően változnak, a hello blob tároló hozzáférési szint: **nyilvános** vagy **nincs névtelen hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-123">These steps differ based on hello blob container access level: **Public** or **No anonymous access**.</span></span>

<span data-ttu-id="64c5c-124">**toodetermine a hozzáférési szint**</span><span class="sxs-lookup"><span data-stu-id="64c5c-124">**toodetermine this access level**</span></span>

1.  <span data-ttu-id="64c5c-125">Nyissa meg **Azure Tártallózó**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-125">Open **Azure Storage Explorer**.</span></span> 

2.  <span data-ttu-id="64c5c-126">A **Blob tárolók**, jelölje be szeretné hello blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="64c5c-126">Under **Blob Containers**, select hello blob container you want.</span></span> 

3.  <span data-ttu-id="64c5c-127">Válassza ki **biztonsági**, **hozzáférési szint**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-127">Select **Security**, **Access Level**.</span></span>

<span data-ttu-id="64c5c-128">Ha hello blob biztonsági hozzáférési szint **nyilvános**, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="64c5c-128">If hello blob security access level is **Public**, follow these steps.</span></span>

![Az Azure Tártallózó "Blobtárolók", "Biztonság" és "Nyilvános" kiemelve](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. <span data-ttu-id="64c5c-130">Töltse fel a hello séma tooyour tárfiók, és másolja a hello URI.</span><span class="sxs-lookup"><span data-stu-id="64c5c-130">Upload hello schema tooyour storage account, and copy hello URI.</span></span>

    ![Storage-fiókkal, és a kijelölt URI](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. <span data-ttu-id="64c5c-132">A **séma hozzáadása**, jelölje be **nagy méretű fájlok**, és adja meg a hello URI hello **tartalom URI** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="64c5c-132">In **Add Schema**, select **Large file**, and provide hello URI in hello **Content URI** text box.</span></span>

    ![A "Hozzáadás" gombra, és a "Nagy file" kiemelt a sémák](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

<span data-ttu-id="64c5c-134">Ha hello blob biztonsági hozzáférési szint **nincs névtelen hozzáférés**, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="64c5c-134">If hello blob security access level is **No anonymous access**, follow these steps.</span></span>

![Az Azure Tártallózó "Blobtárolók", "Biztonság" és "Nincs névtelen hozzáférés" kiemelve](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. <span data-ttu-id="64c5c-136">Töltse fel a hello séma tooyour storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="64c5c-136">Upload hello schema tooyour storage account.</span></span>

    ![Tárfiók](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. <span data-ttu-id="64c5c-138">A közös hozzáférésű jogosultságkód hello séma készítése.</span><span class="sxs-lookup"><span data-stu-id="64c5c-138">Generate a shared access signature for hello schema.</span></span>

    ![A tárfiók, megosztott elérési aláírást lap kiemelve](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. <span data-ttu-id="64c5c-140">A **séma hozzáadása**, jelölje be **nagy méretű fájlok**, és adja meg a hello közös hozzáférésű jogosultságkódot URI a hello **tartalom URI** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="64c5c-140">In **Add Schema**, select **Large file**, and provide hello shared access signature URI in hello **Content URI** text box.</span></span>

    ![A "Hozzáadás" gombra, és a "Nagy file" kiemelt a sémák](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. <span data-ttu-id="64c5c-142">A hello **sémák** integrációs fiókja panelen az újonnan hozzáadott sémát kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="64c5c-142">In hello **Schemas** blade of your integration account, your newly added schema should appear.</span></span>

    ![Az integrációs fiókját, a "Sémák" és a kiemelt hello új séma](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a><span data-ttu-id="64c5c-144">Sémák szerkesztése</span><span class="sxs-lookup"><span data-stu-id="64c5c-144">Edit schemas</span></span>

1. <span data-ttu-id="64c5c-145">Válassza ki a hello **sémák** csempére.</span><span class="sxs-lookup"><span data-stu-id="64c5c-145">Choose hello **Schemas** tile.</span></span>

2. <span data-ttu-id="64c5c-146">Hello után **sémák** panel nyílik meg, válassza hello séma, amelyet az tooedit.</span><span class="sxs-lookup"><span data-stu-id="64c5c-146">After hello **Schemas** blade opens, select hello schema that you want tooedit.</span></span>

3. <span data-ttu-id="64c5c-147">A hello **sémák** paneljén válassza **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-147">On hello **Schemas** blade, choose **Edit**.</span></span>

    ![Sémák panel](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. <span data-ttu-id="64c5c-149">Jelölje be hello sémafájl, hogy szeretné, hogy tooedit, majd válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-149">Select hello schema file that you want tooedit, then select **Open**.</span></span>

    ![Nyissa meg séma fájl tooedit](media/logic-apps-enterprise-integration-schemas/edit-31.png)

<span data-ttu-id="64c5c-151">Az Azure mutat be egy üzenet, amely séma hello sikeresen feltöltve.</span><span class="sxs-lookup"><span data-stu-id="64c5c-151">Azure shows a message that hello schema uploaded successfully.</span></span>

## <a name="delete-schemas"></a><span data-ttu-id="64c5c-152">Sémák törlése</span><span class="sxs-lookup"><span data-stu-id="64c5c-152">Delete schemas</span></span>

1. <span data-ttu-id="64c5c-153">Válassza ki a hello **sémák** csempére.</span><span class="sxs-lookup"><span data-stu-id="64c5c-153">Choose hello **Schemas** tile.</span></span>

2. <span data-ttu-id="64c5c-154">Hello után **sémák** panel nyílik meg, válassza hello sémát, toodelete.</span><span class="sxs-lookup"><span data-stu-id="64c5c-154">After hello **Schemas** blade opens, select hello schema you want toodelete.</span></span>

3. <span data-ttu-id="64c5c-155">A hello **sémák** paneljén válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-155">On hello **Schemas** blade, choose **Delete**.</span></span>

    ![Sémák panel](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. <span data-ttu-id="64c5c-157">megjeleníteni kívánt toodelete hello tooconfirm kiválasztva séma, válassza a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="64c5c-157">tooconfirm that you want toodelete hello selected schema, choose **Yes**.</span></span>

    ![Jóváhagyást kérő üzenet "Delete séma"](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    <span data-ttu-id="64c5c-159">A hello **sémák** panelen hello séma lista frissül, és már nem tartalmazza a törölt hello séma.</span><span class="sxs-lookup"><span data-stu-id="64c5c-159">In hello **Schemas** blade, hello schema list refreshes  and no longer includes hello schema that you deleted.</span></span>

    ![A fiókhoz, amely a kijelölt "sémák" integráció](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a><span data-ttu-id="64c5c-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64c5c-161">Next steps</span></span>
* <span data-ttu-id="64c5c-162">[További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a hello vállalati integrációs csomag").</span><span class="sxs-lookup"><span data-stu-id="64c5c-162">[Learn more about hello Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about hello enterprise integration pack").</span></span>  

