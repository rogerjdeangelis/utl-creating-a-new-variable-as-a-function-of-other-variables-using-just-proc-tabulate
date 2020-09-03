# utl-creating-a-new-variable-as-a-function-of-other-variables-using-just-proc-tabulate
Creating a new variable as a function of other variables using just proc tabulate
    Creating a new variable as a function of other variables using just proc tabulate;                                                                
                                                                                                                                                      
    I was unable to add a variable to tabulate output when the variable was not in the input table.                                                   
                                                                                                                                                      
        Two Solutions                                                                                                                                 
                                                                                                                                                      
            a. Prep data using a sas view                                                                                                             
            b. Using dosubl (cannot use a view inside 'proc tabulate'?)                                                                               
               Allos you to keep the code together.                                                                                                   
                                                                                                                                                      
    github                                                                                                                                            
    https://tinyurl.com/yxcwcoyv                                                                                                                      
    https://github.com/rogerjdeangelis/utl-creating-a-new-variable-as-a-function-of-other-variables-using-just-proc-tabulate                          
                                                                                                                                                      
    stackoverflow                                                                                                                                     
    https://tinyurl.com/y5yd4ejo                                                                                                                      
    https://stackoverflow.com/questions/63618079/in-proc-tabulate-access-multiple-variable-values-at-once                                             
                                                                                                                                                      
    Perhaps it is not a good idea to try to create variables inside proc tabulate or                                                                  
    use complex across variable computations with proc report.                                                                                        
    Code is often hard to maintain, difficult to update snd less flexible.                                                                            
                                                                                                                                                      
    I think this may be possible using proc report but I would not do it.                                                                             
    Adding rows this way is tricky in 'proc report'.                                                                                                  
    Very simple to do do with a an additional datastep view.                                                                                          
                                                                                                                                                      
    Op wanted to asummarize variable, score_1_or_score_2, without the variable in the imput table.                                                    
    Score_1 and Score_2 were the only variables in the input dataset.                                                                                 
                                                                                                                                                      
    My solution provides a 'proc tabulate' output dataset and a static listing                                                                        
                                                                                                                                                      
    ----------------------------------------------                                                                                                    
    |SCORE_1                       ||       TOTAL|                                                                                                    
    |------------------------------+|            |                                                                                                    
    |non-critical                  ||        1.00|                                                                                                    
    |------------------------------++------------|                                                                                                    
    |critical                      ||        2.00|                                                                                                    
    |------------------------------++------------|                                                                                                    
    |SCORE_2                       ||            |                                                                                                    
    |------------------------------+|            |                                                                                                    
    |non-critical                  ||        1.00|                                                                                                    
    |------------------------------++------------|                                                                                                    
    |critical                      ||        2.00|                                                                                                    
    |------------------------------++------------|                                                                                                    
                                                                                                                                                      
    Op wants to add this section given only SCORE_1 and SCORE_2                                                                                       
                                                                                                                                                      
    ---------------------------------------------                                                                                                     
    |SCORE_1_OR_SCORE_2            ||            | *** ADD this section                                                                               
    |------------------------------+|            |                                                                                                    
    |non-critical                  ||           0|                                                                                                    
    |------------------------------++------------|                                                                                                    
    |critical                      ||        3.00|                                                                                                    
    ----------------------------------------------                                                                                                    
                                                                                                                                                      
    /*                   _                                                                                                                            
    (_)_ __  _ __  _   _| |_                                                                                                                          
    | | `_ \| `_ \| | | | __|                                                                                                                         
    | | | | | |_) | |_| | |_                                                                                                                          
    |_|_| |_| .__/ \__,_|\__|                                                                                                                         
            |_|                                                                                                                                       
    */                                                                                                                                                
                                                                                                                                                      
    proc format;                                                                                                                                      
    value cut_fmt                                                                                                                                     
        low - 2 = 'non-critical'                                                                                                                      
        3 - high = 'critical'                                                                                                                         
        ;                                                                                                                                             
    run;quit;                                                                                                                                         
                                                                                                                                                      
                                                                                                                                                      
    /*                                                                                                                                                
    WORK.HAVE total obs=3                                                                                                                             
                                  SCORE_                                                                                                              
                                  1_OR_                                                                                                               
    Obs    SCORE_1    SCORE_2    SCORE_2                                                                                                              
                                                                                                                                                      
     1        2          7          3                                                                                                                 
     2        4          4          3                                                                                                                 
     3        7          2          3                                                                                                                 
    */                                                                                                                                                
                                                                                                                                                      
    /*           _               _                                                                                                                    
      ___  _   _| |_ _ __  _   _| |_ ___                                                                                                              
     / _ \| | | | __| `_ \| | | | __/ __|                                                                                                             
    | (_) | |_| | |_| |_) | |_| | |_\__ \                                                                                                             
     \___/ \__,_|\__| .__/ \__,_|\__|___/                                                                                                             
                    |_|                                                                                                                               
    */                                                                                                                                                
                                                                                                                                                      
                                                                                                                                                      
    ----------------------------------------------                                                                                                    
    |SCORE_1                       ||            |                                                                                                    
    |------------------------------+|            |                                                                                                    
    |non-critical                  ||        1.00|                                                                                                    
    |------------------------------++------------|                                                                                                    
    |critical                      ||        2.00|                                                                                                    
    |------------------------------++------------|                                                                                                    
    |SCORE_2                       ||            |                                                                                                    
    |------------------------------+|            |                                                                                                    
    |non-critical                  ||        1.00|                                                                                                    
    |------------------------------++------------|                                                                                                    
    |critical                      ||        2.00|                                                                                                    
    |--------------------------------------------|                                                                                                    
    |SCORE_1_OR_SCORE_2            ||            |                                                                                                    
    |------------------------------+|            |                                                                                                    
    |non-critical                  ||           0|                                                                                                    
    |------------------------------++------------|                                                                                                    
    |critical                      ||        3.00|                                                                                                    
    ----------------------------------------------                                                                                                    
                                                                                                                                                      
                                                                                                                                                      
    SAS TABLE                                                                                                                                         
    =========                                                                                                                                         
                                                                                                                                                      
    WORK.WANT total obs=9                                                                                                                             
                                                                                                                                                      
    O  SCORES                SPACE    TOTAL                                                                                                           
                                                                                                                                                      
       SCORE_1                          0                                                                                                             
       non-critical            N        1                                                                                                             
       critical                N        2                                                                                                             
       SCORE_2                          0                                                                                                             
       non-critical            N        1                                                                                                             
       critical                N        2                                                                                                             
       SCORE_1_OR_SCORE_2               0                                                                                                             
       non-critical            N        0                                                                                                             
       critical                N        3                                                                                                             
                                                                                                                                                      
                Variables in Creation Order                                                                                                           
                                                                                                                                                      
    #    Variable    Type    Len    Format     Informat                                                                                               
                                                                                                                                                      
    1    SCORES      Char     18    $18.       $18.                                                                                                   
    2    SPACE       Char      1    $1.        $1.                                                                                                    
    3    TOTAL       Num       8    BEST12.    BEST32.   ** numeric even though missing='0'                                                           
                                                                                                                                                      
    /*                                                                                                                                                
    /*         _       _   _                                                                                                                          
     ___  ___ | |_   _| |_(_) ___  _ __  ___                                                                                                          
    / __|/ _ \| | | | | __| |/ _ \| `_ \/ __|                                                                                                         
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \                                                                                                         
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/                                                                                                         
      __ _     _ __  _ __ ___ _ __     __| | __ _| |_ __ _                                                                                            
     / _` |   | `_ \| `__/ _ \ `_ \   / _` |/ _` | __/ _` |                                                                                           
    | (_| |_  | |_) | | |  __/ |_) | | (_| | (_| | || (_| |                                                                                           
     \__,_(_) | .__/|_|  \___| .__/   \__,_|\__,_|\__\__,_|                                                                                           
              |_|            |_|                                                                                                                      
    */                                                                                                                                                
                                                                                                                                                      
                                                                                                                                                      
    data have;                                                                                                                                        
      input score_1 score_2;                                                                                                                          
       if score_1>2 or score_2>2 then score_1_or_score_2=3;                                                                                           
       else                           score_1_or_score_2=1;                                                                                           
    cards4;                                                                                                                                           
    2 7                                                                                                                                               
    4 4                                                                                                                                               
    7 2                                                                                                                                               
    ;;;;                                                                                                                                              
    run;quit                                                                                                                                          
                                                                                                                                                      
    * PREP HAVE;                                                                                                                                      
                                                                                                                                                      
    data havVue/view=havVue;                                                                                                                          
                                                                                                                                                      
      set have;                                                                                                                                       
      if score_1>2 or score_2>2 then score_1_or_score_2=3;                                                                                            
      else                           score_1_or_score_2=1;                                                                                            
                                                                                                                                                      
    run;quit;                                                                                                                                         
                                                                                                                                                      
                                                                                                                                                      
    %utl_odsrpt(setup);                                                                                                                               
                                                                                                                                                      
    options missing='0';                                                                                                                              
                                                                                                                                                      
    proc odstext; p '|scores|space|total';run;quit;  * provide column names;                                                                          
                                                                                                                                                      
    proc tabulate data = have missing ;                                                                                                               
                                                                                                                                                      
       class score_1 score_2 score_1_or_score_2 / order=data preloadfmt ;                                                                             
       keylabel N = '';                                                                                                                               
                                                                                                                                                      
       table                                                                                                                                          
          score_1 * N*f=5.                                                                                                                            
          score_2 * N*f=5.                                                                                                                            
          score_1_or_score_2 * N*f=5.                                                                                                                 
       , all="" / printmiss ;                                                                                                                         
                                                                                                                                                      
       format score_1 score_2 score_1_or_score_2  cut_fmt.;                                                                                           
                                                                                                                                                      
    run;quit;                                                                                                                                         
                                                                                                                                                      
    %utl_odsrpt(outdsn=want);                                                                                                                         
                                                                                                                                                      
    options FORMCHAR='|----|+|---+=|-/\<>*';                                                                                                          
                                                                                                                                                      
    /*                                                                                                                                                
    * options to create long and wide tabulate output;                                                                                                
    * Is need to make the tabulate output to look retangular                                                                                          
    so you can map the output to a sas dataset/                                                                                                       
                                                                                                                                                      
    options papersize=(10in 20in);                                                                                                                    
    options ls=255 ps=500;                                                                                                                            
    */                                                                                                                                                
                                                                                                                                                      
    /*            _                 _     _                                                                                                           
    | |__      __| | ___  ___ _   _| |__ | |                                                                                                          
    | `_ \    / _` |/ _ \/ __| | | | `_ \| |                                                                                                          
    | |_) |  | (_| | (_) \__ \ |_| | |_) | |                                                                                                          
    |_.__(_)  \__,_|\___/|___/\__,_|_.__/|_|                                                                                                          
                                                                                                                                                      
    */                                                                                                                                                
                                                                                                                                                      
    * this encapsulates the view with the tabulate code;                                                                                              
                                                                                                                                                      
    %utl_odsrpt(setup);                                                                                                                               
                                                                                                                                                      
    options missing='0';                                                                                                                              
                                                                                                                                                      
    proc odstext; p '|scores|space|total';run;quit;  * provide column names;                                                                          
                                                                                                                                                      
    proc tabulate data = havV ( where = ( 0 = %qsysfunc(dosubl('                                                                                      
                   data havV;                                                                                                                         
                      set have;                                                                                                                       
                      if score_1>2 or score_2>2 then score_1_or_score_2=3;                                                                            
                      else                           score_1_or_score_2=1;                                                                            
                   run;quit;                                                                                                                          
                   '))))                                                                                                                              
                   missing ;                                                                                                                          
                                                                                                                                                      
       class score_1 score_2 score_1_or_score_2 / order=data preloadfmt ;                                                                             
       keylabel N = '';                                                                                                                               
                                                                                                                                                      
       table                                                                                                                                          
          score_1 * N*f=5.                                                                                                                            
          score_2 * N*f=5.                                                                                                                            
          score_1_or_score_2 * N*f=5.                                                                                                                 
       , all="" / printmiss ;                                                                                                                         
                                                                                                                                                      
       format score_1 score_2 score_1_or_score_2  cut_fmt.;                                                                                           
                                                                                                                                                      
    run;quit;                                                                                                                                         
                                                                                                                                                      
                                                                                                                                                      
    %utl_odsrpt(outdsn=want);                                                                                                                         
                                                                                                                                                      
                                                                                                                                                      
