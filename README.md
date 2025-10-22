# AREP.
<img width="670" height="786" alt="image" src="https://github.com/user-attachments/assets/ff7d5a5a-1d53-4caf-b821-17a955e58401" />

Diseñe un prototipo de sistema de microservicios que tenga un servicio (En la figura se representa con el nombre Math Services) para computar las funciones numéricas.  El servicio de las funciones numéricas debe estar desplegado en al menos dos instancias virtuales de EC2. Adicionalmente, debe implementar un service proxy que reciba las solicitudes de llamado desde los clientes  y se las delegue a las dos instancias del servicio numérico usando un algoritmo de activo-pasivo. Si uno de los servicios está caido debe dirigirla al otro.  El proxy deberá estar desplegado en otra máquina EC2. Asegúrese de poder configurar las direcciones y puertos de las instancias del servicio en el proxy usando variables de entorno del sistema operativo.  Finalmente, construya un cliente Web mínimo con un formulario que reciba el valor y de manera asíncrona invoke el servicio en el PROXY. Puede hacer un formulario para cada una de las funciones. El cliente debe ser escrito en HTML y JS.


Para conectar la llave:
ssh -i "tu-llave.pem" ec2-user@ip-publica-de-tu-instancia
ssh -i "tu-llave.pem" ec2-user@10.10.89.50
chmod 400 tu-llave.pem


Instalamos el nginx:
sudo yum install nginx -y
sudo systemctl start nginx

Archivo de configuracion: 
sudo nano /etc/nginx/nginx.conf

Aquí, puedes agregar la configuración para redirigir las solicitudes. Por ejemplo, si tu frontend está corriendo en el puerto 3000, el archivo de configuración se vería algo así:
```java
server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://localhost:3000;  # Redirige las solicitudes al frontend
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
reiniciar: sudo systemctl restart nginx

configurar fronted: sudo yum install nodejs -y  # Para Amazon Linux
Subir fronted:
git clone https://github.com/tu-repositorio/frontend.git
cd frontend
npm install
se corre: npm start

Fronted: 
```java
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Página Web Sencilla</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            text-align: center;
            padding: 50px;
        }

        h1 {
            color: #333;
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }

        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>

    <h1>Bienvenido a mi página web</h1>
    <button id="myButton">Haz clic aquí</button>
    <p id="message"></p>

    <script>
        document.getElementById('myButton').addEventListener('click', function() {
            document.getElementById('message').innerText = '¡Gracias por hacer clic!';
        });
    </script>

</body>
</html>
```

con node.js, server.js para el backend:
```java
// server.js
const express = require('express');
const path = require('path');
const app = express();

// Configura el puerto
const PORT = process.env.PORT || 3000;

// Sirve el archivo index.html
app.use(express.static(path.join(__dirname, 'public')));

app.listen(PORT, () => {
    console.log(`Servidor corriendo en http://localhost:${PORT}`);
});
```
npm init -y
npm install express

http://<ip-publica-de-tu-instancia>:3000.

lucas: 
```java
function lucas(n) {
    let a = 2, b = 1;
    if (n === 0) return a;
    if (n === 1) return b;
    for (let i = 2; i <= n; i++) {
        let temp = a + b;
        a = b;
        b = temp;
    }
    return b;
}
console.log(lucas(5)); // Ejemplo: L_5 = 11
```

secuencia lineal: 
```java
function secuenciaLineal(a0, d, n) {
    let secuencia = [];
    for (let i = 0; i < n; i++) {
        secuencia.push(a0 + i * d);
    }
    return secuencia;
}
console.log(secuenciaLineal(2, 3, 5)); // Ejemplo: [2, 5, 8, 11, 14]
```

primos: 
```java
function esPrimo(num) {
    if (num <= 1) return false;
    for (let i = 2; i <= Math.sqrt(num); i++) {
        if (num % i === 0) return false;
    }
    return true;
}

function primosHasta(n) {
    let primos = [];
    for (let i = 2; i <= n; i++) {
        if (esPrimo(i)) primos.push(i);
    }
    return primos;
}

console.log(primosHasta(10)); // Ejemplo: [2, 3, 5, 7]
```
no primos: 
```java
function noPrimosHasta(n) {
    let primos = primosHasta(n);  // Usamos la función de los primos
    let noPrimos = [];
    for (let i = 2; i <= n; i++) {
        if (!primos.includes(i)) noPrimos.push(i);
    }
    return noPrimos;
}

console.log(noPrimosHasta(10));  // Ejemplo: [4, 6, 8, 9, 10]
```
collatz: 
```java
function secuenciaCollatz(n) {
    let secuencia = [n];
    while (n !== 1) {
        if (n % 2 === 0) {
            n = n / 2;
        } else {
            n = 3 * n + 1;
        }
        secuencia.push(n);
    }
    return secuencia;
}
```

console.log(secuenciaCollatz(6));  // Ejemplo: [6, 3, 10, 5, 16, 8, 4, 2, 1]

si no es nginx: 
npm install http-proxy

```java
const http = require('http');
const httpProxy = require('http-proxy');

// Crear un servidor proxy
const proxy = httpProxy.createProxyServer({});

// Crear el servidor
const server = http.createServer((req, res) => {
    // Redirigir las solicitudes al backend (por ejemplo, tu app de frontend en el puerto 3000)
    proxy.web(req, res, { target: 'http://localhost:3000' });
});

// Configurar el puerto del servidor proxy
server.listen(80, () => {
    console.log('Servidor proxy inverso escuchando en el puerto 80');
});
```
<img width="707" height="524" alt="image" src="https://github.com/user-attachments/assets/51225487-b650-4924-b141-c94300e76afd" />
<img width="679" height="198" alt="image" src="https://github.com/user-attachments/assets/4f1a3e74-7f24-4b5a-8870-46e7f745112a" />
back
```java

const http = require('http');

// Crear el servidor backend
const server = http.createServer((req, res) => {
    // Configurar la respuesta según la URL solicitada
    if (req.url === '/') {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.end('<h1>¡Hola desde el backend!</h1>');
    } else if (req.url === '/api') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Este es un endpoint de la API' }));
    } else {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('Página no encontrada');
    }
});

// Configurar el puerto para el backend
const PORT = 3000;
server.listen(PORT, () => {
    console.log(`Servidor backend corriendo en http://localhost:${PORT}`);
});
```
Paso 2: Ejecutar el backend

Ejecuta el backend. Ve a la terminal de VS Code y ejecuta el archivo server.js con Node.js:

node server.js


Deberías ver en la terminal algo como esto:

Servidor backend corriendo en http://localhost:3000

Paso 3: Verificar que el backend está funcionando

Abre tu navegador y accede a http://localhost:3000/. Deberías ver el mensaje:

¡Hola desde el backend!


Si accedes a http://localhost:3000/api, verás un JSON con el siguiente contenido:

{
  "message": "Este es un endpoint de la API"
}

Paso 4: Conectar el proxy inverso con el backend

Ahora, si ya tienes el archivo proxy.js corriendo (como te expliqué antes), el proxy inverso redirigirá las solicitudes de localhost:80 a localhost:3000.

Asegúrate de que proxy.js esté en ejecución. Si no está, corre el siguiente comando en la terminal de VS Code:

node proxy.js


Accede a http://localhost/ en tu navegador.

Si todo está bien configurado, Nginx o el proxy inverso debería redirigir tu solicitud al backend que está corriendo en el puerto 3000, y deberías ver el mensaje "¡Hola desde el backend!".

También puedes probar http://localhost/api. El proxy redirigirá la solicitud a localhost:3000/api, y deberías ver el JSON:

{
  "message": "Este es un endpoint de la API"
}

1. Conectarte a tu Instancia EC2

Primero, asegúrate de que tu instancia EC2 está corriendo en AWS. Luego, conéctate a tu instancia usando SSH:

ssh -i "tu-llave.pem" ec2-user@ip-publica-de-tu-instancia


Asegúrate de usar la IP pública de tu instancia EC2, y la llave que descargaste al crearla.

2. Instalar Node.js en la Instancia EC2 (si no lo tienes)

Si no tienes Node.js instalado en la instancia EC2, puedes instalarlo de la siguiente manera:

Para Amazon Linux:
sudo yum update -y
sudo yum install -y nodejs npm

Para Ubuntu:
sudo apt update
sudo apt install -y nodejs npm

3. Subir los Archivos a tu Instancia EC2

Ahora, sube los archivos (server.js y proxy.js) a tu instancia EC2. Si los tienes en tu computadora local, puedes usar SCP para subirlos.

Ejemplo de cómo hacerlo:

scp -i "tu-llave.pem" server.js ec2-user@ip-publica-de-tu-instancia:/home/ec2-user
scp -i "tu-llave.pem" proxy.js ec2-user@ip-publica-de-tu-instancia:/home/ec2-user


Esto sube los archivos server.js y proxy.js al directorio /home/ec2-user de tu instancia EC2.

4. Instalar las Dependencias de Node.js en tu Instancia EC2

Accede a la instancia por SSH y navega al directorio donde subiste los archivos:

cd /home/ec2-user


Luego, instala la librería http-proxy para que el archivo proxy.js funcione correctamente:

npm init -y  # Esto creará un package.json si no tienes uno
npm install http-proxy

5. Ejecutar el Backend y Proxy

Inicia el backend (server.js):

node server.js


Esto iniciará el servidor backend en puerto 3000. Deberías ver el mensaje:

Servidor backend corriendo en http://localhost:3000


Inicia el servidor proxy (proxy.js):

En una nueva terminal (o en una nueva ventana de SSH), corre:

node proxy.js


Esto iniciará el servidor proxy en puerto 80. Deberías ver el mensaje:

Servidor proxy inverso escuchando en el puerto 80

Accede a la IP pública de tu instancia EC2 desde un navegador:

Para el proxy (puerto 80): Abre http://<ip-publica-de-tu-instancia>/ en tu navegador. Deberías ver el mensaje del backend como si estuvieras accediendo a localhost:3000.

Para el backend (puerto 3000): Si quieres probar directamente el backend, puedes acceder a http://<ip-publica-de-tu-instancia>:3000/ y ver el mensaje ¡Hola desde el backend!.


