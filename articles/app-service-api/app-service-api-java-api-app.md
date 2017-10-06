---
title: "aaaBuild és az Azure App Service egy Java API-alkalmazás telepítése"
description: "Megtudhatja, hogyan toocreate egy Java API-alkalmazás csomagot, majd központilag telepíti az App Service tooAzure."
services: app-service\api
documentationcenter: java
author: rmcmurray
manager: erikre
editor: tdykstra
ms.assetid: 8d21ba5f-fc57-4269-bc8f-2fcab936ec22
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: get-started-article
ms.date: 04/25/2017
ms.author: rachelap;robmcm
ms.openlocfilehash: a4056fec870b1c4bed8ee14bb0e748b3ee89b9e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Java API-alkalmazás buildjének elkészítése és telepítése az Azure App Service platformon
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Ez az oktatóanyag bemutatja, hogyan toocreate egy Java-alkalmazást, és telepítse azt tooAzure App Service API Apps segítségével [Git]. hello az oktatóanyag utasításai követhetők bármely operációs rendszeren, amely képes Java. Ebben az oktatóanyagban hello kód-t [Maven]. [Jax-RS] használt toocreate hello RESTful szolgáltatás, és jön létre hello alapján [Swagger] hello segítségével metaadat-specifikáció [Swagger Editor].

## <a name="prerequisites"></a>Előfeltételek
1. [Java Developer's Kit 8]\(vagy újabb)
2. A fejlesztői gépen telepítve van a [Maven]
3. A fejlesztő gépen telepítve van a [Git]
4. Fizetett vagy [ingyenes próbaverzió] előfizetés túl[Microsoft Azure]
5. HTTP-tesztalkalmazás, például [Postman]

## <a name="scaffold-hello-api-using-swaggerio"></a>Scaffold hello API Swagger.IO használatával
Hello swagger.io online szerkesztő segítségével adhatja meg az API struktúráját hello képviselő Swagger JSON- vagy YAM-kóddal. Miután hello API felület tervezték, exportálhatja a különböző platformokon és keretrendszerek kódját. A következő szakaszban hello hello generált kód lesz módosított tooinclude a funkciók utánzatait. 

Ebben a bemutatóban kezdődik egy Swagger JSON-törzsére, amely akkor lesz illessze be hello swagger.io szerkesztő, amely majd használt toogenerate kód végez egy REST API-végpont JAX-RS tooaccess használatát. Ezt követően kell szerkeszteni hello generált kód tooreturn próbaadatokat, szimulálva egy adatperzisztencia-mechanizmus REST API.  

1. A következő Swagger JSON-kód tooyour vágólapra másolás hello:
   
        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }
2. Keresse meg a toohello [Online Swagger Editor]. Ezután kattintson hello **fájl -> JSON beillesztése** menüpont.
   
    ![A JSON beillesztése menüpont][paste-json]
3. Illessze be a névjegyek lista API Swagger JSON korábban kimásolt hello. 
   
    ![JSON-kód beillesztése a Swaggerbe][pasted-swagger]
4. Nézet hello dokumentációs oldalakat és API-összefoglalót hello szerkesztőben megjelenítve. 
   
    ![A Swagger által generált dokumentumok megtekintése][view-swagger-generated-docs]
5. Jelölje be hello **kiszolgáló generálása -> JAX-RS** menü beállítás tooscaffold hello kiszolgálóoldali kód amelyet szerkeszteni fog újabb tooadd utánzatait végrehajtására. 
   
    ![A Kód generálása menüpont][generate-code-menu-item]
   
    Miután hello kód jön létre, lesz egy ZIP-fájl toodownload megadni. Ez a fájl hello Swagger kódgeneráló által generált hello kódot tartalmaz, és összes kapcsolódó fordítási parancsprogramot. Bontsa ki a hello teljes tooa könyvtárat a fejlesztő munkaállomás. 

## <a name="edit-hello-code-tooadd-api-implementation"></a>Hello kód tooadd API-implementáció szerkesztése
Ebben a szakaszban cseréli le hello Swagger által generált kód kiszolgálóoldali implementációját a saját kódjára. hello új kódot ad vissza egy ArrayList Contact entitásokat toohello hívó ügyfelet. 

1. Nyissa meg hello *Contact.java* modellfájl, amely hello *src/gen/java/io/swagger/model* mappa, használatával [Visual Studio Code] vagy kedvenc szövegszerkesztőjével. 
   
    ![A Contact modellfájl megnyitása][open-contact-model-file]
2. Adja hozzá a következő belül hello konstruktor hello **forduljon** osztály. 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. Nyissa meg hello *mappában* fájlt, amely hello *src/main/java/io/swagger/api/impl* mappa, használatával [Visual Studio Code]vagy kedvenc szövegszerkesztőjével.
   
    ![A Contact szolgáltatásimplementációs fájl megnyitása][open-contact-service-code-file]
4. Hello kód hello fájl felülírása az új kódot tooadd utánzatait megvalósítási toohello szolgáltatás kódot. 
   
        package io.swagger.api.impl;
   
        import io.swagger.api.*;
        
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
               
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;
   
        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
   
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
   
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
   
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
   
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }
5. Nyisson meg egy parancssort, és módosítsa a könyvtárat toohello az alkalmazás gyökérmappájában.
6. Hajtható végre a következő Maven parancs toobuild hello kód hello, és futtassa helyileg a Jetty alkalmazáskiszolgálóval hello segítségével. 
   
        mvn package jetty:run
7. Meg kell jelennie hello parancsablakot tükrözi, hogy Jetty elindította a kódot a 8080-as porton. 
   
    ![A Contact szolgáltatásimplementációs fájl megnyitása][run-jetty-war]
8. Használjon [Postman] toomake kérelem toohello "get all contacts" API-metódus: 8080/api/Contacts címen.
   
    ![Hello névjegyek API hívása][calling-contacts-api]
9. Használjon [Postman] toomake kérelem toohello "get specific contact" API-módszer helye: 8080/api/contacts/2.
   
    ![Hello névjegyek API hívása][calling-specific-contact-api]
10. Végezetül fordítsa le hello Java WAR (Web ARchive) fájlt a következő Maven-parancsot a konzolon hello végrehajtásával. 
    
         mvn package war:war
11. Miután összeállította hello WAR-fájlt, akkor bekerülnek hello **cél** mappát. Hello navigálni **cél** mappa, és nevezze át hello WAR-fájl túl**ROOT.war**. (Győződjön meg arról, hogy hello és nagybetűkre).
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. Végezetül parancsok követően az alkalmazás toocreate hello gyökérmappájában hello egy **telepítése** mappa toouse toodeploy hello WAR-fájl tooAzure. 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## <a name="publish-hello-output-tooazure-app-service"></a>Hello kimeneti tooAzure App Service közzététele
Ebben a szakaszban megtudhatja, hogyan toocreate hello segítségével új API-alkalmazást az Azure portál, adott API-alkalmazások előkészítése a Java-alkalmazások és hello telepítése újonnan létrehozott WAR fájlt az új API-alkalmazás az App Service toorun tooAzure. 

1. Hozzon létre egy új API-alkalmazás hello [Azure-portálon], hello kattintva **új -> Web + mobil -> API-alkalmazás** menüpont, írja be az adatokat, majd kattintson **létrehozása**.
   
    ![Új API-alkalmazás létrehozása][create-api-app]
2. Az API-alkalmazás létrehozása után, nyissa meg az alkalmazás **beállítások** panelen, majd kattintson a hello **Alkalmazásbeállítások** menüpont. Válassza ki hello hello rendelkezésre álló lehetőségeket, a legújabb Java-verziót, majd válassza ki a legújabb tomcat-verziót a hello hello **webes tároló** menüben, majd kattintson **mentése**.
   
    ![Java API-alkalmazás paneljének hello beállítása][set-up-java]
3. Kattintson a hello **üzembe helyezési hitelesítő adatok** menüpontra, és írja be egy felhasználónevet és jelszót fájlok tooyour API-alkalmazás-közzététel toouse kívánja. 
   
    ![Telepítési hitelesítő adatok beállítása][deployment-credentials]
4. Kattintson a hello **központi telepítés forrásának** menüpontra. Ezután kattintson hello **forrás választása** gombra, jelölje be hello **helyi Git-tárház** lehetőséget, majd kattintson a **OK**. Ezzel létrehoz egy Git-tárházat, amely az Azure-ban fut és az Ön API-alkalmazásához van társítva. Minden alkalommal, amikor véglegesíti kód toohello *fő* fiókirodai a Git-tárház, a kódot közzéteszi a környezet az élő futó API-alkalmazáspéldányban. 
   
    ![Új helyi Git-tárház beállítása][select-git-repo]
5. Hello új Git-tárház URL-cím tooyour vágólapra másolása. Mentse, mert hamarosan szüksége lesz rá. 
   
    ![Új Git-tárház beállítása az alkalmazáshoz][copy-git-repo-url]
6. Git push hello WAR fájl toohello online tárházba. toodo, hello navigálni **telepítése** mappa a korábban létrehozott, hogy könnyen véglegesíthesse hello kód fel az App Service-ben futó toohello tárházba. Egyszer Ön hello konzolablakban, és belépett hello mappáját hello webapps mappát, adja ki a Git parancsok toolaunch hello folyamatot követve hello és a telepítés megkezdéséhez. 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    Hello kiadása után **leküldéses** kérelem, meg kell adnia a hello hello telepítési hitelesítő adatokhoz korábban létrehozott jelszót. Miután megadta a hitelesítő adatait, meg kell jelennie, a portál megjelenítése, frissítés hello tették elérhetővé telepítésre.
7. Még egyszer használatakor Postman toohit hello újonnan telepített API-alkalmazás fusson az Azure App Service-ben, látni fogja, hogy hello viselkedése konzisztens, és most adja vissza kapcsolattartási adatokat várt módon, és egyszerű kód módosítások toohello Swagger.io használatával generált Java-kódot. 
   
    ![A Java Contacts REST API használata élőben az Azure-ban][postman-calling-azure-contacts]

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben egy Swagger JSON-fájlt, és néhány generált Java-kóddal indultunk el hello Swagger.io szerkesztő képes toostart volt. Ezekből egyszerű módosításokkal és a Git telepítési folyamatának eredményeként egy működő, Java nyelven írt API-alkalmazást kaptunk. hello tovább az oktatóanyag bemutatja, hogyan túl[JavaScript-ügyfelekkel a CORS segítségével API-alkalmazásokat felhasználni][App Service API CORS]. Későbbi részei hello adatsor megjelenítése hogyan tooimplement hitelesítéshez és engedélyezéshez.

erre a példára toobuild, akkor tudhat meg többet hello [Storage SDK for Java] toopersist hello JSON-blobok. Vagy használhat hello [Document DB Java SDK] toosave a kapcsolattartó adatok tooAzure Document DB rendszerbe. 

<a name="see-also"></a>

## <a name="see-also"></a>Lásd még:
További információk az Azure és a Java együttes használatáról lásd: [Azure Java-fejlesztőknek](/java/azure).

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure-portálon]: https://portal.azure.com/
[Document DB Java SDK]: ../documentdb/documentdb-java-application.md
[ingyenes próbaverzió]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Azure Java Developer Center]: /develop/java/
[Java Developer's Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger Editor]: http://editor2.swagger.io/
[Postman]: https://www.getpostman.com/
[Storage SDK for Java]:../storage/blobs/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger Editor]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
