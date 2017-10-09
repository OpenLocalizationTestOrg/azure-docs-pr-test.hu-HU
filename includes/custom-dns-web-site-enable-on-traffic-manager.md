Miután hello rögzíti a tartománynévhez tartozó propagálása, akkor kell tudni toouse a böngésző tooverify, amely az egyéni tartománynevet is kell használt tooaccess webalkalmazását az Azure App Service.

> [!NOTE]
> A CNAME toopropagate keresztül hello DNS rendszer némi időt is igénybe vehet. A szolgáltatás használhatja például a <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> , amely hello CNAME tooverify érhető el.
> 
> 

Ha nem már hozzáadta a webes alkalmazás a Traffic Manager-végpontként, ezt névfeloldás működnek, mint hello egyéni tartomány nevét útvonalak tooTraffic Manager előtt kell megtennie. A TRAFFIC Manager ezután tooyour webalkalmazás küldi. A hello információk [telepítése és törlése végpontok](../articles/traffic-manager/traffic-manager-endpoints.md) tooadd a webalkalmazás a Traffic Manager-profil végpontjaként.

> [!NOTE]
> Ha a webalkalmazás nem szerepel a végpont hozzáadásakor, győződjön meg arról, hogy van-e konfigurálva a **szabványos** App Service-csomag mód. Használjon **szabványos** mód rendelés toowork a Traffic Managerrel a webalkalmazás számára.
> 
> 

1. A böngészőben nyissa meg a hello [Azure Portal](https://portal.azure.com).
2. A hello **Web Apps** fülre, kattintson a webalkalmazás, jelölje be a hello neve **beállítások**, majd válassza ki **egyéni tartományok**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. A hello **egyéni tartományok** panelen kattintson a **állomásnév hozzáadása**.
4. Használjon hello **állomásnév** szöveg mezőkbe tooenter hello Traffic Manager tartomány neve tooassociate webes alkalmazáshoz.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Kattintson a **ellenőrzése** toosave hello tartomány névbeállításainak.
6. Gomb megnyomásakor **ellenőrzése** Azure lesz indítsa tartományok ellenőrzésének munkafolyamat. Ez a tartomány tulajdonosa, valamint állomásnév rendelkezésre állás és a jelentés sikeres vagy az előírásoknak megfelelő guidence hogyan toofix hello hiba a hibával kapcsolatos részletes ellenőrzi.    
7. Sikeres érvényesítése után **állomásnév hozzáadása** gomb aktívvá válik, és nem fogja tudni toohello hozzárendelése állomásnév. Most lépjen tooyour egyéni tartománynév a böngészőben. Ekkor megjelenik az alkalmazás fut, az egyéni tartománynév használatával. 
   
   Konfiguráció befejezése után hello egyéni tartománynevet szereplő hello **tartománynevek** webalkalmazás szakasz.

Ezen a ponton képes tooenter hello Traffic Manager tartománynév neve legyen a böngésző és tekintse meg, hogy sikeresen vesz igénybe, tooyour webalkalmazás.

