---
title: "JACKSON ObjectMapper"
layout: post
category: JSON
tags: [JSON]
excerpt: "JACKSON ObjectMapper에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > JACKSON에서 지원하는 ObjectMapper 클래스에 대해서 배워 봅시다.

## JACKSON ObjectMapper

  > JACKSON ObjectMapper
  >
  > [Baeldung JACKSON ObjectMapper](https://www.baeldung.com/jackson-object-mapper-tutorial)
  >
  > [Jenkov Tutorials JACKSON ObjectMapper](https://www.google.com/search?q=objectmapper.readvalue&oq=objectMapper.&aqs=chrome.1.69i57j0l5.6256j0j4&sourceid=chrome&ie=UTF-8)

  스프링은 JSON Format을 사용하기 위해서는 JACKSON 라이브러리를 사용하는데, 사용하기 위해서는 스프링에서는 수동으로 등록해야 하며, 스프링 부트는 기본으로 포함 되어 있습니다.


### readValue()

  ```java
  // arg : 변환할 대상
  // type : arg를 어떤 타입으로 변환할 것인지 클래스를 명시
  // type에는 Class 객체, TypeReference 객체가 올 수 있다.
  readValue(arg, type)
  ```

  ```java
  mapper.readValue(arg, ArrayList.class);
  mapper.readValue(arg, new ArrayList<HashMap<String, String>>().getClass());
  mapper.readValue(arg, new TypeReference<ArrayList<HashMap<String, String>>>(){});
  ```

### writeValueAsString()

  ```java
  // value : String 타입으로 변환할 대상
  writeValueAsString(value)
  ```

### Jacson Dependency

  ```xml
<dependency>
  <groupId>org.codehaus.jackson</groupId>
  <artifactId>jackson-mapper-asl</artifactId>
  <version>1.9.13</version>
</dependency>
  ```

  ```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.8</version>
</dependency>
  ```

## OBJECT Convert to JSON String Example

### POJO

  ```java
  import java.util.List;

  @Getter @Setter
  public class User {
       private String name;
       private int age;
       private List<String> messages;
  }
  ```

### Java Object to JSON

  ```java
public class JacksonExample {
  public static void main(String[] args) {
      ObjectMapper objMapper = new ObjectMapper();

      // For testing
      User user = createDummyUser();

      try {
          // Convert object to JSON string and save into file directly
          objMapper.writeValue(new File("D:\\user.json"), user);

          // Convert ojbect ot JSON string
          String jsonString = objMapper.writeValueAsString(user);

          // Convert object to JSON string and pretty print
          jsonString = objMapper.writerWithDefaultPrettyPrinter().writeValueAsString(user);
      } catch (JsonGenerationException e) {
          throw new RuntimeException();
      } catch (JsonMappingException e) {
          throw new RumtimeException();
      } catch (IOException e) {
          throw new RuntimeException();
      }
  }
}
  ```

### Output

  ```
  //new json file is created in D:\\user.json"
  {"name":"BAEKJH","age":33,"messages":["hello jackson 1","hello jackson 2","hello jackson 3"]}

  { "name" : "mkyong",
     "age" : 33,
     "messages" : [ "hello jackson 1", "hello jackson 2", "hello jackson 3" ]
  }
  ```

### JSON to Java Object

  ```java
  public class JacksonExample {
      public static void main(String[] args) {
          ObjectMapper objMapper = new ObjectMapper();

          try {
              // Convert JSON string from file to Object
              User user = objMapper.readValue(new File)

              // Convert JSON string to Object
              String jsonString = "{\"age\":26,\"messages\":[\"msg 1\",\"msg 2\"],\"name\":\"BAERKJH\"}";
              User userr = objMapper.readValue(jsonInStrting, User.class);
          }
          } catch (JsonGenerationException e) {
              throw new RuntimeException();
          } catch (JsonMappingException e) {
              throw new RumtimeException();
          } catch (IOException e) {
              throw new RuntimeException();
          }
      }
  }
  ```

## JSON String Convert to List

  ```java
@Override
public List<XXXDto> jsonToListXXXDto(String json) {
	try {
		return  objectMapper.readValue(json, new TypeReference<List<XXXDto>>(){});
	}catch (IOException e) {
		throw new RuntimeException("Fail Convert... JSON CONVERT TO JAVA OBJECT");
	}
}
  ```

## Map Convert to JSON String

  ```java
public class MapConvertToJsonString {
  public static void main(String[] args) throws Exception {

      ObjectMapper mapper = new ObjectMapper();

      HashMap<String, String> map = new HashMap<String, String>();
      map.put("name", "steave");
      map.put("age", "32");
      map.put("job", "baker");

      System.out.println(map);
      System.out.println(mapper.writeValueAsString(map));
  }
}
  ```

## JSON Convert to Map

  ```java
public class JsonStringConvertToMap {
  public static void main(String[] args) throws Exception {

      ObjectMapper mapper = new ObjectMapper();
      HashMap<String, String> map = new HashMap<String, String>();

      String jsn = "{\"age\":\"32\",\"name\":\"steave\",\"job\":\"baker\"}";

      map = mapper.readValue(jsn,
              new TypeReference<HashMap<String, String>>() {});        

      System.out.println(map);
  }
}
  ```

## List<Map<?,?>> Convert to JSON String

  ```java
public class ListMapConvertToJsonString {
  public static void main(String[] args) throws Exception {

      ObjectMapper mapper = new ObjectMapper();

      // map -> json
      ArrayList<HashMap<String, String>> list
              = new ArrayList<HashMap<String,String>>();

      HashMap<String, String> map = new HashMap<String, String>();
      map.put("name", "steave");
      map.put("age", "32");
      map.put("job", "baker");
      list.add(map);

      map = new HashMap<String, String>();
      map.put("name", "matt");
      map.put("age", "25");
      map.put("job", "soldier");
      list.add(map);

      System.out.println(mapper.writeValueAsString(list));

      // json -> map
      String jsn = "[{\"age\":\"32\",\"name\":\"steave\",\"job\":\"baker\"},"
              + "{\"age\":\"25\",\"name\":\"matt\",\"job\":\"soldier\"}]";
      list = mapper.readValue(jsn,
              new TypeReference<ArrayList<HashMap<String, String>>>() {});        

      System.out.println(list);
  }
}
  ```
