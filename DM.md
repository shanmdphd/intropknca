# Data Management {#DM}

> PC와 PP는 RELREC으로 연결된다!

"Relating PC and PP Domains using RELREC Records"
Dr. Peter Schaefer, Director Product Management, Certara™
peter.schaefer@certara.com
http://www.phusewiki.org/docs/Posters2013CSS/PP02.pdf

서울아산병원 임상약리학과에서는 다음과 같은 흐름으로 표준화된 DM은 진행된다.

1. **WORD: **Final CRF Word파일을 Adobe PDF로 출력
2. **ACROBAT: **Adobe Acrobat 으로 fillable PDF Unit (최소단위) 생성.
3. **CONCATENATE :** VISIT, PERIOD를 고려하여 JAVA iText Programming 
3. Acrobat DC Reader로 실제환자 자료 입력 요청.
4. Acrobat DC Reader로 자료 입력 완료후 PDF 파일 전공의 수령.
5. Adobe Acrobat에서 FDF Export하여 CSV생성. 
7. R을 사용하여 table을 CDISC format으로 변경. 
8. R을 사용하여 CDISC format의 table을 CSV로 export. (DM.csv)
9. R을 사용하여 통계분석. 
10. R을 사용하여 CSR Appendix 생성.
11. DM에서 보낸것과 1:1 비교.

## SDTM {#SDTM}
14페이지 SDTM을 참고한다.

Comments ()
CM / EX / SU / PR(콧줄 언제 꽂았다)
SD

### TS.XLSX - 
XPT는 SAS있어야 한다. 나중에 하고 CSV로 하자.

TSVAL title???
DM 작업이 엉터리가 

TA.XLSX - 
TAETORD - ORDER
ETCD - CODE
EPOCH - 시기. ELEMENT - 

TI.XLSX
INCL | EXCL 

TE.XLSX
TRIAL ELEMENT - EPOCH보다 세분화된 시기

TV.XLSX - VISIT
VISIT 외래는 따로. dosing한개를 한개의 visit으로 본다. 
1ST 2ND 4RD 4TH 5TH

DA8159 - FDA REVIEW 들어갔는데 자료를 요구했다. 이왕 줄거면 CDISC, SDTM

PCSEQ 8159 PARENT 8164 METABOLITE

USUBJID - UNIQUE 제품 전체에 걸쳐서 unique해야 한다.

PCORRES - LLOQ
PCSTRESC - PC STANDARD RESULT SC=STANDARD CHARACTER | N=NUMERIC
PCREASND - REASON NOT DONE
PCFAST - PC 공복
PCLLOQ - LOWER LIMIT OF QUANT | UPPER LIMIT OF QUANT
PCTPT - TIME POINT
PCT NUM 1 2 3 4 SORTING용
pcrftdtc - reference date time - 투약시간 - 측정시간  첫투약시간
PPSPEC | SPECIMEN 

PP.XLSX PP PARAMETR
VZ OBSERVED BY F

RFSTDTC - 투약일 - REF START DATE
RFICDTC - INFORMED CONSENT 날짜만 있다
ARMCD - CODE 
ACT ACTUAL


SE SUBJECT ELEMENT

SESTDTC - PROGRAMMING이 필요하다.

RDOMAIN IDVAR RELID
PC PCGRPID A
PP PPGRPID A
PC의 pcgrpid같으면 연결해서 생각하시오. 농도와 PK

XPT로는 한꺼번에 DEFINE. 할수 있다.
첫작업은 엑셀으로 COPY&PASTE로 만들 수 있다 -> 필요하면 R로 ..
CRF를 어떻게 만들지! 생각해놔야한다.
랩은 CRF는 안해도 된다. 했냐 안했냐만 기록한다.
EMR에서 download하지만 CSV 여러개로 나눠받아야한다.
R과 SAS로 둘다가능하면 R로하자. 반올림함수 만들어서 쓰자.

* = user defined code list

SDTM
HANMI-RAOL
BLANK CRF를 만들어. 그다음에 CSV로 만든다음에 

RTF - 잘 만들어준다. addTable | addTOC 
단점은 특수문자를 잘 못쓴다 - CDISC에선 오히려 장점이다.

blankCRF -> PDF -> R&Acrobat (R1)
Fillable
annotated CRF 같이 만들어야 한다. 빨간색으로. 
PDF를 모아서 acrobat에서 한줄의 csv -> R에서 짤라서.(R2) -> SDTM Table을 만들어. 
R(RTF) (R3) - CSR appendix table (Data table listing)을 만든다 - 100페이지 내외로. 다시 coordinator (CRC) source document랑 비교 - LAB(EMR), Worksheet

* PDF naming rule 잘 만들어야겠지 - 어떻게 해야할지 고민해야 한다
* 한미라올에서 곧 SDTM table 같은걸 받을거다 -> SDTM으로 만들어서 -> R(RTF) -> 

1페이지의 CRF로부터 한개의 PDF페이지를 만들자.

====

#2016.04.07목 오전

CSR appendix에 들어갈 Table의 생성.

Data Dictionary

SAS

BASE
GRAPH
STAT

KOREAN

SAS에서 

DM을 가장 잘하는 곳. LSK GLOBAL PS. CRO | 통계로 먹고 사는 것이 아님. Monitoring. 이영작박사 (새누리당)

LongFormatConcRecieved
RandLabel - Sample reblinding한거 unblind

X는 character. n은 숫자.

%%% 5.1 total width | 1은  소숫점 %5.1f 총 다섯자리인데 소숫점 sprintf함수!!! 맞춰서 출력해준다. %%%

LibTabList - 잘라낸 것. 
PrTab function = ColJust - centering
Round 함수. 


## The CDISC standard domain models (SDTMIG 3.1.2 and SENDIG 3.0)

Special-Purpose Domains:
* Demographics - DM 
* Comments - CO
* Subject Elements - SE
* Subject Visits - SV (SDTMIG only)

Interventions:
* Concomitant Medications - CM (SDTMIG only)
* Exposure - EX
* Substance Use - SU (SDTMIG only)

Events:
* Adverse Events - AE (SDTMIG only)
* Disposition - DS
* Medical History - MH (SDTMIG only)
* Protocol Deviations - DV (SDTMIG only)
* Clinical Events - CE (SDTMIG only)

Findings:
* Body Weight - BW (SEND only)
* Body Weight Gain - BG (SEND only)
* Clinical Observations - CL (SEND only)
* Death Diagnosis - DD (SEND only)
* Drug Accountability - DA (SDTMIG only)
* ECG Tests - EG
* Findings About Events or Interventions - FA (SDTMIG only)
* Food and Water Consumption - FW (SEND only)
* Inclusion/Exclusion Exceptions - IE (SDTMIG only)
* Laboratory Tests - LB
* Macroscopic Findings - MA (SEND only)
* Microbiology Specimens - MB (SDTMIG only)
* Microbiology Susceptibility - MS (SDTMIG only)
* Microscopic Findings - MI (SEND only)
* Organ Measurements - OM (SEND only)
* Palpable Masses - PA (SEND only)
* Pharmacokinetics Concentrations - PC 
* Pharmacokinetics Parameters - PP 
* Physical Examinations - PE (SDTMIG only)
* Questionnaires - QS (SDTMIG only)
* Subject Characteristics - SC
* Tumor Findings - TF (SEND only)
* Vital Signs - VS

Trial Design Domains:
* Trial Elements - TE
* Trial Arms - TA
* Trial Visits - TV (SDTMIG only)
* Trial Inclusion/Exclusion Criteria – TI (SDTMIG only)
* Trial Summary - TS

Special-Purpose Relationship Datasets:
* Supplemental Qualifiers - SUPPQUAL
* Relate Records - RELREC