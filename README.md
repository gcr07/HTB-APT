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































