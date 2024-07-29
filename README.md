public int insertThirtyTwoDetails(ArrayList<RW32> reportBeanList, RW32 report, String branchCode, String user_Id,
			String quarter, String financial_year, String quarter_end_date, String circleCode, String report_master_id,
			String generatedSequence, String reportName, String reportId, String requestType) {
		// TODO Auto-generated method stub
		TransactionDefinition def = new DefaultTransactionDefinition();
		TransactionStatus status = transactionManager.getTransaction(def);

		
		int flag = -1;
		boolean isInserted = false;
		String dataFlag=report.getDataFlag();

		if (dataFlag.equalsIgnoreCase("U")) {
			boolean sts = makerService.getListOfTablesToReportMasterId(reportId, report_master_id);

		}


		if (!generatedSequence.equalsIgnoreCase("")) {

			try {
				String query = "insert into CRS_FACRED (FACRED_ID,FACRED_DATE,FACRED_BRANCH	,FACRED_CREDIR_DT,FACRED_SYSNO, "
						+ "FACRED_AMOUNT,FACRED_PAYEE,REPORT_MASTER_LIST_ID_FK,FACRED_BGL,FACRED_REASON) "
						+ "values(CER5_SEQ.nextVal,to_date(?,'dd/mm/yyyy'),?,to_date(?,'dd/mm/yyyy'),?,?,?,?,?,?)";
				for (RW32 ReportBean1 : reportBeanList) {

					flag = jdbcTemplateObject.update(query, new Object[] {

							quarter_end_date, branchCode, ReportBean1.getDateofCredit(), ReportBean1.getSystemNo(),
							ReportBean1.getAmountOutstand(), ReportBean1.getPayee(), generatedSequence,ReportBean1.getBglNo(),ReportBean1.getOsReason()
					});



				}
				if (flag > 0) {
					flag = 0;
				}

				transactionManager.commit(status);
			}

			catch(DataAccessException sqle){
				log.info("******** DataAccessException***********"+sqle);
				transactionManager.rollback(status);
				flag=2;
			}catch(TransactionException sqle){
				log.info("********* TransactionException*********");
				transactionManager.rollback(status);
				flag=2;
			}

		} else {
			flag = 2;
		}
		//log.info("existing insertDetails flag" + flag);

		return flag;
	}
