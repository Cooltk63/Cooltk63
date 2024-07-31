@Override
    public ResponseEntity saveReportAndGetDisplayMessage(Map<String, Object> map) {

        ResponseVO<Integer> responseVO = new ResponseVO<>();

        try {
            // Save report values Here for Update DataTable
            int result = updateDataTable(map);

            responseVO.setStatusCode(HttpStatus.OK.value());
            responseVO.setMessage("Data Saved Successfully");
            responseVO.setResult(result);
        } catch (NullPointerException e) {
            log.info("NullPointer Exception Occurred: " + e.getCause());
            responseVO.setStatusCode(HttpStatus.INTERNAL_SERVER_ERROR.value());
            responseVO.setMessage("Getting Error Message " + e.getMessage());
        }

        return new ResponseEntity<>(responseVO, HttpStatus.OK);
    }

    // Used for Save the report values & Update the Data Tables
    private int updateDataTable(Map<String, Object> map) {

        Map<String, String> loginUserData = (Map<String, String>) map.get("user");
        Map<String, String> data = (Map<String, String>) map.get("data");

        // Check if the data flag indicates an update
        /* if (dataFlag.equalsIgnoreCase("U")) {
           boolean sts = makerService.getListOfTablesToReportMasterId(reportId, report_master_id);
            // Handle the result of sts if needed
        }*/

        try {

            // Assign data to Bean for Save
            RW32 entity = new RW32();
            entity.setFacredid(31917);
            entity.setFacreddate(loginUserData.get("qed"));
            entity.setFacredbranch(loginUserData.get("branchCode"));
            entity.setFacredcredir_dt(data.get("creditDate"));
            entity.setFacredsysno(data.get("systemNo"));
            entity.setFacredamount(data.get("amountOutstand"));
            entity.setFacredpayee(data.get("payee"));
            entity.setFacredbgl(data.get("bglNo"));
            entity.setFacredreason(data.get("osReason"));
            entity.setReportmaster_list_id_fk(data.get("reportmaster_Id"));

            // Sending data for Save Report-32
            RW32 rw32 = rw32Repository.save(entity);
            log.info("Data we Get ::" + rw32.getFacredid());

            return rw32.getFacredid();
           
        } catch (NullPointerException e) {
            log.info("Error Occurred while updateDataTable getCause" + e.getCause());
            log.info("UpdateDataTable get error Message" + e.getMessage());
            return 0;
        }
