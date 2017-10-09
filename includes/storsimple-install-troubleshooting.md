<!--author=alkohli last changed: 03/17/16-->

## <a name="troubleshooting-update-failures"></a>A frissítéssel kapcsolatos hibák elhárítása
**Mi történik, ha megjelenik egy értesítés, hogy a frissítés előtti ellenőrzések hello meghibásodott?**

Ha egyik előzetes ellenőrzése sikertelen, győződjön meg arról, hogy rendelkezik-e nézett alján hello hello hello részletes értesítési sáv. Ez nyújt útmutatást, toowhich előzetes ellenőrzése sikertelen volt. hello alábbi ábrán egy példányát, például egy értesítés jelenik meg. Ebben az esetben hello tartományvezérlő állapotának ellenőrzése és hardver összetevő állapot-ellenőrzése nem járt sikerrel. A hello **hardverállapot** szakaszban látható, amely mindkét **vezérlő 0** és **vezérlő 1** összetevők kezeléséről.

  ![Előzetes ellenőrzési hiba](./media/storsimple-install-troubleshooting/HCS_PreUpdateCheckFailed-include.png)

Arról, hogy, hogy mindkét tartományvezérlők kifogástalan és online toomake kell. Arról, hogy minden hello hardverösszetevőinek hello StorSimple eszköz látható toobe kifogástalan hello karbantartási oldalon toomake is szüksége lesz. Próbálja tooinstall frissítések. Ha nem tudja toofix hello hardverproblémák összetevő, akkor szüksége lesz a Microsoft Support toocontact további lépéseket.

**Mi történik, ha "Nem tudta telepíteni a frissítést" hibaüzenetet kap, és javaslatot hello az útmutató toodetermine hello hello hiba okának elhárítása toorefer toohello frissítés?**

Ennek egyik legvalószínűbb oka lehet, hogy nincs kapcsolat toohello Microsoft Update-kiszolgálókról. Ezzel a beállítással, amelyet a toobe végre manuális ellenőrzi. Ha megszakad a kapcsolat toohello frissítési kiszolgáló, a frissítési feladat sikertelen lesz. Hello kapcsolat hello hello Windows PowerShell felületet a StorSimple eszköz követően – a parancsmag futtatásával ellenőrizheti:

 `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter>`

Hello parancsmagot futtathatja mindkét tartományvezérlők.

Ha meggyőződött hello kapcsolat létezik, és továbbra is toosee a probléma, forduljon a Microsoft Support további lépéseket.

**Mi történik, ha az eszköz tooUpdate 4 frissítésekor frissítés hibát látja, és mindkét hello tartományvezérlők futnak Update 4?**

Update 4 indítása, ha mindkét hello tartományvezérlőkön futó hello azonos szoftverének verziójával, és ha egy frissítés sikertelen, hello, tartományvezérlői ne lépje helyreállítási módba. Ez a helyzet akkor merülhet fel, ha hello eszköz szoftver gyorsjavítás (1. rendelés update) egy alkalmazott tooboth hello tartományvezérlők sikeresen azonban más gyorsjavítások (rendelés 2. és 3. sorrendben) még toobe alkalmazva. Update 4-től kezdődően hello vezérlők kerül helyreállítási módba csak akkor, ha hello két vezérlő más szoftver-verziót futtat. 

Ha hello felhasználó frissítés hibát látja, ha mindkét tartományvezérlők frissítése 4 futnak, javasoljuk, hogy azok Várjon néhány percet, és ismételje meg a frissítése. Ha hello újrapróbálkozási nem sikerül, akkor kell forduljon a Microsoft Support.
