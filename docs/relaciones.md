# 1.3 Justificación de relaciones — PetCare

Para elegir el tipo de cada relación se respondió siempre la misma pregunta:
**si elimino una de las partes, ¿qué pasa con la otra?** De ahí sale si es
composición (la parte muere con el todo), agregación (la parte sobrevive) o
asociación (objetos independientes que solo se usan).

## Composiciones (rombo relleno)

### Mascota ◆— HistorialClinico
El historial clínico guarda las consultas de **una** mascota concreta. No tiene
sentido un historial que no pertenezca a ninguna mascota. La mascota crea su
propio historial al nacer en el sistema, y si la mascota se da de baja, su
historial se elimina con ella. Por eso es composición y no agregación: la parte
no sobrevive al todo ni se comparte con otra mascota.

### Clinica ◆— Consultorio
Los consultorios son espacios de la clínica, numerados y propios de ella. La
clínica los crea y los posee; si la clínica desaparece, sus consultorios
desaparecen también (no van a parar a otra clínica). El ciclo de vida del
consultorio está atado al de la clínica, así que es composición.

### Clinica ◆— Cita
Las citas se generan y se gestionan dentro de la clínica. Una cita no existe por
fuera del sistema que la agenda; si la clínica deja de operar, sus citas dejan
de tener sentido. La clínica es la dueña del ciclo de vida de las citas, por eso
es composición.

## Agregación (rombo hueco)

### Clinica ◇— Veterinario
La clínica emplea a un equipo de veterinarios, pero cada veterinario existe por
sí mismo: se recibe ya creado y, si la clínica cierra, el veterinario sigue
existiendo y puede trabajar en otra. La clínica los **agrupa**, no los **posee**.
Como la parte sobrevive al todo, es agregación y no composición. Esta es la
diferencia clave frente a los consultorios: el consultorio muere con la clínica,
el veterinario no.

## Asociaciones (flecha simple)

### Cita → Mascota, Veterinario y Consultorio
Una cita relaciona a una mascota con un veterinario en un consultorio y una
fecha. Pero esos tres objetos son independientes y existen antes y después de la
cita: la mascota vive aunque la cita se cancele, el veterinario atiende muchas
citas y el consultorio se reutiliza. La cita solo los **referencia** para hacer
su trabajo; no es dueña de ellos. Por eso es asociación y no composición.

### Cita → Servicio
Cada cita cobra un servicio (consulta, vacunación o cirugía). El servicio define
cómo se calcula el costo, pero se trata como un objeto que la cita usa para
facturar, no como una parte interna que nazca y muera con ella. Es asociación.

### Cita → Comprobante
Al atender una cita se genera un comprobante con el total. La cita apunta al
comprobante que produjo, pero el comprobante es un documento con su propio folio
que tiene valor como registro de cobro. Se modela como asociación con
multiplicidad 0..1 (una cita aún no atendida no tiene comprobante).

### Mascota → Dueno
Cada mascota pertenece a un dueño. Son dos objetos independientes: el dueño
existe aunque su mascota se dé de baja, y puede tener varias mascotas. La mascota
solo guarda una referencia a quién es su dueño. Por eso es asociación y no
composición.

## Resumen del criterio

| Relación | Tipo | ¿Qué pasa si borro el todo? |
|---|---|---|
| Mascota — HistorialClinico | Composición | El historial se borra con la mascota. |
| Clinica — Consultorio | Composición | Los consultorios desaparecen con la clínica. |
| Clinica — Cita | Composición | Las citas dejan de existir. |
| Clinica — Veterinario | Agregación | El veterinario sigue existiendo. |
| Cita — Mascota/Veterinario/Consultorio | Asociación | Siguen existiendo; solo se usaban. |
| Cita — Servicio | Asociación | El servicio es independiente. |
| Cita — Comprobante | Asociación | El comprobante queda como registro. |
| Mascota — Dueno | Asociación | El dueño sigue existiendo. |
