SELECT
    STND_ASTS_NAME_OF_BORROWER,
    STND_ASTS_INFRA_NON_INFRA,
    CASE
        WHEN STND_ASTS_INFRA_NON_INFRA = 'INFRA' THEN STND_ASTS_INFRA_WITHIN2YRS
        ELSE 0
    END AS STND_ASTS_INFRA_WITHIN2YRS,
    CASE
        WHEN STND_ASTS_INFRA_NON_INFRA = 'INFRA' THEN STND_ASTS_INFRA_ACCTS2YRS
        ELSE 0
    END AS STND_ASTS_INFRA_ACCTS2YRS,
    CASE
        WHEN STND_ASTS_INFRA_NON_INFRA = 'NONINFRA' THEN STND_ASTS_NONINFRA_WITHIN1YR
        ELSE 0
    END AS STND_ASTS_NONINFRA_WITHIN1YR,
    CASE
        WHEN STND_ASTS_INFRA_NON_INFRA = 'NONINFRA' THEN STND_ASTS_NONINFRA_ACCTS1YR
        ELSE 0
    END AS STND_ASTS_NONINFRA_ACCTS1YR,
    STND_ASSETS_SEQ
FROM CRS_STND_ASSETS ax
WHERE REPORT_MASTER_LIST_ID_FK = :submissionId
ORDER BY STND_ASSETS_SEQ;