Regulaciones internacionales sectoriales. Ver [[Regulaciones de Protección de Datos]] para el contexto general.

---

## HIPAA — Health Insurance Portability and Accountability Act (1996)

**Origen**: Estados Unidos.

**Alcance**: protección de datos relacionados con la **salud de las personas** (_PHI — Protected Health Information_ / _PPI — Personal Protected Information_).

**A quién aplica**: proveedores de salud, aseguradoras, y cualquier organización que maneje datos de salud de ciudadanos estadounidenses.

**Requisitos clave**:
- Controles de acceso estrictos a datos de salud.
- Cifrado de PHI en reposo y en tránsito. Ver [[Cifrado en Reposo]] y [[Cifrado en Tránsito]].
- Notificación obligatoria ante brechas.
- Auditorías y registros de acceso.

---

## PCI-DSS — Payment Card Industry Data Security Standard

**Origen**: International (consorcio Visa, Mastercard, Amex, Discover).

**Alcance**: regulación del uso de **información de tarjetas de crédito**.

**A quién aplica**: **obligatorio para cualquier empresa que desee realizar operaciones con tarjetas de crédito** (comercios, procesadores de pago, etc.).

**Requisitos clave**:
- Cifrado de datos de tarjetas en almacenamiento y transmisión.
- Segmentación de red para aislar el ambiente de datos de tarjetas.
- Monitoreo continuo y pruebas regulares de seguridad ([[PenTest]]).
- [[Least Privilege]] en el acceso a datos de tarjetas.

---

## SOX — Sarbanes-Oxley Act (2002)

**Origen**: Ley federal de Estados Unidos.

**Alcance**: controles financieros y contables en **empresas que cotizan en bolsa**.

**Contexto**: surgió como respuesta a los escándalos corporativos de Enron y WorldCom.

**Requisitos de seguridad relevantes**:
- Los sistemas que soportan los reportes financieros deben tener controles de acceso auditables.
- Se exige evidencia de que los controles funcionan correctamente.
- Retención de registros financieros y de auditoría. Ver [[Retención de Información]].

---

## Relación con otros conceptos

- [[Regulaciones de Protección de Datos]]: el hub del que forman parte.
- [[Cumplimiento y Auditoría]]: el proceso organizacional para cumplir con estas regulaciones.
- [[PenTest]]: PCI-DSS exige pruebas de penetración periódicas.
