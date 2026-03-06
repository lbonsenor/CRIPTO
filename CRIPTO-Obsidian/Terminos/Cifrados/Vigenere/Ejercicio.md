Sea $c$:
```
PBVRQ VICAD SKAÑS DETSJ PSIED BGGMP SLRPW RÑPWY EDSDE ÑDRDP CRCPQ MNPWK UBZVS FNVRD MTIPW UEQVV CBOVN UEDIF QLONM WNUVR SEIKA ZYEAC EYEDS ETFPH LBHGU ÑESOM EHLBX VAEEP UÑELI SEVEF WHUNM CLPQP MBRRN BPVIÑ MTIBV VEÑID ANSJA MTJOK MDODS ELPWI UFOZM QMVNF OHASE SRJWR SFQCO TWVMB JGRPW VSUEX INQRS JEUEM GGRBD GNNIL AGSJI DSVSU EEINT GRUEE TFGGM PORDF OGTSS TOSEQ OÑTGR RYVLP WJIFW XOTGG RPQRR JSKET XRNBL ZETGG NEMUO TXJAT ORVJH RSFHV NUEJI BCHAS EHEUE UOTIE FFGYA TGGMP IKTBW UEÑEN IEEU
```

- `GGMP` aparece 3 veces separadas por 256 y 104 caracteres
- `YEDS` aparece 2 veces separadas por 72 caracteres
- `HASE` aparece 2 veces separadas por 156 caracteres
- `VSUE` aparece 2 veces separadas por 32 caracteres

$MCD(32,72,104,156,156)=4$
Reescribamos para que sea cada 4 caracteres

```
PBVR QVIC ADSK AÑSD ETSJ PSIE DBGG MPSL RPWR ÑPWY EDSD EÑDR DPCR CPQM NPWK UBZV SFNV RDMT IPWU EQVV CBOV NUED IFQL ONMW NUVR SEIK AZYE ACEY EDSE TFPH LBHG UÑES OMEH LBXV AEEP UÑEL ISEV EFWH UNMC LPQP MBRR NBPV IÑMT IBVV EÑID ANSJ AMTJ OKMD ODSE LPWI UFOZ MQMV NFOH ASES RJWR SFQC OTWV MBJG RPWV SUEX INQR SJEU EMGG RBDG NNIL AGSJ IDSV SUEE INTG RUEE TFGG MPOR DFOG TSST OSEQ OÑTG RRYV LPWJ IFWX OTGG RPQR RJSK ETXR NBLZ ETGG NEMU OTXJ ATOR VJHR SFHV NUEJ IBCH ASEH EUEU OTIE FFGY ATGG MPIK TBWU EÑEN IEEU
```

Recordando la tabla de frecuencias del castellano

| Frec. Alta |         | Frec. Media |        | Frec. Baja |        |
| ---------- | ------- | ----------- | ------ | ---------- | ------ |
| E          | $13.11$ | C           | $4.85$ | Y          | $0.79$ |
| A          | $10.60$ | L           | $4.42$ | Q          | $0.74$ |
| S          | $8.47$  | U           | $4.34$ | H          | $0.60$ |
| O          | $8.23$  | M           | $3.11$ | Z          | $0.26$ |
| I          | $7.16$  | P           | $2.71$ | J          | $0.25$ |
| N          | $7.14$  | G           | $1.40$ | X          | $0.15$ |
| R          | $6.95$  | B           | $1.16$ | W          | $0.12$ |
| D          | $5.87$  | F           | $1.13$ | K          | $0.11$ |
| T          | $5.40$  | V           | $0.82$ | Ñ          | $0.10$ |

Podemos transformarlo en este descifrado con la clave $K=ABER$

```
PARA QUEL ACOS ANOM ESOR PREN DACO MOOT ROSA NOSH ECOM ENZA DOYA CONU NOSS UAVE SEKE RCIC IOSD EPRE CALE NTAM IENT ONIF NTRA SDES AYUN ABAH ECON TEMP LADO UNAB OLAP LATE ADAY UNAT IRAE EESP UNIL LONY MANA NAME INIC IARE ENEM ANOR ALPR OJIM OCON LOSQ UELI MPIE NELP ARAB RISA SENL OSSE MAFO ROSE STAG INNA SIAD ELCO RAZO NNET AFOR ICOE STAN INPO RTAN TECO MOLA DELO TROC ORAZ ONPO RQUE LOSR IESG OSCO RONA RIOS ESTA NAHI ESCO NDID OSTR ASLA VIDA SEDE NTAR IAYP ARAP ETAD OSEN FECH ASCO MOES TASD ENAW IDAD
```

$$\implies d(c,\text{ABER})=p$$
Tal que $p=$ 
	
