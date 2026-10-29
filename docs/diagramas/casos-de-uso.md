# Diagrama de casos de uso — PetCare

Muestra **quién** usa el sistema (actores) y **para qué** (casos de uso).
GitHub renderiza el bloque Mermaid automáticamente.

## Actores

| Actor | Rol en el sistema |
|---|---|
| Recepcionista | Da de alta personal, dueños y mascotas, agenda y reprograma citas, emite comprobantes y gestiona consultorios. |
| Veterinario | Atiende o cancela citas, registra consultas en el historial y revisa su total cobrado. |
| Dueño | Consulta la información de su mascota (historial y costos). |

## Diagrama

```mermaid
flowchart LR
    recep(["👤 Recepcionista"])
    vet(["👤 Veterinario"])
    dueno(["👤 Dueño"])

    subgraph Sistema PetCare
        uc1(["Registrar dueño y mascota"])
        uc2(["Registrar veterinario"])
        uc3(["Registrar recepcionista"])
        uc4(["Agendar cita"])
        uc5(["Reprogramar cita"])
        uc6(["Atender o cancelar cita"])
        uc7(["Registrar consulta en historial"])
        uc8(["Generar comprobante"])
        uc9(["Consultar historial clínico"])
        uc10(["Ver total cobrado por veterinario"])
        uc11(["Consultar costos de su mascota"])
        uc12(["Listar mascotas y su sonido"])
        uc13(["Consultar disponibilidad de consultorios"])
        uc14(["Actualizar datos de una mascota"])
        uc15(["Dar de baja una mascota"])
    end

    recep --- uc1
    recep --- uc2
    recep --- uc3
    recep --- uc4
    recep --- uc5
    recep --- uc8
    recep --- uc12
    recep --- uc13
    recep --- uc14
    recep --- uc15

    vet --- uc6
    vet --- uc7
    vet --- uc9
    vet --- uc10

    dueno --- uc9
    dueno --- uc11
```

## Descripción breve de cada caso de uso

| Caso de uso | Actor principal | Qué hace |
|---|---|---|
| Registrar dueño y mascota | Recepcionista | Da de alta un dueño y su mascota (perro, gato o ave). |
| Registrar veterinario | Recepcionista | Añade un veterinario y lo asigna a la clínica. |
| Registrar recepcionista | Recepcionista | Da de alta personal de mostrador con su turno. |
| Agendar cita | Recepcionista | Programa una cita entre mascota, veterinario y consultorio en una fecha. |
| Reprogramar cita | Recepcionista | Cambia la fecha u hora de una cita ya programada, validando que no choque. |
| Atender o cancelar cita | Veterinario | Marca la cita como atendida o la cancela, respetando las reglas de negocio. |
| Registrar consulta en historial | Veterinario | Anota en el historial de la mascota lo realizado en la atención. |
| Generar comprobante | Recepcionista | Emite el comprobante con el total de una cita atendida. |
| Consultar historial clínico | Veterinario / Dueño | Revisa las consultas registradas de una mascota. |
| Ver total cobrado por veterinario | Veterinario | Muestra cuánto ha cobrado un veterinario. |
| Consultar costos de su mascota | Dueño | Muestra al dueño lo cobrado por los servicios de su mascota. |
| Listar mascotas y su sonido | Recepcionista | Recorre las mascotas llamando a `hacerSonido()` (demostración de polimorfismo). |
| Consultar disponibilidad de consultorios | Recepcionista | Verifica qué consultorios están libres antes de agendar. |
| Actualizar datos de una mascota | Recepcionista | Corrige peso o edad respetando las validaciones de negocio. |
| Dar de baja una mascota | Recepcionista | Elimina la mascota del sistema junto con su historial. |
```
