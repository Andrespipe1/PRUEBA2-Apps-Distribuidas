
---

## Instalación y Ejecución

1. **Clona el repositorio:**
   ```sh
   git clone https://github.com/tuusuario/tu-repo.git
   cd tu-repo
   ```

2. **Levanta los servicios:**
   ```sh
   docker-compose up --build
   ```

3. **Accede a las aplicaciones:**
   - Aplicación web: [http://localhost](http://localhost)
   - phpMyAdmin: [http://localhost:8080](http://localhost:8080)  
     Usuario: `root` | Contraseña: `root`

---

## Configuración de la Replicación Maestro-Maestro

1. **Crea el usuario de replicación en ambos servidores MySQL:**
   ```sql
   CREATE USER 'replicador'@'%' IDENTIFIED BY 'replica123';
   GRANT REPLICATION SLAVE ON *.* TO 'replicador'@'%';
   FLUSH PRIVILEGES;
   ```

2. **Obtén el estado del binlog en cada servidor:**
   ```sql
   SHOW MASTER STATUS;
   ```

3. **Configura cada servidor como esclavo del otro:**
   ```sql
   CHANGE MASTER TO
     MASTER_HOST='maestro',
     MASTER_USER='replicador',
     MASTER_PASSWORD='replica123',
     MASTER_LOG_FILE='mysql-bin.XXXXXX',
     MASTER_LOG_POS=YYYYY;
   START SLAVE;
   ```
   (Reemplaza los valores por los obtenidos en el paso anterior y repite el proceso en ambos servidores, apuntando al otro.)

4. **Verifica la replicación:**
   ```sql
   SHOW SLAVE STATUS;
   ```
   Ambos deben mostrar `Slave_IO_Running: Yes` y `Slave_SQL_Running: Yes`.

---

## Pruebas de Funcionamiento

- Inserta datos en una aplicación y verifica que se replican en ambas bases de datos.
- Accede varias veces a la web para comprobar el balanceo de carga entre los dos servidores Flask.

---

## Recomendaciones de Seguridad

- Cambia las contraseñas por valores seguros en producción.
- Restringe el acceso a phpMyAdmin.
- Considera el uso de redes privadas y certificados SSL.

---

## Autor

- **Tecnología Superior en Desarrollo de Software**
- Andres Tufiño
- Darwin Cachimil

---

## Licencia

Este proyecto es solo para fines educativos.