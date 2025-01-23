# Trabajo Consulta 2: Conexión Base de Datos Relacional

## ¿Qué es JDBC?

JDBC (Java Database Connectivity) es una API de Java que permite a las aplicaciones comunicarse con bases de datos relacionales. JDBC proporciona un conjunto de interfaces y clases que permiten ejecutar consultas SQL, realizar actualizaciones y gestionar conexiones a bases de datos de manera estándar e independiente del proveedor de la base de datos.

### Componentes principales de JDBC

1. **Driver Manager**: Gestiona la carga y el uso de controladores de bases de datos (drivers) que permiten conectarse a diferentes tipos de bases de datos.
2. **Driver**: Proporciona la implementación específica para conectarse y comunicarse con una base de datos.
3. **Connection**: Representa la conexión entre la aplicación y la base de datos.
4. **Statement**: Permite ejecutar consultas SQL (puede ser Statement, PreparedStatement o CallableStatement).
5. **ResultSet**: Contiene los resultados de las consultas ejecutadas.
6. **SQLException**: Maneja los errores relacionados con la base de datos.

---

## Librerías de Scala para Conectarse a Bases de Datos Relacionales

| **Librería** | **Descripción** | **Diferencias clave** |
|--------------|-----------------|-----------------------|
| **Slick**    | Es una librería de Scala para trabajar con bases de datos relacionales usando programación funcional. | Ofrece un enfoque funcional y orientado a tipos para las consultas SQL. |
| **Doobie**   | Librería funcional de manejo de bases de datos que se integra bien con Cats Effect. | Más sencilla y flexible para integrarse con efectos asíncronos (Cats Effect), ideal para bases de datos pequeñas. |

---

## Documentar cómo Establecer una Conexión a una Base de Datos Relacional (MySQL)

### Pasos:

1. **Genere una base de datos en MySQL**:
    ```sql
    CREATE DATABASE peliculas_db;
    ```
<img width="718" alt="creardatabase" src="https://github.com/user-attachments/assets/5d10eb2e-1cab-4373-8084-f913c44ff8fd" />

2. **Genere una tabla con datos de prueba**:
    ```sql
    USE peliculas_db;

    CREATE TABLE peliculas (
        id_pelicula INT AUTO_INCREMENT PRIMARY KEY,
        genero VARCHAR(50),
        titulo VARCHAR(100)
    );

    INSERT INTO peliculas (genero, titulo) VALUES 
    ('Acción', 'Vingadores: Endgame'),
    ('Comedia', 'La gran aventura de los hermanos'),
    ('Drama', 'La vida de un héroe');
    ```
<img width="950" alt="creartabla" src="https://github.com/user-attachments/assets/f774c244-d3b5-4268-a144-cdce14a06f47" />

3. **Desde Scala, establezca la conexión a la base de datos**:

    ```scala
    libraryDependencies += "mysql" % "mysql-connector-java" % "8.0.26"
    ```
<img width="591" alt="image" src="https://github.com/user-attachments/assets/6c568f67-a1e8-48b2-b081-6e11f08bc901" />


    ```scala
    import java.sql.{Connection, DriverManager, ResultSet}

    object ConexionPeliculas {
      def main(args: Array[String]): Unit = {
        // Detalles de la conexión
        val url = "jdbc:mysql://localhost:3306/peliculas_db"
        val user = "USER"
        val password = "" // Dejo vacio porque me 

        var connection: Connection = null

        try {
          // Establecer conexión
          connection = DriverManager.getConnection(url, user, password)
          println("Conexión exitosa a la base de datos!")

          // Crear y ejecutar una consulta
          val statement = connection.createStatement()
          val resultSet: ResultSet = statement.executeQuery("SELECT * FROM peliculas")

          // Mostrar los resultados
          println("Películas en la base de datos:")
          while (resultSet.next()) {
            val id = resultSet.getInt("id_pelicula")
            val genero = resultSet.getString("genero")
            val titulo = resultSet.getString("titulo")
            println(s"ID: $id, Género: $genero, Título: $titulo")
          }
        } catch {
          case e: Exception =>
            e.printStackTrace()
        } finally {
          if (connection != null) {
            connection.close()
            println("Conexión cerrada.")
          }
        }
      }
    }
    ```

    
<img width="959" alt="Captura de pantalla 2025-01-22 224320" src="https://github.com/user-attachments/assets/9dae7cab-452e-46a1-91fa-13841cba0294" />



<img width="824" alt="error" src="https://github.com/user-attachments/assets/8bf38077-c70c-4d6f-bf66-0543e570d769" />


