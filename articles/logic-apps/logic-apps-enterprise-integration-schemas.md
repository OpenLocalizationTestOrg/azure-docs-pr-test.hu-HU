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
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-hello-enterprise-integration-pack"></a>Ellenőrizze az XML a sémák Azure Logic Apps és hello vállalati integrációs csomag

Sémák ellenőrizze, hogy hello XML-dokumentumok kap érvényes és hello várható rendelkező adatok előre meghatározott formátumban. Sémák is hozzájárulhat a B2B forgatókönyvek cserélt üzeneteket ellenőrzése.

## <a name="add-a-schema"></a>A séma hozzáadása

1. Hello Azure-portálon, válassza ki **további szolgáltatások**.

    ![Azure-portálon "Szolgáltatás"](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. Hello szűrő keresési mezőbe, írja be a **integrációs**, és válassza ki **integrációs fiókok** hello eredmények listából.

    ![Szűrő keresőmezőbe](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. Jelölje be hello **integrációs fiók** tooadd hello séma, ahová.

    ![Integrációs fiókok listája](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. Válassza ki a hello **sémák** csempére.

    ![Példa integrációs fiók "Sémák"](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a>Egy 2 MB-nál kisebb sémafájl hozzáadása

1. A hello **sémák** panel (az előző lépésekben hello), megnyílik a választható **Hozzáadás**.

    !["Add" sémák panelen](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. Adjon meg egy nevet a sémát. Töltse fel a hello sémafájl hello mappa ikon következő toohello kiválasztásával **séma** mezőbe. Hello feltöltési folyamat befejezése után válassza ki a **OK**.

    ![Képernyőkép a "Séma hozzáadása", "Kis fájl" kiemelve](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-too8-mb-maximum"></a>A (felfelé too8 MB maximális) 2 MB-nál nagyobb sémafájl hozzáadása

Ezeket a lépéseket attól függően változnak, a hello blob tároló hozzáférési szint: **nyilvános** vagy **nincs névtelen hozzáférés**.

**toodetermine a hozzáférési szint**

1.  Nyissa meg **Azure Tártallózó**. 

2.  A **Blob tárolók**, jelölje be szeretné hello blob-tároló. 

3.  Válassza ki **biztonsági**, **hozzáférési szint**.

Ha hello blob biztonsági hozzáférési szint **nyilvános**, kövesse az alábbi lépéseket.

![Az Azure Tártallózó "Blobtárolók", "Biztonság" és "Nyilvános" kiemelve](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. Töltse fel a hello séma tooyour tárfiók, és másolja a hello URI.

    ![Storage-fiókkal, és a kijelölt URI](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. A **séma hozzáadása**, jelölje be **nagy méretű fájlok**, és adja meg a hello URI hello **tartalom URI** szövegmezőben.

    ![A "Hozzáadás" gombra, és a "Nagy file" kiemelt a sémák](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

Ha hello blob biztonsági hozzáférési szint **nincs névtelen hozzáférés**, kövesse az alábbi lépéseket.

![Az Azure Tártallózó "Blobtárolók", "Biztonság" és "Nincs névtelen hozzáférés" kiemelve](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. Töltse fel a hello séma tooyour storage-fiók.

    ![Tárfiók](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. A közös hozzáférésű jogosultságkód hello séma készítése.

    ![A tárfiók, megosztott elérési aláírást lap kiemelve](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. A **séma hozzáadása**, jelölje be **nagy méretű fájlok**, és adja meg a hello közös hozzáférésű jogosultságkódot URI a hello **tartalom URI** szövegmezőben.

    ![A "Hozzáadás" gombra, és a "Nagy file" kiemelt a sémák](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. A hello **sémák** integrációs fiókja panelen az újonnan hozzáadott sémát kell megjelennie.

    ![Az integrációs fiókját, a "Sémák" és a kiemelt hello új séma](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Sémák szerkesztése

1. Válassza ki a hello **sémák** csempére.

2. Hello után **sémák** panel nyílik meg, válassza hello séma, amelyet az tooedit.

3. A hello **sémák** paneljén válassza **szerkesztése**.

    ![Sémák panel](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. Jelölje be hello sémafájl, hogy szeretné, hogy tooedit, majd válassza ki **nyitott**.

    ![Nyissa meg séma fájl tooedit](media/logic-apps-enterprise-integration-schemas/edit-31.png)

Az Azure mutat be egy üzenet, amely séma hello sikeresen feltöltve.

## <a name="delete-schemas"></a>Sémák törlése

1. Válassza ki a hello **sémák** csempére.

2. Hello után **sémák** panel nyílik meg, válassza hello sémát, toodelete.

3. A hello **sémák** paneljén válassza **törlése**.

    ![Sémák panel](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. megjeleníteni kívánt toodelete hello tooconfirm kiválasztva séma, válassza a **Igen**.

    ![Jóváhagyást kérő üzenet "Delete séma"](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    A hello **sémák** panelen hello séma lista frissül, és már nem tartalmazza a törölt hello séma.

    ![A fiókhoz, amely a kijelölt "sémák" integráció](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a hello vállalati integrációs csomag").  

