AFS comandos básicos
====================

Para obtener el token (o renovarlo):

    $ kinit usuario@IFIC.UV.ES     
    $ aklog     

El token caducará a las 24 horas desde su obtención.  

El directorio del usuario será:    
    
    /afs/ific.uv.es/users/inicial/usuario

Para ver el token:

    $ tokens
    
Para descartar el token:
 
    $ unlog
    
Para ver la quota. En el directorio del usuario:      
    
    $ fs -q 

---
Modificado: 2018-08-18
