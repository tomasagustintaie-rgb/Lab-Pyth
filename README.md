# Lab-Pyth
UTN Frre Lab Pyth Com 1.1 Group A26
# AMBIENTE / DEFINICIÓN DE ESTRUCTURAS
clientes = []
bicicletas = []
alquileres = []

# Contadores y constantes
PRECIO_HORA = 50

# PROCEDIMIENTOS Y FUNCIONES
def dni_existe(dni):
    for cliente in clientes:
        if cliente["dni"] == dni:
            return True
    return False


def registrar_cliente():
    print("\n--- Registrar Cliente ---")
    while True:
        nombre = input("Nombre: ").strip()

        if nombre == "":
            print("Error: El nombre no puede estar vacío.")
        else:
            break
    while True:
        try:
            dni = int(input("DNI: "))

            if dni < 1000000:
                print("Error: El DNI debe tener al menos 7 dígitos.")
            elif dni_existe(dni):
                print("Error: Ya existe un cliente registrado con ese DNI.")
            else:
                break

        except ValueError:
            print("Error: El DNI debe contener solo números.")

    telefono = input("Telefono: ")

    nuevo_id = len(clientes) + 1

    nuevo_cliente = {
        "id": nuevo_id,
        "nombre": nombre,
        "dni": dni,
        "telefono": telefono
    }

    clientes.append(nuevo_cliente)

    print(f"Cliente registrado con éxito. ID asignado: {nuevo_id}")


def agregar_bicicleta():
    print("\n--- Agregar Bicicleta ---")

    while True:
        try:
            codigo = int(input("Codigo de la bicicleta: "))

            if codigo_bicicleta_existe(codigo):
                print("Error: Ya existe una bicicleta con ese código.")
            else:
                break

        except ValueError:
            print("Error: El código debe ser numérico.")

    while True:
        try:
            rodado = int(input("Rodado: "))

            if rodado <= 0:
                print("Error: El rodado debe ser mayor que cero.")
            else:
                break

        except ValueError:
            print("Error: Ingrese un rodado válido.")

    nueva_bici = {
        "codigo": codigo,
        "estado": "disponible",
        "rodado": rodado
    }

    bicicletas.append(nueva_bici)

    print("Bicicleta agregada con éxito.")


def codigo_bicicleta_existe(codigo):
    for bicicleta in bicicletas:
        if bicicleta["codigo"] == codigo:
            return True
    return False


def buscar_bicicleta(codigo):
    # En Python los índices empiezan en 0.
    # Devolvemos el índice si la encuentra, o -1 si no existe.
    for i in range(len(bicicletas)):
        if bicicletas[i]["codigo"] == codigo:
            return i
    return -1


def alquilar():
    print("\n--- Alquilar Bicicleta ---")
    id_cliente = int(input("ID Cliente: "))
    codigo = int(input("Codigo Bicicleta: "))
    hora_inicio = int(input("Hora Inicio: "))

    pos = buscar_bicicleta(codigo)

    if pos == -1:
        print("Bicicleta inexistente")
    else:
        if bicicletas[pos]["estado"] == "disponible":
            nuevo_alquiler = {
                "idCliente": id_cliente,
                "codigoBici": codigo,
                "horaInicio": hora_inicio,
                "horaFin": 0,
                "importe": 0.0,
                "activo": True
            }
            alquileres.append(nuevo_alquiler)
            bicicletas[pos]["estado"] = "alquilada"
            print("Alquiler registrado con éxito.")
        else:
            print("Bicicleta no disponible")


def buscar_alquiler_activo(codigo):
    for i in range(len(alquileres)):
        if (alquileres[i]["codigoBici"] == codigo and
                alquileres[i]["activo"]):
            return i
    return -1


def devolver():
    print("\n--- Devolver Bicicleta ---")
    codigo = int(input("Codigo Bicicleta: "))
    hora_fin = int(input("Hora Fin: "))

    pos_alquiler = buscar_alquiler_activo(codigo)

    if pos_alquiler == -1:
        print("No existe alquiler activo")
    else:
        horas = hora_fin - alquileres[pos_alquiler]["horaInicio"]

        if horas < 1:
            horas = 1

        alquileres[pos_alquiler]["horaFin"] = hora_fin
        alquileres[pos_alquiler]["importe"] = horas * PRECIO_HORA
        alquileres[pos_alquiler]["activo"] = False

        pos_bici = buscar_bicicleta(codigo)
        if pos_bici != -1:
            bicicletas[pos_bici]["estado"] = "disponible"

        importe = alquileres[pos_alquiler]['importe']
        print(f"Importe a abonar: ${importe:.2f}")


def estadisticas():
    print("\n--- Estadísticas del Sistema ---")
    print(f"Clientes registrados: {len(clientes)}")
    print(f"Bicicletas: {len(bicicletas)}")
    print(f"Alquileres realizados: {len(alquileres)}")


# PROCESO PRINCIPAL (MENU)
def main():
    opcion = -1

    while opcion != 0:
        print("\n==============================")
        print(" SISTEMA ALQUILER BICICLETAS ")
        print("==============================")
        print("1 Registrar Cliente")
        print("2 Agregar Bicicleta")
        print("3 Alquilar")
        print("4 Devolver")
        print("5 Estadisticas")
        print("0 Salir")
        try:
            opcion = int(input("Seleccione una opción: "))
        except ValueError:
            print("Por favor, ingrese un número válido.")
            continue

        if opcion == 1:
            registrar_cliente()
        elif opcion == 2:
            agregar_bicicleta()
        elif opcion == 3:
            alquilar()
        elif opcion == 4:
            devolver()
        elif opcion == 5:
            estadisticas()
        elif opcion == 0:
            print("Saliendo del sistema...")
        else:
            print("Opción incorrecta.")


# Ejecutar el programa
if __name__ == "__main__":
    main()
