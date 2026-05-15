CREATE DATABASE RemoteTrack;
GO

USE Remote Track;
--SUCCURSALES
CREATE TABLE Sucursales (
    idSucursal INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    descripcion VARCHAR(MAX)
);
--CATEGORIAS
CREATE TABLE Categorias (
    idCategoria INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL UNIQUE
);
--ESTADO ASISTENCIAS
CREATE TABLE EstadosAsistencia (
    idEstadoAsistencia INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL UNIQUE
);
--ESTADOS LICENCIAS
CREATE TABLE EstadosLicencias (
    idEstadoLicencia INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL UNIQUE
);
--MOTIVOSLICENCIAS
CREATE TABLE MotivosLicencias (
    idMotivoLicencia INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL UNIQUE
);
--EMPLEADOS
CREATE TABLE Empleados (
    idEmpleado INT IDENTITY(1,1) PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL,
    dni VARCHAR(20) NOT NULL UNIQUE,
    telefono VARCHAR(20),
    email VARCHAR(100) NOT NULL UNIQUE,
    fechaAlta DATE NOT NULL CHECK (fechaAlta <= GETDATE()),
    idSucursal INT NOT NULL,
    idCategoria INT NOT NULL,
    FOREIGN KEY (idSucursal) REFERENCES Sucursales(idSucursal)
        ON UPDATE CASCADE ON DELETE NO ACTION,
    FOREIGN KEY (idCategoria) REFERENCES Categorias(idCategoria)
        ON UPDATE CASCADE ON DELETE NO ACTION
);
--ASISTENCIAS
CREATE TABLE Asistencias (
    idAsistencia INT IDENTITY(1,1) PRIMARY KEY,
    fecha DATE NOT NULL,
    horaIngreso TIME NOT NULL,
    horaEgreso TIME,
    idEstadoAsistencia INT NOT NULL,
    idEmpleado INT NOT NULL,
    FOREIGN KEY (idEstadoAsistencia) REFERENCES EstadosAsistencia(idEstadoAsistencia)
        ON UPDATE CASCADE ON DELETE NO ACTION,
    FOREIGN KEY (idEmpleado) REFERENCES Empleados(idEmpleado)
        ON UPDATE CASCADE ON DELETE NO ACTION,
    CHECK (horaEgreso IS NULL OR horaEgreso > horaIngreso)
);
--HORASEXTRA
CREATE TABLE HorasExtras (
    idHorasExtra INT IDENTITY(1,1) PRIMARY KEY,
    cantidadHoras INT NOT NULL,
    motivo VARCHAR(MAX) NOT NULL,
    idEmpleado INT NOT NULL,
    FOREIGN KEY (idEmpleado) REFERENCES Empleados(idEmpleado)
        ON UPDATE CASCADE ON DELETE NO ACTION
);
--NOVEDADES
CREATE TABLE Novedades (
    idNovedad INT IDENTITY(1,1) PRIMARY KEY,
    descripcion VARCHAR(MAX) NOT NULL,
    fecha DATE NOT NULL,
    idEmpleado INT NOT NULL,
    FOREIGN KEY (idEmpleado) REFERENCES Empleados(idEmpleado)
        ON UPDATE CASCADE ON DELETE NO ACTION
);
--LICENCIAS
CREATE TABLE Licencias (
    idLicencia INT IDENTITY(1,1) PRIMARY KEY,
    fechaInicio DATE NOT NULL,
    fechaFin DATE NOT NULL,
    idEstadoLicencia INT NOT NULL,
    idMotivoLicencia INT NOT NULL,
    idEmpleado INT NOT NULL,
    FOREIGN KEY (idEstadoLicencia) REFERENCES EstadosLicencias(idEstadoLicencia)
        ON UPDATE CASCADE ON DELETE NO ACTION,
    FOREIGN KEY (idMotivoLicencia) REFERENCES MotivosLicencias(idMotivoLicencia)
        ON UPDATE CASCADE ON DELETE NO ACTION,
    FOREIGN KEY (idEmpleado) REFERENCES Empleados(idEmpleado)
        ON UPDATE CASCADE ON DELETE NO ACTION,
    CHECK (fechaFin >= fechaInicio)
);

-- Sucursales
INSERT INTO Sucursales (nombre, descripcion)
VALUES ('Sucursal Centro', 'Sucursal principal en el centro'),
       ('Sucursal Norte', 'Sucursal ubicada en zona norte');

-- Categorías
INSERT INTO Categorias (nombre)
VALUES ('Administrativo'),
       ('Supervisor'),
       ('Operario');

-- Empleados
INSERT INTO Empleados 
(nombre, apellido, dni, telefono, email, fechaAlta, idSucursal, idCategoria)
VALUES
('Juan', 'Pérez', '30111222', '3515551111', 'juan.perez@empresa.com', '2023-01-10', 1, 1),
('María', 'Gómez', '28999888', '3515552222', 'maria.gomez@empresa.com', '2022-11-05', 1, 2),
('Carlos', 'López', '31222333', '3515553333', 'carlos.lopez@empresa.com', '2024-03-01', 2, 3),
('Lucía', 'Fernández', '29888777', '3515554444', 'lucia.fernandez@empresa.com', '2023-07-15', 2, 2),
('Sofía', 'Martínez', '27666111', '3515555555', 'sofia.martinez@empresa.com', '2024-01-20', 1, 3);
GO

