# AREP.

Para conectar la llave:
ssh -i "tu-llave.pem" ec2-user@ip-publica-de-tu-instancia

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



