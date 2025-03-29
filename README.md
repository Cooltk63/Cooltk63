String pdfFileName = CleanPath.cleanString(tempoutFilePath + File.separator + fileName.split("\\.")[0] + ".pdf");
                log.info("333333 >> " + pdfFileName);
                log.info("333333 >> " + System.currentTimeMillis());


                try (
                        PdfWriter pdfWriter = new PdfWriter(pdfFileName);
                        PdfDocument pdfDocument = new PdfDocument(pdfWriter);
                        Document document = new Document(pdfDocument, PageSize.A3.rotate());
                        BufferedReader br = new BufferedReader(new FileReader(serverFilePath));
                ) {

                    PdfFont monoFont = PdfFontFactory.createFont(StandardFonts.COURIER);
                    document.setFont(monoFont);
                    document.setFontSize(10);
                    String line;
                    while ((line = br.readLine()) != null) {
                       // document.add(new Paragraph(line.trim()));//.setTextAlignment(TextAlignment.JUSTIFIED));
                        document.add(new Paragraph(line));//.setTextAlignment(TextAlignment.JUSTIFIED));
//                        log.info("line >>>>>>>>>>>>>>>> " + line);
//                        lines.add(line);
                    }

                    log.info("PDF FILE created successfuly");
                }
