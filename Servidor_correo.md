# Servidor de correo electrónico
Los mensajes no se mandan a un orcenador, sino a un conjunto de ordenadores. MUA: es el cliente, utiiza un programa de escritorio o una aplicación web. Se envía a un MTA, servidor de correo. PAra que un MUA envíe correos se necesita un MTA y se envía a través del protocolo SMTP, de nivel de aplicación, en el puerto 25 de TCP. 

El servidor tiene que tener una dirección, por ejmplo smtp.example1.com. El servidor tiene que enviarlo al otro MTA. Para enviar el correo de servidor a servidor también se utiliza el protocolo smtp. Para saber dónde enviar el correo se utiliza el registro MX de la configuración DNS. 

Los servidores de correo no suelen servir para recibir correos, cuando los recibe se los da a MDA, el servidor de correo entrante, que los guarda en un buzón del usuario correspondiente. Los nombres de este servidor se le suele llamar pop.example1.com o imap.example1.com. Para leer el correo del cliente al MDA hay dos protocolos:
- Protocolo **POP** (110/TCP). El más antiguo. Se accede al servidor y se descargan los mesajes. Normalmente se borran los mensajes del buzón pero se puede configurar para que estos no se borren del buzón. Ya no se utiliza.
- Protocolo **IMAP** (143/TCP). Se sincronizan los correos. 

Si se quiere enviar un correo hay que configurar el servidor del correo saliente en el MUA. 

En ocasiones los correos no se mandan de un servidor a otro, sino que se encuentra servidores intermedios, que reciben el correo y lo reenvía cuando no es para sí mismo. 

Para el servidor MTA se utiliza la aplicación **POTFIX** y para MDA se utiliza **clovecot**. 

Por defecto, la conexión entre el MUA y el MTA no es ni cifrada ni autentificada. Pero para recibir el correo, del MDA al MUA, es autentificada, pero no cifrada. Todo esto por defecto, pero se puede configurar para que no sea así. Se van a definir una serie de características para que al servidor de correo no se pueda conectar cualquiera. 

En MTA está el buzón. **mbox** es un fichero donde se guardan todos los ficheros. **maildir** es un direcotrio ocn todos los correos. 

### Instalar el servidor de correo
Para instalar **postfix**:
~~~
debian@croqueta:~$ sudo apt install postfix
~~~

Se selecciona el tipo de configuración Internet Site:
~~~
 ┌──────────────────────┤ Postfix Configuration ├──────────────────────┐
 │ Please select the mail server configuration type that best meets    │ 
 │ your needs.                                                         │ 
 │                                                                     │ 
 │  No configuration:                                                  │ 
 │   Should be chosen to leave the current configuration unchanged.    │ 
 │  Internet site:                                                     │ 
 │   Mail is sent and received directly using SMTP.                    │ 
 │  Internet with smarthost:                                           │ 
 │   Mail is received directly using SMTP or by running a utility      │ 
 │ such                                                                │ 
 │   as fetchmail. Outgoing mail is sent using a smarthost.            │ 
 │  Satellite system:                                                  │ 
 │   All mail is sent to another machine, called a 'smarthost', for    │ 
 │ delivery.                                                           │ 
 │  Local only:                                                        │ 
 │   The only delivered mail is the mail for local users. There is no  │ 
 │ network.                                                            │ 
 │                                                                     │ 
 │ General type of mail configuration:                                 │ 
 │                                                                     │ 
 │                      No configuration                               │ 
 │                      Internet Site                                  │ 
 │                      Internet with smarthost                        │ 
 │                      Satellite system                               │ 
 │                      Local only                                     │ 
 │                                                                     │ 
 │                                                                     │ 
 │                  <Ok>                      <Cancel>                 │ 
 │                                                                     │ 
 └─────────────────────────────────────────────────────────────────────┘ 
  ┌──────────────────────┤ Postfix Configuration ├──────────────────────┐
 │ The "mail name" is the domain name used to "qualify" _ALL_ mail     │ 
 │ addresses without a domain name. This includes mail to and from     │ 
 │ <root>: please do not make your machine send out mail from          │ 
 │ root@example.org unless root@example.org has told you to.           │ 
 │                                                                     │ 
 │ This name will also be used by other programs. It should be the     │ 
 │ single, fully qualified domain name (FQDN).                         │ 
 │                                                                     │ 
 │ Thus, if a mail address on the local host is foo@example.org, the   │ 
 │ correct value for this option would be example.org.                 │ 
 │                                                                     │ 
 │ System mail name:                                                   │ 
 │                                                                     │ 
 │ croqueta.paloma.gonzalonazareno.org________________________________ │ 
 │                                                                     │ 
 │                  <Ok>                      <Cancel>                 │ 
 │                                                                     │ 
 └─────────────────────────────────────────────────────────────────────┘ 
~~~

El nombre del correo está en **/etc/mailname** y los ficheros de configuración de postfix se encuentran en **/etc/postfix**.

### Creación del cliente de correo
**mail** es una herramienta muy sencilla que desde donde se pueden enviar correos con ciertos atributos:
- **mail**: correo al que enviar.
- **cc**: correo donde va a llegar una copia del correo.
- **subject**
Se finaliza con CONTROL+D.

Paquete para usar mail:
~~~
debian@croqueta:/etc/postfix$ sudo apt install mailutils
~~~

Enviar correo dentro del servidor:
~~~
debian@croqueta:/etc/postfix$ mail root@croqueta.paloma.gonzalonzareno.orgCc: 
Subject: prueba
esto es una prueba dentro del servidor
~~~

Ver el correo:
~~~
root@croqueta:/etc/postfix# mail
"/var/mail/root": 1 message 1 new
>N   1 Debian             Tue Jan 28 07:53  13/616   prueba
? 1
Return-Path: <debian@croqueta.paloma.gonzalonazareno.org>
X-Original-To: root@croqueta.paloma.gonzalonazareno.org
Delivered-To: root@croqueta.paloma.gonzalonazareno.org
Received: by croqueta.paloma.gonzalonazareno.org (Postfix, from userid 1000)
	id 137DD20E09; Tue, 28 Jan 2020 07:53:07 +0000 (UTC)
To: <root@croqueta.paloma.gonzalonazareno.org>
Subject: prueba
X-Mailer: mail (GNU Mailutils 3.5)
Message-Id: <20200128075307.137DD20E09@croqueta.paloma.gonzalonazareno.org>
Date: Tue, 28 Jan 2020 07:53:07 +0000 (UTC)
From: Debian <debian@croqueta.paloma.gonzalonazareno.org>

esto es una prueba dentro del servidor
~~~

Enviar correo fuera del servidor:
~~~
debian@croqueta:/etc/postfix$ mail root@croqueta.paloma.gonzalonazareno.org
Cc: 
Subject: prueba
esto es una prueba dentro del servidor
You have new mail in /var/mail/debian
~~~

Enviar correo estableciendo la conexión manualmente, con el protocolo ESMTP por el puerto 25:
~~~
debian@croqueta:/etc/postfix$ telnet localhost 25
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
220 croqueta.paloma.gonzalonazareno.org ESMTP Postfix (Debian/GNU)
HELO paloma.gonzalonzareno.org         
250 croqueta.paloma.gonzalonazareno.org
mail from:debian@croqueta.paloma.gonzalonazareno.org
250 2.1.0 Ok
rcpt to:debian@croqueta.amorales.gonzalonazareno.org
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
from: debian@croqueta.paloma.gonzalonazareno.org
to: debian@croqueta.amorales.gonzalonzareno.org
subject: prueba

hola, esto es una prueba

.
250 2.0.0 Ok: queued as 9571320E0C
~~~

### Enviar correo
Se recibe un correo al MUA y se tiene una serie de trestricciones:
- **Client**: restricciones que tiene que ver con los que se conecten (solo se conectan los que estan en el rango 192.168.100.0-192.168.100.50, por ejemplo).
- **Sendor**: mail from.
- **Recipent**: RCPT to.
- **Relay**: tiene que ver con el reenvío. Es la única que aparece por defecto.


### Practiquita
1. Documenta en redmine una prueba de funcionamiento, donde envíes desde tu servidor local al exterior. Muestra el log donde se vea el envío. Muestra el correo que has recibido.


Tarea 2 (1 puntos)(Obligatorio): Documenta en redmine una prueba de funcionamiento, donde envíes un correo desde el exterior (gmail, hotmail,…) a tu servidor local. Muestra el log donde se vea el envío. Muestra cómo has leído el correo.




