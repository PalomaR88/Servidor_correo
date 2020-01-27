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


