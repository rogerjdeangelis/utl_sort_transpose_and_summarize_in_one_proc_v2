# utl_sort_transpose_and_summarize_in_one_proc_v2
Sort transpose and summarize in one proc.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Sort transpose and summarize in one proc version 2;

    see github
    https://tinyurl.com/y73rhd2z
    https://github.com/rogerjdeangelis/utl_sort_transpose_and_summarize_in_one_proc_v2

    see SAS Forum
    https://tinyurl.com/ycw9feu7
    https://communities.sas.com/t5/SAS-Data-Management/How-to-transpose-one-column-into-several-columns-decoded-binary/m-p/446814

      Two Solutions (do not need dummy variable and will work with multiple location*number*id records)
         1. Proc corresp
         2. Proc report


    How to transpose one column into several columns

    INPUT
    =====

    WORK.HAVE total obs=7

     LOCATION    NUMBER    ID

        l1        1040      4
        l1        1040      5
        l1        1040      8
        l1        1040     57

        l1        1041      4
        l1        1041      8
        l1        1041     57

     WANT (example)

          LABEL        _4    _5    _8    _57      SUM

          l1 * 1040     1     1     1     1        4
          l1 * 1041     1     0     1     1        3

          Sum           2     1     2     2        7


    PROCESS (all the code)
    ======================

     1. Proc corresp

      ods exclude all;
      ods output observed=want;
      proc corresp data=have observed dim=1 cross=both;
      tables location number, id;
      run;quit;
      ods select all;

     2. Proc report

      options missing='0'; * make a numeric 0 in this case;
      proc report data=have nowd missing
             out=havXpo (rename=(
                               _c3_=id_4
                               _c4_=id_5
                               _c5_=id_8
                               _c6_=id_57
             ));
      cols location number (id N);
      define location / group;
      define number / group;
      define id / across;
      rbreak after / summarize;
      run;quit;


    OUTPUT
    ======


      1. Proc corresp

       WORK.WANT total obs=3

          LABEL        _4    _5    _8    _57    SUM

          l1 * 1040     1     1     1     1      4
          l1 * 1041     1     0     1     1      3
          Sum           2     1     2     2      7

       2. Proc report

        WORK.HAVXPO total obs=3

         LOCATION    NUMBER    ID_4    ID_5    ID_8    ID_57    N    _BREAK_

            l1        1040       1       1       1       1      4
            l1        1041       1       0       1       1      3

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;


    INPUT
    =====

    data have;
     input location$ number$ id;
    cards4;
    l1 1040 4
    l1 1040 5
    l1 1040 8
    l1 1040 57
    l1 1041 4
    l1 1041 8
    l1 1041 57
    ;;;;
    run;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/

    ;

    see process


