# utl-altair-personal-slc-usng-an-excel-database-to-analyze-healthcare-labor-costs
Altair personal slc usng an excel database to analyze healthcare labor costs
    %let pgm=utl-altair-personal-slc-usng-an-excel-database-to-analyze-healthcare-labor-costs;

    %stop_submission;

    Altair personal slc usng an excel database to analyze healthcare labor costs

    Too long to post on a listserv, see github

    github
    https://github.com/rogerjdeangelis/utl-altair-personal-slc-usng-an-excel-database-to-analyze-healthcare-labor-costs

    community.altair.com
    https://community.altair.com/discussion/38593


    Once you create this table, the rest is straight forward( see below).

    Table AllTre

     Vendor               Position              Total_Amount   ICU      NICU    SURGERY   LAB ER

    Vendor 1 Emergency Medical Technician (EMT)     $8,077.5 $8,077.5       .         .    .  .
    Vendor 1 Patient Care Technician (PCT)          $8,415.0       .  $8,415.0        .    .  .
    Vendor 1 Emergency Medical Technician (EMT)     $8,077.5       .        .   $8,077.5   .  .

    Vendor 3 Emergency Medical Technician (EMT)    $10,372.5       .        .  $10,372.5   .  .
    Vendor 3 Register Nurse (RN)                    $7,200.0       .  $7,200.0        .    .  .
    Vendor 3 Emergency Medical Technician (EMT)     $7,200.0 $7,200.0       .         .    .  .


    1 What is the Total Expenditure on Contract Labor Across the 3 Vendors?
    =======================================================================

       GRAND_TOTAL          TOTAL_EXPENSE
      ----------------------------------
      Sum of total_amount        1495868

       proc sql;
        select
          'Sum of total_amount' as Grand_Total
          ,sum(total_amount) as total_expense
        from
          alltre
      ;quit;

    2  What Contract Labor Position is the most expensive?
    ======================================================

       Position                             max_pos_sum

       Emergency Medical Technician (EMT)     604417.5

       proc sql;
         reset outobs=1;
         select
            position
           ,sum(total_amount) as max_pos_sum
         from
            alltre
         group
            by position
         order
            by max_pos_sum descending
       ;quit;

       proc print data=maxpos;
       run;

    3  Which site (ICU, NICU, SURGERY, LAB, ER) spent the most on Contract Labor?
    =============================================================================

       VAR    MAX_DEPT_SUM

       ICU      366052.5

      data nrm;
         set alltre;
         var='SURGERY'; val=surgery;  output;
         var='NICU'   ; val=nicu; output;
         var='LAB'    ; val=lab;  output;
         var='ER'     ; val=er ;  output;
         var='ICU'    ; val=icu;  output;
       run;quit;

       proc sql;
          reset outobs=1;
          create
             table dept as
          select
             var
            ,sum(val) as max_dept_sum
          from
             nrm
          group
             by var
          order
             by max_dept_sum descending
       ;quit;

       proc print data=dept width=min;
       run;quit;

    /*                   _         _        _     _              _ _ _
      ___ _ __ ___  __ _| |_ ___  | |_ __ _| |__ | | ___    __ _| | | |_ _ __ ___
     / __| `__/ _ \/ _` | __/ _ \ | __/ _` | `_ \| |/ _ \  / _` | | | __| `__/ _ \
    | (__| | |  __/ (_| | ||  __/ | || (_| | |_) | |  __/ | (_| | | | |_| | |  __/
     \___|_|  \___|\__,_|\__\___|  \__\__,_|_.__/|_|\___|  \__,_|_|_|\__|_|  \___|

    */

    libname ven1 excel "d:/xls/vendor 1.xlsx";
    libname ven2 excel "d:/xls/vendor 2.xlsx";
    libname ven3 excel "d:/xls/vendor 3.xlsx";

    proc sql;
      create
        table alltre as
      select
        *
      from
        ven1.'ven 1$'n(keep=VENDOR POSITION TOTAL_AMOUNT ICU NICU SURGERY LAB ER)
      union
        all
      select
        *
      from
        ven2.'ven 2$'n(keep=VENDOR POSITION TOTAL_AMOUNT ICU NICU SURGERY LAB ER)
      union
        all
      select
        *
      from
        ven3.'ven 3$'n(keep=VENDOR POSITION TOTAL_AMOUNT ICU NICU SURGERY LAB ER)
    ;quit;

    libname ven1 clear;
    libname ven2 clear;
    libname ven3 clear;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    Table work.AllTre

     Vendor               Position              Total_Amount   ICU      NICU    SURGERY   LAB ER

    Vendor 1 Emergency Medical Technician (EMT)     $8,077.5 $8,077.5       .         .    .  .
    Vendor 1 Patient Care Technician (PCT)          $8,415.0       .  $8,415.0        .    .  .
    Vendor 1 Emergency Medical Technician (EMT)     $8,077.5       .        .   $8,077.5   .  .

    Vendor 3 Emergency Medical Technician (EMT)    $10,372.5       .        .  $10,372.5   .  .
    Vendor 3 Register Nurse (RN)                    $7,200.0       .  $7,200.0        .    .  .
    Vendor 3 Emergency Medical Technician (EMT)     $7,200.0 $7,200.0       .         .    .  .

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    1373      ODS _ALL_ CLOSE;
    1374      ODS LISTING;
    1375      FILENAME WBGSF 'd:\wpswrk\_TD9616/listing_images';
    1376      OPTIONS DEVICE=GIF;
    1377      GOPTIONS GSFNAME=WBGSF;
    1378        libname ven1 excel "d:/xls/vendor 1.xlsx";
    NOTE: Library ven1 assigned as follows:
          Engine:        OLEDB
          Physical Name: d:/xls/vendor 1.xlsx

    1379      libname ven2 excel "d:/xls/vendor 2.xlsx";
    NOTE: Library ven2 assigned as follows:
          Engine:        OLEDB
          Physical Name: d:/xls/vendor 2.xlsx

    1380      libname ven3 excel "d:/xls/vendor 3.xlsx";
    NOTE: Library ven3 assigned as follows:
          Engine:        OLEDB
          Physical Name: d:/xls/vendor 3.xlsx

    1381
    1382      proc sql;
    1383        create
    1384          table alltre as
    1385        select
    1386          *
    1387        from
    1388          ven1.'ven 1$'n(keep=VENDOR POSITION TOTAL_AMOUNT ICU NICU SURGERY LAB ER)
    1389        union
    1390          all
    1391        select
    1392          *
    1393        from
    1394          ven2.'ven 2$'n(keep=VENDOR POSITION TOTAL_AMOUNT ICU NICU SURGERY LAB ER)
    1395        union
    1396          all
    1397        select
    1398          *
    1399        from
    1400          ven3.'ven 3$'n(keep=VENDOR POSITION TOTAL_AMOUNT ICU NICU SURGERY LAB ER)
    1401      ;quit;
    NOTE: Data set "WORK.alltre" has 150 observation(s) and 8 variable(s)
    NOTE: Procedure sql step took :
          real time : 0.365
          cpu time  : 0.265


    NOTE: Libref VEN1 has been deassigned.
    1402
    1403      libname ven1 clear;
    NOTE: Libref VEN2 has been deassigned.
    1404      libname ven2 clear;
    NOTE: Libref VEN3 has been deassigned.
    1405      libname ven3 clear;
    1406
    1407
    1408
    1409
    1410
    1411
    1412
    1413
    1414
    1415      quit; run;
    1416      ODS _ALL_ CLOSE;
    1417      FILENAME WBGSF CLEAR;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
