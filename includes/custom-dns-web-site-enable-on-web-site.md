Miután a tartománynév a rekordjainak propagálása, azokat a webalkalmazás kell hozzárendelni. Az alábbi lépések segítségével engedélyezze a tartománynév, a webböngésző segítségével.

> [!NOTE]
> TXT rekordot a DNS-rendszerében propagálásához az előző lépésekben létrehozott némi időt is igénybe vehet. Mindaddig, amíg a TXT-rekord propagálása a tartomány neve nem lehet hozzáadni a webes alkalmazást. Ha egy A rekordot használ, nem adhat az A rekord tartománynév a webalkalmazáshoz mindaddig, amíg az előző lépésben létrehozott TXT-rekord propagálása.
> 
> A szolgáltatás használhatja például a <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> annak ellenőrzéséhez, hogy a TXT-rekord érhető el.
> 
> 

1. A böngészőben nyissa meg a [Azure Portal](https://portal.azure.com).
2. Az a **webalkalmazások** lapra, kattintson a webalkalmazás nevét, majd **egyéni tartományok**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. Az a **egyéni tartományok** panelen kattintson a **állomásnév hozzáadása**.
4. Használja a **állomásnév** szövegmezőbe írja be a tartomány társítása a webalkalmazás számára.
   
    ![](./media/custom-dns-web-site/add-custom-domain.png)
5. Kattintson a **érvényesítése**.
6. Gomb megnyomásakor **ellenőrzése** Azure lesz indítsa tartományok ellenőrzésének munkafolyamat. Ez ellenőrzi, hogy a tartomány tulajdonosa, valamint állomásnév rendelkezésre állás és a jelentés sikeres vagy az előírásoknak megfelelő guidence a hiba megoldásával kapcsolatos hiba részleteit.    

Ezen a ponton meg kell tudni adja meg az egyéni tartománynév a böngészőben, és tekintse meg, hogy sikeresen tart, a webes alkalmazást.

