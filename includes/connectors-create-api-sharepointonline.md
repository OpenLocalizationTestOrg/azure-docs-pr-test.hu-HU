

A sorrend tooconnect túl**SharePoint Online**, tooprovide az identity (felhasználónév és jelszó, intelligens kártya hitelesítő adatainak stb.) tooSharePoint Online van szüksége. Után, hogy hitelesített, végrehajtható a logic App toouse hello SharePoint Online Connectort. 

A hello tervezője a Logic Apps alkalmazást, kövesse ezeket a lépéseket toosign a SharePoint toocreate hello **kapcsolat** használható a Logic Apps alkalmazást:

1. Adja meg a SharePoint hello keresőmezőbe, majd várja meg a hello keresési tooreturn összes eseményindítók és műveletek kapcsolódó tooSharePoint Online:   
   ![A SharePoint konfigurálása][1]  
2. Jelölje be hello **SharePoint online-hoz - fájl jön létre** eseményindító  
3. Válassza ki **tooSharePoint Online bejelentkezés**:   
   ![A SharePoint konfigurálása][2]    
4. Adja meg a SharePoint hitelesítő adatok toosign tooauthenticate a SharePoint   
   ![A SharePoint konfigurálása][3]     
5. Hello hitelesítés befejezése után lesz átirányított tooyour logikai alkalmazást. Ennyi az egész, hello kapcsolat létrejött. Figyelje meg, amely azt jelzi, hogy Ön most már csatlakoztatott tooSharePoint hello alján üdvözlőüzenetére.  
   ![A SharePoint konfigurálása][4]  
6. Más eseményindítók és műveletek toocomplete a Logic Apps alkalmazást újra kell majd is hozzáadhat.   

[1]: ./media/connectors-create-api-sharepointonline/connectionconfig1.png
[2]: ./media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ./media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ./media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ./media/connectors-create-api-sharepointonline/connectionconfig5.png
