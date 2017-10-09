<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Frissítések előkészítése
Vizsgálat és hello frissítés telepítése előtt az alábbi lépések tooperform hello lesz szüksége:

1. Pillanatkép készítése felhő hello eszközön tárolt adatok.
2. Győződjön meg arról, hogy a vezérlő rögzített IP-címei irányítható, és képes kapcsolódni az toohello Internet. Ezek rögzített IP-címei lesznek használt tooservice frissítések tooyour eszköz. Futtassa a következő parancsmag minden tartományvezérlőn hello Windows PowerShell felületén hello eszköz hello tesztelheti:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **A Test-Connection csatlakoztatásakor rögzített IP-címek is toohello Internet példa a kimenetre**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Miután ezek manuális előzetes ellenőrzése sikeresen befejeződött, tooscan folytatni, és hello frissítések telepítése.

