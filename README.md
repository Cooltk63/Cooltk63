"select " +
            "ax.STND_ASTS_NAME_OF_BORROWER, ax.STND_ASTS_INFRA_NON_INFRA, ax.STND_ASTS_INFRA_WITHIN2YRS, \n" +
            "ax.STND_ASTS_INFRA_ACCTS2YRS, ax.STND_ASTS_NONINFRA_WITHIN1YR, ax.STND_ASTS_NONINFRA_ACCTS1YR,ax.STND_ASSETS_SEQ  " +
            "from CRS_STND_ASSETS ax \n" +
            "where ax.REPORT_MASTER_LIST_ID_FK =:submissionId " +
            "order by ax.STND_ASSETS_SEQ"
