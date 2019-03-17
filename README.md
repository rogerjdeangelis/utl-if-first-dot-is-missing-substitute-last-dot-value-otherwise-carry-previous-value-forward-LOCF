# utl-if-first-dot-is-missing-substitute-last-dot-value-otherwise-carry-previous-value-forward-LOCF
If first dot is missing substitute last dot value otherwise carry previous value forward 

    If first dot is missing substitute last dot value otherwise carry previous value forward                                            
                                                                                                                                        
    * This works for your sample but does not work for edge cases;                                                                      
                                                                                                                                        
    github                                                                                                                              
    http://tinyurl.com/y3tj7s6l                                                                                                         
    https://github.com/rogerjdeangelis/utl-if-first-dot-is-missing-substitute-last-dot-value-otherwise-carry-previous-value-forward-LOCF
                                                                                                                                        
    SAS Forum                                                                                                                           
    http://tinyurl.com/yyqj43sw                                                                                                         
    https://communities.sas.com/t5/SAS-Programming/how-assign-missing-values-when-first-occurence-or-last-occurence/m-p/543782          
                                                                                                                                        
    *_                   _                                                                                                              
    (_)_ __  _ __  _   _| |_                                                                                                            
    | | '_ \| '_ \| | | | __|                                                                                                           
    | | | | | |_) | |_| | |_                                                                                                            
    |_|_| |_| .__/ \__,_|\__|                                                                                                           
            |_|                                                                                                                         
    ;                                                                                                                                   
                                                                                                                                        
    data have;                                                                                                                          
     input country$ date$ amount ;                                                                                                      
    cards4;                                                                                                                             
    ind 1-Mar-19 345                                                                                                                    
    ind 2-Mar-19 .                                                                                                                      
    ind 3-Mar-19 463                                                                                                                    
    aus 1-Mar-19 .                                                                                                                      
    aus 2-Mar-19 357                                                                                                                    
    aus 3-Mar-19 500                                                                                                                    
    usa 1-Mar-19 345                                                                                                                    
    usa 2-Mar-19 222                                                                                                                    
    usa 3-Mar-19 463                                                                                                                    
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
                                                                                                                                        
    Up to 40 obs WORK.HAVE total obs=9    | RULES                                                                                       
                                          | -----                                                                                       
    Obs    COUNTRY      DATE      AMOUNT  |                                                                                             
                                                                                                                                        
     1       ind      1-Mar-19      345   |  345                                                                                        
     2       ind      2-Mar-19        .   |  345  Previous value forward                                                                
     3       ind      3-Mar-19      463   |  463                                                                                        
                                                                                                                                        
     4       aus      1-Mar-19        .   |  500 Use last because first.amount missing                                                  
     5       aus      2-Mar-19      357   |  257                                                                                        
     6       aus      3-Mar-19      500   |  500                                                                                        
                                                                                                                                        
     7       usa      1-Mar-19      345   |  345                                                                                        
     8       usa      2-Mar-19      222   |  222                                                                                        
     9       usa      3-Mar-19      463   |  463                                                                                        
                                                                                                                                        
    *            _               _                                                                                                      
      ___  _   _| |_ _ __  _   _| |_                                                                                                    
     / _ \| | | | __| '_ \| | | | __|                                                                                                   
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                    
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                   
                    |_|                                                                                                                 
    ;                                                                                                                                   
                                                                                                                                        
    Up to 40 obs WORK.WANT total obs=9                                                                                                  
                                                                                                                                        
    Obs    AMOUNT    COUNTRY      DATE                                                                                                  
                                                                                                                                        
     1       345       ind      1-Mar-19                                                                                                
     2       345       ind      2-Mar-19                                                                                                
     3       463       ind      3-Mar-19                                                                                                
     4       500       aus      1-Mar-19                                                                                                
     5       357       aus      2-Mar-19                                                                                                
     6       500       aus      3-Mar-19                                                                                                
     7       345       usa      1-Mar-19                                                                                                
     8       222       usa      2-Mar-19                                                                                                
     9       463       usa      3-Mar-19                                                                                                
                                                                                                                                        
    *          _       _   _                                                                                                            
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                                 
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                               
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                               
                                                                                                                                        
    ;                                                                                                                                   
                                                                                                                                        
    data want(drop=amount lstamt rename=savAmt=amount);                                                                                 
      retain lstAmt savAmt;                                                                                                             
      do until (last.country);                                                                                                          
        set have ;                                                                                                                      
        by country notsorted;                                                                                                           
        if last.country then lstAmt=amount;                                                                                             
      end;                                                                                                                              
      do until (last.country);                                                                                                          
        set have ;                                                                                                                      
        by country notsorted;                                                                                                           
        if first.country and missing(amount) then savAmt=lstAmt;                                                                        
        else if not missing(amount) then savAmt=amount;                                                                                 
        output;                                                                                                                         
      end;                                                                                                                              
    run;quit;                                                                                                                           
                                                                                                                                        
