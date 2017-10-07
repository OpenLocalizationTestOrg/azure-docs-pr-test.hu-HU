---
title: az Azure App Service Web Apps Python aaaConfiguring
description: "Ez az oktatóanyag leírja a szerzői műveletekhez és az alapszintű webes kiszolgáló átjáró felület (WSGI) kompatibilis Python alkalmazást az Azure App Service Web Apps beállításait."
services: app-service
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: fd00dc91-9935-4331-b955-4bd71e66d518
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/26/2016
ms.author: huvalo
ms.openlocfilehash: 00d49fb01491e9adb4b6fededfb95669a8dbd485
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-python-with-azure-app-service-web-apps"></a>Python konfigurálása az Azure App Service Web Apps alkalmazások
Ez az oktatóanyag ismerteti, szerzői és egy alapszintű Web Server átjáró felület (WSGI) kompatibilis Python alkalmazást beállításának beállítások [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Git üzemelő példányt, például a virtuális környezet és a csomag telepítése a requirements.txt használatával további szolgáltatásait ismerteti.

## <a name="bottle-django-or-flask"></a>Bottle, a Django vagy Flask?
hello Azure piactér hello Bottle, a Django és a Flask keretrendszerek sablonjait tartalmazza. Ha az első webalkalmazás az Azure App Service fejleszt, vagy még nem ismeri a Gitet, javasoljuk, hogy ezek az oktatóanyagok, amely részletes utasításokat tartalmaz egy működő alkalmazást Git központi telepítéssel hello gyűjteményből felépítése egyik kövesse a Windows vagy Mac:

* [Webalkalmazások létrehozása a Bottle](web-sites-python-create-deploy-bottle-app.md)
* [Webalkalmazások létrehozása a djangóval](web-sites-python-create-deploy-django-app.md)
* [Webalkalmazások létrehozása a Flask](web-sites-python-create-deploy-flask-app.md)

## <a name="web-app-creation-on-azure-portal"></a>Webalkalmazás létrehozása az Azure portálon
Ez az oktatóanyag azt feltételezi, hogy egy meglévő Azure előfizetés és a hozzáférés toohello Azure portálon.

Ha még nem rendelkezik egy létező webalkalmazása, létrehozhat egyet a hello [Azure Portal](https://portal.azure.com).  Új gombjára hello hello bal felső sarokban, majd kattintson a **Web + mobil** > **webalkalmazás**.

## <a name="git-publishing"></a>Git-közzététel
Az újonnan létrehozott webalkalmazáshoz tartozó Git-közzététel konfigurálása: hello utasításokat követve [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md). Ez az oktatóanyag a Git toocreate használja, kezelése és a Python webes alkalmazás tooAzure App Service közzététele.

Git-közzététel beállítása után a Git-tárház létrehozása, és a webalkalmazás társított. hello tárház URL-cím fog megjelenni, és ezentúl hello helyi fejlesztési környezet toohello felhő használt toopush adatait. toopublish alkalmazások keresztül Git, ellenőrizze, hogy egy Git-ügyfél telepítése és használata hello arra vonatkozó útmutatást toopush a webes alkalmazás tartalom tooAzure App Service.

## <a name="application-overview"></a>Az alkalmazás áttekintése
A következő szakaszokban hello hello következő fájlok jönnek létre. Ezek hello hello Git-tárház gyökérkönyvtárában kell helyezni.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI kezelő
WSGI egy Python szabvány szerint [EGP 3333](http://www.python.org/dev/peps/pep-3333/) meghatározása hello webkiszolgáló és a Python közötti illesztőfelületet szolgáltasson. Egy egységes felületet biztosít írása a webalkalmazások és -keretrendszerek pythonos környezetekben. Python webes népszerű keretrendszerekre ma használható WSGI. Támogatja az ilyen keretrendszerek; az Azure App Service Web Apps által biztosított Ezenkívül advanced-felhasználók is létrehozható a saját mindaddig, amíg hello egyéni kezelő a következő hello WSGI specification irányelveket.

Íme egy példa egy `app.py` , amely meghatározza, hogy egy egyéni kezelő:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Az alkalmazást helyileg is futtathatja `python app.py`, majd keresse meg a túl`http://localhost:5555` a böngészőben.

## <a name="virtual-environment"></a>Virtuális környezet
Bár a fenti hello példa alkalmazás nincs szükség külső csomagok, valószínű, hogy az alkalmazás néhány szükséges.

toohelp kezeléséhez külső csomagfüggőségek, Azure Git-telepítés a virtuális környezetek hello létrehozását támogatja.

Ha Azure hello legfelső szintű hello tárház a Requirements.txt fájlt észlel, automatikusan létrehoz egy nevű virtuális környezet `env`. Ez csak akkor történik hello első üzemelő példányon, vagy hello után bármely központi telepítése során kiválasztott Python-futtatókörnyezet megváltozott.

Toocreate egy helyi fejlesztési virtuális környezetről, valószínűleg érdemes, de nem adja hozzá a Git-tárházban.

## <a name="package-management"></a>Csomagkezelés
A requirements.txt fájlban felsorolt csomagok automatikusan települnek a pip használatával hello virtuális környezetben. Ez történik, minden üzembe helyezés, de a pip kihagyja a telepítést, ha a csomag már telepítve van.

Példa `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python-verzió
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Példa `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config
Szüksége lesz egy web.config fájl toospecify toocreate hello kiszolgáló hogyan kezelje a kérelmeket.

Vegye figyelembe, hogy ha egy web.x.y.config fájlt a tárházban, ahol x megegyezik hello kijelölt Python-futtatókörnyezet, majd Azure automatikusan másolatot készít a hello megfelelő web.config fájlt.

hello következő web.config példák használja a virtuális környezet proxyparancsfájl, hello a következő szakaszban ismertetett.  Ezek a működnek hello WSGI kezelő hello példában használt `app.py` felett.

Példa `web.config` Python 2.7-hez:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Példa `web.config` Python 3.4 esetén:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statikus fájlok végzi hello webkiszolgáló közvetlenül, Python kódját, javítja a teljesítményt áthaladás nélkül.

A fenti példák hello hello statikus fájlok lemezen hello helyének meg kell felelnie az hello hely hello URL-címben. Ez azt jelenti, hogy kérelmet `http://pythonapp.azurewebsites.net/static/site.css` szolgálja ki hello fájl a lemezen `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`van, amelyben meg kell határoznia hello WSGI kezelő. A fenti példákban hello rendelkezik `app.wsgi_app` mert hello kezelő nevű függvény `wsgi_app` a `app.py` hello gyökérmappában.

`PYTHONPATH`testre szabható, de ha a requirements.txt fájlban megadásával hello virtuális környezetben telepíti a függőségek, nem szabad toochange azt.

## <a name="virtual-environment-proxy"></a>Virtuális környezet Proxy
a következő parancsfájl hello használt tooretrieve hello WSGI kezelő, illetve aktiválja az hello virtuális környezet és a naplófájlok hibák. Tervezett toobe általános és módosítások nélkül használható legyen.

Tartalmát `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject tooterms and conditions of hello Apache License, Version 2.0. A 
     # copy of hello license can be found in hello License.html file at hello root of this distribution. If 
     # you cannot locate hello Apache License, Version 2.0, please send an email too
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing toobe bound 
     # by hello terms of hello Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors tooa log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n')

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')

        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)

        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()

        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))

        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []

        site.main()

        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Git-telepítés testreszabása
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]

## <a name="troubleshooting---package-installation"></a>Hibaelhárítás – Csomagok telepítése
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Hibaelhárítás – Virtuális környezet
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [Python fejlesztői központ](/develop/python/).

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

