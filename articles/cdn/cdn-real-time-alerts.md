---
title: "aaaAzure CDN valós idejű riasztások |} Microsoft Docs"
description: "A Microsoft Azure CDN valós idejű riasztásokat. Valós idejű riasztások adja meg a CDN-profil hello végpontok hello teljesítményével kapcsolatos értesítéseket."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 269c90437088bbc41bf899a8c02749e8e6f3006c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>A Microsoft Azure CDN valós idejű riasztások
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Áttekintés
Ez a dokumentum ismerteti a Microsoft Azure CDN valós idejű riasztásokat. Ezt a funkciót biztosít a valós idejű értesítések hello teljesítmény hello végpontok a CDN-profilt.  E-mailek vagy a HTTP-riasztások alapján állíthat be:

* Sávszélesség
* Állapotkódok
* Gyorsítótár-állapotok
* Kapcsolatok

## <a name="creating-a-real-time-alert"></a>A valós idejű riasztások létrehozása
1. A hello [Azure Portal](https://portal.azure.com), keresse meg a tooyour CDN-profilt.
   
    ![CDN-profil panelje](./media/cdn-real-time-alerts/cdn-profile-blade.png)
2. A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    Megnyílik a hello CDN felügyeleti portálon.
3. Hello az egérmutatót **Analytics** fülre, majd az egérmutatót hello **valós idejű statisztikák** menü.  Kattintson a **valós idejű riasztások**.
   
    ![CDN-felügyeleti portálon](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    megjelenik a meglévő riasztás konfigurációk (ha van ilyen) hello listája.
4. Kattintson a hello **riasztási hozzáadása** gombra.
   
    ![Riasztási gomb felvétele](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    Új riasztás létrehozásához űrlap jelenik meg.
   
    ![Új riasztás űrlap](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. Ha azt szeretné, a riasztás toobe aktív elemre **mentése**, ellenőrizze a hello **riasztás engedélyezve** jelölőnégyzetet.
6. Adjon meg egy leíró nevet a riasztáshoz hello **neve** mező.
7. A hello **médiatípus** legördülő menüből válassza **HTTP nagy objektum**.
   
    ![A kijelölt HTTP nagy objektum adathordozó-típus](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > Ki kell választania **HTTP nagy objektum** , hello **médiatípus**.  hello egyéb lehetőségek nem használja a **Azure CDN Verizon**.  Hiba tooselect **HTTP nagy objektum** , akkor a riasztás toonever elindul.
   > 
   > 
8. Hozzon létre egy **kifejezés** toomonitor kiválasztásával egy **metrika**, **operátor**, és **érték indítás**.
   
   * A **metrika**, válassza ki a kívánt figyelt feltétel hello típusa.  **Sávszélesség MB/s** hello mennyiségű sávszélesség-használat megabit / másodperc.  **Kapcsolatok száma összesen** hello száma párhuzamos HTTP-kapcsolatok tooour peremhálózati kiszolgálóinak van.  Hello definíciójának különböző gyorsítótár állapotok és állapotkódok, lásd: [Azure CDN gyorsítótár állapotkódok](https://msdn.microsoft.com/library/mt759237.aspx) és [Azure CDN HTTP-állapotkódok](https://msdn.microsoft.com/library/mt759238.aspx)
   * **Operátor** hello matematikai operátor hello hello metrika és hello eseményindító érték közötti kapcsolatot.
   * **Indítás, érték** van, amelyeknek teljesülniük kell ahhoz, egy értesítés küldése hello küszöbértéket.
     
     Az alábbi példában hello létrehoztam hello kifejezés azt jelzi, hogy értesítést kap, ha 404-es állapotkód hello száma nagyobb, mint 25 toobe szeretném.
     
     ![Valós idejű riasztási mintakifejezés](./media/cdn-real-time-alerts/cdn-expression.png)
9. A **időköz**, adja meg, hogy milyen gyakran szeretné hello kiértékelendő kifejezés.
10. A hello **értesítés** legördülő menüből válassza, ha szeretné toobe értesíti, ha a hello kifejezés értéke true.
    
    * **A feltétel Start** azt jelzi, hogy értesítést küld, amikor hello megadott feltétel először észlel.
    * **A feltétel End** azt jelzi, hogy értesítést küld, amikor hello megadott feltétel már nem észlelhető. Csak akkor is kiváltódik ezt az értesítést, miután a hálózatfigyelési rendszer azt észlelte, hogy a megadott hello állapot fordult elő.
    * **Folyamatos** azt jelzi, hogy értesítést küld minden alkalommal, amikor hálózati figyelési rendszer hello észleli hello megadott feltétel. Ne feledje, hogy hello hálózatfigyelési rendszer lesz csak ellenőrzés egyszer időszakonként hello a megadott feltétel.
    * **Az állapot kezdő és záró** azt jelzi, hogy egy értesítést küld-e az első alkalom, hogy a megadott feltétel hello hello észlel, és ismét amikor hello feltétel már nem észlelhető.
11. Ha azt szeretné, tooreceive értesítéseket e-mailben, ellenőrizze a hello **e-mailben értesítse** jelölőnégyzetet.  
    
    ![E-mailek űrlappal értesítése](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    A hello **való** mezőbe írja be a hello e-mail címet, ha azt szeretné, hogy értesítést küldeni. A **tulajdonos** és **törzs**, előfordulhat, hogy hagyja hello alapértelmezett vagy üdvözlőüzenetére hello segítségével szabhatja testre **elérhető kulcsszavak** lista toodynamically riasztási adatok beszúrása során hello üzenetet küld.
    
    > [!NOTE]
    > Hello e-mailben értesítést hello kattintva tesztelheti **ellenőrző értesítés** gomb, de csak hello riasztás konfigurációjának mentése után.
    > 
    > 
12. Ha azt szeretné, hogy a közzétett tooa webkiszolgáló értesítések toobe, ellenőrizze a hello **HTTP Post által értesítendő** jelölőnégyzetet.
    
    ![Közli a HTTP Post képernyő](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    A hello **URL-cím** mezőbe írja be, ha azt szeretné, hogy HTTP üdvözlőüzenetére közzétett hello URL-CÍMÉT. A hello **fejlécek** szövegmezőhöz hello HTTP-fejlécek toobe hello kérelemben küldött adja meg.  A **törzs** szabhatja testre a hello segítségével üdvözlőüzenetére **elérhető kulcsszavak** lista toodynamically beszúrása riasztási adatokat hello üzenet elküldésekor.  **Fejlécek** és **törzs** alapértelmezett tooan XML hasznos hasonló toohello alábbi példában.
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > Hello HTTP Post értesítési hello kattintva tesztelheti **ellenőrző értesítés** gomb, de csak hello riasztás konfigurációjának mentése után.
    > 
    > 
13. Kattintson a hello **mentése** toosave gombra a riasztás konfigurálásában.  Ha bejelölte **riasztás engedélyezve** 5. lépésben, a riasztás mostantól aktív.

## <a name="next-steps"></a>Következő lépések
* Elemezze [Azure CDN valós idejű statisztikák](cdn-real-time-stats.md)
* Mélyebb a Dig [speciális HTTP-jelentések](cdn-advanced-http-reports.md)
* Elemezze [használati minták](cdn-analyze-usage-patterns.md)

