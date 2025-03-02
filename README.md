# query-sql-PT-AOI
-- CREATE TABLES
CREATE TABLE Dokter (
  IdDokter VARCHAR(10) PRIMARY KEY,
  name TEXT NOT NULL
);

CREATE TABLE Karyawan (
  NIK INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  factory VARCHAR(255) NOT NULL,
  department VARCHAR(255) NOT NULL,
  SubDepartment VARCHAR(255) NOT NULL,
  posisi VARCHAR(255) NOT NULL
);

CREATE TABLE Penyakit (
  KodePenyakit VARCHAR(10) PRIMARY KEY,
  jenispenyakit VARCHAR(255) NOT NULL,
  namapenyakit VARCHAR(255) NOT NULL
);

CREATE TABLE Diagnosa (
  kodediagnosa VARCHAR(10) PRIMARY KEY,
  IdDokter VARCHAR(10),
  NIK INTEGER,
  KodePenyakit VARCHAR(10),
  tanggaldiagnosa DATE NOT NULL,
  FOREIGN KEY (IdDokter) REFERENCES Dokter(IdDokter) ON DELETE CASCADE,
  FOREIGN KEY (NIK) REFERENCES Karyawan(NIK) ON DELETE CASCADE,
  FOREIGN KEY (KodePenyakit) REFERENCES Penyakit(KodePenyakit) ON DELETE CASCADE
);

CREATE TABLE KeluhanKecelakaan (
  kodekeluhankecelakaan VARCHAR(10) PRIMARY KEY,
  waktukejadian DATE NOT NULL,
  kronologi VARCHAR(255) NOT NULL,
  lokasikejadian VARCHAR(255) NOT NULL,
  mesinyangdigunakan VARCHAR(255)
);

CREATE TABLE KeluhanKesehatan (
  kodekeluhankesehatan VARCHAR(10) PRIMARY KEY,
  keluhankesehatan VARCHAR(255) NOT NULL
);

CREATE TABLE TransaksiKunjungan (
  IdKunjungan VARCHAR(10) PRIMARY KEY,
  kodekeluhankecelakaan VARCHAR(10),
  NIK INTEGER,
  kodekeluhankesehatan VARCHAR(10),
  waktukunjungan DATE NOT NULL,
  FOREIGN KEY (kodekeluhankecelakaan) REFERENCES KeluhanKecelakaan(kodekeluhankecelakaan) ON DELETE CASCADE,
  FOREIGN KEY (NIK) REFERENCES Karyawan(NIK) ON DELETE CASCADE,
  FOREIGN KEY (kodekeluhankesehatan) REFERENCES KeluhanKesehatan(kodekeluhankesehatan) ON DELETE CASCADE
);

CREATE TABLE Obat (
  kodeobat VARCHAR(10) PRIMARY KEY,
  namaobat VARCHAR(255) NOT NULL
);

CREATE TABLE ResepObat (
  koderesep VARCHAR(10) PRIMARY KEY,
  kodeobat VARCHAR(10),
  kodediagnosa VARCHAR(10),
  jumlahobat INTEGER NOT NULL,
  jadwalkonsumsi VARCHAR(255) NOT NULL,
  FOREIGN KEY (kodeobat) REFERENCES Obat(kodeobat) ON DELETE CASCADE,
  FOREIGN KEY (kodediagnosa) REFERENCES Diagnosa(kodediagnosa) ON DELETE CASCADE
);

-- INSERT DATA
INSERT INTO Dokter VALUES ('h2AQ', 'dr. Anisa');

INSERT INTO Karyawan VALUES (140808, 'Fulan', 'Produksi', 'Sewing', 'Line 12', 'Operator');

INSERT INTO Penyakit VALUES ('P001', 'Infeksi', 'Flu');

INSERT INTO Diagnosa VALUES ('kd12', 'h2AQ', 140808, 'P001', '2023-02-12');

INSERT INTO Obat VALUES ('PIM PCT', 'Paracetamol');

INSERT INTO ResepObat VALUES ('K12', 'PIM PCT', 'kd12', 3, '3 Kali sehari');

-- FETCH DATA
SELECT * FROM Dokter;
SELECT * FROM Karyawan;
SELECT * FROM Obat;

SELECT 
  ResepObat.koderesep,
  Obat.kodeobat,
  Obat.namaobat,
  ResepObat.jumlahobat,
  ResepObat.jadwalkonsumsi
FROM ResepObat
INNER JOIN Obat 
ON ResepObat.kodeobat = Obat.kodeobat;

