---
title: "날짜 처리 메소드"
layout: post
category: Java
tags: [Java]
excerpt: "날짜 및 시간을 처리하고 Date타입과 String 타입으로 변환가능한 유용한 메소드"
author: BAEKJungHo
---

* content
{:toc}

## 날짜 처리 메소드

  다음 날짜 처리 메소드들은 `FulfillmentService Project`를 진행하면 구현한 메소드입니다.

  ```java
  package util;

  import java.text.SimpleDateFormat;
  import java.time.LocalDateTime;
  import java.time.format.DateTimeFormatter;
  import java.util.Calendar;
  import java.util.Date;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;

  public class DateController {
	private static final Logger LOG = LoggerFactory.getLogger(DateController.class);
	// 어제 시간
	public String beforeTime() {
		LocalDateTime yTime = LocalDateTime.now();
		yTime = yTime.minusDays(1);
    	DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
    	return yTime.format(dtf);
	}

	// 어제 시간
	public String beforeDay() {
		LocalDateTime yDay = LocalDateTime.now();
		yDay = yDay.minusDays(1);
    	DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd");
    	return yDay.format(dtf);
	}

	// 전 달
	public String beforeMonth() {
		LocalDateTime yMonth = LocalDateTime.now();
		yMonth = yMonth.minusMonths(1);
    	DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM");
    	return yMonth.format(dtf);
	}

	// 어제 날짜
	public String yesterday() {
		LocalDateTime yesterday = LocalDateTime.now();
		yesterday = yesterday.minusDays(1);
    	DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd");
    	return yesterday.format(dtf);
	}

	// 현재 시간
	public String currentTime() {
		LocalDateTime cTime = LocalDateTime.now();
    	DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
    	return cTime.format(dtf);
	}

	// 현재 날짜
	public String getToday() {
		LocalDateTime today = LocalDateTime.now();
    	DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd");
    	return today.format(dtf);
	}

	// 현재 달
	public String getCurrentMonth() {
		LocalDateTime curMonth = LocalDateTime.now();
    	DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM");
    	return curMonth.format(dtf);
	}

	// 문자열을 날짜로 변환
	public Date transStringToDate(String date) {
		LOG.debug(date);
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm");
		Date cpDate = null;
		try {
			cpDate = sdf.parse(date);
			LOG.debug("cpDate : " + String.valueOf(cpDate));
		} catch(Exception e) {}
		return cpDate;
	}

	// 문자열을 날짜로 변환 yyyy-MM-dd
	public Date transStringToDateNotHours(String date) {
		LOG.debug(date);
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		Date cpDate = null;
		try {
			cpDate = sdf.parse(date);
			LOG.debug("cpDate : " + String.valueOf(cpDate));
		} catch(Exception e) {}
		return cpDate;
	}

	// 날짜를 문자열로 변환
	public String transDateToString(Date date) {
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm");
		String time = sdf.format(date);
		return time;
	}

	// 날짜를 문자열로 변환 yyyy-MM-dd
	public String transDateToStringNotHours(Date date) {
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String time = sdf.format(date);
		return time;
	}

	// 오전 오후 구하기
	public String getAmPm(Date time) {
		Calendar cal = Calendar.getInstance();
		cal.setTime(time);
		LOG.debug("getAmPm() : " + String.valueOf(cal.getTime()));
		if (cal.get(Calendar.AM_PM) == Calendar.AM)
			return "am";
		else if(cal.get(Calendar.AM_PM) == Calendar.PM)
			return "pm";
		return null;
	}

	// 오전 오후 시간
	public String getNumericTime(String amPm) {
		String timeStr = null;
		if (amPm.equals("am"))
			timeStr = " 09:00:00";
		if (amPm.equals("pm"))
			timeStr = " 18:00:00";
		return timeStr;
	}

	// 납품 전용 시간
	public String getReleaseTime(String time) {
		String releaseTime = null;
		if(time.compareTo(beforeDay()) == 0) {
			releaseTime = getToday() + " 10:00:00";
			return releaseTime;
		}
		return time;
	}

	// 청구 전용 시간
	public String getChargeTime(String time) {
		String chargeTime = null;
		if(time.equals(beforeMonth())) {
			chargeTime = getCurrentMonth() + "-01";
			return chargeTime;
		}
		return time;
	}

	// 지급 전용 시간
	public String getPayTime(String time) {
		String payTime = null;
		if(time.equals(beforeMonth())) {
			payTime = getCurrentMonth() + "-01";
			return payTime;
		}
		return time;
	 }
  }
  ```

## 단위 테스트

  ```java
  package utilTest;

  import static org.junit.Assert.assertEquals;

  import java.text.ParseException;
  import java.text.SimpleDateFormat;
  import java.util.Date;

  import org.junit.Before;
  import org.junit.Test;
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;

  import util.DateHandler;

  public class DateHandlerTest {
	private static final Logger LOG = LoggerFactory.getLogger(DateHandlerTest.class);
	private DateHandler dc = null;

	@Before
	public void beforeTest() {
		dc = new DateHandler();
		LOG.debug("beforeTest()");
	}

	@Test
	public void beforeTimeTest() {
		LOG.debug("beforeTimeTest()");
		assertEquals("2019-05-22 10:09", dc.beforeTime());
	}

	@Test
	public void beforeDayTest() {
		LOG.debug("beforeDayTest()");
		assertEquals("2019-05-22", dc.beforeDay());
	}

	@Test
	public void beforeMonthTest() {
		LOG.debug("beforeMonthTest()");
		assertEquals("2019-04", dc.beforeMonth());
	}

	@Test
	public void yesterdayTest() {
		LOG.debug("yesterdayTest()");
		assertEquals("2019-05-22", dc.yesterday());
	}

	@Test
	public void currentTimeTest() {
		LOG.debug("currentTimeTest()");
		assertEquals("2019-05-23 10:12", dc.currentTime());
	}

	@Test
	public void getTodayTest() {
		LOG.debug("getTodayTest()");
		assertEquals("2019-05-23", dc.getToday());
	}

	@Test
	public void getCurrentMonthTest() {
		LOG.debug("getCurrentMonthTest()");
		assertEquals("2019-05", dc.getCurrentMonth());
	}

	@Test
	public void transStringToDateTest() {
		LOG.debug("transStringToDateTest()");
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm");
		String time = "2019-05-23 10:20";
		Date date = null;
		try {
			date = sdf.parse(time);
		} catch (ParseException e) {
			LOG.debug("transStringToDateTest() ERROR");
		}
		assertEquals(date, dc.transStringToDate(time));
	}

	@Test
	public void transStringToDateNotHoursTest() {
		LOG.debug("transStringToDateNotHoursTest()");
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String time = "2019-05-23";
		Date date = null;
		try {
			date = sdf.parse(time);
		} catch (ParseException e) {
			LOG.debug("transStringToDateNotHoursTest() ERROR");
		}
		assertEquals(date, dc.transStringToDateNotHours(time));
	}

	@Test
	public void transDateToStringTest() {
		LOG.debug("transDateToStringTest()");
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm");
		String time = "2019-05-23 10:22";
		Date date = null;
		try {
			date = sdf.parse(time);
		} catch (ParseException e) {
			LOG.debug("transDateToStringTest() ERROR");
		}
		assertEquals(time, dc.transDateToString(date));
	}

	@Test
	public void transDateToStringNotHoursTest() {
		LOG.debug("transDateToStringNotHoursTest()");
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String time = "2019-05-23";
		Date date = null;
		try {
			date = sdf.parse(time);
		} catch (ParseException e) {
			LOG.debug("transDateToStringNotHoursTest() ERROR");
		}
		assertEquals(time, dc.transDateToStringNotHours(date));
	}

	@Test
	public void getAmPmTest() {
		LOG.debug("getAmPmTest()");
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm");
		String am = "2019-05-23 11:59";
		String pm = "2019-05-23 12:00";
		Date amDate = null;
		Date pmDate = null;
		try {
			amDate = sdf.parse(am);
			pmDate = sdf.parse(pm);
		} catch (ParseException e) {
			LOG.debug("getAmPmTest() ERROR");
		}
		assertEquals("am", dc.getAmPm(amDate));
		assertEquals("pm", dc.getAmPm(pmDate));
	}

	@Test
	public void getNumericTimeTest() {
		LOG.debug("getNumericTimeTest()");
		assertEquals(" 09:00:00", dc.getNumericTime("am"));
		assertEquals(" 18:00:00", dc.getNumericTime("pm"));
	}

	@Test
	public void getReleaseTimeTest() {
		LOG.debug("getReleaseTimeTest()");
		assertEquals("2019-05-23 10:00:00", dc.getReleaseTime("2019-05-22"));
	}

	@Test
	public void getChargeTimeTest() {
		LOG.debug("getChargeTimeTest()");
		assertEquals("2019-05-01", dc.getChargeTime("2019-04"));
	}

	@Test
	public void getPayTimeTest() {
		LOG.debug("getPayTimeTest()");
		assertEquals("2019-05-01", dc.getChargeTime("2019-04"));
	 }
  }
  ```
