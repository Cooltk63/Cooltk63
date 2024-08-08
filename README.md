select  STND_ASTS_NAME_OF_BORROWER,  
        STND_ASTS_INFRA_NON_INFRA, 
        STND_ASTS_INFRA_WITHIN2YRS, 
           STND_ASTS_INFRA_ACCTS2YRS, 
           
           STND_ASTS_NONINFRA_WITHIN1YR,  
           STND_ASTS_NONINFRA_ACCTS1YR,
        
        STND_ASSETS_SEQ from CRS_STND_ASSETS ax  
         where REPORT_MASTER_LIST_ID_FK =:submissionId order by STND_ASSETS_SEQ;


I need the query with case where only show /select the columns based on some column value condition
condition as per below

when STND_ASTS_INFRA_NON_INFRA value='INFRA' then only select  STND_ASTS_INFRA_WITHIN2YRS, STND_ASTS_INFRA_ACCTS2YRS, this two columns along with STND_ASSETS_SEQ
else STND_ASTS_INFRA_NON_INFRA value='NONINFRA' then only select   STND_ASTS_NONINFRA_WITHIN1YR,  STND_ASTS_NONINFRA_ACCTS1YR, this two columns along with STND_ASSETS_SEQ please give me the query
