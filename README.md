# utl_appending_two_tables_with_different_corresponding_names_without_renaming
Appending two tables with different corresponding names without renaming.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.


    Appending two tables with different corresponding names without renaming

    Same results in WPS and SAS

    The object is use corresponding names in havOne for havTwo
    and create a column that contains both height and weight, without renaming.


    inspired by
    https://communities.sas.com/t5/SAS-Communities-Library/SQL-Allows-Multiple-Columns-with-Same-Name/ta-p/475974


    INPUT
    =====

     WORK.HAVONE total obs=5

        NAME       SEX    HEIGHT    WEIGHT

        Alfred      M      69.0      112.5
        Henry       M      63.5      102.5
        James       M      57.3       83.0
        Jeffrey     M      62.5       84.0
        John        M      59.0       99.5


     WORK.HAVETWO total obs=5

        NAME       SEX     HGT     WGT    ** different corresponding names

        Alice       F     56.5     84.0
        Barbara     F     65.3     98.0
        Carol       F     62.8    102.5
        Jane        F     59.8     84.5
        Janet       F     62.5    112.5



     EXAMPLE OUTPUT


     WORK.WANT total obs=10
                                                       both_height_and_weight_height = HEIGHT in haveOne
         Maps corresponding names in One to Two        both_height_and_weight_height = WEIGHT in haveTwo
        ==================================================================================================

                                                        BOTH_HEIGHT_
        NAME       SEX    HEIGHT    WEIGHT     TYP       AND_WEIGHT

        Alice       F      56.5       84.0    Height         56.5    = Height
        Jane        F      59.8       84.5    Height         59.8
        Janet       F      62.5      112.5    Height         62.5
        Carol       F      62.8      102.5    Height         62.8
        Barbara     F      65.3       98.0    Height         65.3

        James       M      57.3       83.0    Weight         83.0    = Weight
        John        M      59.0       99.5    Weight         99.5
        Jeffrey     M      62.5       84.0    Weight         84.0
        Henry       M      63.5      102.5    Weight        102.5
        Alfred      M      69.0      112.5    Weight        112.5


    PROCESS
    =======

      proc sql;
        create
           table want as
        select
          *
          ,"Weight"  as typ
          ,weight as both_height_and_weight
        from
           havOne
        union
        select
           *
          ,"Height"  as typ
          ,hgt as both_height_and_weight
        from
           haveTwo
        order
           by typ
      ;quit;


    OUTPUT
    ======

     WORK.WANT total obs=10
                                                        BOTH_HEIGHT_
        NAME       SEX    HEIGHT    WEIGHT     TYP       AND_WEIGHT

        Alice       F      56.5       84.0    Height         56.5    = Height
        Jane        F      59.8       84.5    Height         59.8
        Janet       F      62.5      112.5    Height         62.5
        Carol       F      62.8      102.5    Height         62.8
        Barbara     F      65.3       98.0    Height         65.3

        James       M      57.3       83.0    Weight         83.0    = Weight
        John        M      59.0       99.5    Weight         99.5
        Jeffrey     M      62.5       84.0    Weight         84.0
        Henry       M      63.5      102.5    Weight        102.5
        Alfred      M      69.0      112.5    Weight        112.5

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;


    data havOne;
       set sashelp.class(keep=name sex height weight obs=5 where=(sex='M'));
    run;quit;

    data haveTwo;
       set sashelp.class(keep=name sex height weight obs=5
          rename=(height=hgt weight=wgt)  where=(sex='F'));
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    *sas see process;

    * wps;
    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
      proc sql;
        create
           table wrk.wantwps as
        select
          *
          ,"Weight"  as typ
          ,weight as both_height_and_weight
        from
           wrk.havOne
        union
        select
           *
          ,"Height"  as typ
          ,hgt as both_height_and_weight
        from
           wrk.haveTwo
        order
           by typ
      ;quit;
    proc print;
    run;quit;
    ');

    proc print data=wantwps;
    run;quit;

