# utl-remove-values-greater-than-the_97th-percentile
Remove values greater than the 97th percentile
    Remove values greater than the 97th percentile                                                                                        
                                                                                                                                          
    Slight variation of                                                                                                                   
                                                                                                                                          
    github                                                                                                                                
    https://tinyurl.com/y8umfo6n                                                                                                          
    https://github.com/rogerjdeangelis/utl-remove-values-greater-than-the_97th-percentile                                                 
                                                                                                                                          
    sas forum                                                                                                                             
    https://tinyurl.com/ya59mz3t                                                                                                          
    https://communities.sas.com/t5/SAS-Programming/Proc-univariate-with-observation-values-equal-to-zero-removed/m-p/647334               
                                                                                                                                          
    macros                                                                                                                                
    https://tinyurl.com/y9nfugth                                                                                                          
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                                            
                                                                                                                                          
    gather macro by                                                                                                                       
    Alea Iacta                                                                                                                            
    https://github.com/clindocu                                                                                                           
                                                                                                                                          
    *_                   _                                                                                                                
    (_)_ __  _ __  _   _| |_                                                                                                              
    | | '_ \| '_ \| | | | __|                                                                                                             
    | | | | | |_) | |_| | |_                                                                                                              
    |_|_| |_| .__/ \__,_|\__|                                                                                                             
            |_|                                                                                                                           
    ;                                                                                                                                     
                                                                                                                                          
    options validvarname=upcase;                                                                                                          
                                                                                                                                          
    libname sd1 "d:/sd1";                                                                                                                 
                                                                                                                                          
    data have;                                                                                                                            
                                                                                                                                          
      do rec=1 to 100;                                                                                                                    
        x1=rec;                                                                                                                           
        x2=10*rec;                                                                                                                        
        x3=100*rec;                                                                                                                       
        output;                                                                                                                           
      end;                                                                                                                                
      drop rec;                                                                                                                           
                                                                                                                                          
    run;quit;                                                                                                                             
                                                                                                                                          
    HAVE total obs=100                                                                                                                    
                                                                                                                                          
       X1     X2       X3                                                                                                                 
                                                                                                                                          
       1      10      100                                                                                                                 
       2      20      200                                                                                                                 
       3      30      300                                                                                                                 
     ....                                                                                                                                 
      95     950     9500                                                                                                                 
      96     960     9600                                                                                                                 
      97     970     9700                                                                                                                 
                                                                                                                                          
      98     980     9800   | RULES                                                                                                       
      99     990     9900   | Need to drop these 3 rows > 97.5 percentile                                                                 
     100    1000    10000   |                                                                                                             
                                                                                                                                          
    *            _               _                                                                                                        
      ___  _   _| |_ _ __  _   _| |_                                                                                                      
     / _ \| | | | __| '_ \| | | | __|                                                                                                     
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                      
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                     
                    |_|                                                                                                                   
    ;                                                                                                                                     
                                                                                                                                          
    I don't think you can safely transpose the general case                                                                               
    because the transpose can generate missig vales;                                                                                      
    This is a better data structure for most problems any way.                                                                            
    Pivot what you want or SQL join to get what you want.                                                                                 
                                                                                                                                          
    WORK.WANT total obs=300                                                                                                               
                                                                                                                                          
      VAR    VAL                                                                                                                          
                                                                                                                                          
      X1       1                                                                                                                          
      X1       2                                                                                                                          
      X1       3                                                                                                                          
    ...                                                                                                                                   
      X1      95                                                                                                                          
      X1      96                                                                                                                          
      X1      97                                                                                                                          
    ...             98,99 and 100 removed                                                                                                 
      X2      10                                                                                                                          
      X2      20                                                                                                                          
      X2      30                                                                                                                          
    ...                                                                                                                                   
      X2      950                                                                                                                         
      X2      960                                                                                                                         
      X2      970                                                                                                                         
     ..             980,990 and 1000 removed                                                                                              
      X3     1000                                                                                                                         
      X3     2000                                                                                                                         
      X3     3000                                                                                                                         
     ...                                                                                                                                  
      X3     9500                                                                                                                         
      X3     9600                                                                                                                         
      X3     9700                                                                                                                         
                   9800,9900 and 10000 removed                                                                                            
                                                                                                                                          
    *                                                                                                                                     
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                    
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                                   
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                   
    | .__/|_|  \___/ \___\___||___/___/                                                                                                   
    |_|                                                                                                                                   
    ;                                                                                                                                     
                                                                                                                                          
    * transpose;                                                                                                                          
    %utl_gather(sd1.have,var,val,,havxpo,valformat=6.);                                                                                   
                                                                                                                                          
    /*                                                                                                                                    
    WORK.HAVXPO total obs=300                                                                                                             
                                                                                                                                          
       VAR     VAL                                                                                                                        
                                                                                                                                          
       X1        1                                                                                                                        
       X2       10                                                                                                                        
       X3      100                                                                                                                        
       X1        2                                                                                                                        
       X2       20                                                                                                                        
       X3      200                                                                                                                        
      ...                                                                                                                                 
    */                                                                                                                                    
                                                                                                                                          
    * get percentiles;                                                                                                                    
    proc univariate data=havxpo noprint;                                                                                                  
    where val ne 0;                                                                                                                       
    class var;                                                                                                                            
    var val;                                                                                                                              
    output out=havPtl pctlpts=97 pctlpre=_ pctlname=pctl;                                                                                 
    run;                                                                                                                                  
                                                                                                                                          
    /*                                                                                                                                    
    WORK.HAVPTL total obs=3                                                                                                               
                                                                                                                                          
      VAR     _PCTL                                                                                                                       
                                                                                                                                          
      X1       97.5                                                                                                                       
      X2      975.0                                                                                                                       
      X3     9750.0                                                                                                                       
    */                                                                                                                                    
                                                                                                                                          
    * subset < 97.5 profile;                                                                                                              
    proc sql;                                                                                                                             
      create                                                                                                                              
          table want as                                                                                                                   
      select                                                                                                                              
          *                                                                                                                               
      from                                                                                                                                
          havXpo as l,  havPtl as r                                                                                                       
      where                                                                                                                               
          l.var = r.var     and                                                                                                           
          l.val le r._pctl  and                                                                                                           
          l.val ne .                                                                                                                      
      order                                                                                                                               
           by l.var, l.val                                                                                                                
    ;quit;                                                                                                                                
                                                                                                                                          
