---
title: yml语法
published: 2025-01-17 18:24:29
description: yml 一种便捷简单的标记语言
category: Standard
toc: true
tags: [Markuplanguage]
image: "./73491672_p0.jpg"
draft: false
lang: zh
---
**前言：之前在用ssm框架的时候还没有接触这个语言，但是它给我的第一印象是清楚便捷，很容易划分层级关系和便于进行参数控制的语言，在用hexo主题的时候也多被用于对插件脚本的开启和参数的调整，之后在学习springboot的时候yml语言也作为配置文件常被用到（这意味着我从xml和properties中解脱了）。**

## 一、YML简介
* **YAML（IPA: /ˈjæməl/，尾音类似camel骆驼）是一个可读性高，用来表达资料序列的编程语言。YAML参考了其他多种语言，
包括：XML、C语言、Python、Perl以及电子邮件格式RFC2822。Clark Evans在2001年在首次发表了这种语言，另外Ingy döt Net与Oren Ben-Kiki也是这语言的共同设计者。目前已经有数种编程语言或脚本语言支援（或者说解析）这种语言，通常以.yml为后缀的文件，是一种直观的能够被电脑识别的数据序列化格式，并且容易被人类阅读，容易和脚本语言交互的，可以被支持YAML库的不同的编程语言程序导入，一种专门用来写配置文件的语言。**

## 二、YML特点

* **YAML易于人们阅读**

* **YAML数据在编程语言之间是可移植的**

* **YAML匹配敏捷语言的本机数据结构**

* **YAML具有一致的模型来支持通用工具**

* **YAML具有一致的模型来支持通用工具**

* **YAML具有表现力和可扩展性**

* **YAML易于实现和使用**

## 三、YML语法

### 1、基本规定
* **k: v 表示一对键值对关系，冒号后面必要一个空格**

* **k: v 表示一对键值对关系，冒号后面必要一个空格**

* **大小写敏感**

### 2、取值范围

**字面量：普通的值（数字、字符串、布尔）**

k: v: 字面直接来写

字符串默认不用加上单引号或者双引号；

“ “：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身表现形式输出
name：”cygames \n princessconnect”：输出为：

cygames
princessconnect

‘ ‘：单引号；会转义字符串里特殊字符；特殊字符最后输出为普通的字符串
name：’cygames \n princessconnect’：输出为：

cygames \n princessconnect

---：表示一个文档开始，比如springboot配置文件中两个不同的配置文件用—分隔开

```yml
	server:
	  port: 8084
	---
	server:
	  port: 8085
	  address: 127.0.0.3
```
---和...配合使用在一个配置文件中，表示一个文件的结束

!!：用以类型强制转换

```yml
player:
  card: !!str 58964
  classes: !!str true
```

**对象、Map（属性和值）（键值对）**

下一行写对象的属性和值的关系，注意缩进

```yml
player
    pid: szb001
    name: 竜ヶ崎ヒイロ
    age: 15
    class: dargon
```

**行内写法**

```yml
player:{pid: szb001,name: 竜ヶ崎ヒイロ,age: 15,class: dargon}
```

**数组写法（List、Set）**
用-值表示数组中的一个元素

```yml
shadowverse:
 - card
 - class
 - cost
 - effect
 - leader
```

**行内写法**

```yml
shadowverse: [card,class,cost,effect,leader]
```

### 3、值的获取

**yml文件配置**
```yml
server:
  port: 8082

player:
  pid: szb001
  name: 竜ヶ崎ヒイロ
  age: 15
  classes: dargon
  map: {szb001: dargon,szb002: witch}
  list:
    - 竜ヶ崎ヒイロ
    - 夜那月路西亚
  card:
    cardid: c001
    cost: 1
    effect: dd
    classes: normal
```

在实体类中直接和yml配置文件进行绑定

```java
	package com.princess.character.entity;

	import org.springframework.boot.context.properties.ConfigurationProperties;
	import org.springframework.stereotype.Component;

	import java.util.List;
	import java.util.Map;

	/**
	 * 将配置文件中配置的每一个属性的值，映射到这个组件中
	 *   @ConfigurationProperties：告诉springboot将本类中的所有属性和配置文件中相关的配置进行绑定；
	 *          prefix = "player":配置文件中哪个下面的所有属性进行一一映射
	 *   只有这个组件是容器中的组件，才能用容器中提供的 @ConfigurationProperties 功能
	 *   
	 */
	//component意思为把普通pojo实例化到spring容器中，相当于配置文件中的
	//<bean id="" class=""/>
	@Component
	@ConfigurationProperties(prefix = "player")
	public class Player {
		private String pid;
		private String name;
		private String age;
		private String classes;

		private Map<String,Object> map;
		private List<Object> list;
		private Card card;

		public String getPid() {
			return pid;
		}

		public void setPid(String pid) {
			this.pid = pid;
		}

		public String getName() {
			return name;
		}

		public void setName(String name) {
			this.name = name;
		}

		public String getAge() {
			return age;
		}

		public void setAge(String age) {
			this.age = age;
		}

		public String getClasses() {
			return classes;
		}

		public void setClasses(String classes) {
			this.classes = classes;
		}

		public Map<String, Object> getMap() {
			return map;
		}

		public void setMap(Map<String, Object> map) {
			this.map = map;
		}

		public List<Object> getList() {
			return list;
		}

		public void setList(List<Object> list) {
			this.list = list;
		}

		public Card getCard() {
			return card;
		}

		public void setCard(Card card) {
			this.card = card;
		}

		@Override
		public String toString() {
			return "Player{" +
					"pid='" + pid + '\'' +
					", name='" + name + '\'' +
					", age='" + age + '\'' +
					", classes='" + classes + '\'' +
					", map=" + map +
					", list=" + list +
					", card=" + card +
					'}';
		}
	}
```

**测试**

```java
	package com.princess.character;

	import com.princess.character.entity.Player;
	import net.bytebuddy.implementation.bind.annotation.RuntimeType;
	import org.junit.jupiter.api.Test;
	import org.junit.runner.RunWith;
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.boot.context.properties.ConfigurationProperties;
	import org.springframework.boot.test.context.SpringBootTest;
	import org.springframework.context.annotation.ComponentScan;
	import org.springframework.stereotype.Component;
	import org.springframework.test.context.junit4.SpringRunner;

	import javax.annotation.security.RunAs;

	/**
	 * SpringBoot单元测试
	 *
	 * 可以在测试期间方便的类似编码一样进行自动注入容器等功能
	 */
	@RunWith(SpringRunner.class)
	@SpringBootTest
	class CharacterApplicationTests {

		@Autowired
		Player pp;

		@Test
		public void contextLoads() {
			System.out.println(pp);
		}

	}
```
*******************

**参考资料：**

[叩丁狼教育简书](https://jianshu.com/p/97222440cd08)