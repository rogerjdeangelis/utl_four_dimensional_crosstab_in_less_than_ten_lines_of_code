# utl_four_dimensional_crosstab_in_less_than_ten_lines_of_code
Four dimensional crosstab in  less than tem lines of code.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Four dimensional crosstab in less than tem lines of code

    github
    https://tinyurl.com/yboyfvwp
    https://github.com/rogerjdeangelis/utl_four_dimensional_crosstab_in_less_than_ten_lines_of_code

    https://tinyurl.com/y7cdqcar
    https://communities.sas.com/t5/ODS-and-Base-Reporting/using-Proc-Report/m-p/486264


    INPUT
    =====

    WORK.HAVE total obs=48

    Obs    STATUS     SHIFTS    GRP      YEAR    VAL

      1    UPCOME      FULL     COUNT    2013     75
      2    UPCOME      FULL     COUNT    2013     74
      3    UPCOME      FULL     COUNT    2014     29
      4    UPCOME      FULL     COUNT    2014     29
      5    UPCOME      FULL     WORK     2013     99
      6    UPCOME      FULL     WORK     2013     21
      7    UPCOME      FULL     WORK     2014     29
      8    UPCOME      FULL     WORK     2014     56
     ....
     45    FORCAST     PART     FT       2013     22
     46    FORCAST     PART     FT       2013     91
     47    FORCAST     PART     FT       2014     19
     48    FORCAST     PART     FT       2014      4


    EXAMPLE OUTPUT  (beautify later)
    ==================================

    WORK.HAVCHG total obs=6

                     COUNT___  COUNT___ COUNT___    FT___  FT___              WORK___  WORK___  WORK___
      LABEL            2013      2014      CHG       2013   2014  FT___CHG      2013     2014     CHG

      UPCOME * FULL     178       149       29       171    217      -46        106      141      -35
      UPCOME * PART     133       255     -122       119    153      -34        135      132        3
      Sum               311       404      -93       290    370      -80        241      273      -32

      FORCAST * FUL      99       201     -102        57     43       14        183      114       69
      FORCAST * PART    101       214     -113       221    193       28        176      221      -45
      Sum               200       415     -215       278    236       42        359      335       24

    PORCESS
    =======


    * use proc correst to sort, summarize and transpose;
    ods exclude all;

    ods output observed=havFor;
    proc corresp data=have(where=(status='FORCAST')) observed dim=1 cross=both;
    tables status shifts, grp year;weight val;run;quit;

    ods output observed=havUpc;
    proc corresp data=have(where=(status='UPCOME')) observed dim=1 cross=both;
    tables status shifts, grp year;weight val;run;quit;

    ods select all;

    * add the change column;
    data havChg(drop=sum);
     retain  label
         count___2013  ft___2013  work___2013
         count___2014  ft___2014  work___2014
         count___chg   ft___chg   work___chg;
      set havUpc havFor;
        count___chg = count___2013 - count___2014;
        ft___chg    = ft___2013    - ft___2014;
        work___chg  = work___2013  - work___2014;
    run;quit;


    OUTPUT
    ======

    WORK.HAVCHG total obs=6

                     COUNT___  COUNT___ COUNT___    FT___  FT___              WORK___  WORK___  WORK___
      LABEL            2013      2014      CHG       2013   2014  FT___CHG      2013     2014     CHG

      UPCOME * FULL     178       149       29       171    217      -46        106      141      -35
      UPCOME * PART     133       255     -122       119    153      -34        135      132        3
      Sum               311       404      -93       290    370      -80        241      273      -32

      FORCAST * FUL      99       201     -102        57     43       14        183      114       69
      FORCAST * PART    101       214     -113       221    193       28        176      221      -45
      Sum               200       415     -215       278    236       42        359      335       24

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data have;
      call streaminit(1234);
      do status='UPCOME ','FORCAST';
        do shifts='FULL','PART';
           do grp='COUNT','WORK','FT';
             do year='2013','2014';
               do rec=1 to 2;
                  val=int(100*rand('uniform'));
                  drop rec;
                  output;
               end;
             end;
           end;
        end;
      end;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process

