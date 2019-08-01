---
title: "Business Logic"
layout: post
category: PROJECT
tags: [Java, MySQL, JSP, Servlet]
excerpt: "Business Logic에 대해서 배워 봅시다"
author: BAEKJungHo
---

* content
{:toc}

## Business Logic(Service)

  __비지니스 로직(Business Logic)은 서비스(Service)라고도 합니다.__

  > Business Logic : 업무 절차를 정보 시스템으로 구현하기 위한 자료구조와 알고리즘

  - 비지니스 로직?

    - 비지니스 로직이란 예를들어 `발주`라는 기능을 만들기 위해서 사용자(User) 입장에서는
    화면에 나와있는 발주 버튼을 클릭만 하면 되지만, 발주가 동작하기 위해서는 다음과 같은 조건들이 필요합니다.
      - 제품 상태(재고부족, 재고부족예상 등)
      - 시간
      - 발주 상태(이미완료된 물품인지 아닌지)

  즉, 발주라는 업무 절차를 코드로 구현하는 것이 비지니스 로직입니다.

  Business Logic을 제대로 이해하지 못하고 코드를 작성하게 되면 Model(DAO)나 Controller 부분에서
  `Business Logic`을 처리하는 메소드나 코드를 담게 되며, 그렇게 되면 코드가 지저분해지고 재사용성과,
  유지보수성이 낮아집니다.

### 동작 과정

  스프링을 사용하지않은 JSP 프로젝트에서 Model, View, Controller, Business Logic의 동작과정은 다음과 같습니다.

  > View - Controller - Business Logic - Model - Controller - View
  >
  > View - Controller - Model - Controller - View

  스프링을 사용하게 된다면 동작과정은 다음과 같습니다.

  > Mapper.xml - DTO - DAO Interface - (DAOImpl 생략가능) - Service Interface - ServiceImpl - Controller - View

### 스프링에서 Business Logic은 어디에서 구현해야 할까?

  맨위에서 Business Logic을 Service라고 했습니다. 즉, Business Logic은 Service에서 구현해야 합니다.

  Service Interafce에 Business Logic을 위한 추상메서드를 만들고, ServiceImpl에서 구현해서 사용하면 됩니다.

  __최대한 Controller는 깔끔하게 가져가야 합니다.__

  Service에 Business Logic을 두는 이유는 유지보수성과 재사용성 때문입니다. 만약 다른 패키지의 컨트롤러에서도 같은 비지니스 로직을 사용하게 된다면, 해당 비지니스 로직은 유틸성을 가지고 있으므로, Utils 패키지에 따로 클래스로 빼내어 만들어 사용하면 됩니다.

## EXAMPLE

  - Business Logic

  ```java
  public class OrderHandler {
	private static final Logger LOG = LoggerFactory.getLogger(OrderHandler.class);
	DateHandler dc = new DateHandler();
	OrderDAO oDao = new OrderDAO();
	OrderDTO order = new OrderDTO();

	ArrayList<OrderDTO> oList = new ArrayList<OrderDTO>();

	// 제품 상태에 따른 발주 가능 여부 판단 메소드
	public boolean isPossibleOrderByProductState(String pState) {
		ProductState state = ProductState.valueOf(pState);
		LOG.debug(String.valueOf(state));
		switch(state) {
		case P :
			return false;
		case 재고부족 :
			return true;
		case 재고부족예상 :
			return true;
		}
		return false;
	}

	// 발주 상태에 따른 납품 가능 여부 판단 메소드
	public boolean isPossibleOrderByOrderState(String oState) {
		OrderState state = OrderState.valueOf(oState);
		LOG.debug(String.valueOf(state));
		switch(state) {
		case 구매요청 :
			return true;
		case 공급실행 :
			return false;
		case 구매확인요청 :
			return false;
		case 구매확정 :
			return false;
		}
		return false;
	}

	// 납품 가능시간 여부 판단 메소드
	public boolean isPossibleReleaseByDate(String date) {
		String strDate = dc.getReleaseTime(date.substring(0, 10));
		if(date.compareTo(strDate) >= 0) return false;
		else return true;
	}

	// 발주
	public void processOrder(int pSupplierId, int pId, int oQuantity, int oTotalPrice, String pState) {
		if(isPossibleOrderByProductState(pState) == true) {
			order = new OrderDTO(pSupplierId, pId, oQuantity, oTotalPrice, dc.currentTime());
			LOG.debug(order.toString());
			oDao.addOrderProducts(order);
		}
	}

	// 납품
	public void processRelease(int oAdminId, String date) {
		oList = oDao.getAllOrderLists(oAdminId, date);
		for(OrderDTO order : oList) {
			if(isPossibleReleaseByDate(order.getoDate())) {
				if(isPossibleOrderByOrderState(order.getoState()))
					oDao.updateOrderState(String.valueOf(OrderState.공급실행), order.getoId(), dc.currentTime());
			}
		}
	 }
  }
  ```
