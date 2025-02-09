# utl-sort-dataset-in-an-arbitrary-prdefined-order-ie-sort-a-b-b--c-c-d-in-d-b-c-a-order-r-and-sql
Sort a dataset in an arbitrary prdefined order example sort a b b c c d in d b c a order r and sql
    %let pgm=utl-sort-dataset-in-an-arbitrary-prdefined-order-ie-sort-a-b-b--c-c-d-in-d-b-c-a-order-r-and-sql;

    %stop_submission;

    Sort a dataset in an arbitrary prdefined order example sort a b b c c d in d b c a order r and sql

         SOLUTIONS
             1 sas sql (works in excel python and r)
               see https://tinyurl.com/ymu776ft for r  python excel
             2 sas sql with format lookup
             3 base r

    github
    https://tinyurl.com/mrynpw3y
    https://github.com/rogerjdeangelis/utl-sort-dataset-in-an-arbitrary-prdefined-order-ie-sort-a-b-b--c-c-d-in-d-b-c-a-order-r-and-sql

    stackoverflow
    https://tinyurl.com/ydrhn2wr
    https://stackoverflow.com/questions/1568511/how-do-i-sort-one-vector-based-on-values-of-another

    /**************************************************************************************************************************/
    /*                             |                                      |                                                   */
    /*          INPUTS             |       PROCESS                        | OUTPUT                                            */
    /*          ======             |       =======                        | ======                                            */
    /*                             |                                      |                                                   */
    /*     DATA   |   ORDER        | Use the order dataset to             | NEW_LTR                                           */
    /*            |                | reorder the ltr column               | _ORDER                                            */
    /*  ID    LTR | ID    ODR      |                                      |                                                   */
    /*            |                | Most sql databases have              |   d                                               */
    /*   1     a  |  1     d       | rowid so you do not                  |   b                                               */
    /*   2     b  |  2     b       | need the ID columns.                 |   b                                               */
    /*   3     b  |  3     c       |                                      |   c                                               */
    /*   4     c  |  4     a       | We need ID for SAS                   |   c                                               */
    /*   5     c  |                |                                      |   a                                               */
    /*   6     d  |                | 1 SAS SQL(SAME R EXCEL PYTHON)       |                                                   */
    /*                             | ==============================       |                                                   */
    /*                             |                                      |                                                   */
    /*  options                    | proc sql;                            |                                                   */
    /*  validvarname=upcase;       |   create                             |                                                   */
    /*  libname sd1 "d:/sd1";      |      table want as                   |                                                   */
    /*  data sd1.ltr;              |   select                             |                                                   */
    /*    input id ltr $2.;        |      r.ltr as new_order              |                                                   */
    /*  cards4;                    |   from                               |                                                   */
    /*  1 a                        |      sd1.odr as l                    |                                                   */
    /*  2 b                        |   left                               |                                                   */
    /*  3 b                        |      join sd1.ltr as r               |                                                   */
    /*  4 c                        |   on                                 |                                                   */
    /*  5 c                        |     l.odr = r.ltr                    |                                                   */
    /*  6 d                        |   order                              |                                                   */
    /*  ;;;;                       |     by l.id                          |                                                   */
    /*  run;quit;                  | ;quit;                               |                                                   */
    /*                             |                                      |                                                   */
    /*                             |--------------------------------------|                                                   */
    /*                             |                                      |                                                   */
    /*  data sd1.odr;              | 2 SAS SQL WITH FORMAT LOOKUP         |                                                   */
    /*                             | ============================         |                                                   */
    /*    input id odr $2.;        |                                      |                                                   */
    /*  cards4;                    | data lookup;                         |                                                   */
    /*  1 d                        |   retain                             |                                                   */
    /*  2 b                        |     fmtname "$lookup"                |                                                   */
    /*  3 c                        |     start                            |                                                   */
    /*  4 a                        |     label;                           |                                                   */
    /*  ;;;;                       |   input start$;                      |                                                   */
    /*  run;quit;                  |   label=_n_;                         |                                                   */
    /*                             | cards4;                              |                                                   */
    /*                             | d                                    |                                                   */
    /*                             | b                                    |                                                   */
    /*                             | c                                    |                                                   */
    /*                             | a                                    |                                                   */
    /*                             | ;;;;                                 |                                                   */
    /*                             | run;quit;                            |                                                   */
    /*                             |                                      |                                                   */
    /*                             | proc format                          |                                                   */
    /*                             |   cntlin=lookup;                     |                                                   */
    /*                             | run;quit;                            |                                                   */
    /*                             |                                      |                                                   */
    /*                             | proc sql;                            |                                                   */
    /*                             |   create                             |                                                   */
    /*                             |      table want as                   |                                                   */
    /*                             |   select                             |                                                   */
    /*                             |      ltr as new_ltr_order            |                                                   */
    /*                             |   from                               |                                                   */
    /*                             |      sd1.ltr                         |                                                   */
    /*                             |   order                              |                                                   */
    /*                             |      by input(ltr,lookup.)           |                                                   */
    /*                             | ;quit;                               |                                                   */
    /*                             |                                      |                                                   */
    /*                             |--------------------------------------|                                                   */
    /*                             |                                      |                                                   */
    /*                             | 3 BASE R                             |                                                   */
    /*                             | ========                             |                                                   */
    /*                             | Had some problems getting all the    |                                                   */
    /*                             | '[($,' in the right places           |                                                   */
    /*                             |                                      |                                                   */
    /*                             | %utl_rbeginx;                        |                                                   */
    /*                             | parmcards4;                          |                                                   */
    /*                             | library(haven)                       |                                                   */
    /*                             | library(sqldf)                       |                                                   */
    /*                             | source("c:/oto/fn_tosas9x.R")        |                                                   */
    /*                             | odr<-read_sas(                       |                                                   */
    /*                             |  "d:/sd1/odr.sas7bdat")              |                                                   */
    /*                             | ltr<-read_sas(                       |                                                   */
    /*                             |  "d:/sd1/ltr.sas7bdat")              |                                                   */
    /*                             | want<- as.data.frame(                |                                                   */
    /*                             |     ltr$LTR[order(                   |                                                   */
    /*                             |      ordered(ltr$LTR                 |                                                   */
    /*                             |     ,levels = odr$ODR))])            |                                                   */
    /*                             | colnames(want)<-"NEW_LTR_ORDER"      |                                                   */
    /*                             | want                                 |                                                   */
    /*                             | fn_tosas9x(                          |                                                   */
    /*                             |       inp    = want                  |                                                   */
    /*                             |      ,outlib ="d:/sd1/"              |                                                   */
    /*                             |      ,outdsn ="want"                 |                                                   */
    /*                             |      )                               |                                                   */
    /*                             | ;;;;                                 |                                                   */
    /*                             | %utl_rendx;                          |                                                   */
    /*                             |                                      |                                                   */
    /*                             | proc print data=sd1.want;            |                                                   */
    /*                             | run;quit;                            |                                                   */
    /*                             |                                      |                                                   */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options
    validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.ltr;
      input id ltr $2.;
    cards4;
    1 a
    2 b
    3 b
    4 c
    5 c
    6 d
    ;;;;
    run;quit;

    data sd1.odr;

      input id odr $2.;
    cards4;
    1 d
    2 b
    3 c
    4 a
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* ltr.sas7bdat  | odr.sas7bdat                                                                                           */
    /*               |                                                                                                        */
    /*     DATA      |   ORDER                                                                                                */
    /*               |                                                                                                        */
    /*  ID    LTR    | ID    ODR                                                                                              */
    /*               |                                                                                                        */
    /*   1     a     |  1     d                                                                                               */
    /*   2     b     |  2     b                                                                                               */
    /*   3     b     |  3     c                                                                                               */
    /*   4     c     |  4     a                                                                                               */
    /*   5     c     |                                                                                                        */
    /*   6     d     |                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                             _
    / |  ___  __ _ ___   ___  __ _| |
    | | / __|/ _` / __| / __|/ _` | |
    | | \__ \ (_| \__ \ \__ \ (_| | |
    |_| |___/\__,_|___/ |___/\__, |_|
                                |_|
    */

    proc sql;
      create
         table want as
      select
         r.ltr as new_order
      from
         sd1.odr as l
      left
         join sd1.ltr as r
      on
        l.odr = r.ltr
      order
        by l.id
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  NEW_LTR                                                                                                               */
    /*  _ORDER                                                                                                                */
    /*                                                                                                                        */
    /*    d                                                                                                                   */
    /*    b                                                                                                                   */
    /*    b                                                                                                                   */
    /*    c                                                                                                                   */
    /*    c                                                                                                                   */
    /*    a                                                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                              _              _             _
    |___ \   ___  __ _ ___   ___  __ _| | __      __ | | ___   ___ | | ___   _ _ __
      __) | / __|/ _` / __| / __|/ _` | | \ \ /\ / / | |/ _ \ / _ \| |/ / | | | `_ \
     / __/  \__ \ (_| \__ \ \__ \ (_| | |  \ V  V /  | | (_) | (_) |   <| |_| | |_) |
    |_____| |___/\__,_|___/ |___/\__, |_|   \_/\_/   |_|\___/ \___/|_|\_\\__,_| .__/
                                    |_|                                       |_|
    */

     data lookup;
       retain
         fmtname "$lookup"
         start
         label;
       input start$;
       label=_n_;
     cards4;
     d
     b
     c
     a
     ;;;;
     run;quit;

     proc format
       cntlin=lookup;
     run;quit;

     proc sql;
       create
          table want as
       select
          ltr as new_ltr_order
       from
          sd1.ltr
       order
          by input(ltr,lookup.)
     ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  NEW_LTR                                                                                                               */
    /*  _ORDER                                                                                                                */
    /*                                                                                                                        */
    /*    d                                                                                                                   */
    /*    b                                                                                                                   */
    /*    b                                                                                                                   */
    /*    c                                                                                                                   */
    /*    c                                                                                                                   */
    /*    a                                                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____   _
    |___ /  | |__   __ _ ___  ___   _ __
      |_ \  | `_ \ / _` / __|/ _ \ | `__|
     ___) | | |_) | (_| \__ \  __/ | |
    |____/  |_.__/ \__,_|___/\___| |_|

    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    odr<-read_sas(
     "d:/sd1/odr.sas7bdat")
    ltr<-read_sas(
     "d:/sd1/ltr.sas7bdat")
    want<- as.data.frame(
        ltr$LTR[order(
         ordered(ltr$LTR
        ,levels = odr$ODR))])
    colnames(want)<-"NEW_LTR_ORDER"
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*               |                                                                                                        */
    /* R             |  SAS                                                                                                   */
    /*               |              NEW_LTR_                                                                                  */
    /* NEW_LTR_ORDER |  ROWNAMES     ORDER                                                                                    */
    /*               |                                                                                                        */
    /*             d |      1          d                                                                                      */
    /*             b |      2          b                                                                                      */
    /*             b |      3          b                                                                                      */
    /*             c |      4          c                                                                                      */
    /*             c |      5          c                                                                                      */
    /*             a |      6          a                                                                                      */
    /*               |                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
