**/proyecto de preparacion /**
**/mysql/**


-- Creación de la base de datos
CREATE DATABASE taszff_normalizada;
USE taszff_normalizada;

-- Tabla Puestos (no depende de ninguna otra tabla)
CREATE TABLE Puestos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    salario_base DECIMAL(10, 2)
);

-- Tabla Proveedores (no depende de ninguna otra tabla)
CREATE TABLE Proveedores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100)
);

-- Tabla Clientes (no depende de ninguna otra tabla)
CREATE TABLE Clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    email VARCHAR(100) UNIQUE
);

-- Tabla TelefonosClientes (depende de Clientes)
CREATE TABLE TelefonosClientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT,
    telefono VARCHAR(20),
    FOREIGN KEY (cliente_id) REFERENCES Clientes(id)
);

-- Tabla Ubicaciones (no tiene referencias completas a otras tablas)
CREATE TABLE Ubicaciones (
    id INT PRIMARY KEY AUTO_INCREMENT,
    entidad_id INT,
    tipo_entidad ENUM('Cliente', 'Proveedor', 'Empleado'),  -- Asegurarse de que esto esté claro, esta relación no tiene una tabla específica
    direccion VARCHAR(255),
    ciudad VARCHAR(100),
    estado VARCHAR(50),
    codigo_postal VARCHAR(10),
    pais VARCHAR(50)
);

-- Tabla Empleados (depende de Puestos)
CREATE TABLE Empleados (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    puesto_id INT,
    fecha_contratacion DATE,
    FOREIGN KEY (puesto_id) REFERENCES Puestos(id)
);

-- Tabla TiposProductos (no depende de ninguna otra tabla)
CREATE TABLE TiposProductos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    tipo_nombre VARCHAR(100),
    descripcion TEXT,
    tipo_padre_id INT,
    FOREIGN KEY (tipo_padre_id) REFERENCES TiposProductos(id) -- Debe asegurarse de que no sea un bucle sin terminación, intente enhebrar adecuadamente
);

-- Tabla Productos (depende de Proveedores y TiposProductos)
CREATE TABLE Productos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    precio DECIMAL(10, 2),
    proveedor_id INT,
    tipo_id INT,
    FOREIGN KEY (proveedor_id) REFERENCES Proveedores(id),
    FOREIGN KEY (tipo_id) REFERENCES TiposProductos(id)
);

-- Tabla Pedidos (depende de Clientes)
CREATE TABLE Pedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT,
    fecha DATE,
    total DECIMAL(10, 2),
    FOREIGN KEY (cliente_id) REFERENCES Clientes(id)
);

-- Tabla DetallesPedido (depende de Pedidos y Productos)
CREATE TABLE DetallesPedido (
    id INT PRIMARY KEY AUTO_INCREMENT,
    pedido_id INT,
    producto_id INT,
    cantidad INT,
    precio_unitario DECIMAL(10, 2),
    FOREIGN KEY (pedido_id) REFERENCES Pedidos(id),
    FOREIGN KEY (producto_id) REFERENCES Productos(id)
);

-- Tabla HistorialPedidos (depende de Pedidos)
CREATE TABLE HistorialPedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    pedido_id INT,
    fecha_modificacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    accion ENUM('Creado', 'Actualizado', 'Eliminado'),
    detalles_cambio TEXT,
    FOREIGN KEY (pedido_id) REFERENCES Pedidos(id)
);

-- Tabla ContactoProveedores (depende de Proveedores)
CREATE TABLE ContactoProveedores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    proveedor_id INT,
    nombre_contacto VARCHAR(100),
    telefono VARCHAR(20),
    email VARCHAR(100),
    FOREIGN KEY (proveedor_id) REFERENCES Proveedores(id)
);

-- Relación muchos a muchos entre Empleados y Proveedores (depende de Empleados y Proveedores)
CREATE TABLE EmpleadosProveedores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    empleado_id INT,
    proveedor_id INT,
    FOREIGN KEY (empleado_id) REFERENCES Empleados(id),
    FOREIGN KEY (proveedor_id) REFERENCES Proveedores(id)
);

INSERT INTO Clientes (nombre, email) VALUES 
('Juan Pérez', 'juanperez@example.com'),
('María González', 'mariagonzalez@example.com'),
('Carlos López', 'carloslopez@example.com'),
('Ana Torres', 'anatorres@example.com'),
('Luis Martinez', 'luismartinez@example.com'),
('Cristina Fernández', 'cristinafernandez@example.com'),
('José Sánchez', 'josesanchez@example.com'),
('Laura Ruiz', 'lauraruiz@example.com'),
('Manuel Díaz', 'manueldiaz@example.com'),
('Sofía Castro', 'sofiacastro@example.com'),
('Diego Morales', 'diegomorales@example.com'),
('Patricia Jiménez', 'patriciajimenez@example.com'),
('Fernando Ortega', 'fernandoortega@example.com'),
('Elena Romero', 'elenaromero@example.com'),
('Miguel Ángel Reyes', 'miguelangelreyes@example.com'),
('Claudia Navarro', 'claudianavarro@example.com'),
('Ricardo Vargas', 'ricardovargas@example.com'),
('Marisol Silva', 'marisilsilva@example.com'),
('Jonathan Castillo', 'jonathancastillo@example.com'),
('Gabriela Bravo', 'gabrielabravo@example.com'),
('Fernando Quintana', 'fernandoquintana@example.com'),
('Alicia Medina', 'aliciamedina@example.com'),
('Francisco Salinas', 'franciscosalinas@example.com'),
('Valeria Pineda', 'valeriapineda@example.com'),
('Ángel Muñoz', 'angelmunoz@example.com'),
('Berta Zamora', 'bertazamora@example.com');

INSERT INTO TelefonosClientes (cliente_id, telefono) VALUES 
(1, '555-0101'),
(1, '555-0102'),
(2, '555-0201'),
(3, '555-0301'),
(3, '555-0302'),
(4, '555-0401'),
(5, '555-0501'),
(6, '555-0601'),
(7, '555-0701'),
(8, '555-0801'),
(9, '555-0901'),
(10, '555-1001'),
(11, '555-1101'),
(12, '555-1201'),
(13, '555-1301'),
(14, '555-1401'),
(15, '555-1501'),
(16, '555-1601'),
(17, '555-1701'),
(18, '555-1801'),
(19, '555-1901'),
(20, '555-2001'),
(21, '555-2101'),
(22, '555-2201'),
(23, '555-2301'),
(24, '555-2401'),
(25, '555-2501');

INSERT INTO Ubicaciones (entidad_id, tipo_entidad, direccion, ciudad, estado, codigo_postal, pais) VALUES 
(1, 'Cliente', 'Calle Falsa 123', 'Madrid', 'Madrid', '28001', 'España'),
(2, 'Cliente', 'Avenida Siempre Viva 456', 'Barcelona', 'Cataluña', '08001', 'España'),
(3, 'Cliente', 'Boulevard de los Sueños Rotos 789', 'Valencia', 'Valencia', '46001', 'España'),
(4, 'Cliente', 'Gran Vía 123', 'Sevilla', 'Andalucía', '41001', 'España'),
(5, 'Cliente', 'Calle del Sol 45', 'Zaragoza', 'Aragón', '50001', 'España'),
(6, 'Cliente', 'Avenida del Primero de Mayo 111', 'Bilbao', 'País Vasco', '48001', 'España'),
(7, 'Cliente', 'Calle de la Libertad 22', 'Malaga', 'Andalucía', '29001', 'España'),
(8, 'Cliente', 'Calle de la Paz 12', 'Murcia', 'Murcia', '30001', 'España'),
(9, 'Cliente', 'Avenida de la Constitución 99', 'Palma', 'Baleares', '07001', 'España'),
(10, 'Cliente', 'Calle de la Rioja 3', 'Logroño', 'La Rioja', '26001', 'España'),
(11, 'Proveedor', 'Calle Industrial 10', 'Madrid', 'Madrid', '28002', 'España'),
(12, 'Proveedor', 'Calle Comercial 20', 'Barcelona', 'Cataluña', '08002', 'España'),
(13, 'Proveedor', 'Avenida del Progreso 50', 'Valencia', 'Valencia', '46002', 'España'),
(14, 'Proveedor', 'Calle del Negocio 7', 'Sevilla', 'Andalucía', '41002', 'España'),
(15, 'Proveedor', 'Calle del Emprendedor 6', 'Zaragoza', 'Aragón', '50002', 'España'),
(16, 'Empleado', 'Calle del Trabajo 3', 'Madrid', 'Madrid', '28003', 'España'),
(17, 'Empleado', 'Avenida del Joven 25', 'Barcelona', 'Cataluña', '08003', 'España'),
(18, 'Empleado', 'Calle de la Productividad 44', 'Valencia', 'Valencia', '46003', 'España'),
(19, 'Empleado', 'Calle del Éxito 12', 'Sevilla', 'Andalucía', '41003', 'España'),
(20, 'Empleado', 'Calle del Futuro 31', 'Zaragoza', 'Aragón', '50003', 'España'),
(21, 'Proveedor', 'Calle de los Colaboradores 15', 'Bilbao', 'País Vasco', '48002', 'España'),
(22, 'Empleado', 'Calle de la Innovación 8', 'Bilbao', 'País Vasco', '48003', 'España'),
(23, 'Cliente', 'Calle de los Recuerdos 33', 'Las Palmas', 'Canarias', '35001', 'España'),
(24, 'Empleado', 'Avenida del Talento 22', 'Madrid', 'Madrid', '28004', 'España'),
(25, 'Empleado', 'Calle de la Sabiduría 7', 'Granada', 'Andalucía', '18001', 'España');


INSERT INTO Empleados (nombre, puesto_id, fecha_contratacion) VALUES 
('Juan Carlos Pérez', 1, '2022-01-15'),  -- Gantry Grader, ID del puesto 1
('María Fernanda López', 2, '2022-02-20'), -- Junior Developer, ID del puesto 2
('Carlos Alberto Ramírez', 3, '2022-03-12'), -- Senior Developer, ID del puesto 3
('Ana María Torres', 4, '2022-04-05'), -- HR Manager, ID del puesto 4
('Luis Miguel González', 5, '2022-05-16'), -- Sales Manager, ID del puesto 5
('Cristina Rodríguez', 1, '2022-06-14'), -- Gantry Grader, ID del puesto 1
('José Luis Martínez', 2, '2022-07-01'), -- Junior Developer, ID del puesto 2
('Laura Jiménez', 3, '2022-08-18'), -- Senior Developer, ID del puesto 3
('Manuel Alejandro Cruz', 4, '2022-09-10'), -- HR Manager, ID del puesto 4
('Sofía Ramos', 5, '2022-10-22'), -- Sales Manager, ID del puesto 5
('Diego Hernández', 1, '2022-11-30'), -- Gantry Grader, ID del puesto 1
('Patricia González', 2, '2022-12-15'), -- Junior Developer, ID del puesto 2
('Fernando Ortega', 3, '2023-01-11'), -- Senior Developer, ID del puesto 3
('Elena Ruiz', 4, '2023-02-07'), -- HR Manager, ID del puesto 4
('Ricardo Sinquefield', 5, '2023-03-25'), -- Sales Manager, ID del puesto 5
('Claudia Navarrete', 1, '2023-04-30'), -- Gantry Grader, ID del puesto 1 
('Marisol Silva', 2, '2023-05-11'), -- Junior Developer, ID del puesto 2
('Jonathan Castro', 3, '2023-06-05'), -- Senior Developer, ID del puesto 3
('Gabriela Bravo', 4, '2023-07-15'), -- HR Manager, ID del puesto 4
('Francisco Salinas', 5, '2023-08-20'), -- Sales Manager, ID del puesto 5
('Alicia Medina', 1, '2023-09-12'), -- Gantry Grader, ID del puesto 1
('Francisco Benitez', 2, '2023-10-10'), -- Junior Developer, ID del puesto 2
('Luis Cuervo', 3, '2023-11-09'), -- Senior Developer, ID del puesto 3
('Natalia Flores', 4, '2023-12-25'), -- HR Manager, ID del puesto 4
('Rodrigo De Souza', 5, '2023-12-30'); -- Sales Manager, ID del puesto 5

INSERT INTO Puestos (nombre, salario_base) VALUES 
('Gerente', 1200.00),
('Supervisor', 800.00),
('Vendedor', 500.00),
('Analista', 700.00),
('Recepcionista', 600.00),
('Contador', 950.00),
('Desarrollador', 1100.00),
('Diseñador', 750.00),
('Marketing', 900.00),
('Logística', 650.00),
('Asistente', 550.00),
('Gerente de ventas', 1300.00),
('Investigador', 1400.00),
('Director de Operaciones', 1600.00),
('Gerente de Proyectos', 1500.00),
('Ingeniero', 1000.00),
('Técnico', 450.00),
('Coordinador', 800.00),
('Jefe de área', 850.00),
('Auxiliar administrativo', 400.00),
('Entrenador', 600.00),
('Mecánico', 700.00),
('Administrador de sistemas', 1200.00),
('Consultor', 950.00),
('Operador', 500.00),
('Preparador de pedidos', 450.00);

INSERT INTO Proveedores (nombre) VALUES 
('Proveedor A'),
('Proveedor B'),
('Proveedor C'),
('Proveedor D'),
('Proveedor E'),
('Proveedor F'),
('Proveedor G'),
('Proveedor H'),
('Proveedor I'),
('Proveedor J'),
('Proveedor K'),
('Proveedor L'),
('Proveedor M'),
('Proveedor N'),
('Proveedor O'),
('Proveedor P'),
('Proveedor Q'),
('Proveedor R'),
('Proveedor S'),
('Proveedor T'),
('Proveedor U'),
('Proveedor V'),
('Proveedor W'),
('Proveedor X'),
('Proveedor Y'),
('Proveedor Z');


INSERT INTO ContactoProveedores (proveedor_id, nombre_contacto, telefono, email) VALUES 
(1, 'Laura Velasco', '555-3001', 'lauravelasco@example.com'),
(2, 'José Armando', '555-3002', 'josearmando@example.com'),
(3, 'Patricia Castro', '555-3003', 'patriciacastro@example.com'),
(4, 'Carlos Hernández', '555-3004', 'carloshernandez@example.com'),
(5, 'Manuela Morales', '555-3005', 'manuelamorales@example.com'),
(6, 'Fernando Ruiz', '555-3006', 'fernandoruiz@example.com'),
(7, 'Julia Salas', '555-3007', 'juliasalas@example.com'),
(8, 'Oscar Mendez', '555-3008', 'oscarmendez@example.com'),
(9, 'Cristela Castillo', '555-3009', 'cristelacastillo@example.com'),
(10, 'Sergio Gutiérrez', '555-3010', 'sergiogutierrez@example.com'),
(11, 'Veronique Martinez', '555-3011', 'veroniquemartinez@example.com'),
(12, 'Carina Espinoza', '555-3012', 'carinaespinoza@example.com'),
(13, 'Marta Ruiz', '555-3013', 'martaruiz@example.com'),
(14, 'Salvador Donoso', '555-3014', 'salvadordonoso@example.com'),
(15, 'Claudio Nieto', '555-3015', 'claudionieto@example.com'),
(16, 'Araceli Cuadra', '555-3016', 'aracelicuadra@example.com'),
(17, 'Samuel Rojas', '555-3017', 'samuelrojas@example.com'),
(18, 'Martina Álvarez', '555-3018', 'martinaalvarez@example.com'),
(19, 'Adriana Medina', '555-3019', 'adrianamedina@example.com'),
(20, 'Marlon Rivera', '555-3020', 'marlonrivera@example.com'),
(21, 'Jaime Torres', '555-3021', 'jaimetorres@example.com'),
(22, 'Alma Pinedo', '555-3022', 'almapinedo@example.com'),
(23, 'Elias Pereira', '555-3023', 'eliaspereira@example.com'),
(24, 'Diana Oliva', '555-3024', 'dianaoliva@example.com'),
(25, 'Jacinto Flores', '555-3025', 'jacintoflores@example.com');


INSERT INTO TiposProductos (tipo_nombre, descripcion, tipo_padre_id) VALUES 
('Electrónica', 'Productos electrónicos y gadgets', NULL),
('Ropa', 'Prendas de vestir y accesorios', NULL),
('Alimentos', 'Alimentos y bebidas', NULL),
('Muebles', 'Muebles y decoración del hogar', NULL),
('Juguetes', 'Artículos para niños y juegos', NULL),
('Herramientas', 'Herramientas para el hogar y trabajo', NULL),
('Deportes', 'Artículos deportivos', NULL),
('Belleza', 'Productos de cuidado personal y belleza', NULL),
('Salud', 'Suplementos y productos de salud', NULL),
('Libros', 'Literatura y material educativo', NULL),
('Computadoras', 'Computadoras de escritorio y portátiles', NULL),
('Accesorios', 'Accesorios de computación y multimedia', NULL),
('Automotriz', 'Productos para automóviles y motocicletas', NULL),
('Jardinería', 'Artículos de jardinería y plantas', NULL),
('Caza y pesca', 'Artículos para caza y pesca', NULL),
('Artículos de oficina', 'Suministros y equipos de oficina', NULL),
('Arte', 'Materiales de arte y manualidades', NULL),
('Cocina', 'Utensilios y electrodomésticos de cocina', NULL),
('Construcción', 'Materiales para construcción', NULL),
('Viajes', 'Artículos relacionados con viajes', NULL),
('Tecnología', 'Dispositivos tecnológicos modernos', NULL),
('Fotografía', 'Cámaras y accesorios de fotografía', NULL),
('Relojería', 'Relojes y accesorios de tiempo', NULL),
('Experiencias', 'Paquetes turísticos y experiencias', NULL),
('Servicios', 'Servicios y suscripciones', NULL),
('Sustentabilidad', 'Productos ecológicos y sustentables', NULL);


INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES 
('Smartphone XYZ', 699.99, 1, 1),
('Camiseta de algodón', 25.00, 2, 2),
('Agua embotellada', 1.50, 3, 3),
('Sofá moderno', 1200.00, 4, 4),
('Juguete de construcción', 30.00, 5, 5),
('Taladro inalámbrico', 70.00, 6, 6),
('Pelota de fútbol', 25.00, 7, 7),
('Crema hidratante', 15.00, 8, 8),
('Vitaminas C', 10.00, 9, 9),
('Novela de ficción', 20.00, 10, 10),
('Laptop Pro 15', 999.00, 11, 11),
('Ratón ergonómico', 35.00, 12, 12),
('Aceite de motor', 40.00, 13, 13),
('Tijeras de podar', 25.00, 14, 14),
('Bolsas de deporte', 45.00, 15, 15),
('Pinceles de arte', 12.50, 16, 16),
('Cuchillos de cocina', 50.00, 17, 17),
('Herramienta multifuncional', 60.00, 18, 18),
('Baúl de juguetes', 150.00, 19, 19),
('Pasaporte de viajes', 0.00, 20, 20),
('Cámara DSLR', 1200.00, 21, 21),
('Reloj inteligente', 250.00, 22, 22),
('Experiencia de cata', 80.00, 23, 23),
('Suscripción a un servicio de streaming', 10.00, 24, 24),
('Set de productos ecológicos', 115.00, 25, 25);


INSERT INTO Pedidos (cliente_id, fecha, total) VALUES 
(1, '2023-01-10', 800.00),
(2, '2023-01-15', 250.00),
(3, '2023-02-10', 1000.00),
(4, '2023-03-10', 550.00),
(5, '2023-03-15', 300.00),
(6, '2023-04-20', 150.00),
(7, '2023-05-25', 600.00),
(8, '2023-06-01', 450.00),
(9, '2023-06-15', 700.00),
(10, '2023-07-01', 900.00),
(11, '2023-07-30', 650.00),
(12, '2023-08-15', 800.00),
(13, '2023-09-01', 500.00),
(14, '2023-09-20', 300.00),
(15, '2023-10-01', 1200.00),
(16, '2023-10-15', 100.00),
(17, '2023-11-01', 750.00),
(18, '2023-11-15', 600.00),
(19, '2023-12-10', 1100.00),
(20, '2023-12-20', 900.00),
(21, '2023-12-30', 750.00),
(22, '2023-11-11', 300.00),
(23, '2023-10-20', 400.00),
(24, '2023-09-10', 800.00),
(25, '2023-08-10', 150.00);


INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad, precio_unitario) VALUES 
(1, 1, 2, 699.99),
(1, 2, 1, 25.00),
(2, 3, 5, 1.50),
(3, 4, 1, 1200.00),
(3, 1, 1, 699.99),
(4, 5, 3, 30.00),
(5, 6, 2, 70.00),
(6, 7, 1, 25.00),
(7, 8, 3, 15.00),
(8, 9, 2, 10.00),
(9, 10, 5, 20.00),
(10, 11, 1, 999.00),
(11, 12, 4, 35.00),
(12, 13, 2, 40.00),
(13, 14, 3, 25.00),
(14, 15, 1, 45.00),
(15, 16, 2, 12.50),
(16, 17, 4, 50.00),
(17, 18, 1, 60.00),
(18, 19, 1, 150.00),
(19, 20, 10, 0.00),
(20, 21, 1, 1200.00),
(21, 22, 3, 250.00),
(22, 23, 1, 80.00),
(23, 24, 2, 10.00),
(24, 25, 2, 115.00);


INSERT INTO HistorialPedidos (pedido_id, fecha_modificacion, accion, detalles_cambio) VALUES 
(1, NOW(), 'Creado', 'Pedido inicial creado'),
(2, NOW(), 'Creado', 'Pedido inicial creado'),
(3, NOW(), 'Creado', 'Pedido inicial creado'),
(4, NOW(), 'Creado', 'Pedido inicial creado'),
(5, NOW(), 'Actualizado', 'Se modificó la cantidad de productos'),
(6, NOW(), 'Creado', 'Pedido inicial creado'),
(7, NOW(), 'Creado', 'Pedido inicial creado'),
(8, NOW(), 'Creado', 'Pedido inicial creado'),
(9, NOW(), 'Actualizado', 'Se agregaron más productos al pedido'),
(10, NOW(), 'Creado', 'Pedido inicial creado'),
(11, NOW(), 'Creado', 'Pedido inicial creado'),
(12, NOW(), 'Creado', 'Pedido inicial creado'),
(13, NOW(), 'Creado', 'Pedido inicial creado'),
(14, NOW(), 'Creado', 'Pedido inicial creado'),
(15, NOW(), 'Creado', 'Pedido inicial creado'),
(16, NOW(), 'Creado', 'Pedido inicial creado'),
(17, NOW(), 'Creado', 'Pedido inicial creado'),
(18, NOW(), 'Creado', 'Pedido inicial creado'),
(19, NOW(), 'Creado', 'Pedido inicial creado'),
(20, NOW(), 'Creado', 'Pedido inicial creado'),
(21, NOW(), 'Creado', 'Pedido inicial creado'),
(22, NOW(), 'Actualizado', 'Se realizó un cambio en el estado del pedido'),
(23, NOW(), 'Eliminado', 'Pedido cancelado por el cliente'),
(24, NOW(), 'Creado', 'Pedido inicial creado'),
(25, NOW(), 'Creado', 'Pedido inicial creado');

INSERT INTO EmpleadosProveedores (empleado_id, proveedor_id) VALUES 
('Juan Carlos Pérez', 1),  
('María Fernanda López', 2),
('Carlos Alberto Ramírez', 3),
('Ana María Torres', 4,),
('Luis Miguel González', 5,),
('Cristina Rodríguez', 1),
('José Luis Martínez', 2),
('Laura Jiménez', 3),
('Manuel Alejandro Cruz', 4),
('Sofía Ramos', 5),
('Diego Hernández', 1),
('Patricia González', 2),
('Fernando Ortega', 3),
('Elena Ruiz', 4),
('Ricardo Sinquefield', 5), 
('Claudia Navarrete', 1), 
('Marisol Silva', 2),
('Jonathan Castro', 3),
('Gabriela Bravo', 4), 
('Francisco Salinas', 5), 
('Alicia Medina', 1), 
('Francisco Benitez', 2), 
('Luis Cuervo', 3), 
('Natalia Flores', 4), 
('Rodrigo De Souza', 5); 

   SELECT 
       Pedidos.id AS pedido_id,
       Pedidos.fecha AS fecha_pedido,
       Pedidos.total AS total_pedido,
       Clientes.nombre AS nombre_cliente
   FROM 
       Pedidos
   INNER JOIN 
       Clientes ON Pedidos.cliente_id = Clientes.id;


​       


   SELECT 
       Productos.id AS producto_id,
       Productos.nombre AS nombre_producto,
       Productos.precio AS precio_producto,
       Proveedores.nombre AS nombre_proveedor
   FROM 
       Productos
   INNER JOIN 
       Proveedores ON Productos.proveedor_id = Proveedores.id;
   



   SELECT 
       Pedidos.id AS pedido_id,
       Pedidos.fecha AS fecha_pedido,
       Pedidos.total AS total_pedido,
       Clientes.nombre AS nombre_cliente,
       Ubicaciones.direccion AS direccion_cliente,
       Ubicaciones.ciudad AS ciudad_cliente,
       Ubicaciones.estado AS estado_cliente,
       Ubicaciones.codigo_postal AS codigo_postal_cliente,
       Ubicaciones.pais AS pais_cliente
   FROM 
       Pedidos
   INNER JOIN 
       Clientes ON Pedidos.cliente_id = Clientes.id
   LEFT JOIN 
       Ubicaciones ON Clientes.id = Ubicaciones.entidad_id AND Ubicaciones.tipo_entidad = 'Cliente';
   


   SELECT 
       Empleados.id AS empleado_id,
       Empleados.nombre AS nombre_empleado,
       Pedidos.id AS pedido_id,
       Pedidos.fecha AS fecha_pedido,
       Pedidos.total AS total_pedido
   FROM 
       Empleados
   LEFT JOIN 
       Clientes ON Clientes.id = Empleados.id  -- Esta línea supone que hay alguna relación que conecta empleados con clientes
   LEFT JOIN 
       Pedidos ON Pedidos.cliente_id = Clientes.id;
 
   SELECT 
       TiposProductos.tipo_nombre AS tipoproductos,
       Productos.nombre AS producto
   FROM 
       TiposProductos
   INNER JOIN 
       Productos ON TiposProductos.id = Productos.tipo_id;
   
   SELECT 
       Clientes.nombre AS nombre_cliente,
       COUNT(Pedidos.id) AS numero_de_pedidos
   FROM 
       Clientes
   LEFT JOIN 
       Pedidos ON Clientes.id = Pedidos.cliente_id
   GROUP BY 
       Clientes.id, Clientes.nombre;
   
   ALTER TABLE Pedidos ADD empleado_id INT;
   
   ALTER TABLE Pedidos ADD FOREIGN KEY (empleado_id) REFERENCES Empleados(id);
 
   SELECT
       Empleados.id AS empleado_id,
       Empleados.nombre AS nombre_empleado,
       Pedidos.id AS pedido_id,
       Pedidos.fecha AS fecha_pedido,
       Pedidos.total AS total_pedido
   FROM
       Pedidos
   LEFT JOIN
       Empleados ON Pedidos.empleado_id = Empleados.id;  -- Ahora esto funcionará
  
   SELECT 
       Productos.id AS producto_id,
       Productos.nombre AS nombre_producto,
       Productos.precio AS precio_producto
   FROM 
       Productos
   LEFT JOIN 
       DetallesPedido ON Productos.id = DetallesPedido.producto_id
   WHERE 
       DetallesPedido.id IS NULL;  -- Filtramos para obtener solo productos sin pedidos
  
   SELECT 
       Clientes.id AS cliente_id,
       Clientes.nombre AS nombre_cliente,
       COUNT(Pedidos.id) AS total_pedidos,
       Ubicaciones.direccion AS direccion_cliente,
       Ubicaciones.ciudad AS ciudad_cliente,
       Ubicaciones.estado AS estado_cliente,
       Ubicaciones.codigo_postal AS codigo_postal_cliente,
       Ubicaciones.pais AS pais_cliente
   FROM 
       Clientes
   LEFT JOIN 
       Pedidos ON Clientes.id = Pedidos.cliente_id
   LEFT JOIN 
       Ubicaciones ON Clientes.id = Ubicaciones.entidad_id AND Ubicaciones.tipo_entidad = 'Cliente'
   GROUP BY 
       Clientes.id, 
       Clientes.nombre, 
       Ubicaciones.direccion,
       Ubicaciones.ciudad,
       Ubicaciones.estado,
       Ubicaciones.codigo_postal,
       Ubicaciones.pais;

    SELECT 
        Productos.id AS producto_id,
        Productos.nombre AS nombre_producto,
        Productos.precio AS precio_producto,
        Proveedores.id AS proveedor_id,
        Proveedores.nombre AS nombre_proveedor,
        TiposProductos.tipo_nombre AS tipo_producto
    FROM 
        Productos
    INNER JOIN 
        Proveedores ON Productos.proveedor_id = Proveedores.id
    INNER JOIN 
        TiposProductos ON Productos.tipo_id = TiposProductos.id;
 
   SELECT * 
   FROM Productos 
   WHERE Precio > 50;
  
   SELECT 
       Clientes.id AS cliente_id,
       Clientes.nombre AS nombre_cliente
   FROM 
       Clientes
   JOIN 
       Ubicaciones ON Clientes.id = Ubicaciones.entidad_id
   WHERE 
       Ubicaciones.ciudad = 'NombreDeLaCiudad';  -- Reemplaza 'NombreDeLaCiudad' por la ciudad que deseas consultar

   SELECT 
       id AS empleado_id,
       nombre AS nombre_empleado,
       fecha_contratacion
   FROM 
       Empleados
   WHERE 
       fecha_contratacion >= DATE_SUB(CURRENT_DATE, INTERVAL 2 YEAR);
  
   SELECT 
       Proveedores.id AS proveedor_id,
       Proveedores.nombre AS nombre_proveedor,
       COUNT(Productos.id) AS numero_de_productos
   FROM 
       Proveedores
   JOIN 
       Productos ON Proveedores.id = Productos.proveedor_id
   GROUP BY 
       Proveedores.id, Proveedores.nombre
   HAVING 
       COUNT(Productos.id) > 5;
 
   SELECT 
       Clientes.id AS cliente_id,
       Clientes.nombre AS nombre_cliente,
       Clientes.email AS email_cliente
   FROM 
       Clientes
   LEFT JOIN 
       Ubicaciones ON Clientes.id = Ubicaciones.entidad_id AND Ubicaciones.tipo_entidad = 'Cliente'
   WHERE 
       Ubicaciones.id IS NULL;  -- Filtra aquellos clientes que no tienen dirección registrada
  
   SELECT 
       Clientes.id AS cliente_id,
       Clientes.nombre AS nombre_cliente,
       SUM(Pedidos.total) AS total_ventas
   FROM 
       Clientes
   LEFT JOIN 
       Pedidos ON Clientes.id = Pedidos.cliente_id
   GROUP BY 
       Clientes.id, 
       Clientes.nombre;
  
   ALTER TABLE Empleados
   ADD COLUMN salario DECIMAL(10, 2);
   
   UPDATE Empleados
   SET salario = 60000.00
   WHERE id = 1;  -- Actualiza el salario del empleado con id 1
   
   UPDATE Empleados
   SET salario = CASE 
       WHEN id = 1 THEN 60000.00
       WHEN id = 2 THEN 50000.00
       WHEN id = 3 THEN 45000.00
       WHEN id = 4 THEN 70000.00
       ELSE salario  -- No hace nada si no coincide
   END;
 
   SELECT 
       id AS empleado_id,
       nombre AS nombre_empleado,
       salario
   FROM 
       Empleados;
   
   SELECT 
       id AS tipo_producto_id,
       tipo_nombre AS nombre_tipo_producto
   FROM 
       TiposProductos;
  
   SELECT 
       id AS producto_id,
       nombre AS nombre_producto,
       precio AS precio_producto
   FROM 
       Productos
   ORDER BY 
       precio DESC
   LIMIT 3;
  
    SELECT 
        Clientes.id AS cliente_id,
        Clientes.nombre AS nombre_cliente,
        COUNT(Pedidos.id) AS total_pedidos
    FROM 
        Clientes
    JOIN 
        Pedidos ON Clientes.id = Pedidos.cliente_id
    GROUP BY 
        Clientes.id, Clientes.nombre
    ORDER BY 
        total_pedidos DESC
    LIMIT 1;

   SELECT 
       Pedidos.id AS pedido_id,
       Pedidos.fecha AS fecha_pedido,
       Pedidos.total AS total_pedido,
       Clientes.id AS cliente_id,
       Clientes.nombre AS nombre_cliente,
       Clientes.email AS email_cliente
   FROM 
       Pedidos
   JOIN 
       Clientes ON Pedidos.cliente_id = Clientes.id;
  
   SELECT 
       Pedidos.id AS pedido_id,
       Pedidos.fecha AS fecha_pedido,
       Pedidos.total AS total_pedido,
       Clientes.id AS cliente_id,
       Clientes.nombre AS nombre_cliente,
       Ubicaciones.direccion AS direccion_cliente,
       Ubicaciones.ciudad AS ciudad_cliente,
       Ubicaciones.estado AS estado_cliente,
       Ubicaciones.codigo_postal AS codigo_postal_cliente,
       Ubicaciones.pais AS pais_cliente
   FROM 
       Pedidos
   JOIN 
       Clientes ON Pedidos.cliente_id = Clientes.id
   JOIN 
       Ubicaciones ON Clientes.id = Ubicaciones.entidad_id AND Ubicaciones.tipo_entidad = 'Cliente';

   SELECT 
       Productos.id AS producto_id,
       Productos.nombre AS nombre_producto,
       Productos.precio AS precio_producto,
       Proveedores.id AS proveedor_id,
       Proveedores.nombre AS nombre_proveedor,
       TiposProductos.tipo_nombre AS tipo_producto
   FROM 
       Productos
   JOIN 
       Proveedores ON Productos.proveedor_id = Proveedores.id
   JOIN 
       TiposProductos ON Productos.tipo_id = TiposProductos.id;
    

SELECT 
    Empleados.id AS empleado_id,
    Empleados.nombre AS nombre_empleado,
    Clientes.id AS cliente_id,
    Clientes.nombre AS nombre_cliente,
    Ubicaciones.direccion AS direccion_cliente,
    Ubicaciones.ciudad AS ciudad_cliente
FROM 
    Empleados
JOIN 
    Clientes ON Empleados.id = Clientes.id  -- Asegúrate de que esta relación tenga sentido en tu modelo
JOIN 
    Pedidos ON Clientes.id = Pedidos.cliente_id
JOIN 
    Ubicaciones ON Clientes.id = Ubicaciones.entidad_id 
WHERE 
    Ubicaciones.tipo_entidad = 'Cliente' AND 
    Ubicaciones.ciudad = 'NombreDeLaCiudad';  -- Cambia 'NombreDeLaCiudad' por la ciudad de interés

SELECT 
    Productos.id AS producto_id,
    Productos.nombre AS nombre_producto,
    SUM(DetallesPedido.cantidad) AS total_vendidos
FROM 
    Productos
JOIN 
    DetallesPedido ON Productos.id = DetallesPedido.producto_id
GROUP BY 
    Productos.id, Productos.nombre
ORDER BY 
    total_vendidos DESC
LIMIT 5;

   SELECT 
       Clientes.id AS cliente_id,
       Clientes.nombre AS nombre_cliente,
       Ubicaciones.ciudad AS ciudad_cliente,
       COUNT(Pedidos.id) AS total_pedidos
   FROM 
       Clientes
   LEFT JOIN 
       Pedidos ON Clientes.id = Pedidos.cliente_id
   LEFT JOIN 
       Ubicaciones ON Clientes.id = Ubicaciones.entidad_id AND Ubicaciones.tipo_entidad = 'Cliente'
   GROUP BY 
       Clientes.id, 
       Clientes.nombre, 
       Ubicaciones.ciudad;
  
   SELECT 
       c.id AS cliente_id,
       c.nombre AS nombre_cliente,
       p.id AS proveedor_id,
       p.nombre AS nombre_proveedor,
       uc.ciudad
   FROM 
       Clientes c
   JOIN 
       Ubicaciones uc ON c.id = uc.entidad_id AND uc.tipo_entidad = 'Cliente'
   JOIN 
       Proveedores p ON EXISTS (
           SELECT 1
           FROM Ubicaciones up
           WHERE p.id = up.entidad_id AND up.tipo_entidad = 'Proveedor' AND up.ciudad = uc.ciudad
       );
   
   SELECT 
       TiposProductos.id AS tipo_producto_id,
       TiposProductos.tipo_nombre AS nombre_tipo_producto,
       SUM(DetallesPedido.cantidad * Productos.precio) AS total_ventas
   FROM 
       DetallesPedido
   JOIN 
       Productos ON DetallesPedido.producto_id = Productos.id
   JOIN 
       TiposProductos ON Productos.tipo_id = TiposProductos.id
   GROUP BY 
       TiposProductos.id, TiposProductos.tipo_nombre;
   

   


   SELECT 
       Empleados.id AS empleado_id,
       Empleados.nombre AS nombre_empleado,
       Proveedores.id AS proveedor_id,
       Proveedores.nombre AS nombre_proveedor,
       Pedidos.id AS pedido_id,
       Pedidos.fecha AS fecha_pedido,
       Pedidos.total AS total_pedido
   FROM 
       Empleados
   JOIN 
       Pedidos ON Empleados.id = Pedidos.empleado_id  -- Asumiendo que hay una relación directa entre empleados y pedidos
   JOIN 
       DetallesPedido ON Pedidos.id = DetallesPedido.pedido_id
   JOIN 
       Productos ON DetallesPedido.producto_id = Productos.id
   JOIN 
       Proveedores ON Productos.proveedor_id = Proveedores.id
   WHERE 
       Proveedores.nombre = 'NombreDelProveedor';  -- Cambiar por el proveedor específico que deseas filtrar
  
    SELECT 
        Proveedores.id AS proveedor_id,
        Proveedores.nombre AS nombre_proveedor,
        SUM(DetallesPedido.cantidad * Productos.precio) AS ingreso_total
    FROM 
        Proveedores
    JOIN 
        Productos ON Proveedores.id = Productos.proveedor_id
    JOIN 
        DetallesPedido ON Productos.id = DetallesPedido.producto_id
    JOIN 
        Pedidos ON DetallesPedido.pedido_id = Pedidos.id
    GROUP BY 
        Proveedores.id, Proveedores.nombre;

   SELECT 
       p.id AS producto_id,
       p.nombre AS nombre_producto,
       p.precio AS precio_producto,
       tp.id AS tipo_producto_id,
       tp.tipo_nombre AS nombre_tipo_producto
   FROM 
       Productos p
   JOIN 
       TiposProductos tp ON p.tipo_id = tp.id
   WHERE 
       p.precio = (
           SELECT 
               MAX(sub_producto.precio)
           FROM 
               Productos sub_producto
           WHERE 
               sub_producto.tipo_id = p.tipo_id
       );
  
SELECT 
    Clientes.id AS cliente_id,
    Clientes.nombre AS nombre_cliente,
    SUM(Pedidos.total) AS total_pedidos
FROM 
    Clientes
JOIN 
    Pedidos ON Clientes.id = Pedidos.cliente_id
GROUP BY 
    Clientes.id, Clientes.nombre
ORDER BY 
    total_pedidos DESC
LIMIT 1;



ALTER TABLE Empleados
ADD COLUMN salario_base DECIMAL(10, 2);  -- Asegúrate de configurar el tipo de dato según sea necesario

UPDATE Empleados
SET salario_base = 60000.00
WHERE id = 1;  -- Actualiza el salario del empleado con id 1


UPDATE Empleados
SET salario_base = CASE 
    WHEN id = 1 THEN 60000.00
    WHEN id = 2 THEN 65000.00
    WHEN id = 3 THEN 55000.00
    ELSE salario_base  -- No cambia el salario base si no coincide
END;

SELECT 
    id AS empleado_id,
    nombre AS nombre_empleado,
    salario_base
FROM 
    Empleados;


SELECT 
    id AS empleado_id,
    nombre AS nombre_empleado,
    salario_base
FROM 
    Empleados;

SELECT 
    Productos.id AS producto_id,
    Productos.nombre AS nombre_producto,
    COUNT(DetallesPedido.id) AS total_pedidos
FROM 
    Productos
JOIN 
    DetallesPedido ON Productos.id = DetallesPedido.producto_id
GROUP BY 
    Productos.id, Productos.nombre
HAVING 
    COUNT(DetallesPedido.id) > 5;


SELECT 
    Pedidos.id AS pedido_id,
    Pedidos.fecha AS fecha_pedido,
    Pedidos.total AS total_pedido
FROM 
    Pedidos
WHERE 
    Pedidos.total > (SELECT AVG(total) FROM Pedidos);


SELECT 
    Proveedores.id AS proveedor_id,
    Proveedores.nombre AS nombre_proveedor,
    COUNT(Productos.id) AS total_productos
FROM 
    Proveedores
JOIN 
    Productos ON Proveedores.id = Productos.proveedor_id
GROUP BY 
    Proveedores.id, Proveedores.nombre
ORDER BY 
    total_productos DESC
LIMIT 3;


SELECT 
    p.id AS producto_id,
    p.nombre AS nombre_producto,
    p.precio AS precio_producto,
    tp.id AS tipo_producto_id,
    tp.tipo_nombre AS nombre_tipo_producto
FROM 
    Productos p
JOIN 
    TiposProductos tp ON p.tipo_id = tp.id
WHERE 
    p.precio > (
        SELECT AVG(p2.precio)
        FROM Productos p2
        WHERE p2.tipo_id = p.tipo_id
    );

SELECT 
    Clientes.id AS cliente_id,
    Clientes.nombre AS nombre_cliente,
    COUNT(Pedidos.id) AS total_pedidos
FROM 
    Clientes
JOIN 
    Pedidos ON Clientes.id = Pedidos.cliente_id
GROUP BY 
    Clientes.id, Clientes.nombre
HAVING 
    COUNT(Pedidos.id) > (SELECT AVG(pedido_count) FROM (
        SELECT COUNT(id) AS pedido_count
        FROM Pedidos
        GROUP BY cliente_id
    ) AS promedio);


SELECT 
    id AS producto_id,
    nombre AS nombre_producto,
    precio AS precio_producto
FROM 
    Productos
WHERE 
    precio > (SELECT AVG(precio) FROM Productos);


ALTER TABLE Empleados
ADD COLUMN departamento_id INT;  -- Cambia INT por el tipo de dato que desees si es diferente

UPDATE Empleados
SET departamento_id = 1  -- Cambia 1 por el ID correspondiente del departamento
WHERE id = 1;            -- Cambia 1 por el ID del empleado

UPDATE Empleados
SET departamento_id = CASE 
    WHEN id = 1 THEN 1  -- Juan Pérez en el departamento 1
    WHEN id = 2 THEN 2  -- María López en el departamento 2
    WHEN id = 3 THEN 1  -- Carlos García en el departamento 1
    WHEN id = 4 THEN 3  -- Ana Martínez en el departamento 3
    ELSE departamento_id  -- No cambia si no coincide
END;

SELECT 
    id AS empleado_id,
    nombre AS nombre_empleado,
    salario_base,
    departamento_id
FROM 
    Empleados;

   DELIMITER //
   
   CREATE PROCEDURE ActualizarPrecioProductosProveedor(
       IN p_proveedor_id INT,
       IN p_nuevo_precio DECIMAL(10, 2)
   )
   BEGIN
       UPDATE Productos
       SET precio = p_nuevo_precio
       WHERE proveedor_id = p_proveedor_id;
   END //
   
   DELIMITER ;
   
 
   DELIMITER //
   
   CREATE PROCEDURE ObtenerDireccionCliente(
       IN p_cliente_id INT
   )
   BEGIN
       SELECT 
           U.direccion,
           U.ciudad,
           U.estado,
           U.codigo_postal,
           U.pais
       FROM 
           Clientes C
       JOIN 
           Ubicaciones U ON C.id = U.entidad_id
       WHERE 
           C.id = p_cliente_id AND 
           U.tipo_entidad = 'Cliente';
   END //
   
   DELIMITER ;

   DELIMITER //
   
   CREATE PROCEDURE RegistrarPedidoNuevo(
       IN p_cliente_id INT,
       IN p_fecha DATE,
       IN p_detalles TEXT  -- JSON o una cadena separada por comas, dependiendo de qué formato uses
   )
   BEGIN
       DECLARE new_pedido_id INT;
       DECLARE total DECIMAL(10, 2) DEFAULT 0.00;
   
       -- Insertar el nuevo pedido en la tabla Pedidos
       INSERT INTO Pedidos (cliente_id, fecha, total)
       VALUES (p_cliente_id, p_fecha, 0); -- Se coloca un total inicial de 0
   
       -- Obtener el ID del nuevo pedido
       SET new_pedido_id = LAST_INSERT_ID();
   
       -- Supongamos que p_detalles es un formato JSON que contiene producto_id y cantidad
       -- Por ejemplo: '[{"producto_id": 1, "cantidad": 2}, {"producto_id": 2, "cantidad": 3}]'
   
       -- Aquí debes implementar la lógica necesaria para descomponer la cadena
       -- o leer el JSON. Esto puede ser un poco complejo en MySQL sin funciones específicas:
       
       -- Este es un esqueleto, necesitabas un loop en caso de ser necesario, 
       -- aquí se usa un seudocódigo. Debes ajustar esto según tus requerimientos y el formato de p_detalles.
       
       -- Asumiendo que puedes descomponer el JSON y operar de alguna forma (esto podría requerir un UDF o lógica adicional)
       
       -- Ejemplo ficticio de una bucle hipotético:
       DECLARE done INT DEFAULT FALSE;
   
       -- Aquí deberás adecuar la lógica a tu modelo de trabajar con p_detalles.
       WHILE NOT done DO
           -- Este es un ejemplo seudocódigo
           -- Ejemplo: 
           -- SET product = JSON_EXTRACT(p_detalles, '$[index]');  -- Lógica para obtener los detalles.
           
           -- INSERTAR EN DETALLES
           INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad, precio_unitario)
           VALUES (new_pedido_id, producto_id, cantidad, precio_unitario);
           
           -- Actualizar el total acumulado
           SET total = total + (cantidad * precio_unitario);
           
           -- Continuar con el siguiente producto...
           -- Si hay un límite de detalles, puedes manejarlo con un contador o manejar la lógica para salir.
       END WHILE;
   
       -- Ahora, actualizar el total del pedido con el total acumulado
       UPDATE Pedidos
       SET total = total
       WHERE id = new_pedido_id;
   
   END //
   
   DELIMITER ;
   
   
  
   DELIMITER //
   
   CREATE PROCEDURE CalcularTotalVentasCliente(
       IN p_cliente_id INT,
       OUT p_total_ventas DECIMAL(10, 2)
   )
   BEGIN
       SELECT 
           SUM(Pedidos.total) INTO p_total_ventas
       FROM 
           Pedidos
       WHERE 
           Pedidos.cliente_id = p_cliente_id;
   END //
   
   DELIMITER ;
   
  
   DELIMITER //
   
   CREATE PROCEDURE ObtenerEmpleadosPorPuesto(
       IN p_puesto_id INT
   )
   BEGIN
       SELECT 
           E.id AS empleado_id,
           E.nombre AS nombre_empleado,
           P.nombre AS nombre_puesto
       FROM 
           Empleados E
       JOIN 
           Puestos P ON E.puesto_id = P.id
       WHERE 
           E.puesto_id = p_puesto_id;
   END //
   
   DELIMITER ;
   
   
   DELIMITER //
   
   CREATE PROCEDURE ActualizarSalarioPorPuesto(
       IN p_puesto_id INT,
       IN p_nuevo_salario DECIMAL(10, 2)
   )
   BEGIN
       UPDATE Empleados
       SET salario_base = p_nuevo_salario
       WHERE puesto_id = p_puesto_id;
   END //
   
   DELIMITER ;
   
   
   DELIMITER //
   
   CREATE PROCEDURE ListarPedidosEntreFechas(
       IN p_fecha_inicio DATE,
       IN p_fecha_fin DATE
   )
   BEGIN
       SELECT 
           id AS pedido_id,
           cliente_id,
           fecha,
           total
       FROM 
           Pedidos
       WHERE 
           fecha BETWEEN p_fecha_inicio AND p_fecha_fin;
   END //
   
   DELIMITER ;
   
  
   DELIMITER //
   
   CREATE PROCEDURE AplicarDescuentoPorCategoria(
       IN p_tipo_id INT,
       IN p_descuento_porcentaje DECIMAL(5, 2)  -- Por ejemplo, 10.00 para un 10% de descuento
   )
   BEGIN
       UPDATE Productos
       SET precio = precio * (1 - p_descuento_porcentaje / 100)
       WHERE tipo_id = p_tipo_id;
   END //
   
   DELIMITER ;
  
   DELIMITER //
   
   CREATE PROCEDURE ListarProveedoresPorTipoProducto(
       IN p_tipo_id INT
   )
   BEGIN
       SELECT 
           DISTINCT P.id AS proveedor_id,
           P.nombre AS nombre_proveedor
       FROM 
           Proveedores P
       JOIN 
           Productos Pr ON P.id = Pr.proveedor_id
       WHERE 
           Pr.tipo_id = p_tipo_id;
   END //
   
   DELIMITER ;
   
   DELIMITER //
   
   CREATE PROCEDURE ObtenerPedidoMayorValor()
   BEGIN
       SELECT 
           id AS pedido_id,
           cliente_id,
           fecha,
           total
       FROM 
           Pedidos
       ORDER BY 
           total DESC
       LIMIT 1;
   END //
   
   DELIMITER ;
   

   
   CREATE FUNCTION DiasTranscurridos(p_fecha DATE) 
   RETURNS INT
   DETERMINISTIC
   BEGIN
       DECLARE dias_transcurridos INT;
       
       SET dias_transcurridos = DATEDIFF(CURRENT_DATE, p_fecha);
       
       RETURN dias_transcurridos;
   END //
   
   DELIMITER ;
   
   
   DELIMITER $$
   CREATE FUNCTION calcular_total_con_impuesto(monto DECIMAL(10, 2), tasa_impuesto DECIMAL(5, 2))
   RETURNS DECIMAL(10, 2)
   DETERMINISTIC
   BEGIN
       RETURN monto * (1 + tasa_impuesto / 100);
   END $$
   DELIMITER ;
   

   DELIMITER //
   
   CREATE FUNCTION DiasTranscurridos(p_fecha DATE) 
   RETURNS INT
   DETERMINISTIC
   BEGIN
       DECLARE dias_transcurridos INT;
       
       SET dias_transcurridos = DATEDIFF(CURRENT_DATE, p_fecha);
       
       RETURN dias_transcurridos;
   END //
   
   DELIMITER ;
 
   DELIMITER //
   
   CREATE FUNCTION AplicarDescuento(
       p_producto_id INT,
       p_descuento_porcentaje DECIMAL(5, 2)  -- Por ejemplo, 10.00 para un 10% de descuento
   )
   RETURNS DECIMAL(10, 2)
   DETERMINISTIC
   BEGIN
       DECLARE nuevo_precio DECIMAL(10, 2);
   
       -- Obtener el precio actual del producto
       SELECT precio INTO nuevo_precio
       FROM Productos
       WHERE id = p_producto_id;
   
       -- Calcular el nuevo precio después de aplicar el descuento
       SET nuevo_precio = nuevo_precio * (1 - p_descuento_porcentaje / 100);
   
       RETURN nuevo_precio;  -- Retornar el nuevo precio
   END //
   
   DELIMITER ;
   

   DELIMITER $$
   CREATE FUNCTION tiene_direccion(cliente_id INT)
   RETURNS BOOLEAN
   DETERMINISTIC
   BEGIN
       RETURN (SELECT CASE WHEN direccion IS NOT NULL THEN TRUE ELSE FALSE END
               FROM clientes
               WHERE id_cliente = cliente_id);
   END $$
   DELIMITER ;
   
 
   DELIMITER $$
   CREATE FUNCTION salario_anual(salario_mensual DECIMAL(10, 2))
   RETURNS DECIMAL(10, 2)
   DETERMINISTIC
   BEGIN
       RETURN salario_mensual * 12;
   END $$
   DELIMITER ;
   
 
   DELIMITER //
   
   CREATE FUNCTION CalcularTotalVentasPorTipo(
       p_tipo_id INT
   ) 
   RETURNS DECIMAL(10, 2)
   DETERMINISTIC
   BEGIN
       DECLARE total_ventas DECIMAL(10, 2);
   
       SELECT 
           SUM(DP.cantidad * P.precio) INTO total_ventas
       FROM 
           DetallesPedido DP
       JOIN 
           Productos P ON DP.producto_id = P.id
       WHERE 
           P.tipo_id = p_tipo_id;
   
       RETURN IFNULL(total_ventas, 0);  -- Devuelve 0 si no hay ventas
   END //
   
   DELIMITER ;
   
 
   DELIMITER $$
   CREATE FUNCTION nombre_cliente(cliente_id INT)
   RETURNS VARCHAR(100)
   DETERMINISTIC
   BEGIN
       RETURN (SELECT nombre 
               FROM clientes 
               WHERE id_cliente = cliente_id);
   END $$
   DELIMITER ;
   

   DELIMITER //
   
   CREATE FUNCTION ObtenerNombreClientePorID(
       p_cliente_id INT
   ) 
   RETURNS VARCHAR(100)
   DETERMINISTIC
   BEGIN
       DECLARE nombre_cliente VARCHAR(100);
   
       SELECT nombre INTO nombre_cliente
       FROM Clientes
       WHERE id = p_cliente_id;
   
       RETURN IFNULL(nombre_cliente, 'Cliente no encontrado');  -- Retorna un mensaje si no se encuentra el cliente
   END //
   
   DELIMITER ;
 
    DELIMITER //
    
    CREATE FUNCTION EstaEnInventario(
        p_producto_id INT
    ) 
    RETURNS BOOLEAN
    DETERMINISTIC
    BEGIN
        DECLARE producto_en_inventario BOOLEAN;
    
        SELECT
            CASE
                WHEN cantidad_en_inventario > 0 THEN TRUE
                ELSE FALSE
            END INTO producto_en_inventario
        FROM 
            Productos
        WHERE 
            id = p_producto_id;
    
        RETURN IFNULL(producto_en_inventario, FALSE);  -- Retorna FALSE si el producto no se encuentra
    END //
    
    DELIMITER ;
    

   DELIMITER //
   
   CREATE TRIGGER RegistroCambioSalario
   AFTER UPDATE ON Empleados
   FOR EACH ROW
   BEGIN
       IF OLD.salario_base <> NEW.salario_base THEN
           INSERT INTO HistorialSalarios (empleado_id, salario_anterior, salario_nuevo)
           VALUES (NEW.id, OLD.salario_base, NEW.salario_base);
       END IF;
   END //
   
   DELIMITER ;

   DELIMITER //
   
   CREATE TRIGGER EvitarBorrarProducto
   BEFORE DELETE ON Productos
   FOR EACH ROW
   BEGIN
       DECLARE productoCuenta INT;
   
       SELECT COUNT(*) INTO productoCuenta
       FROM DetallesPedido
       WHERE producto_id = OLD.id;
   
       IF productoCuenta > 0 THEN
           SIGNAL SQLSTATE '45000'
           SET MESSAGE_TEXT = 'No se puede borrar el producto porque tiene pedidos activos.';
       END IF;
   END //
   
   DELIMITER ;
 
   DELIMITER //
   
   CREATE TRIGGER RegistroHistorialPedidos
   AFTER UPDATE ON Pedidos
   FOR EACH ROW
   BEGIN
       -- Insertar registro en HistorialPedidos
       INSERT INTO HistorialPedidos (pedido_id, total_anterior, total_nuevo)
       VALUES (OLD.id, OLD.total, NEW.total);
   END //
   
   DELIMITER ;
   
 
    
   DELIMITER //
   
   CREATE TRIGGER ActualizarInventario
   AFTER INSERT ON DetallesPedido
   FOR EACH ROW
   BEGIN
       UPDATE Productos
       SET cantidad_en_inventario = cantidad_en_inventario - NEW.cantidad
       WHERE id = NEW.producto_id;
   END //
   
   DELIMITER ;
 
   DELIMITER //
   
   CREATE TRIGGER EvitarActualizarPrecio
   BEFORE UPDATE ON Productos
   FOR EACH ROW
   BEGIN
       IF NEW.precio < 1 THEN
           SIGNAL SQLSTATE '45000'
           SET MESSAGE_TEXT = 'No se puede actualizar el precio a menos de $1.';
       END IF;
   END //
   
   DELIMITER ;
   

   DELIMITER //
   
   CREATE TRIGGER RegistroHistorialPedido
   AFTER INSERT ON Pedidos
   FOR EACH ROW
   BEGIN
       INSERT INTO HistorialPedidos (pedido_id, fecha_creacion)
       VALUES (NEW.id, NEW.fecha);
   END //
   
   DELIMITER ;
 
   DELIMITER //
   
   CREATE TRIGGER ActualizarTotalPedido
   AFTER INSERT ON DetallesPedido
   FOR EACH ROW
   BEGIN
       DECLARE nuevo_total DECIMAL(10, 2);
   
       -- Sumar el total de los productos para el pedido
       SELECT SUM(cantidad * precio_unitario) INTO nuevo_total
       FROM DetallesPedido
       WHERE pedido_id = NEW.pedido_id;
   
       -- Actualizar el total en la tabla Pedidos
       UPDATE Pedidos
       SET total = nuevo_total
       WHERE id = NEW.pedido_id;
   END //
   
   DELIMITER ;
  
   DELIMITER //
   
   CREATE TRIGGER ValidarUbicacionCliente
   BEFORE INSERT ON Clientes
   FOR EACH ROW
   BEGIN
       DECLARE ubicaciones_count INT;
   
       -- Contar las ubicaciones para el cliente
       SELECT COUNT(*) INTO ubicaciones_count
       FROM UbicacionCliente
       WHERE cliente_id = NEW.id;  -- Comprobamos si el cliente ya tiene ubicaciones registradas
   
       -- Si no hay ubicaciones, lanzamos un error
       IF ubicaciones_count = 0 THEN
           SIGNAL SQLSTATE '45000'
           SET MESSAGE_TEXT = 'Error: El cliente necesita al menos una ubicación registrada.';
       END IF;
   END //
   
   DELIMITER ;
  

   
   DELIMITER //
   
   CREATE TRIGGER RegistroModificacionProveedores
   AFTER UPDATE ON Proveedores
   FOR EACH ROW
   BEGIN
       -- Registro de cambios en el campo 'nombre'
       IF OLD.nombre <> NEW.nombre THEN
           INSERT INTO LogActividades (proveedor_id, campo_modificado, valor_anterior, valor_nuevo)
           VALUES (NEW.id, 'nombre', OLD.nombre, NEW.nombre);
       END IF;
   
       -- Registro de cambios en el campo 'direccion'
       IF OLD.direccion <> NEW.direccion THEN
           INSERT INTO LogActividades (proveedor_id, campo_modificado, valor_anterior, valor_nuevo)
           VALUES (NEW.id, 'direccion', OLD.direccion, NEW.direccion);
       END IF;
   
   END //
   
   DELIMITER ;
 
    DELIMITER //
    
    CREATE TRIGGER RegistroHistorialEmpleados
    AFTER UPDATE ON Empleados
    FOR EACH ROW
    BEGIN
        -- Registro de cambios en el campo 'salario_base'
        IF OLD.salario_base <> NEW.salario_base THEN
            INSERT INTO HistorialContratos (empleado_id, campo_modificado, valor_anterior, valor_nuevo)
            VALUES (NEW.id, 'salario_base', OLD.salario_base, NEW.salario_base);
        END IF;
    
        -- Registro de cambios en el campo 'puesto_id'
        IF OLD.puesto_id <> NEW.puesto_id THEN
            INSERT INTO HistorialContratos (empleado_id, campo_modificado, valor_anterior, valor_nuevo)
            VALUES (NEW.id, 'puesto_id', OLD.puesto_id, NEW.puesto_id);
        END IF;
    
    END //
    
    DELIMITER ;
    


    


DELIMITER //

CREATE FUNCTION CalcularDescuento(tipo_id INT, precio_original DECIMAL(10, 2))
RETURNS DECIMAL(10, 2)
DETERMINISTIC
BEGIN
    DECLARE descuento DECIMAL(10, 2);
    -- Verificar si la categoría es "Electrónica"
    IF (SELECT tipo_nombre FROM TiposProductos WHERE id = tipo_id) = 'Electrónica' THEN
        SET descuento = precio_original * 0.90; -- Aplicar descuento del 10%
    ELSE
        SET descuento = precio_original; -- Precio sin descuento
    END IF;
    RETURN descuento;
END //


SELECT 
    p.nombre AS NombreProducto,
    p.precio AS PrecioOriginal,
    CalcularDescuento(p.tipo_id, p.precio) AS PrecioConDescuento
FROM 
    Productos p
JOIN 
    TiposProductos t ON p.tipo_id = t.id;


     ALTER TABLE Clientes
     ADD COLUMN fecha_nacimiento DATE;
     
   
DELIMITER //

CREATE FUNCTION CalcularEdad(fecha_nacimiento DATE)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE edad INT;
    SET edad = TIMESTAMPDIFF(YEAR, fecha_nacimiento, CURDATE());
    RETURN edad;
END //

DELIMITER ;

SELECT 
    c.nombre AS NombreCliente,
    c.email AS Email,
    c.fecha_nacimiento AS FechaNacimiento,
    CalcularEdad(c.fecha_nacimiento) AS Edad
FROM 
    Clientes c
WHERE 
    CalcularEdad(c.fecha_nacimiento) >= 18;


     DELIMITER //
     
     CREATE FUNCTION CalcularImpuesto(precio DECIMAL(10, 2))
     RETURNS DECIMAL(10, 2)
     DETERMINISTIC
     BEGIN
         DECLARE precio_final DECIMAL(10, 2);
         SET precio_final = precio * 1.15; -- Aplicar impuesto del 15%
         RETURN precio_final;
     END //
     
     DELIMITER ;
     
   
   SELECT 
       p.nombre AS NombreProducto,
       p.precio AS PrecioOriginal,
       CalcularImpuesto(p.precio) AS PrecioConImpuesto
   FROM 
       Productos p;
   
   
     DELIMITER //
     
     CREATE FUNCTION TotalPedidosCliente(cliente_id INT)
     RETURNS DECIMAL(10, 2)
     DETERMINISTIC
     BEGIN
         DECLARE total DECIMAL(10, 2);
         -- Sumar el total de todos los pedidos del cliente
         SELECT COALESCE(SUM(total), 0) INTO total
         FROM Pedidos
         WHERE cliente_id = cliente_id;
         RETURN total;
     END //
     
     DELIMITER ;
     

SELECT 
    c.nombre AS NombreCliente,
    TotalPedidosCliente(c.id) AS TotalPedidos
FROM 
    Clientes c
WHERE 
    TotalPedidosCliente(c.id) > 1000;


     DELIMITER //
     
     CREATE FUNCTION SalarioAnual(salario_mensual DECIMAL(10, 2))
     RETURNS DECIMAL(10, 2)
     DETERMINISTIC
     BEGIN
         RETURN salario_mensual * 12;
     END //
     
     DELIMITER ;
    
     SELECT 
         e.nombre AS NombreEmpleado,
         SalarioAnual(p.salario_base) AS SalarioAnual
     FROM 
         Empleados e
     JOIN 
         Puestos p ON e.puesto_id = p.id
     WHERE 
         SalarioAnual(p.salario_base) > 50000;
     
 
     DELIMITER //
     
     CREATE FUNCTION Bonificacion(salario DECIMAL(10, 2))
     RETURNS DECIMAL(10, 2)
     DETERMINISTIC
     BEGIN
         RETURN salario * 0.10;
     END //
     
     DELIMITER ;
     
     
        SELECT 
            e.nombre AS NombreEmpleado,
            p.salario_base AS SalarioBase,
            Bonificacion(p.salario_base) AS Bonificacion,
            (p.salario_base + Bonificacion(p.salario_base)) AS SalarioAjustado
        FROM 
            Empleados e
        JOIN 
            Puestos p ON e.puesto_id = p.id;
        

     DELIMITER //
     
     CREATE FUNCTION DiasDesdeUltimoPedido(cliente_id INT)
     RETURNS INT
     DETERMINISTIC
     BEGIN
         DECLARE dias INT;
         -- Buscar la fecha del último pedido y calcular la diferencia con la fecha actual
         SELECT DATEDIFF(CURDATE(), MAX(fecha)) INTO dias
         FROM Pedidos
         WHERE cliente_id = cliente_id;
         
         -- Si el cliente no tiene pedidos, retornar NULL
         IF dias IS NULL THEN
             SET dias = -1; -- Indicador para clientes sin pedidos
         END IF;
     
         RETURN dias;
     END //
     
     DELIMITER ;
     

   SELECT 
       c.nombre AS NombreCliente,
       DiasDesdeUltimoPedido(c.id) AS DiasDesdeUltimoPedido
   FROM 
       Clientes c
   WHERE 
       DiasDesdeUltimoPedido(c.id) >= 0 -- Ignorar clientes sin pedidos
       AND DiasDesdeUltimoPedido(c.id) <= 30; -- Filtrar por pedidos recientes
   


   

     CREATE TABLE Inventario (
         id INT PRIMARY KEY AUTO_INCREMENT,
         producto_id INT,
         cantidad INT,
         FOREIGN KEY (producto_id) REFERENCES Productos(id)
     );
     
    
     DELIMITER //
     
     CREATE FUNCTION TotalInventarioProducto(producto_id INT)
     RETURNS DECIMAL(10, 2)
     DETERMINISTIC
     BEGIN
         DECLARE total DECIMAL(10, 2);
         -- Multiplicar cantidad por precio
         SELECT 
             COALESCE(i.cantidad * p.precio, 0) INTO total
         FROM 
             Inventario i
         JOIN 
             Productos p ON i.producto_id = p.id
         WHERE 
             i.producto_id = producto_id;
     
         RETURN total;
     END //
     
     DELIMITER ;
     
  
       

      
          SELECT 
              p.nombre AS NombreProducto,
              TotalInventarioProducto(p.id) AS TotalInventario
          FROM 
              Productos p
          JOIN 
              Inventario i ON p.id = i.producto_id
          WHERE 
              TotalInventarioProducto(p.id) > 500;
          
        

          

     



     ```mysql
     CREATE TABLE HistorialPrecios (
         id INT PRIMARY KEY AUTO_INCREMENT,
         producto_id INT NOT NULL,
         precio_antiguo DECIMAL(10, 2) NOT NULL,
         precio_nuevo DECIMAL(10, 2) NOT NULL,
         fecha_cambio TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
         FOREIGN KEY (producto_id) REFERENCES Productos(id)
     );
     

     


   DELIMITER //
   
   CREATE TRIGGER RegistroCambioPrecio
   BEFORE UPDATE ON Productos
   FOR EACH ROW
   BEGIN
       -- Registrar el cambio solo si el precio cambia
       IF OLD.precio != NEW.precio THEN
           INSERT INTO HistorialPrecios (producto_id, precio_antiguo, precio_nuevo)
           VALUES (OLD.id, OLD.precio, NEW.precio);
       END IF;
   END //
   
   DELIMITER ;


UPDATE Productos 
SET precio = 550.00 
WHERE id = 1;

```

Consultar el Historial de Precios

```mysql
SELECT * FROM HistorialPrecios;



DELIMITER //

CREATE PROCEDURE ReporteVentasMensuales(
    IN p_mes INT,
    IN p_anio INT
)
BEGIN
    SELECT 
        e.nombre AS NombreEmpleado,
        SUM(p.total) AS TotalVentas
    FROM 
        Empleados e
    JOIN 
        Pedidos p ON e.id = p.cliente_id -- Ajustar según la lógica de asignación de empleados a ventas
    WHERE 
        MONTH(p.fecha) = p_mes
        AND YEAR(p.fecha) = p_anio
    GROUP BY 
        e.id, e.nombre
    HAVING 
        TotalVentas > 0
    ORDER BY 
        TotalVentas DESC;
END //

DELIMITER ;






 
   CALL ReporteVentasMensuales(11, 2024);
   
 

   


     SELECT 
         prov.nombre AS NombreProveedor,
         prod.nombre AS ProductoMasVendido,
         MAX(ventas.TotalVendido) AS CantidadVendida
     FROM 
         Proveedores prov
     JOIN 
         Productos prod ON prov.id = prod.proveedor_id
     JOIN 
         (
             SELECT 
                 dp.producto_id,
                 SUM(dp.cantidad) AS TotalVendido
             FROM 
                 DetallesPedido dp
             GROUP BY 
                 dp.producto_id
         ) ventas ON prod.id = ventas.producto_id
     WHERE 
         ventas.TotalVendido = (
             SELECT 
                 MAX(SUM(dp.cantidad))
             FROM 
                 DetallesPedido dp
             JOIN 
                 Productos p ON dp.producto_id = p.id
             WHERE 
                 p.proveedor_id = prov.id
             GROUP BY 
                 dp.producto_id
         )
     GROUP BY 
         prov.id, prod.id;
     
     




   SELECT 
       prov.nombre AS NombreProveedor,
       prod.nombre AS ProductoMasVendido,
       ventas.TotalVendido AS CantidadVendida
   FROM 
       Proveedores prov
   JOIN 
       Productos prod ON prov.id = prod.proveedor_id
   JOIN 
       (
           SELECT 
               p.proveedor_id,
               dp.producto_id,
               SUM(dp.cantidad) AS TotalVendido
           FROM 
               DetallesPedido dp
           JOIN 
               Productos p ON dp.producto_id = p.id
           GROUP BY 
               p.proveedor_id, dp.producto_id
       ) ventas ON prod.id = ventas.producto_id
   WHERE 
       ventas.TotalVendido = (
           SELECT 
               MAX(SUM(dp.cantidad))
           FROM 
               DetallesPedido dp
           JOIN 
               Productos p ON dp.producto_id = p.id
           WHERE 
               p.proveedor_id = prov.id
           GROUP BY 
               dp.producto_id
       );
   
   

 

  
        DELIMITER //
        
        CREATE FUNCTION EstadoStock(cantidad INT)
        RETURNS VARCHAR(10)
        DETERMINISTIC
        BEGIN
            DECLARE estado VARCHAR(10);
            
            IF cantidad >= 100 THEN
                SET estado = 'Alto';
            ELSEIF cantidad BETWEEN 50 AND 99 THEN
                SET estado = 'Medio';
            ELSE
                SET estado = 'Bajo';
            END IF;
        
            RETURN estado;
        END;
        //
        
        DELIMITER ;
      

        

      SELECT 
          prod.nombre AS NombreProducto,
          prod.precio AS Precio,
          invent.cantidad AS CantidadEnStock,
          EstadoStock(invent.cantidad) AS EstadoDeStock
      FROM 
          Productos prod
      JOIN 
          Inventarios invent ON prod.id = invent.producto_id;
      
     

   



  
     DELIMITER //
     
     CREATE TRIGGER ActualizarInventario
     BEFORE INSERT ON DetallesPedido
     FOR EACH ROW
     BEGIN
         DECLARE stock_actual INT;
         
         -- Obtener el stock actual del producto
         SELECT cantidad INTO stock_actual
         FROM Inventarios
         WHERE producto_id = NEW.producto_id;
         
         -- Verificar si el stock es suficiente
         IF stock_actual < NEW.cantidad THEN
             SIGNAL SQLSTATE '45000'
             SET MESSAGE_TEXT = 'Stock insuficiente para el producto.';
         ELSE
             -- Actualizar el inventario
             UPDATE Inventarios
             SET cantidad = cantidad - NEW.cantidad
             WHERE producto_id = NEW.producto_id;
         END IF;
     END;
     //
     
     DELIMITER ;
     
    
  


   ERROR 1644 (45000): Stock insuficiente para el producto.
 





  
     DELIMITER //
     
     CREATE PROCEDURE ClientesInactivos()
     BEGIN
         -- Seleccionamos los clientes que no han realizado pedidos en los últimos 6 meses
         SELECT c.id, c.nombre, c.email
         FROM Clientes c
         WHERE NOT EXISTS (
             SELECT 1
             FROM Pedidos p
             WHERE p.cliente_id = c.id
             AND p.fecha > CURDATE() - INTERVAL 6 MONTH
         );
     END;
     //
     
     DELIMITER ;
     
    