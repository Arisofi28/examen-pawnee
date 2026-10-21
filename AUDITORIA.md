# Auditoría · Versión B
Código: ___
Repositorio público propio: ___
Rama: ___

## B-01 · No se guarda la edición del avistamiento
### Síntoma
Acción/datos, esperado y observado ANTES:
Se intenta editar. Cambio de ubicación de estacionamiento a Edificio. 
Respuesta: 
Campos no permitidos en el cuerpo · petición 02c8f3c7-a758-4bc6-a42c-f03976a55b64
No me deja editar. 


### Causa
Explicación causal y archivo/capa, no solo su nombre:

El campo criaturaId no forma parte de su contrato de escritura. 
frontend/src/paginas/FormularioAvistamiento.tsx linea 19. 


### Cambio
Qué modifiqué, por qué resuelve la causa y commit de corrección:
Respuesta: 
Campos no permitidos en el cuerpo · petición 02c8f3c7-a758-4bc6-a42c-f03976a55b64
No me deja editar. 
Estaba esto: 
datos[id ? 'criaturaId' : 'criatura'] = form.criatura;

El campo criaturaId no forma parte de su contrato de escritura.
y lo cambie por:
datos.criatura = form.criatura;

### Prueba
Pasos/comando, datos, status/resultado esperado y observado DESPUÉS; archivo de evidencia:

Al cambiarlo logre que se guardaran los datos. Cambié de estacionamiento a piscina. 
Adjunto las imagenes en el pdf. 

### Estado
Resuelto / parcial / no resuelto. Si es parcial, qué falta:
RESUELTO




## B-02 · El detalle no identifica a la criatura
### Síntoma
Acción/datos, esperado y observado ANTES:

En avistamientos. Al momento de ver el detalle de cada criatura, en la sección criatura: aparece ID
65a000000000000000000203
Criatura
ID: 65a000000000000000000102. 
Es incorrecto porque debería salir el nombre ya que arriba ya se encuentra el ID. 

### Causa
Explicación causal y archivo/capa, no solo su nombre:
No tiene acceso a la información adentro del Atlas sino solo al ID. 
backend/src/services/avistamientos.service.ts

### Cambio
Qué modifiqué, por qué resuelve la causa y commit de corrección:
añadi: 
    .populate('criatura');
Antes, findById() solo habia esto devolviendo la info. Con .populate('criatura'), Moongoose puede recuperar el documento relacionado, permitiendo acceder a criatura.nombre.

### Prueba
Pasos/comando, datos, status/resultado esperado y observado DESPUÉS; archivo de evidencia:
original: 
export async function obtener(id: string) {
  const doc = await Avistamiento.findById(idValido(id));
  if (!doc) throw new ApiError(404, 'Avistamiento no encontrado');
  return doc;
}
mi cambio: 
export async function obtener(id: string) {
  const doc = await Avistamiento.findById(idValido(id))
    .populate('criatura');

  if (!doc) {
    throw new ApiError(404, 'Avistamiento no encontrado');
  }

  return doc;
}

ahora aparece: 
ID
65a000000000000000000203
Criatura
El Dragón del Estacionamiento
Adjunto las imagenes en el pdf. 

### Estado
Resuelto / parcial / no resuelto. Si es parcial, qué falta:
RESUELTO




## B-03 · Se acepta un tipo de criatura inválido
### Síntoma
Acción/datos, esperado y observado ANTES:
Probar manualmente porque desde las opciones desplegadas no se puede. Abrir en ThunderClient. Escogí un Id de alguna criatura. 
http://127.0.0.1:3000/api/criaturas/65a000000000000000000103
le cambie {
  "tipo": "fantasma"
} y no me dio ningun error. me dejó cambiarle. 

### Causa
Explicación causal y archivo/capa, no solo su nombre:
El problema es que tenemos solo 4 parámetros aceptados pero desde el código si se puede cambiar a otro no permitido. 
backend/src/services/criaturas.service.ts 

### Cambio
Qué modifiqué, por qué resuelve la causa y commit de corrección:
original:

  const doc = await Criatura.findByIdAndUpdate(
    idValido(id),
    { $set: cambios },
    { new: true }
  );

cambie: 
  const doc = await Criatura.findByIdAndUpdate(
    idValido(id),
    { $set: cambios },
    {
      new: true,
      runValidators: true
    }
  );

El problema es que findIdAndUpdate no ejecuta los validadores del esquema por defecto
Al incorporar runValidators: true, Mongoose valida los valores actualizados contra las reglas existentes del modelo.


### Prueba
Pasos/comando, datos, status/resultado esperado y observado DESPUÉS; archivo de evidencia:

### Estado
Resuelto / parcial / no resuelto. Si es parcial, qué falta:

## B-04 · El filtro de avistamientos no actualiza
### Síntoma
Acción/datos, esperado y observado ANTES:

### Causa
Explicación causal y archivo/capa, no solo su nombre:

### Cambio
Qué modifiqué, por qué resuelve la causa y commit de corrección:

### Prueba
Pasos/comando, datos, status/resultado esperado y observado DESPUÉS; archivo de evidencia:

### Estado
Resuelto / parcial / no resuelto. Si es parcial, qué falta:

