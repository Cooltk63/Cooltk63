 Map<String, Object> ResultData = new HashMap<>();
        ResultData.put("quarterEndDate",loginUserData.get("qed"));
        ResultData.put("branchCode",loginUserData.get("branchCode"));
        ResultData.put("creditDate",data.get("creditDate"));
        ResultData.put("systemNo",data.get("systemNo"));
        ResultData.put("amountOutstand",data.get("amountOutstand"));
        ResultData.put("payee",data.get("payee"));
        ResultData.put("generatedSequence",data.get("generatedSequence"));
        ResultData.put("bglNo",data.get("bglNo"));
        ResultData.put("osReason",data.get("osReason"));


package com.crs.report.Model;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import lombok.Getter;
import lombok.Setter;


@Getter
@Setter
@Entity
@Table(name = "CRS_FACRED")
public class RW32 {

    @Id
    String facredid;
    String nilFlag;
    private String facreddate;
    private String facredbranch;
    private String facredcredir_dt;
    private String facredsysno;
    private String facredamount;
    private String facredpayee;
    private String reportmaster_list_id_fk;
    private String facredbgl;
    private String facredreason;

}
