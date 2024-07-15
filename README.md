select distinct BRANCHNO, BRANCHNO || case when  BR_NAME is null then ' ' else '-'  end || BR_NAME as BRNAME from BRANCH_MASTER where CIRCLE_CODE =:circleCode order by BRANCHNO
