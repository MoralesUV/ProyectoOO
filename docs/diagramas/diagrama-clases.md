# Diagrama de clases — PetCare

Diagrama de clases del sistema de la clínica veterinaria PetCare.
GitHub renderiza este bloque Mermaid automáticamente al abrir el archivo.

## Leyenda de relaciones

| Notación | Significado | Ejemplo en el diagrama |
|---|---|---|
| Triángulo hueco (línea sólida) | Herencia ("es un") | `Perro` hereda de `Mascota` |
| Triángulo hueco (línea punteada) | Realización de interfaz | `Servicio` implementa `Facturable` |
| Rombo relleno | Composición (la parte muere con el todo) | `Clinica` posee `Consultorio` |
| Rombo hueco | Agregación (la parte sobrevive al todo) | `Clinica` emplea `Veterinario` |
| Flecha simple | Asociación (uso entre objetos independientes) | `Cita` usa `Mascota` |

## Diagrama

```mermaid
classDiagram
    direction TB

    %% ===================== PERSONAS =====================
    class Persona {
        <<abstract>>
        #String nombre
        #String apellido
        #String telefono
        #String correo
        +getNombreCompleto() String
        +describir()* String
        +toString() String
    }
    class Dueno {
        -String direccion
        +describir() String
    }
    class Veterinario {
        -String cedulaProfesional
        -String especialidad
        +describir() String
        +getTotalCobrado() double
    }
    class Recepcionista {
        -String turno
        +describir() String
    }

    %% ===================== MASCOTAS =====================
    class Mascota {
        <<abstract>>
        #String nombre
        #int edad
        #double peso
        +setPeso(double)
        +setEdad(int)
        +hacerSonido()* String
        +toString() String
    }
    class Perro {
        -String raza
        -String nivelEnergia
        +hacerSonido() String
    }
    class Gato {
        -boolean esDomestico
        +hacerSonido() String
    }
    class Ave {
        -double longitudAlas
        +hacerSonido() String
    }
    class HistorialClinico {
        -List~String~ consultas
        +agregarConsulta(String)
        +listarConsultas() List
    }

    %% ===================== SERVICIOS =====================
    class Facturable {
        <<interface>>
        +calcularCosto() double
    }
    class Servicio {
        <<abstract>>
        #String descripcion
        +calcularCosto()* double
    }
    class ConsultaGeneral {
        -double tarifaBase
        +calcularCosto() double
    }
    class Vacunacion {
        -double tarifaBase
        -double costoVacuna
        +calcularCosto() double
    }
    class Cirugia {
        -int complejidad
        -double horasQuirofano
        +calcularCosto() double
    }
    class Comprobante {
        -int contadorFolios$
        -int folio
        -double total
        +imprimir() String
    }

    %% ===================== APP / NUCLEO =====================
    class EstadoCita {
        <<enumeration>>
        PROGRAMADA
        ATENDIDA
        CANCELADA
    }
    class Cita {
        -LocalDate fecha
        -LocalTime hora
        -EstadoCita estado
        +atender()
        +cancelar()
        +generarComprobante() Comprobante
    }
    class Consultorio {
        -int numero
        -boolean ocupado
        +estaDisponible() boolean
    }
    class Clinica {
        -String nombre
        +agregarVeterinario(Veterinario)
        +agendarCita(Cita)
        +darDeBajaMascota(Mascota)
    }

    %% ===================== HERENCIA =====================
    Persona <|-- Dueno
    Persona <|-- Veterinario
    Persona <|-- Recepcionista

    Mascota <|-- Perro
    Mascota <|-- Gato
    Mascota <|-- Ave

    %% ===================== REALIZACION DE INTERFAZ =====================
    Facturable <|.. Servicio
    Servicio <|-- ConsultaGeneral
    Servicio <|-- Vacunacion
    Servicio <|-- Cirugia

    %% ===================== COMPOSICION =====================
    Mascota "1" *-- "1" HistorialClinico : posee
    Clinica "1" *-- "0..*" Consultorio : contiene
    Clinica "1" *-- "0..*" Cita : gestiona

    %% ===================== AGREGACION =====================
    Clinica "1" o-- "0..*" Veterinario : emplea

    %% ===================== ASOCIACIONES =====================
    Cita "0..*" --> "1" Mascota : paciente
    Cita "0..*" --> "1" Veterinario : atiende
    Cita "0..*" --> "1" Consultorio : ocurre en
    Cita "1" --> "1" Servicio : cobra
    Cita "1" --> "0..1" Comprobante : genera
    Mascota "0..*" --> "1" Dueno : pertenece a
```
