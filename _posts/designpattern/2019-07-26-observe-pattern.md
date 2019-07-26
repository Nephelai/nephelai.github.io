---
layout: post
title: Observer Pattern
tags: []
categories: [designpattern]

---

## 정의

한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식으로 one-to-many 의존성을 정의

## 구성

크게 2가지 부분으로 볼 수 있습니다. 하나는 연락을 받는 객체, Observer이고, 변화할 수 있는 객체 (Publisher)입니다.

## 사용 이유

User와 Chatting Room이 있다고 가정해 봅시다. User가 말을 하고 Chatting Room이 이를 들을 수 있습니다.

``` java
public class User {
    private Room room;
    public void setRoom(Room room) {
        this.room = room;
    }
    public void talk(String msg) {
        room.receive(msg);
    }
}
```

``` java
public class Room {
    public void receive(String msg) {
        System.out.println(msg);
    }
}
```

``` java
public class Client {
    public static void main(String args[]) {
        User user = new User();
        Room room = new room();
        
        user.setRoom(room);
        
        String msg = "Hello~";
        user.talk(msg);
    }
}
```

만일 User가 여러 채팅방에 입장하게 되었습니다. 그리고 User가 Chatting은 모든 Chatting Room에 전달되어야 합니다.

* Publisher : User , Observer : Chatting Rooms

```java
public class Room {
    public String roomName;
    public void receive(String msg) {
        System.out.println(this.roomName + "is received msg : " + msg);
    }
}
```

```java
public class ChatRoom extends Room {
    public ChatRoom(String roomName) {
        this.roomName = roomName;
    }
}
```

``` java
public class GameRoom extends Room {
    public GameRoom(String roomName) {
        this.roomName = roomName;
    }
}
```

``` java
public class DevRoom extends Room {
    public DevRoom(String roomName) {
        this.roomName = roomName;
    }
}
```

ChatRoom, GameRoom, DevRoom 을 정의하고 Room Class로 Encapsulation.

``` java
import java.util.List;

public class User {
	private List<Room> rooms;
	public void setRoom(List<Room> rooms) {
		this.rooms = rooms;
	}
	public void talk(String msg) {
		for(Room r:rooms) {
            r.receive(msg);
        }
	}
}
```

``` java
import java.util.ArrayList;
import java.util.List;

public class Client {
    public static void main(String args[]) {
        User user = new User();
        List<Room> rooms = new ArrayList<Room>();
        rooms.add(new ChatRoom("Chat Room"));
        rooms.add(new GameRoom("Game Room"));
        rooms.add(new DevRoom("Dev Room"));
        
        user.setRoom(rooms);
        
        String msg = "Hello";
        user.talk(msg);
    }
}
```

Client Class를 보면 User와 Room은 매우 강하게 연결되어 있습니다. 이런 상황에서 다른 방이 생성되거나 특정 방이 삭제되어 메시지를 보내야 한다면 어떻게 해야할까요? List에 직접 추가, 삭제를 해야 가능합니다.

## 그렇다면 여기에 Observer Pattern을 적용해 봅시다.

먼저 ChatRoom, GameRoom, DevRoom을 Observer로 둡니다. 그러기 위해서 Observer Class를 정의하여 Encapsulation을 하도록 합니다.

* Interface 또는 Abstract Class로 정의해도 되지만, Class로 진행했습니다.

``` java
public class Observer {
    public String roomName;
    public void receive(String msg) {
        System.out.println(this.roomName + "is received msg : " + msg);
    }
}
```

```java
public class ChatRoom extends Observer {
    public ChatRoom(String roomName) {
        this.roomName = roomName;
    }
}
```

``` java
public class GameRoom extends Observer {
    public GameRoom(String roomName) {
        this.roomName = roomName;
    }
}
```

``` java
public class DevRoom extends Observer {
    public DevRoom(String roomName) {
        this.roomName = roomName;
    }
}
```

Code 결과가 Room의 Class를 상속받는 것과 비슷한 것을 볼 수 있습니다.

다음으로 Observer를 추가하고 제거하고 메시지를 알리는 기능들을 정의하는 Subject(Publisher) Class를 정의합니다. 그리고 User Class에서 Subject(Publisher) Class를 상속받도록 합니다. 즉, User Class에서 Observer들을 관리할 수 있습니다.

```java
import java.util.ArrayList;
import java.util.List;

public class Subject {
    private List<Observer> observers = new ArrayList<Observer>();
    // Register Observer
    public void attach(Observer observer) {
        observers.add(observer);
    }
    // Delete Observer
    public void detach(Observer observer) {
        observers.remove(observer);
    }
    // notify to Obsersers
    public void notifyObservers(String msg) {
        for(Observer o:observers) {
            o.receive(msg);
        }
    }
}
```

``` java
public class User extends Subject {
    // Subject 함수 이외의 다른 Code...
}
```

마지막으로 Client에서는 어떻게 Code가 바뀌는지 보겠습니다.

``` java
public class Client {
    public static void main(String args[]) {
        User user = new User();
        ChatRoom chatRoom = new ChatRoom("Chat Room");
        GameRoom gameRoom = new GameRoom("Game Room");
        DevRoom devRoom = new DevRoom("Dev Room");
        user.attach(chatRoom);
        user.attach(gameRoom);
        user.attach(devRoom);
        
        String msg = "Hello";
        user.notifyObservers(msg);
        
        // if 수정이 발생한다면...
        user.detach(chatRoom);
        msg = "Bye";
        user.notifyObservers(msg);
    }
}
```

의존성이 많이 제거된 것을 확인할 수 있습니다.

![Observer Pattern](https://nephelai.github.io/images/posts/observer_pattern.jpg)























