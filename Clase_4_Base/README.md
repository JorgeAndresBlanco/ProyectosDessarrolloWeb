# Proyecto Base — Clase 4

Aplicación Spring Boot + Thymeleaf + Bootstrap, lista para que durante la **Clase 4** se le agregue persistencia con **MySQL, JPA e Hibernate**.

---

## ✅ Lo que ya tiene

- Spring Boot 3.3.5 + Java 21
- Thymeleaf con `spring.thymeleaf.cache=false` (reload sin reiniciar)
- Bootstrap 5.3 + Bootstrap Icons por CDN
- CSS propio en `static/css/styles.css`
- Fragmento **navbar** y **footer** reutilizables (`fragments/header.html`)
- 3 páginas: `/`, `/cursos`, `/cursos/{id}`
- POJO `Curso` con id, nombre, descripción, créditos y profesor
- Datos en **memoria** dentro del `CursoController` (6 cursos)
- DevTools activo

## 🚧 Lo que falta (lo agregamos en clase)

- Dependencias **JPA** y **MySQL Connector** en `pom.xml` (ya comentadas)
- Configuración de conexión en `application.properties` (ya comentada)
- Anotaciones **`@Entity`**, **`@Id`**, **`@GeneratedValue`**, **`@Column`** sobre la clase `Curso`
- `CursoRepository` (interface que extiende `JpaRepository`)
- `CursoService` con `@Autowired CursoRepository`
- Refactor del `CursoController` para usar el `service`
- Insertar los datos en MySQL con `seed-data.sql`

---

## 🚀 Cómo correr

### Antes de cualquier cambio (modo memoria)

```bash
# Desde la carpeta clase4base/
./mvnw spring-boot:run        # Linux/Mac (si tenés el wrapper)
mvnw.cmd spring-boot:run      # Windows
```

O abrir el proyecto en **VS Code** y darle Run a `Clase4baseApplication.java`.

Abrir: <http://localhost:8080>

### Después de las modificaciones de la Clase 4 (con MySQL)

1. Tener MySQL Server corriendo en `localhost:3306`.
2. Crear la BD en Workbench:
   ```sql
   CREATE DATABASE cursoswebdb CHARACTER SET utf8mb4;
   ```
3. En `application.properties`, descomentar la sección "CLASE 4" y poner tu password de root.
4. En `pom.xml`, descomentar las dependencias JPA y MySQL.
5. Arrancar la app — Hibernate creará la tabla `cursos` automáticamente.
6. Cargar los datos seed:
   - File → Open SQL Script → `seed-data.sql`
   - Click en el rayo (Execute).
7. Refrescar <http://localhost:8080/cursos> — los datos ahora vienen de MySQL.

---

## 🗂️ Estructura

```
clase4base/
├── pom.xml                                  (sin JPA - se agrega en clase)
├── seed-data.sql                            (INSERTs para Workbench)
├── README.md
├── .gitignore
└── src/main/
    ├── java/com/ufide/clase4base/
    │   ├── Clase4baseApplication.java       (main)
    │   ├── Curso.java                       (POJO - en clase pasa a @Entity)
    │   ├── HomeController.java
    │   └── CursoController.java             (datos en memoria - se refactoriza)
    └── resources/
        ├── application.properties           (config MySQL comentada)
        ├── static/css/styles.css
        └── templates/
            ├── home.html
            ├── cursos.html
            ├── curso.html
            └── fragments/header.html        (navbar + footer)
```

---

## 🎯 Pasos de la Clase 4 sobre este proyecto

1. **Crear la BD** `cursoswebdb` en Workbench.
2. **Agregar dependencias** en `pom.xml` (descomentar la sección "CLASE 4").
3. **Configurar conexión** en `application.properties` (descomentar y poner password).
4. **Convertir `Curso` en Entity** — agregar `@Entity`, `@Table`, `@Id`, `@GeneratedValue`, `@Column`.
5. **Crear `CursoRepository`** que extienda `JpaRepository<Curso, Long>`.
6. **Crear `CursoService`** con métodos `listar()`, `guardar()`, `eliminar()`.
7. **Refactorizar `CursoController`** — borrar la lista en memoria, inyectar el service.
8. **Cargar `seed-data.sql`** en Workbench.
9. **Refrescar `/cursos`** — los mismos cursos pero ahora desde MySQL.
10. **Commit + push** del cambio.

Ver `guion_clase4.md` para el paso a paso completo con explicaciones.

---

## 🛣️ URLs disponibles

| URL | Qué muestra |
|---|---|
| `/` | Página de inicio con tu nombre. Aceptá `?nombre=` opcional. |
| `/?nombre=Esteban` | Saludo personalizado. |
| `/cursos` | Lista de cursos (grid responsive). |
| `/cursos/1` | Detalle del curso con ID = 1. |
| `/cursos/999` | Mensaje de "curso no encontrado". |

---

## 🆘 Si algo falla

- **App no arranca:** verificar que `Clase4baseApplication.java` esté en `src/main/java/com/ufide/clase4base/` y el `pom.xml` apunte al artifact `clase4base`.
- **Bootstrap no carga:** chequear conexión a internet (los CDNs).
- **Después de agregar JPA: `Failed to determine driver`:** la dependencia `mysql-connector-j` no está descomentada en el `pom.xml`.
- **`Access denied for user 'root'`:** password mal en `application.properties`.
- **Tabla `cursos` no se crea:** falta `spring.jpa.hibernate.ddl-auto=update` o no se agregaron las anotaciones `@Entity` a `Curso`.
- **Página vacía:** la tabla está vacía. Ejecutar `seed-data.sql` en Workbench.
