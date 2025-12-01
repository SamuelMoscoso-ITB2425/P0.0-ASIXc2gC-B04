🚀 Despliegue de Servidor Web LAMP con Visualizador de Datos B_N04
==================================================================

Este proyecto documenta el proceso paso a paso para desplegar un **servidor web (Apache + PHP)** conectado a una **base de datos MySQL** local. El objetivo es visualizar datos importados desde un CSV (OpenData Barcelona) en una interfaz web moderna con **modo oscuro** y **filtrado en tiempo real**.

![Imatge de LAMP stack architecture showing Linux, Apache, MySQL, and PHP components working together](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcTq9dE9HBPMahxS8Dw7N7G45UDjUCn7idrCdo4fp_KJ0Z_u-2-D6IC-Kx9UebGK0Ivx8IirUD0ji9qKN_4PxWnTvT_Osj-sMyVhJLlECESBk0jFF10)

Shutterstock

Explora

* * * * *

📋 Requisitos Previos
---------------------

-   **Ubuntu Server 22.04 LTS**.

-   Acceso a internet (para instalación de paquetes) o repositorios locales.

-   Usuario con privilegios `sudo`.

* * * * *

1️⃣ Instalación del Stack LAMP
------------------------------

Primero, actualizamos el sistema e instalamos el servidor web **Apache**, el lenguaje **PHP** y los módulos necesarios para la conexión con **MySQL**.

Bash

```
# 1. Actualizar repositorios
sudo apt update

# 2. Instalar Apache, PHP y librerías de conexión
sudo apt install apache2 php libapache2-mod-php php-mysql -y

# 3. Verificar estado del servicio
sudo systemctl status apache2

```

* * * * *

2️⃣ Configuración de Permisos en MySQL
--------------------------------------

Otorgamos permisos explícitos al usuario `bchecker` para conectarse desde `localhost`.

Bash

```
# Acceder a MySQL como root
sudo mysql -u root

```

Dentro de la consola de MySQL ejecutamos:

SQL

```
-- Crear usuario local si no existe
CREATE USER IF NOT EXISTS 'bchecker'@'localhost' IDENTIFIED BY 'bchecker121';

-- Otorgar todos los permisos sobre la base de datos B_N04
GRANT ALL PRIVILEGES ON B_N04.* TO 'bchecker'@'localhost';

-- Aplicar cambios y salir
FLUSH PRIVILEGES;
EXIT;

```

* * * * *

3️⃣ Configuración de Apache y Firewall
--------------------------------------

### 3.1 Abrir puertos en el Firewall (UFW)

Bash

```
sudo ufw allow 80/tcp
sudo ufw reload

```

### 3.2 Asegurar escucha en todas las interfaces

Editamos la configuración de puertos para forzar la escucha en **IPv4** (`0.0.0.0`).

Bash

```
sudo nano /etc/apache2/ports.conf

```

Cambiamos la línea `Listen 80` por:

Plaintext

```
Listen 0.0.0.0:80

```

Reiniciamos el servicio para aplicar cambios:

Bash

```
sudo systemctl restart apache2

```

* * * * *

4️⃣ Despliegue de la Aplicación Web
-----------------------------------

### 4.1 Limpiar directorio web

Bash

```
sudo rm /var/www/html/index.html

```

### 4.2 Crear el archivo `index.php`

Bash

```
sudo nano /var/www/html/index.php

```

Contenido del archivo (`index.php`):

PHP

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Equipamientos B_N04</title>
    <style>
        /* --- ESTILOS OSCUROS (DARK MODE) --- */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #121212;
            color: #e0e0e0;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 98%;
            margin: 0 auto;
            background-color: #1e1e1e;
            padding: 20px;
            border-radius: 8px;
            border: 1px solid #333;
            box-shadow: 0 4px 15px rgba(0,0,0,0.7);
        }
        h1 {
            text-align: center;
            color: #4db8ff;
            margin-bottom: 20px;
            text-transform: uppercase;
            letter-spacing: 1.5px;
        }
                /* --- BARRA DE BÚSQUEDA PROPIA --- */
        .search-box {
            margin-bottom: 20px;
            text-align: right;
        }
        #miBuscador {
            background-color: #333;
            border: 1px solid #555;
            color: #fff;
            padding: 10px;
            width: 300px;
            border-radius: 5px;
            font-size: 16px;
        }
        #miBuscador::placeholder { color: #aaa; }
                /* --- TABLA --- */
        .table-wrapper {
            overflow-x: auto; /* Scroll horizontal */
            border: 1px solid #333;
            border-radius: 4px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 14px;
            background-color: #1e1e1e;
        }
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #333;
            white-space: nowrap; /* Evita saltos de línea feos */
        }
        th {
            background-color: #005f99;
            color: white;
            position: sticky;
            top: 0;
            text-transform: uppercase;
            font-size: 12px;
            border-bottom: 2px solid #004080;
        }
        tr:nth-child(even) { background-color: #252525; }
        tr:hover { background-color: #333; }
                /* Mensaje de conteo */
        .status-info {
            color: #00ff00;
            font-weight: bold;
            text-align: center;
            margin-bottom: 15px;
            padding: 8px;
            background-color: #1a3300;
            border-radius: 4px;
            border: 1px solid #2b5400;
        }
    </style>
</head>
<body>
<div class="container">
    <h1>Listado Completo de Equipamientos</h1>
    <?php
    // CONEXIÓN
    $conn = new mysqli("localhost", "bchecker", "bchecker121", "B_N04");
    $conn->set_charset("utf8");
        if ($conn->connect_error) {
        die("<h3 style='color:red;text-align:center'>Error Conexión: " . $conn->connect_error . "</h3>");
    }
    // CONSULTA
    $sql = "SELECT * FROM equipaments";
    $result = $conn->query($sql);
    if ($result && $result->num_rows > 0) {
        echo "<div class='status-info'>Estado: Conectado | Total Registros: <span id='totalReg'>" . $result->num_rows . "</span></div>";
                // --- INPUT BUSCADOR ---
        echo "<div class='search-box'>";
        echo "<input type='text' id='miBuscador' onkeyup='filtrarTabla()' placeholder='🔍 Buscar en cualquier columna...'>";
        echo "</div>";
        echo "<div class='table-wrapper'>";
        echo "<table id='miTabla'>";
                // CABECERA
        $fields = $result->fetch_fields();
        echo "<thead><tr>";
        foreach ($fields as $field) {
            echo "<th>" . htmlspecialchars($field->name) . "</th>";
        }
        echo "</tr></thead>";
                // CUERPO
        echo "<tbody>";
        while($row = $result->fetch_assoc()) {
            echo "<tr>";
            foreach ($row as $data) {
                $val = ($data === NULL || $data === '') ? '-' : htmlspecialchars($data);
                // Cortar si es muy largo (opcional)
                if(strlen($val) > 50) $val = substr($val, 0, 50) . "...";
                echo "<td>" . $val . "</td>";
            }
            echo "</tr>";
        }
        echo "</tbody>";
        echo "</table>";
        echo "</div>"; // Fin wrapper
    } else {
        echo "<h3 style='color:orange;text-align:center'>Tabla vacía o error SQL.</h3>";
    }
    $conn->close();
    ?>
</div>

<script>
function filtrarTabla() {
    // Variables
    var input, filter, table, tr, td, i, j, txtValue;
    input = document.getElementById("miBuscador");
    filter = input.value.toUpperCase();
    table = document.getElementById("miTabla");
    tr = table.getElementsByTagName("tr");
    // Recorrer filas (empezando en 1 para saltar cabecera)
    for (i = 1; i < tr.length; i++) {
        var filaVisible = false;
        // Recorrer todas las celdas de la fila
        td = tr[i].getElementsByTagName("td");
        for (j = 0; j < td.length; j++) {
            if (td[j]) {
                txtValue = td[j].textContent || td[j].innerText;
                if (txtValue.toUpperCase().indexOf(filter) > -1) {
                    filaVisible = true;
                    break; // Si encuentra coincidencia en una celda, muestra la fila y pasa a la siguiente
                }
            }
        }
        // Mostrar u ocultar fila
        tr[i].style.display = filaVisible ? "" : "none";
    }
}
</script>
</body>
</html>

```

### 4.3 Asignar permisos

Aseguramos que el usuario de Apache (`www-data`) pueda leer y ejecutar los archivos.

Bash

```
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html

```

* * * * *

5️⃣ Verificación
----------------

Accedemos desde el navegador de la máquina cliente usando la **IP del servidor**:

Plaintext

```
http://<IP-DEL-SERVIDOR>

```

(Ejemplo: `http://192.168.50.2`)

### 📝 Características implementadas:

-   ✅ **Conexión PHP-MySQL** verificada.

-   ✅ Visualización de tabla completa con scroll horizontal.

-   ✅ Estilo "**Dark Mode**" profesional.

-   ✅ Buscador en **tiempo real** (JavaScript puro).
