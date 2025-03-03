class Libro:
    def __init__(self, titulo, autor, categoria, isbn):
        self.info = (titulo, autor)  # Tupla para título y autor (inmutables)
        self.categoria = categoria
        self.isbn = isbn

    def __str__(self):
        return f"{self.info[0]} por {self.info[1]} (ISBN: {self.isbn}, Categoría: {self.categoria})"


class Usuario:
    def __init__(self, nombre, user_id):
        self.nombre = nombre
        self.user_id = user_id
        self.libros_prestados = []  # Lista de libros prestados

    def __str__(self):
        return f"Usuario: {self.nombre} (ID: {self.user_id})"


class Biblioteca:
    def __init__(self):
        self.libros_disponibles = {}  # Diccionario con ISBN como clave y objeto Libro como valor
        self.usuarios_registrados = set()  # Conjunto para IDs de usuarios únicos

    def agregar_libro(self, libro):
        if libro.isbn not in self.libros_disponibles:
            self.libros_disponibles[libro.isbn] = libro
            print(f"Libro agregado: {libro}")
        else:
            print("Este libro ya está registrado en la biblioteca.")

    def quitar_libro(self, isbn):
        if isbn in self.libros_disponibles:
            eliminado = self.libros_disponibles.pop(isbn)
            print(f"Libro eliminado: {eliminado}")
        else:
            print("Libro no encontrado.")

    def registrar_usuario(self, usuario):
        if usuario.user_id not in self.usuarios_registrados:
            self.usuarios_registrados.add(usuario.user_id)
            print(f"Usuario registrado: {usuario}")
        else:
            print("El ID de usuario ya está registrado.")

    def dar_baja_usuario(self, user_id):
        if user_id in self.usuarios_registrados:
            self.usuarios_registrados.remove(user_id)
            print(f"Usuario con ID {user_id} dado de baja.")
        else:
            print("Usuario no encontrado.")

    def prestar_libro(self, usuario, isbn):
        if usuario.user_id in self.usuarios_registrados and isbn in self.libros_disponibles:
            libro = self.libros_disponibles.pop(isbn)
            usuario.libros_prestados.append(libro)
            print(f"Libro prestado a {usuario.nombre}: {libro}")
        else:
            print("No se puede realizar el préstamo. Verifica el usuario o la disponibilidad del libro.")

    def devolver_libro(self, usuario, isbn):
        for libro in usuario.libros_prestados:
            if libro.isbn == isbn:
                usuario.libros_prestados.remove(libro)
                self.libros_disponibles[isbn] = libro
                print(f"Libro devuelto: {libro}")
                return
        print("Este libro no pertenece a la biblioteca o no fue prestado.")

    def buscar_libro(self, criterio, valor):
        resultados = [libro for libro in self.libros_disponibles.values() if valor.lower() in str(getattr(libro, criterio)).lower()]
        if resultados:
            print("Libros encontrados:")
            for libro in resultados:
                print(libro)
        else:
            print("No se encontraron libros con ese criterio.")

    def listar_libros_prestados(self, usuario):
        if usuario.libros_prestados:
            print(f"Libros prestados a {usuario.nombre}:")
            for libro in usuario.libros_prestados:
                print(libro)
        else:
            print(f"{usuario.nombre} no tiene libros prestados.")


# Pruebas del sistema
biblio = Biblioteca()

# Creación de libros
libro1 = Libro("1984", "George Orwell", "Ficción", "12345")
libro2 = Libro("Cien años de soledad", "Gabriel García Márquez", "Realismo mágico", "67890")

# Registro de usuario
usuario1 = Usuario("Juan Pérez", "U001")

biblio.registrar_usuario(usuario1)
biblio.agregar_libro(libro1)
biblio.agregar_libro(libro2)

# Préstamo y devolución
biblio.prestar_libro(usuario1, "12345")
biblio.listar_libros_prestados(usuario1)
biblio.devolver_libro(usuario1, "12345")

