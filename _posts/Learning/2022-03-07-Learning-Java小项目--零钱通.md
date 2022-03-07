---
layout:     post title:      "Java入门小项目--零钱通"
subtitle:   "Practice"
date:       2022-03-07 16：47 author:     "Xiaohan"
header-img: "img/code001.jpg"
category: Learning catalog: true tags:
- Java实战项目
---

> 总要有个开始

今天实现了一个Java的小项目，很简单，适合只有入门级的Java Learner进行项目练习<br>

主要实现了以下几个功能

### 功能展示

* 初始界面

```
============Menu===========
1. 明细          
2. 收入          
3. 消费          
4. 清除          
5. 退出   
```       

* 查询记录

``` 
2022-03-07 16:38收入100
2022-03-07 16:38收入100
2022-03-07 16:38收入340
结余540 
```

* 存入

```
2
请输入金额：
50
收入50
```

* 取出

```
3
请输入金额：
25
消费25
```

* 清除

```
No record
结余0
```

* 退出

```
5
你确定要退出吗？y/n
y
退出了零钱通系统！
```

### 核心代码

#### Main

```java
    public void start() throws IOException {
        // 获取用户输入（交互界面）
        scanner = new Scanner(System.in);
        // 记录存储
        if (detail.exists()) {
            detail.delete();
            detail.createNewFile();
        }
        // 使用循环不断获取操作
        do {
            System.out.println(MainMenu);
            int key = Integer.parseInt(scanner.next());
            getOptions(key);
        } while (loop);
    }
```

#### 实现功能

单独设置method用来具体导向不同功能的函数

```java
    public void getOptions(int key) throws IOException {
        if (key == 1) {
            Functions.getPrint();
            System.out.println("结余" + TotalMoney);
        } else if (key == 2) {
            System.out.println("请输入金额：");
            int money = Integer.parseInt(scanner.next());
            TotalMoney = Functions.getIn(money, TotalMoney, detail);
        } else if (key == 3) {
            System.out.println("请输入金额：");
            int money = Integer.parseInt(scanner.next());
            TotalMoney = Functions.getOut(money, TotalMoney, detail);
        } else if (key == 4) {
            Functions.getClear(detail);
            TotalMoney = 0;
        } else if (key == 5) {
            Functions.quit(scanner);
            loop = false;
        } else {
            System.out.println("Sorry, we don't have " + key + " option!");
        }
    }
```

#### 具体实现

存入取出类似

```java
    public static int getIn(int money, int totalMoney, File detail) {
        if (money < 0) {
            System.out.println("数据输入有误，收入应该 > 0");
        } else {
            detail(true, money, detail);
            totalMoney = totalMoney + money;
            System.out.println("收入" + money);
        }
        return totalMoney;
    }
```

操作记录

```java
    public static void detail(boolean in, int money, File detail) {
        if (in) {
            record = "收入";
        } else {
            record = "支出";
        }
        date = new Date();
        sb.setLength(0);
        sb.append(sdf.format(date) + record + money);
        try {
            if (!detail.exists()) {
                detail.createNewFile();
            }
            FileWriter fw = new FileWriter(detail, true);
            fw.write(sb.toString() + "\n");
            fw.flush();
            fw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

退出程序

```java
    public static void quit(Scanner scanner) {
        System.out.println("你确定要退出吗？y/n");
        if (scanner.hasNext() && scanner.next().equals("y")) {
            System.out.println("退出了零钱通系统！");
        } else {
            System.out.println("你确定要退出吗？y/n");
        }
        scanner.close();
    }
```

完整代码可在 [SmallChangePacket](https://github.com/Yangxiaohan0120/LearningJava/tree/main/src/main/java/Project/SmallChangePacket) 查看