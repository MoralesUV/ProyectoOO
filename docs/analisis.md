# Modelo de análisis — Clínica veterinaria PetCare

## 1.1 Identificación de clases y objetos

A partir del caso de estudio se identifican las siguientes clases candidatas.
Para elegirlas se buscaron los **sustantivos** del enunciado (las cosas que el
sistema necesita recordar) y los **verbos** (lo que esas cosas deben hacer).

### Tabla de análisis

| # | Clase candidata | Tipo | Atributos principales | Responsabilidades (métodos) | Justificación |
|---|---|---|---|---|---|
| 1 | `Persona` | Abstracta | nombre, apellido, telefono, correo | `getNombreCompleto()`, `describir()` (abstracto), `toString()` | Dueños, veterinarios y recepcionistas comparten los mismos datos de contacto. Se concentran aquí para no repetirlos. No se crea una "persona" suelta: siempre es alguno de los tres tipos. |
| 2 | `Dueno` | Concreta | dirección, (hereda los de Persona) | `describir()`, `toString()` | Es una persona con un dato propio (dirección) y es quien posee mascotas. Sí se crea directamente. |
| 3 | `Veterinario` | Concreta | cedulaProfesional, especialidad | `describir()`, `atenderCita()` | Persona con cédula y especialidad. Atiende citas y se le consulta su total cobrado. Existe aunque la clínica cierre. |
| 4 | `Recepcionista` | Concreta | turno | `describir()` | Persona con turno de trabajo. Tercera clase de la jerarquía de personas. |
| 5 | `Mascota` | Abstracta | nombre, edad, peso, dueño, historial | `hacerSonido()` (abstracto), `toString()` | Perros, gatos y aves comparten datos (nombre, edad, peso) y la obligación de "hacer sonido". No llega "una mascota" genérica: llega un perro, un gato o un ave. |
| 6 | `Perro` | Concreta | raza, nivelEnergia | `hacerSonido()` → "Guau" | Tipo de mascota con datos propios. Se crea directamente. |
| 7 | `Gato` | Concreta | esDomestico | `hacerSonido()` → "Miau" | Tipo de mascota con dato propio (si es doméstico). |
| 8 | `Ave` | Concreta | longitudAlas | `hacerSonido()` → "Pío" | Tipo de mascota exótica con dato propio (longitud de alas). |
| 9 | `HistorialClinico` | Concreta | lista de consultas | `agregarConsulta()`, `listarConsultas()` | Guarda las consultas de UNA mascota. No tiene sentido sin su mascota: si la mascota se elimina, su historial también (composición). |
| 10 | `Consultorio` | Concreta | numero, ocupado | `estaDisponible()`, `ocupar()`, `liberar()` | Espacio físico numerado. Pertenece a la clínica y desaparece con ella (composición). |
| 11 | `Clinica` | Concreta | nombre, lista de veterinarios, lista de consultorios, lista de citas | `agregarVeterinario()`, `agendarCita()`, `darDeBajaMascota()` | Es el "centro" que agrupa todo: contiene a su personal, sus consultorios y coordina las citas. |
| 12 | `Cita` | Concreta | fecha, hora, estado, mascota, veterinario, consultorio, servicio | `atender()`, `cancelar()`, `generarComprobante()` | Relaciona a una mascota, un veterinario y un consultorio en una fecha. Cambia de estado (programada, atendida, cancelada). |
| 13 | `EstadoCita` | Enumeración | PROGRAMADA, ATENDIDA, CANCELADA | — | Una cita solo puede estar en uno de tres estados. Un enum evita errores de escribir el estado "a mano". |
| 14 | `Facturable` | Interfaz | — | `calcularCosto()` | Contrato: "yo sé cuánto cobro". Lo cumplen consulta, vacunación y cirugía, aunque cada una calcule distinto. Permite sumar costos sin preguntar el tipo. |
| 15 | `Servicio` | Abstracta | descripción | `calcularCosto()` (abstracto) | Base común de los tres servicios. Implementa `Facturable`. Agrupa lo compartido entre consulta, vacunación y cirugía. |
| 16 | `ConsultaGeneral` | Concreta | tarifaBase | `calcularCosto()` → tarifa base | Servicio más simple: cobra una tarifa fija. |
| 17 | `Vacunacion` | Concreta | tarifaBase, costoVacuna | `calcularCosto()` → tarifa + costo vacuna | Servicio que suma el costo de la vacuna aplicada. |
| 18 | `Cirugia` | Concreta | complejidad, horasQuirofano | `calcularCosto()` → según complejidad y horas | Servicio más caro: su costo depende de la dificultad y el tiempo de quirófano. |
| 19 | `Comprobante` | Concreta | folio, total, fecha | `imprimir()` | Se genera al atender una cita. Lleva un contador static de folios (comprobantes generados). |

> Son 19 clases candidatas; el examen pide mínimo 12. Tener más da margen para
> descartar algunas sin quedar por debajo del mínimo.

### Tres objetos de ejemplo

| Objeto | Clase | Valores |
|---|---|---|
| `firulais` | `Perro` | nombre = "Firulais", edad = 4, peso = 12.5, raza = "Labrador", nivelEnergia = "alto" |
| `dra_lopez` | `Veterinario` | nombre = "Ana", apellido = "López", cédula = "VET-1024", especialidad = "Cirugía" |
| `cita001` | `Cita` | fecha = 2026-11-03, hora = 10:00, estado = PROGRAMADA, mascota = firulais, veterinario = dra_lopez |

### Clases candidatas que se descartaron

- **`Direccion`**: la dirección del dueño es un solo dato de texto; crear una
  clase aparte solo la complicaría sin aportar comportamiento. Se deja como
  atributo `String` dentro de `Dueno`.
- **`Telefono`**: igual que la dirección, es un dato simple de contacto. No tiene
  comportamiento propio que justifique una clase; se queda como atributo de
  `Persona`.
