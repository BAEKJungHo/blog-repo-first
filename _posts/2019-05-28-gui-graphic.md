---
title: "Swing Graphics"
layout: post
category: Java
tags: [Java]
excerpt: "Java의 GUI의 Graphics에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## Graphics

  > 모든 컴포넌트는 자신의 모양을 스스로 그리며. 컨테이너는 자신을 그린 후 자식들에게 그리기를 지시

  - public void paintComponent(Graphics g)

    - 스윙 컴포넌트가 자신의 모양을 그리는 메소드

    - JComponent의 메소드
      - 모든 스윙 컴포넌트가 이 메소드를 가지고 있음

    - 컴포넌트가 그려져야 하는 시점마다 호출
      - 기가 변경되거나, 위치가 변경되거나 컴포넌트가 가려졌던 것이 사라지는 등


  - Graphics 객체(java.awt.Graphics)

    - 컴포넌트 그리기에 필요한 도구를 제공하는 객체
      - 색 지정, 도형 그리기, 클리핑, 이미지 그리기 등의 메소드 제공

  > 사용자가 원하는 모양을 그리고자 할 때 : paintComponent(Graphic g)를 오버라이딩하여 재작성

## EXAMPLE

  다음은 눈사람 얼굴 그리기 예제 입니다.

  ```java
  import java.awt.*;
  import java.awt.event.*;
  import javax.swing.*;

  public class SnowManFace extends JFrame {

	public SnowManFace() {
		setTitle("눈사람 얼굴");
		setSize(280, 300);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
		add(new MyPanel());
	}

	class MyPanel extends JPanel {
		public void paintComponent(Graphics g) {
			super.paintComponent(g);
			g.setColor(Color.WHITE);
			g.fillOval(20, 30, 200, 200);
			g.setColor(Color.BLACK);
			g.drawArc(60, 80, 50, 50, 180, -180); // -180은 위 로 볼록한 반원 모양(웃고 있는 눈)
			g.drawArc(150, 80, 50, 50, 180, -180);
			g.drawArc(70, 130, 100, 70, 180, 180); // 180은 아래로 볼록한 반원 모양(웃고 있는 입)
		}
	}

	public static void main(String[] args) {
		new SnowManFace();
	 }
  }
  ```

  다음은 Panel 크기에 맞게 이미지를 출력하며, Panel 크기를 늘릴때마다 `paintComponent`를 호출하며
  Panel 크기에 맞게 이미지의 크기도 변하는 예제 입니다.

  ```java
  import java.awt.*;
  import java.awt.event.*;
  import javax.swing.*;

  public class GraphicsDrawImageEx extends JFrame {
  	Container contentPane;
  	GraphicsDrawImageEx1() {
  		setTitle("drawImage 사용 예제");
  		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
  		contentPane = getContentPane();
  		MyPanel panel = new MyPanel();
  		contentPane.add(panel, BorderLayout.CENTER);
  		setSize(300, 400);
  		setVisible(true);
  	}

  	class MyPanel extends JPanel {
  		ImageIcon imageIcon = new ImageIcon("images/call.png");
  		Image image = imageIcon.getImage();

  		public void paintComponent(Graphics g) {
  			System.out.println("paintComponent 호출"); // Panel 크기를 늘릴때 마다 호출됨
  			super.paintComponent(g);
  			g.drawImage(image, 0, 0, this.getWidth(), this.getHeight(), this);
  			// g.drawImage(image, 20, 20, 250, 100, 100, 50, 200, 200, this);
  		}
  	}
  	public static void main(String[] args) {
  		new GraphicsDrawImageEx();
  	}
  }
  ```
