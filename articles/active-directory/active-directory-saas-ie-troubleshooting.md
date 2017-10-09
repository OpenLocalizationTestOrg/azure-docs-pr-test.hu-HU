---
title: "aaaTroubleshooting hello Azure hozzáférési Panel bővítményét az Internet Explorer |} Microsoft Docs"
description: "Hogyan toouse csoport házirend toodeploy hello Internet Explorer bővítmény hello saját alkalmazások portálhoz."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56b3230-26fd-42ec-9e3d-2c12daf15479
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23cbb6117f34759330206de3a26f1397486acedb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-access-panel-extension-for-internet-explorer"></a>Az Internet Explorer hello hozzáférési Panel bővítmény hibaelhárítása
Ez a cikk hello a következő problémák elhárításához nyújt segítséget:

* Ön éppen nem tooaccess hello saját alkalmazások portálon keresztül az alkalmazások Internet Explorer használata során.
* Lásd a "Szoftver telepítése" üzenet, annak ellenére, hogy a korábban telepített hello szoftver hello.

Ha Ön rendszergazda, további információ: [hogyan tooDeploy hello hozzáférési Panel bővítményét az Internet Explorer csoportházirend használatával](active-directory-saas-ie-group-policy.md)

## <a name="run-hello-diagnostic-tool"></a>Hello diagnosztikai eszköz futtatása
Ha letölti és hello hozzáférési Panel diagnosztikai eszközt futtató felderítheti hello hozzáférési Panel bővítmény telepítési problémákat:

1. [Kattintson ide a toodownload hello diagnosztikai eszköz.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Nyitott hello fájlt, és nyomja le az **az összes kibontása** gombra.
   
    ![Nyomja meg az összes kibontása](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. Nyomja le az hello **kibontása** gomb toocontinue.
   
    ![Nyomja le az Extract](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. toorun hello eszközt, kattintson a jobb gombbal hello fájlt **AccessPanelExtensionDiagnosticTool**, majd jelölje be **nyissa meg a > a Microsoft Windows alapú parancsfájlfuttató**.
   
    ![Nyissa meg a > a Microsoft Windows alapú parancsfájlfuttató](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. Ekkor megjelenik a következő diagnosztikai ablak, amely leírja, milyen lehet még a telepítés hello.
   
    ![Egy minta hello diagnosztikai ablak](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. Kattintson a "**Igen**" toolet hello program javítás hello problémákat talált.
7. a módosítások, toosave minden Internet Explorer ablak bezárása, és ezután nyissa meg az Internet Explorert.<br />Ha továbbra sem tud hozzáférni az alkalmazásokat, próbálja hello alábbi lépéseket.

## <a name="check-that-hello-access-panel-extension-is-enabled"></a>Ellenőrizze, hogy engedélyezve van a hozzáférési Panel bővítmény hello
amely a hozzáférési Panel bővítmény hello tooverify engedélyezve van az Internet Explorerben:

1. Az Internet Explorer, kattintson a hello **fogaskerék ikonra** a hello jobb felső sarkában hello ablak. Válassza ki **Internetbeállítások**.<br />(Az Internet Explorer korábbi verzióiban ez alatt található **eszközök > Internetbeállítások**.
   
    ![Nyissa meg tooTools > Internetbeállítások](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. Kattintson a hello **programok** lapra, majd kattintson az hello **bővítmények kezelése** gombra.
   
    ![Kattintson a bővítmények kezelése](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. Ezen a párbeszédpanelen válassza ki a **hozzáférési Panel bővítmény** majd hello **engedélyezése** gombra.
   
    ![Kattintson az Engedélyezés parancsra](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. a módosítások, toosave minden Internet Explorer ablak bezárása, és ezután nyissa meg az Internet Explorert.

## <a name="enable-extensions-for-inprivate-browsing"></a>Bővítmények engedélyezése az InPrivate-böngészés
Ha a hello InPrivate-böngészés módot:

1. Az Internet Explorer, kattintson a hello **fogaskerék ikonra** a hello jobb felső sarkában hello ablak. Válassza ki **Internetbeállítások**.<br />(Az Internet Explorer korábbi verzióiban ez alatt található **eszközök > Internetbeállítások**.
   
    ![Egy minta hello diagnosztikai ablak](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. Nyissa meg toohello **adatvédelmi** lapra, majd **, törölje a jelet** hello feliratú jelölőnégyzet **letiltása eszköztárak és kiterjesztések InPrivate-böngészés indításakor**</p>
   
    ![Törölje a jelet letiltása eszköztárak és kiterjesztések InPrivate-böngészés indításakor](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. a módosítások, toosave minden Internet Explorer ablak bezárása, és ezután nyissa meg az Internet Explorert.

## <a name="uninstall-hello-access-panel-extension"></a>Távolítsa el a hozzáférési Panel bővítmény hello
toouninstall hello hozzáférési Panel bővítmény a számítógépről:

1. A billentyűzeten, nyomja le az ENTER hello **Windows kulcs** tooopen hello Start menüből. Ha hello menü meg nyitva, beírhatja minden toodo keresést. Írja be a "Vezérlőpult", és nyissa meg hello **Vezérlőpult** amikor megjelenik a hello keresési eredmények között.
   
    ![Keresse meg a Vezérlőpult](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. A hello jobb felső sarkában hello Vezérlőpult, módosítsa a hello **megtekintéséhez** beállítás túl**nagy ikonok**. Majd keresse meg és kattintson a hello **programok és szolgáltatások** gombra.
   
    ![Átalakítás hello nézet tooshow nagy ikonok](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. Hello listájából válassza ki **hozzáférési Panel bővítmény**, és kattintson a hello hello **Eltávolítás** gombra.
   
    ![Kattintson az Eltávolítás gombra](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. Majd tooinstall hello bővítmény megpróbálhatja újra toosee Ha hello problémát sikerült megoldani.

Ha hibát tapasztal hello bővítmény eltávolítása, is eltávolíthatja azt hello segítségével [Microsoft javítsa ki az](https://go.microsoft.com/?linkid=9779673) eszköz.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md)
* [Hogyan tooDeploy hello hozzáférési Panel bővítményét az Internet Explorer csoportházirend használatával](active-directory-saas-ie-group-policy.md)

