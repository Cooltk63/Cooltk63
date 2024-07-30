import org.springframework.data.jpa.repository.Modifying;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.CrudRepository;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;

@Repository
public interface CrsFacredRepository extends CrudRepository<CrsFacred, Long> {

    /**
     * Inserts a record into the CRS_FACRED table.
     * 
     * @param quarterEndDate       The end date of the quarter.
     * @param branchCode           The branch code.
     * @param creditDate           The credit date.
     * @param systemNo             The system number.
     * @param amountOutstand       The outstanding amount.
     * @param payee                The payee.
     * @param generatedSequence    The generated sequence identifier.
     * @param bglNo                The BGL number.
     * @param osReason             The reason for outstanding.
     */
    @Modifying
    @Transactional
    @Query(value = "INSERT INTO CRS_FACRED (FACRED_ID, FACRED_DATE, FACRED_BRANCH, FACRED_CREDIR_DT, FACRED_SYSNO, "
                 + "FACRED_AMOUNT, FACRED_PAYEE, REPORT_MASTER_LIST_ID_FK, FACRED_BGL, FACRED_REASON) "
                 + "VALUES (CER5_SEQ.NEXTVAL, TO_DATE(:quarterEndDate, 'dd/mm/yyyy'), :branchCode, TO_DATE(:creditDate, 'dd/mm/yyyy'), "
                 + ":systemNo, :amountOutstand, :payee, :generatedSequence, :bglNo, :osReason)",
           nativeQuery = true)
    void insertFacred(@Param("quarterEndDate") String quarterEndDate,
                      @Param("branchCode") String branchCode,
                      @Param("creditDate") String creditDate,
                      @Param("systemNo") String systemNo,
                      @Param("amountOutstand") String amountOutstand,
                      @Param("payee") String payee,
                      @Param("generatedSequence") String generatedSequence,
                      @Param("bglNo") String bglNo,
                      @Param("osReason") String osReason);
}

repo
xxxxxx


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class YourService {

    @Autowired
    private CrsFacredRepository crsFacredRepository;

    /**
     * Inserts multiple records into the CRS_FACRED table.
     * 
     * @param reportBeanList       List of RW32 beans containing data to insert.
     * @param report               The report object containing metadata.
     * @param branchCode           The branch code.
     * @param user_Id              The user ID.
     * @param quarter              The quarter.
     * @param financial_year       The financial year.
     * @param quarter_end_date     The end date of the quarter.
     * @param circleCode           The circle code.
     * @param report_master_id     The report master ID.
     * @param generatedSequence    The generated sequence identifier.
     * @param reportName           The report name.
     * @param reportId             The report ID.
     * @param requestType          The request type.
     * @return                    The status flag indicating success or failure.
     */
    @Transactional
    public int insertThirtyTwoDetails(ArrayList<RW32> reportBeanList, RW32 report, String branchCode, String user_Id,
                                      String quarter, String financial_year, String quarter_end_date, String circleCode,
                                      String report_master_id, String generatedSequence, String reportName, String reportId,
                                      String requestType) {

        int flag = -1; // Initialize flag to indicate status: -1 means not processed yet
        String dataFlag = report.getDataFlag();

        // Check if the data flag indicates an update
        if (dataFlag.equalsIgnoreCase("U")) {
            boolean sts = makerService.getListOfTablesToReportMasterId(reportId, report_master_id);
            // Handle the result of sts if needed
        }

        // Ensure the generated sequence is not empty before proceeding
        if (!generatedSequence.equalsIgnoreCase("")) {
            try {
                // Iterate over each RW32 bean and insert data into CRS_FACRED table
                for (RW32 reportBean : reportBeanList) {
                    crsFacredRepository.insertFacred(
                        quarter_end_date, 
                        branchCode, 
                        reportBean.getDateofCredit(), 
                        reportBean.getSystemNo(), 
                        reportBean.getAmountOutstand(), 
                        reportBean.getPayee(), 
                        generatedSequence, 
                        reportBean.getBglNo(), 
                        reportBean.getOsReason()
                    );
                }
                flag = 0;  // Set flag to 0 to indicate successful operation

            } catch (Exception e) {
                log.info("Exception occurred: " + e);
                flag = 2;  // Set flag to 2 to indicate failure
            }
        } else {
            flag = 2;  // Set flag to 2 to indicate failure due to empty sequence
        }

        return flag;  // Return the status flag
    }
}