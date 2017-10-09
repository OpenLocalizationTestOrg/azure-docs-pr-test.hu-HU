---
title: "a Visual Studio kapcsolódó szolgáltatások használatával egy Azure Active Directory aaaAdding |} Microsoft Docs"
description: "Adja hozzá az Azure Active Directory-alapú hello Visual Studio kapcsolódó szolgáltatások hozzáadása párbeszédpanelen"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: kraigb
ms.openlocfilehash: 26c8f68edf9ec5f7bf65cbab34e4f9b4085ed18d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Egy Azure Active Directory hozzáadása a kapcsolódó szolgáltatások a Visual Studio használatával
Azure Active Directory (Azure AD) segítségével a webes API-szolgáltatásokban támogathat egyszeri bejelentkezés (SSO) az ASP.NET MVC webes alkalmazásokhoz, vagy az Active Directory-hitelesítéssel. Az Azure Active Directory-hitelesítéssel a felhasználók fiókokat fogja tudni használni az Azure Active Directory tooconnect tooyour webes alkalmazásokból. Azure Active Directory Authentication webes API-t hello előnyei közé tartozik továbbfejlesztett adatok biztonsága, amikor egy API-webalkalmazások kitettségének. Az Azure ad-vel nincs toomanage egy külön hitelesítési rendszere saját fiókot és a felhasználó-kezeléssel.

## <a name="prerequisites"></a>Előfeltételek
- Azure-fiók – Ha nem rendelkezik Azure-fiókot, akkor [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-tooazure-active-directory-using-hello-connected-services-dialog"></a>TooAzure hello kapcsolódó szolgáltatásokat használó Active Directory Connect párbeszédpanel
1. A Visual Studio hozzon létre, vagy nyisson meg egy ASP.NET MVC projekt vagy egy ASP.NET Web API-projektet.

1. A Solution Explorer hello, kattintson a jobb gombbal hello **kapcsolódó szolgáltatások** csomópont, és hello helyi menüből válassza ki a **kapcsolódó szolgáltatások hozzáadása**.

1. A hello **kapcsolódó szolgáltatások** lapon jelölje be **hitelesítés az Azure Active Directory**.
   
    ![Kapcsolódó szolgáltatások](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. A hello **bemutatása** hello oldalán **konfigurálása Azure AD-alapú hitelesítés** varázslóban válassza **következő**.
   
    ![Bemutató lapja](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. A hello **egyszeri bejelentkezést a** hello oldalán **konfigurálása Azure AD-alapú hitelesítés** varázsló, válasszon ki egy tartományt a hello **tartomány** legördülő listából. tartományok listáját hello hello fiókjaihoz hello Fiókbeállítások párbeszédpanel által elérhető összes tartományt tartalmazza. Alternatív megoldásként adhat meg egy tartomány nevét, ha nem találja a hello keres, például egy `mydomain.onmicrosoft.com`. Válassza ki a hello beállítás toocreate egy Azure Active Directory-alkalmazást, vagy egy meglévő Azure Active Directory-alkalmazás hello beállításainak használata. Válassza ki **következő** végzett.
   
    ![Egyszeri bejelentkezés az oldalon](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. A hello **Directory elérés** hello oldalán **konfigurálása Azure AD-alapú hitelesítés** varázsló, győződjön meg arról, hogy hello **címtáradatok olvasása** beállítást. 
   
    ![Directory adatelérési lap](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Válassza ki **Befejezés** tooadd hello szükséges konfigurációs kódot és hivatkozások tooenable a projekthez az Azure AD-alapú hitelesítés. Hello tekintheti meg az Active Directory-tartomány hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. A Visual Studio megjeleníti a [Mi történt](#how-your-project-is-modified) cikk tooshow meg módját a projekt módosítva lett. Ha azt szeretné, hogy még működött toocheck, nyissa meg az egyik hello módosított konfigurációs fájlokat, és győződjön meg arról, hogy hello cikkben említett hello-beállítások vannak-e. 

## <a name="how-your-project-is-modified"></a>A projekt módosítása hogyan
Hello varázsló futtatásakor a Visual Studio Azure Active Directory és a kapcsolódó hivatkozások tooyour projekt hozzáadása. A konfigurációs fájlok és a projekt kódfájlok egyaránt módosított tooadd támogatás az Azure AD. hello adott módosításokat, amelynek eredményeképpen a Visual Studio hello projekt típusától függ. Hogyan módosultak a ASP.NET MVC-projektek kapcsolatos részletes információkért lásd: [milyen happened – MVC projektek](http://go.microsoft.com/fwlink/p/?LinkID=513809). Webes API-projektet, lásd: [Mi történt – a webes API-projektek](http://go.microsoft.com/fwlink/p/?LinkId=513810).

## <a name="next-steps"></a>Következő lépések
* [Az Azure Active Directory MSDN fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [Azure Active Directory dokumentációja](https://azure.microsoft.com/documentation/services/active-directory/)
* [Blogbejegyzés: Bevezetés tooAzure Active Directory](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

