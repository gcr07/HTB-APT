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

























