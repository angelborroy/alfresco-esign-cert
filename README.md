esign-cert
==================
Provides an Alfresco Share action for signing PDF files (PAdES-BES format) and any other file (CAdES-BES format detached) via java applet (@firma miniApplet, opensource at https://github.com/ctt-gob-es/clienteafirma) or local application AutoFirma by protocol (http://forja-ctt.administracionelectronica.gob.es/web/clienteafirma) where applets are not possible (i. e. Google Chrome).

**AutoFirma** local application is currently supported only for Windows, but Mac OS and Linux versions are on the roadmap. This Windows application shall be installed before using the addon.

Currently following browser and OS combinations are supported:

Windows
* IE Edge: not supported
* IE Classic: Local application
* Google Chrome: Local application
* Mozilla Firefox: Local application / Applet

Mac OS 
* Google Chrome: Local application (not yet available)
* Mozilla Firefox: Applet
* Apple Safari: Applet

Linux Ubuntu
* Mozilla Firefox: Applet

**Notice**: this module superseed previous one [alfresco-firma-pdf](https://github.com/keensoft/alfresco-firma-pdf)

This module uses a software digital certificate or a cryptographic hardware supported by a smart card.

**License**
The plugin is licensed under the [LGPL v3.0](http://www.gnu.org/licenses/lgpl-3.0.html). 

**State**
Current addon release 1.0-SNAPSHOT is ***BETA***

**Compatibility**
The current version has been developed using Alfresco 5.1 and Alfresco SDK 2.1.1, although it should run in Alfresco 5.0.d and Alfresco 5.0.c

Browser compatibility: 100% supported (refer previous paragraph)

**Languages**
Currently provided in English and Spanish.

***No original Alfresco resources have been overwritten***

Downloading the ready-to-deploy-plugin
--------------------------------------
The binary distribution is made of two amp files:

* [repo AMP](https://github.com/keensoft/alfresco-esign-cert/releases/download/1.0-SNAPSHOT/esign-cert-repo.amp)
* [share AMP](https://github.com/keensoft/alfresco-esign-cert/releases/download/1.0-SNAPSHOT/esign-cert-share-0.9.0.amp)

You can install them by using standard [Alfresco deployment tools](http://docs.alfresco.com/community/tasks/dev-extensions-tutorials-simple-module-install-amp.html)

Building the artifacts
----------------------
If you are new to Alfresco and the Alfresco Maven SDK, you should start by reading [Jeff Potts' tutorial on the subject](http://ecmarchitect.com/alfresco-developer-series-tutorials/maven-sdk/tutorial/tutorial.html).

You can build the artifacts from source code using maven
```$ mvn clean package```

Signing the applet
------------------
You can download plain applet from http://forja-ctt.administracionelectronica.gob.es/web/clienteafirma

Oracle [jarsigner](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/jarsigner.html) can be used to perform a signature on [miniapplet-full_1_4.jar](https://github.com/keensoft/alfresco-firma-pdf/blob/master/src/main/amp/web/sign/miniapplet-full_1_4.jar). To deploy this change, just replace current JAR for your signed JAR and rebuild the artifacts.

Running under SSL
-----------------
Signature window is built on an IFRAME, so when running Alfresco under SSL, following JavaScript console error may appear:

```Refused to display 'https://alfresco.keensoft.es/share/sign/sign-frame.jsp?mimeType=pdf' in a frame because it set 'X-Frame-Options' to 'DENY'.```

If so, check your web server configuration in order to set appropiate **X-Frame-Options** header value.

For instance, Apache HTTP default configuration for SSL includes...

```Header always set X-Frame-Options DENY```

... and it should be set to **SAMEORIGIN** instead

```Header always set X-Frame-Options SAMEORIGIN```

Configuration
----------------------
Before installation, following properties must be included in **alfresco-global.properties**

```
# Native @firma parameters separated by tab (\t)
esign.cert.params.pades=signaturePage=1\tsignaturePositionOnPageLowerLeftX=120\tsignaturePositionOnPageLowerLeftY=50\tsignaturePositionOnPageUpperRightX=220\tsignaturePositionOnPageUpperRightY=150\t
esign.cert.params.cades=mode=explicit
# Signature algorithm: SHA1withRSA, SHA256withRSA, SHA384withRSA, SHA512withRSA
esign.cert.signature.alg=SHA512withRSA

```
Usage
----------------------
Every document is including a **Sign** action to perform a client signature depending on the mime type:
* PDF files are signed as PAdES (with a visible signature)
* Other files are signed as CAdES (detached)

Both documents include also signer metadata:
```
Format: CAdES-BES Detached
Date: Wed 2 Mar 2016 22:31:32
Signer: CN=NOMBRE BORROY LOPEZ ANGEL FERNANDO - NIF 25162750Z, OU=500050546, OU=FNMT Clase 2 CA, O=FNMT, C=ES
Serial number: 1022640006
Caducity: Tue 12 Apr 2016
Issuer: OU=FNMT Clase 2 CA, O=FNMT, C=ES
```