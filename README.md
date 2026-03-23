# U4W3D1-SQL
## create table
CREATE TABLE clienti (
    numero_cliente SERIAL PRIMARY KEY,
    nome VARCHAR(50),
    cognome VARCHAR(50),
    anno_di_nascita INT,
    regione_residenza VARCHAR(50)
);

CREATE TABLE prodotti (
    id_prodotto SERIAL PRIMARY KEY,
    descrizione TEXT,
    in_produzione BOOLEAN,
    in_commercio BOOLEAN,
    data_attivazione DATE,
    data_disattivazione DATE
);

CREATE TABLE fornitori (
    numero_fornitore SERIAL PRIMARY KEY,
    denominazione VARCHAR(100),
    regione_residenza VARCHAR(50)
);

CREATE TABLE fatture (
    numero_fattura SERIAL PRIMARY KEY,
    tipologia CHAR(1),
    importo DECIMAL(10, 2),
    iva INT,
    id_cliente INT REFERENCES clienti(numero_cliente),
    data_fattura DATE,
    numero_fornitore INT REFERENCES fornitori(numero_fornitore)
);
### es 1 | Extract all customers with the name "Mario"
SELECT * FROM clienti 
WHERE nome = 'Mario';

### es 2 | Extract the first and last names of customers born in 1982
SELECT nome, cognome FROM clienti 
WHERE anno_di_nascita = 1982;

### es 3 | Extract the number of invoices with 20% VAT (how many are there?)
SELECT COUNT(*) AS totale_fatture
FROM fatture
WHERE iva = 20;

### es 4 | Extract products activated in 2017 that are either in production or on the market (To extract the year from a date, you can use EXTRACT(YEAR FROM data))
SELECT * FROM prodotti 
WHERE EXTRACT(YEAR FROM data_attivazione) = 2017
AND(in_produzione = TRUE or in_commercio = TRUE);

### es 5 | Extract invoices with an amount less than 1000 and the data of the customers linked to them
SELECT * FROM fatture
JOIN clienti
ON fatture.id_cliente = clienti.numero_cliente
WHERE fatture.importo < 1000;

### es 6 | Report the list of invoices (number, amount, VAT, and date) plus the name of the supplier
SELECT 
    fatture.numero_fattura, 
    fatture.importo, 
    fatture.iva, 
    fatture.data_fattura, 
    fornitori.denominazione
FROM fatture
JOIN fornitori 
  ON fatture.numero_fornitori = fornitori.numero_fornitori
WHERE fatture.iva = 20;


### es 7 | Considering only invoices with 20% VAT, extract the number of invoices (invoice count) for each year (To extract the year from a date, you can use EXTRACT(YEAR FROM data))
SELECT EXTRACT(YEAR FROM data_fattura) AS anno, COUNT(*) AS numero_fatture
FROM fatture
WHERE iva = 20
GROUP BY anno;

### es 8 Report the number of invoices (invoice count) and the sum of the relative amounts divided by billing year
