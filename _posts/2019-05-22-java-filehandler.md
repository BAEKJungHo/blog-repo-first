---
title: "CSV FileHandler"
layout: post
category: Java
tags: [Java]
excerpt: "폴더 안에 있는 CSV 파일을 한 번에 읽고 처리하는 Java 파일 입니다."
author: BAEKJungHo
---

* content
{:toc}

## CSV FileHandler

  폴더안에 있는 CSV 파일을 한 번에 읽고, 읽은 파일은 백업(BackUp)시키기 위해 다른 폴더로 이동시키며,
  읽은 파일을 DB에 전부 저장하는 자바 소스 코드 입니다.

  아래는 `FulfillmentService Project`를 진행하면 만든 송장처리 파일핸들러 입니다.

  ```java
  package util;

  import java.io.BufferedReader;
  import java.io.File;
  import java.io.FileInputStream;
  import java.io.InputStreamReader;
  import java.util.ArrayList;
  import java.util.StringTokenizer;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;

  import admin.AdminDAO;
  import admin.AdminDTO;
  import invoice.InvoiceDAO;
  import invoice.InvoiceDTO;
  import invoice.InvoiceProductDTO;
  import storage.StorageDAO;
  import storage.StorageDTO;

  public class InvoiceHandler {
	private static final Logger LOG = LoggerFactory.getLogger(InvoiceHandler.class);
	ArrayList<InvoiceDTO> invoiceList = new ArrayList<InvoiceDTO>();
	ArrayList<InvoiceProductDTO> productList = new ArrayList<InvoiceProductDTO>();
	ArrayList<String> fullFileName = new ArrayList<String>();
	ArrayList<String> splitFileName = new ArrayList<String>();
	ArrayList<String> filePath = new ArrayList<String>();
	StorageDAO pDao = new StorageDAO();
	StorageDTO pDto = new StorageDTO();
	AdminDAO aDao = new AdminDAO();
	AdminDTO admin = new AdminDTO();
	InvoiceDTO invoice = new InvoiceDTO();
	InvoiceDAO vDao = new InvoiceDAO();
	InvoiceProductDTO product = new InvoiceProductDTO();

	// 송장 읽고 DB에 넣기
	public void readCSV() {
		// 해당 디렉터리에 있는 파일 목록 읽기
		String path = "C:\\Temp\\shop\\";
		File dir = new File(path);
		File[] fileList = dir.listFiles();

		int number = 0;
		for(File file : fileList) {
			int count = 0;
			if(file.isFile()) {
				fullFileName.add(file.getName());
				filePath.add(path+fullFileName.get(number));
				StringTokenizer st = new StringTokenizer(fullFileName.get(number), ".");
				splitFileName.add(st.nextToken());
				admin = aDao.getOneAdminByName(splitFileName.get(number).substring(12));
				LOG.debug(splitFileName.get(number).substring(0, 12));
				LOG.debug(splitFileName.get(number));
				invoice.setvId(createInvoiceNumber(splitFileName.get(number)));
				LOG.debug(createInvoiceNumber(splitFileName.get(number)));
				invoice.setvAdminId(admin.getaId());
				invoice.setvShopName(splitFileName.get(number).substring(12));
				String year = splitFileName.get(number).substring(0, 4);
				String month = splitFileName.get(number).substring(4, 6);
				String date = splitFileName.get(number).substring(6, 8);
				String hour = splitFileName.get(number).substring(8, 10);
				String minute = splitFileName.get(number).substring(10, 12);
				LOG.debug(year+"-"+month+"-"+date+" "+hour+":"+minute);
				invoice.setvDate(year+"-"+month+"-"+date+" "+hour+":"+minute);
				LOG.debug(splitFileName.get(number).substring(0, 12));
			}
			try {
				BufferedReader br = new BufferedReader(new InputStreamReader(
	                    new FileInputStream(filePath.get(number)), "euc-kr"));
	            String line = "";
	            int totalPrice = 0;
	            while ((line = br.readLine()) != null) { // 파일 읽기
	            	product.setpInvoiceId(invoice.getvId());
	                String[] token = line.split(",");
	                for(int p=0; p<5; p++) {
	                	if((token[p] != null) && !(token[p].equals(" ")) && !(token[p].equals(""))) {
	                		LOG.debug(token[p] + " ");
	                		System.out.println(token[p] + " ");
							if(p==0)invoice.setvName(token[p]);
							if(p==1)invoice.setvTel(token[p]);
							if(p==2) {
								invoice.setvAddress(token[p]);
								invoice.setVlogisId(selectLogis(token[p]));
							}
							if(p==3) {
								product.setIpProductId(Integer.parseInt(token[p]));
								pDto = pDao.getOneProductById(Integer.parseInt(token[p]));
							}
							if(p==4) {
								product.setIpQuantity(Integer.parseInt(token[p]));
								totalPrice += pDto.getpPrice()*product.getIpQuantity();
							}
	                	}
	                }
	                LOG.debug("readCSV실행");
	                if(count == 0) {
	                	vDao.addInvoice(invoice);
	                	count++;
	                	LOG.debug("count : " + count);
	                }
	                vDao.updateInvoicePrice(invoice.getvId(), totalPrice);
	                vDao.addInvoiceProduct(product);
	                count++;
	            }
				number++;
				br.close();
			}
	        catch (Exception e) {
	            e.printStackTrace();
	        }
		}
	}

  // 읽은 파일을 BACKUP
	public void moveFile() {
		File original_dir = new File("C:\\Temp\\shop");
		File move_dir = new File("C:\\Temp\\complete");

		if(original_dir.exists()) { // 폴더의 내용물 확인 -> 폴더 & 파일..
			File[] fileNames = original_dir.listFiles(); // 내용 목록 반환
			LOG.debug("--------------폴더 읽기-----------------");
			for(int i=0; i<fileNames.length; i++) {
				if(fileNames[i].isDirectory()) {
					System.out.println(fileNames[i].getName()); // 폴더 존재 유무
					}
				}
			LOG.debug("--------------파일 읽기-----------------");
			for(int i=0; i<fileNames.length; i++) {
				if(fileNames[i].isFile()) {
					if(fileNames[i].exists()) {
						if(original_dir.exists()) {
							File MoveFile = new File(move_dir, fileNames[i].getName()); // 이동될 파일 경로 및 파일 이름
							fileNames[i].renameTo(MoveFile); // 변경(이동)
							LOG.debug(fileNames[i].getName()); // 폴더내에 있는 파일 리스트
						}
					}
				}
			}
		}
	}

	// 송장 번호 생성
  public String createInvoiceNumber(String fileName) {
	ArrayList<InvoiceDTO> vList = new ArrayList<InvoiceDTO>();
	String date = fileName.substring(0, 8);
	String name = fileName.substring(12);
	admin = aDao.getOneAdminByName(name);
	LOG.debug(String.valueOf(admin.getaId()).substring(0, 2));
	String idNum = String.valueOf(admin.getaId()).substring(0, 2);
	String count = String.valueOf((int)((Math.random()*89)+10));
	String invoiceNumber = date + idNum + count;
	vList = vDao.getAllInvoiceLists();
	boolean complete = true;

	LOG.debug(vList.toString());
	// 중복 방지
	while(complete) {
		if(vList.isEmpty()) { complete = false; break; }
		for(InvoiceDTO invoice : vList) {
			if(invoice.getvId().equals(invoiceNumber)) {
				invoiceNumber = date + String.valueOf((int)((Math.random()*89)+10)) + idNum;
			} else {
				complete = false;
				break;
			}
		}
	}
	LOG.debug(String.valueOf(invoiceNumber));
	return invoiceNumber;
	}
  ```
